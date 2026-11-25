# 第 7 章　短信中的哪几个字？

上一章我们已经能把助手的答卷变成登记卡。现在卡上写着取件码 3821，原短信却是「您的包裹已到，取件码 3821。」。我们还需要在短信中把这四个数字标出来。

## 7.1 从字的位置开始

```python
text = "您的包裹已到，取件码 3821。"
start = text.index("3821")
end = start + len("3821")
assert text[start:end] == "3821"
print(start, end)
```

这是一个字符区间（character interval）。左边包含，右边不包含。为什么不用最后一个字符的位置？因为 end - start 恰好就是长度，也可以直接交给 Python 切片。

可是助手回答的是 "Pickup code"，原文写的是 "PICKUP  CODE"。直接找字符串找不到。我们决定换一种单位：先给原文切分，然后在切分后的序列中找。

```python
import re

text = "PICKUP  CODE: 3821"
parts = [(m.group(), *m.span()) for m in re.finditer(r"[^\W\d_]+|\d+|[^\w\s]", text)]
print(parts)
assert text[parts[0][1]:parts[1][2]] == "PICKUP  CODE"
```

注意这里没有把字符串 split 后再 join 回去。两个空格是原件的一部分。我们保留原件，另外建一张索引表。

| token 序号 | 内容 | 字符区间 |
|---|---|---|
| 0 | PICKUP | [0,6) |
| 1 | CODE | [8,12) |
| 2 | : | [12,13) |
| 3 | 3821 | [14,18) |

这里的 token 是证据对齐用的文本单元，不是大模型计算费用时使用的 token。两套切分系统有不同的目标，不能混用。

## 7.2 给索引表定义数据结构

正式源码位于 langextract/core/tokenizer.py。

源码出处：[langextract/core/tokenizer.py:65–75](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L65-L75)。

````python
@dataclasses.dataclass(slots=True)
class CharInterval:
  """Represents a range of character positions in the original text.

  Attributes:
    start_pos: The starting character index (inclusive).
    end_pos: The ending character index (exclusive).
  """

  start_pos: int
  end_pos: int
````

CharInterval 保存 start_pos、end_pos，不校验实际文本；调用方负责保证合法。slots=True 限制实例字段，有利于大量小对象的内存开销。

源码出处：[langextract/core/tokenizer.py:78–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L78-L91)。

````python
@dataclasses.dataclass(slots=True)
class TokenInterval:
  """Represents an interval over tokens in tokenized text.

  The interval is defined by a start index (inclusive) and an end index
  (exclusive).

  Attributes:
    start_index: The index of the first token in the interval.
    end_index: The index one past the last token in the interval.
  """

  start_index: int = 0
  end_index: int = 0
````

TokenInterval 保存 token 列表中的半开区间，默认 [0,0)。比如 [0,2) 对应上述两个单词，并不等于字符 [0,2)。

源码出处：[langextract/core/tokenizer.py:94–105](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L94-L105)。

````python
class TokenType(enum.IntEnum):
  """Enumeration of token types produced during tokenization.

  Attributes:
    WORD: Represents an alphabetical word token.
    NUMBER: Represents a numeric token.
    PUNCTUATION: Represents punctuation characters.
  """

  WORD = 0
  NUMBER = 1
  PUNCTUATION = 2
````

TokenType 用 IntEnum 表达单词、数字、标点三类；枚举比散落在各处的 0、1、2 更能说明意图。没有独立的空白类型。

源码出处：[langextract/core/tokenizer.py:108–131](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L108-L131)。

````python
@dataclasses.dataclass(slots=True)
class Token:
  """Represents a token extracted from text.

  Each token is assigned an index and classified into a type (word, number,
  punctuation, or acronym). The token also records the range of characters
  (its CharInterval) that correspond to the substring from the original text.
  Additionally, it tracks whether it follows a newline.

  Attributes:
    index: The position of the token in the sequence of tokens.
    token_type: The type of the token, as defined by TokenType.
    char_interval: The character interval within the original text that this
      token spans.
    first_token_after_newline: True if the token immediately follows a newline
      or carriage return.
  """

  index: int
  token_type: TokenType
  char_interval: CharInterval = dataclasses.field(
      default_factory=lambda: CharInterval(0, 0)
  )
  first_token_after_newline: bool = False
````

Token 保存自己的序号、类型、字符区间、是否紧接换行后的第一个 token。default_factory 使每个实例有独立的默认区间；不能把同一个可变区间作为所有 token 的默认值。

源码出处：[langextract/core/tokenizer.py:134–145](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L134-L145)。

````python
@dataclasses.dataclass
class TokenizedText:
  """Holds the result of tokenizing a text string.

  Attributes:
    text: The text that was tokenized. For UnicodeTokenizer, this is
      NOT normalized to NFC (to preserve indices).
    tokens: A list of Token objects extracted from the text.
  """

  text: str
  tokens: list[Token] = dataclasses.field(default_factory=list)
````

TokenizedText 把原始字符串和 token 列表装在一起。切分过的字符串不是原文的替代品；所有坐标仍指向原文。

## 7.3 第一种切分器：用规则识别词

用正则表达式扫描时，finditer 可以同时拿到匹配内容和 span。源码有三个候选：字母串、数字串、相同标点的连续串。符号规则里的反向引用让 "!!" 合为一个 token，但 "!?" 分开。字母规则排除了数字和下划线，并不限于 ASCII。

源码出处：[langextract/core/tokenizer.py:165–177](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L165-L177)。

````python
class Tokenizer(abc.ABC):
  """Abstract base class for tokenizers."""

  @abc.abstractmethod
  def tokenize(self, text: str) -> TokenizedText:
    """Splits text into tokens.

    Args:
      text: The text to tokenize.

    Returns:
      A TokenizedText object.
    """
````

Tokenizer 是抽象基类；它的 tokenize 抽象方法规定输入字符串、输出 TokenizedText。调用者不应依赖具体切分算法，这使我们可以给中文换另一把尺子。

源码出处：[langextract/core/tokenizer.py:180–227](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L180-L227)。

````python
class RegexTokenizer(Tokenizer):
  """Regex-based tokenizer (default).

  The RegexTokenizer is faster than UnicodeTokenizer for English text because it
  skips involved Unicode handling.
  """

  @debug_utils.debug_log_calls
  def tokenize(self, text: str) -> TokenizedText:
    """Splits text into tokens (words, digits, or punctuation).

    Each token is annotated with its character position and type. Tokens
    following a newline or carriage return have `first_token_after_newline`
    set to True.

    Args:
      text: The text to tokenize.

    Returns:
      A TokenizedText object containing all extracted tokens.
    """
    tokenized = TokenizedText(text=text)
    previous_end = 0
    for token_index, match in enumerate(_TOKEN_PATTERN.finditer(text)):
      start_pos, end_pos = match.span()
      matched_text = match.group()
      token = Token(
          index=token_index,
          char_interval=CharInterval(start_pos=start_pos, end_pos=end_pos),
          token_type=TokenType.WORD,
          first_token_after_newline=False,
      )
      if token_index > 0:
        # Optimization: Check gap without slicing.
        has_newline = text.find("\n", previous_end, start_pos) != -1
        if not has_newline:
          has_newline = text.find("\r", previous_end, start_pos) != -1
        if has_newline:
          token.first_token_after_newline = True
      if regex.fullmatch(_DIGITS_PATTERN, matched_text):
        token.token_type = TokenType.NUMBER
      elif _WORD_PATTERN.fullmatch(matched_text):
        token.token_type = TokenType.WORD
      else:
        token.token_type = TokenType.PUNCTUATION
      tokenized.tokens.append(token)
      previous_end = end_pos
    return tokenized
````

RegexTokenizer 是默认实现。其 tokenize 逐个匹配生成 Token，先假设 WORD，再用数字和单词正则确定类型。它保留 previous_end，在两个匹配间用 find 检查 LF 或 CR，不必创建空白子串。注意第一个 token 不做这个换行检查，后续 token 即使间隔多个空格也能记住换行。

源码出处：[langextract/core/tokenizer.py:234–246](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L234-L246)。

````python
def tokenize(
    text: str, tokenizer: Tokenizer = _DEFAULT_TOKENIZER
) -> TokenizedText:
  """Splits text into tokens using the provided tokenizer (default: RegexTokenizer).

  Args:
    text: The text to tokenize.
    tokenizer: The tokenizer instance to use.

  Returns:
    A TokenizedText object.
  """
  return tokenizer.tokenize(text)
````

模块级 tokenize 把实际工作委托给传入的 Tokenizer，默认使用共享 RegexTokenizer 实例。这样旧代码调用 tokenize(text) 仍然可用，也不需要每次创建新实例。

## 7.4 一个眼睛看到的字，未必是一个 Python 字符

```python
import unicodedata

a = "é"
b = "e\u0301"
print(len(a), len(b))
assert unicodedata.normalize("NFC", b) == a
assert len(b) == 2
```

它们看起来几乎一样，却有不同的字符数。家庭 emoji 还可能由多个 emoji 和零宽连接符组成。直接按 Python 字符切开会破坏视觉单元。这样的视觉单元叫字素簇（grapheme cluster）。项目使用第三方 regex 的 \X 识别它，不是标准库 re 的功能。不能为了比较方便就先把整篇原文 NFC 化：那会改变证据坐标。

另一个困难是中文没有词间空格。如果用字母串规则处理「快递已到」，很可能整段被当作一个 token。对齐「快递」就变得困难。UnicodeTokenizer 对 CJK 以及若干不靠空格分词的文字按字素切开；它并不是用于语言学意义的中文分词器。

## 7.5 怎样知道字符属于哪一种文字

源码出处：[langextract/core/tokenizer.py:257–264](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L257-L264)。

````python
class Sentinel:
  """Sentinel class for unique object identification."""

  def __init__(self, name: str):
    self.name = name

  def __repr__(self) -> str:
    return f"<{self.name}>"
````

Sentinel 是只用来表达特殊身份的对象。__init__ 保存名字，__repr__ 返回带尖括号的调试表示。源码用两个不同实例表示禁止合组和未知文字系统；is 比较身份，不会把它们误认成普通文字系统名称。

源码出处：[langextract/core/tokenizer.py:312–318](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L312-L318)。

````python
@functools.lru_cache(maxsize=4096)
def _get_common_script_cached(c: str) -> str | Sentinel:
  """Determines script using regex, cached for performance."""
  match = _COMMON_SCRIPTS_PATTERN.match(c)
  if match:
    return match.lastgroup
  return _UNKNOWN_SCRIPT
````

_get_common_script_cached 利用带名称的正则分组识别拉丁、西里尔、希腊、阿拉伯、希伯来和天城文等常见文字系统，match.lastgroup 给出命中的名称，没命中返回未知哨兵。最多缓存 4096 项，重复字符免去正则查找。

源码出处：[langextract/core/tokenizer.py:273–279](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L273-L279)。

````python
def _get_script_fast(char: str) -> str | Sentinel:
  # Fast path for ASCII: Avoids regex and unicodedata lookups.
  if ord(char) < 128:
    return _LATIN_SCRIPT

  # Fallback to the robust regex method
  return _get_common_script_cached(char)
````

_get_script_fast 对 ASCII 直接返回 Latin，否则调用上述缓存函数。这个快速路径是在已知需要判断文字系统的地方使用，并不意味着所有 ASCII 符号都算字母。

源码出处：[langextract/core/tokenizer.py:282–291](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L282-L291)。

````python
def _classify_grapheme(g: str) -> TokenType:
  if not g:
    return TokenType.PUNCTUATION
  c = g[0]
  cat = unicodedata.category(c)
  if cat.startswith("L"):
    return TokenType.WORD
  if cat.startswith("N"):
    return TokenType.NUMBER
  return TokenType.PUNCTUATION
````

_classify_grapheme 判断字素首个码点的 Unicode 类别，以 L 开头是字母，以 N 开头是数值，其余归标点；空串也归标点。它只处理类别，不决定两个相邻字素是否应该合并。

## 7.6 用状态机把字素合成 token

想象手中拿着一个正在形成的单词。读到下一个字时，只有两个选择：继续写在这个单词后面，或者先把旧单词交给登记员，再开一个新单词。

```python
def runs(text):
    current = ""
    for ch in text:
        if current and ch.isdigit() != current[-1].isdigit():
            yield current
            current = ""
        current += ch
    if current:
        yield current

assert list(runs("ABC3821OK")) == ["ABC", "3821", "OK"]
```

这是教学简化状态机，不处理空白或 Unicode。真正的状态需要保存起点、类型、文字系统和上一个已提交 token 的结束位置。

源码出处：[langextract/core/tokenizer.py:321–467](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L321-L467)。

````python
class UnicodeTokenizer(Tokenizer):
  """Unicode-aware tokenizer for better non-English support.

  This tokenizer uses Unicode character properties (Unicode Standard Annex #29)
  via the `regex` library's `\\X` pattern to correctly handle grapheme clusters
  like Emojis and Hangul.


  Unlike some Unicode tokenizers, this class does NOT normalize text to NFC.
  This ensures that token indices exactly match the original input string.

  Note: Grapheme clustering makes this tokenizer slower than RegexTokenizer.
  """

  @debug_utils.debug_log_calls
  def tokenize(self, text: str) -> TokenizedText:
    """Splits text into tokens using Unicode properties.

    Args:
      text: The text to tokenize.

    Returns:
      A TokenizedText object.
    """
    tokens: list[Token] = []

    current_start = 0
    current_type = None
    current_script = None
    previous_end = 0

    for match in regex.finditer(r"\X", text):
      grapheme = match.group()
      start, _ = match.span()

      # 1. Handle Whitespace
      if grapheme.isspace():
        if current_type is not None:
          self._emit_token(
              tokens, text, current_start, start, current_type, previous_end
          )
          previous_end = start
          current_type = None
          current_script = None
        # Keep `previous_end` to detect newlines within the whitespace gap.
        continue

      g_type = _classify_grapheme(grapheme)

      # 2. Determine if we should merge with the current token
      should_merge = False
      if current_type is not None:
        if current_type == g_type:
          if current_type == TokenType.WORD:
            # Script Check
            first_char = grapheme[0]

            # Fast path: Explicit NO_GROUP (CJK/Thai) never merges.
            if current_script is _NO_GROUP_SCRIPT:
              should_merge = False

            # CJK and Non-Spaced scripts require fragmentation.
            elif _CJK_PATTERN.match(first_char) or _NON_SPACED_PATTERN.match(
                first_char
            ):
              should_merge = False

            else:
              g_script = _get_script_fast(first_char)
              # Safety: Do not merge distinct unknown scripts.
              if (
                  current_script == g_script
                  and current_script is not _UNKNOWN_SCRIPT
              ):
                should_merge = True

          elif current_type == TokenType.NUMBER:
            should_merge = True

          elif current_type == TokenType.PUNCTUATION:
            # Heuristic: Merge punctuation only if identical (e.g. "!!").
            last_grapheme = text[current_start:start]
            if last_grapheme == grapheme:
              should_merge = True
            elif len(last_grapheme) >= len(grapheme) and last_grapheme.endswith(
                grapheme
            ):
              should_merge = True

      # 3. State Transition
      if should_merge:
        # Extend current token
        pass
      else:
        # Flush previous token if exists
        if current_type is not None:
          self._emit_token(
              tokens, text, current_start, start, current_type, previous_end
          )
          previous_end = start

        # Start new token
        current_start = start
        current_type = g_type

        # Determine script for the new token
        if current_type == TokenType.WORD:
          c = grapheme[0]
          if _CJK_PATTERN.match(c) or _NON_SPACED_PATTERN.match(c):
            current_script = _NO_GROUP_SCRIPT
          else:
            current_script = _get_script_fast(c)
        else:
          current_script = None

    # 4. Flush final token
    if current_type is not None:
      self._emit_token(
          tokens, text, current_start, len(text), current_type, previous_end
      )

    return TokenizedText(text=text, tokens=tokens)

  def _emit_token(
      self,
      tokens: list[Token],
      text: str,
      start: int,
      end: int,
      token_type: TokenType,
      previous_end: int,
  ):
    """Helper to create and append a token."""
    token = Token(
        index=len(tokens),
        char_interval=CharInterval(start_pos=start, end_pos=end),
        token_type=token_type,
        first_token_after_newline=False,
    )

    # Check for newlines in the gap between the previous token and this one
    if start > previous_end:
      gap = text[previous_end:start]
      if "\n" in gap or "\r" in gap:
        token.first_token_after_newline = True

    tokens.append(token)
````

UnicodeTokenizer.tokenize 遍历字素簇：遇空白提交当前 token 并清空状态；相邻类型相同才可能合并。数字可连续合并，标点只有与之前相同才合并；普通单词要求相同且已知的 script，CJK 和非空格文字不合并。不能合并时先提交旧 token，再以当前字素开新 token。循环结束后还要提交最后一项，否则最后一个词会丢失。整个过程始终从原文取 span，没有规范化。

_emit_token 是这个状态机的出口：按列表长度填写 index，记录起止和类型，检查前一个 token 结束到当前起点之间是否有 CR/LF 并标记，然后 append。它与 RegexTokenizer 的差别之一是首 token 之前的换行也可能被记录。

## 7.7 从 token 区间找回原文

源码出处：[langextract/core/tokenizer.py:470–507](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L470-L507)。

````python
def tokens_text(
    tokenized_text: TokenizedText,
    token_interval: TokenInterval,
) -> str:
  """Reconstructs the substring of the original text spanning a given token interval.

  Args:
    tokenized_text: A TokenizedText object containing token data.
    token_interval: The interval specifying the range [start_index, end_index)
      of tokens.

  Returns:
    The exact substring of the original text corresponding to the token
    interval.

  Raises:
    InvalidTokenIntervalError: If the token_interval is invalid or out of range.
  """
  if token_interval.start_index == token_interval.end_index:
    return ""

  if (
      token_interval.start_index < 0
      or token_interval.end_index > len(tokenized_text.tokens)
      or token_interval.start_index > token_interval.end_index
  ):

    raise InvalidTokenIntervalError(
        f"Invalid token interval. start_index={token_interval.start_index}, "
        f"end_index={token_interval.end_index}, "
        f"total_tokens={len(tokenized_text.tokens)}."
    )

  start_token = tokenized_text.tokens[token_interval.start_index]
  end_token = tokenized_text.tokens[token_interval.end_index - 1]
  return tokenized_text.text[
      start_token.char_interval.start_pos : end_token.char_interval.end_pos
  ]
````

tokens_text 用第一个 token 起点及最后一个 token 终点切原文，因此保留中间空白。相等的起止索引先返回空字符串；其他负起点、超范围终点和反向区间会抛 InvalidTokenIntervalError。注意相等区间的判断在边界校验之前。

源码出处：[langextract/core/tokenizer.py:53–54](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L53-L54)。

````python
class BaseTokenizerError(exceptions.LangExtractError):
  """Base class for all tokenizer-related errors."""
````
源码出处：[langextract/core/tokenizer.py:57–58](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L57-L58)。

````python
class InvalidTokenIntervalError(BaseTokenizerError):
  """Error raised when a token interval is invalid or out of range."""
````
源码出处：[langextract/core/tokenizer.py:61–62](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L61-L62)。

````python
class SentenceRangeError(BaseTokenizerError):
  """Error raised when the start token index for a sentence is out of range."""
````

三种异常没有新增字段。BaseTokenizerError 是分词领域的异常基类，两个子类分别用于非法 token 区间和非法句子起点。调用方既可单独捕获错误，也可用基类统管分词错误。

## 7.8 句号一定是句尾吗？

如果一封信写着「Dr. Li arrived.」，照着句号切会把称呼 Dr. 拆开。换行也不一定是句尾，例如一段英文被编辑器自动折行。

源码出处：[langextract/core/tokenizer.py:510–546](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L510-L546)。

````python
def _is_end_of_sentence_token(
    text: str,
    tokens: Sequence[Token],
    current_idx: int,
    known_abbreviations: Set[str] = _KNOWN_ABBREVIATIONS,
) -> bool:
  """Checks if the punctuation token at `current_idx` ends a sentence.

  A token is considered a sentence terminator and is not part of a known
  abbreviation. Only searches the text corresponding to the current token.

  Args:
    text: The entire input text.
    tokens: The sequence of Token objects.
    current_idx: The current token index to check.
    known_abbreviations: Abbreviations that should not count as sentence enders
      (e.g., "Dr.").

  Returns:
    True if the token at `current_idx` ends a sentence, otherwise False.
  """
  current_token_text = text[
      tokens[current_idx]
      .char_interval.start_pos : tokens[current_idx]
      .char_interval.end_pos
  ]
  if _END_OF_SENTENCE_PATTERN.search(current_token_text):
    if current_idx > 0:
      prev_token_text = text[
          tokens[current_idx - 1]
          .char_interval.start_pos : tokens[current_idx - 1]
          .char_interval.end_pos
      ]
      if f"{prev_token_text}{current_token_text}" in known_abbreviations:
        return False
    return True
  return False
````

_is_end_of_sentence_token 取当前 token 原文，判断是否符合句末标点模式（包括中英文标点和印地语句号）；再把前一个 token 与它拼接，检查是否为 Dr.、Mr. 等已知缩写。匹配缩写就不结束。这是启发式规则，不是语法分析。

源码出处：[langextract/core/tokenizer.py:549–577](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L549-L577)。

````python
def _is_sentence_break_after_newline(
    text: str,
    tokens: Sequence[Token],
    current_idx: int,
) -> bool:
  """Checks if the next token starts uppercase and follows a newline.

  Args:
    text: The entire input text.
    tokens: The sequence of Token objects.
    current_idx: The current token index.

  Returns:
    True if a newline is found between current_idx and current_idx+1, and
    the next token (if any) begins with an uppercase character.
  """
  if current_idx + 1 >= len(tokens):
    return False

  next_token = tokens[current_idx + 1]

  if not next_token.first_token_after_newline:
    return False

  next_token_text = text[
      next_token.char_interval.start_pos : next_token.char_interval.end_pos
  ]
  # Assume break unless lowercase (covers numbers/quotes).
  return bool(next_token_text) and not next_token_text[0].islower()
````

_is_sentence_break_after_newline 检查下一个 token 是否存在、是否为换行后的第一个 token，并且首字符不是小写字母。数字、中文和引号同样可能满足最后一项，不能简单理解为只认大写。

源码出处：[langextract/core/tokenizer.py:580–647](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L580-L647)。

````python
def find_sentence_range(
    text: str,
    tokens: Sequence[Token],
    start_token_index: int,
    known_abbreviations: Set[str] = _KNOWN_ABBREVIATIONS,
) -> TokenInterval:
  """Finds a 'sentence' interval from a given start index.

  Sentence boundaries are defined by:
    - punctuation tokens in _END_OF_SENTENCE_PATTERN
    - newline breaks followed by an uppercase letter
    - not abbreviations in _KNOWN_ABBREVIATIONS (e.g., "Dr.")

  This favors terminating a sentence prematurely over missing a sentence
  boundary, and will terminate a sentence early if the first line ends with new
  line and the second line begins with a capital letter.

  Args:
    text: The text to analyze.
    tokens: The tokens that make up `text`.
      Note: For UnicodeTokenizer, use normalized text.
    start_token_index: The index of the token to start the sentence from.
    known_abbreviations: A set of strings that are known abbreviations and
      should not be treated as sentence boundaries.


  Returns:
    A TokenInterval representing the sentence range [start_token_index, end). If
    no sentence boundary is found, the end index will be the length of
    `tokens`.

  Raises:
    SentenceRangeError: If `start_token_index` is out of range.
  """
  if not tokens:
    return TokenInterval(0, 0)

  if start_token_index < 0 or start_token_index >= len(tokens):
    raise SentenceRangeError(
        f"start_token_index={start_token_index} out of range. "
        f"Total tokens: {len(tokens)}."
    )

  i = start_token_index
  while i < len(tokens):
    if tokens[i].token_type == TokenType.PUNCTUATION:
      if _is_end_of_sentence_token(text, tokens, i, known_abbreviations):
        end_index = i + 1
        # Consume any trailing closing punctuation (e.g. quotes, parens)
        while end_index < len(tokens):
          next_token_text = text[
              tokens[end_index]
              .char_interval.start_pos : tokens[end_index]
              .char_interval.end_pos
          ]
          if (
              tokens[end_index].token_type == TokenType.PUNCTUATION
              and next_token_text in _CLOSING_PUNCTUATION
          ):
            end_index += 1
          else:
            break
        return TokenInterval(start_index=start_token_index, end_index=end_index)
    if _is_sentence_break_after_newline(text, tokens, i):
      return TokenInterval(start_index=start_token_index, end_index=i + 1)
    i += 1

  return TokenInterval(start_index=start_token_index, end_index=len(tokens))
````

find_sentence_range 从指定 token 向右扫描，空 token 列表返回空区间，非空列表中的非法起点抛 SentenceRangeError。遇到句末标点时，继续吸收紧随的闭引号、括号等；否则尝试换行规则。两者都没命中就扫到文本末尾。结果仍是半开 token 区间，并不会往前找句首。

## 7.9 练习

1. 修改第一个例子，让它在同一条短信有两个取件码时列出两个坐标。
2. 在正式项目中比较两种 tokenizer 处理「包裹到了！」和组合 emoji 的结果。
3. 尝试 Dr. Li arrived. 以及没有句号的两行文字，观察启发式分句的边界。

下一章我们不再处理一条短信，而要把整个月的记录装进一个个适合模型处理的小信封。
