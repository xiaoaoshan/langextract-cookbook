# 第 14 章　根据模型名称选择服务

快递站新来了三位工作人员，各自使用不同的扫描仪。顾客只填写「请用某型号扫描仪」，不应该被迫知道扫描仪对象怎样创建。我们希望程序自己选择供应商。

## 14.1 从 if/else 到工厂

```python
class A:
    def __init__(self, name):
        self.name = name

class B(A):
    pass

def create(name):
    if name.startswith("a-"):
        return A(name)
    if name.startswith("b-"):
        return B(name)
    raise ValueError(name)

assert isinstance(create("b-small"), B)
```

把对象构造集中在 create 中，这就是工厂的基本想法。但每有新供应商都修改 create 不方便，尤其别人想开发自己的插件时。我们把选择规则变成一张可注册的表。

```python
import re

registry = [(r"^a-", A, 10), (r"^b-", B, 10)]

def choose(name):
    for pattern, cls, priority in sorted(registry, key=lambda e: e[2], reverse=True):
        if re.search(pattern, name):
            return cls
    raise ValueError(name)

assert choose("b-small") is B
registry.append((r"^b-small$", A, 20))
assert choose("b-small") is A
```

工厂负责造对象，路由器负责决定应造哪种对象。把这两个问题分开，就能独立测试选择规则。

## 14.2 注册表中的一行是什么？

源码出处：[langextract/providers/router.py:40–46](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L40-L46)。

````python
@dataclasses.dataclass(frozen=True, slots=True)
class _Entry:
  """Registry entry for a provider."""

  patterns: tuple[re.Pattern[str], ...]
  loader: typing.Callable[[], type[base_model.BaseLanguageModel]]
  priority: int
````

_Entry 是冻结且有 slots 的数据对象：保存已经编译的正则模式元组、返回类的 loader 函数及 priority。它不保存实际模型实例，因此查询类和创建实例是两回事。

源码出处：[langextract/providers/router.py:55–83](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L55-L83)。

````python
def _add_entry(
    *,
    provider_id: str,
    patterns: tuple[re.Pattern[str], ...],
    loader: typing.Callable[[], type[base_model.BaseLanguageModel]],
    priority: int,
) -> None:
  """Add an entry to the registry with deduplication."""
  key = (provider_id, tuple(p.pattern for p in patterns), priority)
  if key in _entry_keys:
    logging.debug(
        "Skipping duplicate registration for %s with patterns %s at"
        " priority %d",
        provider_id,
        [p.pattern for p in patterns],
        priority,
    )
    return
  _entry_keys.add(key)
  _entries.append(_Entry(patterns=patterns, loader=loader, priority=priority))
  # A new entry can take over an ID or name whose lookup is already cached.
  resolve.cache_clear()
  resolve_provider.cache_clear()
  logging.debug(
      "Registered provider %s with patterns %s at priority %d",
      provider_id,
      [p.pattern for p in patterns],
      priority,
  )
````

_add_entry 按 provider_id、模式字符串元组和优先级去重，已有项只记日志并返回。新项写入集合及列表后清除两个查询缓存，这非常关键：否则刚注册的高优先级处理器可能永远被旧缓存挡住。

## 14.3 为什么不在程序启动时 import 所有模型？

假如我们只用本地模型，就不应要求安装所有在线服务的 SDK。Python import 会执行模块顶层代码，不能把它理解成单纯声明头文件。用一个字符串记下将来要导入谁。

源码出处：[langextract/providers/router.py:86–108](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L86-L108)。

````python
def register_lazy(
    *patterns: str | re.Pattern[str], target: str, priority: int = 0
) -> None:
  """Register a provider lazily using string import path.

  Args:
    *patterns: One or more regex patterns to match model IDs.
    target: Import path in format "module.path:ClassName".
    priority: Priority for resolution (higher wins on conflicts).
  """
  compiled = tuple(re.compile(p) if isinstance(p, str) else p for p in patterns)

  def _loader() -> type[base_model.BaseLanguageModel]:
    module_path, class_name = target.rsplit(":", 1)
    module = importlib.import_module(module_path)
    return getattr(module, class_name)

  _add_entry(
      provider_id=target,
      patterns=compiled,
      loader=_loader,
      priority=priority,
  )
````

register_lazy 接收若干正则（字符串则先编译）、target（module:Class）和优先级。内部 _loader 是闭包，保留 target；真正调用时 rsplit 得到模块和类名，通过 importlib.import_module 和 getattr 取得类。注册本身只调用 _add_entry，不执行 loader。

源码出处：[langextract/providers/router.py:111–138](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L111-L138)。

````python
def register(
    *patterns: str | re.Pattern[str], priority: int = 0
) -> typing.Callable[[type[TLanguageModel]], type[TLanguageModel]]:
  """Decorator to register a provider class directly.

  Args:
    *patterns: One or more regex patterns to match model IDs.
    priority: Priority for resolution (higher wins on conflicts).

  Returns:
    Decorator function that registers the class.
  """
  compiled = tuple(re.compile(p) if isinstance(p, str) else p for p in patterns)

  def _decorator(cls: type[TLanguageModel]) -> type[TLanguageModel]:
    def _loader() -> type[base_model.BaseLanguageModel]:
      return cls

    provider_id = f"{cls.__module__}:{cls.__name__}"
    _add_entry(
        provider_id=provider_id,
        patterns=compiled,
        loader=_loader,
        priority=priority,
    )
    return cls

  return _decorator
````

另一条路径 register 返回内部 _decorator，而不是直接注册某个未知类。_decorator 收到类时建立只返回 cls 的 _loader，按模块名和类名组合标识，登记后原样返回 cls。这就是三个阶段：传入规则、装饰类、以后按需查询。

```python
table = {}

def register(key):
    def decorator(cls):
        table[key] = cls
        return cls
    return decorator

@register("local")
class Worker:
    pass

assert table["local"] is Worker
```

## 14.4 找到正确的供应商

源码出处：[langextract/providers/router.py:141–169](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L141-L169)。

````python
@functools.lru_cache(maxsize=128)
def resolve(model_id: str) -> type[base_model.BaseLanguageModel]:
  """Resolve a model ID to a provider class.

  Args:
    model_id: The model identifier to resolve.

  Returns:
    The provider class that handles this model ID.

  Raises:
    InferenceConfigError: If no provider is registered for the model ID.
  """
  # Providers should be loaded by the caller (e.g., factory.create_model)
  # Router doesn't load providers to avoid circular dependencies

  sorted_entries = sorted(_entries, key=lambda e: e.priority, reverse=True)

  for entry in sorted_entries:
    if any(pattern.search(model_id) for pattern in entry.patterns):
      return entry.loader()

  available_patterns = [str(p.pattern) for e in _entries for p in e.patterns]
  raise exceptions.InferenceConfigError(
      f"No provider registered for model_id={model_id!r}. "
      f"Available patterns: {available_patterns}\n"
      "Tip: You can explicitly specify a provider using 'config' parameter "
      "with factory.ModelConfig and a provider class."
  )
````

resolve 先按 priority 降序排序，逐项检查任一 pattern.search(model_id)，找到第一项便调用 loader 返回类。LRU 缓存最多 128 个查询结果；同优先级保持原先顺序。没有匹配时抛 InferenceConfigError，并列出可用模式。这不是检查远端模型是否真正存在，正则只决定将请求交给谁。

源码出处：[langextract/providers/router.py:172–217](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L172-L217)。

````python
@functools.lru_cache(maxsize=128)
def resolve_provider(provider_name: str) -> type[base_model.BaseLanguageModel]:
  """Resolve a provider name to a provider class.

  This allows explicit provider selection by name or class name.

  Args:
    provider_name: The provider name (e.g., "gemini", "openai") or
      class name (e.g., "GeminiLanguageModel").

  Returns:
    The provider class.

  Raises:
    InferenceConfigError: If no provider matches the name.
  """
  # Providers should be loaded by the caller (e.g., factory.create_model)
  # Router doesn't load providers to avoid circular dependencies

  for entry in _entries:
    for pattern in entry.patterns:
      if pattern.pattern == f"^{re.escape(provider_name)}$":
        return entry.loader()

  for entry in _entries:
    try:
      provider_class = entry.loader()
      class_name = provider_class.__name__
      if provider_name.lower() in class_name.lower():
        return provider_class
    except (ImportError, AttributeError):
      continue

  try:
    pattern = re.compile(f"^{provider_name}$", re.IGNORECASE)
    for entry in _entries:
      for entry_pattern in entry.patterns:
        if pattern.pattern == entry_pattern.pattern:
          return entry.loader()
  except re.error:
    pass

  raise exceptions.InferenceConfigError(
      f"No provider found matching: {provider_name!r}. "
      "Available providers can be listed with list_providers()"
  )
````

resolve_provider 允许显式名称。先找精确锚定的转义名称模式，再依注册顺序加载类并检查大小写无关的类名子串（捕获 ImportError、AttributeError），最后尝试与另一种锚定正则字符串匹配。未找到抛配置异常。它与 resolve 的优先级排序不同，显式名称路径可能实际导入多个候选模块。

源码出处：[langextract/providers/router.py:220–226](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L220-L226)。

````python
def clear() -> None:
  """Clear all registered providers. Mainly for testing."""
  global _entries  # pylint: disable=global-statement
  _entries = []
  _entry_keys.clear()  # Also clear dedup keys to allow re-registration
  resolve.cache_clear()
  resolve_provider.cache_clear()
````

clear 清空条目列表、去重集合和两个查询缓存。测试结束时清零状态可避免上一用例影响下一用例；它不会从 sys.modules 卸载 Python 模块。

源码出处：[langextract/providers/router.py:229–238](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L229-L238)。

````python
def list_providers() -> list[tuple[tuple[str, ...], int]]:
  """List all registered providers with their patterns and priorities.

  Returns:
    List of (patterns, priority) tuples for debugging.
  """
  return [
      (tuple(p.pattern for p in entry.patterns), entry.priority)
      for entry in _entries
  ]
````
源码出处：[langextract/providers/router.py:241–247](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L241-L247)。

````python
def list_entries() -> list[tuple[list[str], int]]:
  """List all registered patterns and priorities. Mainly for debugging.

  Returns:
    List of (patterns, priority) tuples.
  """
  return [([p.pattern for p in e.patterns], e.priority) for e in _entries]
````

两个函数都返回规则和优先级而不是实际模型。前者每项规则是 tuple，后者是 list，主要供检查注册状态。

## 14.5 内置规则从哪里来

源码出处：[langextract/providers/builtin_registry.py:26–31](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/builtin_registry.py#L26-L31)。

````python
class ProviderConfig(TypedDict):
  """Configuration for a provider registration."""

  patterns: tuple[str, ...]
  target: str
  priority: int
````

ProviderConfig 是 TypedDict：描述 patterns、target、priority 三个键的类型。它没有运行时校验功能，不是新的模型配置类。其后的 BUILTIN_PROVIDERS 是 Gemini、Ollama、OpenAI 的静态注册资料，具体正则集中在 patterns.py。

源码出处：[langextract/providers/__init__.py:49–68](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/__init__.py#L49-L68)。

````python
def load_builtins_once() -> None:
  """Load built-in providers to register their patterns.

  Idempotent function that ensures provider patterns are available
  for model resolution. Uses lazy registration to ensure providers
  can be re-registered after registry.clear() even if their modules
  are already in sys.modules.
  """
  global _builtins_loaded  # pylint: disable=global-statement

  # Register built-ins lazily so they can be re-registered after a registry.clear()
  # even if their modules were already imported earlier in the test run.
  for config in builtin_registry.BUILTIN_PROVIDERS:
    router.register_lazy(
        *config["patterns"],
        target=config["target"],
        priority=config["priority"],
    )

  _builtins_loaded = True
````

load_builtins_once 遍历静态资料调用 register_lazy，再将 _builtins_loaded 标为真。虽然名称含 once，函数仍遍历注册资料，依赖路由去重保证幂等；这样即使测试清空 router 也能恢复。

## 14.6 第三方插件如何被发现

Python 包可在安装元数据中声明 entry point：某个分组、名字及导入目标。主程序通过 importlib.metadata 读取这些信息，而不必列出所有第三方包。

源码出处：[langextract/providers/__init__.py:71–139](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/__init__.py#L71-L139)。

````python
def load_plugins_once() -> None:
  """Load provider plugins from installed packages.

  Discovers and loads langextract provider plugins using entry points.
  This function is idempotent - multiple calls have no effect.
  """
  global _plugins_loaded  # pylint: disable=global-statement
  if _plugins_loaded:
    return

  if os.environ.get("LANGEXTRACT_DISABLE_PLUGINS", "").lower() in (
      "1",
      "true",
      "yes",
  ):
    logging.info("Plugin loading disabled via LANGEXTRACT_DISABLE_PLUGINS")
    _plugins_loaded = True
    return

  load_builtins_once()

  try:

    eps = metadata.entry_points()

    # Try different APIs based on what's available
    if hasattr(eps, "select"):
      # Python 3.10+ API
      provider_eps = eps.select(group="langextract.providers")
    elif hasattr(eps, "get"):
      # Python 3.9 API
      provider_eps = eps.get("langextract.providers", [])
    else:
      # Fallback for older versions
      provider_eps = [
          ep
          for ep in eps
          if getattr(ep, "group", None) == "langextract.providers"
      ]

    for entry_point in provider_eps:
      try:

        provider_class = entry_point.load()
        logging.info("Loaded provider plugin: %s", entry_point.name)

        if hasattr(provider_class, "get_model_patterns"):
          patterns = provider_class.get_model_patterns()
          for pattern in patterns:
            router.register(
                pattern,
                priority=getattr(
                    provider_class,
                    "pattern_priority",
                    20,  # Default plugin priority
                ),
            )(provider_class)
          logging.info(
              "Registered %d patterns for %s", len(patterns), entry_point.name
          )
      except Exception as e:
        logging.warning(
            "Failed to load provider plugin %s: %s", entry_point.name, e
        )

  except Exception as e:
    logging.warning("Error discovering provider plugins: %s", e)

  _plugins_loaded = True
````

load_plugins_once 检查已加载标记，亦支持通过环境配置关闭插件加载，然后确保注册内置项。它兼容新版 select、旧版 get 和可迭代入口三种 API，选择 langextract.providers 分组。

对每个 entry point 调用 load（这会执行插件导入代码），若类有 get_model_patterns，则逐条注册，优先级默认 20，也可由类提供 pattern_priority。单插件或发现操作失败记警告而不阻断其他插件，最后设置已加载标记。插件是运行在本进程内的 Python 代码，应仅安装可信来源，而不是安全隔离机制。

源码出处：[langextract/providers/__init__.py:142–146](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/__init__.py#L142-L146)。

````python
def _reset_for_testing() -> None:
  """Reset plugin loading state for testing. Should only be used in tests."""
  global _plugins_loaded, _builtins_loaded  # pylint: disable=global-statement
  _plugins_loaded = False
  _builtins_loaded = False
````

_reset_for_testing 只把两个加载标记改回 False；若需要清除规则，还要独立调用 router.clear。

源码出处：[langextract/providers/__init__.py:149–159](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/__init__.py#L149-L159)。

````python
def __getattr__(name: str):
  """Lazy loading for submodules."""
  if name == "router":
    return importlib.import_module("langextract.providers.router")
  elif name == "schemas":
    return importlib.import_module("langextract.providers.schemas")
  elif name == "_plugins_loaded":
    return _plugins_loaded
  elif name == "_builtins_loaded":
    return _builtins_loaded
  raise AttributeError(f"module {__name__!r} has no attribute {name!r}")
````

模块级 __getattr__ 在属性普通查询失败时运行，根据名称延迟导入 router、schemas，或返回内部加载标记；不认识的名称必须抛 AttributeError，保持 Python 属性语义。

## 14.7 另一套按名称发现插件的接口

plugins.py 提供名称到导入路径的映射，和 router 的模型正则不是同一个数据结构。

源码出处：[langextract/plugins.py:44–59](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L44-L59)。

````python
def _safe_entry_points(group: str) -> list:
  """Get entry points with Python 3.8-3.12 compatibility.

  Args:
    group: Entry point group name.

  Returns:
    List of entry points in the specified group.
  """
  eps = metadata.entry_points()
  try:
    # Python 3.10+
    return list(eps.select(group=group))
  except AttributeError:
    # Python 3.8-3.9
    return list(getattr(eps, "get")(group, []))
````

_safe_entry_points 从 metadata.entry_points 取指定组，新版用 select，AttributeError 时用旧 get 接口，统一返回 list。

源码出处：[langextract/plugins.py:62–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L62-L91)。

````python
@functools.lru_cache(maxsize=1)
def _discovered() -> dict[str, str]:
  """Cache discovered third-party providers.

  Returns:
    Dictionary mapping provider names to import specs.
  """
  # LangExtract registers its own providers in this entry-point group too.
  # Skip exact (name, spec) matches so they neither shadow a third-party entry
  # of the same name (first seen wins) nor leak when include_optional=False,
  # while still keeping third-party aliases for built-in classes.
  builtin_entries = {*_BUILTINS.items(), *_OPTIONAL_BUILTINS.items()}
  discovered: dict[str, str] = {}
  for ep in _safe_entry_points("langextract.providers"):
    # Handle both old and new entry_points API
    if hasattr(ep, "value"):
      spec = ep.value
    else:
      # Legacy API - construct from module and attr
      spec = f"{ep.module}:{ep.attr}" if ep.attr else ep.module
    if (ep.name, spec) in builtin_entries:
      continue
    discovered.setdefault(ep.name, spec)

  if discovered:
    logging.debug(
        "Discovered third-party providers: %s", list(discovered.keys())
    )

  return discovered
````

_discovered 缓存一次第三方发现结果，兼容 ep.value 及旧 module/attr。准确等于内置名称和目标的入口被跳过；同名第三方入口使用 setdefault 保留先遇到者。它只读取规格字符串，并不实例化模型。

源码出处：[langextract/plugins.py:94–117](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L94-L117)。

````python
def available_providers(
    allow_override: bool = False, include_optional: bool = True
) -> dict[str, str]:
  """Get all available providers (built-in + optional + third-party).

  Args:
    allow_override: If True, third-party providers can override built-ins.
                   If False (default), built-ins take precedence.
    include_optional: If True (default), include optional built-in providers
                     that may require extra dependencies. If False, omit
                     optional built-ins (a third-party provider registered
                     under an optional built-in's name is still returned).

  Returns:
    Dictionary mapping provider names to import specifications.
  """

  built_in = dict(_OPTIONAL_BUILTINS) if include_optional else {}
  built_in.update(_BUILTINS)
  third_party = _discovered()

  if allow_override:
    return {**built_in, **third_party}
  return {**third_party, **built_in}
````

available_providers 汇总必选内置、可选内置和第三方映射。include_optional 决定是否纳入可选内置资料，而不是检查 SDK 安装状态。字典解包顺序决定同名键由哪方保留，默认内置优先，allow_override 时第三方优先。

源码出处：[langextract/plugins.py:120–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L120-L175)。

````python
def _load_class(spec: str) -> type[base_model.BaseLanguageModel]:
  """Load a provider class from module:Class specification.

  Args:
    spec: Import specification in format "module.path:ClassName".

  Returns:
    The loaded provider class.

  Raises:
    ImportError: If the spec is invalid or module cannot be imported.
    TypeError: If the loaded class is not a BaseLanguageModel.
  """
  module_path, _, class_name = spec.partition(":")
  if not module_path or not class_name:
    raise ImportError(
        f"Invalid provider spec '{spec}' - expected 'module:Class'"
    )

  try:
    module = importlib.import_module(module_path)
  except ImportError as e:
    raise ImportError(
        f"Failed to import provider module '{module_path}': {e}"
    ) from e

  try:
    cls = getattr(module, class_name)
  except AttributeError as e:
    raise ImportError(
        f"Provider class '{class_name}' not found in module '{module_path}'"
    ) from e

  # Validate it's a language model
  if not isinstance(cls, type) or not issubclass(
      cls, base_model.BaseLanguageModel
  ):
    # Fallback: check structural compatibility for non-ABC classes
    missing = []
    for method in ("infer", "parse_output"):
      if not hasattr(cls, method):
        missing.append(method)

    if missing:
      raise TypeError(
          f"{cls} is not a BaseLanguageModel and missing required methods:"
          f" {missing}"
      )

    logging.warning(
        "Provider %s does not inherit from BaseLanguageModel but appears"
        " compatible",
        cls,
    )

  return cls
````

_load_class 按冒号分解导入路径，导入模块、取类属性，分别包装缺模块和缺属性的 ImportError。优先要求 BaseLanguageModel 子类，否则检查 infer、parse_output 两个属性是否存在，缺失报 TypeError，貌似兼容则警告。这只是粗略结构检查，并不能证明插件遵守全部协议。

源码出处：[langextract/plugins.py:178–210](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L178-L210)。

````python
@functools.lru_cache(maxsize=None)  # Cache all loaded classes
def get_provider_class(
    name: str, allow_override: bool = False, include_optional: bool = True
) -> type[base_model.BaseLanguageModel]:
  """Get a provider class by name.

  Args:
    name: Provider name (e.g., "gemini", "openai", "ollama").
    allow_override: If True, allow third-party providers to override built-ins.
    include_optional: If True (default), include optional providers that
                     may require extra dependencies.

  Returns:
    The provider class.

  Raises:
    KeyError: If the provider name is not found.
    ImportError: If the provider module cannot be imported (including
                missing optional dependencies).
    TypeError: If the provider class is not compatible.
  """
  providers = available_providers(allow_override, include_optional)

  if name not in providers:
    available = sorted(providers.keys())
    raise KeyError(
        f"Unknown provider '{name}'. Available providers:"
        f" {', '.join(available) if available else 'none'}.\nHint: Did you"
        " install the necessary extras (e.g., pip install"
        f" langextract[{name}])?"
    )

  return _load_class(providers[name])
````

get_provider_class 用可用名称字典查找目标，不存在时抛带候选列表的 KeyError，存在时调用 _load_class 并缓存返回类别。缓存无大小限制，以名称及配置参数为查询键。

## 14.8 工厂需要哪些配置

源码出处：[langextract/factory.py:40–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L40-L55)。

````python
@dataclasses.dataclass(slots=True, frozen=True)
class ModelConfig:
  """Configuration for instantiating a language model provider.

  Attributes:
    model_id: The model identifier (e.g., "gemini-3.5-flash", "gpt-4o").
    provider: Optional explicit provider name or class name. Use this to
      disambiguate when multiple providers support the same model_id.
    provider_kwargs: Optional provider-specific keyword arguments.
  """

  model_id: str | None = None
  provider: str | None = None
  provider_kwargs: dict[str, typing.Any] = dataclasses.field(
      default_factory=dict
  )
````

ModelConfig 是冻结且使用 slots 的 dataclass：model_id、显式 provider 名称和 provider_kwargs。default_factory=dict 保证每份配置拥有自己的初始字典。冻结的是字段赋值，内部字典仍可变。

源码出处：[langextract/factory.py:58–109](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L58-L109)。

````python
def _kwargs_with_environment_defaults(
    model_id: str, kwargs: dict[str, typing.Any]
) -> dict[str, typing.Any]:
  """Add environment-based defaults to provider kwargs.

  Args:
    model_id: The model identifier.
    kwargs: Existing keyword arguments.

  Returns:
    Updated kwargs with environment defaults.
  """
  resolved = dict(kwargs)

  if "api_key" not in resolved and not resolved.get("vertexai", False):
    model_lower = model_id.lower()
    env_vars_by_model_pattern = {
        "gemini": ("GEMINI_API_KEY", "LANGEXTRACT_API_KEY"),
        "gpt": ("OPENAI_API_KEY", "LANGEXTRACT_API_KEY"),
        patterns.OPENAI_REASONING_PATTERN: (
            "OPENAI_API_KEY",
            "LANGEXTRACT_API_KEY",
        ),
    }

    for model_pattern, env_vars in env_vars_by_model_pattern.items():
      if re.search(model_pattern, model_lower):
        found_keys = []
        for env_var in env_vars:
          key_val = os.getenv(env_var)
          if key_val:
            found_keys.append((env_var, key_val))

        if found_keys:
          resolved["api_key"] = found_keys[0][1]

          if len(found_keys) > 1:
            keys_list = ", ".join(k[0] for k in found_keys)
            warnings.warn(
                f"Multiple API keys detected in environment: {keys_list}. "
                f"Using {found_keys[0][0]} and ignoring others.",
                UserWarning,
                stacklevel=3,
            )
        break

  if "ollama" in model_id.lower() and "base_url" not in resolved:
    resolved["base_url"] = os.getenv(
        "OLLAMA_BASE_URL", "http://localhost:11434"
    )

  return resolved
````

_kwargs_with_environment_defaults 先复制调用参数。没有 api_key 键且非 Vertex 模式时根据模型名称模式寻找对应环境默认值，优先服务商专用设置再考虑通用设置；多个候选存在时只报告变量名称。Ollama 名称匹配且没有 base_url 键时补充环境或本机默认地址。注意它依模型名称匹配，不是根据已选定的类；显式键存在就不会被环境覆盖。

## 14.9 从配置走到实例

源码出处：[langextract/factory.py:223–253](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L223-L253)。

````python
def _resolve_provider_class(
    config: ModelConfig,
) -> type[base_model.BaseLanguageModel]:
  """Load providers and resolve the provider class for config.

  Both model-creation paths use this so provider import failures are reported
  the same way.

  Args:
    config: Model configuration with model_id and/or provider.

  Returns:
    The provider class.

  Raises:
    InferenceConfigError: If no provider matches the model_id or provider, or
      the matching provider's module cannot be imported.
  """
  providers.load_builtins_once()
  providers.load_plugins_once()

  try:
    if config.provider:
      return router.resolve_provider(config.provider)
    return router.resolve(config.model_id)
  except ImportError as e:
    raise exceptions.InferenceConfigError(
        "Failed to load provider. "
        "This may be due to missing dependencies. "
        f"Check that all required packages are installed. Error: {e}"
    ) from e
````

_resolve_provider_class 先确保内置和插件规则加载，然后优先按显式 provider 查找，否则按 model_id。路由 ImportError 包装为 InferenceConfigError 并保留异常链，使缺依赖问题在两条工厂路径中一致报告。

源码出处：[langextract/factory.py:112–181](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L112-L181)。

````python
def create_model(
    config: ModelConfig,
    examples: typing.Sequence[typing.Any] | None = None,
    use_schema_constraints: bool = False,
    fence_output: bool | None = None,
    return_fence_output: bool = False,
    output_schema: core_types.JsonSchema | None = None,
) -> base_model.BaseLanguageModel | tuple[base_model.BaseLanguageModel, bool]:
  """Create a language model instance from configuration.

  Args:
    config: Model configuration with optional model_id and/or provider.
    examples: Optional examples for schema generation (if use_schema_constraints=True).
    use_schema_constraints: Whether to apply schema constraints from examples.
    fence_output: Explicit fence output preference. If None, computed from schema.
    return_fence_output: If True, also return computed fence_output value.
    output_schema: Optional user-provided JSON schema for the raw LangExtract
      output. When provided, it is used verbatim and example-derived schema
      generation is skipped, regardless of use_schema_constraints. Cannot be
      combined with fence_output=True.

  Returns:
    An instantiated language model provider.
    If return_fence_output=True: Tuple of (model, model.requires_fence_output).

  Raises:
    ValueError: If neither model_id nor provider is specified.
    InferenceConfigError: If no provider matches the model_id or provider,
      the matching provider's module cannot be imported, or provider
      instantiation fails.
  """
  if not config.model_id and not config.provider:
    raise ValueError("Either model_id or provider must be specified")

  if (
      use_schema_constraints
      or fence_output is not None
      or output_schema is not None
  ):
    model = _create_model_with_schema(
        config=config,
        examples=examples,
        use_schema_constraints=use_schema_constraints,
        fence_output=fence_output,
        output_schema=output_schema,
    )
    if return_fence_output:
      return model, model.requires_fence_output
    return model

  provider_class = _resolve_provider_class(config)

  model_id = config.model_id

  kwargs = _kwargs_with_environment_defaults(
      model_id or config.provider or "", config.provider_kwargs
  )

  if model_id:
    kwargs["model_id"] = model_id

  try:
    model = provider_class(**kwargs)
    if return_fence_output:
      return model, model.requires_fence_output
    return model
  except (ValueError, TypeError) as e:
    raise exceptions.InferenceConfigError(
        f"Failed to create provider {provider_class.__name__}: {e}"
    ) from e
````

create_model 首先要求 model_id 或 provider 至少一项。如果使用示范约束、显式围栏设置或 output_schema，就走带规格的构造路径；否则先找类，补环境默认、写入 model_id，再用 provider_class(**kwargs) 创建对象。构造 ValueError、TypeError 包装成配置异常；return_fence_output 为真时返回模型与其围栏需求的二元组。

源码出处：[langextract/factory.py:184–206](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L184-L206)。

````python
def create_model_from_id(
    model_id: str | None = None,
    provider: str | None = None,
    *,
    output_schema: core_types.JsonSchema | None = None,
    **provider_kwargs: typing.Any,
) -> base_model.BaseLanguageModel:
  """Convenience function to create a model.

  Args:
    model_id: The model identifier (e.g., "gemini-3.5-flash").
    provider: Optional explicit provider name to disambiguate.
    output_schema: Optional user-provided JSON schema for the raw LangExtract
      output.
    **provider_kwargs: Optional provider-specific keyword arguments.

  Returns:
    An instantiated language model provider.
  """
  config = ModelConfig(
      model_id=model_id, provider=provider, provider_kwargs=provider_kwargs
  )
  return create_model(config, output_schema=output_schema)
````

create_model_from_id 是语法便利层，把直接参数组装为 ModelConfig，再调用 create_model；没有另写一条推理流程。

## 14.10 让工厂同时配置第 13 章的规格

源码出处：[langextract/factory.py:209–220](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L209-L220)。

````python
def _unsupported_output_schema_error(
    config: ModelConfig,
    provider_class: type[base_model.BaseLanguageModel],
) -> exceptions.InferenceConfigError:
  """Build an error naming the model or provider without output_schema support."""
  if config.model_id:
    target = f"model_id={config.model_id!r}"
  elif config.provider:
    target = f"provider={config.provider!r}"
  else:
    target = provider_class.__name__
  return exceptions.unsupported_output_schema_error(f"Provider for {target}")
````

_unsupported_output_schema_error 根据配置中的 model_id、provider 或具体类名构造目标说明，委托公共错误生成器；返回异常对象，由调用者 raise。

源码出处：[langextract/factory.py:256–346](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L256-L346)。

````python
def _create_model_with_schema(
    config: ModelConfig,
    examples: typing.Sequence[typing.Any] | None = None,
    use_schema_constraints: bool = True,
    fence_output: bool | None = None,
    output_schema: core_types.JsonSchema | None = None,
) -> base_model.BaseLanguageModel:
  """Internal helper to create a model with optional schema constraints.

  This function creates a language model and optionally configures it with
  schema constraints derived from the provided examples. It also computes
  appropriate fence defaulting based on the schema's capabilities.

  Args:
    config: Model configuration with model_id and/or provider.
    examples: Optional sequence of ExampleData for schema generation.
    use_schema_constraints: Whether to generate and apply schema constraints.
    fence_output: Whether to wrap output in markdown fences. If None,
      will be computed based on schema's requires_raw_output.
    output_schema: Optional user-provided JSON schema for the raw LangExtract
      output. When provided, it replaces example-derived schema generation.

  Returns:
    A model instance with fence_output configured appropriately.
  """
  if output_schema is not None and fence_output is True:
    raise exceptions.output_schema_fence_error()
  if output_schema is not None and not output_schema_lib.is_json_format_type(
      config.provider_kwargs.get("format_type")
  ):
    raise exceptions.output_schema_format_error()

  provider_class = _resolve_provider_class(config)

  schema_instance = None
  if output_schema is not None:
    schema_class = provider_class.get_schema_class()
    if schema_class is None:
      raise _unsupported_output_schema_error(config, provider_class)
    try:
      schema_instance = schema_class.from_schema_dict(output_schema)
    except NotImplementedError as e:
      raise _unsupported_output_schema_error(config, provider_class) from e
    core_schema.mark_from_output_schema(schema_instance)
  elif use_schema_constraints and examples:
    schema_class = provider_class.get_schema_class()
    if schema_class is not None:
      schema_instance = schema_class.from_examples(examples)

  if schema_instance:
    kwargs = schema_instance.to_provider_config()
    provider_kwargs = config.provider_kwargs
    if output_schema is not None:
      reserved = schema_instance.output_schema_reserved_provider_kwargs()
      conflicts = sorted(
          key for key in reserved if provider_kwargs.get(key) is not None
      )
      if conflicts:
        raise exceptions.output_schema_provider_kwargs_error(conflicts)
      provider_kwargs = {
          key: value
          for key, value in provider_kwargs.items()
          if value is not None or key not in reserved
      }
    kwargs.update(provider_kwargs)
  else:
    kwargs = dict(config.provider_kwargs)

  if schema_instance:
    schema_instance.sync_with_provider_kwargs(kwargs)

  # Add environment defaults
  model_id = config.model_id
  kwargs = _kwargs_with_environment_defaults(
      model_id or config.provider or "", kwargs
  )

  if model_id:
    kwargs["model_id"] = model_id

  try:
    model = provider_class(**kwargs)
  except (ValueError, TypeError) as e:
    raise exceptions.InferenceConfigError(
        f"Failed to create provider {provider_class.__name__}: {e}"
    ) from e

  model.apply_schema(schema_instance)
  model.set_fence_output(fence_output)

  return model
````

这条路径先拒绝显式 schema 与围栏、非 JSON 格式的冲突，再解析 provider 类。显式 schema 优先于示范：从服务商 get_schema_class 取得规格类，不支持或 from_schema_dict 抛 NotImplementedError 就产生配置错误；成功则标记来源。没有显式规格而请求示范约束且有示范时，用 from_examples。

若有规格，先取 to_provider_config；显式规格禁止与不为空的 reserved provider kwargs 冲突，过滤这些键的 None 值；随后用调用者参数覆盖普通配置，并调用 sync_with_provider_kwargs。没有规格则复制配置。再补环境默认、模型 ID，创建实例，最后调用 apply_schema 和 set_fence_output。传 None 让模型自行计算围栏默认。

这个设计不是要求每个供应商都支持所有规格，而是用共同协议询问能力，并对错误配置早期拒绝。

## 14.11 练习

1. 给教学 registry 增加优先级较低的重叠规则，验证结果不变。
2. 将 register 的规则存为可调用 loader，打印何时才真正加载类。
3. 在项目中使用 ModelConfig 指定模型和 provider，比较明确选择和正则选择。无需真的发请求，只研究 router 返回类。

我们现在已经知道怎样找到需要的模型类。下一章要研究类是怎样接通各服务并统一输出的。
