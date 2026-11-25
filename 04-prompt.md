# 第 4 章　教会助手整理短信

现在有了答卷格式，但助手如何知道什么叫 pickup_code？如果只给字段名称，它也许会把所有数字填进去。你会怎样教新同事？告诉他规则，再给一个做好的示范，最后把待处理短信交给他。

## 4.1 拼出第一份任务单

```python
description = "提取快递公司、取件码。保留原文，不要猜测。"
example = 'Q: 顺丰快递，取件码 3821。\nA: {"extractions":[{"pickup_code":"3821"},{"company":"顺丰"}]}'
question = "Q: 中通，凭 7619 取件。"
prompt = "\n".join([description, example, question, "A: "])
print(prompt)
```

这段给模型的文本就叫提示词（prompt）。放入若干示范帮助模型理解任务的方式常叫少样本提示。它不是重新训练模型。

若每处理一条短信都重新手写这段字符串，很容易弄丢示范或格式。把不变的规则和示范保存起来。

源码出处：[langextract/prompting.py:39–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L39-L49)。

````python
@dataclasses.dataclass
class PromptTemplateStructured:
  """A structured prompt template for few-shot examples.

  Attributes:
    description: Instructions or guidelines for the LLM.
    examples: ExampleData objects demonstrating expected input→output behavior.
  """

  description: str
  examples: list[data.ExampleData] = dataclasses.field(default_factory=list)
````

description 是任务说明，examples 是前面理解的 ExampleData 列表。default_factory 避免共享列表。它没有继承模型或格式类，只描述任务本身。

## 4.2 让示范可以保存在文件里

有时规则由业务人员修改，而不是写程序的人。把配置放进 JSON/YAML 文件后，不必修改 Python 源码即可更新。

源码出处：[langextract/prompting.py:31–32](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L31-L32)。

````python
class PromptBuilderError(exceptions.LangExtractError):
  """Failure to build prompt."""
````

源码出处：[langextract/prompting.py:35–36](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L35-L36)。

````python
class ParseError(PromptBuilderError):
  """Prompt template cannot be parsed."""
````

两个异常没有自己的字段或方法。父类型表示提示构建失败，子类型进一步说明失败在模板解析。调用者可以捕获前者处理一类失败，也可以捕获后者针对性提示修正文件。父类型来源是项目统一错误基类，具体体系留到工程篇讨论。

源码出处：[langextract/prompting.py:52–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L52-L81)。

````python
def read_prompt_template_structured_from_file(
    prompt_path: str,
    format_type: data.FormatType = data.FormatType.YAML,
) -> PromptTemplateStructured:
  """Reads a structured prompt template from a file.

  Args:
    prompt_path: Path to a file containing PromptTemplateStructured data.
    format_type: The format of the file; YAML or JSON.

  Returns:
    A PromptTemplateStructured object loaded from the file.

  Raises:
    ParseError: If the file cannot be parsed successfully.
  """
  adapter = pydantic.TypeAdapter(PromptTemplateStructured)
  try:
    with pathlib.Path(prompt_path).open("rt") as f:
      data_dict = {}
      prompt_content = f.read()
      if format_type == data.FormatType.YAML:
        data_dict = yaml.safe_load(prompt_content)
      elif format_type == data.FormatType.JSON:
        data_dict = json.loads(prompt_content)
      return adapter.validate_python(data_dict)
  except Exception as e:
    raise ParseError(
        f"Failed to parse prompt template from file: {prompt_path}"
    ) from e
````

输入模板路径和格式（默认 YAML），返回 PromptTemplateStructured。pathlib.Path.open 读取文本，with 自动关闭文件，yaml.safe_load 或 json.loads 按格式读成 Python 数据。随后 Pydantic TypeAdapter 按类型声明检查并转换数据，例如将嵌套示范变成对应对象。如果文件不存在、语法错或对象结构不满足类型要求，except 将原错误转换为 ParseError，并用 from e 保留原因。

为什么不直接 `PromptTemplateStructured(**data_dict)`？这种做法不能自动验证和递归转换嵌套的 ExampleData。手写转换也可实现，依赖更少，却需要自行维护校验代码。

## 4.3 一份模板，生成很多任务单

源码出处：[langextract/prompting.py:84–138](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L84-L138)。

````python
@dataclasses.dataclass
class QAPromptGenerator:
  """Generates question-answer prompts from the provided template."""

  template: PromptTemplateStructured
  format_handler: format_handler.FormatHandler
  examples_heading: str = "Examples"
  question_prefix: str = "Q: "
  answer_prefix: str = "A: "

  def __str__(self) -> str:
    """Returns a string representation of the prompt with an empty question."""
    return self.render("")

  def format_example_as_text(self, example: data.ExampleData) -> str:
    """Formats a single example for the prompt.

    Args:
      example: The example data to format.

    Returns:
      A string representation of the example, including the question and answer.
    """
    question = example.text
    answer = self.format_handler.format_extraction_example(example.extractions)

    return "\n".join([
        f"{self.question_prefix}{question}",
        f"{self.answer_prefix}{answer}\n",
    ])

  def render(self, question: str, additional_context: str | None = None) -> str:
    """Generate a text representation of the prompt.

    Args:
      question: That will be presented to the model.
      additional_context: Additional context to include in the prompt. An empty
        string is ignored.

    Returns:
      Text prompt with a question to be presented to a language model.
    """
    prompt_lines: list[str] = [f"{self.template.description}\n"]

    if additional_context:
      prompt_lines.append(f"{additional_context}\n")

    if self.template.examples:
      prompt_lines.append(self.examples_heading)
      for ex in self.template.examples:
        prompt_lines.append(self.format_example_as_text(ex))

    prompt_lines.append(f"{self.question_prefix}{question}")
    prompt_lines.append(self.answer_prefix)
    return "\n".join(prompt_lines)
````

这个类把任务描述、示范和生成规则放在一起。template 是描述与示范，format_handler 是上一章的共享答卷格式对象，examples_heading 是示范标题，question_prefix 和 answer_prefix 分别标明问题与回答。默认使用 Q: 和 A:。

逐个研究方法：

- format_example_as_text 接收一张示范，取 example.text 当问题，用 format_handler.format_extraction_example 写出正确答案，再拼接两行文字。它不自行决定 JSON 字段、围栏或属性后缀，因此不会和解析端各自发展出不同协议。
- render 接收真正要处理的文字 question，以及可选背景。先加入任务说明；背景非空才添加；有示范时加入标题并循环生成示范问答；最后加入真正的问题和空的回答前缀。使用 join 一次连接列表，生成完整字符串，不修改模板。
- __str__ 用 render("") 产生问题内容为空的预览。方便调试，但不能误认为它代表一次真实的模型请求。

把生成逻辑放进类并非唯一方式。一个接收模板、格式、问题的纯函数也足够。但反复处理短信时，每次传模板与格式比较繁琐，类适合保存这两个共同依赖。

Annotator 的构造函数将创建 QAPromptGenerator；此处先不研究整个流水线，只记住它是纸上的任务单打印机。

## 4.4 一份长通知被分成两半

假设短信汇总很长，必须分成几段处理，出现这样的两段：

第一段：「顺丰的包裹已到达菜鸟驿站。」

第二段：「请你凭取件码 3821 领取。」

如果模型只看第二段，就不知道来自哪个公司。可以附上上一段的尾部作为参考；但是不同手机的通知绝不应该互相串联。这里的上下文（context）就是除了当前待处理片段以外的补充信息。注意它不必成为原文的一部分。

先写最简单的构建器。

源码出处：[langextract/prompting.py:141–176](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L141-L176)。

````python
class PromptBuilder:
  """Builds prompts for text chunks using a QAPromptGenerator.

  This base class provides a simple interface for prompt generation. Subclasses
  can extend this to add stateful behavior like cross-chunk context tracking.
  """

  def __init__(self, generator: QAPromptGenerator):
    """Initializes the builder with the given prompt generator.

    Args:
      generator: The underlying prompt generator to use.
    """
    self._generator = generator

  def build_prompt(
      self,
      chunk_text: str,
      document_id: str,
      additional_context: str | None = None,
  ) -> str:
    """Builds a prompt for the given chunk.

    Args:
      chunk_text: The text of the current chunk to process.
      document_id: Identifier for the source document.
      additional_context: Optional additional context from the document.

    Returns:
      The rendered prompt string ready for the language model.
    """
    del document_id  # Unused in base class.
    return self._generator.render(
        question=chunk_text,
        additional_context=additional_context,
    )
````

__init__ 保存现成的 QAPromptGenerator；build_prompt 接收片段文字、所属文档编号和附加背景。基础版本不需要文档编号，del 只是删除本地参数名，不会删除文档。它直接委托 render 返回任务单。

为什么多一层？参见源码说明：后续子类可以加入跨片段状态。这使使用方一直调用 build_prompt，却无需了解是否启用历史文字。

源码出处：[langextract/prompting.py:179–276](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L179-L276)。

````python
class ContextAwarePromptBuilder(PromptBuilder):
  """Prompt builder with cross-chunk context tracking.

  Extends PromptBuilder to inject text from the previous chunk into each
  prompt. This helps language models resolve coreferences across chunk
  boundaries (e.g., connecting "She" to "Dr. Sarah Johnson" from the
  previous chunk).

  Context is tracked per document_id, so multiple documents can be processed
  without context bleeding between them.
  """

  _CONTEXT_PREFIX = "[Previous text]: ..."

  def __init__(
      self,
      generator: QAPromptGenerator,
      context_window_chars: int | None = None,
  ):
    """Initializes the builder with context tracking configuration.

    Args:
      generator: The underlying prompt generator to use.
      context_window_chars: Number of characters from the previous chunk's
          tail to include as context. Defaults to None (disabled).
    """
    super().__init__(generator)
    self._context_window_chars = context_window_chars
    self._prev_chunk_by_doc_id: dict[str, str] = {}

  @property
  def context_window_chars(self) -> int | None:
    """Number of trailing characters from previous chunk to include."""
    return self._context_window_chars

  @override
  def build_prompt(
      self,
      chunk_text: str,
      document_id: str,
      additional_context: str | None = None,
  ) -> str:
    """Builds a prompt, injecting previous chunk context if enabled.

    Args:
      chunk_text: The text of the current chunk to process.
      document_id: Identifier for the source document (used to track context
          per document).
      additional_context: Optional additional context from the document.

    Returns:
      The rendered prompt string ready for the language model.
    """
    effective_context = self._build_effective_context(
        document_id, additional_context
    )
    prompt = self._generator.render(
        question=chunk_text,
        additional_context=effective_context,
    )
    self._update_state(document_id, chunk_text)
    return prompt

  def _build_effective_context(
      self,
      document_id: str,
      additional_context: str | None,
  ) -> str | None:
    """Combines previous chunk context with any additional context.

    Args:
      document_id: Identifier for the source document.
      additional_context: Optional additional context from the document.

    Returns:
      Combined context string, or None if no context is available.
    """
    context_parts: list[str] = []

    if self._context_window_chars and document_id in self._prev_chunk_by_doc_id:
      prev_text = self._prev_chunk_by_doc_id[document_id]
      window = prev_text[-self._context_window_chars :]
      context_parts.append(f"{self._CONTEXT_PREFIX}{window}")

    if additional_context:
      context_parts.append(additional_context)

    return "\n\n".join(context_parts) if context_parts else None

  def _update_state(self, document_id: str, chunk_text: str) -> None:
    """Stores current chunk as context for the next chunk in this document.

    Args:
      document_id: Identifier for the source document.
      chunk_text: The current chunk text to store.
    """
    if self._context_window_chars:
      self._prev_chunk_by_doc_id[document_id] = chunk_text
````

类继承基础构建器，_CONTEXT_PREFIX 是标记历史文字的固定前缀。__init__ 调用父构造器保存 generator，并增加 _context_window_chars（上一段末尾最多取多少字符）与 _prev_chunk_by_doc_id（按文档编号保存最近处理的片段）。必须按编号分别保存，而不是仅用一个 prev 变量。

context_window_chars 属性只读出配置。build_prompt 先调用 _build_effective_context 合并背景，再用当前片段 render，最后调用 _update_state 记下当前片段供下一次使用。顺序非常关键：先更新会错把当前片段当成上一段。

_build_effective_context 创建空列表；当窗口启用且编号已有历史时，切片取上一段尾部并加上前缀；有用户背景则随后加入。两段背景之间用空行隔开；完全没有则返回 None。

_update_state 只在窗口启用时把当前文本存到相应编号。它保存整段而不是仅保存尾巴；读取时才切片。这便于简单实现，但长期处理很多文档会保留每个文档最近一段文本。可替代方案是只保存截断后的窗口，或在文档结束清除状态。

想一想：窗口太短可能缺少关键人物和地点；太长会增加模型输入，同时混入无关内容。作者的文档说明明确将该功能用于跨片段指代解析。是否提高某一具体任务的准确率则需要测试。

## 4.5 实验

用前一章的 FormatHandler 构造一个 QAPromptGenerator，把它交给 ContextAwarePromptBuilder。依次构建 A 文档第一段、B 文档第一段、A 文档第二段的提示词。最后一个提示应该包含 A 的历史而不是 B 的历史。打印观察比直接请求付费模型更容易调试。
