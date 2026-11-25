# 第 2 章　给抽取结果做一张登记卡

上一章找到了取件码。现在给每条结果加一张登记卡。为什么不直接返回一个字符串？因为同一个数字可能是取件码，也可能是订单号。我们还需要知道它从哪里来。

## 2.1 先用字典

```python
text = "取件码 3821"
record = {
    "kind": "pickup_code",
    "value": "3821",
    "start": 4,
    "end": 8,
}
assert text[record["start"]:record["end"]] == record["value"]
```

字典很好用。可是在不同函数中，有人写 start，有人写 begin，于是程序悄悄产生了两套格式。用一个类定义登记卡，可以集中声明字段，并让编辑器、类型检查工具和读代码的人看到统一约定。dataclass 是 Python 帮我们自动生成部分常用方法的工具，不是数据库。

## 2.2 位置不是数字本身

Python 切片采用左闭右开的区间：起点包括，终点不包括。例如 `"ABCDE"[1:3]` 得到 BC。长度正好是 3 - 1 = 2。空区间也可以用左右端相同来描述。作者的字符区间使用同一约定。

源码出处：[langextract/core/data.py:50–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L50-L60)。

````python
@dataclasses.dataclass
class CharInterval:
  """Class for representing a character interval.

  Attributes:
    start_pos: The starting position of the interval (inclusive).
    end_pos: The ending position of the interval (exclusive).
  """

  start_pos: int | None = None
  end_pos: int | None = None
````

两个字段都可以为 None，允许调用者暂时没有确定位置。它并不自动检查 start 小于 end；需要有效区间的其他步骤需要承担验证责任。与传一对整数相比，封装后更容易理解这两个整数是一组坐标。代价是多创建一个对象。

如果助手返回的数据和原文完全一致，就可以给它贴上“精确匹配”标签。若只找到一部分或包含更多字词，就应该明确区分，而不要给所有结果盖同一个章。

源码出处：[langextract/core/data.py:43–47](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L43-L47)。

````python
class AlignmentStatus(enum.Enum):
  MATCH_EXACT = "match_exact"
  MATCH_GREATER = "match_greater"
  MATCH_LESSER = "match_lesser"
  MATCH_FUZZY = "match_fuzzy"
````

四个成员分别表示精确、找到更长片段、找到更短片段、模糊匹配。枚举让状态只有一组明确的名字。注意 None 不等同于一种匹配等级：它表示字段没有设置。具体算法如何选择标签将在证据定位章节研究。

## 2.3 正式的登记卡 Extraction

我们先理解另一个位置概念。比如“快递 到了 3821”可人工分成三个词，3821 是第三个词；按字符数和按词数定位是两种不同坐标。项目把后一种单位称为 token，暂时只需要理解它是供文本匹配算法操作的小片段，而非模型计费所用 token 的必然同义词。

源码出处：[langextract/core/data.py:63–126](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L63-L126)。

````python
@dataclasses.dataclass(init=False)
class Extraction:
  """Represents an extraction extracted from text.

  This class encapsulates an extraction's characteristics and its position
  within the source text. It can represent a diverse range of information for
  NLP information extraction tasks.

  Attributes:
    extraction_class: The class of the extraction.
    extraction_text: The text of the extraction.
    char_interval: The character interval of the extraction in the original
      text. None when the extraction text could not be located in the source
      document.
    alignment_status: The alignment status of the extraction.
    extraction_index: The index of the extraction in the list of extractions.
    group_index: The index of the group the extraction belongs to.
    description: A description of the extraction.
    attributes: A list of attributes of the extraction.
    token_interval: The token interval of the extraction.
  """

  extraction_class: str
  extraction_text: str
  char_interval: CharInterval | None = None
  alignment_status: AlignmentStatus | None = None
  extraction_index: int | None = None
  group_index: int | None = None
  description: str | None = None
  attributes: dict[str, str | list[str]] | None = None
  _token_interval: tokenizer.TokenInterval | None = dataclasses.field(
      default=None, repr=False, compare=False
  )

  def __init__(
      self,
      extraction_class: str,
      extraction_text: str,
      *,
      token_interval: tokenizer.TokenInterval | None = None,
      char_interval: CharInterval | None = None,
      alignment_status: AlignmentStatus | None = None,
      extraction_index: int | None = None,
      group_index: int | None = None,
      description: str | None = None,
      attributes: dict[str, str | list[str]] | None = None,
  ):
    self.extraction_class = extraction_class
    self.extraction_text = extraction_text
    self.char_interval = char_interval
    self._token_interval = token_interval
    self.alignment_status = alignment_status
    self.extraction_index = extraction_index
    self.group_index = group_index
    self.description = description
    self.attributes = attributes

  @property
  def token_interval(self) -> tokenizer.TokenInterval | None:
    return self._token_interval

  @token_interval.setter
  def token_interval(self, value: tokenizer.TokenInterval | None) -> None:
    self._token_interval = value
````

| 字段 | 登记卡上的意义 |
|---|---|
| extraction_class | 信息类别，例如 pickup_code |
| extraction_text | 抽取出的文字，如 3821 |
| char_interval | 在原文中的字符位置，可为空 |
| alignment_status | 对找到的位置属于哪种匹配的说明 |
| extraction_index | 结果排列编号 |
| group_index | 来自模型答案中第几个条目组 |
| description | 可选说明 |
| attributes | 附加信息，例如地点名称、类型 |
| _token_interval | 内部保存的词片段坐标 |

构造器只要求类别和文本，星号使其他参数必须写成关键字形式。这样 `Extraction("pickup_code", "3821")` 就能表示一个尚未定位的答案，而不需要假造坐标。

类的装饰器设置 init=False，因此作者自行编写 __init__，将公开参数 token_interval 存进私有字段 _token_interval。该字段还设置 repr=False 和 compare=False，打印或 dataclass 比较时不把它算进去。例如内部切分方式改变未必应该使登记卡比较不相等。这里的设计理由是根据实现推断，不是作者明确宣称的意图。

token_interval 的两个同名方法分别是属性读取器和写入器。执行 `card.token_interval` 会返回内部字段，执行 `card.token_interval = interval` 会更新字段。没有执行校验或重新计算。找证据的算法可在获得答案之后填充这个位置。

相比直接返回字典，一个这样的对象可以被解析、定位、保存和展示步骤共享；代价是需要序列化转换才能存成 JSON。

## 2.4 一张短信有多张登记卡

假设两条短信内容完全相同，但分别来自两部手机。如果我们按短信文字分组，两个来源可能混在一起。于是为每条短信指定编号，类似图书馆每本书的馆藏号。

另外，某些背景信息不在短信内，例如“这批消息来自日本的快递公司”。可以独立保存这类附加背景，而不是把它拼进原文：拼接会改变原来的字符位置。

源码出处：[langextract/core/data.py:129–203](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L129-L203)。

````python
@dataclasses.dataclass
class Document:
  """Document class for annotating documents.

  Attributes:
    text: Raw text representation for the document.
    document_id: Unique identifier for each document and is auto-generated if
      not set.
    additional_context: Additional context to supplement prompt instructions.
    tokenized_text: Tokenized text for the document, computed from `text`.
  """

  text: str
  additional_context: str | None = None
  _document_id: str | None = dataclasses.field(
      default=None, init=False, repr=False, compare=False
  )
  _tokenized_text: tokenizer.TokenizedText | None = dataclasses.field(
      init=False, default=None, repr=False, compare=False
  )

  def __init__(
      self,
      text: str,
      *,
      document_id: str | None = None,
      additional_context: str | None = None,
  ):
    self.text = text
    self.additional_context = additional_context
    self._document_id = document_id

  @property
  def document_id(self) -> str:
    """Returns the document ID, generating a unique one if not set."""
    if self._document_id is None:
      self._document_id = f"doc_{uuid.uuid4().hex[:8]}"
    return self._document_id

  @document_id.setter
  def document_id(self, value: str | None) -> None:
    """Sets the document ID."""
    self._document_id = value

  @property
  def tokenized_text(self) -> tokenizer.TokenizedText:
    # `text` is a public field, so the cache may hold tokens for older text.
    if self._tokenized_text is None or self._tokenized_text.text != self.text:
      self._tokenized_text = tokenizer.tokenize(self.text)
    return self._tokenized_text

  @tokenized_text.setter
  def tokenized_text(self, value: tokenizer.TokenizedText) -> None:
    self._tokenized_text = value

  def with_additional_context(
      self, additional_context: str | None
  ) -> "Document":
    """Return a copy of this Document with additional_context overridden.

    The copy shares this Document's ID, generating one if needed, and
    preserves any cached tokenization without invoking the tokenization
    property getter.

    Args:
      additional_context: Value to set on the returned copy.
    """
    new_doc = Document(
        text=self.text,
        document_id=self.document_id,
        additional_context=additional_context,
    )
    if self._tokenized_text is not None:
      new_doc.tokenized_text = self._tokenized_text
    return new_doc
````

Document 的 text 保存原始文字，additional_context 保存背景，_document_id 保存身份编号，_tokenized_text 保存已完成的分词结果（缓存）。构造时只需要原文。

逐个看看它的动作：

- __init__ 接收文本、可选编号和背景，保存参数，并不做分词。
- document_id getter 在编号为空时才生成 doc_ 加 UUID 的前八位，以后直接返回。同名 setter 可以更新或清空编号。这个编号不是内容散列，也不是绝对无碰撞保证。
- tokenized_text getter 先判断缓存是否为空，或缓存记录的原文是否已经和当前 text 不同。只有需要时才重新调用 tokenizer.tokenize。这样同一条短信反复使用不会每次都分词。setter 允许调用者直接替换缓存。
- with_additional_context 返回新的 Document。它保留原编号，并直接检查旧缓存字段，如果存在便赋给新对象。它没有调用旧对象的 tokenized_text getter，因此不会意外触发分词。新旧对象共享已有分词对象；这不像深拷贝那样完全隔离，但可以省重复工作。

这里可以观察到一个通用设计：不要急着计算暂时用不到的东西；但如果源数据可能改变，复用计算结果前就要检查它是否仍然有效。另一种方案是在构造时立即分词，读取简单，构造成本却更高。

## 2.5 已经批改完的短信

输入是短信，输出是短信和登记卡合集。我们可以直接给 Document 新增 extractions，但作者采取另一种做法：输入与输出使用不同的类。

源码出处：[langextract/core/data.py:206–260](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L206-L260)。

````python
@dataclasses.dataclass
class AnnotatedDocument:
  """Class for representing annotated documents.

  Attributes:
    document_id: Unique identifier for each document - autogenerated if not
      set.
    extractions: List of extractions in the document.
    text: Raw text representation of the document.
    tokenized_text: Tokenized text of the document, computed from `text`.
  """

  extractions: list[Extraction] | None = None
  text: str | None = None
  _document_id: str | None = dataclasses.field(
      default=None, init=False, repr=False, compare=False
  )
  _tokenized_text: tokenizer.TokenizedText | None = dataclasses.field(
      init=False, default=None, repr=False, compare=False
  )

  def __init__(
      self,
      *,
      document_id: str | None = None,
      extractions: list[Extraction] | None = None,
      text: str | None = None,
  ):
    self.extractions = extractions
    self.text = text
    self._document_id = document_id

  @property
  def document_id(self) -> str:
    """Returns the document ID, generating a unique one if not set."""
    if self._document_id is None:
      self._document_id = f"doc_{uuid.uuid4().hex[:8]}"
    return self._document_id

  @document_id.setter
  def document_id(self, value: str | None) -> None:
    """Sets the document ID."""
    self._document_id = value

  @property
  def tokenized_text(self) -> tokenizer.TokenizedText | None:
    if self.text is None:
      self._tokenized_text = None
    elif self._tokenized_text is None or self._tokenized_text.text != self.text:
      self._tokenized_text = tokenizer.tokenize(self.text)
    return self._tokenized_text

  @tokenized_text.setter
  def tokenized_text(self, value: tokenizer.TokenizedText) -> None:
    self._tokenized_text = value
````

extractions 是结果列表，可为 None；text 是原文，也允许 None。_document_id 和 _tokenized_text 与输入文档承担类似职责。

__init__ 只保存传入的三项值；document_id 的 getter、setter 与 Document 的行为一致。tokenized_text getter 有一个明显不同的开头：text 为 None 时清空缓存并返回 None。否则和 Document 一样按需更新。setter 用于直接写入缓存。为什么要额外检查？因为这个类允许不提供原文，而 Document 的 text 声明为必需字符串。

分开两个类的好处是接口可以表明数据是否已经过处理，避免无意修改输入；缺点是重复了身份和缓存管理逻辑。另一种方案是用基类抽出共享部分，但它会为两个数据对象引入额外继承关系。

## 2.6 给助手一张已填写的示范

如果让人整理第一次见到的快递短信，我们通常先给他一个填好的例子。示范也需要保存原文与抽取项。

源码出处：[langextract/core/data.py:263–273](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L263-L273)。

````python
@dataclasses.dataclass
class ExampleData:
  """A single training/example data instance for a structured prompting.

  Attributes:
    text: The raw input text (sentence, paragraph, etc.).
    extractions: A list of Extraction objects extracted from the text.
  """

  text: str
  extractions: list[Extraction] = dataclasses.field(default_factory=list)
````

它有 text 和 extractions 两个字段。列表的 default_factory=list 让每个实例都获得自己的空列表，而不是让不同示范共享同一张空表。这类数据供后面的提示词生成器使用，不需要强制包含文档编号和完整定位信息。

目前已经把基础的“是什么数据”讲清楚。下一章研究如何把这种数据真的交给模型。
