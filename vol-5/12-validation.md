# 第 12 章　示范本身也可能出错

我们在第 4 章为助手提供例子：「取件码 3821」，答卷却写成了 8321。助手可能学会错误的填表习惯。与其等正式处理一万条短信后发现问题，不如先检查示范中的每张登记卡是否能对应自己的原文。

## 12.1 一个非常简单的检查器

```python
examples = [
    ("Pickup 3821.", "3821"),
    ("Pickup 9246.", "9264"),
]

issues = []
for i, (text, answer) in enumerate(examples):
    if answer not in text:
        issues.append((i, answer))
assert issues == [(1, "9264")]
```

这只能识别字面不相等。前面两章已经有了对齐器，为什么不让它再做一次相同的工作？它可以区分完全匹配、近似匹配与没有证据。重要的是，正式抽取阶段和示范校验阶段应该采取同一套判定规则。

## 12.2 不要把发现问题与如何处理问题放在一起

检查员发现答卷有问题，未必有权阻止整个工作。实验阶段我们可能只想看到警告，正式任务则希望遇到问题立刻停止。

源码出处：[langextract/prompt_validation.py:56–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L56-L60)。

````python
class IssueKind(enum.Enum):
  """Categorization of alignment issues."""

  FAILED = "failed"  # alignment_status is None
  NON_EXACT = "non_exact"  # MATCH_FUZZY or MATCH_LESSER
````

IssueKind 有 FAILED 和 NON_EXACT 两种问题；精确匹配并不产生问题记录。枚举使用字符串值，便于报告和测试。

源码出处：[langextract/prompt_validation.py:48–53](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L48-L53)。

````python
class PromptValidationLevel(enum.Enum):
  """Validation levels for prompt alignment checks."""

  OFF = "off"
  WARNING = "warning"
  ERROR = "error"
````

PromptValidationLevel 定义 OFF、WARNING、ERROR。它描述处理策略，不描述某张卡的实际问题。不能把 ERROR 与 FAILED 当成同一个概念。

源码出处：[langextract/prompt_validation.py:63–87](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L63-L87)。

````python
@dataclasses.dataclass(frozen=True)
class ValidationIssue:
  """Represents a single validation issue found during alignment."""

  example_index: int
  example_id: str | None
  extraction_class: str
  extraction_text_preview: str
  alignment_status: data.AlignmentStatus | None
  issue_kind: IssueKind
  char_interval: tuple[int, int] | None = None
  token_interval: tuple[int, int] | None = None

  def short_msg(self) -> str:
    """Returns a concise message describing the issue."""
    ex_id = f" id={self.example_id}" if self.example_id else ""
    span = ""
    if self.char_interval:
      span = f" char_span={self.char_interval}"
    return (
        f"[example#{self.example_index}{ex_id}] "
        f"class='{self.extraction_class}' "
        f"status={self.alignment_status} "
        f"text='{self.extraction_text_preview}'{span}"
    )
````

ValidationIssue 是 frozen dataclass，记录示范序号、可选 ID、卡的类别、截短后的内容、匹配状态、问题类型与可选字符/token 区间。冻结实例避免报告创建后被意外重新赋值。

short_msg 把示范索引、可选 ID、类名、状态、文本预览及可选字符区间拼成短消息。它不重新运行对齐，也不负责决定异常级别。

源码出处：[langextract/prompt_validation.py:124–127](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L124-L127)。

````python
def _preview(s: str, n: int = 120) -> str:
  """Creates a preview of text for logging, collapsing whitespace."""
  s = " ".join(s.split())  # Collapse whitespace for logs
  return s if len(s) <= n else s[: n - 1] + "…"
````

_preview 首先 split 后用单个空格拼接，把多行短信折成便于日志阅读的一行。超过 n（默认 120）时保留 n-1 个字符，加省略号。此预览不用于坐标对齐，也不能充当原件。

源码出处：[langextract/prompt_validation.py:90–104](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L90-L104)。

````python
@dataclasses.dataclass
class ValidationReport:
  """Collection of validation issues from prompt alignment checks."""

  issues: list[ValidationIssue]

  @property
  def has_failed(self) -> bool:
    """Returns True if any extraction failed to align."""
    return any(i.issue_kind is IssueKind.FAILED for i in self.issues)

  @property
  def has_non_exact(self) -> bool:
    """Returns True if any extraction has non-exact alignment."""
    return any(i.issue_kind is IssueKind.NON_EXACT for i in self.issues)
````

ValidationReport 只是问题列表容器。has_failed 用 any 检查是否有 FAILED，has_non_exact 同理检查 NON_EXACT。没有问题时两者都为 False；两类问题可能同时存在。

## 12.3 同一套对齐参数应该如何传递？

源码出处：[langextract/prompt_validation.py:111–121](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L111-L121)。

````python
@dataclasses.dataclass(frozen=True)
class AlignmentPolicy:
  """Configuration for alignment validation behavior."""

  enable_fuzzy_alignment: bool = True
  fuzzy_alignment_threshold: float = _FUZZY_ALIGNMENT_MIN_THRESHOLD
  accept_match_lesser: bool = True
  _: dataclasses.KW_ONLY
  fuzzy_alignment_algorithm: str = _DEFAULT_FUZZY_ALGORITHM
  fuzzy_alignment_min_density: float = _FUZZY_ALIGNMENT_MIN_DENSITY
  exact_alignment_algorithm: str = _DEFAULT_EXACT_ALGORITHM
````

AlignmentPolicy 保存对齐相关的规则：是否启用模糊、覆盖率阈值、是否接受局部匹配、模糊算法与密度、精确算法。frozen 限制字段重新赋值。KW_ONLY 之后的三个参数必须显式写关键字，新增选项不会破坏前三个位置参数的意义。默认值与实际对齐器相同（覆盖率 0.75，密度 1/3，lcs 和 dp）。

原来我们可能写六七个形参，把它们依次传递给若干层函数；用一个配置对象则能把这些属于同一职责的值集中起来。

## 12.4 校验时绝不能改动老师写的示范

第 9、10 章的对齐器会修改 Extraction，填写坐标和状态。若直接把示范对象传入，它就会不知不觉改变调用者的数据。

```python
import copy

example = {"text": "3821", "attributes": {"tags": ["pickup"]}}
trial = copy.deepcopy(example)
trial["attributes"]["tags"].append("checked")
assert example["attributes"]["tags"] == ["pickup"]
```

浅复制只复制外层字典，内部列表仍共享。deepcopy 则递归复制嵌套内容，这是验证器常用的防御性复制。

源码出处：[langextract/prompt_validation.py:130–220](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L130-L220)。

````python
def validate_prompt_alignment(
    examples: Sequence[data.ExampleData],
    aligner: resolver.WordAligner | None = None,
    policy: AlignmentPolicy | None = None,
    tokenizer: tokenizer_lib.Tokenizer | None = None,
) -> ValidationReport:
  """Align extractions to their own example text and collect issues.

  Args:
    examples: The few-shot examples to validate.
    aligner: WordAligner instance to use (creates new if None).
    policy: Alignment configuration (uses defaults if None).
    tokenizer: Optional tokenizer to use for alignment. If None, defaults to
      RegexTokenizer.

  Returns:
    ValidationReport containing any alignment issues found.
  """
  if not examples:
    return ValidationReport(issues=[])

  aligner = aligner or resolver.WordAligner()
  policy = policy or AlignmentPolicy()

  issues: list[ValidationIssue] = []

  for idx, ex in enumerate(examples):
    # Defensive copy so validation never mutates user examples.
    copied_extractions = [[copy.deepcopy(e) for e in ex.extractions]]
    aligned_groups = aligner.align_extractions(
        extraction_groups=copied_extractions,
        source_text=ex.text,
        token_offset=0,
        char_offset=0,
        enable_fuzzy_alignment=policy.enable_fuzzy_alignment,
        fuzzy_alignment_threshold=policy.fuzzy_alignment_threshold,
        fuzzy_alignment_algorithm=policy.fuzzy_alignment_algorithm,
        fuzzy_alignment_min_density=policy.fuzzy_alignment_min_density,
        exact_alignment_algorithm=policy.exact_alignment_algorithm,
        accept_match_lesser=policy.accept_match_lesser,
        tokenizer_impl=tokenizer,
    )

    for aligned in aligned_groups[0]:
      status = getattr(aligned, "alignment_status", None)
      char_interval = getattr(aligned, "char_interval", None)
      token_interval = getattr(aligned, "token_interval", None)
      klass = getattr(aligned, "extraction_class", "<unknown>")
      text = getattr(aligned, "extraction_text", "")

      if status is None:
        issues.append(
            ValidationIssue(
                example_index=idx,
                example_id=getattr(ex, "example_id", None),
                extraction_class=klass,
                extraction_text_preview=_preview(text),
                alignment_status=None,
                issue_kind=IssueKind.FAILED,
                char_interval=None,
                token_interval=None,
            )
        )
      elif status in (
          data.AlignmentStatus.MATCH_FUZZY,
          data.AlignmentStatus.MATCH_LESSER,
      ):
        char_interval_tuple = None
        token_interval_tuple = None
        if char_interval:
          char_interval_tuple = (char_interval.start_pos, char_interval.end_pos)
        if token_interval:
          token_interval_tuple = (
              token_interval.start_index,
              token_interval.end_index,
          )

        issues.append(
            ValidationIssue(
                example_index=idx,
                example_id=getattr(ex, "example_id", None),
                extraction_class=klass,
                extraction_text_preview=_preview(text),
                alignment_status=status,
                issue_kind=IssueKind.NON_EXACT,
                char_interval=char_interval_tuple,
                token_interval=token_interval_tuple,
            )
        )

  return ValidationReport(issues=issues)
````

validate_prompt_alignment 空示范直接返回空报告，否则选用传入的 aligner 与 policy，或建立默认对象。对每个 ExampleData，复制其 extractions，包成一个组；调用 align_extractions 对齐本示范的原文，两个偏移都是零，因为这里没有文档切块。

随后逐个检查对齐结果。用 getattr 读取状态、坐标、类别、抽取文本，提供缺省值以兼容对象字段差异。状态为 None，记录 FAILED 并不给坐标。MATCH_FUZZY 或 MATCH_LESSER 则产生 NON_EXACT，将区间对象转换为简单元组。MATCH_EXACT 不记录。最终返回 ValidationReport。

这个函数校验的是可定位性，而不是语义。把短信里的「3821」误标为快递公司名称，仍可能通过位置检查。

## 12.5 检查结果由谁决定是否报错？

源码出处：[langextract/prompt_validation.py:107–108](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L107-L108)。

````python
class PromptAlignmentError(RuntimeError):
  """Raised when prompt alignment validation fails under ERROR mode."""
````

PromptAlignmentError 继承 RuntimeError，不增加字段。它表示示范对齐校验在严格处理模式下失败，与第 6 章的模型答卷解析异常不同。

源码出处：[langextract/prompt_validation.py:223–269](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L223-L269)。

````python
def handle_alignment_report(
    report: ValidationReport,
    level: PromptValidationLevel,
    *,
    strict_non_exact: bool = False,
) -> None:
  """Log or raise based on validation level.

  Args:
    report: The validation report to handle.
    level: The validation level determining behavior.
    strict_non_exact: If True, treat non-exact matches as errors in ERROR mode.

  Raises:
    PromptAlignmentError: If validation fails in ERROR mode.
  """
  if level is PromptValidationLevel.OFF:
    return

  for issue in report.issues:
    if issue.issue_kind is IssueKind.NON_EXACT:
      logging.warning(
          "Prompt alignment: non-exact match: %s", issue.short_msg()
      )
    else:
      logging.warning(
          "Prompt alignment: FAILED to align: %s", issue.short_msg()
      )

  if level is PromptValidationLevel.ERROR:
    failed = [i for i in report.issues if i.issue_kind is IssueKind.FAILED]
    non_exact = [
        i for i in report.issues if i.issue_kind is IssueKind.NON_EXACT
    ]

    if failed:
      sample = failed[0].short_msg()
      raise PromptAlignmentError(
          f"Prompt alignment validation failed: {len(failed)} extraction(s) "
          f"could not be aligned (e.g., {sample})"
      )
    if strict_non_exact and non_exact:
      sample = non_exact[0].short_msg()
      raise PromptAlignmentError(
          "Prompt alignment validation failed under strict mode: "
          f"{len(non_exact)} non-exact match(es) found (e.g., {sample})"
      )
````

handle_alignment_report 接受已经生成的报告。如果 OFF 则立即返回；其余级别首先逐项记录警告。ERROR 模式将问题分为 failed 与 non_exact 两份列表，如果 failed 非空立即抛 PromptAlignmentError，消息含问题总数与第一条样例；仅在 strict_non_exact=True 时也把非精确对齐当成错误。

| 模式 | 无法定位 | 非精确定位 |
|---|---|---|
| OFF | 不处理报告 | 不处理报告 |
| WARNING | 警告后继续 | 警告后继续 |
| ERROR | 警告并抛异常 | 警告，严格模式才抛异常 |

注意 OFF 在这个函数内意味着不处理现有报告；第 11 章 extract 更早检查级别，OFF 时直接不执行校验。两个层次的职责不同。

## 12.6 把验证放到真正调用模型之前

回到 extract 的开始部分。它遍历 dataclasses.fields(AlignmentPolicy)，如果 resolver_params 中对应字段非 None，就传给 policy。这样调用者无需单独维护两套对齐参数。示范问题可以在耗费推理资源之前被发现。

一个教学流程是先用 ERROR 且 strict_non_exact=True 检查自己撰写的精确示范，再根据业务需要选择正式抽取时的模糊策略。示范越清楚，助手越容易知道怎样填写答卷，但它依然不能保证结果正确。

## 12.7 练习

1. 在正式项目中构造两条 ExampleData：一条精确，另一条含不存在的取件码，打印 report.issues。
2. 在 WARNING 和 ERROR 之间切换，观察不同处理策略。
3. 验证调用 validate_prompt_alignment 后原示范 Extraction 的对齐状态没有改变。

到目前为止我们依靠示范和文字说明来要求模型填写答卷。如果接下来能直接规定表格中每个字段的类型、允许值，我们还能减少一些格式层面的错误。
