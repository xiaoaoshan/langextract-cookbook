# 第 9 章　同一句话中出现两个相同取件码

现在我们把小信封交给助手，得到两张登记卡：取件码 3821，取件码 3821。原短信是「上午 3821，下午 3821」。如果每张卡都调用 text.find("3821")，它们会被贴到同一个地方。

## 9.1 找出所有可能的位置

```python
def occurrences(source, wanted):
    if not wanted:
        return []
    return [i for i in range(len(source)-len(wanted)+1)
            if source[i:i+len(wanted)] == wanted]

source = ["上午", "3821", "下午", "3821"]
assert occurrences(source, ["3821"]) == [1, 3]
```

这里使用的是我们手工构造的 token 列表。真正切分依赖第 7 章的 tokenizer。比较时通常忽略英文大小写，但标注证据时仍然保留原文字。

源码出处：[langextract/resolver.py:1285–1311](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1285-L1311)。

````python
def _tokenize_with_lowercase(
    text: str,
    tokenizer_inst: tokenizer_lib.Tokenizer | None = None,
) -> Iterator[str]:
  """Extract and lowercase tokens from the input text into words.

  This function utilizes the tokenizer module to tokenize text and yields
  lowercased words.

  Args:
    text (str): The text to be tokenized.
    tokenizer_inst: Optional tokenizer instance.

  Yields:
    Iterator[str]: An iterator over tokenized words.
  """
  if tokenizer_inst is not None:
    tokenized_pb2 = tokenizer_inst.tokenize(text)
  else:
    tokenized_pb2 = tokenizer_lib.tokenize(text)
  original_text = tokenized_pb2.text
  for token in tokenized_pb2.tokens:
    start = token.char_interval.start_pos
    end = token.char_interval.end_pos
    token_str = original_text[start:end]
    token_str = token_str.lower()
    yield token_str
````

_tokenize_with_lowercase 接受文本和可选 tokenizer，逐 token 用原文切片、lower 后 yield。它不修改 TokenizedText 的原始字符串或坐标。两个文本即便空白或大小写不同，仍可能视为 token 级精确匹配。

源码出处：[langextract/resolver.py:1115–1133](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1115-L1133)。

````python
def _find_token_occurrences(
    source_tokens: Sequence[str],
    extraction_tokens: Sequence[str],
) -> list[int]:
  """Returns all start indices of extraction_tokens within source_tokens."""
  num_source = len(source_tokens)
  num_extraction = len(extraction_tokens)
  if num_extraction == 0 or num_source < num_extraction:
    return []
  # Normalize so the slice comparison works for any Sequence input.
  expected = list(extraction_tokens)
  first_token = expected[0]
  return [
      i
      for i in range(num_source - num_extraction + 1)
      # Cheap first-token filter before the O(m) slice compare.
      if source_tokens[i] == first_token
      and list(source_tokens[i : i + num_extraction]) == expected
  ]
````

_find_token_occurrences 在 source_tokens 内枚举所有合法起点，空目标或目标更长时直接返回空列表。它把目标转为 list，先比较首 token，才比较整个窗口。这避免多数无关位置创建列表，也支持不同 Sequence 类型。返回全部匹配的起点，而不是第一个。

## 9.2 先来一个看起来不错的贪心方案

按答卷顺序，每张卡选最早不与上张卡重叠的候选。对于两次取件码的例子确实够用，但下面这个例子就不行了：

原文 token 为 A B C D，卡片 1 是 B，卡片 2 是 A B C。

| 卡片 | 候选区间 | 权重（匹配 token 数） |
|---|---|---:|
| B | [1,2) | 1 |
| A B C | [0,3) | 3 |

如果先选第一张卡，就不能选第二张卡。我们的目标是匹配总 token 数最多，而不是盲目保证每张卡都找到位置。跳过卡片 1 才是本阶段的最优选择。

先尝试枚举所有方案：

```python
def choose(occurrences, lengths, i=0, end=0):
    if i == len(lengths):
        return 0, {}
    best = choose(occurrences, lengths, i+1, end)
    for start in occurrences[i]:
        if start >= end:
            weight, selected = choose(occurrences, lengths, i+1, start+lengths[i])
            candidate = weight + lengths[i], {i: start, **selected}
            if candidate[0] > best[0]:
                best = candidate
    return best

assert choose([[1], [0]], [1, 3]) == (3, {1: 0})
```

它对每张卡尝试跳过或选取候选，规模稍大就会反复计算。这是一条通向动态规划的路。

## 9.3 将来还要继续放卡片：保留哪些历史？

假如两个历史方案分别匹配了 5、3 个 token，并都在位置 10 以前结束，前者结束还更早。那么后者没有理由留下：能接在后者后面的候选都能接在前者后面，且前者已有更高分。这叫支配关系。

| 方案 | 结束位置 | 匹配数 | 是否保留 |
|---|---:|---:|---|
| A | 4 | 2 | 是 |
| B | 7 | 5 | 是 |
| C | 9 | 3 | 否，B 支配 C |
| D | 12 | 8 | 是 |

保留的方案按结束位置排序，其权重也严格递增。这就是 frontier（这里是非支配状态的边界）。

源码出处：[langextract/resolver.py:1136–1141](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1136-L1141)。

````python
class _ChainNode(typing.NamedTuple):
  """One selected occurrence in a monotonic match chain."""

  extraction_index: int
  start: int
  parent: _ChainNode | None
````

_ChainNode 是不可变命名元组，保存卡片序号、选中的起点和前驱节点。它不复制整个选择历史，最后从 parent 逐级回溯。

源码出处：[langextract/resolver.py:1144–1149](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1144-L1149)。

````python
class _FrontierEntry(typing.NamedTuple):
  """Pareto-optimal: no kept chain ends earlier with at least this weight."""

  end: int
  weight: int
  node: _ChainNode
````

_FrontierEntry 保存一个方案的半开终点 end、累计 token 权重 weight 及链尾 node。用 end 而不是 start 判断下一张卡是否能接上。

源码出处：[langextract/resolver.py:1152–1224](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1152-L1224)。

````python
def _select_monotonic_matches(
    occurrence_lists: Sequence[Sequence[int]],
    extraction_lengths: Sequence[int],
) -> dict[int, int]:
  """Selects exact-match occurrences maximizing total matched tokens.

  Chooses at most one occurrence per extraction, keeping selections in
  extraction output order without overlap. Token-count weighting
  preserves the legacy preference for longer extractions in contested
  regions; ties prefer the earliest-ending chain, so repeated mentions
  resolve to successive occurrences.

  Args:
    occurrence_lists: Per-extraction sorted start token indices, in
      extraction output order.
    extraction_lengths: Per-extraction token counts.

  Returns:
    Dict mapping extraction index to its selected start token index.
  """
  # Frontier entries are strictly increasing in both end and weight.
  frontier: list[_FrontierEntry] = []
  frontier_end = operator.attrgetter("end")

  def best_ending_at_or_before(position: int) -> _FrontierEntry | None:
    idx = bisect.bisect_right(frontier, position, key=frontier_end)
    return frontier[idx - 1] if idx else None

  def insert_if_undominated(entry: _FrontierEntry) -> None:
    covering = best_ending_at_or_before(entry.end)
    if covering is not None and covering.weight >= entry.weight:
      return
    low = bisect.bisect_left(frontier, entry.end, key=frontier_end)
    # Evict entries ending at or after entry.end with no more weight;
    # they can never outscore entry as a predecessor.
    dominated_end = low
    while (
        dominated_end < len(frontier)
        and frontier[dominated_end].weight <= entry.weight
    ):
      dominated_end += 1
    frontier[low:dominated_end] = [entry]

  for extraction_index, (occurrences, length) in enumerate(
      zip(occurrence_lists, extraction_lengths, strict=True)
  ):
    if not occurrences or length == 0:
      continue
    # Candidates are computed against the pre-insert frontier so an
    # extraction cannot extend a chain that already contains it.
    candidates = []
    for start in occurrences:
      predecessor = best_ending_at_or_before(start)
      weight = length + (predecessor.weight if predecessor else 0)
      parent = predecessor.node if predecessor else None
      candidates.append(
          _FrontierEntry(
              start + length,
              weight,
              _ChainNode(extraction_index, start, parent),
          )
      )
    for candidate in candidates:
      insert_if_undominated(candidate)

  if not frontier:
    return {}
  selection: dict[int, int] = {}
  node: _ChainNode | None = frontier[-1].node
  while node is not None:
    selection[node.extraction_index] = node.start
    node = node.parent
  return selection
````

_select_monotonic_matches 实现上述动态规划。输入每张卡的有序候选起点和 token 长度，返回卡片序号到原文 token 起点的映射。

内部函数 best_ending_at_or_before 使用 bisect_right 在有序 frontier 中查找 end <= position 的最后一项。由于权重严格递增，最后一项就是合法前驱中最重的方案；无合法前驱返回 None。

内部函数 insert_if_undominated 先寻找结束不晚于新候选的已有方案，如果已有权重不小于新候选，则直接丢弃它。否则用 bisect_left 找插入位置，并删除其后结束较晚而权重不更大的连续条目，以切片替换写入新候选。

外层对每张卡先计算所有候选的前驱、权重和链节点，再统一插入。这个细节非常重要：若边计算边插入，同一张卡可能接在自己另一个候选后，被错误算两次。zip(strict=True) 防止两组平行数据长度不一致。没有候选或长度为零就跳过，旧 frontier 仍保留，表示可以不选本卡。

例如两张各长 1 的卡，都有候选 [1,3]：

| 处理阶段 | frontier（end, weight） | 含义 |
|---|---|---|
| 初始 | 空 | 没有选中 |
| 第 1 张 | (2,1) | 位置 1 优于位置 3 的同权重方案 |
| 第 2 张 | (2,1), (4,2) | 可以在位置 3 接上第一张 |

末尾取 frontier[-1]（最大权重），沿 parent 回溯到根节点，得到字典。同权重偏好较早结束的链。二分查找能减少前驱查找开销，但 Python 列表切片插入和删除仍有移动成本，因此不能因此宣称整个算法一定是 O(n log n)。

这一步只选择不重叠且遵守答卷顺序的精确匹配，不保证所有卡都匹配。后续回退匹配可以产生重叠证据。

## 9.4 把算法结果写回登记卡

源码出处：[langextract/resolver.py:1227–1282](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1227-L1282)。

````python
def _apply_monotonic_exact_matches(
    ordered_extractions: Sequence[data.Extraction],
    source_tokens: Sequence[str],
    tokenized_text: tokenizer_lib.TokenizedText,
    token_offset: int,
    char_offset: int,
    tokenizer_impl: tokenizer_lib.Tokenizer | None = None,
) -> list[data.Extraction]:
  """Aligns extractions exactly via the monotonic occurrence DP.

  Extractions the DP cannot place are left untouched for the difflib
  (lesser-match) and fuzzy fallback phases.

  Args:
    ordered_extractions: Extractions in model output order.
    source_tokens: Lowercased source tokens.
    tokenized_text: The tokenized source text, for interval lookup.
    token_offset: Token offset of the current chunk.
    char_offset: Character offset of the current chunk.
    tokenizer_impl: Optional tokenizer instance.

  Returns:
    The extractions aligned by the DP.
  """
  extraction_token_lists = [
      list(
          _tokenize_with_lowercase(
              extraction.extraction_text, tokenizer_inst=tokenizer_impl
          )
      )
      for extraction in ordered_extractions
  ]
  selection = _select_monotonic_matches(
      [
          _find_token_occurrences(source_tokens, tokens)
          for tokens in extraction_token_lists
      ],
      [len(tokens) for tokens in extraction_token_lists],
  )
  matched: list[data.Extraction] = []
  for extraction_index, start in selection.items():
    extraction = ordered_extractions[extraction_index]
    num_tokens = len(extraction_token_lists[extraction_index])
    extraction.token_interval = tokenizer_lib.TokenInterval(
        start_index=start + token_offset,
        end_index=start + num_tokens + token_offset,
    )
    start_token = tokenized_text.tokens[start]
    end_token = tokenized_text.tokens[start + num_tokens - 1]
    extraction.char_interval = data.CharInterval(
        start_pos=char_offset + start_token.char_interval.start_pos,
        end_pos=char_offset + end_token.char_interval.end_pos,
    )
    extraction.alignment_status = data.AlignmentStatus.MATCH_EXACT
    matched.append(extraction)
  return matched
````

_apply_monotonic_exact_matches 分词并小写每张卡内容，生成候选起点列表和长度列表，调用 DP。然后对选中的卡原地填写 token_interval、char_interval 及 MATCH_EXACT，返回成功的卡列表；未选中卡交给后续阶段。

为什么还要加 offset？假设第 8 章某块从全局 token 20、字符 100 开始，块内匹配 token [2,4)、字符 [8,15)，最终应保存全局 token [22,24)、字符 [108,115)。字符和 token 的偏移必须分别计算。

## 9.5 如果不用动态规划呢？

Python 标准库 difflib 可以寻找两组序列中连续相等的片段。

```python
from difflib import SequenceMatcher

m = SequenceMatcher(None, ["A", "B", "C"], ["B", "C"], autojunk=False)
assert list(m.get_matching_blocks()) == [(1, 0, 2), (3, 2, 0)]
```

元组是 (原文起点, 答卷起点, 连续相同的长度)，最后是长度 0 的哨兵。SequenceMatcher 是启发式匹配工具，不等同于最长公共子序列，也不是上一节的候选 DP。

源码出处：[langextract/resolver.py:581–585](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L581-L585)。

````python
  def __init__(self):
    """Initialize the WordAligner with difflib SequenceMatcher."""
    self.matcher = difflib.SequenceMatcher(autojunk=False)
    self.source_tokens: Sequence[str] | None = None
    self.extraction_tokens: Sequence[str] | None = None
````

WordAligner 负责让已解析的 Extraction 找回原文证据。__init__ 建立 SequenceMatcher(autojunk=False)，避免重复 token 被自动视为无用，同时把两组序列初始化为 None。

源码出处：[langextract/resolver.py:587–611](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L587-L611)。

````python
  def _set_seqs(
      self,
      source_tokens: Sequence[str] | Iterator[str],
      extraction_tokens: Sequence[str] | Iterator[str],
  ):
    """Sets the source and extraction tokens for alignment.

    Args:
      source_tokens: A nonempty sequence or iterator of word-level tokens from
        source text.
      extraction_tokens: A nonempty sequence or iterator of extraction tokens in
        order for matching to the source.
    """

    if isinstance(source_tokens, Iterator):
      source_tokens = list(source_tokens)
    if isinstance(extraction_tokens, Iterator):
      extraction_tokens = list(extraction_tokens)

    if not source_tokens or not extraction_tokens:
      raise ValueError("Source tokens and extraction tokens cannot be empty.")

    self.source_tokens = source_tokens
    self.extraction_tokens = extraction_tokens
    self.matcher.set_seqs(a=source_tokens, b=extraction_tokens)
````

_set_seqs 把一次性 Iterator 转成列表，拒绝空序列，保存两组序列并调用 matcher.set_seqs。匹配工具需要能重复访问序列，不能仅靠一次性迭代流。

源码出处：[langextract/resolver.py:613–628](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L613-L628)。

````python
  def _get_matching_blocks(self) -> Sequence[tuple[int, int, int]]:
    """Utilizes difflib SequenceMatcher and returns matching blocks of tokens.

    Returns:
      Sequence of matching blocks between source_tokens (S) and
      extraction_tokens
      (E). Each block (i, j, n) conforms to: S[i:i+n] == E[j:j+n], guaranteed to
      be monotonically increasing in j. Final entry is a dummy with value
      (len(S), len(E), 0).
    """
    if self.source_tokens is None or self.extraction_tokens is None:
      raise ValueError(
          "Source tokens and extraction tokens must be set before getting"
          " matching blocks."
      )
    return self.matcher.get_matching_blocks()
````

_get_matching_blocks 在未设置序列时抛 ValueError，否则委托 SequenceMatcher.get_matching_blocks。调用方会舍弃末尾长度为零的哨兵。

## 9.6 先理解整个对齐流程

类的完整源码：[WordAligner:578–1112](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L578-L1112)。各方法分第 9、10 章分别阅读。

WordAligner 不仅运行一个算法，它要组织多张卡的对齐。在默认 DP 阶段之后，项目还会尝试 difflib 匹配没有处理好的卡：用特殊分隔符把多张卡的 token 串起来，并建立各卡在拼接串中的起点索引。只有从卡起点开始的连续块才用来填这张卡。长度完全相同标记 MATCH_EXACT；长度不足仅在 accept_match_lesser 为真时标记 MATCH_LESSER，否则撤销坐标。

精确对齐并非字符串逐字相等，因为我们比较的是小写后的 token 序列。要查看证据真正是什么，始终以原文 char_interval 切片为准。

下一章将补上剩余部分：当助手漏词、多写词或单复数略有差别时，我们怎样决定它是否仍然找到了正确的原文？

## 9.7 练习

1. 在 choose 的输入中加入重复候选，检查一张卡不能被选两次。
2. 构造两个同权重方案，比较较早结束的方案对下一张卡有什么好处。
3. 对比 SequenceMatcher 和 DP 在有交错、重复词的短信中的结果。
