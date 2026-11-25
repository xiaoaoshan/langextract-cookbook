# 第 3 章　让机器和人约定一张表

公司让两个员工整理快递短信，一个交 Excel，一个交 Word。即使内容正确，汇总程序也得分别处理。我们先约定答卷的形状，再检查交来的答卷是不是符合约定。

## 3.1 格式和数据不是一回事

```python
import json
record = {"company": "顺丰"}
wire_text = json.dumps(record, ensure_ascii=False)
assert isinstance(wire_text, str)
assert json.loads(wire_text) == record
```

YAML 是另一种可表示相同数据的文字格式。项目允许二选一。为了避免每处代码都自行猜测格式，把允许值放进枚举。

源码出处：[langextract/core/types.py:45–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/types.py#L45-L49)。

````python
class FormatType(enum.Enum):
  """Enumeration of prompt output formats."""

  YAML = 'yaml'
  JSON = 'json'
````

成员 value 是字符串 json 或 yaml；调用者可以用枚举成员比较格式，而不用到处比较大小写各异的字符串。这不是抽取类型，而是如何把抽取结果写成文本。

不过命令行配置、旧代码可能传入 " JSON " 而不是枚举，需要统一一下。

源码出处：[langextract/core/format_handler.py:49–64](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L49-L64)。

````python
def _normalize_format_type(
    format_type: data.FormatType | str | None,
) -> data.FormatType:
  """Normalize a FormatType enum, string, or None into a FormatType enum."""
  if format_type is None:
    return data.FormatType.JSON
  if isinstance(format_type, data.FormatType):
    return format_type
  if isinstance(format_type, str):
    return data.FormatType(format_type.strip().lower())
  if hasattr(format_type, "value") and isinstance(format_type.value, str):
    return data.FormatType(format_type.value.strip().lower())
  raise TypeError(
      "format_type must be a FormatType enum or string ('json', 'yaml'), "
      f"got {type(format_type).__name__}"
  )
````

None 默认 JSON，已经是枚举直接返回，字符串先 strip 再 lower 后交给枚举构造器；如果是其他有字符串 value 的对象，也取它的 value 进行同样转换。未知枚举值会由枚举构造器报 ValueError，不适合的对象则主动报 TypeError。比在每次 dumps/loads 前处理各种输入更集中；代价是入口规则需要维护。

## 3.2 约定答卷的外形

一份答卷可以直接是一组记录，也可以是一个叫 extractions 的外层容器。项目的示范通常采用后者：

```json
{
  "extractions": [
    {"company": "顺丰", "company_attributes": {}},
    {"pickup_code": "3821", "pickup_code_attributes": {"station": "菜鸟驿站"}}
  ]
}
```

类别做键，文本做值，类别后加 _attributes 表示附加信息。这和内部 Extraction 对象不一样，因为这里面向模型。JSON 的外壳还有一个好处：可以给答卷内容规定固定入口。

Markdown 中用三个反引号包围代码的写法叫代码围栏，例如标记为 json。对于喜欢写解释的模型，围栏可以隔开答案和其他文字。严格 JSON 输出的服务往往不需要围栏。

过去如果让提示词代码和解析代码分别决定这些规则，同一份答卷可能在写出后又无法读回。于是集中成一个 FormatHandler，让两个步骤共享它。

源码出处：[langextract/core/format_handler.py:84–122](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L84-L122)。

````python
def __init__(
    self,
    format_type: data.FormatType | str | None = data.FormatType.JSON,
    use_wrapper: bool = True,
    wrapper_key: str | None = None,
    use_fences: bool = True,
    attribute_suffix: str = data.ATTRIBUTE_SUFFIX,
    strict_fences: bool = False,
    allow_top_level_list: bool = True,
) -> None:
  """Initialize format handler.

  Args:
    format_type: Output format type enum or string ('json', 'yaml').
    use_wrapper: Whether to wrap extractions in a container dictionary.
      True: {"extractions": [...]}, False: [...]
    wrapper_key: Key name for the container dictionary. When use_wrapper=True:
      - If None: defaults to EXTRACTIONS_KEY ("extractions")
      - If provided: uses the specified key as container
      When use_wrapper=False, this parameter is ignored.
    use_fences: Whether to use ```json or ```yaml fences.
    attribute_suffix: Suffix for attribute fields.
    strict_fences: If True, require exact fence format. If False, be lenient
      with model output variations.
    allow_top_level_list: Allow top-level list when not strict and
      wrapper not required.
  """
  self.format_type = _normalize_format_type(format_type)
  self.use_wrapper = use_wrapper
  if use_wrapper:
    self.wrapper_key = (
        wrapper_key if wrapper_key is not None else data.EXTRACTIONS_KEY
    )
  else:
    self.wrapper_key = None
  self.use_fences = use_fences
  self.attribute_suffix = attribute_suffix
  self.strict_fences = strict_fences
  self.allow_top_level_list = allow_top_level_list
````

| 成员 | 用途 |
|---|---|
| format_type | JSON 或 YAML |
| use_wrapper | 是否要求外层对象 |
| wrapper_key | 外层字段名，默认 extractions；不用外层则为 None |
| use_fences | 是否处理代码围栏 |
| attribute_suffix | 属性字段的后缀 |
| strict_fences | 是否严格要求恰好一个合适围栏 |
| allow_top_level_list | 是否允许直接用列表作为答卷 |

构造器统一格式名，设置包装键，再保存其他配置。注意严格围栏和严格解析是不同的控制。前者关注边界标记，后者关注内容是否满足规则。

源码出处：[langextract/core/format_handler.py:124–132](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L124-L132)。

````python
def __repr__(self) -> str:
  return (
      "FormatHandler("
      f"format_type={self.format_type!r}, use_wrapper={self.use_wrapper}, "
      f"wrapper_key={self.wrapper_key!r}, use_fences={self.use_fences}, "
      f"attribute_suffix={self.attribute_suffix!r}, "
      f"strict_fences={self.strict_fences}, "
      f"allow_top_level_list={self.allow_top_level_list})"
  )
````

repr 是方便调试的表示。打印 handler 时可看到每项配置；它并不序列化提取记录，也不生成提示词。

## 3.3 写出一份示范答卷

源码出处：[langextract/core/format_handler.py:134–167](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L134-L167)。

````python
def format_extraction_example(
    self, extractions: list[data.Extraction]
) -> str:
  """Format extractions for a prompt example.

  Args:
    extractions: List of extractions to format

  Returns:
    Formatted string for the prompt
  """
  items = [
      {
          ext.extraction_class: ext.extraction_text,
          f"{ext.extraction_class}{self.attribute_suffix}": (
              ext.attributes or {}
          ),
      }
      for ext in extractions
  ]

  if self.use_wrapper and self.wrapper_key:
    payload = {self.wrapper_key: items}
  else:
    payload = items

  if self.format_type == data.FormatType.YAML:
    formatted = yaml.safe_dump(
        payload, default_flow_style=False, sort_keys=False
    )
  else:
    formatted = json.dumps(payload, indent=2, ensure_ascii=False)

  return self._add_fences(formatted) if self.use_fences else formatted
````

逐条将登记卡转换成一对字段：例如 pickup_code 对应文本，pickup_code_attributes 对应属性（没有则用空字典）。如果启用包装，把列表放在 wrapper_key 下。接着按选定格式序列化，JSON 的 ensure_ascii=False 让中文直接保留，indent=2 便于人阅读。最后按设置添加围栏。

源码出处：[langextract/core/format_handler.py:265–268](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L265-L268)。

````python
def _add_fences(self, content: str) -> str:
  """Add code fences around content."""
  fence_type = self.format_type.value
  return f"```{fence_type}\n{content.strip()}\n```"
````

只做一件事：用格式名构成开头标记，加入内容和结束标记。strip 去掉内容头尾空白。拆出这个小函数，主函数可以专注于数据变换。反过来说，只用一次的短逻辑也完全可以内联。

## 3.4 收回答卷，先找正文

如果助手回答「这是结果」再附上一段围栏内 JSON，直接 json.loads 会失败。先取围栏内容。项目正则以 lang 捕获语言标签，用 body 捕获内部文字，允许跨行。非贪婪匹配让它尽量停在遇到的结束围栏，而不是吞掉多个代码块。

源码出处：[langextract/core/format_handler.py:270–277](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L270-L277)。

````python
def _is_valid_language_tag(
    self, lang: str | None, valid_tags: dict[data.FormatType, set[str]]
) -> bool:
  """Check if language tag is valid for the format type."""
  if lang is None:
    return True
  tag = lang.strip().lower()
  return tag in valid_tags.get(self.format_type, set())
````

没有标签也算候选；有标签则小写并去空格，再查当前格式对应的合法集合。映射中 YAML 接受 yaml 和 yml，JSON 接受 json。

源码出处：[langextract/core/format_handler.py:296–351](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L296-L351)。

````python
def _extract_content(self, text: str) -> str:
  """Extract content from text, handling fences if configured.

  Args:
    text: Input text that may contain fenced blocks

  Returns:
    Extracted content

  Raises:
    FormatParseError: When fences required but not found or multiple
      blocks found.
  """
  if not self.use_fences:
    return text.strip()

  matches = list(_FENCE_RE.finditer(text))

  valid_tags = {
      data.FormatType.YAML: {_YAML_FORMAT, _YML_FORMAT},
      data.FormatType.JSON: {_JSON_FORMAT},
  }

  candidates = [
      m
      for m in matches
      if self._is_valid_language_tag(m.group("lang"), valid_tags)
  ]

  if self.strict_fences:
    if len(candidates) != 1:
      if len(candidates) == 0:
        raise exceptions.FormatParseError(
            "Input string does not contain valid fence markers."
        )
      else:
        raise exceptions.FormatParseError(
            "Multiple fenced blocks found. Expected exactly one."
        )
    return candidates[0].group("body").strip()

  if len(candidates) == 1:
    return candidates[0].group("body").strip()
  elif len(candidates) > 1:
    raise exceptions.FormatParseError(
        "Multiple fenced blocks found. Expected exactly one."
    )

  if matches:
    if not self.strict_fences and len(matches) == 1:
      return matches[0].group("body").strip()
    raise exceptions.FormatParseError(
        f"No {self.format_type.value} code block found."
    )

  return text.strip()
````

关闭围栏时直接返回去空白的原文。否则收集所有围栏，再筛选合法候选。严格模式必须恰好一个；宽松模式也不允许多个合法候选，因为选择哪一个会有歧义。仅一个不匹配的围栏，宽松模式允许尝试其内容。没有任何围栏时宽松模式直接尝试原文。

例如模型把 JSON 围栏错标为 text，宽松模式仍可尝试实际 JSON。这个容错提高兼容性，但可能把错误标记隐藏起来。另一种实现是要求服务总是返回纯 JSON，这样完全不需要本函数，但不是所有服务都能做到。

## 3.5 解析真正的数据

源码出处：[langextract/core/format_handler.py:279–294](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L279-L294)。

````python
def _parse_with_fallback(self, content: str, strict: bool):
  """Parse content, retrying without <think> tags on failure."""
  try:
    if self.format_type == data.FormatType.YAML:
      return yaml.safe_load(content)
    return json.loads(content)
  except (yaml.YAMLError, json.JSONDecodeError):
    if strict:
      raise
    # Reasoning models (DeepSeek-R1, QwQ) emit <think> tags before JSON.
    if _THINK_TAG_RE.search(content):
      stripped = _THINK_TAG_RE.sub("", content).strip()
      if self.format_type == data.FormatType.YAML:
        return yaml.safe_load(stripped)
      return json.loads(stripped)
    raise
````

首先按指定格式读取；失败后，如果严格模式开启则原样抛出异常。如果不是严格模式，再检查是否有完整的 think 标签块，若有则去掉这些块再按相同格式解析。这里并没有从 JSON 任意文本中猜答案，也不在第一次解析成功时删除合法内容。注释明确解释了这是为了兼容会在答案前输出思考标签的模型。

源码出处：[langextract/core/format_handler.py:169–263](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L169-L263)。

````python
def parse_output(
    self, text: str, *, strict: bool | None = None
) -> Sequence[Mapping[str, ExtractionValueType]]:
  """Parse model output to extract data.

  Args:
    text: Raw model output.
    strict: If True, enforce strict schema validation. When strict is
      True, always require wrapper object if wrapper_key is configured,
      reject top-level lists even if allow_top_level_list is True, and
      enforce exact format compliance.

  Returns:
    List of extraction dictionaries.

  Raises:
    FormatError: Various subclasses for specific parsing failures.
  """
  if not text:
    raise exceptions.FormatParseError("Empty or invalid input string.")

  content = self._extract_content(text)

  try:
    parsed = self._parse_with_fallback(content, strict)
  except (yaml.YAMLError, json.JSONDecodeError) as e:
    msg = (
        f"Failed to parse {self.format_type.value.upper()} content:"
        f" {str(e)[:200]}"
    )
    raise exceptions.FormatParseError(msg) from e

  if parsed is None:
    if self.use_wrapper:
      raise exceptions.FormatParseError(
          f"Content must be a mapping with an '{self.wrapper_key}' key."
      )
    else:
      raise exceptions.FormatParseError(
          "Content must be a list of extractions or a dict."
      )

  require_wrapper = self.wrapper_key is not None and (
      self.use_wrapper or bool(strict)
  )

  if isinstance(parsed, dict):
    if require_wrapper:
      if self.wrapper_key not in parsed:
        raise exceptions.FormatParseError(
            f"Content must contain an '{self.wrapper_key}' key."
        )
      items = parsed[self.wrapper_key]
    else:
      if data.EXTRACTIONS_KEY in parsed:
        items = parsed[data.EXTRACTIONS_KEY]
      elif self.wrapper_key and self.wrapper_key in parsed:
        items = parsed[self.wrapper_key]
      else:
        items = [parsed]
  elif isinstance(parsed, list):
    if require_wrapper and (strict or not self.allow_top_level_list):
      raise exceptions.FormatParseError(
          f"Content must be a mapping with an '{self.wrapper_key}' key."
      )
    if strict and self.use_wrapper:
      raise exceptions.FormatParseError(
          "Strict mode requires a wrapper object."
      )
    if not self.allow_top_level_list:
      raise exceptions.FormatParseError("Top-level list is not allowed.")
    # Some models return [...] instead of {"extractions": [...]}.
    items = parsed
  else:
    raise exceptions.FormatParseError(
        f"Expected list or dict, got {type(parsed)}"
    )

  if not isinstance(items, list):
    raise exceptions.FormatParseError(
        "The extractions must be a sequence (list) of mappings."
    )

  for item in items:
    if not isinstance(item, dict):
      raise exceptions.FormatParseError(
          "Each item in the sequence must be a mapping."
      )
    for k in item.keys():
      if not isinstance(k, str):
        raise exceptions.FormatParseError(
            "All extraction keys must be strings (got a non-string key)."
        )

  return items
````

现在把所有零件连起来：

1. 空字符串直接失败。
2. _extract_content 取得正文。
3. _parse_with_fallback 把正文变为 Python 对象，并把 JSON/YAML 语法错误统一包装成 FormatParseError。
4. 若得到字典且需要外层键，就取该键对应的值；否则识别默认 extractions、配置键，或将单个字典包装成列表。
5. 若得到列表，检查是否允许省略包装。
6. 确认最后确实是列表，每个元素是字典，每个键是字符串。

为什么 json.loads 成功还不够？因为 `123`、`null` 都是合法 JSON，可不是我们想要的登记卡集合。语法正确和符合业务结构是两件不同的事。此处只验证外形；类别和值的具体要求留给后续解析步骤。

## 3.6 为什么有两种创建方式？

旧版本把格式参数直接交给后续的答案解析器。若直接删掉旧参数，升级后的用户程序就无法运行。因此作者添加兼容转换，并通过弃用警告提醒用户迁移。这种做法不是核心抽取算法，却是实际软件长期维护的重要部分。

源码出处：[langextract/core/format_handler.py:365–440](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L365-L440)。

````python
@classmethod
def from_resolver_params(
    cls,
    *,
    resolver_params: dict | None,
    base_format_type: data.FormatType,
    base_use_fences: bool,
    base_attribute_suffix: str = data.ATTRIBUTE_SUFFIX,
    base_use_wrapper: bool = True,
    base_wrapper_key: str | None = data.EXTRACTIONS_KEY,
    warn_on_legacy: bool = True,
) -> tuple[FormatHandler, dict]:
  """Create FormatHandler from resolver_params with legacy support.

  This method handles backward compatibility for legacy resolver parameters
  and will be removed in v2.0.0.

  Args:
    resolver_params: May contain legacy keys or a 'format_handler'.
    base_format_type: Default format when not overridden.
    base_use_fences: Default fence usage from the model.
    base_attribute_suffix: Default attribute suffix.
    base_use_wrapper: Default wrapper behavior.
    base_wrapper_key: Default wrapper key.
    warn_on_legacy: If True, emit DeprecationWarnings.

  Returns:
    (format_handler, remaining_resolver_params)
  """
  rp = dict(resolver_params or {})

  if rp.get("format_handler") is not None:
    handler = rp.pop("format_handler")
    for k in list(rp.keys()):
      if k in cls._LEGACY_FORMAT_KEYS:
        rp.pop(k, None)
    return handler, rp

  kwargs = {
      "format_type": base_format_type,
      "use_fences": base_use_fences,
      "attribute_suffix": base_attribute_suffix,
      "use_wrapper": base_use_wrapper,
      "wrapper_key": base_wrapper_key if base_use_wrapper else None,
  }

  mapping = {
      "fence_output": "use_fences",
      "format_type": "format_type",
      "strict_fences": "strict_fences",
      "require_extractions_key": "use_wrapper",
      "extraction_attributes_suffix": "attribute_suffix",
      "attribute_suffix": "attribute_suffix",
  }

  used_legacy = []
  for legacy_key, fh_key in mapping.items():
    if legacy_key in rp and rp[legacy_key] is not None:
      val = rp.pop(legacy_key)
      if fh_key == "format_type":
        val = _normalize_format_type(val)
      kwargs[fh_key] = val
      used_legacy.append(legacy_key)

  if warn_on_legacy and used_legacy:
    warnings.warn(
        "Resolver legacy params are deprecated and will be removed in"
        f" v2.0.0: {used_legacy}. Pass a FormatHandler explicitly via"
        " `resolver_params={'format_handler': FormatHandler(...)}` or rely"
        " on defaults configured by the model.",
        DeprecationWarning,
        stacklevel=3,
    )

  handler = cls(**kwargs)
  return handler, rp
````

输入旧参数字典和默认配置，输出 handler 与剩余参数。先复制字典，避免 pop 改动调用者的原对象。若已有 handler，直接优先使用并删去旧格式键。否则从默认值建立 kwargs，再按 mapping 将旧名称转成新名称。已使用的旧键被移出，并按设置发警告。非格式参数留给其他组件。

这就是为什么调用者可以统一传入一包配置，而不同组件只拿自己需要的部分。

源码出处：[langextract/core/format_handler.py:442–495](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L442-L495)。

````python
@classmethod
def from_kwargs(cls, **kwargs) -> FormatHandler:
  """Create FormatHandler from legacy resolver keyword arguments.

  This method will be removed in v2.0.0.

  Args:
    **kwargs: Legacy parameters like fence_output, format_type, etc.

  Returns:
    FormatHandler configured with legacy parameters.
  """
  legacy_params = {
      "fence_output",
      "format_type",
      "strict_fences",
      "require_extractions_key",
  }
  used_legacy = legacy_params.intersection(kwargs.keys())

  if used_legacy:
    warnings.warn(
        f"Using legacy Resolver parameters {used_legacy} is deprecated. "
        "Please use FormatHandler directly. "
        "This compatibility layer will be removed in v2.0.0.",
        DeprecationWarning,
        stacklevel=3,
    )

  fence_output = kwargs.pop("fence_output", True)
  format_type = kwargs.pop("format_type", None)
  strict_fences = kwargs.pop("strict_fences", False)
  require_extractions_key = kwargs.pop("require_extractions_key", True)
  attribute_suffix = kwargs.pop("attribute_suffix", data.ATTRIBUTE_SUFFIX)

  if format_type is None:
    format_type = data.FormatType.JSON
  elif hasattr(format_type, "value"):
    pass
  else:
    format_type = (
        data.FormatType.JSON
        if str(format_type).lower() == "json"
        else data.FormatType.YAML
    )

  return cls(
      format_type=format_type,
      use_wrapper=require_extractions_key,
      wrapper_key=data.EXTRACTIONS_KEY if require_extractions_key else None,
      use_fences=fence_output,
      strict_fences=strict_fences,
      attribute_suffix=attribute_suffix,
  )
````

另一条旧入口从关键字参数直接构造 handler。检查一组旧名字，发出警告，再用 pop 获得旧配置及其默认值。没有格式则 JSON；具有 value 的对象交由构造器继续规范化；其他值按是否等于 json 选择 JSON 或 YAML。后一行为与严格的 _normalize_format_type 不完全相同，是此兼容路径要注意的地方。

返回的对象把旧 require_extractions_key 映射为 use_wrapper。源码明确标注这些兼容方法计划在 2.0.0 删除。迁移完成后删除适配代码，才能收回维护成本。

## 3.7 实验

在项目安装后，创建关闭围栏的 FormatHandler，用同一个对象先 format_extraction_example，再 parse_output。分别尝试传入合法答卷、空字符串、只含数字的 JSON 和缺失外层键的对象，观察哪一步拒绝数据。
