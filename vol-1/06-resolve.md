# 第 6 章　把答卷整理回登记卡

助手交回的 JSON 已经过格式检查，意味着它是一组字典。但我们的原文证据、位置、分组等信息都保存在登记卡对象里。怎样从前者走到后者？

## 6.1 先做最小的转换

```python
from dataclasses import dataclass

@dataclass
class Record:
    kind: str
    text: str

def deserialize(items):
    records = []
    for group in items:
        for key, value in group.items():
            records.append(Record(key, value))
    return records

print(deserialize([{"pickup_code": "3821"}]))
```

当答卷里出现 pickup_code_attributes 时，程序也会将它当成一个抽取项。还可能收到空白内容、数字类型，或者希望按照指定序号重排列。因此格式正确只是第一关，我们还要理解每个字段的业务含义。

代码里称负责这一步的组件为 Resolver。把它想象成拿到答卷之后逐张录入登记卡的工作人员。

源码出处：[langextract/resolver.py:93–105](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L93-L105)。

````python
def _is_blank(text: str) -> bool:
  """Returns whether text has nothing to ground in the source.

  True for empty or whitespace-only text, and for text made only of control or
  format characters such as zero-width spaces, which `str.strip()` keeps.

  Args:
    text: The extraction text to check.
  """
  return all(
      char.isspace() or unicodedata.category(char) in _INVISIBLE_CATEGORIES
      for char in text
  )
````

直接 strip 能去掉很多空白，却不一定能去掉零宽空格等看不见的字符。这里逐字符检查是否为空白，或者 Unicode 类别属于 Cc（控制字符）和 Cf（格式字符）。all 在空字符串时也为真。因此空串、只有空白或不可见控制格式字符的串，都不值得建立证据登记卡。

源码出处：[langextract/resolver.py:228–229](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L228-L229)。

````python
class ResolverParsingError(exceptions.LangExtractError):
  """Error raised when content cannot be parsed as the given format."""
````

该异常没有额外成员，把录入过程的解析失败从其他项目异常中区分出来。

## 6.2 两个职责为何没有放进一个大函数

把文本变成登记卡，是解析；把登记卡中的文本对应回原短信坐标，是对齐。后者可能找不到正确位置，即使前者成功。比如助手回答 7691，格式和字段都正确，短信中却只有 7619。

源码出处：[langextract/resolver.py:108–225](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L108-L225)。

````python
class AbstractResolver(abc.ABC):
  """Resolves LLM text outputs into structured data."""

  # TODO: Review value and requirements for abstract class.
  def __init__(
      self,
      fence_output: bool = True,
      constraint: schema.Constraint = schema.Constraint(),
      format_type: data.FormatType = data.FormatType.JSON,
  ):
    """Initializes the BaseResolver.

    Delimiters are used for parsing text blocks, and are used primarily for
    models that do not have constrained-decoding support.

    Args:
      fence_output: Whether to expect/generate fenced output (```json or
        ```yaml). When True, the model is prompted to generate fenced output and
        the resolver expects it. When False, raw JSON/YAML is expected. If your
        model utilizes schema constraints, this can generally be set to False
        unless the constraint also accounts for code fence delimiters.
      constraint: Applies constraint when decoding the output. Defaults to no
        constraint.
      format_type: The format type for the output (JSON or YAML).
    """
    self._fence_output = fence_output
    self._constraint = constraint
    self._format_type = format_type

  @property
  def fence_output(self) -> bool:
    """Returns whether fenced output is expected."""
    return self._fence_output

  @fence_output.setter
  def fence_output(self, fence_output: bool) -> None:
    """Sets whether fenced output is expected.

    Args:
      fence_output: Whether to expect fenced output.
    """
    self._fence_output = fence_output

  @property
  def format_type(self) -> data.FormatType:
    """Returns the format type."""
    return self._format_type

  @format_type.setter
  def format_type(self, new_format_type: data.FormatType) -> None:
    """Sets a new format type."""
    self._format_type = new_format_type

  @abc.abstractmethod
  def resolve(
      self,
      input_text: str,
      **kwargs,
  ) -> Sequence[data.Extraction]:
    """Run resolve function on input text.

    Args:
        input_text: The input text to be processed.
        **kwargs: Additional arguments for subclass implementations.

    Returns:
        Annotated text in the form of Extractions.
    """

  @abc.abstractmethod
  def align(
      self,
      extractions: Sequence[data.Extraction],
      source_text: str,
      token_offset: int,
      char_offset: int | None = None,
      enable_fuzzy_alignment: bool = True,
      fuzzy_alignment_threshold: float = _FUZZY_ALIGNMENT_MIN_THRESHOLD,
      accept_match_lesser: bool = True,
      *,
      fuzzy_alignment_algorithm: str = _DEFAULT_FUZZY_ALGORITHM,
      fuzzy_alignment_min_density: float = _FUZZY_ALIGNMENT_MIN_DENSITY,
      **kwargs,
  ) -> Iterator[data.Extraction]:
    """Aligns extractions with source text, setting token/char intervals and alignment status.

    Uses exact matching first (difflib), then fuzzy alignment fallback if
    enabled.

    Alignment Status Results:
    - MATCH_EXACT: Perfect token-level match
    - MATCH_LESSER: Partial exact match (extraction longer than matched text)
    - MATCH_FUZZY: Best overlap window meets threshold (>=
    fuzzy_alignment_threshold)
    - None: No alignment found

    Args:
      extractions: Annotated extractions to align with the source text.
      source_text: The text in which to align the extractions.
      token_offset: The token_offset corresponding to the starting token index
        of the chunk.
      char_offset: The char_offset corresponding to the starting character index
        of the chunk.
      enable_fuzzy_alignment: Whether to use fuzzy alignment when exact matching
        fails.
      fuzzy_alignment_threshold: Minimum fraction of extraction tokens that
        must be matched (0-1). Default 0.75.
      accept_match_lesser: Whether to accept partial exact matches (MATCH_LESSER
        status).
      fuzzy_alignment_algorithm: "lcs" (default) or "legacy" (deprecated,
        will be removed in a future release). Keyword-only.
      fuzzy_alignment_min_density: Minimum ratio of matched tokens to source
        span length (LCS only). Keyword-only.
      **kwargs: Additional keyword arguments for provider-specific alignment.

    Yields:
      Aligned extractions with updated token intervals and alignment status.
    """
````

抽象类定义共同契约。__init__ 保存 _fence_output、_format_type 和 _constraint。前两项分别通过 getter/setter 提供旧式读写操作；约束保存为内部字段。resolve 必须由子类实现从答卷文本到登记卡序列的转换；align 必须实现将记录与原文片段对应并逐项返回。抽象方法中的参数说明让不同实现可以保持同样的接口，即使内部算法不同。

对齐接收片段在大文档中的 token_offset 和 char_offset：例如片段从原文第 100 个字符开始，片段内部第 5 个字符应对应原文第 105 个字符。模糊匹配允许文本不完全一致；threshold 表示覆盖程度要求，density 表示匹配内容在选中原文范围内是否过于稀疏。这些计算留到算法专章，而不是把参数名当作概念讲解。

源码出处：[langextract/resolver.py:243–294](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L243-L294)。

````python
def __init__(
    self,
    format_handler: fh.FormatHandler | None = None,
    extraction_index_suffix: str | None = None,
    **kwargs,  # Collect legacy parameters
):
  """Constructor.

  Args:
    format_handler: The format handler that knows how to parse output.
    extraction_index_suffix: Suffix identifying index keys that determine the
      ordering of extractions.
    **kwargs: Legacy parameters (fence_output, format_type, etc.) for backward
      compatibility. These will be used to create a FormatHandler if one is not
      provided. Support for these parameters will be removed in v2.0.0.
  """
  constraint = kwargs.pop("constraint", None)
  extraction_attributes_suffix = kwargs.pop(
      "extraction_attributes_suffix", None
  )

  if format_handler is None:
    if kwargs or extraction_attributes_suffix is not None:
      handler_kwargs = dict(kwargs)
      if extraction_attributes_suffix is not None:
        handler_kwargs["attribute_suffix"] = extraction_attributes_suffix
      format_handler = fh.FormatHandler.from_kwargs(**handler_kwargs)
      for param in [
          "fence_output",
          "format_type",
          "strict_fences",
          "require_extractions_key",
          "attribute_suffix",
      ]:
        kwargs.pop(param, None)
    else:
      format_handler = fh.FormatHandler()

  if kwargs:
    raise TypeError(
        f"got an unexpected keyword argument '{list(kwargs.keys())[0]}'"
    )

  constraint = constraint or schema.Constraint()
  super().__init__(
      fence_output=format_handler.use_fences,
      format_type=format_handler.format_type,
      constraint=constraint,
  )
  self.format_handler = format_handler
  self.extraction_index_suffix = extraction_index_suffix
  self._constraint = constraint
````

Resolver 继承 AbstractResolver，主成员 format_handler 保存格式处理对象，extraction_index_suffix 指定从何种后缀寻找显式排序号，_constraint 保存约束。构造器先取出旧约束与属性后缀，若没有现成 handler，则根据旧参数创建或采用默认 handler。识别的旧参数从 kwargs 删除，仍有未识别参数便报 TypeError，避免拼写错的配置被忽略。

然后用 handler 的配置初始化父类。这是一种兼容已有调用者的设计；如果从零设计项目，仅允许传 handler 会简单得多。

## 6.3 解析与错误处理

源码出处：[langextract/resolver.py:296–355](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L296-L355)。

````python
def resolve(
    self,
    input_text: str,
    suppress_parse_errors: bool = False,
    **kwargs,
) -> Sequence[data.Extraction]:
  """Runs resolve function on text with YAML/JSON extraction data.

  Args:
      input_text: The input text to be processed.
      suppress_parse_errors: When True, logs a warning and returns []
        on parse failures (FormatError) or schema/type errors (ValueError)
        instead of raising.
      **kwargs: Additional keyword arguments.

  Returns:
      Annotated text in the form of a sequence of data.Extraction objects.

  Raises:
      ResolverParsingError: If the content within the string cannot be parsed
      due to formatting errors, or if the parsed content is not as expected.
  """
  logging.debug("Starting resolver process for input text.")
  logging.debug("Input Text: %s", input_text)

  try:
    constraint = getattr(self, "_constraint", schema.Constraint())
    strict = getattr(constraint, "strict", False)
    extraction_data = self.format_handler.parse_output(
        input_text, strict=strict
    )
    logging.debug("Parsed content: %s", extraction_data)

  except exceptions.FormatError as e:
    if suppress_parse_errors:
      logging.warning(
          "Chunk omitted from results (parse error): %s. Set"
          " resolver_params={'suppress_parse_errors': False} to raise"
          " instead.",
          e,
      )
      return []
    raise ResolverParsingError(str(e)) from e

  try:
    processed_extractions = self.extract_ordered_extractions(extraction_data)
  except ValueError as e:
    if suppress_parse_errors:
      logging.warning(
          "Chunk omitted from results (schema error): %s. Set"
          " resolver_params={'suppress_parse_errors': False} to raise"
          " instead.",
          e,
      )
      return []
    raise ResolverParsingError(str(e)) from e

  logging.debug("Completed the resolver process.")

  return processed_extractions
````

调用 format_handler.parse_output 得到字典序列，strict 从约束对象读取（缺省 False）。再调用 extract_ordered_extractions 得到真正的登记卡。格式错误与业务值错误分别捕获。

suppress_parse_errors 为真时，记录警告、返回空列表；为假时，向上抛出 ResolverParsingError 并保留异常原因。读者应理解空结果的歧义：可能短信确实没有快递信息，也可能这一段回答格式坏掉。务必查看日志。公共 extract 默认设置压制片段解析错误，意味着一次模型坏答卷不必毁掉整个大文档，但也可能导致遗漏。

源码出处：[langextract/resolver.py:432–465](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L432-L465)。

````python
def string_to_extraction_data(
    self,
    input_string: str,
) -> Sequence[Mapping[str, fh.ExtractionValueType]]:
  """Parses a YAML or JSON-formatted string into extraction data.

  This method is kept for backward compatibility with tests.
  It delegates to the FormatHandler for actual parsing.

  Args:
      input_string: A string containing YAML or JSON content.

  Returns:
      Sequence[Mapping[str, fh.ExtractionValueType]]: A sequence of parsed objects.

  Raises:
      ResolverParsingError: If the content within the string cannot be parsed.
      ValueError: If the input is invalid or does not contain expected format.
  """
  if not input_string or not isinstance(input_string, str):
    logging.error("Input string must be a non-empty string.")
    raise ValueError("Input string must be a non-empty string.")

  try:
    constraint = getattr(self, "_constraint", schema.Constraint())
    strict = getattr(constraint, "strict", False)
    return self.format_handler.parse_output(input_string, strict=strict)

  except exceptions.FormatError as e:
    raise ResolverParsingError(str(e)) from e

  except Exception as e:
    logging.exception("Failed to parse content.")
    raise ResolverParsingError("Failed to parse content.") from e
````

这是旧接口保留的方法：接收非空字符串，取严格配置后委托 handler，返回字典序列，不负责变成登记卡。参数为空或不是字符串时 ValueError；格式错误转换为 ResolverParsingError；其他意外也记录并包装。注释明确说明它因测试兼容而保留。

## 6.4 知道哪些字段是正文

源码出处：[langextract/resolver.py:467–575](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L467-L575)。

````python
def extract_ordered_extractions(
    self,
    extraction_data: Sequence[Mapping[str, fh.ExtractionValueType]],
) -> Sequence[data.Extraction]:
  """Extracts and orders extraction data based on their associated indexes.

  This function processes a list of dictionaries, each containing pairs of
  extraction class keys and their corresponding values, along with optionally
  associated index keys (identified by the index_suffix). It sorts these pairs
  by their indices in ascending order and excludes pairs without an index key,
  returning a list of lists of tuples (extraction_class: str, extraction_text:
  str). Values made only of whitespace, control, or format characters (such
  as zero-width spaces) are skipped because they have no text to ground in
  the source.

  Args:
      extraction_data: A list of dictionaries. Each dictionary contains pairs
        of extraction class keys and their values, along with optional index
        keys.

  Returns:
      Extractions sorted by the index attribute or by order of appearance. If
      two
      extractions have the same index, their group order dictates the sorting
      order.
  Raises:
      ValueError: If an index is not an integer, attributes are not a dict
      or None, or extraction text is not a string, integer, or float.
  """
  logging.debug("Starting to extract and order extractions from data.")

  if not extraction_data:
    logging.debug("Received empty extraction data.")

  processed_extractions = []
  extraction_index = 0
  index_suffix = self.extraction_index_suffix
  attributes_suffix = self.format_handler.attribute_suffix

  for group_index, group in enumerate(extraction_data):
    for extraction_class, extraction_value in group.items():
      if index_suffix and extraction_class.endswith(index_suffix):
        if not isinstance(extraction_value, int):
          logging.debug(
              "Index must be an integer. Found: %s",
              type(extraction_value),
          )
          raise ValueError("Index must be an integer.")
        continue

      if attributes_suffix and extraction_class.endswith(attributes_suffix):
        if not isinstance(extraction_value, (dict, type(None))):
          logging.debug(
              "Attributes must be a dict or None. Found: %s",
              type(extraction_value),
          )
          raise ValueError(
              "Extraction value must be a dict or None for attributes."
          )
        continue

      if not isinstance(extraction_value, (str, int, float)):
        logging.debug(
            "Extraction text must be a string, integer, or float. Found: %s",
            type(extraction_value),
        )
        raise ValueError(
            "Extraction text must be a string, integer, or float."
        )

      if not isinstance(extraction_value, str):
        extraction_value = str(extraction_value)

      if _is_blank(extraction_value):
        # No text to ground in the source; treat it like an omitted key.
        logging.debug(
            "Empty value for %s. Skipping extraction.", extraction_class
        )
        continue

      if index_suffix:
        index_key = extraction_class + index_suffix
        extraction_index = group.get(index_key, None)
        if extraction_index is None:
          logging.debug(
              "No index value for %s. Skipping extraction.", extraction_class
          )
          continue
      else:
        extraction_index += 1

      attributes = None
      if attributes_suffix:
        attributes_key = extraction_class + attributes_suffix
        attributes = group.get(attributes_key, None)

      processed_extractions.append(
          data.Extraction(
              extraction_class=extraction_class,
              extraction_text=extraction_value,
              extraction_index=extraction_index,
              group_index=group_index,
              attributes=attributes,
          )
      )

  processed_extractions.sort(key=operator.attrgetter("extraction_index"))
  logging.debug("Completed extraction and ordering of extractions.")
  return processed_extractions
````

手工走一遍这个输入：

```python
items = [
    {"company": "顺丰", "company_attributes": {"type": "快递"}},
    {"pickup_code": 3821},
]
```

外层 enumerate 给第一组编号 0，第二组编号 1。内层遍历组中字段。

- 如果开启了显式编号后缀，编号字段必须是整数，然后跳过，因为它不是正文。
- 属性后缀字段必须为字典或 None，也是辅助字段，不单独创建记录。
- 正文值须是字符串、整数或浮点数；数字转换为字符串。因此 3821 得到 "3821"。
- _is_blank 拒绝无文字证据的值。
- 若启用编号后缀，则寻找同类别的编号，缺少编号就跳过；否则把自然顺序编号逐一递增。
- 用类别加属性后缀查出附加字典。
- 构造 Extraction，把类别、文字、排序号、分组号与属性一并保存。

最后 sort 根据 extraction_index 升序排序，Python 的排序是稳定的，相等编号保留原来顺序。可见排序号与组号不一样：前者描述结果顺序，后者描述原答卷中来自哪个组。

如果取件码存在前导 0，应要求模型以字符串输出；数值 0038 一旦以数字表达就无法保留原来的字符形式。这个实现做了类型容错，但无法找回已损失的信息。

## 6.5 把对齐算法暂时当成一位查找证据的同事

源码出处：[langextract/resolver.py:357–430](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L357-L430)。

````python
def align(
    self,
    extractions: Sequence[data.Extraction],
    source_text: str,
    token_offset: int,
    char_offset: int | None = None,
    enable_fuzzy_alignment: bool = True,
    fuzzy_alignment_threshold: float = _FUZZY_ALIGNMENT_MIN_THRESHOLD,
    accept_match_lesser: bool = True,
    tokenizer_inst: tokenizer_lib.Tokenizer | None = None,
    *,
    fuzzy_alignment_algorithm: str = _DEFAULT_FUZZY_ALGORITHM,
    fuzzy_alignment_min_density: float = _FUZZY_ALIGNMENT_MIN_DENSITY,
    exact_alignment_algorithm: str = _DEFAULT_EXACT_ALGORITHM,
    **kwargs,
) -> Iterator[data.Extraction]:
  """Aligns annotated extractions with source text.

  Args:
    extractions: Annotated extractions.
    source_text: The text chunk in which to align the extractions.
    token_offset: The starting token index of the chunk.
    char_offset: The starting character index of the chunk.
    enable_fuzzy_alignment: Whether to enable fuzzy alignment fallback.
    fuzzy_alignment_threshold: Minimum fraction of extraction tokens that
      must be matched (0-1). Default 0.75.
    accept_match_lesser: Whether to accept partial exact matches.
    tokenizer_inst: Optional tokenizer instance.
    fuzzy_alignment_algorithm: "lcs" (default) or "legacy" (deprecated,
      will be removed in a future release). Keyword-only.
    fuzzy_alignment_min_density: Minimum ratio of matched tokens to source
      span length (LCS only). Keyword-only.
    exact_alignment_algorithm: "dp" (default) or "difflib" (legacy).
      Keyword-only.
    **kwargs: Additional parameters.

  Yields:
      Iterator on aligned extractions.
  """
  logging.debug("Starting alignment process for provided chunk text.")

  if not extractions:
    logging.debug(
        "No extractions found in the annotated text; exiting alignment"
        " process."
    )
    return
  else:
    extractions_group = [extractions]

  aligner = WordAligner()
  aligned_yaml_extractions = aligner.align_extractions(
      extractions_group,
      source_text,
      token_offset,
      char_offset or 0,
      enable_fuzzy_alignment=enable_fuzzy_alignment,
      fuzzy_alignment_threshold=fuzzy_alignment_threshold,
      fuzzy_alignment_algorithm=fuzzy_alignment_algorithm,
      fuzzy_alignment_min_density=fuzzy_alignment_min_density,
      exact_alignment_algorithm=exact_alignment_algorithm,
      accept_match_lesser=accept_match_lesser,
      tokenizer_impl=tokenizer_inst,
  )
  logging.debug(
      "Aligned extractions count: %d",
      sum(len(group) for group in aligned_yaml_extractions),
  )

  for extraction in itertools.chain(*aligned_yaml_extractions):
    logging.debug("Yielding aligned extraction: %s", extraction)
    yield extraction

  logging.debug("Completed alignment process for the provided source_text.")
````

接收已经解析的登记卡、原文片段、两个坐标偏移及匹配设置。没有登记卡时直接结束生成器，否则把列表包进一个组，创建 WordAligner 并调用 align_extractions。传入自定义切分器、精确算法选择和模糊匹配选项。然后用 itertools.chain 将分组结果展开，逐张 yield。

这个方法本身不计算最长公共子序列，也不实现动态规划。分层让答案录入与寻找证据分别测试。后续需要真正研究 WordAligner，而不能把一句“调用算法”当作对算法的解释。

至此模型回答已经成为登记卡。下一阶段将集中研究如何处理长文本，以及如何证明卡片中填写的信息确实来自原文中相应的一段。
