# 源码教学覆盖率

提交：`b3499195e5ee711d3bb7da5178ba40456046f5f3`

仅正文已展开解释的定义计入覆盖，单独列出符号不计入。当前不是全书最终版。

| 范围 | Python 文件 | 定义总数 | 已讲解 | 待讲解 | 覆盖率 |
|---|---:|---:|---:|---:|---:|
| 运行包 | 46 | 416 | 152 | 264 | 36.5% |
| 测试及辅助代码 | 50 | 1302 | 0 | 1302 | 0.0% |
| 合计 | 96 | 1718 | 152 | 1566 | 8.8% |

## 已讲章节

| 章节 | 已讲解定义数 |
|---|---:|
| [02-records.md](02-records.md) | 20 |
| [03-format.md](03-format.md) | 13 |
| [04-prompt.md](04-prompt.md) | 17 |
| [05-model.md](05-model.md) | 16 |
| [06-resolve.md](06-resolve.md) | 16 |
| [07-tokenizer.md](07-tokenizer.md) | 26 |
| [08-chunking.md](08-chunking.md) | 23 |
| [09-exact-alignment.md](09-exact-alignment.md) | 12 |
| [10-fuzzy-alignment.md](10-fuzzy-alignment.md) | 9 |

## 模块待讲情况

| 文件 | 已讲 | 总定义 |
|---|---:|---:|
| .github/scripts/pr_security_check.py | 0 | 32 |
| .github/scripts/zenodo_publish.py | 0 | 9 |
| benchmarks/benchmark.py | 0 | 10 |
| benchmarks/config.py | 0 | 6 |
| benchmarks/fuzzy_benchmark.py | 0 | 9 |
| benchmarks/plotting.py | 0 | 13 |
| benchmarks/utils.py | 0 | 8 |
| examples/custom_provider_plugin/langextract_provider_example/__init__.py | 0 | 0 |
| examples/custom_provider_plugin/langextract_provider_example/provider.py | 0 | 5 |
| examples/custom_provider_plugin/langextract_provider_example/schema.py | 0 | 6 |
| examples/custom_provider_plugin/test_example_provider.py | 0 | 1 |
| examples/ollama/demo_ollama.py | 0 | 11 |
| langextract/__init__.py | 0 | 4 |
| langextract/_compat/__init__.py | 0 | 0 |
| langextract/_compat/exceptions.py | 0 | 1 |
| langextract/_compat/inference.py | 0 | 2 |
| langextract/_compat/registry.py | 0 | 1 |
| langextract/_compat/schema.py | 0 | 1 |
| langextract/annotation.py | 0 | 11 |
| langextract/chunking.py | 23 | 23 |
| langextract/core/__init__.py | 0 | 0 |
| langextract/core/base_model.py | 12 | 12 |
| langextract/core/data.py | 20 | 20 |
| langextract/core/debug_utils.py | 0 | 7 |
| langextract/core/exceptions.py | 0 | 17 |
| langextract/core/format_handler.py | 12 | 12 |
| langextract/core/output_schema.py | 0 | 8 |
| langextract/core/schema.py | 0 | 15 |
| langextract/core/tokenizer.py | 26 | 26 |
| langextract/core/types.py | 5 | 5 |
| langextract/data.py | 0 | 0 |
| langextract/data_lib.py | 0 | 3 |
| langextract/exceptions.py | 0 | 0 |
| langextract/extraction.py | 0 | 2 |
| langextract/factory.py | 0 | 7 |
| langextract/inference.py | 0 | 1 |
| langextract/io.py | 0 | 8 |
| langextract/plugins.py | 0 | 5 |
| langextract/progress.py | 0 | 14 |
| langextract/prompt_validation.py | 0 | 12 |
| langextract/prompting.py | 17 | 17 |
| langextract/providers/__init__.py | 0 | 4 |
| langextract/providers/builtin_registry.py | 0 | 1 |
| langextract/providers/gemini.py | 0 | 13 |
| langextract/providers/gemini_batch.py | 0 | 32 |
| langextract/providers/ollama.py | 0 | 13 |
| langextract/providers/openai.py | 0 | 12 |
| langextract/providers/openai_batch.py | 0 | 18 |
| langextract/providers/patterns.py | 0 | 0 |
| langextract/providers/router.py | 0 | 12 |
| langextract/providers/schemas/__init__.py | 0 | 0 |
| langextract/providers/schemas/gemini.py | 0 | 10 |
| langextract/providers/schemas/openai.py | 0 | 13 |
| langextract/registry.py | 0 | 1 |
| langextract/resolver.py | 37 | 37 |
| langextract/schema.py | 0 | 1 |
| langextract/tokenizer.py | 0 | 0 |
| langextract/visualization.py | 0 | 15 |
| scripts/check_mock_patch_spec.py | 0 | 3 |
| scripts/create_provider_plugin.py | 0 | 31 |
| scripts/validate_community_providers.py | 0 | 5 |
| skills/langextract-usage/examples/basic_extraction.py | 0 | 0 |
| skills/langextract-usage/examples/multiple_documents.py | 0 | 0 |
| skills/langextract-usage/examples/relationship_extraction.py | 0 | 0 |
| tests/annotation_test.py | 0 | 40 |
| tests/check_mock_patch_spec_test.py | 0 | 17 |
| tests/chunking_test.py | 0 | 22 |
| tests/create_provider_plugin_test.py | 0 | 83 |
| tests/data_lib_test.py | 0 | 28 |
| tests/extract_precedence_test.py | 0 | 29 |
| tests/extract_schema_integration_test.py | 0 | 27 |
| tests/factory_schema_test.py | 0 | 28 |
| tests/factory_test.py | 0 | 69 |
| tests/format_handler_test.py | 0 | 10 |
| tests/fuzzy_alignment_cases_test.py | 0 | 29 |
| tests/gemini_retry_test.py | 0 | 70 |
| tests/inference_test.py | 0 | 55 |
| tests/init_test.py | 0 | 36 |
| tests/io_test.py | 0 | 17 |
| tests/openai_batch_test.py | 0 | 46 |
| tests/plugins_test.py | 0 | 6 |
| tests/pr_security_check_test.py | 0 | 56 |
| tests/progress_test.py | 0 | 6 |
| tests/prompt_validation_test.py | 0 | 17 |
| tests/prompting_test.py | 0 | 15 |
| tests/provider_plugin_test.py | 0 | 51 |
| tests/provider_schema_test.py | 0 | 48 |
| tests/registry_test.py | 0 | 37 |
| tests/resolver_test.py | 0 | 53 |
| tests/schema_test.py | 0 | 56 |
| tests/test_gemini_batch_api.py | 0 | 70 |
| tests/test_kwargs_passthrough.py | 0 | 38 |
| tests/test_live_api.py | 0 | 31 |
| tests/test_ollama_integration.py | 0 | 7 |
| tests/tokenizer_test.py | 0 | 46 |
| tests/visualization_test.py | 0 | 10 |
