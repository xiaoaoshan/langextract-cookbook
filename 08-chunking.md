# 第 8 章　一个月的短信一次看不完

快递短信积攒了一个月。我们把它们交给助手，结果助手说一次只能读有限长度的文字。怎么办？

## 8.1 先尝试直接切字符串

```python
text = "Pickup 3821. Pickup 9246."
print([text[i:i+10] for i in range(0, len(text), 10)])
```

第一片可能只有 "Pickup 382"，单独处理会误把取件码当成 382。即使每一片能送进模型，也不能保证信息完整。

上一章已经有了两把尺子：字符用来衡量长度，token 用来寻找边界。切块时我们希望尽量留下完整句子；句子太长，尽量留下完整 token。一个特别长的 token 本身超过限制时，宁可让它独占一块，也不要切掉它的一半。

## 8.2 给小信封编号

我们不能只把字符串交给下游。假如第二片中的取件码在位置 7，它在整篇文档中不一定在位置 7。

```python
from dataclasses import dataclass

@dataclass
class Envelope:
    original: str
    start: int
    end: int

    @property
    def text(self):
        return self.original[self.start:self.end]

chunk = Envelope("Pickup 3821. Pickup 9246.", 13, 25)
assert chunk.text == "Pickup 9246."
local_start = chunk.text.index("9246")
assert chunk.original[chunk.start + local_start:chunk.start + local_start + 4] == "9246"
```

我们没有复制整篇原文，也没有手工记录新旧字符串的转换表。正式实现的 TextChunk 更进一步，保存 token 区间和 Document 引用，按需计算文字及字符区间。

源码出处：[langextract/chunking.py:39–140](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L39-L140)。

````python
@dataclasses.dataclass
class TextChunk:
  """Stores a text chunk with attributes to the source document.

  Attributes:
    token_interval: The token interval of the chunk in the source document.
    document: The source document.
  """

  token_interval: tokenizer_lib.TokenInterval
  document: data.Document | None = None
  _chunk_text: str | None = dataclasses.field(
      default=None, init=False, repr=False
  )
  _sanitized_chunk_text: str | None = dataclasses.field(
      default=None, init=False, repr=False
  )
  _char_interval: data.CharInterval | None = dataclasses.field(
      default=None, init=False, repr=False
  )

  def __str__(self):
    interval_repr = (
        f"start_index: {self.token_interval.start_index}, end_index:"
        f" {self.token_interval.end_index}"
    )

    doc_id_repr = (
        f"Document ID: {self.document_id}"
        if self.document_id
        else "Document ID: None"
    )

    try:
      chunk_text_repr = f"'{self.chunk_text}'"
    except ValueError:
      chunk_text_repr = "<unavailable: document_text not set>"

    return (
        "TextChunk(\n"
        f"  interval=[{interval_repr}],\n"
        f"  {doc_id_repr},\n"
        f"  Chunk Text: {chunk_text_repr}\n"
        ")"
    )

  @property
  def document_id(self) -> str | None:
    """Gets the document ID from the source document."""
    if self.document is not None:
      return self.document.document_id
    return None

  @property
  def document_text(self) -> tokenizer_lib.TokenizedText | None:
    """Gets the tokenized text from the source document."""
    if self.document is not None:
      return self.document.tokenized_text
    return None

  @property
  def chunk_text(self) -> str:
    """Gets the chunk text. Raises an error if `document_text` is not set."""
    if self.document_text is None:
      raise ValueError("document_text must be set to access chunk_text.")
    if self._chunk_text is None:
      self._chunk_text = get_token_interval_text(
          self.document_text, self.token_interval
      )
    return self._chunk_text

  @property
  def sanitized_chunk_text(self) -> str:
    """Gets the sanitized chunk text."""
    if self._sanitized_chunk_text is None:
      self._sanitized_chunk_text = _sanitize(self.chunk_text)
    return self._sanitized_chunk_text

  @property
  def additional_context(self) -> str | None:
    """Gets the additional context for prompting from the source document."""
    if self.document is not None:
      return self.document.additional_context
    return None

  @property
  def char_interval(self) -> data.CharInterval:
    """Gets the character interval corresponding to the token interval.

    Returns:
      data.CharInterval: The character interval for this chunk.

    Raises:
      ValueError: If document_text is not set.
    """
    if self._char_interval is None:
      if self.document_text is None:
        raise ValueError("document_text must be set to compute char_interval.")
      self._char_interval = get_char_interval(
          self.document_text, self.token_interval
      )
    return self._char_interval
````

TextChunk 的三个私有缓存初始为 None，不允许从构造参数传进来，也不出现在默认 repr 中。document 可以为空，但这时不能取得片段文本或字符坐标。缓存依赖文档和区间保持不变；修改对象并不会自动清理缓存。

document_id 属性把查询交给原文档，没有文档时返回 None。document_text 取的是 Document.tokenized_text，而不是普通字符串，因此复用第 2、7 章的索引结构。

chunk_text 第一次调用 get_token_interval_text 切片并保存，以后直接返回；文档不存在时抛 ValueError。sanitized_chunk_text 同样延迟计算，使用 _sanitize 把连续空白换成空格；不能把这种修改后的字符串坐标直接照搬到原始文本。

additional_context 原样委托给 document.additional_context，允许同一文档的不同块都带上背景信息。char_interval 由 token 区间换算并缓存，没有来源文档就明确报错。

__str__ 为调试输出区间、文档编号和文字，如果获取文字遇到 ValueError 则显示不可用说明；它不是序列化接口。

## 8.3 切块的尺子

源码出处：[langextract/chunking.py:143–166](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L143-L166)。

````python
def create_token_interval(
    start_index: int, end_index: int
) -> tokenizer_lib.TokenInterval:
  """Creates a token interval.

  Args:
    start_index: first token's index (inclusive).
    end_index: last token's index + 1 (exclusive).

  Returns:
    Token interval.

  Raises:
    ValueError: If the token indices are invalid.
  """
  if start_index < 0:
    raise ValueError(f"Start index {start_index} must be positive.")
  if start_index >= end_index:
    raise ValueError(
        f"Start index {start_index} must be < end index {end_index}."
    )
  return tokenizer_lib.TokenInterval(
      start_index=start_index, end_index=end_index
  )
````

create_token_interval 检查 start_index 非负且严格小于 end_index，因此切块的区间必须非空。它不知道 token 列表长度，所以不会检查终点是否超范围。

源码出处：[langextract/chunking.py:169–213](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L169-L213)。

````python
def get_token_interval_text(
    tokenized_text: tokenizer_lib.TokenizedText,
    token_interval: tokenizer_lib.TokenInterval,
) -> str:
  """Get the text within an interval of tokens.

  Args:
    tokenized_text: Tokenized documents.
    token_interval: An interval specifying the start (inclusive) and end
      (exclusive) indices of the tokens to extract. These indices refer to the
      positions in the list of tokens within `tokenized_text.tokens`, not the
      value of the field `index` of `token_pb2.Token`. If the tokens are
      [(index:0, text:A), (index:5, text:B), (index:10, text:C)], we should use
      token_interval=[0, 2] to represent taking A and B, not [0, 6]. Please see
      details from the implementation of tokenizer_lib.tokens_text

  Returns:
    Text within the token interval.

  Raises:
    ValueError: If the token indices are invalid.
    TokenUtilError: If tokenizer_lib.tokens_text returns an empty
    string.
  """
  if token_interval.start_index >= token_interval.end_index:
    raise ValueError(
        f"Start index {token_interval.start_index} must be < end index "
        f"{token_interval.end_index}."
    )
  return_string = tokenizer_lib.tokens_text(tokenized_text, token_interval)
  logging.debug(
      "Token util returns string: %s for tokenized_text: %s, token_interval:"
      " %s",
      return_string,
      tokenized_text,
      token_interval,
  )
  if tokenized_text.text and not return_string:
    raise TokenUtilError(
        "Token util returns an empty string unexpectedly. Number of tokens is"
        f" tokenized_text: {len(tokenized_text.tokens)}, token_interval is"
        f" {token_interval.start_index} to {token_interval.end_index}, which"
        " should not lead to empty string."
    )
  return return_string
````

get_token_interval_text 再次拒绝空或逆向区间，委托上一章的 tokens_text 取得原件文字，并记录 debug 信息。如果原文非空而结果意外为空，抛 TokenUtilError。这一层的约束比允许空区间的 tokens_text 强。

源码出处：[langextract/chunking.py:35–36](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L35-L36)。

````python
class TokenUtilError(exceptions.LangExtractError):
  """Error raised when token_util returns unexpected values."""
````

TokenUtilError 是用于上述意外结果的领域异常；继承 LangExtractError，不新增数据。

源码出处：[langextract/chunking.py:216–243](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L216-L243)。

````python
def get_char_interval(
    tokenized_text: tokenizer_lib.TokenizedText,
    token_interval: tokenizer_lib.TokenInterval,
) -> data.CharInterval:
  """Returns the char interval corresponding to the token interval.

  Args:
    tokenized_text: Document.
    token_interval: Token interval.

  Returns:
    Char interval of the token interval of interest.

  Raises:
    ValueError: If the token_interval is invalid.
  """
  if token_interval.start_index >= token_interval.end_index:
    raise ValueError(
        f"Start index {token_interval.start_index} must be < end index "
        f"{token_interval.end_index}."
    )
  start_token = tokenized_text.tokens[token_interval.start_index]
  # Penultimate token prior to interval.end_index
  final_token = tokenized_text.tokens[token_interval.end_index - 1]
  return data.CharInterval(
      start_pos=start_token.char_interval.start_pos,
      end_pos=final_token.char_interval.end_pos,
  )
````

get_char_interval 使用开始 token 的字符起点和最后 token 的字符终点建立 data.CharInterval。列表位置是索引依据，不是 Token.index 字段。源码只直接检查 start < end，其他不合法索引可能导致 IndexError；调用者需要保障范围合法。

源码出处：[langextract/chunking.py:246–262](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L246-L262)。

````python
def _sanitize(text: str) -> str:
  """Converts all whitespace characters in input text to a single space.

  Args:
    text: Input to sanitize.

  Returns:
    Sanitized text with newlines and excess spaces removed.

  Raises:
    ValueError: If the sanitized text is empty.
  """

  sanitized_text = re.sub(r"\s+", " ", text.strip())
  if not sanitized_text:
    raise ValueError("Sanitized text is empty.")
  return sanitized_text
````

_sanitize 先 strip，再将连续空白替换成单个空格；如果输出为空抛 ValueError。保留原文版本和供处理的简化版本是一种常见设计。

## 8.4 不必把所有句子预先放进列表

还记得 Python 的 for 循环吗？它先调用 iter，再反复 next，直到 StopIteration。

```python
class Pages:
    def __init__(self, pages):
        self.pages = pages
        self.pos = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.pos == len(self.pages):
            raise StopIteration
        page = self.pages[self.pos]
        self.pos += 1
        return page

p = Pages(["第一条短信", "第二条短信"])
assert next(p) == "第一条短信"
assert list(p) == ["第二条短信"]
```

这个迭代器是有状态的；list(p) 消耗余下页面，而不是从头重新开始。

源码出处：[langextract/chunking.py:282–340](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L282-L340)。

````python
class SentenceIterator:
  """Iterate through sentences of a tokenized text."""

  def __init__(
      self,
      tokenized_text: tokenizer_lib.TokenizedText,
      curr_token_pos: int = 0,
  ):
    """Constructor.

    Args:
      tokenized_text: Document to iterate through.
      curr_token_pos: Iterate through sentences from this token position.

    Raises:
      IndexError: if curr_token_pos is not within the document.
    """
    self.tokenized_text = tokenized_text
    self.token_len = len(tokenized_text.tokens)
    if curr_token_pos < 0:
      raise IndexError(
          f"Current token position {curr_token_pos} can not be negative."
      )
    elif curr_token_pos > self.token_len:
      raise IndexError(
          f"Current token position {curr_token_pos} is past the length of the "
          f"document {self.token_len}."
      )
    self.curr_token_pos = curr_token_pos

  def __iter__(self) -> Iterator[tokenizer_lib.TokenInterval]:
    return self

  def __next__(self) -> tokenizer_lib.TokenInterval:
    """Returns next sentence's interval starting from current token position.

    Returns:
      Next sentence token interval starting from current token position.

    Raises:
      StopIteration: If end of text is reached.
    """
    assert self.curr_token_pos <= self.token_len
    if self.curr_token_pos == self.token_len:
      raise StopIteration
    # This locates the sentence which contains the current token position.
    sentence_range = tokenizer_lib.find_sentence_range(
        self.tokenized_text.text,
        self.tokenized_text.tokens,
        self.curr_token_pos,
    )
    assert sentence_range
    # Start the sentence from the current token position.
    # If we are in the middle of a sentence, we should start from there.
    sentence_range = create_token_interval(
        self.curr_token_pos, sentence_range.end_index
    )
    self.curr_token_pos = sentence_range.end_index
    return sentence_range
````

SentenceIterator 保存 tokenized_text、token_len 和 curr_token_pos。__init__ 接受从中间开始的位置，负值或大于 token_len 抛 IndexError，等于长度合法（意味着没有剩余句子）。__iter__ 返回自身。

__next__ 如果到达末尾就抛 StopIteration，否则调用 find_sentence_range 从当前位置寻找句尾。它用 create_token_interval 明确生成从当前游标开始的非空区间，把游标移动到 end_index。即使我们位于半句中也会继续向后读，而不是回到句首。这将在切长句时发挥作用。

## 8.5 按句子优先填充信封

先写一个只处理完整句子的简化版本：

```python
def pack(sentences, limit):
    block = ""
    for sentence in sentences:
        candidate = block + (" " if block else "") + sentence
        if block and len(candidate) > limit:
            yield block
            block = sentence
        else:
            block = candidate
    if block:
        yield block

assert list(pack(["A.", "B.", "C."], 5)) == ["A. B.", "C."]
```

这里还不能处理超长句，并且 join 时自己添加了空格，真实项目不用这个办法恢复原文。

源码出处：[langextract/chunking.py:343–506](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L343-L506)。

````python
class ChunkIterator:
  r"""Iterate through chunks of a tokenized text.

  Chunks may consist of sentences or sentence fragments that can fit into the
  maximum character buffer that we can run inference on.

  A)
  If a sentence length exceeds the max char buffer, then it needs to be broken
  into chunks that can fit within the max char buffer. We do this in a way that
  maximizes the chunk length while respecting newlines (if present) and token
  boundaries.
  Consider this sentence from a poem by John Donne:
  ```
  No man is an island,
  Entire of itself,
  Every man is a piece of the continent,
  A part of the main.
  ```
  With max_char_buffer=40, the chunks are:
  * "No man is an island,\nEntire of itself," len=38
  * "Every man is a piece of the continent," len=38
  * "A part of the main." len=19

  B)
  If a single token exceeds the max char buffer, it comprises the whole chunk.
  Consider the sentence:
  "This is antidisestablishmentarianism."
  With max_char_buffer=20, the chunks are:
  * "This is" len=7
  * "antidisestablishmentarianism" len=28
  * "." len(1)

  C)
  If multiple *whole* sentences can fit within the max char buffer, then they
  are used to form the chunk.
  Consider the sentences:
  "Roses are red. Violets are blue. Flowers are nice. And so are you."
  With max_char_buffer=60, the chunks are:
  * "Roses are red. Violets are blue. Flowers are nice." len=50
  * "And so are you." len=15
  """

  def __init__(
      self,
      text: str | tokenizer_lib.TokenizedText | None,
      max_char_buffer: int,
      tokenizer_impl: tokenizer_lib.Tokenizer,
      document: data.Document | None = None,
  ):
    """Constructor.

    Args:
      text: Document to chunk. Can be either a string or a tokenized text.
      max_char_buffer: Size of buffer that we can run inference on.
      tokenizer_impl: Tokenizer instance to use.
      document: Optional source document.
    """
    if text is None:
      if document is None:
        raise ValueError("Either text or document must be provided.")
      text = document.text or ""

    if isinstance(text, str):
      text = tokenizer_impl.tokenize(text)
    elif isinstance(text, tokenizer_lib.TokenizedText) and not text.tokens:
      text_to_tokenize = text.text or (document.text if document else "")
      text = tokenizer_impl.tokenize(text_to_tokenize)
    self.tokenized_text = text
    self.max_char_buffer = max_char_buffer
    self.sentence_iter = SentenceIterator(self.tokenized_text)
    self.broken_sentence = False

    # TODO: Refactor redundancy between document and text.
    if document is None:
      self.document = data.Document(text=text.text)
    else:
      self.document = document
    self.document.tokenized_text = self.tokenized_text

  def __iter__(self) -> Iterator[TextChunk]:
    return self

  def _tokens_exceed_buffer(
      self, token_interval: tokenizer_lib.TokenInterval
  ) -> bool:
    """Check if the token interval exceeds the maximum buffer size.

    Args:
      token_interval: Token interval to check.

    Returns:
      True if the token interval exceeds the maximum buffer size.
    """
    char_interval = get_char_interval(self.tokenized_text, token_interval)
    return (
        char_interval.end_pos - char_interval.start_pos
    ) > self.max_char_buffer

  def __next__(self) -> TextChunk:
    sentence = next(self.sentence_iter)
    # If the next token is greater than the max_char_buffer, let it be the
    # entire chunk.
    curr_chunk = create_token_interval(
        sentence.start_index, sentence.start_index + 1
    )
    if self._tokens_exceed_buffer(curr_chunk):
      self.sentence_iter = SentenceIterator(
          self.tokenized_text, curr_token_pos=sentence.start_index + 1
      )
      self.broken_sentence = curr_chunk.end_index < sentence.end_index
      return TextChunk(
          token_interval=curr_chunk,
          document=self.document,
      )

    # Append tokens to the chunk up to the max_char_buffer.
    start_of_new_line = -1
    for token_index in range(curr_chunk.start_index, sentence.end_index):
      if self.tokenized_text.tokens[token_index].first_token_after_newline:
        start_of_new_line = token_index
      test_chunk = create_token_interval(
          curr_chunk.start_index, token_index + 1
      )
      if self._tokens_exceed_buffer(test_chunk):
        # Only break at newline if: 1) newline exists (> 0) and
        # 2) it's after chunk start (prevents empty intervals)
        if start_of_new_line > 0 and start_of_new_line > curr_chunk.start_index:
          # Terminate the curr_chunk at the start of the most recent newline.
          curr_chunk = create_token_interval(
              curr_chunk.start_index, start_of_new_line
          )
        self.sentence_iter = SentenceIterator(
            self.tokenized_text, curr_token_pos=curr_chunk.end_index
        )
        self.broken_sentence = True
        return TextChunk(
            token_interval=curr_chunk,
            document=self.document,
        )
      else:
        curr_chunk = test_chunk

    if self.broken_sentence:
      self.broken_sentence = False
    else:
      for sentence in self.sentence_iter:
        test_chunk = create_token_interval(
            curr_chunk.start_index, sentence.end_index
        )
        if self._tokens_exceed_buffer(test_chunk):
          self.sentence_iter = SentenceIterator(
              self.tokenized_text, curr_token_pos=curr_chunk.end_index
          )
          return TextChunk(
              token_interval=curr_chunk,
              document=self.document,
          )
        else:
          curr_chunk = test_chunk

    return TextChunk(
        token_interval=curr_chunk,
        document=self.document,
    )
````

ChunkIterator 是按字符预算输出 TextChunk 的有状态迭代器。__init__ 接受原始字符串、已有 TokenizedText 或由 document 提供文字；两者都缺失报错。字符串用传入 tokenizer_impl 切分；空 token 列表可能触发重新切分。随后建立 SentenceIterator，并把 tokenized_text 写回文档对象供所有块共享。broken_sentence 初始 False。

__iter__ 返回自身。_tokens_exceed_buffer 用区间两端字符位置相减，与 max_char_buffer 比较，等于预算仍允许。这里限制的是原文字数（Python 字符），并不是模型 token 数，也没有计算 prompt 模板所占预算。

__next__ 首先取下一句，并从首 token 建立候选。它的三个关键阶段如下：

1. 若首 token 单独已超预算，直接产出它，重建 SentenceIterator 从下一个 token 开始；根据句尾位置记录是否仍是半句。
2. 逐 token 扩大当前块，记住最近换行后的 token。超预算时有合适换行位置便退回该处（但不能退到块起点造成空块）；否则保留最后一个能装下的区间。重建句子迭代器从块尾继续，标记 broken_sentence。
3. 当前句装完后，若它是此前长句留下的后半部分，就清除标记而不继续拼下一句。否则尝试塞入更多完整句子；遇到装不下的一句，将游标恢复到当前块尾，下一次再读它。

最后返回引用原文档的 TextChunk。重建迭代器其实相当于图书管理员把书签拨回还没有装进信封的位置，避免试读失败后一整句丢失。

以 "A. B. Longword." 为例，预算 5 时前两句可以一起放进去，Longword 超预算却会独占一块，最后的句点另成一块。最大长度是软约束：不强行破坏 token 是更高的优先级。只有空白的文本没有 token，因此取下一块时自然结束。

## 8.6 切块和分批是不同的事

快递每一箱有重量上限，一辆车则可以装多箱。切块解决箱子的容量问题，分批解决一次调用处理几箱的问题。

源码出处：[langextract/chunking.py:265–279](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L265-L279)。

````python
def make_batches_of_textchunk(
    chunk_iter: Iterator[TextChunk],
    batch_length: int,
) -> Iterable[Sequence[TextChunk]]:
  """Processes chunks into batches of TextChunk for inference, using itertools.batched.

  Args:
    chunk_iter: Iterator of TextChunks.
    batch_length: Number of chunks to include in each batch.

  Yields:
    Batches of TextChunks.
  """
  for batch in more_itertools.batched(chunk_iter, batch_length):
    yield list(batch)
````

make_batches_of_textchunk 接收 TextChunk 迭代器和 batch_length，用 more_itertools.batched 每次取有限个块，转为列表 yield。最后一批不满也保留；这里没有负责真正模型调用或实现并行。这样上游按需产出，下游也可以按需消费。

## 8.7 练习

1. 给 pack 加上空输入和超长句测试，写出你希望程序满足的约束。
2. 在正式 ChunkIterator 中用很小的预算处理含超长英文单词的短信，检查区间连续性与每块的异常长度。
3. 检查带多空格、换行的文本，验证 chunk_text 与 sanitized_chunk_text 的区别。

信封已经准备好。下一章我们来处理一件看似琐碎、实则需要算法的事：同一个取件码出现两次，到底应该指向哪一个？
