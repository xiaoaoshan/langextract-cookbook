# 第 17 章　让抽取结果可以被人检查

假设抽取完毕后，登记卡上写着「取件码 3821」。我们还需要核对它来自原短信哪里。打开浏览器，把抽取出来的那几个字涂上颜色，比对着数字坐标更容易理解。

## 17.1 从一个高亮区间开始

```python
import html

def highlight(text, start, end):
    return (html.escape(text[:start]) +
            "<mark>" + html.escape(text[start:end]) +
            "</mark>" + html.escape(text[end:]))

text = "Pickup 3821."
assert highlight(text, 7, 11) == "Pickup <mark>3821</mark>."
```

HTML 标签描述展示方式。原文始终是数据，所以先转义再插入标签。即使原文里有普通的数学不等号，页面也应当显示它而不是将其理解为标记。

```python
assert html.escape("3 < 5 & 7 > 2") == "3 &lt; 5 &amp; 7 &gt; 2"
```

和前面切块时的原则相同：不要改变原文坐标。先依据原文切片，然后将各片段转为展示所需的表示。

## 17.2 颜色是类别的属性

源码出处：[langextract/visualization.py:200–212](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L200-L212)。

````python
def _filter_valid_extractions(
    extractions: list[data.Extraction],
) -> list[data.Extraction]:
  """Filters extractions to only include those with valid char intervals."""
  return [
      e
      for e in extractions
      if (
          e.char_interval
          and e.char_interval.start_pos is not None
          and e.char_interval.end_pos is not None
      )
  ]
````

_filter_valid_extractions 只保留存在 char_interval 且两端点非 None 的卡。无法对齐的卡仍存在于原 AnnotatedDocument，但无法在原文画高亮，因此在可视化阶段不显示。

注意这里的 valid 只表示坐标存在，不保证 start、end 实际都在文本范围内，也没有检查 start<end。展示层后续会做部分检查；应用应确保数据自身坐标合法。

源码出处：[langextract/visualization.py:183–197](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L183-L197)。

````python
def _assign_colors(extractions: list[data.Extraction]) -> dict[str, str]:
  """Assigns a background colour to each extraction class.

  Args:
    extractions: list of extractions.

  Returns:
    Mapping from extraction_class to a hex colour string.
  """
  classes = {e.extraction_class for e in extractions if e.char_interval}
  color_map: dict[str, str] = {}
  palette_cycle = itertools.cycle(_PALETTE)
  for cls in sorted(classes):
    color_map[cls] = next(palette_cycle)
  return color_map
````

_assign_colors 收集存在字符区间的类别名，排序后用 itertools.cycle 循环调色板。相同类别同色，排序令结果不依赖卡片偶然顺序；类别多于调色板容量时颜色会复用。

## 17.3 同时高亮许多区间

我们不能简单对原字符串从左到右反复插入标签：插入第一个标签后，原坐标就不再适用。一个办法是把每个高亮转换为两个事件，开始与结束。

```python
text = "A B C"
spans = [(0, 3), (4, 5)]
events = []
for start, end in spans:
    events.extend([(start, 1, "<mark>"), (end, 0, "</mark>")])
cursor = 0
parts = []
for pos, _, tag in sorted(events):
    parts.append(text[cursor:pos])
    parts.append(tag)
    cursor = pos
parts.append(text[cursor:])
assert "".join(parts) == "<mark>A B</mark> <mark>C</mark>"
```

所有事件坐标始终相对原文。cursor 只是告诉我们下一段还没输出的文字从哪里开始。我们不对已插入标签后的字符串再做坐标计算。

源码出处：[langextract/visualization.py:215–219](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L215-L219)。

````python
class TagType(enum.Enum):
  """Enum for span boundary tag types."""

  START = 'start'
  END = 'end'
````

TagType 是 START 和 END 的枚举，把事件含义从裸字符串分离出来。

源码出处：[langextract/visualization.py:222–236](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L222-L236)。

````python
@dataclasses.dataclass(frozen=True)
class SpanPoint:
  """Represents a span boundary point for HTML generation.

  Attributes:
    position: Character position in the text.
    tag_type: Type of span boundary (START or END).
    span_idx: Index of the span for HTML data-idx attribute.
    extraction: The extraction data associated with this span.
  """

  position: int
  tag_type: TagType
  span_idx: int
  extraction: data.Extraction
````

SpanPoint 是冻结的 dataclass，每项保存字符位置、事件类型、高亮编号 span_idx 和关联 Extraction。后面可根据编号在浏览器中选中特定卡的标签。

源码出处：[langextract/visualization.py:239–315](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L239-L315)。

````python
def _build_highlighted_text(
    text: str,
    extractions: list[data.Extraction],
    color_map: dict[str, str],
) -> str:
  """Returns text with <span> highlights inserted, supporting nesting.

  Args:
    text: Original document text.
    extractions: List of extraction objects with char_intervals.
    color_map: Mapping of extraction_class to colour.
  """
  points = []
  span_lengths = {}
  for index, extraction in enumerate(extractions):
    if (
        not extraction.char_interval
        or extraction.char_interval.start_pos is None
        or extraction.char_interval.end_pos is None
        or extraction.char_interval.start_pos
        >= extraction.char_interval.end_pos
    ):
      continue

    start_pos = extraction.char_interval.start_pos
    end_pos = extraction.char_interval.end_pos
    points.append(SpanPoint(start_pos, TagType.START, index, extraction))
    points.append(SpanPoint(end_pos, TagType.END, index, extraction))
    span_lengths[index] = end_pos - start_pos

  def sort_key(point: SpanPoint):
    """Sorts span boundary points for proper HTML nesting.

    Sorts by position first, then handles ties at the same position to ensure
    proper HTML nesting. At the same position:
    1. End tags come before start tags (to close before opening)
    2. Among end tags: shorter spans close first
    3. Among start tags: longer spans open first

    Args:
      point: SpanPoint containing position, tag_type, span_idx, and extraction.

    Returns:
      Sort key tuple ensuring proper nesting order.
    """
    span_length = span_lengths.get(point.span_idx, 0)

    if point.tag_type == TagType.END:
      return (point.position, 0, span_length)
    else:  # point.tag_type == TagType.START
      return (point.position, 1, -span_length)

  points.sort(key=sort_key)

  html_parts: list[str] = []
  cursor = 0
  for point in points:
    if point.position > cursor:
      html_parts.append(html.escape(text[cursor : point.position]))

    if point.tag_type == TagType.START:
      colour = color_map.get(point.extraction.extraction_class, '#ffff8d')
      highlight_class = ' lx-current-highlight' if point.span_idx == 0 else ''

      span_html = (
          f'<span class="lx-highlight{highlight_class}"'
          f' data-idx="{point.span_idx}" style="background-color:{colour};">'
      )
      html_parts.append(span_html)
    else:  # point.tag_type == TagType.END
      html_parts.append('</span>')

    cursor = point.position

  if cursor < len(text):
    html_parts.append(html.escape(text[cursor:]))
  return ''.join(html_parts)
````

_build_highlighted_text 遍历卡片，跳过无坐标、端点缺失以及 start>=end 的项。为每张有效卡建立两个 SpanPoint，同时记录跨度长度。

内部 sort_key 先按位置排序。同位置的事件必须进一步排序：结束事件先于开始事件（相邻区间不应互相嵌套）；多个结束事件中短跨度先关；多个开始事件中长跨度先开。对应键分别是 (position,0,length) 和 (position,1,-length)。

| 区间 | 同位置规则 | 示例 |
|---|---|---|
| 相邻 | 先结束再开始 | [0,3)、[3,5) |
| 同起点嵌套 | 长的先开始 | [0,5)、[0,2) |
| 同终点嵌套 | 短的先结束 | [0,5)、[3,5) |

排序后按事件扫描，先输出 cursor 到当前事件位置的原始文字（html.escape），START 插入带背景色、data-idx 的 span 开标签，END 插入闭标签；第零项附当前高亮类名。循环结束再输出尾部文字。

这个方法适合不相交或正确嵌套的区间。但任意交叉区间（例如 [0,4)、[2,6)）不能只靠一个开始和结束标签表示成合法的独立嵌套元素，这是 HTML 树结构的限制。源码没有为每种交叉关系拆分跨度；展示重叠证据时应注意此限制。

## 17.4 在颜色旁显示卡片属性

源码出处：[langextract/visualization.py:318–332](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L318-L332)。

````python
def _build_legend_html(color_map: dict[str, str]) -> str:
  """Builds legend HTML showing extraction classes and their colors."""
  if not color_map:
    return ''

  legend_items = []
  for extraction_class, colour in color_map.items():
    legend_items.append(
        '<span class="lx-label"'
        f' style="background-color:{colour};">{html.escape(extraction_class)}</span>'
    )
  return (
      '<div class="lx-legend">Highlights Legend:'
      f' {" ".join(legend_items)}</div>'
  )
````

_build_legend_html 在空颜色映射时返回空串，否则遍历类别与颜色，创建带背景色的 span 图例，转义类别名称，把它们组合为 legend 容器。图例不参与对齐算法。

源码出处：[langextract/visualization.py:335–360](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L335-L360)。

````python
def _format_attributes(attributes: dict | None) -> str:
  """Formats attributes as a single-line string."""
  if not attributes:
    return '{}'

  valid_attrs = {
      key: value
      for key, value in attributes.items()
      if value not in (None, '', 'null')
  }

  if not valid_attrs:
    return '{}'

  attrs_parts = []
  for key, value in valid_attrs.items():
    # Clean up array formatting for better readability
    if isinstance(value, list):
      value_str = ', '.join(str(v) for v in value)
    else:
      value_str = str(value)
    attrs_parts.append(
        f'<span class="lx-attr-key">{html.escape(str(key))}</span>: <span'
        f' class="lx-attr-value">{html.escape(value_str)}</span>'
    )
  return '{' + ', '.join(attrs_parts) + '}'
````

_format_attributes 无属性时返回 {} 字符串，忽略 None、空字符串、字符串 null。列表值用逗号拼接其元素的文字，其余转 str。属性名和值经 html.escape，再放入不同 CSS 类的 span，方便给名称和取值设置独立样式。

这只是人类阅读格式，不是可逆 JSON 序列化：列表结构、空属性等可能在显示时被简化；持久化应使用第 16 章接口。

## 17.5 页面怎样知道应该显示哪一个卡片

源码出处：[langextract/visualization.py:363–418](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L363-L418)。

````python
def _prepare_extraction_data(
    text: str,
    extractions: list[data.Extraction],
    color_map: dict[str, str],
    context_chars: int = 150,
) -> list[dict]:
  """Prepares JavaScript data for extractions."""
  extraction_data = []
  for i, extraction in enumerate(extractions):
    # Assertions to inform pytype about the invariants guaranteed by _filter_valid_extractions
    assert (
        extraction.char_interval is not None
    ), 'char_interval must be non-None for valid extractions'
    assert (
        extraction.char_interval.start_pos is not None
    ), 'start_pos must be non-None for valid extractions'
    assert (
        extraction.char_interval.end_pos is not None
    ), 'end_pos must be non-None for valid extractions'

    start_pos = extraction.char_interval.start_pos
    end_pos = extraction.char_interval.end_pos

    context_start = max(0, start_pos - context_chars)
    context_end = min(len(text), end_pos + context_chars)

    before_text = text[context_start:start_pos]
    extraction_text = text[start_pos:end_pos]
    after_text = text[end_pos:context_end]

    colour = color_map.get(extraction.extraction_class, '#ffff8d')

    # Build attributes display
    attributes_html = (
        '<div><strong>class:</strong>'
        f' {html.escape(extraction.extraction_class)}</div>'
    )
    attributes_html += (
        '<div><strong>attributes:</strong>'
        f' {_format_attributes(extraction.attributes)}</div>'
    )

    extraction_data.append({
        'index': i,
        'class': extraction.extraction_class,
        'text': extraction.extraction_text,
        'color': colour,
        'startPos': start_pos,
        'endPos': end_pos,
        'beforeText': html.escape(before_text),
        'extractionText': html.escape(extraction_text),
        'afterText': html.escape(after_text),
        'attributesHtml': attributes_html,
    })

  return extraction_data
````

_prepare_extraction_data 将每张有效卡变成前端要用的字典。断言字符区间与端点已存在，计算前后 context_chars（默认 150）的上下文，夹在文本范围内；分别切出前文、证据原文和后文，经 HTML 转义。

输出同时保存卡片 index、class、原抽取文字 text、颜色、起止位置、转义后的上下文和已经组装的 attributesHtml。注意 extractionText 是按原文区间切出的证据，text 是模型报告中的文字；模糊匹配时两者可能不完全相等。

源码出处：[langextract/visualization.py:421–431](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L421-L431)。

````python
def _to_script_safe_json(value: object) -> str:
  """Returns `value` as JSON that is safe to embed in an inline <script>."""
  # Without these escapes, a string containing '</script>' or '<!--' could end
  # the script element early or change how the rest of the page is parsed.
  # The escapes decode back to the original characters.
  return (
      json.dumps(value)
      .replace('<', '\\u003c')
      .replace('>', '\\u003e')
      .replace('&', '\\u0026')
  )
````

_to_script_safe_json 先用 json.dumps 生成合法 JSON，再将 HTML 上下文敏感的三个字符替换成对应的 Unicode JSON 转义。浏览器读取后能恢复原文字值，使数据既可供 JavaScript 使用，又不会意外改变内联 script 的 HTML 解析边界。

这里体现了一个重要设计原则：不同上下文有不同的编码要求。HTML 正文用 HTML 转义，脚本内的 JSON 用适合脚本上下文的序列化；不能一概采用 str。

## 17.6 将数据交给浏览器动画

源码出处：[langextract/visualization.py:434–568](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L434-L568)。

````python
def _build_visualization_html(
    text: str,
    extractions: list[data.Extraction],
    color_map: dict[str, str],
    animation_speed: float = 1.0,
    show_legend: bool = True,
) -> str:
  """Builds the complete visualization HTML."""
  if not extractions:
    return (
        '<div class="lx-animated-wrapper"><p>No extractions to'
        ' animate.</p></div>'
    )

  # Sort extractions by position for proper HTML nesting.
  def _extraction_sort_key(extraction):
    """Sort by position, then by span length descending for proper nesting."""
    start = extraction.char_interval.start_pos
    end = extraction.char_interval.end_pos
    span_length = end - start
    return (start, -span_length)  # longer spans first

  sorted_extractions = sorted(extractions, key=_extraction_sort_key)

  highlighted_text = _build_highlighted_text(
      text, sorted_extractions, color_map
  )
  extraction_data = _prepare_extraction_data(
      text, sorted_extractions, color_map
  )
  legend_html = _build_legend_html(color_map) if show_legend else ''

  js_data = _to_script_safe_json(extraction_data)

  # Prepare pos_info_str safely for pytype for the f-string below
  first_extraction = extractions[0]
  assert (
      first_extraction.char_interval
      and first_extraction.char_interval.start_pos is not None
      and first_extraction.char_interval.end_pos is not None
  ), 'first extraction must have valid char_interval with start_pos and end_pos'
  pos_info_str = f'[{first_extraction.char_interval.start_pos}-{first_extraction.char_interval.end_pos}]'

  html_content = textwrap.dedent(f"""
    <div class="lx-animated-wrapper">
      <div class="lx-attributes-panel">
        {legend_html}
        <div id="attributesContainer"></div>
      </div>
      <div class="lx-text-window" id="textWindow">
        {highlighted_text}
      </div>
      <div class="lx-controls">
        <div class="lx-button-row">
          <button class="lx-control-btn" onclick="playPause()">▶️ Play</button>
          <button class="lx-control-btn" onclick="prevExtraction()">⏮ Previous</button>
          <button class="lx-control-btn" onclick="nextExtraction()">⏭ Next</button>
        </div>
        <div class="lx-progress-container">
          <input type="range" id="progressSlider" class="lx-progress-slider"
                 min="0" max="{len(extractions)-1}" value="0"
                 onchange="jumpToExtraction(this.value)">
        </div>
        <div class="lx-status-text">
          Entity <span id="entityInfo">1/{len(extractions)}</span> |
          Pos <span id="posInfo">{pos_info_str}</span>
        </div>
      </div>
    </div>

    <script>
      (function() {{
        const extractions = {js_data};
        let currentIndex = 0;
        let isPlaying = false;
        let animationInterval = null;
        let animationSpeed = {animation_speed};

        function updateDisplay() {{
          const extraction = extractions[currentIndex];
          if (!extraction) return;

          document.getElementById('attributesContainer').innerHTML = extraction.attributesHtml;
          document.getElementById('entityInfo').textContent = (currentIndex + 1) + '/' + extractions.length;
          document.getElementById('posInfo').textContent = '[' + extraction.startPos + '-' + extraction.endPos + ']';
          document.getElementById('progressSlider').value = currentIndex;

          const playBtn = document.querySelector('.lx-control-btn');
          if (playBtn) playBtn.textContent = isPlaying ? '⏸ Pause' : '▶️ Play';

          const prevHighlight = document.querySelector('.lx-text-window .lx-current-highlight');
          if (prevHighlight) prevHighlight.classList.remove('lx-current-highlight');
          const currentSpan = document.querySelector('.lx-text-window span[data-idx="' + currentIndex + '"]');
          if (currentSpan) {{
            currentSpan.classList.add('lx-current-highlight');
            currentSpan.scrollIntoView({{block: 'center', behavior: 'smooth'}});
          }}
        }}

        function nextExtraction() {{
          currentIndex = (currentIndex + 1) % extractions.length;
          updateDisplay();
        }}

        function prevExtraction() {{
          currentIndex = (currentIndex - 1 + extractions.length) % extractions.length;
          updateDisplay();
        }}

        function jumpToExtraction(index) {{
          currentIndex = parseInt(index);
          updateDisplay();
        }}

        function playPause() {{
          if (isPlaying) {{
            clearInterval(animationInterval);
            isPlaying = false;
          }} else {{
            animationInterval = setInterval(nextExtraction, animationSpeed * 1000);
            isPlaying = true;
          }}
          updateDisplay();
        }}

        window.playPause = playPause;
        window.nextExtraction = nextExtraction;
        window.prevExtraction = prevExtraction;
        window.jumpToExtraction = jumpToExtraction;

        updateDisplay();
      }})();
    </script>""")

  return html_content
````

_build_visualization_html 无卡片直接返回空提示；有卡片时先用内部 _extraction_sort_key 按字符起点、负跨度长度排序，保证同起点较长者优先。然后统一调用高亮生成、数据准备、可选图例构造，将数据转为脚本安全 JSON。

HTML 主体分三个区域：属性与图例面板、原文高亮窗口、播放及进度控制。JavaScript 在内部函数作用域中保留 currentIndex、isPlaying、animationInterval 与速度；updateDisplay 根据当前索引刷新属性、编号和坐标，移动当前高亮样式并滚动到证据。

nextExtraction 与 prevExtraction 用模运算循环移动索引；jumpToExtraction 从滑条读取索引；playPause 启动或取消定时器。最后把控制函数暴露给按钮事件并调用 updateDisplay 初始化。Python 返回的是完整 HTML 的字符串片段，不是启动一个长期运行的 HTTP 服务。

```python
def next_index(i, count):
    return (i + 1) % count

def prev_index(i, count):
    return (i - 1 + count) % count

assert next_index(2, 3) == 0
assert prev_index(0, 3) == 2
```

这里假设至少一张卡。正因如此，源码先处理了空列表，避免空动画的取模问题。

## 17.7 在 Notebook 和普通 Python 中展示

源码出处：[langextract/visualization.py:43–44](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L43-L44)。

````python
  def get_ipython():  # type: ignore[no-redef]
    return None
````

这是 IPython 导入失败时定义的备用函数，始终返回 None；HTML 也设为 None。可选展示依赖不存在时，模块仍能以普通字符串形式返回 HTML。

源码出处：[langextract/visualization.py:49–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L49-L60)。

````python
def _is_jupyter() -> bool:
  """Check if we're in a Jupyter/IPython environment that can display HTML."""
  try:
    if get_ipython is None:
      return False
    ip = get_ipython()
    if ip is None:
      return False
    # Simple check: if we're in IPython and NOT in a plain terminal
    return ip.__class__.__name__ != 'TerminalInteractiveShell'
  except Exception:
    return False
````

_is_jupyter 尝试调用 get_ipython。没有函数、没有 shell 或处于 TerminalInteractiveShell 时返回 False，其他 shell 视为可展示环境；异常也返回 False。这是环境启发式识别，不是精确的 Jupyter 协议检测。

源码出处：[langextract/visualization.py:571–646](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L571-L646)。

````python
def visualize(
    data_source: data.AnnotatedDocument | str | pathlib.Path,
    *,
    animation_speed: float = 1.0,
    show_legend: bool = True,
    gif_optimized: bool = True,
) -> HTML | str:
  """Visualises extraction data as animated highlighted HTML.

  Args:
    data_source: Either an AnnotatedDocument or path to a JSONL file.
    animation_speed: Animation speed in seconds between extractions.
    show_legend: If ``True``, appends a colour legend mapping extraction classes
      to colours.
    gif_optimized: If ``True``, applies GIF-optimized styling with larger fonts,
      better contrast, and improved dimensions for video capture.

  Returns:
    An :class:`IPython.display.HTML` object if IPython is available, otherwise
    the generated HTML string.
  """
  # Load document if it's a file path
  if isinstance(data_source, (str, pathlib.Path)):
    file_path = pathlib.Path(data_source)
    if not file_path.exists():
      raise FileNotFoundError(f'JSONL file not found: {file_path}')

    documents = list(io.load_annotated_documents_jsonl(file_path))
    if not documents:
      raise ValueError(f'No documents found in JSONL file: {file_path}')

    annotated_doc = documents[0]  # Use first document
  else:
    annotated_doc = data_source

  if not annotated_doc or annotated_doc.text is None:
    raise ValueError('annotated_doc must contain text to visualise.')

  if annotated_doc.extractions is None:
    raise ValueError('annotated_doc must contain extractions to visualise.')

  # Filter valid extractions - show ALL of them
  valid_extractions = _filter_valid_extractions(annotated_doc.extractions)

  if not valid_extractions:
    empty_html = (
        '<div class="lx-animated-wrapper"><p>No valid extractions to'
        ' animate.</p></div>'
    )
    full_html = f'{_HTML_CHARSET_META}{_VISUALIZATION_CSS}{empty_html}'
    if HTML is not None and _is_jupyter():
      return HTML(full_html)
    return full_html

  color_map = _assign_colors(valid_extractions)

  visualization_html = _build_visualization_html(
      annotated_doc.text,
      valid_extractions,
      color_map,
      animation_speed,
      show_legend,
  )

  full_html = f'{_HTML_CHARSET_META}{_VISUALIZATION_CSS}{visualization_html}'

  # Apply GIF optimizations if requested
  if gif_optimized:
    full_html = full_html.replace(
        'class="lx-animated-wrapper"',
        'class="lx-animated-wrapper lx-gif-optimized"',
    )

  if HTML is not None and _is_jupyter():
    return HTML(full_html)
  return full_html
````

visualize 接受 AnnotatedDocument 或 JSONL 路径。路径不存在报 FileNotFoundError，读取后为空报 ValueError；对于多文档文件，源码物化所有文档但只展示第一份。若没有原文或 extractions 为 None 也报错；空抽取列表则输出没有可高亮项的提示。

筛选卡片、分配颜色、构造 HTML，在最开头附 UTF-8 meta 和 CSS；gif_optimized 为真时给 wrapper 增加样式类。若存在 HTML 类并判断为 Notebook 环境，返回 IPython.display.HTML 对象；否则返回字符串。

它的职责是给已有抽取结果做展示，不重新调用模型、也不修改登记卡。

## 17.8 练习

1. 为高亮事件实验添加两个相邻区间，检查原文没有丢字。
2. 给同起点区间设置不同长度，手算 sort_key 的输出顺序。
3. 构造含中文和普通数学符号的原文，验证 HTML 转义能保留可见文字。
4. 手工写一张模糊对齐卡，比较模型文字与原文证据切片。

从短信到可检查的展示，运行功能已经完整。最后一章把注意力转到工程维护：如何处理错误、兼容旧代码、记录调试信息，并用测试证明我们的理解。
