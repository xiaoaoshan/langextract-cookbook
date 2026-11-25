# 第 13 章　让答卷格式得到更严格保证

助手已经学会把短信变成 JSON，但这一次它写了 {"pickup_code": 3821}，下一次却写 {"code": "3821"}。两份都是合法 JSON，却不一定是我们的登记表。

## 13.1 JSON 格式正确，不代表结构正确

```python
import json

answer = json.loads('{"extractions": [{"pickup_code": 3821}]}')
assert isinstance(answer["extractions"], list)
assert not isinstance(answer["extractions"][0]["pickup_code"], str)
```

取件码应当是字符串，不只是因为可能有字母，还因为 "0038" 不能被数字 38 代替。我们要写一份表格规格，不仅要求合法 JSON，也要求字段出现、类型正确。

JSON Schema 是用 JSON 数据描述另一份 JSON 数据的结构。例如：

```json
{
  "type": "object",
  "properties": {"pickup_code": {"type": "string"}},
  "required": ["pickup_code"],
  "additionalProperties": false
}
```

type 指定类型，properties 描述属性，required 表示必填，additionalProperties=false 禁止没有声明的属性。properties 不会自动意味着必填。

我们把规格传给支持结构化输出的模型服务，它可以按服务支持的方式约束生成。这个过程不同于只把「请回复 JSON」写在 prompt 里。格式约束也不能保证取件码真实存在，仍需要第 9、10 章的证据对齐。

## 13.2 先写一个小规格生成器

```python
def item_schema(name):
    return {
        "type": "object",
        "properties": {name: {"type": "string"}},
        "required": [name],
        "additionalProperties": False,
    }

def envelope(item):
    return {
        "type": "object",
        "properties": {"extractions": {"type": "array", "items": item}},
        "required": ["extractions"],
        "additionalProperties": False,
    }

schema = envelope(item_schema("pickup_code"))
assert schema["properties"]["extractions"]["items"]["required"] == ["pickup_code"]
```

回忆第 3 章，LangExtract 答卷最外层有 extractions 数组。数组每项以类别名为键，而不是内部 Extraction 的 extraction_class 和 extraction_text。这两个数据格式不要弄混。

## 13.3 让规格可以由用户编写

正式代码 core/output_schema.py 提供便利函数，返回普通 Python 字典，而不是先要求用户了解各种服务商的 Schema 类。

源码出处：[langextract/core/output_schema.py:189–197](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L189-L197)。

````python
def _copy_schema_mapping(
    schema_mapping: core_types.JsonSchema,
    argument_name: str,
) -> dict[str, Any]:
  if not isinstance(schema_mapping, Mapping):
    raise exceptions.InferenceConfigError(
        f"{argument_name} must be a mapping describing a JSON schema."
    )
  return copy.deepcopy(dict(schema_mapping))
````

_copy_schema_mapping 检查输入是 Mapping，否则抛 InferenceConfigError 并带上参数名；再转换成 dict 深复制。copy.deepcopy 避免用户后续修改嵌套属性影响已经生成的规格。

源码出处：[langextract/core/output_schema.py:249–322](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L249-L322)。

````python
def extraction_item_schema(
    extraction_class: str,
    *,
    attributes: Mapping[str, core_types.JsonSchema] | None = None,
    additional_properties: bool = False,
) -> dict[str, Any]:
  """Builds a schema for one LangExtract extraction object.

  Pair this with `extractions_schema()` to produce the full output envelope.

  Args:
    extraction_class: Extraction class name, such as "emotion".
    attributes: Optional mapping from attribute name to JSON schema.
    additional_properties: Value for each generated object's
      additionalProperties setting, including both the outer extraction item
      object and the nested "<extraction_class>_attributes" object.

  Returns:
    A JSON schema dictionary for an item in the "extractions" array.

  Raises:
    InferenceConfigError: If arguments cannot describe a valid extraction
      object schema.
  """
  if not isinstance(extraction_class, str) or not extraction_class:
    raise exceptions.InferenceConfigError(
        "extraction_class must be a non-empty string."
    )
  if extraction_class.endswith(data.ATTRIBUTE_SUFFIX):
    raise exceptions.InferenceConfigError(
        "extraction_class must not end with reserved suffix "
        f"{data.ATTRIBUTE_SUFFIX!r}."
    )
  if extraction_class in _RESERVED_EXTRACTION_ITEM_KEYS:
    raise exceptions.InferenceConfigError(
        "extraction_class must not use reserved generic key "
        f"{extraction_class!r}."
    )
  if attributes is not None and not isinstance(attributes, Mapping):
    raise exceptions.InferenceConfigError(
        "attributes must be a mapping from names to JSON schemas."
    )
  if attributes is not None and not attributes:
    attributes = None

  properties: dict[str, Any] = {extraction_class: {"type": "string"}}
  required = [extraction_class]

  if attributes is not None:
    attr_properties = {}
    for attr_name, attr_schema in attributes.items():
      if not isinstance(attr_name, str) or not attr_name:
        raise exceptions.InferenceConfigError(
            "attribute names must be non-empty strings."
        )
      attr_properties[attr_name] = _copy_schema_mapping(
          attr_schema, f"attributes[{attr_name!r}]"
      )

    attributes_field = f"{extraction_class}{data.ATTRIBUTE_SUFFIX}"
    properties[attributes_field] = {
        "type": "object",
        "properties": attr_properties,
        "required": list(attr_properties),
        "additionalProperties": additional_properties,
    }
    required.append(attributes_field)

  return {
      "type": "object",
      "properties": properties,
      "required": required,
      "additionalProperties": additional_properties,
  }
````

extraction_item_schema 根据类别名生成字符串属性。非空字符串是基本要求；不允许名称以 _attributes 结尾，也不允许使用内部保留键 attributes、extraction_class、extraction_text。因为解析器会把类别与属性后缀关联，混用会产生歧义。

可选 attributes 必须为 Mapping，空映射视为无属性。每个属性名需非空字符串，值是用户提供的子 schema，经防御性复制后组成 "<class>_attributes" 对象。它及其所有声明属性都加入 required，additionalProperties 参数同时控制外层及属性对象。这个函数不推断值类型，调用者可以自己表达枚举、数值、null 等规则。

源码出处：[langextract/core/output_schema.py:200–246](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L200-L246)。

````python
def extractions_schema(
    item_schema: core_types.JsonSchema,
    *additional_item_schemas: core_types.JsonSchema,
    additional_properties: bool = False,
) -> dict[str, Any]:
  """Wraps extraction item schemas in LangExtract's output envelope.

  Args:
    item_schema: JSON schema for each entry in the "extractions" array. When
      more than one item schema is provided, the helper wraps them in `anyOf`.
      Hand-written item schemas are copied as-is and should include any
      provider-required fields, such as `required` and `additionalProperties`
      for OpenAI strict mode.
    *additional_item_schemas: Additional item schemas for heterogeneous
      extraction classes.
    additional_properties: Value for the envelope's additionalProperties
      setting. Defaults to False so helper output works with OpenAI strict
      structured outputs and Gemini's JSON Schema path.

  Returns:
    A JSON schema dictionary suitable for `extract(output_schema=...)`.
  """
  copied_item_schemas = [
      _copy_schema_mapping(item_schema, "item_schema"),
      *[
          _copy_schema_mapping(
              schema_mapping, f"additional_item_schemas[{index}]"
          )
          for index, schema_mapping in enumerate(additional_item_schemas)
      ],
  ]
  if len(copied_item_schemas) == 1:
    items_schema = copied_item_schemas[0]
  else:
    items_schema = {"anyOf": copied_item_schemas}

  return {
      "type": "object",
      "properties": {
          data.EXTRACTIONS_KEY: {
              "type": "array",
              "items": items_schema,
          }
      },
      "required": [data.EXTRACTIONS_KEY],
      "additionalProperties": additional_properties,
  }
````

extractions_schema 接收一个或多个项规格，逐个深复制。一个时直接作为 items；多个时用 anyOf，使数组元素可以匹配任意一种对象规格。外层生成固定 extractions 数组且要求该字段，默认禁止额外字段。

例如项目接口可以这样表达取件码和快递员两种类别（需安装固定版本项目）：

````python
import langextract as lx

code = lx.schema.extraction_item_schema(
    "pickup_code", attributes={"station": {"type": "string"}}
)
person = lx.schema.extraction_item_schema("courier")
output_schema = lx.schema.extractions_schema(code, person)
````

这里只构造规格，不需要模型账号。手写项 schema 仍需遵守所选服务商支持的规则，工具不会自动修复所有不兼容项。

## 13.4 用户手工传来的规格是否适合我们的录入员？

源码出处：[langextract/core/output_schema.py:80–85](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L80-L85)。

````python
def _is_string_sequence(value: Any) -> bool:
  return (
      isinstance(value, Sequence)
      and not isinstance(value, str)
      and all(isinstance(item, str) for item in value)
  )
````

_is_string_sequence 判断 value 是非字符串的 Sequence，且每项都是字符串。单个字符串本身是字符序列，但不能把 "extractions" 当成 ["extractions"]。空序列可通过此辅助检查，后续还检查实际包含必要键。

源码出处：[langextract/core/output_schema.py:88–105](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L88-L105)。

````python
def _item_schema_branches(items_value: Any) -> list[Mapping[str, Any]]:
  """Returns object-schema branches from a direct object or anyOf union."""
  if not isinstance(items_value, Mapping):
    return []
  if items_value.get("type") == "object":
    return [items_value]
  branches = items_value.get("anyOf")
  if (
      isinstance(branches, Sequence)
      and not isinstance(branches, str)
      and branches
      and all(
          isinstance(branch, Mapping) and branch.get("type") == "object"
          for branch in branches
      )
  ):
    return list(branches)
  return []
````

_item_schema_branches 接受 Mapping，若 type 为 object 则返回单元素列表；否则尝试 anyOf，要求非空序列、不是字符串，且所有分支都是对象 Mapping。不能识别的结构返回 []。它只支持 inline 对象或对象 anyOf，不是完整 JSON Schema 解析器。

源码出处：[langextract/core/output_schema.py:108–186](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L108-L186)。

````python
def validate_output_schema(
    output_schema: core_types.JsonSchema,
) -> dict[str, Any]:
  """Validates the LangExtract output envelope and returns an isolated copy.

  LangExtract's resolver parses a top-level JSON object with an "extractions"
  array whose items are objects keyed by extraction class, optionally with
  "<class>_attributes" objects. This check only enforces that envelope; the
  provider API validates the JSON schema itself.

  Args:
    output_schema: User-provided JSON schema for the raw model output.

  Returns:
    A deep copy of output_schema.

  Raises:
    InferenceConfigError: If output_schema cannot describe LangExtract's
      output envelope.
  """
  if not isinstance(output_schema, Mapping):
    raise exceptions.InferenceConfigError(
        "output_schema must be a mapping describing a JSON object."
    )

  schema_dict = copy.deepcopy(dict(output_schema))
  if not schema_dict:
    raise exceptions.InferenceConfigError("output_schema must not be empty.")
  if schema_dict.get("type") != "object":
    raise exceptions.InferenceConfigError(
        "output_schema top-level type must be 'object'."
    )

  required = schema_dict.get("required")
  if not _is_string_sequence(required) or data.EXTRACTIONS_KEY not in required:
    raise exceptions.InferenceConfigError(
        "output_schema top-level required must include 'extractions'."
    )

  properties = schema_dict.get("properties")
  if not isinstance(properties, Mapping):
    raise exceptions.InferenceConfigError(
        "output_schema top-level properties must be a mapping."
    )

  extractions_property = properties.get(data.EXTRACTIONS_KEY)
  if (
      not isinstance(extractions_property, Mapping)
      or extractions_property.get("type") != "array"
  ):
    raise exceptions.InferenceConfigError(
        "output_schema must declare 'extractions' as an array property."
    )

  item_branches = _item_schema_branches(extractions_property.get("items"))
  if not item_branches:
    raise exceptions.InferenceConfigError(
        "output_schema must declare 'extractions.items' as an inline object "
        "schema or an inline anyOf of object schemas."
    )

  for branch in item_branches:
    branch_properties = branch.get("properties")
    if not isinstance(branch_properties, Mapping) or not branch_properties:
      raise exceptions.InferenceConfigError(
          "output_schema extraction items must declare extraction-class "
          "properties, such as 'condition'."
      )
    reserved_keys = sorted(
        set(branch_properties).intersection(_RESERVED_EXTRACTION_ITEM_KEYS)
    )
    if reserved_keys:
      raise exceptions.InferenceConfigError(
          "output_schema extraction items use extraction-class keys such as "
          "'condition', not LangExtract's internal field names: "
          + ", ".join(reserved_keys)
      )

  return schema_dict
````

validate_output_schema 首先要求 Mapping 并深复制，拒绝空映射及非 object 的顶层类型；required 必须包含 extractions，properties 必须是映射，extractions 必须为 array。

接着调用 _item_schema_branches 分析 items，每个分支必须声明非空 properties，并检查有无内部保留键。如果通过就返回隔离副本，而不是返回布尔值。

它的任务是保证表格外壳能被 LangExtract 现有 resolver 处理，完整的 JSON Schema 语法、服务支持的关键字、严格模式等由服务商进一步约束。例如它不检查每个字段的全部规则是否互相矛盾。

## 13.5 规格与解析格式必须一致

源码出处：[langextract/core/output_schema.py:39–47](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L39-L47)。

````python
def is_json_format_type(format_type: Any) -> bool:
  """Returns True when format_type is JSON or unset."""
  if format_type is None:
    return True
  if isinstance(format_type, data.FormatType):
    return format_type is data.FormatType.JSON
  if isinstance(format_type, str):
    return format_type.lower() == data.FormatType.JSON.value
  return False
````

is_json_format_type 对 None（未指定）返回 True，对 FormatType 枚举用身份比较，对字符串进行小写化后比较 json，其他类型返回 False。

源码出处：[langextract/core/output_schema.py:50–77](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L50-L77)。

````python
def validate_output_schema_format_handler(format_handler: Any) -> None:
  """Rejects resolver output settings that conflict with output_schema.

  output_schema constrains the model to LangExtract's raw JSON envelope, so
  the resolver must parse unfenced JSON with the default "extractions"
  wrapper and "_attributes" suffix.

  Args:
    format_handler: Normalized FormatHandler built from resolver params.

  Raises:
    InferenceConfigError: If the handler cannot parse the schema envelope.
  """
  if format_handler.use_fences:
    raise exceptions.output_schema_fence_error()
  if not is_json_format_type(format_handler.format_type):
    raise exceptions.output_schema_format_error()
  if (
      not format_handler.use_wrapper
      or format_handler.wrapper_key != data.EXTRACTIONS_KEY
      or getattr(format_handler, "attribute_suffix", data.ATTRIBUTE_SUFFIX)
      != data.ATTRIBUTE_SUFFIX
  ):
    raise exceptions.InferenceConfigError(
        "output_schema requires LangExtract's default JSON envelope: keep "
        f"use_wrapper=True, wrapper_key={data.EXTRACTIONS_KEY!r}, and "
        f"attribute_suffix={data.ATTRIBUTE_SUFFIX!r}."
    )
````

validate_output_schema_format_handler 拒绝 Markdown 围栏、非 JSON、禁用 wrapper、自定义 wrapper_key 或属性后缀不一致的配置。显式 schema 规定的是裸 JSON 默认外壳，如果 resolver 期待另一套数据，即使服务正确生成也无法录入。

这里有一个设计原则：尽可能在执行前发现配置冲突，而不是模型付费调用之后再报告格式错误。

## 13.6 为什么需要一层服务商适配？

不同助手的订货单名称不同。Gemini 可以用 response_schema 或 response_json_schema，OpenAI 需要 response_format；有些模型只能保证合法 JSON，而不认识字段约束。

我们需要共同接口：构造规格对象，然后转成服务商所需的配置，而不是让 extract 内部写大量 if/else。

源码出处：[langextract/core/schema.py:39–105](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L39-L105)。

````python
class BaseSchema(abc.ABC):
  """Abstract base class for generating structured constraints from examples."""

  @classmethod
  @abc.abstractmethod
  def from_examples(
      cls,
      examples_data: Sequence[data.ExampleData],
      attribute_suffix: str = data.ATTRIBUTE_SUFFIX,
  ) -> BaseSchema:
    """Factory method to build a schema instance from example data."""

  @classmethod
  def from_schema_dict(cls, output_schema: types.JsonSchema) -> BaseSchema:
    """Build a schema instance from a LangExtract output envelope schema.

    Providers override this to opt in to user-provided schemas.
    """
    raise NotImplementedError(
        f"{cls.__name__} does not support user-provided output_schema."
    )

  @abc.abstractmethod
  def to_provider_config(self) -> dict[str, Any]:
    """Convert schema to provider-specific configuration.

    Returns:
      Dictionary of provider kwargs (e.g., response_schema for Gemini).
      Should be a pure data mapping with no side effects.
    """

  def output_schema_reserved_provider_kwargs(self) -> frozenset[str]:
    """Provider kwargs that would conflict with explicit output_schema."""
    return frozenset(self.to_provider_config())

  @property
  @abc.abstractmethod
  def requires_raw_output(self) -> bool:
    """Whether this schema outputs raw JSON/YAML without fence markers.

    When True, the provider emits syntactically valid JSON directly.
    When False, the provider needs fence markers for structure.
    """

  def validate_format(self, format_handler: fh.FormatHandler) -> None:
    """Validate format compatibility and warn about issues.

    Override in subclasses to check format settings.
    Default implementation does nothing (no validation needed).

    Args:
      format_handler: The format configuration to validate.
    """

  def sync_with_provider_kwargs(self, kwargs: dict[str, Any]) -> None:
    """Hook to update schema state based on provider kwargs.

    This allows schemas to adjust their behavior based on caller overrides.
    For example, FormatModeSchema uses this to sync its format when the caller
    overrides it, ensuring requires_raw_output stays accurate.

    Default implementation does nothing. Override if your schema needs to
    respond to provider kwargs.

    Args:
      kwargs: The effective provider kwargs after merging.
    """
````

BaseSchema 是抽象基类，展示了同一接口中可以同时存在强制实现、默认实现和可选扩展点：

- from_examples 是抽象类方法，用示范建立规格实例，而不需要已有实例。不同服务能采用不同生成策略。
- from_schema_dict 默认抛 NotImplementedError；支持显式 schema 的服务才覆盖它。这比默默忽略用户规格安全，也为旧插件保留兼容性。
- to_provider_config 是抽象方法，约定生成纯配置映射，不应发出网络请求。
- output_schema_reserved_provider_kwargs 默认取配置映射的键组成 frozenset，用来标识不能同时使用的重复规格控制参数；服务商可扩充此集合。
- requires_raw_output 是抽象属性，用来协调是否需要围栏。
- validate_format 默认什么也不做，是让子类检查格式兼容性的钩子。
- sync_with_provider_kwargs 同样默认不做事，允许子类在实际参数覆盖时同步内部状态。

源码出处：[langextract/core/schema.py:108–115](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L108-L115)。

````python
def mark_from_output_schema(schema_instance: BaseSchema) -> None:
  """Mark a schema as coming from a user-authored output_schema when possible."""
  if getattr(schema_instance, "from_output_schema", False):
    return
  try:
    setattr(schema_instance, "from_output_schema", True)
  except (AttributeError, TypeError):
    return
````

mark_from_output_schema 尝试给实例设置 from_output_schema=True；已有真值则直接返回，遇到不允许动态属性的对象（AttributeError 或 TypeError）则放弃。这使显式规格来源可追踪，又不强制旧实现必须提供可写属性。

源码出处：[langextract/core/schema.py:118–164](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L118-L164)。

````python
class FormatModeSchema(BaseSchema):
  """Generic schema for providers that support format modes (JSON/YAML).

  This schema doesn't enforce structure, only output format. Useful for
  providers that can guarantee syntactically valid JSON or YAML but don't
  support field-level constraints.
  """

  def __init__(self, format_type: types.FormatType = types.FormatType.JSON):
    """Initialize with a format type."""
    self.format_type = format_type
    # Keep _format for backward compatibility with tests
    self._format = "json" if format_type == types.FormatType.JSON else "yaml"

  @classmethod
  def from_examples(
      cls,
      examples_data: Sequence[data.ExampleData],
      attribute_suffix: str = data.ATTRIBUTE_SUFFIX,
  ) -> FormatModeSchema:
    """Factory method to build a schema instance from example data."""
    # Default to JSON format
    return cls(format_type=types.FormatType.JSON)

  def to_provider_config(self) -> dict[str, Any]:
    """Convert schema to provider-specific configuration."""
    return {"format": self._format}

  @property
  def requires_raw_output(self) -> bool:
    """JSON format schemas output raw JSON without fences, YAML does not."""
    return self._format == "json"

  def sync_with_provider_kwargs(self, kwargs: dict[str, Any]) -> None:
    """Sync format type with provider kwargs."""
    if "format_type" in kwargs:
      self.format_type = kwargs["format_type"]
      self._format = (
          "json" if self.format_type == types.FormatType.JSON else "yaml"
      )
    if "format" in kwargs:
      self._format = kwargs["format"]
      self.format_type = (
          types.FormatType.JSON
          if self._format == "json"
          else types.FormatType.YAML
      )
````

FormatModeSchema 是只控制输出格式而不限制字段的简单子类。

__init__ 接收 FormatType，保存 format_type 与兼容旧接口的 _format 字符串；JSON 为 "json"，其他为 "yaml"。

from_examples 不分析示范的类别，只返回 JSON 格式实例。to_provider_config 返回 {"format": self._format}。requires_raw_output 只有 _format 为 json 才为真。

sync_with_provider_kwargs 先处理 format_type，再处理 format。第二个键如果存在会覆盖前一个，从而让字符串模式与枚举保持同步。不要误认为使用此类就已经验证了取件码的类型。

## 13.7 Gemini 怎样从示范推断一张表？

想象有两个登记样例：

| 类别 | 属性名 | 示例值 |
|---|---|---|
| pickup_code | station | "南门" |
| pickup_code | station | "北门" |
| pickup_code | tags | ["冷藏"] |

我们可以先收集成 {"pickup_code": {"station": {str}, "tags": {list}}}。集合表示一个属性有时可能出现不同的类型。

```python
examples = [("pickup_code", {"station": "南门"}),
            ("pickup_code", {"tags": ["冷藏"]})]
categories = {}
for kind, attrs in examples:
    for key, value in attrs.items():
        categories.setdefault(kind, {}).setdefault(key, set()).add(type(value))
assert categories["pickup_code"]["tags"] == {list}
```

源码出处：[langextract/providers/schemas/gemini.py:33–204](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L33-L204)。

````python
@dataclasses.dataclass
class GeminiSchema(schema.BaseSchema):
  """Schema implementation for Gemini structured output.

  Converts ExampleData objects into Gemini's OpenAPI-style
  `response_schema`. User-authored schemas stay on Gemini's native JSON Schema
  `response_json_schema` field.
  """

  _schema_dict: dict[str, Any]
  _use_json_schema: bool = False
  from_output_schema: bool = dataclasses.field(
      default=False, repr=False, compare=False
  )

  def __post_init__(self) -> None:
    # Direct construction should get the same caller-mutation isolation as
    # from_schema_dict().
    self._schema_dict = copy.deepcopy(self._schema_dict)

  @property
  def schema_dict(self) -> dict[str, Any]:
    """Returns the schema dictionary."""
    return self._schema_dict

  @schema_dict.setter
  def schema_dict(self, schema_dict: dict[str, Any]) -> None:
    """Sets the schema dictionary."""
    self._schema_dict = copy.deepcopy(schema_dict)

  def to_provider_config(self) -> dict[str, Any]:
    """Convert schema to Gemini-specific configuration.

    Returns:
      Dictionary with Gemini response schema config for the provider API.
    """
    schema_key = (
        "response_json_schema" if self._use_json_schema else "response_schema"
    )
    return {
        schema_key: self._schema_dict,
        "response_mime_type": "application/json",
    }

  def output_schema_reserved_provider_kwargs(self) -> frozenset[str]:
    """Provider kwargs that would override an explicit output_schema."""
    return frozenset({
        "gemini_schema",
        "response_json_schema",
        "response_mime_type",
        "response_schema",
    })

  @property
  def requires_raw_output(self) -> bool:
    """Gemini outputs raw JSON via response_mime_type."""
    return True

  def validate_format(self, format_handler: fh.FormatHandler) -> None:
    """Validate Gemini's format requirements.

    Gemini requires:
    - No fence markers (outputs raw JSON via response_mime_type)
    - Wrapper with EXTRACTIONS_KEY (built into LangExtract schemas)
    """
    if format_handler.use_fences:
      warnings.warn(
          "Gemini outputs native JSON via"
          " response_mime_type='application/json'. Using fence_output=True may"
          " cause parsing issues. Set fence_output=False.",
          UserWarning,
          stacklevel=3,
      )

    if (
        not format_handler.use_wrapper
        or format_handler.wrapper_key != data.EXTRACTIONS_KEY
    ):
      warnings.warn(
          "Gemini's response_schema expects"
          f" wrapper_key='{data.EXTRACTIONS_KEY}'. Current settings:"
          f" use_wrapper={format_handler.use_wrapper},"
          f" wrapper_key='{format_handler.wrapper_key}'",
          UserWarning,
          stacklevel=3,
      )

  @classmethod
  def from_examples(
      cls,
      examples_data: Sequence[data.ExampleData],
      attribute_suffix: str = data.ATTRIBUTE_SUFFIX,
  ) -> GeminiSchema:
    """Creates a GeminiSchema from example extractions.

    Builds a JSON-based schema with a top-level "extractions" array. Each
    element in that array is an object containing the extraction class name
    and an accompanying "<class>_attributes" object for its attributes.

    Args:
      examples_data: A sequence of ExampleData objects containing extraction
        classes and attributes.
      attribute_suffix: String appended to each class name to form the
        attributes field name (defaults to "_attributes").

    Returns:
      A GeminiSchema whose internal dictionary represents the JSON constraint.
    """
    extraction_categories: dict[str, dict[str, set[type]]] = {}
    for example in examples_data:
      for extraction in example.extractions:
        category = extraction.extraction_class
        if category not in extraction_categories:
          extraction_categories[category] = {}

        if extraction.attributes:
          for attr_name, attr_value in extraction.attributes.items():
            if attr_name not in extraction_categories[category]:
              extraction_categories[category][attr_name] = set()
            extraction_categories[category][attr_name].add(type(attr_value))

    extraction_properties: dict[str, dict[str, Any]] = {}

    for category, attrs in extraction_categories.items():
      extraction_properties[category] = {"type": "string"}

      attributes_field = f"{category}{attribute_suffix}"
      attr_properties = {}

      if not attrs:
        attr_properties["_unused"] = {"type": "string"}
      else:
        for attr_name, attr_types in attrs.items():
          if list in attr_types:
            attr_properties[attr_name] = {
                "type": "array",
                "items": {"type": "string"},  # type: ignore[dict-item]
            }
          else:
            attr_properties[attr_name] = {"type": "string"}

      extraction_properties[attributes_field] = {
          "type": "object",
          "properties": attr_properties,
          "nullable": True,
      }

    extraction_schema = {
        "type": "object",
        "properties": extraction_properties,
    }

    schema_dict = {
        "type": "object",
        "properties": {
            data.EXTRACTIONS_KEY: {"type": "array", "items": extraction_schema}
        },
        "required": [data.EXTRACTIONS_KEY],
    }

    return cls(_schema_dict=schema_dict)

  @classmethod
  def from_schema_dict(
      cls, output_schema: core_types.JsonSchema
  ) -> GeminiSchema:
    """Creates a GeminiSchema from a user-provided output schema."""
    return cls(
        _schema_dict=output_schema_lib.validate_output_schema(output_schema),
        _use_json_schema=True,
        from_output_schema=True,
    )
````

GeminiSchema 是 dataclass，保存 _schema_dict、是否采用原生 JSON Schema 的 _use_json_schema，以及来源标记 from_output_schema。

__post_init__ 对传入字典深复制，使直接构造与工厂构造具有类似隔离行为。schema_dict getter 返回内部字典本身，setter 则深复制新值；注意 getter 并非只读副本。

to_provider_config 根据 _use_json_schema 在 response_schema 与 response_json_schema 之间选择，配上 response_mime_type="application/json"。output_schema_reserved_provider_kwargs 返回四个可能覆盖已指定规格的键（gemini_schema、response_json_schema、response_mime_type、response_schema）。

requires_raw_output 总是真；validate_format 在启用围栏或不符合默认包装时分别警告，不自行重写 FormatHandler。

from_examples 遍历所有示范和卡片，构造类别→属性→类型集合。每个类别的文本字段为 string；属性中只要观察到 list 就产生字符串数组类型，否则为 string。无属性类别仍生成含 _unused 字符串的对象；属性对象带 nullable=True。所有类别字段放在共同的项对象 properties 中，最外层要求 extractions。它是从示范推导的 OpenAPI 风格规格，而非用户手写 JSON Schema 的逐字翻译。

from_schema_dict 则先验证外壳，再创建 _use_json_schema=True、from_output_schema=True 的实例，保留用户编写的 JSON Schema 结构。两种来源在发送服务时选择不同参数。

## 13.8 OpenAI 严格模式下，选填字段如何表达？

一份固定表要求所有栏都存在，但「取件站」可能不知道。我们可以要求这一栏出现，允许填 null。如下：

```json
{"anyOf": [{"type": "string"}, {"type": "null"}]}
```

这与省略键（missing）不同；null 是 JSON 的一种值。

源码出处：[langextract/providers/schemas/openai.py:42–48](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L42-L48)。

````python
def _nullable(schema_dict: dict[str, Any]) -> dict[str, Any]:
  """Allows optional LangExtract fields under OpenAI strict mode.

  Strict mode requires every object key to be listed as required. A null
  branch preserves LangExtract's optional-attribute semantics.
  """
  return {"anyOf": [schema_dict, {"type": "null"}]}
````

_nullable 把现有 schema 与 {"type": "null"} 包进 anyOf，表达一个字段可以有有效内容也可以为空。它不会从 required 删除字段。

源码出处：[langextract/providers/schemas/openai.py:51–82](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L51-L82)。

````python
def _attribute_value_schema(attr_types: set[type]) -> dict[str, Any]:
  """Maps example-observed Python values into a strict JSON-Schema union.

  OpenAI strict mode requires declared object keys to be present, so null
  is always included. Unknown value types fall back to strings because the
  examples are a guide for model output, not a full Python type system.

  Args:
    attr_types: Python types observed for this attribute across all
      examples. May be empty if the attribute was declared but never
      assigned a concrete value.

  Returns:
    A JSON-Schema anyOf fragment.
  """
  options: list[dict[str, Any]] = []
  if list in attr_types:
    options.append({"type": "array", "items": {"type": "string"}})
  if bool in attr_types:
    options.append({"type": "boolean"})
  if int in attr_types:
    options.append({"type": "integer"})
  if float in attr_types:
    options.append({"type": "number"})
  recognized_scalars = {bool, int, float, str, list}
  needs_string_fallback = (
      not attr_types or str in attr_types or attr_types - recognized_scalars
  )
  if needs_string_fallback:
    options.append({"type": "string"})
  options.append({"type": "null"})
  return {"anyOf": options}
````

_attribute_value_schema 把观察到的 Python 类型集合映射成 JSON Schema 联合：list 为字符串数组、bool 为 boolean、int 为 integer、float 为 number。观察到 str、未知类型或空集合则加入 string 选项；最后总是加入 null。使用 type(value) 让 bool 与 int 可单独区分。它是轻量类型推断，不是完整运行时类型系统。

源码出处：[langextract/providers/schemas/openai.py:85–109](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L85-L109)。

````python
def _collect_extraction_categories(
    examples_data: Sequence[data.ExampleData],
) -> dict[str, dict[str, set[type]]]:
  """Keeps schema variants aligned with the examples' extraction classes.

  Args:
    examples_data: Example extractions to inspect.

  Returns:
    A nested mapping from category to attribute name to observed Python
    value types.
  """
  extraction_categories: dict[str, dict[str, set[type]]] = {}
  for example in examples_data:
    for extraction in example.extractions:
      category = extraction.extraction_class
      if category not in extraction_categories:
        extraction_categories[category] = {}

      if extraction.attributes:
        for attr_name, attr_value in extraction.attributes.items():
          if attr_name not in extraction_categories[category]:
            extraction_categories[category][attr_name] = set()
          extraction_categories[category][attr_name].add(type(attr_value))
  return extraction_categories
````

_collect_extraction_categories 汇集同类别、同属性的所有实际 Python 类型，与上面的教学实验相似。没有属性仍保留类别记录，因为类别本身需要规格。

源码出处：[langextract/providers/schemas/openai.py:112–154](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L112-L154)。

````python
def _build_extraction_variant(
    category: str,
    attrs: dict[str, set[type]],
    attribute_suffix: str,
) -> dict[str, Any]:
  """Creates the shape OpenAI must enforce for one extraction class.

  Each variant has two top-level keys: category for extracted text and
  category plus suffix for attributes. Both keys are required so OpenAI
  strict mode accepts the shape; the attributes object is wrapped in a
  null union so the model may omit it.

  Args:
    category: The extraction class name, used as the literal property key.
    attrs: Mapping from attribute name to observed Python value types.
    attribute_suffix: Suffix appended to category to form the attributes
      object property key.

  Returns:
    A JSON-Schema object fragment.
  """
  properties: dict[str, Any] = {category: {"type": "string"}}

  # Null unions preserve optional attributes while satisfying OpenAI strict
  # mode's required-key rule.
  attr_properties = {
      attr_name: _attribute_value_schema(attr_types)
      for attr_name, attr_types in attrs.items()
  }
  attributes_field = f"{category}{attribute_suffix}"
  properties[attributes_field] = _nullable({
      "type": "object",
      "properties": attr_properties,
      "required": list(attr_properties),
      "additionalProperties": False,
  })

  return {
      "type": "object",
      "properties": properties,
      "required": list(properties),
      "additionalProperties": False,
  }
````

_build_extraction_variant 为一个类别构造文本字段和属性字段两个键。属性对象中每个字段使用 _attribute_value_schema，所有属性都列入 required，并禁止额外属性；整个属性对象由 _nullable 包装。外层两个键也都必填且禁止额外属性。这样严格模式仍允许没有属性值（填 null），并不会迫使助手捏造属性。

## 13.9 创建 OpenAI 的适配器

源码出处：[langextract/providers/schemas/openai.py:157–329](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L157-L329)。

````python
@dataclasses.dataclass(frozen=True)
class OpenAISchema(schema.BaseSchema):
  """Schema implementation for OpenAI structured outputs.

  Converts ExampleData objects into a JSON-Schema that OpenAI's Chat
  Completions API accepts via response_format. Instances are frozen because
  parallel inference shares one schema across worker threads.
  """

  schema_dict: dict[str, Any]
  schema_name: str = DEFAULT_SCHEMA_NAME
  strict: bool = True
  from_output_schema: bool = dataclasses.field(
      default=False, repr=False, compare=False
  )

  def __post_init__(self) -> None:
    # Copy before publishing the schema to worker threads so caller mutations
    # cannot change in-flight requests.
    object.__setattr__(self, "schema_dict", copy.deepcopy(self.schema_dict))

  @property
  def response_format(self) -> dict[str, Any]:
    """Per-call Chat Completions response_format payload."""
    return {
        "type": "json_schema",
        "json_schema": {
            "name": self.schema_name,
            "schema": copy.deepcopy(self.schema_dict),
            "strict": self.strict,
        },
    }

  def to_provider_config(self) -> dict[str, Any]:
    """OpenAI schemas are applied through the provider schema hook."""
    return {}

  def output_schema_reserved_provider_kwargs(self) -> frozenset[str]:
    """Provider kwargs that would override an explicit output_schema."""
    return frozenset({"openai_schema", "response_format"})

  @property
  def requires_raw_output(self) -> bool:
    """OpenAI structured outputs emit raw JSON without fences."""
    return True

  def validate_format(self, format_handler: fh.FormatHandler) -> None:
    """Validates OpenAI structured output format compatibility.

    Args:
      format_handler: Format handler describing the caller's desired
        output shape (format type, fence usage, wrapper key).

    Raises:
      InferenceConfigError: if format_type is not JSON.
    """
    if not output_schema_lib.is_json_format_type(format_handler.format_type):
      raise exceptions.InferenceConfigError(JSON_SCHEMA_FORMAT_ERROR)

    if format_handler.use_fences:
      warnings.warn(
          "OpenAI structured outputs emit native JSON via response_format. "
          "Using fence_output=True may cause parsing issues. Set "
          "fence_output=False.",
          UserWarning,
          stacklevel=3,
      )

    if (
        not format_handler.use_wrapper
        or format_handler.wrapper_key != data.EXTRACTIONS_KEY
    ):
      warnings.warn(
          "OpenAI's response_format schema expects"
          f" wrapper_key='{data.EXTRACTIONS_KEY}'. Current settings:"
          f" use_wrapper={format_handler.use_wrapper},"
          f" wrapper_key='{format_handler.wrapper_key}'",
          UserWarning,
          stacklevel=3,
      )

  @classmethod
  def from_examples(
      cls,
      examples_data: Sequence[data.ExampleData],
      attribute_suffix: str = data.ATTRIBUTE_SUFFIX,
      strict: bool = True,
  ) -> OpenAISchema:
    """Creates an OpenAISchema from example extractions.

    Builds a JSON schema with a top-level extractions array whose items are
    an anyOf union of one strict-mode object variant per extraction class.

    Args:
      examples_data: A sequence of ExampleData objects containing
        extraction classes and attributes.
      attribute_suffix: String appended to each class name to form the
        attributes-object key. Defaults to "_attributes".
      strict: Whether to emit the schema under OpenAI strict mode. Defaults
        to True. Set to False for schemas OpenAI rejects in strict mode. The
        generated schema shape remains constrained in either mode.

    Returns:
      An OpenAISchema instance ready to pass to a provider.
    """
    extraction_categories = _collect_extraction_categories(examples_data)
    variants = [
        _build_extraction_variant(category, attrs, attribute_suffix)
        for category, attrs in extraction_categories.items()
    ]

    if variants:
      extraction_item_schema = {"anyOf": variants}
    else:
      extraction_item_schema = {
          "type": "object",
          "properties": {},
          "required": [],
          "additionalProperties": False,
      }

    schema_dict = {
        "type": "object",
        "properties": {
            data.EXTRACTIONS_KEY: {
                "type": "array",
                "items": extraction_item_schema,
            }
        },
        "required": [data.EXTRACTIONS_KEY],
        "additionalProperties": False,
    }

    return cls(schema_dict=schema_dict, strict=strict)

  @classmethod
  def from_schema_dict(
      cls,
      output_schema: core_types.JsonSchema,
      *,
      schema_name: str = DEFAULT_SCHEMA_NAME,
      strict: bool = True,
  ) -> OpenAISchema:
    """Creates an OpenAISchema from a user-provided output schema.

    Strict mode requirements (every object lists all properties in
    `required` and sets `additionalProperties: false`) are enforced by the
    OpenAI API itself; the `lx.schema` helpers emit compliant schemas by
    default.

    Args:
      output_schema: JSON schema for LangExtract's raw output object.
      schema_name: Name sent to OpenAI's response_format payload.
      strict: Whether OpenAI should enforce strict structured outputs.

    Returns:
      An OpenAISchema instance ready to apply to the OpenAI provider.
    """
    if not isinstance(schema_name, str) or not schema_name:
      raise exceptions.InferenceConfigError(
          "schema_name must be a non-empty string."
      )
    if not _SCHEMA_NAME_RE.fullmatch(schema_name):
      raise exceptions.InferenceConfigError(
          "schema_name must contain only letters, numbers, underscores, "
          "or dashes, and be at most 64 characters."
      )
    return cls(
        schema_dict=output_schema_lib.validate_output_schema(output_schema),
        schema_name=schema_name,
        strict=strict,
        from_output_schema=True,
    )
````

OpenAISchema 用 frozen dataclass 保存 schema_dict、schema_name、strict 与来源标记。并行调用可能共享一个实例，冻结字段引用有助于避免意外重新赋值。

__post_init__ 用 object.__setattr__ 绕过 dataclass 冻结限制，将深复制结果安装进实例。这保护构造时传入的原始字典，但 frozen 不等于内部 dict 深度不可变。

response_format 属性每次返回独立 payload，结构含 type=json_schema 以及 name、schema 副本和 strict。复制是为了避免每个请求修改共享的规格。

to_provider_config 返回空字典，原因是 OpenAI provider 通过自身 schema hook 使用 response_format，不是通过普通映射注入；因此 output_schema_reserved_provider_kwargs 显式返回 openai_schema 与 response_format，不能依靠空字典推断冲突键。

requires_raw_output 返回 True。validate_format 先用 is_json_format_type 拒绝非 JSON 配置，再分别针对围栏与非默认包装发警告。

from_examples 调用 _collect_extraction_categories，再为每类构造一个独立对象 variant。数组 items 用 anyOf 联合，若没有类别则使用只允许空对象的规格。顶层 required 包含 extractions，禁止额外属性。strict 参数传给实例，生成结构本身不因 strict=False 而改变。

from_schema_dict 接受用户声明的规格以及可选 schema_name、strict。名称必须非空字符串，匹配 1—64 个字母、数字、下划线和连字符。之后 validate_output_schema 取得独立副本，并标记 from_output_schema=True。服务端仍负责完整严格模式规则的判定。

## 13.10 比较三种规格适配器

| 特性 | FormatModeSchema | GeminiSchema | OpenAISchema |
|---|---|---|---|
| 约束字段 | 无 | 有 | 有 |
| 由示范推断 | 默认 JSON | 共同项对象属性 | 每类独立 anyOf 分支 |
| 用户显式 schema | 基类默认不支持 | response_json_schema | response_format |
| 需要裸 JSON | JSON 模式时 | 是 | 是 |

两种服务推导出来的结构并不完全相同；显式规格在支持的服务间更容易保持同一外壳，但具体限制仍取决于提供者。

## 13.11 练习

1. 修改 item_schema，使取件码只能为字符串，同时描述一个属性。
2. 手工写两类短信的 anyOf，区分外层 extractions 与内部登记卡字段。
3. 在正式项目调用 validate_output_schema，观察 required 缺失、内部保留字段与合法规格的区别。
4. 给两种服务的 from_examples 传入相同示范，打印各自生成的规格并比较字段类型。

下一章我们将处理一个在公开 API 里始终存在的问题：调用者只写一个模型名称，程序如何知道应该创建哪一家的模型，以及如何为未来新服务留出扩展位置？
