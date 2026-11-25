# 第 5 章　换一个助手也能工作

前面已经能写出任务单了。现在要把任务单交给助手。假如公司一开始用 Google 的模型，后来部署了自己的模型服务，你不希望为了更换一个服务，把所有登记卡、文本定位和提示词代码重写一遍。

## 5.1 先让程序离线运转

```python
class FakeAssistant:
    def infer(self, prompts):
        for prompt in prompts:
            yield '{"extractions": [{"pickup_code": "3821"}]}'

for answer in FakeAssistant().infer(["短信一", "短信二"]):
    print(answer)
```

这里的假助手不理解文字，只用来验证程序调用过程。infer 意为推理，即让模型根据输入生成输出。yield 表示结果可以逐个交付，不必等整个列表都准备好。

如果真实服务可能交出多个备选答案，该如何承载？每一个任务单对应一组答案，每个答案可以带评分。例如两个任务，每个任务有两个候选：

```python
[
    [("3821", 0.9), ("3827", 0.1)],
    [("7619", None)],
]
```

评分可以为空；它也不是校准过的正确概率，不应当用它替代原文核验。

源码出处：[langextract/core/types.py:69–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/types.py#L69-L81)。

````python
@dataclasses.dataclass(frozen=True)
class ScoredOutput:
  """Scored output from language model inference."""

  score: float | None = None
  output: str | None = None

  def __str__(self) -> str:
    score_str = '-' if self.score is None else f'{self.score:.2f}'
    if self.output is None:
      return f'Score: {score_str}\nOutput: None'
    formatted_lines = textwrap.indent(self.output, prefix='  ')
    return f'Score: {score_str}\nOutput:\n{formatted_lines}'
````

score 和 output 都是可选字段，分别记录服务提供的分数和回答文本。frozen=True 表示构造之后通常不能通过属性赋值修改数据。这样结果可安全传递给下一步骤，而不必担心随手覆盖。

__str__ 是给人看的打印格式。分数为空就显示短横线，否则保留两位小数。没有输出显示 None；有输出便用 textwrap.indent 给各行加两个空格。它不参与评分计算，也不是模型实际发送给我们的 JSON。

## 5.2 约束回答的概念

如果对助手仅仅说“请写 JSON”，它仍可能返回普通句子。某些模型服务还能接受更强的输出限制，比如声明允许的字段和类型。约束（constraint）就是限制回答形式的规则；它不保证填入的信息真实。

源码出处：[langextract/core/types.py:52–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/types.py#L52-L55)。

````python
class ConstraintType(enum.Enum):
  """Enumeration of constraint types."""

  NONE = 'none'
````

源码出处：[langextract/core/types.py:58–66](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/types.py#L58-L66)。

````python
@dataclasses.dataclass
class Constraint:
  """Represents a constraint for model output decoding.

  Attributes:
    constraint_type: The type of constraint applied.
  """

  constraint_type: ConstraintType = ConstraintType.NONE
````

当前枚举只有 NONE，Constraint 保存一个 constraint_type，默认无约束。它体现了一层独立的表示，不要凭这个类的名字误以为已实现完整的约束解码器。实际服务的结构限制还由后续 schema 对象管理。

Schema 即结构说明书，好比一张空白表格规定哪些栏应该是字符串、哪些栏应该是列表。源码中的 JsonValue 和 JsonSchema 是类型别名，不是运行时验证器；JsonValue 允许基本 JSON 值及其嵌套组合，JsonSchema 是字符串键到这类值的映射。

## 5.3 先约定所有助手都会做的一件事

Python 的抽象基类（ABC）能说明“每家服务必须提供 infer 这个动作”。父类负责公共操作，具体模型服务各自实现网络请求。不要把它误认为所有模型能力都相同，它只是约定了共同的部分。

源码出处：[langextract/core/base_model.py:32–231](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L32-L231)。

````python
class BaseLanguageModel(abc.ABC):
  """An abstract inference class for managing LLM inference.

  Attributes:
    _constraint: A `Constraint` object specifying constraints for model output.
  """

  def __init__(self, constraint: types.Constraint | None = None, **kwargs: Any):
    """Initializes the BaseLanguageModel with an optional constraint.

    Args:
      constraint: Applies constraints when decoding the output. Defaults to no
        constraint.
      **kwargs: Additional keyword arguments passed to the model.
    """
    self._constraint = constraint or types.Constraint()
    self._schema: schema.BaseSchema | None = None
    self._fence_output_override: bool | None = None
    self._extra_kwargs: dict[str, Any] = kwargs.copy()

  @classmethod
  def get_schema_class(cls) -> type[Any] | None:
    """Return the schema class this provider supports."""
    return None

  def apply_schema(self, schema_instance: schema.BaseSchema | None) -> None:
    """Apply a schema instance to this provider.

    Optional method that providers can override to store the schema instance
    for runtime use. The default implementation stores it as _schema.

    Args:
      schema_instance: The schema instance to apply, or None to clear.
    """
    self._schema = schema_instance

  def apply_output_schema(self, output_schema: types.JsonSchema) -> None:
    """Apply a user-provided LangExtract output schema to this model.

    Args:
      output_schema: JSON schema for LangExtract's raw output envelope.

    Raises:
      InferenceConfigError: If this provider cannot consume user schemas, or
        if the model already has conflicting schema configuration.
    """
    schema_class = self.get_schema_class()
    if schema_class is None:
      raise exceptions.unsupported_output_schema_error(type(self).__name__)
    try:
      schema_instance = schema_class.from_schema_dict(output_schema)
    except NotImplementedError as e:
      raise exceptions.unsupported_output_schema_error(
          type(self).__name__
      ) from e
    schema.mark_from_output_schema(schema_instance)

    current_schema = self.schema
    if current_schema is not None:
      requested_schema_dict = getattr(schema_instance, 'schema_dict', None)
      if (
          getattr(current_schema, 'from_output_schema', False)
          and requested_schema_dict is not None
          and getattr(current_schema, 'schema_dict', None)
          == requested_schema_dict
      ):
        return
      raise exceptions.InferenceConfigError(
          f'output_schema cannot be applied to {type(self).__name__} because '
          'the model already has a schema configured. Create the model '
          'without schema settings, or pass output_schema when the model is '
          'created.'
      )

    provider_kwargs = getattr(self, '_extra_kwargs', None) or {}
    conflicts = sorted(
        key
        for key in schema_instance.output_schema_reserved_provider_kwargs()
        if provider_kwargs.get(key) is not None
    )
    if conflicts:
      raise exceptions.output_schema_provider_kwargs_error(conflicts)
    self.apply_schema(schema_instance)

  @property
  def schema(self) -> schema.BaseSchema | None:
    """The current schema instance if one is configured.

    Returns:
      The schema instance or None if no schema is applied.
    """
    return getattr(self, '_schema', None)

  def set_fence_output(self, fence_output: bool | None) -> None:
    """Set explicit fence output preference.

    Args:
      fence_output: True to force fences, False to disable, None for auto.
    """
    if not hasattr(self, '_fence_output_override'):
      self._fence_output_override = None
    self._fence_output_override = fence_output

  @property
  def requires_fence_output(self) -> bool:
    """Whether this model requires fence output for parsing.

    Uses explicit override if set, otherwise computes from schema.
    Returns True if no schema or schema doesn't require raw output.
    """
    if (
        hasattr(self, '_fence_output_override')
        and self._fence_output_override is not None
    ):
      return self._fence_output_override

    schema_obj = self.schema
    if schema_obj is None:
      return True
    return not schema_obj.requires_raw_output

  def merge_kwargs(
      self, runtime_kwargs: Mapping[str, Any] | None = None
  ) -> dict[str, Any]:
    """Merge stored extra kwargs with runtime kwargs.

    Runtime kwargs take precedence over stored kwargs.

    Args:
      runtime_kwargs: Kwargs provided at inference time, or None.

    Returns:
      Merged kwargs dictionary.
    """
    base = getattr(self, '_extra_kwargs', {}) or {}
    incoming = dict(runtime_kwargs or {})
    return {**base, **incoming}

  @abc.abstractmethod
  def infer(
      self, batch_prompts: Sequence[str], **kwargs
  ) -> Iterator[Sequence[types.ScoredOutput]]:
    """Implements language model inference.

    Args:
      batch_prompts: Batch of inputs for inference. Single element list can be
        used for a single input.
      **kwargs: Additional arguments for inference, like temperature and
        max_decode_steps.

    Returns: Batch of Sequence of probable output text outputs, sorted by
      descending score.
    """

  def infer_batch(
      self, prompts: Sequence[str], batch_size: int = 32  # pylint: disable=unused-argument
  ) -> list[list[types.ScoredOutput]]:
    """Batch inference with configurable batch size.

    This is a convenience method that collects all results from infer().

    Args:
      prompts: List of prompts to process.
      batch_size: Batch size (currently unused, for future optimization).

    Returns:
      List of lists of ScoredOutput objects.
    """
    results = []
    for output in self.infer(prompts):
      results.append(list(output))
    return results

  def parse_output(self, output: str) -> Any:
    """Parses model output as JSON or YAML.

    Note: This expects raw JSON/YAML without code fences.
    Code fence extraction is handled by resolver.py.

    Args:
      output: Raw output string from the model.

    Returns:
      Parsed Python object (dict or list).

    Raises:
      ValueError: If output cannot be parsed as JSON or YAML.
    """
    # Check if we have a format_type attribute (providers should set this)
    format_type = getattr(self, 'format_type', types.FormatType.JSON)

    try:
      if format_type == types.FormatType.JSON:
        return json.loads(output)
      else:
        return yaml.safe_load(output)
    except Exception as e:
      raise ValueError(
          f'Failed to parse output as {format_type.name}: {str(e)}'
      ) from e
````

### 初始化与成员

__init__ 保存四项共同配置：_constraint 为约束（没有则创建默认对象），_schema 暂时为空，_fence_output_override 为用户有没有明确要求围栏，_extra_kwargs 是额外配置的浅复制。浅复制保护顶层字典的增删，不保护内部可变对象。

### 共同的输入输出动作

infer 是抽象方法，不负责发请求。参数 batch_prompts 是一组任务单字符串，kwargs 是额外的运行配置。它规定返回迭代器，每次产生某份任务单的候选答卷序列，元素是 ScoredOutput。按照文档约定候选按评分递减。后续流水线选择首个候选，所以保持任务与答卷的顺序十分重要。

infer_batch 是便利入口：循环调用 infer 的结果，每组转换成 list，再把各组收集起来。batch_size 虽然出现在签名中，目前没有实际用途。别误认为改变它即可限制请求批次。与直接迭代相比，这个方法较方便，却必须保存全部结果。

parse_output 把不含围栏的答卷直接转换为 JSON 或 YAML 对象。getattr 如果找不到 format_type 就默认 JSON。解析异常统一转换为 ValueError。模型基类提供这个便利方法，实际抽取流水线还使用前一章的 FormatHandler 处理围栏与包装。

merge_kwargs 将构造时额外参数与调用时参数合并，后者覆盖前者。比如默认 temperature=0.1，某次请求指定 0.3 则本次使用 0.3，不必永久改写模型实例。字典解包使优先级一目了然。

### 共同的答卷格式设置

get_schema_class 是类方法，默认返回 None，表示没有公开支持的结构说明类。支持它的服务可以覆盖此方法。

apply_schema 接收已构造好的 schema 对象（或 None），直接保存到 _schema，子类可以增加服务特有行为。schema 属性则读出它，若字段缺失也返回 None。

set_fence_output 接收 True、False 或 None，保存用户的明确覆盖选项。requires_fence_output 先检查这个选项，有值就直接采用；否则如果没有结构说明，默认需要围栏；有说明则根据它是否要求纯文本格式反向决定是否需要围栏。这样项目的提示词生成和格式解析可使用同一项经过计算的配置。

### 直接使用用户提供的结构说明

apply_output_schema 比简单的 apply_schema 多做一系列检查：

1. 从具体服务取得支持的结构说明类；没有就报不支持。
2. 调用 from_schema_dict 构造对应对象，如果服务没有实现此入口，将 NotImplementedError 转成清楚的配置错误。
3. 用 mark_from_output_schema 标明来源，区别用户提供的 schema 与示范推导的 schema。
4. 若模型已有 schema，检查是不是完全相同且同样来源的用户 schema。若是，直接返回；否则拒绝覆盖。
5. 查找与用户 schema 冲突的服务参数；存在冲突则报错。
6. 最后才 apply_schema。

生活中的类比：员工已按照一种固定报表模板工作，不能在一半时悄悄替换模板，否则前后答卷可能不一致。作者在源码中明确检查冲突；将这些检查解释为避免隐式替换和配置漂移，是根据实现作出的设计推断。

相比每个服务各自写所有检查，公共基类能保持规则一致，但它也必须给予子类合理的扩展方法。结构说明从示范产生和服务商具体约束的实现，将在后续专章讨论。

## 5.4 小练习

把开头假助手的 yield 改成每次返回一组候选：列表中放一个带 output 的对象。尝试在外面依次取每个列表的第 0 项。这就接近实际流水线与模型服务之间交换数据的方式了。
