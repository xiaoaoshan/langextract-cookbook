# 第 15 章　真正接入模型

上一章我们已经能根据模型名称挑选工作人员。现在要做的是让他们各自操作自己的设备，但交回的答卷都是同一种对象。上半章讲即时请求，下半章讲耗时更长的批任务。

## 15.1 为什么需要适配器

三个服务返回的答卷可能长得不同：

```python
from dataclasses import dataclass

@dataclass
class Output:
    score: float
    text: str

def adapt_a(reply):
    return Output(1.0, reply["text"])

def adapt_b(reply):
    return Output(1.0, reply["choices"][0]["message"]["content"])

assert adapt_a({"text": "3821"}) == adapt_b(
    {"choices": [{"message": {"content": "3821"}}]})
```

这就是适配器的工作：输入输出差异被藏在共同接口 infer 的背后。第 11 章的 Annotator 不应知道某个 SDK 的响应对象长什么样。这里使用 score=1.0 并不是模型认为准确率为 100%，只是这些实现提供了一个默认评分。

## 15.2 先理解即时请求与并发

如果三个电话分别需要 1、2、3 秒，一位工作人员逐个拨打要 6 秒，三位同时拨打理论上约 3 秒。请求完成的次序不一定等于发送次序。然而 Annotator 用 zip 将信封与结果配对，若调换顺序就会把取件码贴错文档。

```python
from concurrent.futures import ThreadPoolExecutor, as_completed

def fake_request(prompt):
    return prompt.upper()

prompts = ["a", "b", "c"]
results = [None] * len(prompts)
with ThreadPoolExecutor(max_workers=2) as pool:
    mapping = {pool.submit(fake_request, p): i for i, p in enumerate(prompts)}
    for future in as_completed(mapping):
        results[mapping[future]] = future.result()
assert results == ["A", "B", "C"]
```

future 是一张领取结果的凭证，as_completed 逐个交回已完成的凭证。保存原序号就能恢复顺序。上下文管理器负责关闭线程池。本章两个在线服务的 infer 就用了这个模式；Ollama 当前实现则逐个请求。

## 15.3 Gemini：先建立能够工作的客户端

数字配置也需要类型检查：Python 中 bool 是 int 的子类，但让 max_retries=True 等于 1 容易掩盖配置错误。

源码出处：[langextract/providers/gemini.py:63–65](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L63-L65)。

````python
def _is_non_bool_integral(value: Any) -> bool:
  """Return True when `value` is an integer-like value, excluding bool."""
  return isinstance(value, numbers.Integral) and not isinstance(value, bool)
````
源码出处：[langextract/providers/gemini.py:68–70](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L68-L70)。

````python
def _is_non_bool_real(value: Any) -> bool:
  """Return True when `value` is a real number, excluding bool."""
  return isinstance(value, numbers.Real) and not isinstance(value, bool)
````

前一个用 numbers.Integral 检查整数类值并排除 bool，后一个用 numbers.Real 接受实数同样排除 bool。这两个函数让重试参数的意图更清楚。

源码出处：[langextract/providers/gemini.py:73–99](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L73-L99)。

````python
def _has_sdk_retry_options(http_options: Any) -> bool:
  """Return True if http_options enables SDK-level retries.

  Only reports True when SDK retries would *actually* execute: the google-genai
  SDK normalizes `HttpRetryOptions.attempts` of 0 or 1 to `stop_after_attempt(1)`,
  i.e. no retries, so those values do not stack with our provider loop.

  Accepts both HttpOptions (attribute access) and HttpOptionsDict (dict); the
  dict form validates through pydantic camelCase aliases, so both
  `retry_options` and `retryOptions` reach the same field.
  """
  if http_options is None:
    return False
  if isinstance(http_options, dict):
    retry_options = http_options.get('retry_options')
    if retry_options is None:
      retry_options = http_options.get('retryOptions')
  else:
    retry_options = getattr(http_options, 'retry_options', None)
  if retry_options is None:
    return False
  if isinstance(retry_options, dict):
    attempts = retry_options.get('attempts')
  else:
    attempts = getattr(retry_options, 'attempts', None)
  # attempts=None means SDK default (which is >1); 0 or 1 means no retries.
  return attempts is None or attempts > 1
````

_has_sdk_retry_options 接受 SDK 配置对象或字典，同时兼容 retry_options、retryOptions 字典键；尝试获取 attempts。None 配置无重试选项；已配置重试且 attempts 默认或大于 1 时返回 True，0/1 不会多次尝试。目的是避免 SDK 内部重试和外部重试循环相乘。

源码出处：[langextract/providers/gemini.py:245–383](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L245-L383)。

````python
  def __init__(
      self,
      model_id: str = _DEFAULT_MODEL_ID,
      api_key: str | None = None,
      vertexai: bool = False,
      credentials: Any | None = None,
      project: str | None = None,
      location: str | None = None,
      http_options: Any | None = None,
      gemini_schema: schemas.gemini.GeminiSchema | None = None,
      format_type: data.FormatType = data.FormatType.JSON,
      temperature: float = 0.0,
      max_workers: int = 10,
      fence_output: bool = False,
      *,
      max_retries: int = _DEFAULT_MAX_RETRIES,
      retry_delay: float = _DEFAULT_RETRY_DELAY,
      max_retry_delay: float = _DEFAULT_MAX_RETRY_DELAY,
      **kwargs,
  ) -> None:
    """Initialize the Gemini language model.

    Args:
      model_id: The Gemini model ID to use.
      api_key: API key for Gemini service.
      vertexai: Whether to use Vertex AI instead of API key authentication.
      credentials: Optional Google auth credentials for Vertex AI.
      project: Google Cloud project ID for Vertex AI.
      location: Vertex AI location (e.g., 'global', 'us-central1').
      http_options: Optional HTTP options for the client (e.g., for VPC endpoints).
      gemini_schema: Optional schema for structured output.
      format_type: Output format (JSON or YAML).
      temperature: Sampling temperature.
      max_workers: Maximum number of parallel API calls.
      fence_output: Whether to wrap output in markdown fences (ignored,
        Gemini handles this based on schema).
      max_retries: Maximum number of retry attempts for transient errors
        (503, 429, network errors). Set to 0 to disable retries.
      retry_delay: Initial delay in seconds before first retry.
        Subsequent delays increase exponentially.
      max_retry_delay: Maximum delay in seconds between retries.
      **kwargs: Additional Gemini API parameters. Only allowlisted keys are
        forwarded to the API, including generation settings
        (max_output_tokens, top_p, top_k), response schemas, tools, safety
        settings, stop sequences, candidate count, system instructions, and
        thinking_config. See https://ai.google.dev/api/generate-content for
        details.
    """
    try:
      # pylint: disable=import-outside-toplevel
      from google import genai
    except ImportError as e:
      raise exceptions.InferenceConfigError(
          'google-genai is required for Gemini. Install it with: pip install'
          ' google-genai'
      ) from e

    self.model_id = model_id
    self.api_key = api_key
    self.vertexai = vertexai
    self.credentials = credentials
    self.project = project
    self.location = location
    self.http_options = http_options
    self.gemini_schema = None
    self.format_type = format_type
    self.temperature = temperature
    self.max_workers = max_workers
    self.fence_output = fence_output
    for name, value, ok in (
        (
            'max_retries',
            max_retries,
            _is_non_bool_integral(max_retries) and max_retries >= 0,
        ),
        (
            'retry_delay',
            retry_delay,
            _is_non_bool_real(retry_delay) and retry_delay >= 0,
        ),
        (
            'max_retry_delay',
            max_retry_delay,
            _is_non_bool_real(max_retry_delay) and max_retry_delay > 0,
        ),
    ):
      if not ok:
        raise exceptions.InferenceConfigError(f'{name} invalid: {value}')
    self.max_retries = max_retries
    self.retry_delay = retry_delay
    self.max_retry_delay = max_retry_delay

    # Avoid stacking with SDK-level retries (HttpOptions.retry_options).
    if max_retries > 0 and _has_sdk_retry_options(http_options):
      raise exceptions.InferenceConfigError(
          'http_options.retry_options and max_retries>0 both configured; '
          'retries would stack. Set max_retries=0 or clear retry_options.'
      )

    # Extract batch config before we filter kwargs into _extra_kwargs
    batch_cfg_dict = kwargs.pop('batch', None)
    self._batch_cfg = gemini_batch.BatchConfig.from_dict(batch_cfg_dict)

    if not self.api_key and not self.vertexai:
      raise exceptions.InferenceConfigError(
          'Gemini models require either:\n  - An API key via api_key parameter'
          ' or LANGEXTRACT_API_KEY env var\n  - Vertex AI configuration with'
          ' vertexai=True, project, and location'
      )
    if self.vertexai and (not self.project or not self.location):
      raise exceptions.InferenceConfigError(
          'Vertex AI mode requires both project and location parameters'
      )

    if self.api_key and self.vertexai:
      logging.warning(
          'Both API key and Vertex AI configuration provided. '
          'API key will take precedence for authentication.'
      )

    self._client = genai.Client(
        api_key=self.api_key,
        vertexai=vertexai,
        credentials=credentials,
        project=project,
        location=location,
        http_options=http_options,
    )

    super().__init__(
        constraint=schema.Constraint(constraint_type=schema.ConstraintType.NONE)
    )
    self._extra_kwargs = {
        k: v for k, v in (kwargs or {}).items() if k in _API_CONFIG_KEYS
    }
    # Route through apply_schema so self._schema stays in sync and
    # apply_output_schema() can detect the pre-configured schema.
    if gemini_schema is not None:
      self.apply_schema(gemini_schema)
````

完整类源码：[GeminiLanguageModel:190–630](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L190-L630)。

GeminiLanguageModel 继承第 5 章的 BaseLanguageModel，由 router 装饰器注册。构造时导入 SDK，缺失依赖报配置错误；保存模型标识和连接配置，检查整数重试次数、非负初始等待和正最大等待。已有 SDK 重试时拒绝同时配置外层重试。batch 参数单独解析为 BatchConfig。

认证要求 API key 或 Vertex AI 模式（Vertex 必须提供项目及区域），同时给出时发警告，再依参数建立 genai.Client。调用父类初始化约束，筛选 _API_CONFIG_KEYS 中受支持的附加设置。构造器收到规格时调用 apply_schema 让父类与自身状态同步。

源码出处：[langextract/providers/gemini.py:217–224](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L217-L224)。

````python
  @classmethod
  def get_schema_class(cls) -> type[schema.BaseSchema] | None:
    """Return the GeminiSchema class for structured output support.

    Returns:
      The GeminiSchema class that supports strict schema constraints.
    """
    return schemas.gemini.GeminiSchema
````
源码出处：[langextract/providers/gemini.py:226–243](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L226-L243)。

````python
  def apply_schema(self, schema_instance: schema.BaseSchema | None) -> None:
    """Apply a schema instance to this provider.

    Args:
      schema_instance: The schema instance to apply, or None to clear.

    Raises:
      InferenceConfigError: If schema_instance belongs to another provider.
    """
    if schema_instance is not None and not isinstance(
        schema_instance, schemas.gemini.GeminiSchema
    ):
      raise exceptions.InferenceConfigError(
          'GeminiLanguageModel only accepts GeminiSchema instances; got '
          f'{type(schema_instance).__name__}.'
      )
    super().apply_schema(schema_instance)
    self.gemini_schema = schema_instance
````
源码出处：[langextract/providers/gemini.py:385–395](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L385-L395)。

````python
  def _validate_schema_config(self) -> None:
    """Validate that schema configuration is compatible with format type.

    Raises:
      InferenceConfigError: If gemini_schema is set but format_type is not JSON.
    """
    if self.gemini_schema and self.format_type != data.FormatType.JSON:
      raise exceptions.InferenceConfigError(
          'Gemini structured output only supports JSON format. '
          'Set format_type=JSON or use_schema_constraints=False.'
      )
````

get_schema_class 返回第 13 章介绍的 GeminiSchema 类型。apply_schema 仅接受该类或 None，调用父类保存通用 schema 再同步 gemini_schema 字段；其他供应商的规格不能混用。_validate_schema_config 在存在 Gemini 结构化规格时拒绝非 JSON 格式。父类统一能力和子类具体实现均需维护一致。

## 15.4 如果服务暂时忙不过来

请求失败有两种典型原因：填写的地址或参数错误（重复提交没有帮助），或者服务短暂繁忙（稍后可能恢复）。我们只对后者有限次数重试。

```python
def delays(initial, cap, retries):
    delay = initial
    for _ in range(retries):
        yield min(delay, cap)
        delay = min(2 * delay, cap)

assert list(delays(1, 5, 5)) == [1, 2, 4, 5, 5]
```

这叫指数退避。真正的实现再添加随机抖动，避免同一时刻多个请求一起重试；总尝试次数有界。

源码出处：[langextract/providers/gemini.py:397–431](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L397-L431)。

````python
  def _is_retryable_error(self, error: Exception) -> bool:
    """Return True if `error` is a transient failure worth retrying."""
    try:
      from google.genai import errors as genai_errors  # pylint: disable=import-outside-toplevel

      if isinstance(error, genai_errors.APIError):
        return error.code in _RETRYABLE_API_CODES
    except ImportError:
      pass

    # Check both httpx and httpx2: google-genai 3.x uses httpx2 while other
    # providers (e.g. openai) install httpx in the same environment.
    # LocalProtocolError / UnsupportedProtocol are client/config bugs.
    for module_name in _HTTPX_MODULE_NAMES:
      try:
        http_mod = importlib.import_module(module_name)
      except ImportError:
        continue
      if isinstance(
          error,
          (
              http_mod.TimeoutException,
              http_mod.NetworkError,
              http_mod.RemoteProtocolError,
              http_mod.ProxyError,
          ),
      ):
        return True

    # Specifically ConnectionError / TimeoutError. Bare OSError is excluded:
    # it also covers file/permission errors that won't resolve by retrying.
    if isinstance(error, (ConnectionError, TimeoutError)):
      return True

    return bool(_RETRYABLE_MESSAGE_RE.search(str(error)))
````

_is_retryable_error 先看 SDK APIError 的 code 是否在预设瞬时错误集合，再尝试 httpx/httpx2 的网络、超时、远端协议与代理错误类。标准 ConnectionError、TimeoutError 也支持；其余尝试匹配预设错误文本。不是所有 OSError 都应重试，因为文件或权限问题通常无法靠等待恢复。

源码出处：[langextract/providers/gemini.py:433–482](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L433-L482)。

````python
  def _process_single_prompt(
      self, prompt: str, config: dict
  ) -> core_types.ScoredOutput:
    """Run one Gemini request with per-chunk retries for transient failures."""
    delay = self.retry_delay
    for attempt in range(self.max_retries + 1):
      try:
        call_config = dict(config)
        for key, value in self._extra_kwargs.items():
          if key not in call_config and value is not None:
            call_config[key] = value

        if self.gemini_schema:
          self._validate_schema_config()
          for key, value in self.gemini_schema.to_provider_config().items():
            call_config.setdefault(key, value)

        # Generation-setting None markers suppress constructor fallbacks above
        # but must not reach the SDK.
        call_config = {
            key: value
            for key, value in call_config.items()
            if value is not None
        }
        response = self._client.models.generate_content(
            model=self.model_id, contents=prompt, config=call_config
        )
        return self._response_to_scored_output(response)

      except exceptions.InferenceRuntimeError:
        raise
      except Exception as e:
        if attempt < self.max_retries and self._is_retryable_error(e):
          # Cap after jitter so the named maximum applies to the real sleep.
          sleep_for = min(
              delay * random.uniform(0.5, 1.5), self.max_retry_delay
          )
          logging.info(
              'Retryable error on attempt %d/%d: %s. Retrying in %.1fs...',
              attempt + 1,
              self.max_retries + 1,
              e,
              sleep_for,
          )
          time.sleep(sleep_for)
          delay = min(delay * 2, self.max_retry_delay)
          continue
        raise exceptions.InferenceRuntimeError(
            f'Gemini API error: {e}', original=e, provider='Gemini'
        ) from e
````

单个请求先复制 config，用未出现的构造默认值补齐；如有 schema 则验证并 setdefault 合入其配置。最后过滤 None（None 可以先阻止默认值回填）。调用 SDK models.generate_content，转换响应。已被包装的 InferenceRuntimeError 直接抛出；其他异常满足可重试条件且次数未耗尽时，等待 min(delay×随机因子,上限)，再翻倍退避。否则包装原始异常为带 provider 信息的 InferenceRuntimeError。首次加 max_retries 次机会，不是无限重试。

## 15.5 缺少答卷不能当成空抽取

源码出处：[langextract/providers/gemini.py:115–167](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L115-L167)。

````python
def _no_text_diagnostic(
    response: genai_types.GenerateContentResponse, text: str | None
) -> str | None:
  """Diagnose missing output that the SDK reports without raising.

  Returning missing text as success can silently drop a blocked chunk.

  Returns:
    A provider diagnostic, or None for text and empty completions without a
    block or abnormal finish reason, which retain existing resolver handling.
  """
  if text:
    return None

  prompt_feedback = response.prompt_feedback
  block_reason = prompt_feedback.block_reason if prompt_feedback else None
  candidate = response.candidates[0] if response.candidates else None
  finish_reason = candidate.finish_reason if candidate else None
  if block_reason == genai_types.BlockedReason.BLOCKED_REASON_UNSPECIFIED:
    block_reason = None
  if finish_reason == genai_types.FinishReason.FINISH_REASON_UNSPECIFIED:
    finish_reason = None

  if (
      text == ''
      and block_reason is None
      and finish_reason in (None, genai_types.FinishReason.STOP)
  ):
    return None

  if block_reason:
    block_message = prompt_feedback.block_reason_message
    detail = f': {block_message}' if block_message else ''
    return (
        'Gemini blocked the prompt'
        f' (block_reason={block_reason.value}{detail}).'
    )
  if finish_reason and finish_reason != genai_types.FinishReason.STOP:
    hint = (
        _MAX_TOKENS_HINT
        if finish_reason == genai_types.FinishReason.MAX_TOKENS
        else ''
    )
    return (
        f'Gemini returned no text (finish_reason={finish_reason.value}).{hint}'
    )
  if candidate and candidate.content and candidate.content.parts:
    return (
        'Gemini returned only non-text parts (e.g. a function call or'
        ' thought-only output) where text output was expected.'
    )
  detail = f' (finish_reason={finish_reason.value})' if finish_reason else ''
  return f'Gemini returned no text content{detail}.'
````

_no_text_diagnostic 文本非空返回 None（无异常诊断）；否则读取提示反馈和首候选的结束原因，忽略 UNSPECIFIED 值。真正空串且无 block、正常 STOP 可保留给 resolver；其他情况分别描述阻止原因、异常结束（MAX_TOKENS 有预算提示）、只有非文本 parts 或缺少输出。这避免把服务拒绝或失败误作没有信息。

源码出处：[langextract/providers/gemini.py:484–505](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L484-L505)。

````python
  @staticmethod
  def _response_to_scored_output(
      response: genai_types.GenerateContentResponse,
  ) -> core_types.ScoredOutput:
    """Translate one Gemini SDK response into LangExtract output.

    Raises:
      InferenceRuntimeError: If the response has no usable text.
    """
    output_text = response.text
    diagnostic = _no_text_diagnostic(response, output_text)
    if diagnostic is not None:
      raise exceptions.InferenceRuntimeError(diagnostic, provider='Gemini')
    candidates = getattr(response, 'candidates', None)
    if (
        output_text
        and isinstance(candidates, (list, tuple))
        and candidates
        and candidates[0].finish_reason == genai_types.FinishReason.MAX_TOKENS
    ):
      logging.warning(_TRUNCATED_OUTPUT_WARNING)
    return core_types.ScoredOutput(score=1.0, output=output_text)
````

_response_to_scored_output 从 response.text 取得答卷，调用前述诊断，异常时抛 InferenceRuntimeError；非空文本若首候选达到 MAX_TOKENS 则警告可能被截断。返回 ScoredOutput(1.0, text)，不调用 resolver。

源码出处：[langextract/providers/gemini.py:507–630](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L507-L630)。

````python
  def infer(
      self, batch_prompts: Sequence[str], **kwargs
  ) -> Iterator[Sequence[core_types.ScoredOutput]]:
    """Runs inference on a list of prompts via Gemini's API.

    Args:
      batch_prompts: A list of string prompts.
      **kwargs: Additional generation params (temperature, top_p, top_k, etc.)

    Yields:
      Lists of ScoredOutputs.
    """
    merged_kwargs = self.merge_kwargs(kwargs)

    config = {
        'temperature': merged_kwargs.get('temperature', self.temperature),
    }
    # Preserve None here as an explicit marker that clears a constructor value.
    for key in _GENERATION_CONFIG_KEYS:
      if key in merged_kwargs:
        config[key] = merged_kwargs[key]

    handled_keys = {'temperature', *_GENERATION_CONFIG_KEYS}
    for key, value in merged_kwargs.items():
      if (
          key not in handled_keys
          and key in _API_CONFIG_KEYS
          and value is not None
      ):
        config[key] = value

    # Use batch API if threshold met
    if self._batch_cfg and self._batch_cfg.enabled:
      if len(batch_prompts) >= self._batch_cfg.threshold:
        try:
          if self.gemini_schema:
            self._validate_schema_config()
          schema_config = (
              self.gemini_schema.to_provider_config()
              if self.gemini_schema
              else None
          )
          # None markers implement clearing but must not reach the batch SDK.
          batch_config = {
              key: value for key, value in config.items() if value is not None
          }
          # Schema fields use the separate schema_config argument.
          batch_config.pop('response_mime_type', None)
          batch_config.pop('response_schema', None)
          batch_config.pop('response_json_schema', None)
          # Extract top-level fields that don't belong in generationConfig
          system_instruction = batch_config.pop('system_instruction', None)
          safety_settings = batch_config.pop('safety_settings', None)
          tools = batch_config.pop('tools', None)
          outputs = gemini_batch.infer_batch(
              client=self._client,
              model_id=self.model_id,
              prompts=batch_prompts,
              schema_config=schema_config,
              gen_config=batch_config,
              cfg=self._batch_cfg,
              system_instruction=system_instruction,
              safety_settings=safety_settings,
              tools=tools,
              project=self.project,
              location=self.location,
          )
        except exceptions.InferenceRuntimeError:
          raise
        except Exception as e:
          raise exceptions.InferenceRuntimeError(
              f'Gemini Batch API error: {e}', original=e
          ) from e

        for text in outputs:
          yield [core_types.ScoredOutput(score=1.0, output=text)]
        return
      else:
        logging.info(
            'Gemini batch mode enabled but prompt count (%d) is below the'
            ' threshold (%d); using real-time API. Submit at least %d prompts'
            ' to trigger batch mode.',
            len(batch_prompts),
            self._batch_cfg.threshold,
            self._batch_cfg.threshold,
        )

    # Use parallel processing for batches larger than 1
    if len(batch_prompts) > 1 and self.max_workers > 1:
      with concurrent.futures.ThreadPoolExecutor(
          max_workers=min(self.max_workers, len(batch_prompts))
      ) as executor:
        future_to_index = {
            executor.submit(
                self._process_single_prompt, prompt, config.copy()
            ): i
            for i, prompt in enumerate(batch_prompts)
        }

        results: list[core_types.ScoredOutput | None] = [None] * len(
            batch_prompts
        )
        for future in concurrent.futures.as_completed(future_to_index):
          index = future_to_index[future]
          try:
            results[index] = future.result()
          except exceptions.InferenceRuntimeError:
            raise
          except Exception as e:
            raise exceptions.InferenceRuntimeError(
                f'Parallel inference error: {str(e)}', original=e
            ) from e

        for result in results:
          if result is None:
            raise exceptions.InferenceRuntimeError(
                'Failed to process one or more prompts'
            )
          yield [result]
    else:
      # Sequential processing for single prompt or worker
      for prompt in batch_prompts:
        result = self._process_single_prompt(prompt, config.copy())
        yield [result]  # pylint: disable=duplicate-code
````

infer 合并调用时参数与构造默认值，只将受支持字段放入 config。生成设置中特别保留 None，用于清除构造值。启用批任务且提示数达到 threshold 时调用 gemini_batch.infer_batch：规格独立传递，去掉重复 schema 字段，并将 system_instruction、safety_settings、tools 从生成配置分开；逐项包装返回并结束。

否则在提示多于一个且 max_workers>1 时使用线程池。保存 future_to_index，并将完成结果放回原索引，所有结果准备就绪后按原序 yield。失败包装成推理异常。单提示或单 worker 直接顺序执行。并发不会改变一批 prompt 的对应关系。

## 15.6 OpenAI：同一个任务，另一种答卷结构

源码出处：[langextract/providers/openai.py:42–48](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L42-L48)。

````python
def _is_openai_api(base_url: str | None) -> bool:
  """Returns whether requests to base_url reach OpenAI's own API."""
  if not base_url:
    return True
  host = urllib.parse.urlsplit(base_url).hostname or ''
  # Regional endpoints such as eu.api.openai.com behave like the default.
  return host == 'api.openai.com' or host.endswith('.api.openai.com')
````

_is_openai_api 判断缺省或 OpenAI 官方及其子域 API 主机（urlsplit 提取 hostname），用于区分输出长度参数命名；它不是网络安全校验或访问控制。

源码出处：[langextract/providers/openai.py:120–185](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L120-L185)。

````python
  def __init__(
      self,
      model_id: str = 'gpt-4o-mini',
      api_key: str | None = None,
      base_url: str | None = None,
      organization: str | None = None,
      openai_schema: schemas.openai.OpenAISchema | None = None,
      format_type: data.FormatType = data.FormatType.JSON,
      temperature: float | None = None,
      max_workers: int = 10,
      **kwargs,
  ) -> None:
    """Initialize the OpenAI language model.

    Args:
      model_id: The OpenAI model ID to use (e.g., 'gpt-4o-mini', 'gpt-4o').
      api_key: API key for OpenAI service.
      base_url: Base URL for OpenAI service.
      organization: Optional OpenAI organization ID.
      openai_schema: Optional schema for structured output.
      format_type: Output format (JSON or YAML).
      temperature: Sampling temperature.
      max_workers: Maximum number of parallel API calls.
      **kwargs: Additional OpenAI Chat Completions parameters. Pass `batch` as
        True, a dict, or `openai_batch.BatchConfig` to enable OpenAI Batch API
        mode.
    """
    try:
      # pylint: disable=import-outside-toplevel
      import openai
    except ImportError as e:
      raise exceptions.InferenceConfigError(
          'OpenAI provider requires openai package. '
          'Install with: pip install langextract[openai]'
      ) from e

    # Constructor-provided schemas use BaseLanguageModel state when applied.
    super().__init__(
        constraint=schema.Constraint(constraint_type=schema.ConstraintType.NONE)
    )

    self.model_id = model_id
    self.api_key = api_key
    self.base_url = base_url
    self.organization = organization
    self.openai_schema = None
    self.format_type = format_type
    self.temperature = temperature
    self.max_workers = max_workers
    batch_cfg_dict = kwargs.pop('batch', None)
    self._batch_cfg = openai_batch.BatchConfig.from_dict(batch_cfg_dict)
    self._extra_kwargs = kwargs or {}

    if not self.api_key:
      raise exceptions.InferenceConfigError('API key not provided.')

    if openai_schema is not None:
      self.apply_schema(openai_schema)

    # Keep SDK initialization after schema validation so LangExtract reports
    # configuration errors before any client-side transport checks.
    self._client = openai.OpenAI(
        api_key=self.api_key,
        base_url=self.base_url,
        organization=self.organization,
    )
````

完整类源码：[OpenAILanguageModel:51–461](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L51-L461)。

OpenAILanguageModel 也是注册过的 BaseLanguageModel 子类。构造时按需 import openai，缺 SDK 报配置错误。初始化父类、保存模型名、地址、组织、格式、温度、worker 数，并从 kwargs 拆出 BatchConfig；没有 key 拒绝。若有规格先 apply_schema，再创建 SDK 客户端，使格式错误优先于传输检查。

源码出处：[langextract/providers/openai.py:77–80](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L77-L80)。

````python
  @classmethod
  def get_schema_class(cls) -> type[schema.BaseSchema] | None:
    """Return the OpenAISchema class for structured output support."""
    return schemas.openai.OpenAISchema
````
源码出处：[langextract/providers/openai.py:82–108](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L82-L108)。

````python
  def apply_schema(self, schema_instance: schema.BaseSchema | None) -> None:
    """Applies an OpenAI schema instance to this provider.

    Args:
      schema_instance: An OpenAISchema to enforce, or None to clear.

    Raises:
      InferenceConfigError: if schema_instance is a non-OpenAI BaseSchema
        subclass, or if applying an OpenAI schema would conflict with a
        non-JSON format_type.
    """
    if schema_instance is None:
      self.openai_schema = None
    elif isinstance(schema_instance, schemas.openai.OpenAISchema):
      if self.format_type != data.FormatType.JSON:
        raise exceptions.InferenceConfigError(
            schemas.openai.JSON_SCHEMA_FORMAT_ERROR
        )
      self.openai_schema = schema_instance
    else:
      raise exceptions.InferenceConfigError(
          'OpenAILanguageModel only accepts OpenAISchema instances; got '
          f'{type(schema_instance).__name__}. Use the matching provider '
          'for this schema or construct an OpenAISchema via '
          'OpenAISchema.from_examples.'
      )
    super().apply_schema(schema_instance)
````
源码出处：[langextract/providers/openai.py:110–118](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L110-L118)。

````python
  @property
  def requires_fence_output(self) -> bool:
    """OpenAI JSON mode returns raw JSON unless callers override fences."""
    if (
        self._fence_output_override is None
        and self.format_type == data.FormatType.JSON
    ):
      return False
    return super().requires_fence_output
````
源码出处：[langextract/providers/openai.py:187–192](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L187-L192)。

````python
  def _validate_schema_config(self) -> None:
    """Rejects schema settings the OpenAI API cannot honor."""
    if self.openai_schema and self.format_type != data.FormatType.JSON:
      raise exceptions.InferenceConfigError(
          schemas.openai.JSON_SCHEMA_FORMAT_ERROR
      )
````

get_schema_class 返回 OpenAISchema。apply_schema 支持 None 清除或 OpenAISchema 设置，在 JSON 之外禁止该规格，错误类型同理；再调用父类。requires_fence_output 属性在无显式覆盖且 JSON 模式时直接 False，否则走父类默认。_validate_schema_config 检查 OpenAI 规格使用 JSON，供请求构造时再次校验。

源码出处：[langextract/providers/openai.py:194–263](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L194-L263)。

````python
  def _build_chat_completions_params(self, prompt: str, config: dict) -> dict:
    """Build Chat Completions request parameters for one prompt."""
    normalized_config = config.copy()

    system_message = ''
    if self.format_type == data.FormatType.JSON:
      system_message = (
          'You are a helpful assistant that responds in JSON format.'
      )
    elif self.format_type == data.FormatType.YAML:
      system_message = (
          'You are a helpful assistant that responds in YAML format.'
      )

    messages = [{'role': 'user', 'content': prompt}]
    if system_message:
      messages.insert(0, {'role': 'system', 'content': system_message})

    api_params: dict[str, Any] = {
        'model': self.model_id,
        'messages': messages,
        'n': 1,
    }

    temp = normalized_config.get('temperature', self.temperature)
    if temp is not None:
      api_params['temperature'] = temp

    runtime_response_format = normalized_config.get('response_format')
    if self.openai_schema and runtime_response_format is None:
      self._validate_schema_config()
      api_params['response_format'] = self.openai_schema.response_format
    elif runtime_response_format is not None:
      if self.openai_schema:
        # Advanced callers may deliberately override response_format at
        # runtime; warn because that bypasses the configured schema.
        warnings.warn(
            'openai_schema is set but a runtime response_format kwarg '
            'was provided; the schema is bypassed for this call.',
            UserWarning,
            stacklevel=3,
        )
      api_params['response_format'] = runtime_response_format
    elif self.format_type == data.FormatType.JSON:
      api_params['response_format'] = {'type': 'json_object'}

    if (v := normalized_config.get('max_output_tokens')) is not None:
      # OpenAI reasoning models reject the deprecated max_tokens, but some
      # OpenAI-compatible servers only accept it. Check OPENAI_BASE_URL too
      # because the SDK falls back to it when base_url is unset.
      base_url = self.base_url or os.environ.get('OPENAI_BASE_URL')
      if _is_openai_api(base_url):
        api_params['max_completion_tokens'] = v
      else:
        api_params['max_tokens'] = v
    if (v := normalized_config.get('top_p')) is not None:
      api_params['top_p'] = v
    for key in [
        'frequency_penalty',
        'presence_penalty',
        'seed',
        'stop',
        'logprobs',
        'top_logprobs',
        'reasoning_effort',
    ]:
      if (v := normalized_config.get(key)) is not None:
        api_params[key] = v

    return api_params
````

_build_chat_completions_params 复制 config，按 JSON/YAML 添加系统格式说明，将 prompt 作为 user 消息，形成 model、messages、n=1。温度取运行时或默认；有 schema 且无 runtime response_format 使用 schema.response_format，有运行时格式时显式覆盖并在已有 schema 时警告，普通 JSON 模式则设 json_object。

公共 max_output_tokens 映射为官方 max_completion_tokens 或兼容服务器 max_tokens；其他只转发列出的采样、停止、惩罚、日志概率等设置。请求参数转换是供应商适配器的主要职责。

源码出处：[langextract/providers/openai.py:265–281](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L265-L281)。

````python
  def _process_single_prompt(
      self, prompt: str, config: dict
  ) -> core_types.ScoredOutput:
    """Sends one prompt while preserving provider-specific error types."""
    try:
      api_params = self._build_chat_completions_params(prompt, config)
      response = self._client.chat.completions.create(**api_params)
      return self._response_to_scored_output(response)

    except exceptions.InferenceConfigError:
      raise
    except exceptions.InferenceRuntimeError:
      raise
    except Exception as e:
      raise exceptions.InferenceRuntimeError(
          f'OpenAI API error: {str(e)}', original=e, provider='OpenAI'
      ) from e
````

_process_single_prompt 构造参数，调用 chat.completions.create，再转换响应；已有配置和运行时异常保持原类型，其他异常包装并附原始异常、供应商名称。

源码出处：[langextract/providers/openai.py:283–331](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L283-L331)。

````python
  @staticmethod
  def _response_to_scored_output(
      response: chat.ChatCompletion,
  ) -> core_types.ScoredOutput:
    """Translate one OpenAI SDK response into LangExtract output.

    Raises:
      InferenceRuntimeError: If the response has no usable content.
    """
    if not response.choices:
      raise exceptions.InferenceRuntimeError(
          'OpenAI response contained no choices.', provider='OpenAI'
      )

    choice = response.choices[0]
    output_text = choice.message.content
    finish_reason = choice.finish_reason

    # Refusals may accompany content, so do not tie them to empty output.
    refusal = choice.message.refusal
    if refusal:
      raise exceptions.InferenceRuntimeError(
          f'OpenAI refused the request: {refusal}', provider='OpenAI'
      )

    if output_text is None or (
        not output_text and finish_reason not in (None, 'stop')
    ):
      detail = f' (finish_reason={finish_reason})' if finish_reason else ''
      hint = (
          ' The output token budget was exhausted before any text was'
          ' emitted; consider raising max_output_tokens (reasoning models'
          ' may consume the budget before any text is produced).'
          if finish_reason == 'length'
          else ''
      )
      raise exceptions.InferenceRuntimeError(
          f'OpenAI response contained no message content{detail}.{hint}',
          provider='OpenAI',
      )

    if finish_reason == 'length':
      logging.warning(
          'OpenAI stopped at the output token limit (finish_reason=length),'
          ' so the response is likely incomplete and may fail to parse.'
          ' Consider raising max_output_tokens or lowering max_char_buffer.'
      )

    return core_types.ScoredOutput(score=1.0, output=output_text)
````

_response_to_scored_output 取 choices[0]，没有候选报错；拒绝标记即使伴随文本也报错；None 文本或非正常结束伴随空文本报错。达到 length 时若还有文本会警告截断。成功返回 ScoredOutput，保证上游不用读取 SDK 内部字段。

源码出处：[langextract/providers/openai.py:354–461](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L354-L461)。

````python
  def infer(
      self, batch_prompts: Sequence[str], **kwargs
  ) -> Iterator[Sequence[core_types.ScoredOutput]]:
    """Runs inference on a list of prompts via OpenAI's API.

    Args:
      batch_prompts: A list of string prompts.
      **kwargs: Additional generation params (temperature, top_p, etc.)

    Yields:
      Lists of ScoredOutputs.
    """
    batch_size = kwargs.pop('batch_size', None)
    merged_kwargs = self.merge_kwargs(kwargs)

    config = {}

    temp = merged_kwargs.get('temperature', self.temperature)
    if temp is not None:
      config['temperature'] = temp
    if 'max_output_tokens' in merged_kwargs:
      config['max_output_tokens'] = merged_kwargs['max_output_tokens']
    if 'top_p' in merged_kwargs:
      config['top_p'] = merged_kwargs['top_p']

    for key in [
        'frequency_penalty',
        'presence_penalty',
        'seed',
        'stop',
        'logprobs',
        'top_logprobs',
        'reasoning_effort',
        'response_format',
    ]:
      if key in merged_kwargs:
        config[key] = merged_kwargs[key]

    if self._batch_cfg.enabled:
      if len(batch_prompts) >= self._batch_cfg.threshold:
        try:
          texts = openai_batch.infer_batch(
              client=self._client,
              model_id=self.model_id,
              prompts=batch_prompts,
              cfg=self._batch_cfg,
              request_builder=lambda prompt: (
                  self._build_chat_completions_params(prompt, config)
              ),
              batch_size=batch_size,
          )
        except exceptions.InferenceError:
          raise
        except Exception as e:
          raise exceptions.InferenceRuntimeError(
              f'OpenAI Batch API error: {str(e)}',
              original=e,
              provider='OpenAI',
          ) from e

        for text in texts:
          yield [core_types.ScoredOutput(score=1.0, output=text)]
        return

      logging.info(
          'OpenAI batch mode enabled but prompt count (%d) is below the'
          ' threshold (%d); using real-time API.',
          len(batch_prompts),
          self._batch_cfg.threshold,
      )

    if len(batch_prompts) > 1 and self.max_workers > 1:
      with concurrent.futures.ThreadPoolExecutor(
          max_workers=min(self.max_workers, len(batch_prompts))
      ) as executor:
        future_to_index = {
            executor.submit(
                self._process_single_prompt, prompt, config.copy()
            ): i
            for i, prompt in enumerate(batch_prompts)
        }

        results: list[core_types.ScoredOutput | None] = [None] * len(
            batch_prompts
        )
        for future in concurrent.futures.as_completed(future_to_index):
          index = future_to_index[future]
          try:
            results[index] = future.result()
          except exceptions.InferenceConfigError:
            raise
          except exceptions.InferenceRuntimeError:
            raise
          except Exception as e:
            raise exceptions.InferenceRuntimeError(
                f'Parallel inference error: {str(e)}', original=e
            ) from e

        for result in results:
          if result is None:
            raise exceptions.InferenceRuntimeError(
                'Failed to process one or more prompts'
            )
          yield [result]
    else:
      for prompt in batch_prompts:
        result = self._process_single_prompt(prompt, config.copy())
        yield [result]  # pylint: disable=duplicate-code
````

infer 先取批任务 batch_size，再合并 kwargs，提取支持的字段。若启用且达到阈值则把 request_builder（闭包，每次用相同 config 构造请求）交给 openai_batch.infer_batch，异常包装、结果按序 yield；否则记录回退即时路径。并发阶段同 Gemini：用线程池保存 future 原索引，按原序交付；配置或运行异常原样传递，其他失败包装。少量请求走顺序分支。

源码出处：[langextract/providers/openai.py:333–352](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L333-L352)。

````python
  def infer_batch(
      self, prompts: Sequence[str], batch_size: int = 32
  ) -> list[list[core_types.ScoredOutput]]:
    """Return materialized inference results for prompts.

    Args:
      prompts: Prompts to send to the OpenAI provider.
      batch_size: Maximum requests per OpenAI Batch API job when batch mode
        runs. Realtime fallback calls ignore this value.

    Returns:
      Scored outputs aligned with prompts.
    """
    if batch_size <= 0:
      raise exceptions.InferenceConfigError('batch_size must be > 0')

    results = []
    for output in self.infer(prompts, batch_size=batch_size):
      results.append(list(output))
    return results
````

infer_batch 是将 infer 的输出物化为双层列表的便利接口，batch_size 必须正数，只在启用实际 Batch API 时影响任务拆分；它不代表所有调用都使用服务器批任务。

## 15.7 Ollama：直接对本地 HTTP 服务说话

前两家使用 SDK。Ollama 这里直接使用 requests，将 Python 字典作为 HTTP JSON 请求体。对齐模块不需要知道这些差异。

源码出处：[langextract/providers/ollama.py:119–127](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L119-L127)。

````python
def _is_gpt_oss_model(model_id: str) -> bool:
  """Return whether an Ollama model ID is GPT-OSS."""
  normalized_model_id = model_id.lower()
  if normalized_model_id == _GPT_OSS_MODEL_PREFIX:
    return True
  prefix = f'{_GPT_OSS_MODEL_PREFIX}:'
  return normalized_model_id.startswith(prefix) and len(
      normalized_model_id
  ) > len(prefix)
````

_is_gpt_oss_model 小写模型 ID，识别 gpt-oss 本身或带非空冒号后缀的名称；特定模型的 JSON 输出协议需要不同请求路径。

源码出处：[langextract/providers/ollama.py:191–264](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L191-L264)。

````python
  def __init__(
      self,
      model_id: str,
      model_url: str = _OLLAMA_DEFAULT_MODEL_URL,
      base_url: str | None = None,  # Alias for model_url
      format_type: core_types.FormatType | None = None,
      structured_output_format: str | None = None,  # Deprecated
      constraint: schema.Constraint = schema.Constraint(),
      timeout: int | None = None,
      **kwargs,
  ) -> None:
    """Initialize the Ollama language model.

    Args:
      model_id: The Ollama model ID to use.
      model_url: URL for Ollama server (legacy parameter).
      base_url: Alternative parameter name for Ollama server URL.
      format_type: Output format (JSON or YAML). Defaults to JSON.
      structured_output_format: DEPRECATED - use format_type instead.
      constraint: Schema constraints.
      timeout: Request timeout in seconds. Defaults to 120.
      **kwargs: Additional parameters.
    """
    self._requests = requests

    # Handle deprecated structured_output_format parameter
    if structured_output_format is not None:
      warnings.warn(
          "'structured_output_format' is deprecated and will be removed in "
          "v2.0.0. Use 'format_type' instead.",
          FutureWarning,
          stacklevel=2,
      )
      if format_type is None:
        format_type = (
            core_types.FormatType.JSON
            if structured_output_format == 'json'
            else core_types.FormatType.YAML
        )

    fmt = kwargs.pop('format', None)
    if format_type is None and fmt in ('json', 'yaml'):
      format_type = (
          core_types.FormatType.JSON
          if fmt == 'json'
          else core_types.FormatType.YAML
      )

    if format_type is None:
      format_type = core_types.FormatType.JSON

    self._model = model_id
    self._model_url = base_url or model_url or _OLLAMA_DEFAULT_MODEL_URL
    self.format_type = format_type
    self._constraint = constraint

    self._api_key = kwargs.pop('api_key', None)
    self._auth_scheme = kwargs.pop('auth_scheme', 'Bearer')
    self._auth_header = kwargs.pop('auth_header', 'Authorization')

    if self._api_key:
      host = urlparse(self._model_url).hostname
      if host in ('localhost', '127.0.0.1', '::1'):
        warnings.warn(
            'API key provided for localhost Ollama instance. '
            "Native Ollama doesn't require authentication. "
            'This is typically only needed for proxied instances.',
            UserWarning,
        )

    super().__init__(constraint=constraint)
    if timeout is not None:
      kwargs['timeout'] = timeout
    self._extra_kwargs = kwargs or {}
````

完整类源码：[OllamaLanguageModel:142–658](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L142-L658)。

OllamaLanguageModel 是注册过的 BaseLanguageModel 子类。构造器处理弃用的 structured_output_format、兼容 format 参数，显式 format_type 优先，最终缺省 JSON；base_url 优先于旧 model_url。保存身份认证配置，若对本机提供认证信息则警告。调用父类并保留额外参数及可选 timeout。它并没有在构造时运行模型。

源码出处：[langextract/providers/ollama.py:171–178](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L171-L178)。

````python
  @classmethod
  def get_schema_class(cls) -> type[schema.BaseSchema] | None:
    """Return the FormatModeSchema class for JSON output support.

    Returns:
      The FormatModeSchema class that enables JSON mode (non-strict).
    """
    return schema.FormatModeSchema
````
源码出处：[langextract/providers/ollama.py:180–189](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L180-L189)。

````python
  def __repr__(self) -> str:
    """Return string representation with redacted API key."""
    api_key_display = '[REDACTED]' if self._api_key else None
    return (
        f'{self.__class__.__name__}('
        f'model={self._model!r}, '
        f'model_url={self._model_url!r}, '
        f'format_type={self.format_type!r}, '
        f'api_key={api_key_display})'
    )
````

get_schema_class 返回 FormatModeSchema，只保证输出格式而非字段结构。__repr__ 显示模型名称、地址和格式，对已设置的认证 key 使用 [REDACTED]，防止普通调试打印泄漏凭据。

源码出处：[langextract/providers/ollama.py:367–379](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L367-L379)。

````python
  def _request_headers(self) -> dict[str, str]:
    """Returns HTTP headers for Ollama requests."""
    headers = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
    }

    if self._api_key:
      if self._auth_scheme:
        headers[self._auth_header] = f'{self._auth_scheme} {self._api_key}'
      else:
        headers[self._auth_header] = self._api_key
    return headers
````

_request_headers 建立 JSON Content-Type 和 Accept，有配置认证时增加指定 header，支持带 scheme 的值。每次生成独立字典；不要记录完整认证值。

源码出处：[langextract/providers/ollama.py:381–438](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L381-L438)。

````python
  @staticmethod
  def _build_request_options(
      *,
      temperature: float | None = None,
      seed: int | None = None,
      top_k: int | None = None,
      top_p: float | None = None,
      max_output_tokens: int | None = None,
      keep_alive: int | None = None,
      num_threads: int | None = None,
      num_ctx: int | None = None,
      **kwargs,
  ) -> tuple[dict[str, Any], int]:
    """Returns Ollama options and the mirrored top-level keep_alive value."""
    options: dict[str, Any] = {}
    keep_alive_value = (
        keep_alive if keep_alive is not None else _DEFAULT_KEEP_ALIVE
    )
    options['keep_alive'] = keep_alive_value

    if seed is not None:
      options['seed'] = seed
    if temperature is not None:
      options['temperature'] = temperature
    else:
      options['temperature'] = _DEFAULT_TEMPERATURE
    if top_k is not None:
      options['top_k'] = top_k
    if top_p is not None:
      options['top_p'] = top_p
    if num_threads is not None:
      options['num_thread'] = num_threads
    if max_output_tokens is not None:
      options['num_predict'] = max_output_tokens
    if num_ctx is not None:
      options['num_ctx'] = num_ctx
    else:
      options['num_ctx'] = _DEFAULT_NUM_CTX

    reserved_top_level = {
        'model',
        'messages',
        'prompt',
        'system',
        'stop',
        'format',
        'stream',
        'raw',
    }

    for key, value in kwargs.items():
      if value is None:
        continue
      if key in reserved_top_level:
        continue
      if key not in options:
        options[key] = value
    return options, keep_alive_value
````

_build_request_options 把通用参数转换为 Ollama options，包含 keep_alive、温度、上下文窗口默认值；num_threads 映射 num_thread，max_output_tokens 映射 num_predict。保留显式的零值，过滤 None 和属于顶层而非 options 的参数。其他未知项可透传到 options，返回 options 及顶层 keep_alive 值。

源码出处：[langextract/providers/ollama.py:440–477](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L440-L477)。

````python
  def _post_ollama_json(
      self,
      api_url: str,
      payload: Mapping[str, Any],
      request_timeout: int,
      num_threads: int | None,
      model: str,
  ) -> Mapping[str, Any]:
    """Posts a non-streaming Ollama request and returns the JSON response."""
    try:
      response = self._requests.post(
          api_url,
          headers=self._request_headers(),
          json=payload,
          timeout=request_timeout,
      )
    except self._requests.exceptions.RequestException as e:
      if isinstance(e, self._requests.exceptions.ReadTimeout):
        msg = (
            f'Ollama Model timed out (timeout={request_timeout},'
            f' num_threads={num_threads})'
        )
        raise exceptions.InferenceRuntimeError(
            msg, original=e, provider='Ollama'
        ) from e
      raise exceptions.InferenceRuntimeError(
          f'Ollama request failed: {str(e)}', original=e, provider='Ollama'
      ) from e

    response.encoding = 'utf-8'
    if response.status_code == 200:
      return response.json()
    if response.status_code == 404:
      raise exceptions.InferenceConfigError(
          f"Can't find Ollama {model}. Try: ollama run {model}"
      )
    msg = f'Bad status code from Ollama: {response.status_code}'
    raise exceptions.InferenceRuntimeError(msg, provider='Ollama')
````

_post_ollama_json 是网络传输公共层，调用 requests.post，设置 header、JSON 体、超时。ReadTimeout 错误附超时时间和线程数，其他 RequestException 包装为运行时错误；响应编码为 UTF-8，200 解 JSON，404 视模型配置错误，其他状态视运行异常。两个请求格式共享这一层而不复制错误处理。

源码出处：[langextract/providers/ollama.py:543–658](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L543-L658)。

````python
  def _ollama_query(
      self,
      prompt: str,
      model: str | None = None,
      temperature: float | None = None,
      seed: int | None = None,
      top_k: int | None = None,
      top_p: float | None = None,
      max_output_tokens: int | None = None,
      structured_output_format: str | None = None,
      system: str = '',
      raw: bool = False,
      model_url: str | None = None,
      timeout: int | None = None,
      keep_alive: int | None = None,
      think: bool | None = None,
      num_threads: int | None = None,
      num_ctx: int | None = None,
      stop: str | list[str] | None = None,
      **kwargs,
  ) -> Mapping[str, Any]:
    """Sends a prompt to an Ollama model and returns the generated response.

    Note: This is a low-level method. Constructor timeout is only used when
    calling through infer(). Direct calls use the timeout parameter here.

    This function makes an HTTP POST request to the `/api/generate` endpoint of
    an Ollama server. It can optionally load the specified model first, generate
    a response (with or without streaming), then return a parsed JSON response.

    Args:
      prompt: The text prompt to send to the model.
      model: The name of the model to use. Defaults to self._model.
      temperature: Sampling temperature. Higher values produce more diverse
        output.
      seed: Seed for reproducible generation. If None, random seed is used.
      top_k: The top-K parameter for sampling.
      top_p: The top-P (nucleus) sampling parameter.
      max_output_tokens: Maximum tokens to generate. If None, the model's
        default is used.
      structured_output_format: If set to "json" or a JSON schema dict, requests
        structured outputs from the model. See Ollama documentation for details.
      system: A system prompt to override any system-level instructions.
      raw: If True, bypasses any internal prompt templating; you provide the
        entire raw prompt.
      model_url: The base URL for the Ollama server. Defaults to self._model_url.
      timeout: Timeout (in seconds) for the HTTP request. Defaults to 120.
      keep_alive: How long (in seconds) the model remains loaded after
        generation completes.
      think: Whether Ollama should return a separate reasoning trace for
        thinking models.
      num_threads: Number of CPU threads to use. If None, Ollama uses a default
        heuristic.
      num_ctx: Number of context tokens allowed. If None, uses model's default
        or config.
      stop: Stop sequences to halt generation. Can be a string or list of strings.
      **kwargs: Additional parameters passed through.

    Returns:
      A mapping (dictionary-like) containing the server's JSON response. For
      non-streaming calls, the `"response"` key contains the final generated
      text. Thinking models may also return a separate `"thinking"` key with
      reasoning text.

    Raises:
      InferenceConfigError: If the server returns a 404 (model not found).
      InferenceRuntimeError: For any other HTTP errors, timeouts, or request
        exceptions.
    """
    model = model or self._model
    model_url = model_url or self._model_url
    if structured_output_format is None and self.format_type is not None:
      structured_output_format = (
          'json' if self.format_type == core_types.FormatType.JSON else 'yaml'
      )

    options, keep_alive_value = self._build_request_options(
        temperature=temperature,
        seed=seed,
        top_k=top_k,
        top_p=top_p,
        max_output_tokens=max_output_tokens,
        keep_alive=keep_alive,
        num_threads=num_threads,
        num_ctx=num_ctx,
        **kwargs,
    )

    api_url = urljoin(
        model_url if model_url.endswith('/') else model_url + '/',
        'api/generate',
    )

    payload: dict[str, Any] = {
        'model': model,
        'prompt': prompt,
        'system': system,
        'stream': False,
        'raw': raw,
        'options': options,
    }
    payload['keep_alive'] = keep_alive_value

    if structured_output_format is not None:
      payload['format'] = structured_output_format

    if think is not None:
      payload['think'] = think

    if stop is not None:
      payload['stop'] = stop

    request_timeout = timeout if timeout is not None else _DEFAULT_TIMEOUT
    return self._post_ollama_json(
        api_url, payload, request_timeout, num_threads, model
    )
````

_ollama_query 选择模型和地址，必要时从 format_type 推导结构格式，构造 options，向 api/generate 组织 prompt、system、raw、stream=False 等字段并附可选的格式、think、stop 与 keep_alive 和超时，再调用公共 POST 方法。低层直接调用的 timeout 由当前参数决定，通常通过 infer 才自动合入构造选项。

源码出处：[langextract/providers/ollama.py:479–541](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L479-L541)。

````python
  def _ollama_gpt_oss_chat_query(
      self,
      prompt: str,
      model: str | None = None,
      temperature: float | None = None,
      seed: int | None = None,
      top_k: int | None = None,
      top_p: float | None = None,
      max_output_tokens: int | None = None,
      system: str = '',
      model_url: str | None = None,
      timeout: int | None = None,
      keep_alive: int | None = None,
      think: bool | None = None,
      num_threads: int | None = None,
      num_ctx: int | None = None,
      stop: str | list[str] | None = None,
      **kwargs,
  ) -> Mapping[str, Any]:
    """Sends a GPT-OSS JSON prompt through Ollama's chat endpoint."""
    model = model or self._model
    model_url = model_url or self._model_url

    options, keep_alive_value = self._build_request_options(
        temperature=temperature,
        seed=seed,
        top_k=top_k,
        top_p=top_p,
        max_output_tokens=max_output_tokens,
        keep_alive=keep_alive,
        num_threads=num_threads,
        num_ctx=num_ctx,
        **kwargs,
    )

    api_url = urljoin(
        model_url if model_url.endswith('/') else model_url + '/',
        'api/chat',
    )
    payload: dict[str, Any] = {
        'model': model,
        'messages': [
            {
                'role': 'system',
                'content': system or _GPT_OSS_JSON_SYSTEM_INSTRUCTION,
            },
            {'role': 'user', 'content': prompt},
        ],
        'stream': False,
        'options': options,
    }
    payload['keep_alive'] = keep_alive_value

    if think is not None:
      payload['think'] = think

    if stop is not None:
      payload['stop'] = stop

    request_timeout = timeout if timeout is not None else _DEFAULT_TIMEOUT
    return self._post_ollama_json(
        api_url, payload, request_timeout, num_threads, model
    )
````

_ollama_gpt_oss_chat_query 为 GPT-OSS 的 JSON 任务准备 api/chat 请求，用 system 和 user messages 构成对话，缺系统说明时使用默认 JSON 说明，不走普通 generate 的原生 JSON 模式。保留非流式、options、keep_alive、可选 think/stop 与超时，最终仍使用 _post_ollama_json。

源码出处：[langextract/providers/ollama.py:321–339](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L321-L339)。

````python
  @staticmethod
  def _extract_response_text(response: Mapping[str, Any]) -> str:
    """Returns final generated text from an Ollama generate response."""
    output = response.get('response')
    if output:
      return output

    if response.get('thinking'):
      raise exceptions.InferenceRuntimeError(
          'Ollama returned an empty response with a thinking trace. The '
          'thinking field contains reasoning, not final output. Ensure Ollama '
          'extraction requests use think=False, which is LangExtract default.',
          provider='Ollama',
      )
    raise exceptions.InferenceRuntimeError(
        "Ollama response did not include generated text in the 'response' "
        'field.',
        provider='Ollama',
    )
````
源码出处：[langextract/providers/ollama.py:341–365](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L341-L365)。

````python
  @staticmethod
  def _extract_chat_response_text(response: Mapping[str, Any]) -> str:
    """Returns final generated text from an Ollama chat response."""
    message = response.get('message')
    thinking = response.get('thinking')
    if isinstance(message, Mapping):
      output = message.get('content')
      if output:
        return output
      thinking = thinking or message.get('thinking')

    if thinking:
      raise exceptions.InferenceRuntimeError(
          'Ollama returned an empty chat response with only a reasoning '
          'trace. This usually happens when the model returned only '
          "reasoning tokens, such as when 'think=True' is passed to a "
          'reasoning model. LangExtract defaults to think=False so models '
          'emit final JSON instead.',
          provider='Ollama',
      )
    raise exceptions.InferenceRuntimeError(
        'Ollama chat response did not include generated text in the '
        "'message.content' field.",
        provider='Ollama',
    )
````

generate 响应读取 response 字段；chat 响应从 message 映射的 content 获取。两者都不将 thinking 推理记录冒充最终答卷，若只有推理或缺文本抛专门诊断。这与前两家响应转换遵循同样原则：没有最终答卷不可静默当成功。

源码出处：[langextract/providers/ollama.py:266–319](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L266-L319)。

````python
  def infer(
      self, batch_prompts: Sequence[str], **kwargs
  ) -> Iterator[Sequence[core_types.ScoredOutput]]:
    """Runs inference on a list of prompts via Ollama's API.

    Args:
      batch_prompts: A list of string prompts.
      **kwargs: Additional generation params.

    Yields:
      Lists of ScoredOutputs.
    """
    combined_kwargs = dict(self.merge_kwargs(kwargs))
    # LangExtract's concurrency setting, not an Ollama option; this provider
    # sends prompts one at a time.
    combined_kwargs.pop('max_workers', None)
    # LangExtract consumes final structured output, not Ollama reasoning traces.
    combined_kwargs.setdefault('think', False)

    structured_output_format = (
        'json' if self.format_type == core_types.FormatType.JSON else 'yaml'
    )
    # Keep YAML on the existing generate path; issue #116 is JSON-only.
    use_gpt_oss_chat = (
        _is_gpt_oss_model(self._model)
        and self.format_type == core_types.FormatType.JSON
    )

    for prompt in batch_prompts:
      try:
        if use_gpt_oss_chat:
          response = self._ollama_gpt_oss_chat_query(
              prompt=prompt,
              model=self._model,
              model_url=self._model_url,
              **combined_kwargs,
          )
          output = self._extract_chat_response_text(response)
        else:
          response = self._ollama_query(
              prompt=prompt,
              model=self._model,
              structured_output_format=structured_output_format,
              model_url=self._model_url,
              **combined_kwargs,
          )
          output = self._extract_response_text(response)
        yield [core_types.ScoredOutput(score=1.0, output=output)]
      except exceptions.InferenceError:
        raise
      except Exception as e:
        raise exceptions.InferenceRuntimeError(
            f'Ollama API error: {str(e)}', original=e, provider='Ollama'
        ) from e
````

infer 合并参数，移除上游 max_workers（本实现逐个工作），缺省 think=False，确定 JSON/YAML 和 GPT-OSS 特殊 chat 路径。每个 prompt 分别调用相应查询、提取最终文本、包装 ScoredOutput 后 yield；已知 InferenceError 直接传递，其他异常统一包装。

## 15.8 三种适配器的共同契约

| 内容 | Gemini | OpenAI | Ollama |
|---|---|---|---|
| 传输 | genai SDK | OpenAI SDK | requests |
| 即时请求 | 顺序/并发 | 顺序/并发 | 顺序 |
| 规格 | GeminiSchema | OpenAISchema | FormatModeSchema |
| 批任务 | Vertex AI | OpenAI Batch API | 无 |
| 输出 | ScoredOutput | ScoredOutput | ScoredOutput |

这三种类都遵守上游按 prompt 顺序返回候选的约定。默认实时并发只是在客户端同时发出多个请求，并不等于服务器后台批任务。下一节专门研究这种区别。

## 15.9 练习

1. 为适配器实验增加第三种返回结构，确保输出对象与前两个相等。
2. 改变线程池完成顺序，证明不记录索引会造成答卷错位。
3. 分析构造器中的格式配置与运行时参数覆盖关系；使用假客户端练习请求构造，不发送实际模型请求。

继续阅读 [第 15 章下篇：后台批任务](15-batch.md)。
