# 第 10 章　助手漏了一个词

原短信写的是 "Pickup code for your parcel is 3821"。助手填了 "Pickup code 3821"。上一章的完整连续匹配找不到这三个相邻 token，但我们人类一眼就能看出它可能在引用这句话。机器怎样判断？

## 10.1 子串、子序列和最长公共子序列

子串要求连续，子序列只要求次序不变。比如 A、C 是 A B C 的子序列，却不是它的连续子串。

```python
def lcs_length(a, b):
    dp = [[0]*(len(b)+1) for _ in range(len(a)+1)]
    for i in range(1, len(a)+1):
        for j in range(1, len(b)+1):
            if a[i-1] == b[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[-1][-1]

assert lcs_length(["pickup", "code", "is", "3821"],
                  ["pickup", "code", "3821"]) == 3
```

把表格想象成两位读者各自阅读一张纸，dp[i][j] 记录已经读过两张纸的前 i、j 项里最多能配对多少项。若末尾相等，可以配成一对；否则跳过一方的末尾，选较好的结果。表格避免重复递归。

但长度为 3 不能告诉我们应该标亮原短信的哪一段。如果三个匹配词分散在三条毫不相关的短信里呢？

## 10.2 两道门槛

我们既要问答卷中的词有多少被找到，又要问这些词在原文中挨得有多紧。

| 匹配 | 答卷长度 | 证据跨度 | 覆盖率 | 密度 |
|---|---:|---:|---:|---:|
| 3 个词密集出现 | 4 | 4 | 3/4 | 3/4 |
| 3 个词相距甚远 | 4 | 30 | 3/4 | 3/30 |
| 只找到 1 个词 | 4 | 1 | 1/4 | 1 |

覆盖率 = 匹配数 / 答卷 token 数。密度 = 匹配数 / 原文候选跨度的 token 数。上一章的精确匹配，要求全部 token 相邻；这里允许一定误差，但并不能证明语义正确。

源码出处：[langextract/resolver.py:46–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L46-L55)。

````python
class LcsSpan(typing.NamedTuple):
  """Result of _best_lcs_span: matched token count and source span."""

  matches: int
  start: int
  end: int

  @property
  def span_len(self) -> int:
    return 0 if self.start < 0 else self.end - self.start + 1
````

LcsSpan 是记录 matches、start、end 的命名元组。这里 end 是最后一个匹配跨度中的 token 下标（包含），与正式 TokenInterval 的右开约定不同。span_len 属性在 start 为负时返回 0，否则 end-start+1；转换为登记卡区间时要特别注意加 1。

源码出处：[langextract/resolver.py:1416–1443](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1416-L1443)。

````python
def _accept_lcs_match(
    span: LcsSpan,
    extraction_len: int,
    threshold: float = _FUZZY_ALIGNMENT_MIN_THRESHOLD,
    min_density: float = _FUZZY_ALIGNMENT_MIN_DENSITY,
) -> bool:
  """Applies coverage and density gates to an LCS result.

  Coverage gate (threshold): did we find enough of the extraction?
  Requires matches >= ceil(extraction_len * threshold).

  Density gate (min_density): is the match tight enough? Requires
  matches / span_len >= min_density, rejecting scattered matches
  where matched tokens are spread across too much noise.

  Args:
    span: LcsSpan result from _best_lcs_span.
    extraction_len: Number of tokens in the extraction.
    threshold: Minimum fraction of the extraction that must match.
    min_density: Minimum ratio of matched tokens to span length.
  """
  if span.matches == 0 or extraction_len == 0:
    return False
  needed = math.ceil(extraction_len * threshold)
  if span.span_len <= 0:
    return False
  density = span.matches / span.span_len
  return span.matches >= needed and density >= min_density
````

_accept_lcs_match 用 ceil(extraction_len * threshold) 计算整数最低匹配量，避免 4 个词要求 0.75 时少于 3 个也通过。拒绝零匹配、零目标长度或非正跨度，再同时检查覆盖量和密度。默认 threshold=0.75，min_density=1/3。

## 10.3 先让比较少受单复数干扰

源码出处：[langextract/resolver.py:1314–1320](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1314-L1320)。

````python
@functools.lru_cache(maxsize=10000)
def _normalize_token(token: str) -> str:
  """Lowercases and applies light pluralisation stemming."""
  token = token.lower()
  if len(token) > 3 and token.endswith("s") and not token.endswith("ss"):
    token = token[:-1]
  return token
````

_normalize_token 转小写，长度大于 3、以 s 结尾且不以 ss 结尾时去掉最后一个 s。例如 parcels → parcel，而 glass 不变。它用最多 10000 项的 LRU 缓存，节省反复处理相同词的开销。它不是完整词干提取，更不能认出所有同义词；例如 news 也可能被处理成 new。原文本身不会改变。

## 10.4 只保留最长公共子序列还不够

设原文 token 为：

```python
source = ["a", "b", "c"] + ["noise"] * 20 + ["d"]
extraction = ["a", "b", "c", "d"]
```

最长公共子序列能匹配 4 项，但跨度 24，密度只有 1/6，不满足 1/3。前三项自身匹配 3，覆盖率 3/4，跨度 3，密度 1，可以接受。因此项目不只求一个最长结果，而是对每一种匹配数量 k 记录最紧凑的跨度。

下列实验用很慢、但容易检查的穷举方案说明目标：

```python
def best_spans(source, extraction):
    from itertools import combinations
    result = {}
    for k in range(1, min(len(source), len(extraction))+1):
        for indices in combinations(range(len(source)), k):
            candidate = [source[i] for i in indices]
            if not any(candidate == [extraction[j] for j in js]
                       for js in combinations(range(len(extraction)), k)):
                continue
            start, end = indices[0], indices[-1]
            old = result.get(k)
            if old is None or (end-start, start) < (old[1]-old[0], old[0]):
                result[k] = (start, end)
    return result

assert best_spans(["a", "x", "a", "b"], ["a", "b"])[2] == (2, 3)
```

这个程序枚举原文和答卷的索引组合，复杂度很高，不能拿来处理长文。它的作用是说明我们希望正式算法算出什么。

## 10.5 动态规划再加一维

源码出处：[langextract/resolver.py:1326–1402](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1326-L1402)。

````python
def _best_lcs_spans(
    source: Sequence[str],
    extraction: Sequence[str],
) -> dict[int, LcsSpan]:
  """Finds the tightest source span for each achievable match count.

  Runs an O(n * m^2) time / O(m^2) memory DP with rolling rows on the
  i dimension. For each (i, j, k), tracks the latest source index s
  such that source[s..i) contains at least k tokens of extraction[0..j)
  as a subsequence. Preferring later starts produces minimum-span
  results; among spans of equal length, the earliest start wins.
  Tracking per match count lets the caller try a denser k-1 match if
  the maximum-match span fails a density gate.

  Args:
    source: Normalized source tokens.
    extraction: Normalized extraction tokens.

  Returns:
    Dict mapping each achievable match count k (1..m) to its tightest
    LcsSpan. Empty dict if no matches found.
  """
  n = len(source)
  m = len(extraction)
  if n == 0 or m == 0:
    return {}

  # Rolling rows: prev_row[j][k] holds span_start[i-1][j][k],
  # curr_row[j][k] holds span_start[i][j][k]. k=0 is trivially
  # achievable: at i, source[i..i) is empty, so s=i gives 0 matches.
  prev_row = [[-1] * (m + 1) for _ in range(m + 1)]
  curr_row = [[-1] * (m + 1) for _ in range(m + 1)]
  for j in range(m + 1):
    prev_row[j][0] = 0  # i=0 base: k=0 achievable with s=0

  # best_per_k[k] = tightest span seen so far with exactly k matches.
  best_per_k: dict[int, LcsSpan] = {}

  for i in range(1, n + 1):
    src_tok = source[i - 1]
    # Initialize column j=0 of curr_row: k=0 achievable with s=i, k>0 not.
    curr_row[0][0] = i
    for k in range(1, m + 1):
      curr_row[0][k] = -1

    for j in range(1, m + 1):
      curr_row[j][0] = i
      matches_here = src_tok == extraction[j - 1]
      for k in range(1, m + 1):
        skip_source = prev_row[j][k]
        skip_extraction = curr_row[j - 1][k]
        best = skip_source if skip_source > skip_extraction else skip_extraction
        if matches_here:
          if k == 1:
            match_cand = i - 1
          else:
            match_cand = prev_row[j - 1][k - 1]
          best = max(best, match_cand)
        curr_row[j][k] = best

    end = i - 1
    for k in range(1, m + 1):
      s = curr_row[m][k]
      if s < 0:
        continue
      existing = best_per_k.get(k)
      if existing is None:
        best_per_k[k] = LcsSpan(matches=k, start=s, end=end)
        continue
      new_len = end - s + 1
      cur_len = existing.span_len
      if new_len < cur_len or (new_len == cur_len and s < existing.start):
        best_per_k[k] = LcsSpan(matches=k, start=s, end=end)

    prev_row, curr_row = curr_row, prev_row

  return best_per_k
````

_best_lcs_spans 不直接保存最长匹配数，而在状态 (i,j,k) 中保存最晚可行的原文起点 s：source[s:i] 能容纳 extraction[:j] 中的 k 个顺序匹配项。-1 表示不可达。同一个 i 下，起点越晚跨度越短。

空匹配一定可行，起点取 i；空原文或空目标则直接返回空字典。对非零 k，有三个选择：

| 选择 | 取值 |
|---|---|
| 跳过原文末项 | prev_row[j][k] |
| 跳过答卷末项 | curr_row[j-1][k] |
| 两方末项相同并配对 | i-1（k=1）或 prev_row[j-1][k-1] |

取三者有效起点中最大值，保留最短的尾部跨度。为什么可以只留最大的起点？相同 i,j,k 的后续匹配选择完全相同，起点更晚不会使后续跨度变长。

每处理一个原文位置 i，就检查完整答卷对应的各个 k，将 end=i-1 与该起点组成候选。best_per_k 保存历史中跨度最短者；长度相同选择起点更早的跨度。

我们用前面那个 a x a b 的例子观察 k=2：

| i（已读原文） | 此时是否能匹配 a,b | 最晚起点 |
|---:|---|---:|
| 1 | 否 | -1 |
| 2 | 否 | -1 |
| 3 | 否 | -1 |
| 4 | 是 | 2 |

得到 [2,3]（包含末尾）而不是 [0,3]。因为第 i 行只依赖前一行和本行更小 j，项目用两张 (m+1)×(m+1) 的表轮换复用，时间 O(n m²)，内存 O(m²)，其中 n 为原文 token 数、m 为答卷 token 数。与只算 LCS 长度的二维 DP 相比，它多花了资源来同时衡量匹配数量与跨度。

源码出处：[langextract/resolver.py:1405–1413](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1405-L1413)。

````python
def _best_lcs_span(
    source: Sequence[str],
    extraction: Sequence[str],
) -> LcsSpan:
  """Returns the max-match LCS span. Wrapper over _best_lcs_spans."""
  spans = _best_lcs_spans(source, extraction)
  if not spans:
    return _NO_MATCH
  return spans[max(spans.keys())]
````

_best_lcs_span 是简单包装器：返回匹配数量最大的 LcsSpan，没有可行匹配就返回 _NO_MATCH（matches=0、start=end=-1）。它不负责密度筛选。只调用它会遗漏前面说的密集次优匹配。

## 10.6 把密集的近似证据写回登记卡

源码出处：[langextract/resolver.py:756–826](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L756-L826)。

````python
  def _lcs_fuzzy_align_extraction(
      self,
      extraction: data.Extraction,
      source_tokens_norm: list[str],
      tokenized_text: tokenizer_lib.TokenizedText,
      token_offset: int,
      char_offset: int,
      fuzzy_alignment_threshold: float = _FUZZY_ALIGNMENT_MIN_THRESHOLD,
      fuzzy_alignment_min_density: float = _FUZZY_ALIGNMENT_MIN_DENSITY,
      tokenizer_impl: tokenizer_lib.Tokenizer | None = None,
  ) -> data.Extraction | None:
    """Fuzzy-align an extraction using LCS DP with coverage/density gates.

    Args:
      extraction: The extraction to align.
      source_tokens_norm: Pre-normalized source tokens.
      tokenized_text: The tokenized source text.
      token_offset: Token offset of the current chunk.
      char_offset: Character offset of the current chunk.
      fuzzy_alignment_threshold: Minimum coverage fraction for acceptance.
      fuzzy_alignment_min_density: Minimum matched-to-span density.
      tokenizer_impl: Optional tokenizer instance.

    Returns:
      The aligned extraction on success, None otherwise.
    """
    extraction_tokens = list(
        _tokenize_with_lowercase(
            extraction.extraction_text, tokenizer_inst=tokenizer_impl
        )
    )
    if not extraction_tokens:
      return None

    extraction_tokens_norm = [_normalize_token(t) for t in extraction_tokens]

    logging.debug(
        "LCS fuzzy aligning %r (%d tokens)",
        extraction.extraction_text,
        len(extraction_tokens),
    )

    # Try spans by decreasing match count: a sparse max-match span may fail
    # the density gate while a denser sub-match span still passes coverage.
    spans = _best_lcs_spans(source_tokens_norm, extraction_tokens_norm)
    accepted: LcsSpan | None = None
    for k in sorted(spans.keys(), reverse=True):
      candidate = spans[k]
      if _accept_lcs_match(
          candidate,
          len(extraction_tokens_norm),
          threshold=fuzzy_alignment_threshold,
          min_density=fuzzy_alignment_min_density,
      ):
        accepted = candidate
        break
    if accepted is None:
      return None

    extraction.token_interval = tokenizer_lib.TokenInterval(
        start_index=accepted.start + token_offset,
        end_index=accepted.end + 1 + token_offset,
    )
    start_token = tokenized_text.tokens[accepted.start]
    end_token = tokenized_text.tokens[accepted.end]
    extraction.char_interval = data.CharInterval(
        start_pos=char_offset + start_token.char_interval.start_pos,
        end_pos=char_offset + end_token.char_interval.end_pos,
    )
    extraction.alignment_status = data.AlignmentStatus.MATCH_FUZZY
    return extraction
````

_lcs_fuzzy_align_extraction 接收已经规范化的原文 token（同一块复用），自行切分并规范化当前卡文本。空 token 直接返回 None。调用 _best_lcs_spans 后，按照 k 从大到小检查 _accept_lcs_match，一旦通过便选中；都不通过返回 None。

成功时把 start 和 end+1 加上全局 token offset，字符区间使用首尾 Token 的原文位置加 char_offset。登记卡标记 MATCH_FUZZY，而不是 MATCH_EXACT，告诉读者这只是近似证据。它返回同一个被修改的 Extraction。

## 10.7 旧算法为什么还存在？

源码出处：[langextract/resolver.py:630–754](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L630-L754)。

````python
  def _fuzzy_align_extraction(
      self,
      extraction: data.Extraction,
      source_tokens: list[str],
      tokenized_text: tokenizer_lib.TokenizedText,
      token_offset: int,
      char_offset: int,
      fuzzy_alignment_threshold: float = _FUZZY_ALIGNMENT_MIN_THRESHOLD,
      tokenizer_impl: tokenizer_lib.Tokenizer | None = None,
  ) -> data.Extraction | None:
    """Fuzzy-align an extraction using difflib.SequenceMatcher on tokens.

    The algorithm scans every candidate window in `source_tokens` and selects
    the window with the highest SequenceMatcher `ratio`. It uses an efficient
    token-count intersection as a fast pre-check to discard windows that cannot
    meet the alignment threshold. A match is accepted when the ratio is ≥
    `fuzzy_alignment_threshold`. This only runs on unmatched extractions, which
    is usually a small subset of the total extractions.

    Args:
      extraction: The extraction to align.
      source_tokens: The tokens from the source text.
      tokenized_text: The tokenized source text.
      token_offset: The token offset of the current chunk.
      char_offset: The character offset of the current chunk.
      fuzzy_alignment_threshold: The minimum ratio for a fuzzy match.
      tokenizer_impl: Optional tokenizer instance.

    Returns:
      The aligned data.Extraction if successful, None otherwise.
    """

    extraction_tokens = list(
        _tokenize_with_lowercase(
            extraction.extraction_text, tokenizer_inst=tokenizer_impl
        )
    )
    # Work with lightly stemmed tokens so pluralisation doesn't block alignment
    extraction_tokens_norm = [_normalize_token(t) for t in extraction_tokens]

    if not extraction_tokens:
      return None

    logging.debug(
        "Fuzzy aligning %r (%d tokens)",
        extraction.extraction_text,
        len(extraction_tokens),
    )

    best_ratio = 0.0
    best_span: tuple[int, int] | None = None  # (start_idx, window_size)

    len_e = len(extraction_tokens)
    max_window = len(source_tokens)

    extraction_counts = collections.Counter(extraction_tokens_norm)
    min_overlap = int(len_e * fuzzy_alignment_threshold)

    matcher = difflib.SequenceMatcher(autojunk=False, b=extraction_tokens_norm)

    for window_size in range(len_e, max_window + 1):
      if window_size > len(source_tokens):
        break

      # Initialize for sliding window
      window_deque = collections.deque(source_tokens[0:window_size])
      window_counts = collections.Counter(
          [_normalize_token(t) for t in window_deque]
      )

      for start_idx in range(len(source_tokens) - window_size + 1):
        # Optimization: check if enough overlapping tokens exist before expensive
        # sequence matching. This is an upper bound on the match count.
        if (extraction_counts & window_counts).total() >= min_overlap:
          window_tokens_norm = [_normalize_token(t) for t in window_deque]
          matcher.set_seq1(window_tokens_norm)
          matches = sum(size for _, _, size in matcher.get_matching_blocks())
          if len_e > 0:
            ratio = matches / len_e
          else:
            ratio = 0.0
          if ratio > best_ratio:
            best_ratio = ratio
            best_span = (start_idx, window_size)

        # Slide the window to the right
        if start_idx + window_size < len(source_tokens):
          # Remove the leftmost token from the count
          old_token = window_deque.popleft()
          old_token_norm = _normalize_token(old_token)
          window_counts[old_token_norm] -= 1
          if window_counts[old_token_norm] == 0:
            del window_counts[old_token_norm]

          # Add the new rightmost token to the deque and count
          new_token = source_tokens[start_idx + window_size]
          window_deque.append(new_token)
          new_token_norm = _normalize_token(new_token)
          window_counts[new_token_norm] += 1

    if best_span and best_ratio >= fuzzy_alignment_threshold:
      start_idx, window_size = best_span

      try:
        extraction.token_interval = tokenizer_lib.TokenInterval(
            start_index=start_idx + token_offset,
            end_index=start_idx + window_size + token_offset,
        )

        start_token = tokenized_text.tokens[start_idx]
        end_token = tokenized_text.tokens[start_idx + window_size - 1]
        extraction.char_interval = data.CharInterval(
            start_pos=char_offset + start_token.char_interval.start_pos,
            end_pos=char_offset + end_token.char_interval.end_pos,
        )

        extraction.alignment_status = data.AlignmentStatus.MATCH_FUZZY
        return extraction
      except IndexError:
        logging.exception(
            "Index error while setting intervals during fuzzy alignment."
        )
        return None

    return None
````

这是供兼容使用的 difflib 模糊匹配。它先规范化目标 token，创建 Counter 和 SequenceMatcher；窗口大小从目标长度开始，一直到整篇原文大小。对每个大小使用 deque 滑动窗口，移走左端时减少计数，加入右端时增加计数。

两个 Counter 的交集计数是可能匹配数的上界。若交集不足，就没有必要做昂贵的序列匹配。通过预筛时，用 matcher.get_matching_blocks 的长度之和除以目标长度（注意实际实现并非 SequenceMatcher.ratio()）得到覆盖比例，保存严格更高分的窗口；同分保留先遇到的窗口。

最佳比例达到阈值才填写全局 token 和字符区间、标记 MATCH_FUZZY；索引错误记录日志后返回 None。它没有独立密度门槛，而且枚举许多窗口，因此可能找到又长又稀疏的证据。默认 LCS 路径与之有别。

## 10.8 WordAligner：把所有匹配方式串起来

源码出处：[langextract/resolver.py:828–1112](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L828-L1112)。

````python
  def align_extractions(
      self,
      extraction_groups: Sequence[Sequence[data.Extraction]],
      source_text: str,
      token_offset: int = 0,
      char_offset: int = 0,
      delim: str = "\u241F",  # Unicode Symbol for unit separator
      enable_fuzzy_alignment: bool = True,
      fuzzy_alignment_threshold: float = _FUZZY_ALIGNMENT_MIN_THRESHOLD,
      accept_match_lesser: bool = True,
      tokenizer_impl: tokenizer_lib.Tokenizer | None = None,
      *,
      fuzzy_alignment_algorithm: str = _DEFAULT_FUZZY_ALGORITHM,
      fuzzy_alignment_min_density: float = _FUZZY_ALIGNMENT_MIN_DENSITY,
      exact_alignment_algorithm: str = _DEFAULT_EXACT_ALGORITHM,
  ) -> Sequence[Sequence[data.Extraction]]:
    """Aligns extractions with their positions in the source text.

    This method takes a sequence of extractions and the source text, aligning
    each extraction with its corresponding position in the source text. It
    returns a sequence of extractions along with token intervals indicating the
    start and
    end positions of each extraction in the source text. If an extraction cannot
    be
    aligned, its token interval is set to None.

    Args:
      extraction_groups: A sequence of sequences, where each inner sequence
        contains an Extraction object.
      source_text: The source text against which extractions are to be aligned.
      token_offset: The offset to add to the start and end indices of the token
        intervals.
      char_offset: The offset to add to the start and end positions of the
        character intervals.
      delim: Token used to separate multi-token extractions.
      enable_fuzzy_alignment: Whether to use fuzzy alignment when exact matching
        fails.
      fuzzy_alignment_threshold: Minimum fraction of extraction tokens that
        must be matched (0-1). Default 0.75.
      accept_match_lesser: Whether to accept partial exact matches (MATCH_LESSER
        status).
      tokenizer_impl: Optional tokenizer instance.
      fuzzy_alignment_algorithm: "lcs" (default) or "legacy" (deprecated,
        will be removed in a future release). Keyword-only.
      fuzzy_alignment_min_density: Minimum ratio of matched tokens to source
        span length (LCS only). Keyword-only.
      exact_alignment_algorithm: "dp" (default) aligns exact matches via
        an order-preserving occurrence DP, so repeated mentions map to
        successive occurrences and spans of different extractions may
        overlap. "difflib" restores the legacy greedy behavior with
        non-overlapping exact spans. Keyword-only.

    Returns:
      A sequence of extractions aligned with the source text, including token
      intervals.
    """
    if exact_alignment_algorithm not in _VALID_EXACT_ALGORITHMS:
      raise ValueError(
          f"Invalid exact_alignment_algorithm {exact_alignment_algorithm!r};"
          f" expected one of {sorted(_VALID_EXACT_ALGORITHMS)}."
      )
    if enable_fuzzy_alignment:
      if fuzzy_alignment_algorithm not in _VALID_FUZZY_ALGORITHMS:
        raise ValueError(
            f"Invalid fuzzy_alignment_algorithm {fuzzy_alignment_algorithm!r};"
            f" expected one of {sorted(_VALID_FUZZY_ALGORITHMS)}."
        )
      if not 0.0 <= fuzzy_alignment_threshold <= 1.0:
        raise ValueError(
            "fuzzy_alignment_threshold must be in [0, 1]; got"
            f" {fuzzy_alignment_threshold!r}."
        )
      if not 0.0 <= fuzzy_alignment_min_density <= 1.0:
        raise ValueError(
            "fuzzy_alignment_min_density must be in [0, 1]; got"
            f" {fuzzy_alignment_min_density!r}."
        )
      if fuzzy_alignment_algorithm == _FUZZY_ALGORITHM_LEGACY:
        warnings.warn(
            "fuzzy_alignment_algorithm='legacy' is deprecated and will be"
            " removed in a future release. Use the default 'lcs' algorithm.",
            DeprecationWarning,
            stacklevel=2,
        )
    logging.debug(
        "WordAligner: Starting alignment of extractions with the source text."
        " Extraction groups to align: %s",
        extraction_groups,
    )
    if not extraction_groups:
      logging.info("No extraction groups provided; returning empty list.")
      return []

    source_tokens = list(
        _tokenize_with_lowercase(source_text, tokenizer_inst=tokenizer_impl)
    )

    delim_len = len(
        list(_tokenize_with_lowercase(delim, tokenizer_inst=tokenizer_impl))
    )
    if delim_len != 1:
      raise ValueError(f"Delimiter {delim!r} must be a single token.")

    logging.debug("Using delimiter %r for extraction alignment", delim)

    extraction_tokens = list(
        _tokenize_with_lowercase(
            f" {delim} ".join(
                extraction.extraction_text
                for extraction in itertools.chain(*extraction_groups)
            ),
            tokenizer_inst=tokenizer_impl,
        )
    )

    self._set_seqs(source_tokens, extraction_tokens)

    index_to_extraction_group = {}
    extraction_index = 0
    for group_index, group in enumerate(extraction_groups):
      logging.debug(
          "Processing extraction group %d with %d extractions.",
          group_index,
          len(group),
      )
      for extraction in group:
        # Validate delimiter doesn't appear in extraction text
        if delim in extraction.extraction_text:
          raise ValueError(
              f"Delimiter {delim!r} appears inside extraction text"
              f" {extraction.extraction_text!r}. This would corrupt alignment"
              " mapping."
          )

        index_to_extraction_group[extraction_index] = (extraction, group_index)
        extraction_text_tokens = list(
            _tokenize_with_lowercase(
                extraction.extraction_text, tokenizer_inst=tokenizer_impl
            )
        )
        extraction_index += len(extraction_text_tokens) + delim_len

    aligned_extraction_groups: list[list[data.Extraction]] = [
        [] for _ in extraction_groups
    ]
    tokenized_text = (
        tokenizer_impl.tokenize(source_text)
        if tokenizer_impl
        else tokenizer_lib.tokenize(source_text)
    )

    # Track which extractions were aligned in the exact matching phase
    aligned_extractions = []
    exact_matches = 0
    lesser_matches = 0

    # Track DP-aligned extractions by id(): duplicate extractions
    # compare equal, so a value-based set would conflate them.
    dp_matched_extraction_ids: set[int] = set()
    if exact_alignment_algorithm == _EXACT_ALGORITHM_DP:
      dp_matched = _apply_monotonic_exact_matches(
          [ext for ext, _ in index_to_extraction_group.values()],
          source_tokens,
          tokenized_text,
          token_offset,
          char_offset,
          tokenizer_impl=tokenizer_impl,
      )
      exact_matches += len(dp_matched)
      aligned_extractions.extend(dp_matched)
      dp_matched_extraction_ids = {id(ext) for ext in dp_matched}

    # Exact matching phase
    for i, j, n in self._get_matching_blocks()[:-1]:
      extraction, _ = index_to_extraction_group.get(j, (None, None))
      if extraction is None:
        logging.debug(
            "No clean start index found for extraction index=%d iterating"
            " Difflib matching_blocks",
            j,
        )
        continue
      if id(extraction) in dp_matched_extraction_ids:
        continue

      extraction.token_interval = tokenizer_lib.TokenInterval(
          start_index=i + token_offset,
          end_index=i + n + token_offset,
      )

      try:
        start_token = tokenized_text.tokens[i]
        end_token = tokenized_text.tokens[i + n - 1]
        extraction.char_interval = data.CharInterval(
            start_pos=char_offset + start_token.char_interval.start_pos,
            end_pos=char_offset + end_token.char_interval.end_pos,
        )
      except IndexError as e:
        raise IndexError(
            "Failed to align extraction with source text. Extraction token"
            f" interval {extraction.token_interval} does not match source text"
            f" tokens {tokenized_text.tokens}."
        ) from e

      extraction_text_len = len(
          list(
              _tokenize_with_lowercase(
                  extraction.extraction_text, tokenizer_inst=tokenizer_impl
              )
          )
      )
      if extraction_text_len < n:
        raise ValueError(
            "Delimiter prevents blocks greater than extraction length: "
            f"extraction_text_len={extraction_text_len}, block_size={n}"
        )
      if extraction_text_len == n:
        extraction.alignment_status = data.AlignmentStatus.MATCH_EXACT
        exact_matches += 1
        aligned_extractions.append(extraction)
      else:
        # Partial match (extraction longer than matched text)
        if accept_match_lesser:
          extraction.alignment_status = data.AlignmentStatus.MATCH_LESSER
          lesser_matches += 1
          aligned_extractions.append(extraction)
        else:
          # Reset intervals when not accepting lesser matches
          extraction.token_interval = None
          extraction.char_interval = None
          extraction.alignment_status = None

    # Collect unaligned extractions
    unaligned_extractions = []
    for extraction, _ in index_to_extraction_group.values():
      if extraction not in aligned_extractions:
        unaligned_extractions.append(extraction)

    if enable_fuzzy_alignment and unaligned_extractions:
      logging.debug(
          "Starting fuzzy alignment (%s) for %d unaligned extractions",
          fuzzy_alignment_algorithm,
          len(unaligned_extractions),
      )
      src_norm = (
          [_normalize_token(t) for t in source_tokens]
          if fuzzy_alignment_algorithm == _FUZZY_ALGORITHM_LCS
          else None
      )
      for extraction in unaligned_extractions:
        if fuzzy_alignment_algorithm == _FUZZY_ALGORITHM_LCS:
          aligned_extraction = self._lcs_fuzzy_align_extraction(
              extraction,
              src_norm,
              tokenized_text,
              token_offset,
              char_offset,
              fuzzy_alignment_threshold=fuzzy_alignment_threshold,
              fuzzy_alignment_min_density=fuzzy_alignment_min_density,
              tokenizer_impl=tokenizer_impl,
          )
        else:
          aligned_extraction = self._fuzzy_align_extraction(
              extraction,
              source_tokens,
              tokenized_text,
              token_offset,
              char_offset,
              fuzzy_alignment_threshold,
              tokenizer_impl=tokenizer_impl,
          )
        if aligned_extraction:
          aligned_extractions.append(aligned_extraction)
          logging.debug(
              "Fuzzy alignment successful for extraction: %s",
              extraction.extraction_text,
          )

    for extraction, group_index in index_to_extraction_group.values():
      aligned_extraction_groups[group_index].append(extraction)

    logging.debug(
        "Final aligned extraction groups: %s", aligned_extraction_groups
    )
    return aligned_extraction_groups
````

WordAligner 类在第 9 章已经认识；这个类把序列比较对象、精确算法和模糊算法放在同一个协调器内。align_extractions 是真正的入口，输入成组的卡片及当前原文块，输出保持分组的卡片。

首先验证 exact_alignment_algorithm 只能是 dp 或 difflib；启用模糊时验证算法名及两项 [0,1] 的阈值，legacy 会发出弃用警告。空的外层分组直接返回 []。

接下来分词、小写原文，检查分隔符恰好是一个 token，将多张卡的内容用分隔符连接并建立起点到卡片及其组号的映射。卡片文本自身含分隔符会报错，否则该映射将不可信。_set_seqs 设置 difflib 使用的序列，再取带坐标的原文 token 表。

默认 dp 分支先对精确候选运行第 9 章的 DP，记录成功对象的 id；用身份而非值相等来区分看起来相同的卡。随后扫描 difflib 块（排除末尾哨兵），只有从卡片起点开始且未被 DP 处理的块才使用。根据块长度设置 MATCH_EXACT 或在允许时 MATCH_LESSER；不接受的部分匹配清除刚填入的字段。

最后收集没有出现在已匹配列表里的卡片。若启用了模糊匹配，LCS 路径先一次性规范化原文 token，每张剩余卡调用 _lcs_fuzzy_align_extraction；legacy 则逐张调用 _fuzzy_align_extraction。最后遍历原先的映射，把所有卡（包括没有找到位置的卡）放回原始分组顺序。没有找到证据不代表登记卡消失。

这里的精确 DP 只保证它自身选择的链不重叠；difflib 回退及模糊匹配没有全局排斥已经使用过的区间。最终的证据可能重合，也可能不符合人的预期。证据定位不是事实核验。

## 10.9 练习

1. 使用 lcs_length 对比连续匹配和跳词匹配。
2. 改变噪声 token 的数量，手算上例覆盖率和密度，看看在哪个长度时证据被拒绝。
3. 用 best_spans 验证正式 _best_lcs_spans 在小规模输入上的结果。
4. 找一个英语单词，观察 _normalize_token 的词尾规则可能产生的误判。

至此，一个信封中的信息可以被抽取、解析并对齐。下一卷将把信封、文档、模型调用、进度及去重组合成真正的处理流水线。
