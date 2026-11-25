# 完整源码符号索引

固定提交：`b3499195e5ee711d3bb7da5178ba40456046f5f3`。

范围：Git 跟踪的全部 .py 文件，使用 Python AST 递归枚举显式 class、def、async def（含嵌套定义、属性 getter/setter）。 不含 dataclass 自动生成的方法、继承但未在本项目定义的方法、lambda、动态产生的函数或第三方依赖。 同名属性访问器用行号区分。索引不等于教学正文。

## .github/scripts/pr_security_check.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `Finding` | 类 | [177–186](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L177-L186) | 待讲解 |
| `FileDiff` | 类 | [189–217](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L189-L217) | 待讲解 |
| `FileDiff.suffix` | 函数/方法 | [200–202](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L200-L202) | 待讲解 |
| `FileDiff.added_text` | 函数/方法 | [204–206](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L204-L206) | 待讲解 |
| `FileDiff.is_doc` | 函数/方法 | [208–210](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L208-L210) | 待讲解 |
| `FileDiff.runs_in_ci` | 函数/方法 | [212–217](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L212-L217) | 待讲解 |
| `normalize` | 函数/方法 | [220–226](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L220-L226) | 待讲解 |
| `printable` | 函数/方法 | [229–238](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L229-L238) | 待讲解 |
| `safe_excerpt` | 函数/方法 | [241–253](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L241-L253) | 待讲解 |
| `_parse_path` | 函数/方法 | [263–279](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L263-L279) | 待讲解 |
| `_diff_git_path` | 函数/方法 | [282–296](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L282-L296) | 待讲解 |
| `parse_diff` | 函数/方法 | [299–351](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L299-L351) | 待讲解 |
| `_first_line` | 函数/方法 | [354–358](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L354-L358) | 待讲解 |
| `_is_network_line` | 函数/方法 | [361–371](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L361-L371) | 待讲解 |
| `_network_lines` | 函数/方法 | [374–389](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L374-L389) | 待讲解 |
| `_reads_env` | 函数/方法 | [392–397](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L392-L397) | 待讲解 |
| `_check_credential_egress` | 函数/方法 | [400–456](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L400-L456) | 待讲解 |
| `_check_hosts` | 函数/方法 | [459–481](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L459-L481) | 待讲解 |
| `_has_zero_width` | 函数/方法 | [484–495](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L484-L495) | 待讲解 |
| `_check_hidden_characters` | 函数/方法 | [498–518](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L498-L518) | 待讲解 |
| `_lookalike_identifiers` | 函数/方法 | [521–528](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L521-L528) | 待讲解 |
| `_check_file_shape` | 函数/方法 | [531–592](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L531-L592) | 待讲解 |
| `_check_code_patterns` | 函数/方法 | [595–636](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L595-L636) | 待讲解 |
| `_check_configuration` | 函数/方法 | [639–687](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L639-L687) | 待讲解 |
| `_check_across_files` | 函数/方法 | [690–714](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L690-L714) | 待讲解 |
| `check_files` | 函数/方法 | [717–735](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L717-L735) | 待讲解 |
| `check_metadata` | 函数/方法 | [738–797](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L738-L797) | 待讲解 |
| `git_diff` | 函数/方法 | [800–820](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L800-L820) | 待讲解 |
| `sort_findings` | 函数/方法 | [823–826](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L823-L826) | 待讲解 |
| `render_summary` | 函数/方法 | [829–877](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L829-L877) | 待讲解 |
| `_read_optional` | 函数/方法 | [880–883](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L880-L883) | 待讲解 |
| `main` | 函数/方法 | [886–946](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/pr_security_check.py#L886-L946) | 待讲解 |

## .github/scripts/zenodo_publish.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `display_name` | 函数/方法 | [57–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/zenodo_publish.py#L57-L73) | 待讲解 |
| `_check` | 函数/方法 | [76–83](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/zenodo_publish.py#L76-L83) | 待讲解 |
| `new_version_draft` | 函数/方法 | [86–98](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/zenodo_publish.py#L86-L98) | 待讲解 |
| `published_metadata` | 函数/方法 | [101–109](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/zenodo_publish.py#L101-L109) | 待讲解 |
| `release_date` | 函数/方法 | [112–124](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/zenodo_publish.py#L112-L124) | 待讲解 |
| `upload_file` | 函数/方法 | [127–154](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/zenodo_publish.py#L127-L154) | 待讲解 |
| `update_metadata` | 函数/方法 | [157–186](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/zenodo_publish.py#L157-L186) | 待讲解 |
| `publish_draft` | 函数/方法 | [189–196](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/zenodo_publish.py#L189-L196) | 待讲解 |
| `main` | 函数/方法 | [199–232](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/.github/scripts/zenodo_publish.py#L199-L232) | 待讲解 |

## benchmarks/benchmark.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `BenchmarkRunner` | 类 | [66–370](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L66-L370) | 待讲解 |
| `BenchmarkRunner.__init__` | 函数/方法 | [69–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L69-L73) | 待讲解 |
| `BenchmarkRunner.set_tokenizer` | 函数/方法 | [75–82](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L75-L82) | 待讲解 |
| `BenchmarkRunner.print_header` | 函数/方法 | [84–92](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L84-L92) | 待讲解 |
| `BenchmarkRunner.benchmark_tokenization` | 函数/方法 | [94–138](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L94-L138) | 待讲解 |
| `BenchmarkRunner.test_single_extraction` | 函数/方法 | [140–274](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L140-L274) | 待讲解 |
| `BenchmarkRunner.test_diverse_text_types` | 函数/方法 | [276–309](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L276-L309) | 待讲解 |
| `BenchmarkRunner.save_results` | 函数/方法 | [311–356](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L311-L356) | 待讲解 |
| `BenchmarkRunner.run_diverse_benchmark` | 函数/方法 | [358–370](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L358-L370) | 待讲解 |
| `main` | 函数/方法 | [373–434](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/benchmark.py#L373-L434) | 待讲解 |

## benchmarks/config.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `TokenizationConfig` | 类 | [32–37](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/config.py#L32-L37) | 待讲解 |
| `ModelConfig` | 类 | [40–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/config.py#L40-L49) | 待讲解 |
| `TextTypes` | 类 | [52–58](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/config.py#L52-L58) | 待讲解 |
| `DisplayConfig` | 类 | [78–86](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/config.py#L78-L86) | 待讲解 |
| `PathConfig` | 类 | [89–100](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/config.py#L89-L100) | 待讲解 |
| `PathConfig.get_result_path` | 函数/方法 | [95–100](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/config.py#L95-L100) | 待讲解 |

## benchmarks/fuzzy_benchmark.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_generate_source_text` | 函数/方法 | [111–115](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/fuzzy_benchmark.py#L111-L115) | 待讲解 |
| `_plant_span` | 函数/方法 | [118–124](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/fuzzy_benchmark.py#L118-L124) | 待讲解 |
| `_plant_gapped` | 函数/方法 | [127–133](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/fuzzy_benchmark.py#L127-L133) | 待讲解 |
| `_make_extraction` | 函数/方法 | [136–140](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/fuzzy_benchmark.py#L136-L140) | 待讲解 |
| `_build_cases` | 函数/方法 | [143–227](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/fuzzy_benchmark.py#L143-L227) | 待讲解 |
| `_get_metadata` | 函数/方法 | [235–265](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/fuzzy_benchmark.py#L235-L265) | 待讲解 |
| `_run_single` | 函数/方法 | [268–330](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/fuzzy_benchmark.py#L268-L330) | 待讲解 |
| `_tokenize_words` | 函数/方法 | [333–339](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/fuzzy_benchmark.py#L333-L339) | 待讲解 |
| `main` | 函数/方法 | [342–501](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/fuzzy_benchmark.py#L342-L501) | 待讲解 |

## benchmarks/plotting.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `create_diverse_plots` | 函数/方法 | [36–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L36-L81) | 待讲解 |
| `_plot_tokenization_throughput` | 函数/方法 | [84–122](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L84-L122) | 待讲解 |
| `_plot_tokenization_rate` | 函数/方法 | [125–167](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L125-L167) | 待讲解 |
| `_plot_extraction_density` | 函数/方法 | [170–217](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L170-L217) | 待讲解 |
| `_plot_processing_speed` | 函数/方法 | [220–267](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L220-L267) | 待讲解 |
| `_plot_summary_table` | 函数/方法 | [270–317](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L270-L317) | 待讲解 |
| `create_comparison_plots` | 函数/方法 | [320–373](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L320-L373) | 待讲解 |
| `_plot_entity_comparison` | 函数/方法 | [376–432](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L376-L432) | 待讲解 |
| `_plot_time_comparison` | 函数/方法 | [435–489](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L435-L489) | 待讲解 |
| `_plot_tokenization_comparison` | 函数/方法 | [492–547](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L492-L547) | 待讲解 |
| `_plot_success_rate_comparison` | 函数/方法 | [550–603](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L550-L603) | 待讲解 |
| `_plot_token_rate_by_language` | 函数/方法 | [606–642](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L606-L642) | 待讲解 |
| `_plot_timeline` | 函数/方法 | [645–700](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/plotting.py#L645-L700) | 待讲解 |

## benchmarks/utils.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `download_text` | 函数/方法 | [26–39](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/utils.py#L26-L39) | 待讲解 |
| `extract_text_content` | 函数/方法 | [42–74](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/utils.py#L42-L74) | 待讲解 |
| `get_text_from_gutenberg` | 函数/方法 | [77–92](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/utils.py#L77-L92) | 待讲解 |
| `get_optimal_text_size` | 函数/方法 | [95–114](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/utils.py#L95-L114) | 待讲解 |
| `get_extraction_example` | 函数/方法 | [117–128](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/utils.py#L117-L128) | 待讲解 |
| `get_git_info` | 函数/方法 | [131–164](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/utils.py#L131-L164) | 待讲解 |
| `analyze_tokenization` | 函数/方法 | [167–191](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/utils.py#L167-L191) | 待讲解 |
| `format_tokenization_summary` | 函数/方法 | [194–206](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/benchmarks/utils.py#L194-L206) | 待讲解 |

## examples/custom_provider_plugin/langextract_provider_example/provider.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `CustomGeminiProvider` | 类 | [31–184](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/provider.py#L31-L184) | 待讲解 |
| `CustomGeminiProvider.__init__` | 函数/方法 | [60–104](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/provider.py#L60-L104) | 待讲解 |
| `CustomGeminiProvider.get_schema_class` | 函数/方法 | [106–116](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/provider.py#L106-L116) | 待讲解 |
| `CustomGeminiProvider.apply_schema` | 函数/方法 | [118–142](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/provider.py#L118-L142) | 待讲解 |
| `CustomGeminiProvider.infer` | 函数/方法 | [144–184](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/provider.py#L144-L184) | 待讲解 |

## examples/custom_provider_plugin/langextract_provider_example/schema.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `CustomProviderSchema` | 类 | [25–163](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/schema.py#L25-L163) | 待讲解 |
| `CustomProviderSchema.__init__` | 函数/方法 | [40–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/schema.py#L40-L49) | 待讲解 |
| `CustomProviderSchema.from_examples` | 函数/方法 | [51–122](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/schema.py#L51-L122) | 待讲解 |
| `CustomProviderSchema.to_provider_config` | 函数/方法 | [124–144](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/schema.py#L124-L144) | 待讲解 |
| `CustomProviderSchema.requires_raw_output` | 函数/方法 | [146–154](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/schema.py#L146-L154) | 待讲解 |
| `CustomProviderSchema.schema_dict` | 函数/方法 | [156–163](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/langextract_provider_example/schema.py#L156-L163) | 待讲解 |

## examples/custom_provider_plugin/test_example_provider.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `main` | 函数/方法 | [29–54](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/custom_provider_plugin/test_example_provider.py#L29-L54) | 待讲解 |

## examples/ollama/demo_ollama.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `check_ollama_available` | 函数/方法 | [67–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L67-L73) | 待讲解 |
| `ensure_output_directory` | 函数/方法 | [76–80](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L76-L80) | 待讲解 |
| `print_header` | 函数/方法 | [83–87](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L83-L87) | 待讲解 |
| `print_section` | 函数/方法 | [90–93](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L90-L93) | 待讲解 |
| `print_results_summary` | 函数/方法 | [96–111](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L96-L111) | 待讲解 |
| `example_romeo_juliet` | 函数/方法 | [114–172](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L114-L172) | 待讲解 |
| `example_medication_ner` | 函数/方法 | [175–228](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L175-L228) | 待讲解 |
| `example_medication_relationships` | 函数/方法 | [231–325](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L231-L325) | 待讲解 |
| `example_shakespeare_dialogue` | 函数/方法 | [328–415](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L328-L415) | 待讲解 |
| `save_results` | 函数/方法 | [418–455](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L418-L455) | 待讲解 |
| `main` | 函数/方法 | [458–557](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/examples/ollama/demo_ollama.py#L458-L557) | 待讲解 |

## langextract/__init__.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `extract` | 函数/方法 | [53–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/__init__.py#L53-L55) | 待讲解 |
| `visualize` | 函数/方法 | [58–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/__init__.py#L58-L60) | 待讲解 |
| `__getattr__` | 函数/方法 | [87–98](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/__init__.py#L87-L98) | 待讲解 |
| `__dir__` | 函数/方法 | [101–102](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/__init__.py#L101-L102) | 待讲解 |

## langextract/_compat/exceptions.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `__getattr__` | 函数/方法 | [26–44](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/_compat/exceptions.py#L26-L44) | 待讲解 |

## langextract/_compat/inference.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `InferenceType` | 类 | [23–27](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/_compat/inference.py#L23-L27) | 待讲解 |
| `__getattr__` | 函数/方法 | [30–61](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/_compat/inference.py#L30-L61) | 待讲解 |

## langextract/_compat/registry.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `__getattr__` | 函数/方法 | [25–33](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/_compat/registry.py#L25-L33) | 待讲解 |

## langextract/_compat/schema.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `__getattr__` | 函数/方法 | [23–41](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/_compat/schema.py#L23-L41) | 待讲解 |

## langextract/annotation.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_merge_non_overlapping_extractions` | 函数/方法 | [46–84](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L46-L84) | [已讲解](11-pipeline.md) |
| `_extractions_overlap` | 函数/方法 | [87–115](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L87-L115) | [已讲解](11-pipeline.md) |
| `_document_chunk_iterator` | 函数/方法 | [118–160](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L118-L160) | [已讲解](11-pipeline.md) |
| `Annotator` | 类 | [163–638](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L163-L638) | [已讲解](11-pipeline.md) |
| `Annotator.__init__` | 函数/方法 | [166–207](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L166-L207) | [已讲解](11-pipeline.md) |
| `Annotator.annotate_documents` | 函数/方法 | [209–284](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L209-L284) | [已讲解](11-pipeline.md) |
| `Annotator._annotate_documents_single_pass` | 函数/方法 | [286–457](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L286-L457) | [已讲解](11-pipeline.md) |
| `Annotator._annotate_documents_single_pass._capture_docs` | 函数/方法 | [322–332](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L322-L332) | [已讲解](11-pipeline.md) |
| `Annotator._annotate_documents_single_pass._emit_docs_iter` | 函数/方法 | [334–354](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L334-L354) | [已讲解](11-pipeline.md) |
| `Annotator._annotate_documents_sequential_passes` | 函数/方法 | [459–542](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L459-L542) | [已讲解](11-pipeline.md) |
| `Annotator.annotate_text` | 函数/方法 | [544–638](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/annotation.py#L544-L638) | [已讲解](11-pipeline.md) |

## langextract/chunking.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `TokenUtilError` | 类 | [35–36](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L35-L36) | [已讲解](08-chunking.md) |
| `TextChunk` | 类 | [39–140](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L39-L140) | [已讲解](08-chunking.md) |
| `TextChunk.__str__` | 函数/方法 | [60–83](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L60-L83) | [已讲解](08-chunking.md) |
| `TextChunk.document_id` | 函数/方法 | [85–90](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L85-L90) | [已讲解](08-chunking.md) |
| `TextChunk.document_text` | 函数/方法 | [92–97](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L92-L97) | [已讲解](08-chunking.md) |
| `TextChunk.chunk_text` | 函数/方法 | [99–108](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L99-L108) | [已讲解](08-chunking.md) |
| `TextChunk.sanitized_chunk_text` | 函数/方法 | [110–115](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L110-L115) | [已讲解](08-chunking.md) |
| `TextChunk.additional_context` | 函数/方法 | [117–122](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L117-L122) | [已讲解](08-chunking.md) |
| `TextChunk.char_interval` | 函数/方法 | [124–140](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L124-L140) | [已讲解](08-chunking.md) |
| `create_token_interval` | 函数/方法 | [143–166](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L143-L166) | [已讲解](08-chunking.md) |
| `get_token_interval_text` | 函数/方法 | [169–213](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L169-L213) | [已讲解](08-chunking.md) |
| `get_char_interval` | 函数/方法 | [216–243](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L216-L243) | [已讲解](08-chunking.md) |
| `_sanitize` | 函数/方法 | [246–262](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L246-L262) | [已讲解](08-chunking.md) |
| `make_batches_of_textchunk` | 函数/方法 | [265–279](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L265-L279) | [已讲解](08-chunking.md) |
| `SentenceIterator` | 类 | [282–340](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L282-L340) | [已讲解](08-chunking.md) |
| `SentenceIterator.__init__` | 函数/方法 | [285–310](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L285-L310) | [已讲解](08-chunking.md) |
| `SentenceIterator.__iter__` | 函数/方法 | [312–313](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L312-L313) | [已讲解](08-chunking.md) |
| `SentenceIterator.__next__` | 函数/方法 | [315–340](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L315-L340) | [已讲解](08-chunking.md) |
| `ChunkIterator` | 类 | [343–506](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L343-L506) | [已讲解](08-chunking.md) |
| `ChunkIterator.__init__` | 函数/方法 | [385–420](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L385-L420) | [已讲解](08-chunking.md) |
| `ChunkIterator.__iter__` | 函数/方法 | [422–423](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L422-L423) | [已讲解](08-chunking.md) |
| `ChunkIterator._tokens_exceed_buffer` | 函数/方法 | [425–439](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L425-L439) | [已讲解](08-chunking.md) |
| `ChunkIterator.__next__` | 函数/方法 | [441–506](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/chunking.py#L441-L506) | [已讲解](08-chunking.md) |

## langextract/core/base_model.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `BaseLanguageModel` | 类 | [32–231](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L32-L231) | [已讲解](05-model.md) |
| `BaseLanguageModel.__init__` | 函数/方法 | [39–50](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L39-L50) | [已讲解](05-model.md) |
| `BaseLanguageModel.get_schema_class` | 函数/方法 | [52–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L52-L55) | [已讲解](05-model.md) |
| `BaseLanguageModel.apply_schema` | 函数/方法 | [57–66](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L57-L66) | [已讲解](05-model.md) |
| `BaseLanguageModel.apply_output_schema` | 函数/方法 | [68–114](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L68-L114) | [已讲解](05-model.md) |
| `BaseLanguageModel.schema` | 函数/方法 | [116–123](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L116-L123) | [已讲解](05-model.md) |
| `BaseLanguageModel.set_fence_output` | 函数/方法 | [125–133](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L125-L133) | [已讲解](05-model.md) |
| `BaseLanguageModel.requires_fence_output` | 函数/方法 | [135–151](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L135-L151) | [已讲解](05-model.md) |
| `BaseLanguageModel.merge_kwargs` | 函数/方法 | [153–168](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L153-L168) | [已讲解](05-model.md) |
| `BaseLanguageModel.infer` | 函数/方法 | [170–184](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L170-L184) | [已讲解](05-model.md) |
| `BaseLanguageModel.infer_batch` | 函数/方法 | [186–203](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L186-L203) | [已讲解](05-model.md) |
| `BaseLanguageModel.parse_output` | 函数/方法 | [205–231](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/base_model.py#L205-L231) | [已讲解](05-model.md) |

## langextract/core/data.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `AlignmentStatus` | 类 | [43–47](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L43-L47) | [已讲解](02-records.md) |
| `CharInterval` | 类 | [50–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L50-L60) | [已讲解](02-records.md) |
| `Extraction` | 类 | [63–126](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L63-L126) | [已讲解](02-records.md) |
| `Extraction.__init__` | 函数/方法 | [97–118](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L97-L118) | [已讲解](02-records.md) |
| `Extraction.token_interval` | 函数/方法 | [120–122](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L120-L122) | [已讲解](02-records.md) |
| `Extraction.token_interval` | 函数/方法 | [124–126](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L124-L126) | [已讲解](02-records.md) |
| `Document` | 类 | [129–203](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L129-L203) | [已讲解](02-records.md) |
| `Document.__init__` | 函数/方法 | [150–159](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L150-L159) | [已讲解](02-records.md) |
| `Document.document_id` | 函数/方法 | [161–166](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L161-L166) | [已讲解](02-records.md) |
| `Document.document_id` | 函数/方法 | [168–171](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L168-L171) | [已讲解](02-records.md) |
| `Document.tokenized_text` | 函数/方法 | [173–178](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L173-L178) | [已讲解](02-records.md) |
| `Document.tokenized_text` | 函数/方法 | [180–182](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L180-L182) | [已讲解](02-records.md) |
| `Document.with_additional_context` | 函数/方法 | [184–203](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L184-L203) | [已讲解](02-records.md) |
| `AnnotatedDocument` | 类 | [206–260](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L206-L260) | [已讲解](02-records.md) |
| `AnnotatedDocument.__init__` | 函数/方法 | [227–236](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L227-L236) | [已讲解](02-records.md) |
| `AnnotatedDocument.document_id` | 函数/方法 | [238–243](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L238-L243) | [已讲解](02-records.md) |
| `AnnotatedDocument.document_id` | 函数/方法 | [245–248](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L245-L248) | [已讲解](02-records.md) |
| `AnnotatedDocument.tokenized_text` | 函数/方法 | [250–256](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L250-L256) | [已讲解](02-records.md) |
| `AnnotatedDocument.tokenized_text` | 函数/方法 | [258–260](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L258-L260) | [已讲解](02-records.md) |
| `ExampleData` | 类 | [263–273](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/data.py#L263-L273) | [已讲解](02-records.md) |

## langextract/core/debug_utils.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_safe_repr` | 函数/方法 | [49–54](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/debug_utils.py#L49-L54) | 待讲解 |
| `_redact_value` | 函数/方法 | [57–70](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/debug_utils.py#L57-L70) | 待讲解 |
| `_redact_mapping` | 函数/方法 | [73–78](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/debug_utils.py#L73-L78) | 待讲解 |
| `_format_bound_args` | 函数/方法 | [81–103](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/debug_utils.py#L81-L103) | 待讲解 |
| `debug_log_calls` | 函数/方法 | [106–148](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/debug_utils.py#L106-L148) | 待讲解 |
| `debug_log_calls.wrapper` | 函数/方法 | [112–146](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/debug_utils.py#L112-L146) | 待讲解 |
| `configure_debug_logging` | 函数/方法 | [151–185](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/debug_utils.py#L151-L185) | 待讲解 |

## langextract/core/exceptions.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `LangExtractError` | 类 | [42–48](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L42-L48) | 待讲解 |
| `InferenceError` | 类 | [51–52](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L51-L52) | 待讲解 |
| `InferenceConfigError` | 类 | [55–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L55-L60) | 待讲解 |
| `unsupported_output_schema_error` | 函数/方法 | [63–68](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L63-L68) | 待讲解 |
| `output_schema_fence_error` | 函数/方法 | [71–77](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L71-L77) | 待讲解 |
| `output_schema_format_error` | 函数/方法 | [80–85](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L80-L85) | 待讲解 |
| `output_schema_provider_kwargs_error` | 函数/方法 | [88–96](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L88-L96) | 待讲解 |
| `InferenceRuntimeError` | 类 | [99–122](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L99-L122) | 待讲解 |
| `InferenceRuntimeError.__init__` | 函数/方法 | [106–122](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L106-L122) | 待讲解 |
| `InferenceOutputError` | 类 | [125–130](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L125-L130) | 待讲解 |
| `InferenceOutputError.__init__` | 函数/方法 | [128–130](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L128-L130) | 待讲解 |
| `InvalidDocumentError` | 类 | [133–137](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L133-L137) | 待讲解 |
| `InternalError` | 类 | [140–144](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L140-L144) | 待讲解 |
| `ProviderError` | 类 | [147–148](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L147-L148) | 待讲解 |
| `SchemaError` | 类 | [151–152](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L151-L152) | 待讲解 |
| `FormatError` | 类 | [155–156](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L155-L156) | 待讲解 |
| `FormatParseError` | 类 | [159–168](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/exceptions.py#L159-L168) | 待讲解 |

## langextract/core/format_handler.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_normalize_format_type` | 函数/方法 | [49–64](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L49-L64) | [已讲解](03-format.md) |
| `FormatHandler` | 类 | [67–495](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L67-L495) | [已讲解](03-format.md) |
| `FormatHandler.__init__` | 函数/方法 | [84–122](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L84-L122) | [已讲解](03-format.md) |
| `FormatHandler.__repr__` | 函数/方法 | [124–132](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L124-L132) | [已讲解](03-format.md) |
| `FormatHandler.format_extraction_example` | 函数/方法 | [134–167](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L134-L167) | [已讲解](03-format.md) |
| `FormatHandler.parse_output` | 函数/方法 | [169–263](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L169-L263) | [已讲解](03-format.md) |
| `FormatHandler._add_fences` | 函数/方法 | [265–268](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L265-L268) | [已讲解](03-format.md) |
| `FormatHandler._is_valid_language_tag` | 函数/方法 | [270–277](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L270-L277) | [已讲解](03-format.md) |
| `FormatHandler._parse_with_fallback` | 函数/方法 | [279–294](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L279-L294) | [已讲解](03-format.md) |
| `FormatHandler._extract_content` | 函数/方法 | [296–351](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L296-L351) | [已讲解](03-format.md) |
| `FormatHandler.from_resolver_params` | 函数/方法 | [365–440](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L365-L440) | [已讲解](03-format.md) |
| `FormatHandler.from_kwargs` | 函数/方法 | [442–495](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/format_handler.py#L442-L495) | [已讲解](03-format.md) |

## langextract/core/output_schema.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `is_json_format_type` | 函数/方法 | [39–47](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L39-L47) | [已讲解](13-schema.md) |
| `validate_output_schema_format_handler` | 函数/方法 | [50–77](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L50-L77) | [已讲解](13-schema.md) |
| `_is_string_sequence` | 函数/方法 | [80–85](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L80-L85) | [已讲解](13-schema.md) |
| `_item_schema_branches` | 函数/方法 | [88–105](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L88-L105) | [已讲解](13-schema.md) |
| `validate_output_schema` | 函数/方法 | [108–186](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L108-L186) | [已讲解](13-schema.md) |
| `_copy_schema_mapping` | 函数/方法 | [189–197](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L189-L197) | [已讲解](13-schema.md) |
| `extractions_schema` | 函数/方法 | [200–246](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L200-L246) | [已讲解](13-schema.md) |
| `extraction_item_schema` | 函数/方法 | [249–322](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/output_schema.py#L249-L322) | [已讲解](13-schema.md) |

## langextract/core/schema.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `BaseSchema` | 类 | [39–105](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L39-L105) | [已讲解](13-schema.md) |
| `BaseSchema.from_examples` | 函数/方法 | [42–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L42-L49) | [已讲解](13-schema.md) |
| `BaseSchema.from_schema_dict` | 函数/方法 | [51–59](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L51-L59) | [已讲解](13-schema.md) |
| `BaseSchema.to_provider_config` | 函数/方法 | [61–68](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L61-L68) | [已讲解](13-schema.md) |
| `BaseSchema.output_schema_reserved_provider_kwargs` | 函数/方法 | [70–72](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L70-L72) | [已讲解](13-schema.md) |
| `BaseSchema.requires_raw_output` | 函数/方法 | [74–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L74-L81) | [已讲解](13-schema.md) |
| `BaseSchema.validate_format` | 函数/方法 | [83–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L83-L91) | [已讲解](13-schema.md) |
| `BaseSchema.sync_with_provider_kwargs` | 函数/方法 | [93–105](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L93-L105) | [已讲解](13-schema.md) |
| `mark_from_output_schema` | 函数/方法 | [108–115](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L108-L115) | [已讲解](13-schema.md) |
| `FormatModeSchema` | 类 | [118–164](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L118-L164) | [已讲解](13-schema.md) |
| `FormatModeSchema.__init__` | 函数/方法 | [126–130](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L126-L130) | [已讲解](13-schema.md) |
| `FormatModeSchema.from_examples` | 函数/方法 | [132–140](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L132-L140) | [已讲解](13-schema.md) |
| `FormatModeSchema.to_provider_config` | 函数/方法 | [142–144](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L142-L144) | [已讲解](13-schema.md) |
| `FormatModeSchema.requires_raw_output` | 函数/方法 | [146–149](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L146-L149) | [已讲解](13-schema.md) |
| `FormatModeSchema.sync_with_provider_kwargs` | 函数/方法 | [151–164](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/schema.py#L151-L164) | [已讲解](13-schema.md) |

## langextract/core/tokenizer.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `BaseTokenizerError` | 类 | [53–54](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L53-L54) | [已讲解](07-tokenizer.md) |
| `InvalidTokenIntervalError` | 类 | [57–58](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L57-L58) | [已讲解](07-tokenizer.md) |
| `SentenceRangeError` | 类 | [61–62](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L61-L62) | [已讲解](07-tokenizer.md) |
| `CharInterval` | 类 | [65–75](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L65-L75) | [已讲解](07-tokenizer.md) |
| `TokenInterval` | 类 | [78–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L78-L91) | [已讲解](07-tokenizer.md) |
| `TokenType` | 类 | [94–105](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L94-L105) | [已讲解](07-tokenizer.md) |
| `Token` | 类 | [108–131](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L108-L131) | [已讲解](07-tokenizer.md) |
| `TokenizedText` | 类 | [134–145](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L134-L145) | [已讲解](07-tokenizer.md) |
| `Tokenizer` | 类 | [165–177](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L165-L177) | [已讲解](07-tokenizer.md) |
| `Tokenizer.tokenize` | 函数/方法 | [168–177](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L168-L177) | [已讲解](07-tokenizer.md) |
| `RegexTokenizer` | 类 | [180–227](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L180-L227) | [已讲解](07-tokenizer.md) |
| `RegexTokenizer.tokenize` | 函数/方法 | [187–227](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L187-L227) | [已讲解](07-tokenizer.md) |
| `tokenize` | 函数/方法 | [234–246](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L234-L246) | [已讲解](07-tokenizer.md) |
| `Sentinel` | 类 | [257–264](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L257-L264) | [已讲解](07-tokenizer.md) |
| `Sentinel.__init__` | 函数/方法 | [260–261](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L260-L261) | [已讲解](07-tokenizer.md) |
| `Sentinel.__repr__` | 函数/方法 | [263–264](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L263-L264) | [已讲解](07-tokenizer.md) |
| `_get_script_fast` | 函数/方法 | [273–279](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L273-L279) | [已讲解](07-tokenizer.md) |
| `_classify_grapheme` | 函数/方法 | [282–291](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L282-L291) | [已讲解](07-tokenizer.md) |
| `_get_common_script_cached` | 函数/方法 | [312–318](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L312-L318) | [已讲解](07-tokenizer.md) |
| `UnicodeTokenizer` | 类 | [321–467](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L321-L467) | [已讲解](07-tokenizer.md) |
| `UnicodeTokenizer.tokenize` | 函数/方法 | [335–442](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L335-L442) | [已讲解](07-tokenizer.md) |
| `UnicodeTokenizer._emit_token` | 函数/方法 | [444–467](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L444-L467) | [已讲解](07-tokenizer.md) |
| `tokens_text` | 函数/方法 | [470–507](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L470-L507) | [已讲解](07-tokenizer.md) |
| `_is_end_of_sentence_token` | 函数/方法 | [510–546](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L510-L546) | [已讲解](07-tokenizer.md) |
| `_is_sentence_break_after_newline` | 函数/方法 | [549–577](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L549-L577) | [已讲解](07-tokenizer.md) |
| `find_sentence_range` | 函数/方法 | [580–647](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/tokenizer.py#L580-L647) | [已讲解](07-tokenizer.md) |

## langextract/core/types.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `FormatType` | 类 | [45–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/types.py#L45-L49) | [已讲解](03-format.md) |
| `ConstraintType` | 类 | [52–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/types.py#L52-L55) | [已讲解](05-model.md) |
| `Constraint` | 类 | [58–66](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/types.py#L58-L66) | [已讲解](05-model.md) |
| `ScoredOutput` | 类 | [69–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/types.py#L69-L81) | [已讲解](05-model.md) |
| `ScoredOutput.__str__` | 函数/方法 | [76–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/core/types.py#L76-L81) | [已讲解](05-model.md) |

## langextract/data_lib.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `enum_asdict_factory` | 函数/方法 | [28–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/data_lib.py#L28-L55) | 待讲解 |
| `annotated_document_to_dict` | 函数/方法 | [58–83](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/data_lib.py#L58-L83) | 待讲解 |
| `dict_to_annotated_document` | 函数/方法 | [86–133](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/data_lib.py#L86-L133) | 待讲解 |

## langextract/extraction.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_has_preconfigured_output_schema` | 函数/方法 | [39–42](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/extraction.py#L39-L42) | [已讲解](11-pipeline.md) |
| `extract` | 函数/方法 | [45–462](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/extraction.py#L45-L462) | [已讲解](11-pipeline.md) |

## langextract/factory.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `ModelConfig` | 类 | [40–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L40-L55) | [已讲解](14-factory.md) |
| `_kwargs_with_environment_defaults` | 函数/方法 | [58–109](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L58-L109) | [已讲解](14-factory.md) |
| `create_model` | 函数/方法 | [112–181](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L112-L181) | [已讲解](14-factory.md) |
| `create_model_from_id` | 函数/方法 | [184–206](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L184-L206) | [已讲解](14-factory.md) |
| `_unsupported_output_schema_error` | 函数/方法 | [209–220](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L209-L220) | [已讲解](14-factory.md) |
| `_resolve_provider_class` | 函数/方法 | [223–253](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L223-L253) | [已讲解](14-factory.md) |
| `_create_model_with_schema` | 函数/方法 | [256–346](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/factory.py#L256-L346) | [已讲解](14-factory.md) |

## langextract/inference.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `__getattr__` | 函数/方法 | [26–32](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/inference.py#L26-L32) | 待讲解 |

## langextract/io.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `InvalidDatasetError` | 类 | [38–39](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/io.py#L38-L39) | 待讲解 |
| `Dataset` | 类 | [42–84](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/io.py#L42-L84) | 待讲解 |
| `Dataset.load` | 函数/方法 | [50–84](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/io.py#L50-L84) | 待讲解 |
| `save_annotated_documents` | 函数/方法 | [87–143](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/io.py#L87-L143) | 待讲解 |
| `load_annotated_documents_jsonl` | 函数/方法 | [146–192](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/io.py#L146-L192) | 待讲解 |
| `_read_csv` | 函数/方法 | [195–231](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/io.py#L195-L231) | 待讲解 |
| `is_url` | 函数/方法 | [234–270](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/io.py#L234-L270) | 待讲解 |
| `download_text_from_url` | 函数/方法 | [273–361](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/io.py#L273-L361) | 待讲解 |

## langextract/plugins.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_safe_entry_points` | 函数/方法 | [44–59](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L44-L59) | [已讲解](14-factory.md) |
| `_discovered` | 函数/方法 | [62–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L62-L91) | [已讲解](14-factory.md) |
| `available_providers` | 函数/方法 | [94–117](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L94-L117) | [已讲解](14-factory.md) |
| `_load_class` | 函数/方法 | [120–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L120-L175) | [已讲解](14-factory.md) |
| `get_provider_class` | 函数/方法 | [178–210](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/plugins.py#L178-L210) | [已讲解](14-factory.md) |

## langextract/progress.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `create_download_progress_bar` | 函数/方法 | [34–78](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L34-L78) | 待讲解 |
| `create_extraction_progress_bar` | 函数/方法 | [81–102](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L81-L102) | [已讲解](11-pipeline.md) |
| `print_download_complete` | 函数/方法 | [105–119](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L105-L119) | 待讲解 |
| `print_extraction_complete` | 函数/方法 | [122–124](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L122-L124) | [已讲解](11-pipeline.md) |
| `print_extraction_summary` | 函数/方法 | [127–164](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L127-L164) | [已讲解](11-pipeline.md) |
| `create_save_progress_bar` | 函数/方法 | [167–186](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L167-L186) | 待讲解 |
| `create_load_progress_bar` | 函数/方法 | [189–220](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L189-L220) | 待讲解 |
| `print_save_complete` | 函数/方法 | [223–235](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L223-L235) | 待讲解 |
| `print_load_complete` | 函数/方法 | [238–250](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L238-L250) | 待讲解 |
| `get_model_info` | 函数/方法 | [253–268](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L253-L268) | [已讲解](11-pipeline.md) |
| `format_extraction_stats` | 函数/方法 | [271–283](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L271-L283) | [已讲解](11-pipeline.md) |
| `create_extraction_postfix` | 函数/方法 | [286–298](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L286-L298) | [已讲解](11-pipeline.md) |
| `format_extraction_progress` | 函数/方法 | [301–328](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L301-L328) | [已讲解](11-pipeline.md) |
| `create_pass_progress_bar` | 函数/方法 | [331–353](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/progress.py#L331-L353) | [已讲解](11-pipeline.md) |

## langextract/prompt_validation.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `PromptValidationLevel` | 类 | [48–53](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L48-L53) | [已讲解](12-validation.md) |
| `IssueKind` | 类 | [56–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L56-L60) | [已讲解](12-validation.md) |
| `ValidationIssue` | 类 | [63–87](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L63-L87) | [已讲解](12-validation.md) |
| `ValidationIssue.short_msg` | 函数/方法 | [76–87](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L76-L87) | [已讲解](12-validation.md) |
| `ValidationReport` | 类 | [90–104](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L90-L104) | [已讲解](12-validation.md) |
| `ValidationReport.has_failed` | 函数/方法 | [96–99](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L96-L99) | [已讲解](12-validation.md) |
| `ValidationReport.has_non_exact` | 函数/方法 | [101–104](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L101-L104) | [已讲解](12-validation.md) |
| `PromptAlignmentError` | 类 | [107–108](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L107-L108) | [已讲解](12-validation.md) |
| `AlignmentPolicy` | 类 | [111–121](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L111-L121) | [已讲解](12-validation.md) |
| `_preview` | 函数/方法 | [124–127](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L124-L127) | [已讲解](12-validation.md) |
| `validate_prompt_alignment` | 函数/方法 | [130–220](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L130-L220) | [已讲解](12-validation.md) |
| `handle_alignment_report` | 函数/方法 | [223–269](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompt_validation.py#L223-L269) | [已讲解](12-validation.md) |

## langextract/prompting.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `PromptBuilderError` | 类 | [31–32](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L31-L32) | [已讲解](04-prompt.md) |
| `ParseError` | 类 | [35–36](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L35-L36) | [已讲解](04-prompt.md) |
| `PromptTemplateStructured` | 类 | [39–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L39-L49) | [已讲解](04-prompt.md) |
| `read_prompt_template_structured_from_file` | 函数/方法 | [52–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L52-L81) | [已讲解](04-prompt.md) |
| `QAPromptGenerator` | 类 | [84–138](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L84-L138) | [已讲解](04-prompt.md) |
| `QAPromptGenerator.__str__` | 函数/方法 | [94–96](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L94-L96) | [已讲解](04-prompt.md) |
| `QAPromptGenerator.format_example_as_text` | 函数/方法 | [98–113](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L98-L113) | [已讲解](04-prompt.md) |
| `QAPromptGenerator.render` | 函数/方法 | [115–138](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L115-L138) | [已讲解](04-prompt.md) |
| `PromptBuilder` | 类 | [141–176](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L141-L176) | [已讲解](04-prompt.md) |
| `PromptBuilder.__init__` | 函数/方法 | [148–154](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L148-L154) | [已讲解](04-prompt.md) |
| `PromptBuilder.build_prompt` | 函数/方法 | [156–176](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L156-L176) | [已讲解](04-prompt.md) |
| `ContextAwarePromptBuilder` | 类 | [179–276](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L179-L276) | [已讲解](04-prompt.md) |
| `ContextAwarePromptBuilder.__init__` | 函数/方法 | [193–207](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L193-L207) | [已讲解](04-prompt.md) |
| `ContextAwarePromptBuilder.context_window_chars` | 函数/方法 | [209–212](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L209-L212) | [已讲解](04-prompt.md) |
| `ContextAwarePromptBuilder.build_prompt` | 函数/方法 | [214–240](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L214-L240) | [已讲解](04-prompt.md) |
| `ContextAwarePromptBuilder._build_effective_context` | 函数/方法 | [242–266](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L242-L266) | [已讲解](04-prompt.md) |
| `ContextAwarePromptBuilder._update_state` | 函数/方法 | [268–276](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/prompting.py#L268-L276) | [已讲解](04-prompt.md) |

## langextract/providers/__init__.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `load_builtins_once` | 函数/方法 | [49–68](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/__init__.py#L49-L68) | [已讲解](14-factory.md) |
| `load_plugins_once` | 函数/方法 | [71–139](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/__init__.py#L71-L139) | [已讲解](14-factory.md) |
| `_reset_for_testing` | 函数/方法 | [142–146](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/__init__.py#L142-L146) | [已讲解](14-factory.md) |
| `__getattr__` | 函数/方法 | [149–159](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/__init__.py#L149-L159) | [已讲解](14-factory.md) |

## langextract/providers/builtin_registry.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `ProviderConfig` | 类 | [26–31](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/builtin_registry.py#L26-L31) | [已讲解](14-factory.md) |

## langextract/providers/gemini.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_is_non_bool_integral` | 函数/方法 | [63–65](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L63-L65) | [已讲解](15-providers.md) |
| `_is_non_bool_real` | 函数/方法 | [68–70](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L68-L70) | [已讲解](15-providers.md) |
| `_has_sdk_retry_options` | 函数/方法 | [73–99](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L73-L99) | [已讲解](15-providers.md) |
| `_no_text_diagnostic` | 函数/方法 | [115–167](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L115-L167) | [已讲解](15-providers.md) |
| `GeminiLanguageModel` | 类 | [190–630](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L190-L630) | [已讲解](15-providers.md) |
| `GeminiLanguageModel.get_schema_class` | 函数/方法 | [217–224](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L217-L224) | [已讲解](15-providers.md) |
| `GeminiLanguageModel.apply_schema` | 函数/方法 | [226–243](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L226-L243) | [已讲解](15-providers.md) |
| `GeminiLanguageModel.__init__` | 函数/方法 | [245–383](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L245-L383) | [已讲解](15-providers.md) |
| `GeminiLanguageModel._validate_schema_config` | 函数/方法 | [385–395](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L385-L395) | [已讲解](15-providers.md) |
| `GeminiLanguageModel._is_retryable_error` | 函数/方法 | [397–431](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L397-L431) | [已讲解](15-providers.md) |
| `GeminiLanguageModel._process_single_prompt` | 函数/方法 | [433–482](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L433-L482) | [已讲解](15-providers.md) |
| `GeminiLanguageModel._response_to_scored_output` | 函数/方法 | [484–505](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L484-L505) | [已讲解](15-providers.md) |
| `GeminiLanguageModel.infer` | 函数/方法 | [507–630](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini.py#L507-L630) | [已讲解](15-providers.md) |

## langextract/providers/gemini_batch.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_json_default` | 函数/方法 | [84–94](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L84-L94) | [已讲解](15-batch.md) |
| `BatchConfig` | 类 | [97–169](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L97-L169) | [已讲解](15-batch.md) |
| `BatchConfig.__post_init__` | 函数/方法 | [123–151](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L123-L151) | [已讲解](15-batch.md) |
| `BatchConfig.from_dict` | 函数/方法 | [153–169](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L153-L169) | [已讲解](15-batch.md) |
| `_default_job_create_callback` | 函数/方法 | [183–203](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L183-L203) | [已讲解](15-batch.md) |
| `_snake_to_camel` | 函数/方法 | [206–209](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L206-L209) | [已讲解](15-batch.md) |
| `_is_vertexai_client` | 函数/方法 | [212–221](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L212-L221) | [已讲解](15-batch.md) |
| `_get_project_location` | 函数/方法 | [224–241](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L224-L241) | [已讲解](15-batch.md) |
| `_get_bucket_name` | 函数/方法 | [244–247](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L244-L247) | [已讲解](15-batch.md) |
| `_ensure_bucket_lifecycle` | 函数/方法 | [250–286](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L250-L286) | [已讲解](15-batch.md) |
| `_encode_batch_tools` | 函数/方法 | [289–322](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L289-L322) | [已讲解](15-batch.md) |
| `_build_request` | 函数/方法 | [325–397](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L325-L397) | [已讲解](15-batch.md) |
| `_submit_file` | 函数/方法 | [400–480](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L400-L480) | [已讲解](15-batch.md) |
| `GCSBatchCache` | 类 | [483–585](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L483-L585) | [已讲解](15-batch.md) |
| `GCSBatchCache.__init__` | 函数/方法 | [486–490](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L486-L490) | [已讲解](15-batch.md) |
| `GCSBatchCache._compute_hash` | 函数/方法 | [492–500](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L492-L500) | [已讲解](15-batch.md) |
| `GCSBatchCache._get_single` | 函数/方法 | [502–512](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L502-L512) | [已讲解](15-batch.md) |
| `GCSBatchCache.get_multi` | 函数/方法 | [514–534](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L514-L534) | [已讲解](15-batch.md) |
| `GCSBatchCache.set_multi` | 函数/方法 | [536–566](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L536-L566) | [已讲解](15-batch.md) |
| `GCSBatchCache.set_multi._upload` | 函数/方法 | [543–554](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L543-L554) | [已讲解](15-batch.md) |
| `GCSBatchCache.iter_items` | 函数/方法 | [568–585](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L568-L585) | [已讲解](15-batch.md) |
| `_TextResponse` | 类 | [588–591](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L588-L591) | [已讲解](15-batch.md) |
| `_safe_get_nested` | 函数/方法 | [594–616](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L594-L616) | [已讲解](15-batch.md) |
| `_extract_text` | 函数/方法 | [619–640](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L619-L640) | [已讲解](15-batch.md) |
| `_no_text_block_diagnostic` | 函数/方法 | [643–673](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L643-L673) | [已讲解](15-batch.md) |
| `_poll_completion` | 函数/方法 | [676–720](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L676-L720) | [已讲解](15-batch.md) |
| `_item_failure` | 函数/方法 | [723–739](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L723-L739) | [已讲解](15-batch.md) |
| `_is_max_tokens_finish` | 函数/方法 | [742–752](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L742-L752) | [已讲解](15-batch.md) |
| `_parse_batch_line` | 函数/方法 | [755–790](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L755-L790) | [已讲解](15-batch.md) |
| `_extract_from_file` | 函数/方法 | [793–864](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L793-L864) | [已讲解](15-batch.md) |
| `infer_batch` | 函数/方法 | [867–1078](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L867-L1078) | [已讲解](15-batch.md) |
| `infer_batch._process_batch` | 函数/方法 | [989–1034](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/gemini_batch.py#L989-L1034) | [已讲解](15-batch.md) |

## langextract/providers/ollama.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_is_gpt_oss_model` | 函数/方法 | [119–127](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L119-L127) | [已讲解](15-providers.md) |
| `OllamaLanguageModel` | 类 | [142–658](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L142-L658) | [已讲解](15-providers.md) |
| `OllamaLanguageModel.get_schema_class` | 函数/方法 | [171–178](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L171-L178) | [已讲解](15-providers.md) |
| `OllamaLanguageModel.__repr__` | 函数/方法 | [180–189](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L180-L189) | [已讲解](15-providers.md) |
| `OllamaLanguageModel.__init__` | 函数/方法 | [191–264](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L191-L264) | [已讲解](15-providers.md) |
| `OllamaLanguageModel.infer` | 函数/方法 | [266–319](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L266-L319) | [已讲解](15-providers.md) |
| `OllamaLanguageModel._extract_response_text` | 函数/方法 | [321–339](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L321-L339) | [已讲解](15-providers.md) |
| `OllamaLanguageModel._extract_chat_response_text` | 函数/方法 | [341–365](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L341-L365) | [已讲解](15-providers.md) |
| `OllamaLanguageModel._request_headers` | 函数/方法 | [367–379](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L367-L379) | [已讲解](15-providers.md) |
| `OllamaLanguageModel._build_request_options` | 函数/方法 | [381–438](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L381-L438) | [已讲解](15-providers.md) |
| `OllamaLanguageModel._post_ollama_json` | 函数/方法 | [440–477](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L440-L477) | [已讲解](15-providers.md) |
| `OllamaLanguageModel._ollama_gpt_oss_chat_query` | 函数/方法 | [479–541](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L479-L541) | [已讲解](15-providers.md) |
| `OllamaLanguageModel._ollama_query` | 函数/方法 | [543–658](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/ollama.py#L543-L658) | [已讲解](15-providers.md) |

## langextract/providers/openai.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_is_openai_api` | 函数/方法 | [42–48](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L42-L48) | [已讲解](15-providers.md) |
| `OpenAILanguageModel` | 类 | [51–461](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L51-L461) | [已讲解](15-providers.md) |
| `OpenAILanguageModel.get_schema_class` | 函数/方法 | [77–80](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L77-L80) | [已讲解](15-providers.md) |
| `OpenAILanguageModel.apply_schema` | 函数/方法 | [82–108](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L82-L108) | [已讲解](15-providers.md) |
| `OpenAILanguageModel.requires_fence_output` | 函数/方法 | [110–118](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L110-L118) | [已讲解](15-providers.md) |
| `OpenAILanguageModel.__init__` | 函数/方法 | [120–185](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L120-L185) | [已讲解](15-providers.md) |
| `OpenAILanguageModel._validate_schema_config` | 函数/方法 | [187–192](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L187-L192) | [已讲解](15-providers.md) |
| `OpenAILanguageModel._build_chat_completions_params` | 函数/方法 | [194–263](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L194-L263) | [已讲解](15-providers.md) |
| `OpenAILanguageModel._process_single_prompt` | 函数/方法 | [265–281](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L265-L281) | [已讲解](15-providers.md) |
| `OpenAILanguageModel._response_to_scored_output` | 函数/方法 | [283–331](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L283-L331) | [已讲解](15-providers.md) |
| `OpenAILanguageModel.infer_batch` | 函数/方法 | [333–352](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L333-L352) | [已讲解](15-providers.md) |
| `OpenAILanguageModel.infer` | 函数/方法 | [354–461](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai.py#L354-L461) | [已讲解](15-providers.md) |

## langextract/providers/openai_batch.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `BatchConfig` | 类 | [51–128](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L51-L128) | [已讲解](15-batch.md) |
| `BatchConfig.__post_init__` | 函数/方法 | [83–100](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L83-L100) | [已讲解](15-batch.md) |
| `BatchConfig.from_dict` | 函数/方法 | [102–128](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L102-L128) | [已讲解](15-batch.md) |
| `_custom_id` | 函数/方法 | [131–132](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L131-L132) | [已讲解](15-batch.md) |
| `_field` | 函数/方法 | [135–138](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L135-L138) | [已讲解](15-batch.md) |
| `_format_error` | 函数/方法 | [141–149](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L141-L149) | [已讲解](15-batch.md) |
| `_job_errors` | 函数/方法 | [152–153](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L152-L153) | [已讲解](15-batch.md) |
| `_index_from_custom_id` | 函数/方法 | [156–162](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L156-L162) | [已讲解](15-batch.md) |
| `_extract_text_from_response_body` | 函数/方法 | [165–205](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L165-L205) | [已讲解](15-batch.md) |
| `_content_to_text` | 函数/方法 | [208–238](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L208-L238) | [已讲解](15-batch.md) |
| `_error_status_code` | 函数/方法 | [241–245](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L241-L245) | [已讲解](15-batch.md) |
| `_download_error_message` | 函数/方法 | [248–256](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L248-L256) | [已讲解](15-batch.md) |
| `_load_output_file` | 函数/方法 | [259–280](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L259-L280) | [已讲解](15-batch.md) |
| `_delete_uploaded_input_file` | 函数/方法 | [283–295](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L283-L295) | [已讲解](15-batch.md) |
| `_item_error_text` | 函数/方法 | [298–311](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L298-L311) | [已讲解](15-batch.md) |
| `_collect_outputs_from_jsonl` | 函数/方法 | [314–358](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L314-L358) | [已讲解](15-batch.md) |
| `infer_batch` | 函数/方法 | [361–422](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L361-L422) | [已讲解](15-batch.md) |
| `_infer_batch_one_job` | 函数/方法 | [425–583](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/openai_batch.py#L425-L583) | [已讲解](15-batch.md) |

## langextract/providers/router.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_Entry` | 类 | [40–46](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L40-L46) | [已讲解](14-factory.md) |
| `_add_entry` | 函数/方法 | [55–83](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L55-L83) | [已讲解](14-factory.md) |
| `register_lazy` | 函数/方法 | [86–108](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L86-L108) | [已讲解](14-factory.md) |
| `register_lazy._loader` | 函数/方法 | [98–101](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L98-L101) | [已讲解](14-factory.md) |
| `register` | 函数/方法 | [111–138](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L111-L138) | [已讲解](14-factory.md) |
| `register._decorator` | 函数/方法 | [125–136](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L125-L136) | [已讲解](14-factory.md) |
| `register._decorator._loader` | 函数/方法 | [126–127](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L126-L127) | [已讲解](14-factory.md) |
| `resolve` | 函数/方法 | [141–169](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L141-L169) | [已讲解](14-factory.md) |
| `resolve_provider` | 函数/方法 | [172–217](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L172-L217) | [已讲解](14-factory.md) |
| `clear` | 函数/方法 | [220–226](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L220-L226) | [已讲解](14-factory.md) |
| `list_providers` | 函数/方法 | [229–238](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L229-L238) | [已讲解](14-factory.md) |
| `list_entries` | 函数/方法 | [241–247](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/router.py#L241-L247) | [已讲解](14-factory.md) |

## langextract/providers/schemas/gemini.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `GeminiSchema` | 类 | [33–204](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L33-L204) | [已讲解](13-schema.md) |
| `GeminiSchema.__post_init__` | 函数/方法 | [48–51](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L48-L51) | [已讲解](13-schema.md) |
| `GeminiSchema.schema_dict` | 函数/方法 | [53–56](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L53-L56) | [已讲解](13-schema.md) |
| `GeminiSchema.schema_dict` | 函数/方法 | [58–61](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L58-L61) | [已讲解](13-schema.md) |
| `GeminiSchema.to_provider_config` | 函数/方法 | [63–75](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L63-L75) | [已讲解](13-schema.md) |
| `GeminiSchema.output_schema_reserved_provider_kwargs` | 函数/方法 | [77–84](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L77-L84) | [已讲解](13-schema.md) |
| `GeminiSchema.requires_raw_output` | 函数/方法 | [86–89](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L86-L89) | [已讲解](13-schema.md) |
| `GeminiSchema.validate_format` | 函数/方法 | [91–118](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L91-L118) | [已讲解](13-schema.md) |
| `GeminiSchema.from_examples` | 函数/方法 | [120–193](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L120-L193) | [已讲解](13-schema.md) |
| `GeminiSchema.from_schema_dict` | 函数/方法 | [195–204](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/gemini.py#L195-L204) | [已讲解](13-schema.md) |

## langextract/providers/schemas/openai.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_nullable` | 函数/方法 | [42–48](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L42-L48) | [已讲解](13-schema.md) |
| `_attribute_value_schema` | 函数/方法 | [51–82](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L51-L82) | [已讲解](13-schema.md) |
| `_collect_extraction_categories` | 函数/方法 | [85–109](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L85-L109) | [已讲解](13-schema.md) |
| `_build_extraction_variant` | 函数/方法 | [112–154](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L112-L154) | [已讲解](13-schema.md) |
| `OpenAISchema` | 类 | [157–329](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L157-L329) | [已讲解](13-schema.md) |
| `OpenAISchema.__post_init__` | 函数/方法 | [173–176](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L173-L176) | [已讲解](13-schema.md) |
| `OpenAISchema.response_format` | 函数/方法 | [178–188](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L178-L188) | [已讲解](13-schema.md) |
| `OpenAISchema.to_provider_config` | 函数/方法 | [190–192](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L190-L192) | [已讲解](13-schema.md) |
| `OpenAISchema.output_schema_reserved_provider_kwargs` | 函数/方法 | [194–196](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L194-L196) | [已讲解](13-schema.md) |
| `OpenAISchema.requires_raw_output` | 函数/方法 | [198–201](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L198-L201) | [已讲解](13-schema.md) |
| `OpenAISchema.validate_format` | 函数/方法 | [203–236](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L203-L236) | [已讲解](13-schema.md) |
| `OpenAISchema.from_examples` | 函数/方法 | [238–290](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L238-L290) | [已讲解](13-schema.md) |
| `OpenAISchema.from_schema_dict` | 函数/方法 | [292–329](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/providers/schemas/openai.py#L292-L329) | [已讲解](13-schema.md) |

## langextract/registry.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `__getattr__` | 函数/方法 | [28–36](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/registry.py#L28-L36) | 待讲解 |

## langextract/resolver.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `LcsSpan` | 类 | [46–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L46-L55) | [已讲解](10-fuzzy-alignment.md) |
| `LcsSpan.span_len` | 函数/方法 | [53–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L53-L55) | [已讲解](10-fuzzy-alignment.md) |
| `_is_blank` | 函数/方法 | [93–105](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L93-L105) | [已讲解](06-resolve.md) |
| `AbstractResolver` | 类 | [108–225](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L108-L225) | [已讲解](06-resolve.md) |
| `AbstractResolver.__init__` | 函数/方法 | [112–135](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L112-L135) | [已讲解](06-resolve.md) |
| `AbstractResolver.fence_output` | 函数/方法 | [137–140](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L137-L140) | [已讲解](06-resolve.md) |
| `AbstractResolver.fence_output` | 函数/方法 | [142–149](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L142-L149) | [已讲解](06-resolve.md) |
| `AbstractResolver.format_type` | 函数/方法 | [151–154](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L151-L154) | [已讲解](06-resolve.md) |
| `AbstractResolver.format_type` | 函数/方法 | [156–159](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L156-L159) | [已讲解](06-resolve.md) |
| `AbstractResolver.resolve` | 函数/方法 | [161–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L161-L175) | [已讲解](06-resolve.md) |
| `AbstractResolver.align` | 函数/方法 | [177–225](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L177-L225) | [已讲解](06-resolve.md) |
| `ResolverParsingError` | 类 | [228–229](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L228-L229) | [已讲解](06-resolve.md) |
| `Resolver` | 类 | [232–575](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L232-L575) | [已讲解](06-resolve.md) |
| `Resolver.__init__` | 函数/方法 | [243–294](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L243-L294) | [已讲解](06-resolve.md) |
| `Resolver.resolve` | 函数/方法 | [296–355](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L296-L355) | [已讲解](06-resolve.md) |
| `Resolver.align` | 函数/方法 | [357–430](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L357-L430) | [已讲解](06-resolve.md) |
| `Resolver.string_to_extraction_data` | 函数/方法 | [432–465](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L432-L465) | [已讲解](06-resolve.md) |
| `Resolver.extract_ordered_extractions` | 函数/方法 | [467–575](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L467-L575) | [已讲解](06-resolve.md) |
| `WordAligner` | 类 | [578–1112](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L578-L1112) | [已讲解](09-exact-alignment.md) |
| `WordAligner.__init__` | 函数/方法 | [581–585](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L581-L585) | [已讲解](09-exact-alignment.md) |
| `WordAligner._set_seqs` | 函数/方法 | [587–611](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L587-L611) | [已讲解](09-exact-alignment.md) |
| `WordAligner._get_matching_blocks` | 函数/方法 | [613–628](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L613-L628) | [已讲解](09-exact-alignment.md) |
| `WordAligner._fuzzy_align_extraction` | 函数/方法 | [630–754](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L630-L754) | [已讲解](10-fuzzy-alignment.md) |
| `WordAligner._lcs_fuzzy_align_extraction` | 函数/方法 | [756–826](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L756-L826) | [已讲解](10-fuzzy-alignment.md) |
| `WordAligner.align_extractions` | 函数/方法 | [828–1112](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L828-L1112) | [已讲解](10-fuzzy-alignment.md) |
| `_find_token_occurrences` | 函数/方法 | [1115–1133](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1115-L1133) | [已讲解](09-exact-alignment.md) |
| `_ChainNode` | 类 | [1136–1141](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1136-L1141) | [已讲解](09-exact-alignment.md) |
| `_FrontierEntry` | 类 | [1144–1149](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1144-L1149) | [已讲解](09-exact-alignment.md) |
| `_select_monotonic_matches` | 函数/方法 | [1152–1224](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1152-L1224) | [已讲解](09-exact-alignment.md) |
| `_select_monotonic_matches.best_ending_at_or_before` | 函数/方法 | [1176–1178](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1176-L1178) | [已讲解](09-exact-alignment.md) |
| `_select_monotonic_matches.insert_if_undominated` | 函数/方法 | [1180–1193](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1180-L1193) | [已讲解](09-exact-alignment.md) |
| `_apply_monotonic_exact_matches` | 函数/方法 | [1227–1282](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1227-L1282) | [已讲解](09-exact-alignment.md) |
| `_tokenize_with_lowercase` | 函数/方法 | [1285–1311](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1285-L1311) | [已讲解](09-exact-alignment.md) |
| `_normalize_token` | 函数/方法 | [1314–1320](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1314-L1320) | [已讲解](10-fuzzy-alignment.md) |
| `_best_lcs_spans` | 函数/方法 | [1326–1402](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1326-L1402) | [已讲解](10-fuzzy-alignment.md) |
| `_best_lcs_span` | 函数/方法 | [1405–1413](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1405-L1413) | [已讲解](10-fuzzy-alignment.md) |
| `_accept_lcs_match` | 函数/方法 | [1416–1443](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/resolver.py#L1416-L1443) | [已讲解](10-fuzzy-alignment.md) |

## langextract/schema.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `__getattr__` | 函数/方法 | [57–82](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/schema.py#L57-L82) | 待讲解 |

## langextract/visualization.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `get_ipython` | 函数/方法 | [43–44](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L43-L44) | 待讲解 |
| `_is_jupyter` | 函数/方法 | [49–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L49-L60) | 待讲解 |
| `_assign_colors` | 函数/方法 | [183–197](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L183-L197) | 待讲解 |
| `_filter_valid_extractions` | 函数/方法 | [200–212](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L200-L212) | 待讲解 |
| `TagType` | 类 | [215–219](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L215-L219) | 待讲解 |
| `SpanPoint` | 类 | [222–236](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L222-L236) | 待讲解 |
| `_build_highlighted_text` | 函数/方法 | [239–315](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L239-L315) | 待讲解 |
| `_build_highlighted_text.sort_key` | 函数/方法 | [269–289](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L269-L289) | 待讲解 |
| `_build_legend_html` | 函数/方法 | [318–332](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L318-L332) | 待讲解 |
| `_format_attributes` | 函数/方法 | [335–360](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L335-L360) | 待讲解 |
| `_prepare_extraction_data` | 函数/方法 | [363–418](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L363-L418) | 待讲解 |
| `_to_script_safe_json` | 函数/方法 | [421–431](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L421-L431) | 待讲解 |
| `_build_visualization_html` | 函数/方法 | [434–568](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L434-L568) | 待讲解 |
| `_build_visualization_html._extraction_sort_key` | 函数/方法 | [449–454](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L449-L454) | 待讲解 |
| `visualize` | 函数/方法 | [571–646](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/langextract/visualization.py#L571-L646) | 待讲解 |

## scripts/check_mock_patch_spec.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `find_unspecced_patches` | 函数/方法 | [49–72](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/check_mock_patch_spec.py#L49-L72) | 待讲解 |
| `check_paths` | 函数/方法 | [75–98](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/check_mock_patch_spec.py#L75-L98) | 待讲解 |
| `main` | 函数/方法 | [101–113](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/check_mock_patch_spec.py#L101-L113) | 待讲解 |

## scripts/create_provider_plugin.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_replace_placeholder_line` | 函数/方法 | [267–289](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L267-L289) | 待讲解 |
| `_ensure_safe_output_path` | 函数/方法 | [292–295](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L292-L295) | 待讲解 |
| `_refuse_symlink_destination` | 函数/方法 | [298–304](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L298-L304) | 待讲解 |
| `_write_generated_file` | 函数/方法 | [307–322](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L307-L322) | 待讲解 |
| `_replace_generated_file` | 函数/方法 | [325–337](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L325-L337) | 待讲解 |
| `_docstring_safe` | 函数/方法 | [340–342](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L340-L342) | 待讲解 |
| `_derive_sample_model_id` | 函数/方法 | [345–383](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L345-L383) | 待讲解 |
| `create_directory_structure` | 函数/方法 | [386–439](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L386-L439) | 待讲解 |
| `create_pyproject_toml` | 函数/方法 | [442–478](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L442-L478) | 待讲解 |
| `create_provider` | 函数/方法 | [481–617](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L481-L617) | 待讲解 |
| `create_schema` | 函数/方法 | [620–693](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L620-L693) | 待讲解 |
| `create_test_script` | 函数/方法 | [696–945](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L696-L945) | 待讲解 |
| `create_readme` | 函数/方法 | [948–1035](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L948-L1035) | 待讲解 |
| `create_readme._display` | 函数/方法 | [956–958](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L956-L958) | 待讲解 |
| `create_gitignore` | 函数/方法 | [1038–1094](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1038-L1094) | 待讲解 |
| `create_license` | 函数/方法 | [1097–1101](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1097-L1101) | 待讲解 |
| `_retire_stale_schema` | 函数/方法 | [1104–1126](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1104-L1126) | 待讲解 |
| `_find_spec_or_none` | 函数/方法 | [1129–1139](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1129-L1139) | 待讲解 |
| `_parse_version_tuple` | 函数/方法 | [1142–1147](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1142-L1147) | 待讲解 |
| `_local_backend_problem` | 函数/方法 | [1150–1178](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1150-L1178) | 待讲解 |
| `_pip_config_no_index_enabled` | 函数/方法 | [1181–1202](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1181-L1202) | 待讲解 |
| `_pip_no_index_enabled` | 函数/方法 | [1205–1210](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1205-L1210) | 待讲解 |
| `install_and_test` | 函数/方法 | [1213–1291](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1213-L1291) | 待讲解 |
| `parse_arguments` | 函数/方法 | [1294–1346](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1294-L1346) | 待讲解 |
| `validate_names` | 函数/方法 | [1349–1372](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1349-L1372) | 待讲解 |
| `validate_patterns` | 函数/方法 | [1375–1389](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1375-L1389) | 待讲解 |
| `print_summary` | 函数/方法 | [1392–1416](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1392-L1416) | 待讲解 |
| `_validate_generated_output_paths` | 函数/方法 | [1419–1436](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1419-L1436) | 待讲解 |
| `create_plugin` | 函数/方法 | [1439–1471](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1439-L1471) | 待讲解 |
| `print_completion_summary` | 函数/方法 | [1474–1489](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1474-L1489) | 待讲解 |
| `main` | 函数/方法 | [1492–1527](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/create_provider_plugin.py#L1492-L1527) | 待讲解 |

## scripts/validate_community_providers.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `normalize_pypi` | 函数/方法 | [48–50](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/validate_community_providers.py#L48-L50) | 待讲解 |
| `find_table_bounds` | 函数/方法 | [53–61](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/validate_community_providers.py#L53-L61) | 待讲解 |
| `parse_row` | 函数/方法 | [64–67](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/validate_community_providers.py#L64-L67) | 待讲解 |
| `validate` | 函数/方法 | [70–185](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/validate_community_providers.py#L70-L185) | 待讲解 |
| `print_report` | 函数/方法 | [188–198](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/scripts/validate_community_providers.py#L188-L198) | 待讲解 |

## tests/annotation_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `AnnotatorTest` | 类 | [38–591](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L38-L591) | 待讲解 |
| `AnnotatorTest.setUp` | 函数/方法 | [40–48](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L40-L48) | 待讲解 |
| `AnnotatorTest.assert_char_interval_match_source` | 函数/方法 | [50–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L50-L81) | 待讲解 |
| `AnnotatorTest.test_annotate_text_single_chunk` | 函数/方法 | [83–207](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L83-L207) | 待讲解 |
| `AnnotatorTest.test_annotate_text_without_index_suffix` | 函数/方法 | [209–326](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L209-L326) | 待讲解 |
| `AnnotatorTest.test_annotate_text_with_attributes_suffix` | 函数/方法 | [328–470](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L328-L470) | 待讲解 |
| `AnnotatorTest.test_annotate_text_multiple_chunks` | 函数/方法 | [472–566](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L472-L566) | 待讲解 |
| `AnnotatorTest.test_annotate_text_no_extractions` | 函数/方法 | [568–591](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L568-L591) | 待讲解 |
| `AnnotatorMultipleDocumentTest` | 类 | [594–905](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L594-L905) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_list_outputs` | 函数/方法 | [636–685](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L636-L685) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_list_outputs.TrackedPrompt` | 类 | [637–638](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L637-L638) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_list_outputs.build_prompt` | 函数/方法 | [642–650](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L642-L650) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_list_outputs.FakeLanguageModel` | 类 | [655–661](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L655-L661) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_list_outputs.FakeLanguageModel.infer` | 函数/方法 | [657–661](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L657-L661) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_iterator_outputs` | 函数/方法 | [687–734](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L687-L734) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_iterator_outputs.TrackedPrompt` | 类 | [688–689](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L688-L689) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_iterator_outputs.build_prompt` | 函数/方法 | [693–701](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L693-L701) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_iterator_outputs.FakeLanguageModel` | 类 | [706–710](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L706-L710) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_releases_prompts_before_next_batch_with_iterator_outputs.FakeLanguageModel.infer` | 函数/方法 | [708–710](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L708-L710) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_annotate_documents` | 函数/方法 | [736–849](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L736-L849) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_annotate_documents.mock_infer_side_effect` | 函数/方法 | [806–813](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L806-L813) | 待讲解 |
| `AnnotatorMultipleDocumentTest.test_annotate_documents_exceptions` | 函数/方法 | [851–905](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L851-L905) | 待讲解 |
| `AnnotatorMultiPassTest` | 类 | [908–1089](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L908-L1089) | 待讲解 |
| `AnnotatorMultiPassTest.setUp` | 函数/方法 | [911–919](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L911-L919) | 待讲解 |
| `AnnotatorMultiPassTest.test_multipass_extraction_non_overlapping` | 函数/方法 | [921–969](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L921-L969) | 待讲解 |
| `AnnotatorMultiPassTest.test_multipass_extraction_overlapping` | 函数/方法 | [971–1020](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L971-L1020) | 待讲解 |
| `AnnotatorMultiPassTest.test_multipass_extraction_single_pass` | 函数/方法 | [1022–1050](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1022-L1050) | 待讲解 |
| `AnnotatorMultiPassTest.test_multipass_extraction_empty_passes` | 函数/方法 | [1052–1089](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1052-L1089) | 待讲解 |
| `MultiPassHelperFunctionsTest` | 类 | [1092–1224](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1092-L1224) | 待讲解 |
| `MultiPassHelperFunctionsTest.test_merge_non_overlapping_extractions` | 函数/方法 | [1095–1165](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1095-L1165) | 待讲解 |
| `MultiPassHelperFunctionsTest.test_extractions_overlap` | 函数/方法 | [1167–1224](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1167-L1224) | 待讲解 |
| `AnnotateDocumentsGeneratorTest` | 类 | [1227–1308](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1227-L1308) | 待讲解 |
| `AnnotateDocumentsGeneratorTest.setUp` | 函数/方法 | [1230–1262](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1230-L1262) | 待讲解 |
| `AnnotateDocumentsGeneratorTest.setUp.mock_infer` | 函数/方法 | [1236–1255](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1236-L1255) | 待讲解 |
| `AnnotateDocumentsGeneratorTest.test_yields_documents_not_generators` | 函数/方法 | [1264–1308](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1264-L1308) | 待讲解 |
| `CrossChunkContextTest` | 类 | [1311–1466](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1311-L1466) | 待讲解 |
| `CrossChunkContextTest.setUp` | 函数/方法 | [1314–1322](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1314-L1322) | 待讲解 |
| `CrossChunkContextTest.test_context_window_includes_previous_chunk_text` | 函数/方法 | [1324–1373](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1324-L1373) | 待讲解 |
| `CrossChunkContextTest.test_no_context_included_when_disabled` | 函数/方法 | [1375–1411](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1375-L1411) | 待讲解 |
| `CrossChunkContextTest.test_context_window_per_document_isolation` | 函数/方法 | [1413–1466](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/annotation_test.py#L1413-L1466) | 待讲解 |

## tests/check_mock_patch_spec_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_load_checker` | 函数/方法 | [30–36](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L30-L36) | 待讲解 |
| `FindUnspeccedPatchesTest` | 类 | [42–146](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L42-L146) | 待讲解 |
| `FindUnspeccedPatchesTest.test_flags_call_without_replacement` | 函数/方法 | [44–75](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L44-L75) | 待讲解 |
| `FindUnspeccedPatchesTest.test_does_not_flag` | 函数/方法 | [77–136](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L77-L136) | 待讲解 |
| `FindUnspeccedPatchesTest.test_reports_lines_in_ascending_order` | 函数/方法 | [138–146](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L138-L146) | 待讲解 |
| `CheckPathsTest` | 类 | [149–191](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L149-L191) | 待讲解 |
| `CheckPathsTest.setUp` | 函数/方法 | [151–160](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L151-L160) | 待讲解 |
| `CheckPathsTest.test_unspecced_calls_fail_with_file_fix_and_lines` | 函数/方法 | [162–168](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L162-L168) | 待讲解 |
| `CheckPathsTest.test_specced_calls_pass` | 函数/方法 | [170–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L170-L175) | 待讲解 |
| `CheckPathsTest.test_single_file_is_checked` | 函数/方法 | [177–181](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L177-L181) | 待讲解 |
| `CheckPathsTest.test_files_in_subdirectories_are_checked` | 函数/方法 | [183–191](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L183-L191) | 待讲解 |
| `MainTest` | 类 | [194–218](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L194-L218) | 待讲解 |
| `MainTest.setUp` | 函数/方法 | [196–200](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L196-L200) | 待讲解 |
| `MainTest.test_unspecced_call_exits_with_failure` | 函数/方法 | [202–205](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L202-L205) | 待讲解 |
| `MainTest.test_specced_call_exits_cleanly` | 函数/方法 | [207–212](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L207-L212) | 待讲解 |
| `MainTest.test_missing_path_exits_with_usage_error` | 函数/方法 | [214–215](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L214-L215) | 待讲解 |
| `MainTest.test_no_arguments_exits_with_usage_error` | 函数/方法 | [217–218](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/check_mock_patch_spec_test.py#L217-L218) | 待讲解 |

## tests/chunking_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `SentenceIterTest` | 类 | [26–64](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L26-L64) | 待讲解 |
| `SentenceIterTest.test_basic` | 函数/方法 | [28–57](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L28-L57) | 待讲解 |
| `SentenceIterTest.test_empty` | 函数/方法 | [59–64](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L59-L64) | 待讲解 |
| `ChunkIteratorTest` | 类 | [67–342](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L67-L342) | 待讲解 |
| `ChunkIteratorTest.test_multi_sentence_chunk` | 函数/方法 | [69–94](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L69-L94) | 待讲解 |
| `ChunkIteratorTest.test_sentence_with_multiple_newlines_and_right_interval` | 函数/方法 | [96–109](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L96-L109) | 待讲解 |
| `ChunkIteratorTest.test_break_sentence` | 函数/方法 | [111–162](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L111-L162) | 待讲解 |
| `ChunkIteratorTest.test_long_token_gets_own_chunk` | 函数/方法 | [164–205](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L164-L205) | 待讲解 |
| `ChunkIteratorTest.test_newline_at_chunk_boundary_does_not_create_empty_interval` | 函数/方法 | [207–236](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L207-L236) | 待讲解 |
| `ChunkIteratorTest.test_chunk_unicode_text` | 函数/方法 | [238–259](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L238-L259) | 待讲解 |
| `ChunkIteratorTest.test_newlines_is_secondary_sentence_break` | 函数/方法 | [261–310](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L261-L310) | 待讲解 |
| `ChunkIteratorTest.test_tokenizer_propagation` | 函数/方法 | [312–342](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L312-L342) | 待讲解 |
| `BatchingTest` | 类 | [345–444](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L345-L444) | 待讲解 |
| `BatchingTest.test_make_batches_of_textchunk` | 函数/方法 | [354–444](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L354-L444) | 待讲解 |
| `TextChunkTest` | 类 | [447–466](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L447-L466) | 待讲解 |
| `TextChunkTest.test_string_output` | 函数/方法 | [449–466](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L449-L466) | 待讲解 |
| `TextAdditionalContextTest` | 类 | [469–514](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L469-L514) | 待讲解 |
| `TextAdditionalContextTest.test_text_chunk_additional_context` | 函数/方法 | [473–484](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L473-L484) | 待讲解 |
| `TextAdditionalContextTest.test_chunk_iterator_without_additional_context` | 函数/方法 | [486–495](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L486-L495) | 待讲解 |
| `TextAdditionalContextTest.test_multiple_chunks_with_additional_context` | 函数/方法 | [497–514](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L497-L514) | 待讲解 |
| `TextChunkPropertyTest` | 类 | [517–561](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L517-L561) | 待讲解 |
| `TextChunkPropertyTest.test_text_chunk_properties` | 函数/方法 | [519–561](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/chunking_test.py#L519-L561) | 待讲解 |

## tests/create_provider_plugin_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_load_generator_module` | 函数/方法 | [55–62](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L55-L62) | 待讲解 |
| `_CreateProviderPluginTestBase` | 类 | [65–422](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L65-L422) | 待讲解 |
| `_CreateProviderPluginTestBase.setUp` | 函数/方法 | [67–69](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L67-L69) | 待讲解 |
| `_CreateProviderPluginTestBase._make_tempdir` | 函数/方法 | [71–75](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L71-L75) | 待讲解 |
| `_CreateProviderPluginTestBase._generate_full_plugin` | 函数/方法 | [77–103](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L77-L103) | 待讲解 |
| `_CreateProviderPluginTestBase._generate_provider_source` | 函数/方法 | [105–119](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L105-L119) | 待讲解 |
| `_CreateProviderPluginTestBase._generate_readme_source` | 函数/方法 | [121–132](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L121-L132) | 待讲解 |
| `_CreateProviderPluginTestBase._import_generated_package` | 函数/方法 | [134–160](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L134-L160) | 待讲解 |
| `_CreateProviderPluginTestBase._import_generated_package._purge_modules` | 函数/方法 | [153–156](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L153-L156) | 待讲解 |
| `_CreateProviderPluginTestBase._run_generated_test_script` | 函数/方法 | [162–171](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L162-L171) | 待讲解 |
| `_CreateProviderPluginTestBase._make_fake_install` | 函数/方法 | [173–193](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L173-L193) | 待讲解 |
| `_CreateProviderPluginTestBase._make_dir_alias` | 函数/方法 | [195–202](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L195-L202) | 待讲解 |
| `_CreateProviderPluginTestBase._mutate_installed_provider` | 函数/方法 | [204–211](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L204-L211) | 待讲解 |
| `_CreateProviderPluginTestBase._subprocess_env` | 函数/方法 | [213–220](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L213-L220) | 待讲解 |
| `_CreateProviderPluginTestBase._run_generator_cli` | 函数/方法 | [222–236](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L222-L236) | 待讲解 |
| `_CreateProviderPluginTestBase._readme_usage_model_id` | 函数/方法 | [238–251](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L238-L251) | 待讲解 |
| `_CreateProviderPluginTestBase._run_install_with_hermetic_backend` | 函数/方法 | [253–349](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L253-L349) | 待讲解 |
| `_CreateProviderPluginTestBase._run_install_with_hermetic_backend._fake_find_spec` | 函数/方法 | [298–309](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L298-L309) | 待讲解 |
| `_CreateProviderPluginTestBase._run_install_with_hermetic_backend._fake_metadata_version` | 函数/方法 | [311–316](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L311-L316) | 待讲解 |
| `_CreateProviderPluginTestBase._assert_offline_safe_install` | 函数/方法 | [353–368](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L353-L368) | 待讲解 |
| `_CreateProviderPluginTestBase._assert_fails_before_pip_with_upgrade_hint` | 函数/方法 | [370–390](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L370-L390) | 待讲解 |
| `_CreateProviderPluginTestBase._assert_online_isolated_install` | 函数/方法 | [392–422](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L392-L422) | 待讲解 |
| `GeneratedPluginTest` | 类 | [425–791](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L425-L791) | 待讲解 |
| `GeneratedPluginTest.test_provider_without_schema_is_valid_python` | 函数/方法 | [427–438](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L427-L438) | 待讲解 |
| `GeneratedPluginTest.test_provider_with_schema_is_valid_python` | 函数/方法 | [440–455](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L440-L455) | 待讲解 |
| `GeneratedPluginTest.test_readme_heading_is_at_column_zero` | 函数/方法 | [457–467](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L457-L467) | 待讲解 |
| `GeneratedPluginTest.test_generated_schema_supports_current_base_schema_lifecycle` | 函数/方法 | [469–514](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L469-L514) | 待讲解 |
| `GeneratedPluginTest.test_generated_python_is_formatted` | 函数/方法 | [516–574](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L516-L574) | 待讲解 |
| `GeneratedPluginTest.test_generated_test_script_passes_against_installed_plugin` | 函数/方法 | [576–595](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L576-L595) | 待讲解 |
| `GeneratedPluginTest.test_generated_test_script_exits_nonzero_on_failing_check` | 函数/方法 | [597–613](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L597-L613) | 待讲解 |
| `GeneratedPluginTest.test_generated_test_script_fails_when_plugin_not_installed` | 函数/方法 | [615–628](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L615-L628) | 待讲解 |
| `GeneratedPluginTest.test_generated_test_script_distinguishes_broken_installed_plugin` | 函数/方法 | [630–676](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L630-L676) | 待讲解 |
| `GeneratedPluginTest.test_generated_import_helper_isolates_global_registry` | 函数/方法 | [678–704](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L678-L704) | 待讲解 |
| `GeneratedPluginTest.test_uninstalled_plugin_fails_even_through_symlinked_script_dir` | 函数/方法 | [706–734](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L706-L734) | 待讲解 |
| `GeneratedPluginTest.test_generated_test_script_fails_on_result_count_mismatch` | 函数/方法 | [736–758](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L736-L758) | 待讲解 |
| `GeneratedPluginTest.test_generated_test_script_fails_on_empty_inference_output` | 函数/方法 | [760–782](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L760-L782) | 待讲解 |
| `GeneratedPluginTest.test_readme_usage_snippet_is_valid_python_for_quote_patterns` | 函数/方法 | [784–791](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L784-L791) | 待讲解 |
| `GeneratorCliTest` | 类 | [794–1288](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L794-L1288) | 待讲解 |
| `GeneratorCliTest.test_cli_rejects_invalid_names_without_writing` | 函数/方法 | [796–825](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L796-L825) | 待讲解 |
| `GeneratorCliTest.test_cli_fails_cleanly_when_target_is_plain_file` | 函数/方法 | [827–872](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L827-L872) | 待讲解 |
| `GeneratorCliTest.test_cli_fails_cleanly_on_dangling_symlink_target` | 函数/方法 | [874–926](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L874-L926) | 待讲解 |
| `GeneratorCliTest.test_cli_force_refuses_resolvable_outer_symlink` | 函数/方法 | [928–958](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L928-L958) | 待讲解 |
| `GeneratorCliTest.test_cli_force_fails_cleanly_on_inner_symlink_collision` | 函数/方法 | [960–1029](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L960-L1029) | 待讲解 |
| `GeneratorCliTest.test_cli_force_refuses_resolvable_inner_symlink` | 函数/方法 | [1031–1066](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1031-L1066) | 待讲解 |
| `GeneratorCliTest.test_cli_force_fails_cleanly_when_inner_package_path_is_file` | 函数/方法 | [1068–1118](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1068-L1118) | 待讲解 |
| `GeneratorCliTest.test_cli_force_refuses_file_symlink_before_any_write` | 函数/方法 | [1120–1214](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1120-L1214) | 待讲解 |
| `GeneratorCliTest.test_write_refuses_symlink_created_after_preflight` | 函数/方法 | [1216–1249](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1216-L1249) | 待讲解 |
| `GeneratorCliTest.test_write_refuses_symlink_created_after_preflight._swap_destination` | 函数/方法 | [1230–1232](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1230-L1232) | 待讲解 |
| `GeneratorCliTest.test_hostile_but_valid_patterns_generate_valid_python` | 函数/方法 | [1251–1288](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1251-L1288) | 待讲解 |
| `PluginInstallationTest` | 类 | [1291–1755](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1291-L1755) | 待讲解 |
| `PluginInstallationTest.test_pip_no_index_detects_config_file` | 函数/方法 | [1293–1304](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1293-L1304) | 待讲解 |
| `PluginInstallationTest.test_pip_no_index_environment_overrides_config_file` | 函数/方法 | [1306–1315](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1306-L1315) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_preserves_cwd_and_scopes_subprocess_cwd` | 函数/方法 | [1317–1340](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1317-L1340) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_fails_fast_without_langextract` | 函数/方法 | [1342–1350](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1342-L1350) | 待讲解 |
| `PluginInstallationTest.test_generated_test_script_fails_on_whitespace_only_output` | 函数/方法 | [1352–1381](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1352-L1381) | 待讲解 |
| `PluginInstallationTest.test_generated_plugin_completes_strict_public_extract_call` | 函数/方法 | [1383–1467](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1383-L1467) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_editable_install_is_offline_safe` | 函数/方法 | [1469–1505](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1469-L1505) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_offline_succeeds_with_external_wheel` | 函数/方法 | [1507–1515](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1507-L1515) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_offline_succeeds_without_wheel` | 函数/方法 | [1517–1532](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1517-L1532) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_fails_fast_offline_without_setuptools` | 函数/方法 | [1534–1545](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1534-L1545) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_fails_fast_with_configured_no_index` | 函数/方法 | [1547–1558](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1547-L1558) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_online_isolates_build_without_setuptools` | 函数/方法 | [1560–1566](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1560-L1566) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_fails_fast_offline_with_outdated_setuptools` | 函数/方法 | [1568–1607](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1568-L1607) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_online_isolates_build_with_outdated_setuptools` | 函数/方法 | [1609–1619](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1609-L1619) | 待讲解 |
| `PluginInstallationTest.test_install_failure_surfaces_stdout_diagnostics` | 函数/方法 | [1621–1649](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1621-L1649) | 待讲解 |
| `PluginInstallationTest.test_install_failure_shows_both_streams_without_duplication` | 函数/方法 | [1651–1683](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1651-L1683) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_fails_fast_offline_just_below_minimum` | 函数/方法 | [1685–1703](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1685-L1703) | 待讲解 |
| `PluginInstallationTest.test_install_and_test_treats_backend_probe_errors_as_unavailable` | 函数/方法 | [1705–1755](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1705-L1755) | 待讲解 |
| `RegenerationAndPackagingTest` | 类 | [1758–2422](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1758-L2422) | 待讲解 |
| `RegenerationAndPackagingTest.test_force_regenerate_without_schema_removes_stale_schema_recoverably` | 函数/方法 | [1760–1844](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1760-L1844) | 待讲解 |
| `RegenerationAndPackagingTest.test_force_regenerate_retires_dangling_schema_symlink` | 函数/方法 | [1846–1867](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1846-L1867) | 待讲解 |
| `RegenerationAndPackagingTest.test_readme_example_model_id_matches_first_pattern_when_derivable` | 函数/方法 | [1869–1884](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1869-L1884) | 待讲解 |
| `RegenerationAndPackagingTest.test_readme_has_no_misleading_example_for_underivable_pattern` | 函数/方法 | [1886–1898](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1886-L1898) | 待讲解 |
| `RegenerationAndPackagingTest.test_readme_usage_uses_current_extract_keyword` | 函数/方法 | [1900–1926](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1900-L1926) | 待讲解 |
| `RegenerationAndPackagingTest.test_skipped_pattern_forbids_unqualified_all_checks_passed` | 函数/方法 | [1928–1960](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1928-L1960) | 待讲解 |
| `RegenerationAndPackagingTest.test_generated_license_file_matches_declared_license` | 函数/方法 | [1962–2018](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L1962-L2018) | 待讲解 |
| `RegenerationAndPackagingTest.test_generated_test_script_numbering_is_contiguous` | 函数/方法 | [2020–2041](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L2020-L2041) | 待讲解 |
| `RegenerationAndPackagingTest.test_replace_placeholder_line_replaces_single_occurrence` | 函数/方法 | [2043–2054](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L2043-L2054) | 待讲解 |
| `RegenerationAndPackagingTest.test_replace_placeholder_line_rejects_non_unique` | 函数/方法 | [2056–2065](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L2056-L2065) | 待讲解 |
| `RegenerationAndPackagingTest.test_generated_schema_has_no_dead_extraction_types_computation` | 函数/方法 | [2067–2112](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L2067-L2112) | 待讲解 |
| `RegenerationAndPackagingTest.test_generated_provider_api_key_annotation_allows_none` | 函数/方法 | [2114–2143](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L2114-L2143) | 待讲解 |
| `RegenerationAndPackagingTest.test_pep660_editable_install_passes_and_adjacent_source_still_fails` | 函数/方法 | [2145–2214](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L2145-L2214) | 待讲解 |
| `RegenerationAndPackagingTest.test_generated_wheel_and_sdist_carry_license_without_deprecation` | 函数/方法 | [2216–2422](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/create_provider_plugin_test.py#L2216-L2422) | 待讲解 |

## tests/data_lib_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `DataLibToDictParameterizedTest` | 类 | [30–207](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L30-L207) | 待讲解 |
| `DataLibToDictParameterizedTest.test_annotated_document_to_dict` | 函数/方法 | [39–189](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L39-L189) | 待讲解 |
| `DataLibToDictParameterizedTest.test_annotated_document_to_dict_with_int64` | 函数/方法 | [191–207](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L191-L207) | 待讲解 |
| `_serialized_document` | 函数/方法 | [210–229](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L210-L229) | 待讲解 |
| `DictToAnnotatedDocumentTest` | 类 | [232–284](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L232-L284) | 待讲解 |
| `DictToAnnotatedDocumentTest.test_does_not_mutate_input` | 函数/方法 | [234–239](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L234-L239) | 待讲解 |
| `DictToAnnotatedDocumentTest.test_repeated_call_restores_field` | 函数/方法 | [241–269](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L241-L269) | 待讲解 |
| `DictToAnnotatedDocumentTest.test_result_does_not_share_attributes_with_input` | 函数/方法 | [271–277](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L271-L277) | 待讲解 |
| `DictToAnnotatedDocumentTest.test_missing_extractions_key_loads_as_empty_list` | 函数/方法 | [279–284](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L279-L284) | 待讲解 |
| `AnnotatedDocumentRoundTripTest` | 类 | [287–331](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L287-L331) | 待讲解 |
| `AnnotatedDocumentRoundTripTest.test_extractions_survive_dict_round_trip` | 函数/方法 | [290–313](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L290-L313) | 待讲解 |
| `AnnotatedDocumentRoundTripTest.test_none_extractions_survive_jsonl_round_trip` | 函数/方法 | [315–331](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L315-L331) | 待讲解 |
| `IsUrlTest` | 类 | [334–356](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L334-L356) | 待讲解 |
| `IsUrlTest.test_valid_urls` | 函数/方法 | [337–344](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L337-L344) | 待讲解 |
| `IsUrlTest.test_invalid_urls_with_text` | 函数/方法 | [346–350](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L346-L350) | 待讲解 |
| `IsUrlTest.test_invalid_urls_no_scheme` | 函数/方法 | [352–356](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L352-L356) | 待讲解 |
| `DocumentWithAdditionalContextTest` | 类 | [359–406](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L359-L406) | 待讲解 |
| `DocumentWithAdditionalContextTest.test_returns_new_document_with_overridden_context` | 函数/方法 | [362–367](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L362-L367) | 待讲解 |
| `DocumentWithAdditionalContextTest.test_does_not_mutate_original_context_or_tokenization` | 函数/方法 | [369–373](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L369-L373) | 待讲解 |
| `DocumentWithAdditionalContextTest.test_preserves_explicit_document_id` | 函数/方法 | [375–378](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L375-L378) | 待讲解 |
| `DocumentWithAdditionalContextTest.test_generates_shared_document_id_when_unset` | 函数/方法 | [380–387](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L380-L387) | 待讲解 |
| `DocumentWithAdditionalContextTest.test_preserves_cached_tokenization` | 函数/方法 | [389–393](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L389-L393) | 待讲解 |
| `DocumentWithAdditionalContextTest.test_preserves_both_explicit_id_and_cached_tokenization` | 函数/方法 | [395–400](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L395-L400) | 待讲解 |
| `DocumentWithAdditionalContextTest.test_accepts_none_context` | 函数/方法 | [402–406](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L402-L406) | 待讲解 |
| `TokenizedTextCacheTest` | 类 | [409–441](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L409-L441) | 待讲解 |
| `TokenizedTextCacheTest.test_retokenizes_after_text_changes` | 函数/方法 | [412–425](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L412-L425) | 待讲解 |
| `TokenizedTextCacheTest.test_annotated_document_clears_cache_when_text_becomes_none` | 函数/方法 | [427–433](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L427-L433) | 待讲解 |
| `TokenizedTextCacheTest.test_keeps_tokenization_set_for_current_text` | 函数/方法 | [435–441](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/data_lib_test.py#L435-L441) | 待讲解 |

## tests/extract_precedence_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_mock_language_model` | 函数/方法 | [28–37](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L28-L37) | 待讲解 |
| `ExtractParameterPrecedenceTest` | 类 | [40–458](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L40-L458) | 待讲解 |
| `ExtractParameterPrecedenceTest.setUp` | 函数/方法 | [43–56](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L43-L56) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_model_overrides_all_other_parameters` | 函数/方法 | [58–85](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L58-L85) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_config_overrides_model_id_and_language_model_type` | 函数/方法 | [87–123](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L87-L123) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_model_id_and_base_kwargs_override_language_model_type` | 函数/方法 | [125–163](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L125-L163) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_temperature_merges_into_config_provider_kwargs` | 函数/方法 | [165–194](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L165-L194) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_explicit_temperature_overrides_config_provider_kwargs` | 函数/方法 | [196–220](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L196-L220) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_config_temperature_preserved_without_explicit_temperature` | 函数/方法 | [222–244](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L222-L244) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_temperature_with_model_emits_warning` | 函数/方法 | [246–265](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L246-L265) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_model_without_temperature_emits_no_temperature_warning` | 函数/方法 | [267–287](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L267-L287) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_explicit_temperature_overrides_language_model_params` | 函数/方法 | [289–310](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L289-L310) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_language_model_params_temperature_used_without_explicit_temperature` | 函数/方法 | [312–332](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L312-L332) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_language_model_type_only_emits_warning_and_works` | 函数/方法 | [334–363](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L334-L363) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_language_model_params_forward_retry_knobs` | 函数/方法 | [365–403](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L365-L403) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_use_schema_constraints_warns_with_config` | 函数/方法 | [405–434](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L405-L434) | 待讲解 |
| `ExtractParameterPrecedenceTest.test_use_schema_constraints_warns_with_model` | 函数/方法 | [436–458](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L436-L458) | 待讲解 |
| `ExtractAdditionalContextTest` | 类 | [461–781](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L461-L781) | 待讲解 |
| `ExtractAdditionalContextTest.setUp` | 函数/方法 | [464–477](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L464-L477) | 待讲解 |
| `ExtractAdditionalContextTest._setup_mocks` | 函数/方法 | [479–485](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L479-L485) | 待讲解 |
| `ExtractAdditionalContextTest.test_additional_context_propagated_to_passed_documents` | 函数/方法 | [487–547](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L487-L547) | 待讲解 |
| `ExtractAdditionalContextTest.test_no_additional_context_leaves_documents_unchanged` | 函数/方法 | [549–579](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L549-L579) | 待讲解 |
| `ExtractAdditionalContextTest.test_document_ids_preserved_when_applying_global_context` | 函数/方法 | [581–609](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L581-L609) | 待讲解 |
| `ExtractAdditionalContextTest.test_auto_generated_document_ids_preserved_with_global_context` | 函数/方法 | [611–639](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L611-L639) | 待讲解 |
| `ExtractAdditionalContextTest.test_generator_input_works_with_additional_context` | 函数/方法 | [641–668](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L641-L668) | 待讲解 |
| `ExtractAdditionalContextTest.test_generator_input_works_with_additional_context.doc_generator` | 函数/方法 | [651–653](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L651-L653) | 待讲解 |
| `ExtractAdditionalContextTest.test_caller_documents_keep_context_and_token_cache` | 函数/方法 | [670–699](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L670-L699) | 待讲解 |
| `ExtractAdditionalContextTest.test_pre_tokenized_text_preserved_when_applying_global_context` | 函数/方法 | [701–731](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L701-L731) | 待讲解 |
| `ExtractAdditionalContextTest.test_string_and_document_paths_apply_additional_context_identically` | 函数/方法 | [733–781](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_precedence_test.py#L733-L781) | 待讲解 |

## tests/extract_schema_integration_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `ExtractSchemaIntegrationTest` | 类 | [30–348](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L30-L348) | 待讲解 |
| `ExtractSchemaIntegrationTest.setUp` | 函数/方法 | [33–48](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L33-L48) | 待讲解 |
| `ExtractSchemaIntegrationTest.test_extract_with_gemini_uses_schema` | 函数/方法 | [50–84](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L50-L84) | 待讲解 |
| `ExtractSchemaIntegrationTest.test_extract_forwards_gemini_generation_params` | 函数/方法 | [86–117](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L86-L117) | 待讲解 |
| `ExtractSchemaIntegrationTest.test_extract_with_ollama_uses_json_mode` | 函数/方法 | [119–154](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L119-L154) | 待讲解 |
| `ExtractSchemaIntegrationTest.test_extract_explicit_fence_respected` | 函数/方法 | [156–193](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L156-L193) | 待讲解 |
| `ExtractSchemaIntegrationTest.test_extract_gemini_schema_deprecation_warning` | 函数/方法 | [195–235](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L195-L235) | 待讲解 |
| `ExtractSchemaIntegrationTest.test_extract_no_schema_when_disabled` | 函数/方法 | [237–263](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L237-L263) | 待讲解 |
| `ExtractSchemaIntegrationTest.test_validation_triggers_warning_for_gemini` | 函数/方法 | [265–314](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L265-L314) | 待讲解 |
| `ExtractSchemaIntegrationTest.test_validation_triggers_warning_for_gemini.mock_validate_format` | 函数/方法 | [279–286](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L279-L286) | 待讲解 |
| `ExtractSchemaIntegrationTest.test_no_validation_without_schema` | 函数/方法 | [316–348](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L316-L348) | 待讲解 |
| `ExtractOutputSchemaTest` | 类 | [351–576](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L351-L576) | 待讲解 |
| `ExtractOutputSchemaTest.setUp` | 函数/方法 | [355–360](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L355-L360) | 待讲解 |
| `ExtractOutputSchemaTest._patch_infer` | 函数/方法 | [362–370](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L362-L370) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_with_output_schema_allows_no_examples` | 函数/方法 | [372–388](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L372-L388) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_output_schema_overrides_example_schema` | 函数/方法 | [390–418](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L390-L418) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_requires_examples_without_output_schema` | 函数/方法 | [420–426](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L420-L426) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_with_preconfigured_output_schema_model` | 函数/方法 | [428–440](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L428-L440) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_applies_output_schema_to_plain_model` | 函数/方法 | [442–453](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L442-L453) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_reapplies_same_output_schema_idempotently` | 函数/方法 | [455–468](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L455-L468) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_output_schema_conflicts_with_example_schema_model` | 函数/方法 | [470–496](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L470-L496) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_output_schema_rejects_fence_output` | 函数/方法 | [498–508](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L498-L508) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_output_schema_rejects_yaml_format` | 函数/方法 | [510–520](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L510-L520) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_output_schema_rejects_fenced_resolver_params` | 函数/方法 | [522–532](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L522-L532) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_output_schema_rejects_unwrapped_resolver_output` | 函数/方法 | [534–542](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L534-L542) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_output_schema_rejects_custom_attribute_suffix` | 函数/方法 | [544–552](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L544-L552) | 待讲解 |
| `ExtractOutputSchemaTest.test_extract_fence_conflict_leaves_caller_model_unmodified` | 函数/方法 | [554–576](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/extract_schema_integration_test.py#L554-L576) | 待讲解 |

## tests/factory_schema_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `FactorySchemaIntegrationTest` | 类 | [29–244](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L29-L244) | 待讲解 |
| `FactorySchemaIntegrationTest.setUp` | 函数/方法 | [32–45](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L32-L45) | 待讲解 |
| `FactorySchemaIntegrationTest.test_gemini_with_schema_returns_false_fence` | 函数/方法 | [47–70](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L47-L70) | 待讲解 |
| `FactorySchemaIntegrationTest.test_ollama_with_schema_returns_false_fence` | 函数/方法 | [72–94](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L72-L94) | 待讲解 |
| `FactorySchemaIntegrationTest.test_openai_with_schema_returns_false_fence` | 函数/方法 | [96–111](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L96-L111) | 待讲解 |
| `FactorySchemaIntegrationTest.test_openai_explicit_fence_output_respected` | 函数/方法 | [113–127](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L113-L127) | 待讲解 |
| `FactorySchemaIntegrationTest.test_explicit_fence_output_respected` | 函数/方法 | [129–147](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L129-L147) | 待讲解 |
| `FactorySchemaIntegrationTest.test_no_schema_defaults_to_true_fence` | 函数/方法 | [149–174](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L149-L174) | 待讲解 |
| `FactorySchemaIntegrationTest.test_no_schema_defaults_to_true_fence.NoSchemaModel` | 类 | [152–155](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L152-L155) | 待讲解 |
| `FactorySchemaIntegrationTest.test_no_schema_defaults_to_true_fence.NoSchemaModel.infer` | 函数/方法 | [154–155](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L154-L155) | 待讲解 |
| `FactorySchemaIntegrationTest.test_schema_disabled_returns_true_fence` | 函数/方法 | [176–197](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L176-L197) | 待讲解 |
| `FactorySchemaIntegrationTest.test_caller_overrides_schema_config` | 函数/方法 | [199–221](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L199-L221) | 待讲解 |
| `FactorySchemaIntegrationTest.test_no_examples_no_schema` | 函数/方法 | [223–244](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L223-L244) | 待讲解 |
| `SchemaApplicationTest` | 类 | [247–291](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L247-L291) | 待讲解 |
| `SchemaApplicationTest.test_apply_schema_called_when_supported` | 函数/方法 | [250–291](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L250-L291) | 待讲解 |
| `SchemaApplicationTest.test_apply_schema_called_when_supported.SchemaAwareModel` | 类 | [261–268](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L261-L268) | 待讲解 |
| `SchemaApplicationTest.test_apply_schema_called_when_supported.SchemaAwareModel.get_schema_class` | 函数/方法 | [263–265](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L263-L265) | 待讲解 |
| `SchemaApplicationTest.test_apply_schema_called_when_supported.SchemaAwareModel.infer` | 函数/方法 | [267–268](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L267-L268) | 待讲解 |
| `FactoryOutputSchemaTest` | 类 | [294–406](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L294-L406) | 待讲解 |
| `FactoryOutputSchemaTest.setUp` | 函数/方法 | [301–316](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L301-L316) | 待讲解 |
| `FactoryOutputSchemaTest.test_gemini_output_schema_configures_json_schema` | 函数/方法 | [318–329](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L318-L329) | 待讲解 |
| `FactoryOutputSchemaTest.test_openai_output_schema_configures_response_format` | 函数/方法 | [331–342](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L331-L342) | 待讲解 |
| `FactoryOutputSchemaTest.test_output_schema_overrides_example_schema` | 函数/方法 | [344–356](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L344-L356) | 待讲解 |
| `FactoryOutputSchemaTest.test_output_schema_rejects_provider_schema_kwargs` | 函数/方法 | [358–366](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L358-L366) | 待讲解 |
| `FactoryOutputSchemaTest.test_output_schema_ignores_none_provider_schema_kwargs` | 函数/方法 | [368–378](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L368-L378) | 待讲解 |
| `FactoryOutputSchemaTest.test_output_schema_rejects_fence_output` | 函数/方法 | [380–388](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L380-L388) | 待讲解 |
| `FactoryOutputSchemaTest.test_output_schema_rejects_yaml_format` | 函数/方法 | [390–398](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L390-L398) | 待讲解 |
| `FactoryOutputSchemaTest.test_output_schema_rejects_unsupported_provider` | 函数/方法 | [400–406](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_schema_test.py#L400-L406) | 待讲解 |

## tests/factory_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `OpenAIRoutingExtractionTest` | 类 | [42–141](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L42-L141) | 待讲解 |
| `OpenAIRoutingExtractionTest.setUp` | 函数/方法 | [44–50](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L44-L50) | 待讲解 |
| `OpenAIRoutingExtractionTest.test_reasoning_model_key_precedence` | 函数/方法 | [52–76](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L52-L76) | 待讲解 |
| `OpenAIRoutingExtractionTest.test_unrelated_o_prefix_does_not_receive_openai_key` | 函数/方法 | [78–82](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L78-L82) | 待讲解 |
| `OpenAIRoutingExtractionTest.test_extract_with_environment_key` | 函数/方法 | [84–141](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L84-L141) | 待讲解 |
| `FakeGeminiProvider` | 类 | [144–157](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L144-L157) | 待讲解 |
| `FakeGeminiProvider.__init__` | 函数/方法 | [147–151](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L147-L151) | 待讲解 |
| `FakeGeminiProvider.infer` | 函数/方法 | [153–154](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L153-L154) | 待讲解 |
| `FakeGeminiProvider.infer_batch` | 函数/方法 | [156–157](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L156-L157) | 待讲解 |
| `FakeOpenAIProvider` | 类 | [160–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L160-L175) | 待讲解 |
| `FakeOpenAIProvider.__init__` | 函数/方法 | [163–169](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L163-L169) | 待讲解 |
| `FakeOpenAIProvider.infer` | 函数/方法 | [171–172](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L171-L172) | 待讲解 |
| `FakeOpenAIProvider.infer_batch` | 函数/方法 | [174–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L174-L175) | 待讲解 |
| `ProviderImportErrorTest` | 类 | [178–258](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L178-L258) | 待讲解 |
| `ProviderImportErrorTest.setUp` | 函数/方法 | [181–189](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L181-L189) | 待讲解 |
| `ProviderImportErrorTest.tearDown` | 函数/方法 | [191–193](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L191-L193) | 待讲解 |
| `ProviderImportErrorTest.test_create_model_wraps_import_error` | 函数/方法 | [195–235](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L195-L235) | 待讲解 |
| `ProviderImportErrorTest.test_extract_wraps_import_error` | 函数/方法 | [237–258](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L237-L258) | 待讲解 |
| `FactoryTest` | 类 | [261–632](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L261-L632) | 待讲解 |
| `FactoryTest.setUp` | 函数/方法 | [263–269](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L263-L269) | 待讲解 |
| `FactoryTest.tearDown` | 函数/方法 | [271–273](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L271-L273) | 待讲解 |
| `FactoryTest.test_create_model_basic` | 函数/方法 | [275–284](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L275-L284) | 待讲解 |
| `FactoryTest.test_create_model_from_id` | 函数/方法 | [286–292](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L286-L292) | 待讲解 |
| `FactoryTest.test_uses_gemini_api_key_from_environment` | 函数/方法 | [294–300](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L294-L300) | 待讲解 |
| `FactoryTest.test_uses_openai_api_key_from_environment` | 函数/方法 | [302–308](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L302-L308) | 待讲解 |
| `FactoryTest.test_falls_back_to_langextract_api_key_when_provider_key_missing` | 函数/方法 | [310–318](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L310-L318) | 待讲解 |
| `FactoryTest.test_provider_specific_key_takes_priority_over_langextract_key` | 函数/方法 | [320–332](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L320-L332) | 待讲解 |
| `FactoryTest.test_explicit_kwargs_override_env` | 函数/方法 | [334–342](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L334-L342) | 待讲解 |
| `FactoryTest.test_wraps_provider_initialization_error_in_inference_config_error` | 函数/方法 | [344–353](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L344-L353) | 待讲解 |
| `FactoryTest.test_raises_error_when_no_provider_matches_model_id` | 函数/方法 | [355–362](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L355-L362) | 待讲解 |
| `FactoryTest.test_additional_kwargs_passed_through` | 函数/方法 | [364–379](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L364-L379) | 待讲解 |
| `FactoryTest.test_ollama_uses_base_url_from_environment` | 函数/方法 | [381–402](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L381-L402) | 待讲解 |
| `FactoryTest.test_ollama_uses_base_url_from_environment.FakeOllamaProvider` | 类 | [385–397](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L385-L397) | 待讲解 |
| `FactoryTest.test_ollama_uses_base_url_from_environment.FakeOllamaProvider.__init__` | 函数/方法 | [388–391](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L388-L391) | 待讲解 |
| `FactoryTest.test_ollama_uses_base_url_from_environment.FakeOllamaProvider.infer` | 函数/方法 | [393–394](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L393-L394) | 待讲解 |
| `FactoryTest.test_ollama_uses_base_url_from_environment.FakeOllamaProvider.infer_batch` | 函数/方法 | [396–397](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L396-L397) | 待讲解 |
| `FactoryTest.test_ollama_models_select_without_api_keys` | 函数/方法 | [404–428](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L404-L428) | 待讲解 |
| `FactoryTest.test_ollama_models_select_without_api_keys.FakeOllamaProvider` | 类 | [407–418](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L407-L418) | 待讲解 |
| `FactoryTest.test_ollama_models_select_without_api_keys.FakeOllamaProvider.__init__` | 函数/方法 | [410–412](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L410-L412) | 待讲解 |
| `FactoryTest.test_ollama_models_select_without_api_keys.FakeOllamaProvider.infer` | 函数/方法 | [414–415](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L414-L415) | 待讲解 |
| `FactoryTest.test_ollama_models_select_without_api_keys.FakeOllamaProvider.infer_batch` | 函数/方法 | [417–418](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L417-L418) | 待讲解 |
| `FactoryTest.test_model_config_fields_are_immutable` | 函数/方法 | [430–437](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L430-L437) | 待讲解 |
| `FactoryTest.test_model_config_allows_dict_contents_modification` | 函数/方法 | [439–447](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L439-L447) | 待讲解 |
| `FactoryTest.test_uses_highest_priority_provider_when_multiple_match` | 函数/方法 | [449–469](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L449-L469) | 待讲解 |
| `FactoryTest.test_uses_highest_priority_provider_when_multiple_match.AnotherGeminiProvider` | 类 | [452–464](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L452-L464) | 待讲解 |
| `FactoryTest.test_uses_highest_priority_provider_when_multiple_match.AnotherGeminiProvider.__init__` | 函数/方法 | [455–458](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L455-L458) | 待讲解 |
| `FactoryTest.test_uses_highest_priority_provider_when_multiple_match.AnotherGeminiProvider.infer` | 函数/方法 | [460–461](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L460-L461) | 待讲解 |
| `FactoryTest.test_uses_highest_priority_provider_when_multiple_match.AnotherGeminiProvider.infer_batch` | 函数/方法 | [463–464](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L463-L464) | 待讲解 |
| `FactoryTest.test_explicit_provider_overrides_pattern_matching` | 函数/方法 | [471–494](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L471-L494) | 待讲解 |
| `FactoryTest.test_explicit_provider_overrides_pattern_matching.AnotherProvider` | 类 | [474–486](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L474-L486) | 待讲解 |
| `FactoryTest.test_explicit_provider_overrides_pattern_matching.AnotherProvider.__init__` | 函数/方法 | [477–480](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L477-L480) | 待讲解 |
| `FactoryTest.test_explicit_provider_overrides_pattern_matching.AnotherProvider.infer` | 函数/方法 | [482–483](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L482-L483) | 待讲解 |
| `FactoryTest.test_explicit_provider_overrides_pattern_matching.AnotherProvider.infer_batch` | 函数/方法 | [485–486](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L485-L486) | 待讲解 |
| `FactoryTest.test_provider_without_model_id_uses_provider_default` | 函数/方法 | [496–517](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L496-L517) | 待讲解 |
| `FactoryTest.test_provider_without_model_id_uses_provider_default.DefaultProvider` | 类 | [499–511](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L499-L511) | 待讲解 |
| `FactoryTest.test_provider_without_model_id_uses_provider_default.DefaultProvider.__init__` | 函数/方法 | [502–505](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L502-L505) | 待讲解 |
| `FactoryTest.test_provider_without_model_id_uses_provider_default.DefaultProvider.infer` | 函数/方法 | [507–508](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L507-L508) | 待讲解 |
| `FactoryTest.test_provider_without_model_id_uses_provider_default.DefaultProvider.infer_batch` | 函数/方法 | [510–511](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L510-L511) | 待讲解 |
| `FactoryTest.test_raises_error_when_neither_model_id_nor_provider_specified` | 函数/方法 | [519–528](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L519-L528) | 待讲解 |
| `FactoryTest.test_gemini_vertexai_parameters_accepted` | 函数/方法 | [530–577](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L530-L577) | 待讲解 |
| `FactoryTest.test_gemini_vertexai_parameters_accepted.MockGeminiWithVertexAI` | 类 | [537–559](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L537-L559) | 待讲解 |
| `FactoryTest.test_gemini_vertexai_parameters_accepted.MockGeminiWithVertexAI.__init__` | 函数/方法 | [540–556](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L540-L556) | 待讲解 |
| `FactoryTest.test_gemini_vertexai_parameters_accepted.MockGeminiWithVertexAI.infer` | 函数/方法 | [558–559](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L558-L559) | 待讲解 |
| `FactoryTest.test_gemini_vertexai_with_credentials` | 函数/方法 | [579–609](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L579-L609) | 待讲解 |
| `FactoryTest.test_gemini_vertexai_with_credentials.MockGeminiWithCredentials` | 类 | [586–597](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L586-L597) | 待讲解 |
| `FactoryTest.test_gemini_vertexai_with_credentials.MockGeminiWithCredentials.__init__` | 函数/方法 | [589–594](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L589-L594) | 待讲解 |
| `FactoryTest.test_gemini_vertexai_with_credentials.MockGeminiWithCredentials.infer` | 函数/方法 | [596–597](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L596-L597) | 待讲解 |
| `FactoryTest.test_explicit_provider_loads_builtins_before_resolution` | 函数/方法 | [611–620](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L611-L620) | 待讲解 |
| `FactoryTest.test_explicit_provider_loads_builtins_with_schema_constraints` | 函数/方法 | [622–632](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/factory_test.py#L622-L632) | 待讲解 |

## tests/format_handler_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `FormatHandlerTest` | 类 | [28–254](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L28-L254) | 待讲解 |
| `FormatHandlerTest.test_format_and_parse` | 函数/方法 | [31–145](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L31-L145) | 待讲解 |
| `FormatHandlerTest.test_end_to_end_integration_with_prompt_and_resolver` | 函数/方法 | [147–208](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L147-L208) | 待讲解 |
| `FormatHandlerTest.test_format_parse_roundtrip` | 函数/方法 | [210–254](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L210-L254) | 待讲解 |
| `NonGeminiModelParsingTest` | 类 | [257–370](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L257-L370) | 待讲解 |
| `NonGeminiModelParsingTest.test_think_tags_stripped_before_parsing` | 函数/方法 | [260–274](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L260-L274) | 待讲解 |
| `NonGeminiModelParsingTest.test_top_level_list_accepted_as_fallback` | 函数/方法 | [276–288](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L276-L288) | 待讲解 |
| `NonGeminiModelParsingTest.test_deepseek_r1_real_output` | 函数/方法 | [290–306](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L290-L306) | 待讲解 |
| `NonGeminiModelParsingTest.test_format_type_normalization_in_init_and_resolver_params` | 函数/方法 | [308–363](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L308-L363) | 待讲解 |
| `NonGeminiModelParsingTest.test_invalid_format_type_raises` | 函数/方法 | [365–370](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/format_handler_test.py#L365-L370) | 待讲解 |

## tests/fuzzy_alignment_cases_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_generate_source` | 函数/方法 | [101–104](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L101-L104) | 待讲解 |
| `_plant_span` | 函数/方法 | [107–113](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L107-L113) | 待讲解 |
| `_plant_gapped` | 函数/方法 | [116–122](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L116-L122) | 待讲解 |
| `_run` | 函数/方法 | [125–162](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L125-L162) | 待讲解 |
| `FuzzyAlignmentCasesTest` | 类 | [239–298](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L239-L298) | 待讲解 |
| `FuzzyAlignmentCasesTest.setUp` | 函数/方法 | [242–246](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L242-L246) | 待讲解 |
| `FuzzyAlignmentCasesTest.test_planted_positive` | 函数/方法 | [248–283](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L248-L283) | 待讲解 |
| `FuzzyAlignmentCasesTest.test_planted_negative` | 函数/方法 | [285–298](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L285-L298) | 待讲解 |
| `LcsBestSpanTest` | 类 | [301–356](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L301-L356) | 待讲解 |
| `LcsBestSpanTest.test_best_lcs_span` | 函数/方法 | [304–356](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L304-L356) | 待讲解 |
| `LcsAcceptanceGateTest` | 类 | [359–414](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L359-L414) | 待讲解 |
| `LcsAcceptanceGateTest.test_accept_lcs_match` | 函数/方法 | [362–414](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L362-L414) | 待讲解 |
| `LcsFuzzyAlignmentEdgeCasesTest` | 类 | [417–538](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L417-L538) | 待讲解 |
| `LcsFuzzyAlignmentEdgeCasesTest.setUp` | 函数/方法 | [420–424](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L420-L424) | 待讲解 |
| `LcsFuzzyAlignmentEdgeCasesTest.test_repeated_token_selects_min_span` | 函数/方法 | [426–446](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L426-L446) | 待讲解 |
| `LcsFuzzyAlignmentEdgeCasesTest.test_density_gate_rejects_over_sparse` | 函数/方法 | [448–460](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L448-L460) | 待讲解 |
| `LcsFuzzyAlignmentEdgeCasesTest.test_algorithm_switch_legacy_rejects_lcs_accepts` | 函数/方法 | [462–489](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L462-L489) | 待讲解 |
| `LcsFuzzyAlignmentEdgeCasesTest.test_sparse_max_match_falls_back_to_dense_submatch` | 函数/方法 | [491–514](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L491-L514) | 待讲解 |
| `LcsFuzzyAlignmentEdgeCasesTest.test_offsets_are_propagated` | 函数/方法 | [516–538](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L516-L538) | 待讲解 |
| `PositionalCallCompatTest` | 类 | [541–579](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L541-L579) | 待讲解 |
| `PositionalCallCompatTest.test_align_extractions_positional` | 函数/方法 | [544–566](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L544-L566) | 待讲解 |
| `PositionalCallCompatTest.test_alignment_policy_positional` | 函数/方法 | [568–574](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L568-L574) | 待讲解 |
| `PositionalCallCompatTest.test_alignment_policy_rejects_fourth_positional` | 函数/方法 | [576–579](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L576-L579) | 待讲解 |
| `ParameterValidationTest` | 类 | [582–652](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L582-L652) | 待讲解 |
| `ParameterValidationTest.setUp` | 函数/方法 | [585–590](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L585-L590) | 待讲解 |
| `ParameterValidationTest.test_invalid_params_raise` | 函数/方法 | [592–612](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L592-L612) | 待讲解 |
| `ParameterValidationTest.test_disabled_fuzzy_skips_param_validation` | 函数/方法 | [614–622](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L614-L622) | 待讲解 |
| `ParameterValidationTest.test_disabled_fuzzy_skips_deprecation_warning` | 函数/方法 | [624–637](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L624-L637) | 待讲解 |
| `ParameterValidationTest.test_legacy_dispatch_warns` | 函数/方法 | [639–652](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/fuzzy_alignment_cases_test.py#L639-L652) | 待讲解 |

## tests/gemini_retry_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_make_fake_httpx_module` | 函数/方法 | [39–59](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L39-L59) | 待讲解 |
| `_make_fake_httpx_module.FakeTimeoutException` | 类 | [42–43](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L42-L43) | 待讲解 |
| `_make_fake_httpx_module.FakeNetworkError` | 类 | [45–46](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L45-L46) | 待讲解 |
| `_make_fake_httpx_module.FakeRemoteProtocolError` | 类 | [48–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L48-L49) | 待讲解 |
| `_make_fake_httpx_module.FakeProxyError` | 类 | [51–52](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L51-L52) | 待讲解 |
| `_make_response` | 函数/方法 | [62–68](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L62-L68) | 待讲解 |
| `_build_model` | 函数/方法 | [71–75](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L71-L75) | 待讲解 |
| `_MockClientTest` | 类 | [78–86](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L78-L86) | 待讲解 |
| `_MockClientTest.setUp` | 函数/方法 | [81–86](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L81-L86) | 待讲解 |
| `TestGeminiRetryableErrors` | 类 | [89–226](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L89-L226) | 待讲解 |
| `TestGeminiRetryableErrors.setUp` | 函数/方法 | [92–94](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L92-L94) | 待讲解 |
| `TestGeminiRetryableErrors.test_retryable_by_message` | 函数/方法 | [96–127](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L96-L127) | 待讲解 |
| `TestGeminiRetryableErrors.test_not_retryable_by_message` | 函数/方法 | [129–145](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L129-L145) | 待讲解 |
| `TestGeminiRetryableErrors.test_generic_value_error_is_not_retryable` | 函数/方法 | [147–148](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L147-L148) | 待讲解 |
| `TestGeminiRetryableErrors.test_stdlib_network_errors_are_retryable` | 函数/方法 | [150–161](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L150-L161) | 待讲解 |
| `TestGeminiRetryableErrors.test_bare_os_error_is_not_retryable` | 函数/方法 | [163–165](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L163-L165) | 待讲解 |
| `TestGeminiRetryableErrors.test_typed_apierror_retryable_codes` | 函数/方法 | [167–177](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L167-L177) | 待讲解 |
| `TestGeminiRetryableErrors.test_typed_apierror_terminal_codes` | 函数/方法 | [179–187](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L179-L187) | 待讲解 |
| `TestGeminiRetryableErrors.test_httpx_transport_errors_are_retryable` | 函数/方法 | [189–212](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L189-L212) | 待讲解 |
| `TestGeminiRetryableErrors.test_httpx_client_bugs_are_not_retryable` | 函数/方法 | [214–226](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L214-L226) | 待讲解 |
| `TestGeminiHttpxTransportResolution` | 类 | [229–259](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L229-L259) | 待讲解 |
| `TestGeminiHttpxTransportResolution.setUp` | 函数/方法 | [232–237](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L232-L237) | 待讲解 |
| `TestGeminiHttpxTransportResolution.test_falls_back_to_httpx2_when_httpx_missing` | 函数/方法 | [239–245](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L239-L245) | 待讲解 |
| `TestGeminiHttpxTransportResolution.test_checks_httpx2_when_httpx_also_installed` | 函数/方法 | [247–254](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L247-L254) | 待讲解 |
| `TestGeminiHttpxTransportResolution.test_missing_httpx_modules_still_retries_stdlib_errors` | 函数/方法 | [256–259](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L256-L259) | 待讲解 |
| `TestGeminiRetryLogic` | 类 | [262–375](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L262-L375) | 待讲解 |
| `TestGeminiRetryLogic.test_retry_then_success` | 函数/方法 | [265–281](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L265-L281) | 待讲解 |
| `TestGeminiRetryLogic.test_retry_multiple_times_before_success` | 函数/方法 | [283–296](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L283-L296) | 待讲解 |
| `TestGeminiRetryLogic.test_exponential_backoff_is_jittered` | 函数/方法 | [298–317](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L298-L317) | 待讲解 |
| `TestGeminiRetryLogic.test_max_retry_delay_caps_post_jitter_sleep` | 函数/方法 | [319–333](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L319-L333) | 待讲解 |
| `TestGeminiRetryLogic._assert_in_range` | 函数/方法 | [335–337](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L335-L337) | 待讲解 |
| `TestGeminiRetryLogic.test_max_retries_exhausted_raises` | 函数/方法 | [339–351](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L339-L351) | 待讲解 |
| `TestGeminiRetryLogic.test_terminal_errors_not_retried` | 函数/方法 | [353–364](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L353-L364) | 待讲解 |
| `TestGeminiRetryLogic.test_zero_max_retries_disables_retry` | 函数/方法 | [366–375](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L366-L375) | 待讲解 |
| `TestGeminiParallelRetry` | 类 | [378–425](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L378-L425) | 待讲解 |
| `TestGeminiParallelRetry.test_single_chunk_retries_without_failing_batch` | 函数/方法 | [381–399](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L381-L399) | 待讲解 |
| `TestGeminiParallelRetry.test_single_chunk_retries_without_failing_batch.side_effect` | 函数/方法 | [387–391](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L387-L391) | 待讲解 |
| `TestGeminiParallelRetry.test_all_succeed_never_sleeps` | 函数/方法 | [401–410](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L401-L410) | 待讲解 |
| `TestGeminiParallelRetry.test_permanent_error_fails_batch` | 函数/方法 | [412–425](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L412-L425) | 待讲解 |
| `TestGeminiParallelRetry.test_permanent_error_fails_batch.side_effect` | 函数/方法 | [416–419](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L416-L419) | 待讲解 |
| `TestGeminiRetryConfiguration` | 类 | [428–500](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L428-L500) | 待讲解 |
| `TestGeminiRetryConfiguration.test_default_retry_parameters` | 函数/方法 | [431–435](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L431-L435) | 待讲解 |
| `TestGeminiRetryConfiguration.test_custom_retry_parameters` | 函数/方法 | [437–441](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L437-L441) | 待讲解 |
| `TestGeminiRetryConfiguration.test_vertex_ai_with_retry_parameters` | 函数/方法 | [443–453](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L443-L453) | 待讲解 |
| `TestGeminiRetryConfiguration.test_invalid_knobs_raise` | 函数/方法 | [455–500](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L455-L500) | 待讲解 |
| `_http_options_object` | 函数/方法 | [503–507](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L503-L507) | 待讲解 |
| `TestGeminiHttpOptionsRetryGuard` | 类 | [510–587](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L510-L587) | 待讲解 |
| `TestGeminiHttpOptionsRetryGuard.test_raises_when_sdk_retries_effective` | 函数/方法 | [518–546](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L518-L546) | 待讲解 |
| `TestGeminiHttpOptionsRetryGuard.test_allowed_when_sdk_retries_noop` | 函数/方法 | [548–577](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L548-L577) | 待讲解 |
| `TestGeminiHttpOptionsRetryGuard.test_allowed_when_sdk_retries_disabled_via_max_retries` | 函数/方法 | [579–582](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L579-L582) | 待讲解 |
| `TestGeminiHttpOptionsRetryGuard.test_allowed_when_http_options_has_no_retry` | 函数/方法 | [584–587](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L584-L587) | 待讲解 |
| `_make_no_text_response` | 函数/方法 | [590–620](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L590-L620) | 待讲解 |
| `TestGeminiNoTextResponse` | 类 | [623–919](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L623-L919) | 待讲解 |
| `TestGeminiNoTextResponse.setUp` | 函数/方法 | [626–628](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L626-L628) | 待讲解 |
| `TestGeminiNoTextResponse.test_text_is_read_once` | 函数/方法 | [630–651](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L630-L651) | 待讲解 |
| `TestGeminiNoTextResponse.test_blocked_prompt_raises_with_block_reason` | 函数/方法 | [653–672](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L653-L672) | 待讲解 |
| `TestGeminiNoTextResponse.test_non_stop_finish_reason_raises` | 函数/方法 | [674–689](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L674-L689) | 待讲解 |
| `TestGeminiNoTextResponse.test_exhausted_token_budget_raises_with_hint` | 函数/方法 | [691–706](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L691-L706) | 待讲解 |
| `TestGeminiNoTextResponse.test_empty_text_with_normal_stop_passes_through` | 函数/方法 | [708–724](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L708-L724) | 待讲解 |
| `TestGeminiNoTextResponse.test_extract_accepts_empty_text_with_normal_stop` | 函数/方法 | [726–759](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L726-L759) | 待讲解 |
| `TestGeminiNoTextResponse.test_non_text_parts_raise` | 函数/方法 | [761–771](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L761-L771) | 待讲解 |
| `TestGeminiNoTextResponse.test_no_diagnostic_raises_generic_message` | 函数/方法 | [773–781](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L773-L781) | 待讲解 |
| `TestGeminiNoTextResponse.test_blocked_prompt_without_message_preserves_error_without_retry` | 函数/方法 | [783–794](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L783-L794) | 待讲解 |
| `TestGeminiNoTextResponse.test_unspecified_block_reason_is_not_reported_as_blocked` | 函数/方法 | [796–807](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L796-L807) | 待讲解 |
| `TestGeminiNoTextResponse.test_unspecified_finish_reason_allows_empty_text` | 函数/方法 | [809–819](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L809-L819) | 待讲解 |
| `TestGeminiNoTextResponse.test_none_text_with_stop_finish_reason_names_the_reason` | 函数/方法 | [821–830](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L821-L830) | 待讲解 |
| `TestGeminiNoTextResponse.test_truncated_text_is_returned_with_warning` | 函数/方法 | [832–845](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L832-L845) | 待讲解 |
| `TestGeminiNoTextResponse.test_complete_text_does_not_warn` | 函数/方法 | [847–857](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L847-L857) | 待讲解 |
| `TestGeminiNoTextResponse.test_parallel_infer_preserves_no_text_error` | 函数/方法 | [859–870](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L859-L870) | 待讲解 |
| `TestGeminiNoTextResponse.test_extract_propagates_thought_only_response` | 函数/方法 | [872–919](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/gemini_retry_test.py#L872-L919) | 待讲解 |

## tests/inference_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `TestBaseLanguageModel` | 类 | [41–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L41-L91) | 待讲解 |
| `TestBaseLanguageModel.test_merge_kwargs_with_none` | 函数/方法 | [43–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L43-L73) | 待讲解 |
| `TestBaseLanguageModel.test_merge_kwargs_with_none.TestModel` | 类 | [46–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L46-L49) | 待讲解 |
| `TestBaseLanguageModel.test_merge_kwargs_with_none.TestModel.infer` | 函数/方法 | [48–49](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L48-L49) | 待讲解 |
| `TestBaseLanguageModel.test_merge_kwargs_without_extra_kwargs` | 函数/方法 | [75–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L75-L91) | 待讲解 |
| `TestBaseLanguageModel.test_merge_kwargs_without_extra_kwargs.TestModel` | 类 | [78–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L78-L81) | 待讲解 |
| `TestBaseLanguageModel.test_merge_kwargs_without_extra_kwargs.TestModel.infer` | 函数/方法 | [80–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L80-L81) | 待讲解 |
| `TestOllamaLanguageModel` | 类 | [94–613](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L94-L613) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_infer` | 函数/方法 | [96–170](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L96-L170) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_infer_prefers_response_over_thinking` | 函数/方法 | [172–215](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L172-L215) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_infer_raises_on_empty_response_with_thinking` | 函数/方法 | [217–242](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L217-L242) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_infer_raises_when_response_missing` | 函数/方法 | [244–265](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L244-L265) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_infer_preserves_explicit_think` | 函数/方法 | [267–292](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L267-L292) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_infer_default_think_does_not_mutate_stored_kwargs` | 函数/方法 | [294–317](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L294-L317) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_gpt_oss_model_matching` | 函数/方法 | [319–331](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L319-L331) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_chat_empty_content_with_thinking_raises` | 函数/方法 | [333–337](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L333-L337) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_chat_missing_content_raises` | 函数/方法 | [339–343](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L339-L343) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_gpt_oss_yaml_uses_generate_path` | 函数/方法 | [345–372](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L345-L372) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_gpt_oss_uses_chat_without_native_json` | 函数/方法 | [374–423](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L374-L423) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_extra_kwargs_passed_to_api` | 函数/方法 | [425–459](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L425-L459) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_stop_and_top_p_passthrough` | 函数/方法 | [461–487](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L461-L487) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_defaults_when_unspecified` | 函数/方法 | [489–513](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L489-L513) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_runtime_kwargs_override_stored` | 函数/方法 | [515–540](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L515-L540) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_temperature_zero` | 函数/方法 | [542–564](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L542-L564) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_default_timeout` | 函数/方法 | [566–588](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L566-L588) | 待讲解 |
| `TestOllamaLanguageModel.test_ollama_timeout_through_infer` | 函数/方法 | [590–613](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L590-L613) | 待讲解 |
| `TestGeminiLanguageModel` | 类 | [616–893](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L616-L893) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_allowlist_filtering` | 函数/方法 | [618–673](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L618-L673) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_runtime_generation_params_override_constructor` | 函数/方法 | [675–705](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L675-L705) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_runtime_none_clears_constructor_generation_params` | 函数/方法 | [707–736](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L707-L736) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_runtime_none_clears_only_selected_generation_param` | 函数/方法 | [738–761](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L738-L761) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_runtime_kwargs_filtered` | 函数/方法 | [763–809](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L763-L809) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_requires_auth_config` | 函数/方法 | [811–818](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L811-L818) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_vertexai_requires_project_and_location` | 函数/方法 | [820–825](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L820-L825) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_vertexai_initialization` | 函数/方法 | [827–848](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L827-L848) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_warns_when_both_auth_provided` | 函数/方法 | [850–869](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L850-L869) | 待讲解 |
| `TestGeminiLanguageModel.test_gemini_vertexai_with_http_options` | 函数/方法 | [871–893](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L871-L893) | 待讲解 |
| `TestOpenAILanguageModelInference` | 类 | [896–1073](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L896-L1073) | 待讲解 |
| `TestOpenAILanguageModelInference.test_openai_infer_with_parameters` | 函数/方法 | [898–943](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L898-L943) | 待讲解 |
| `TestOpenAILanguageModelInference.test_max_output_tokens_request_parameter` | 函数/方法 | [945–1032](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L945-L1032) | 待讲解 |
| `TestOpenAILanguageModelInference.test_truncated_output_warns_on_length_finish_reason` | 函数/方法 | [1034–1054](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1034-L1054) | 待讲解 |
| `TestOpenAILanguageModelInference.test_complete_output_does_not_warn_on_stop_finish_reason` | 函数/方法 | [1056–1073](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1056-L1073) | 待讲解 |
| `TestOpenAILanguageModel` | 类 | [1076–1343](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1076-L1343) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_parse_output_json` | 函数/方法 | [1078–1089](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1078-L1089) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_parse_output_yaml` | 函数/方法 | [1091–1102](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1091-L1102) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_no_api_key_raises_error` | 函数/方法 | [1104–1107](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1104-L1107) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_extra_kwargs_passed` | 函数/方法 | [1109–1136](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1109-L1136) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_runtime_kwargs_override` | 函数/方法 | [1138–1162](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1138-L1162) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_json_response_format` | 函数/方法 | [1164–1189](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1164-L1189) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_temperature_zero` | 函数/方法 | [1191–1214](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1191-L1214) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_temperature_none_not_sent` | 函数/方法 | [1216–1240](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1216-L1240) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_none_values_filtered` | 函数/方法 | [1242–1266](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1242-L1266) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_no_system_message_when_not_json_yaml` | 函数/方法 | [1268–1295](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1268-L1295) | 待讲解 |
| `TestOpenAILanguageModel.test_openai_reasoning_effort_passed_directly` | 函数/方法 | [1297–1321](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1297-L1321) | 待讲解 |
| `TestOpenAILanguageModel.test_gemini_none_values_filtered` | 函数/方法 | [1323–1343](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/inference_test.py#L1323-L1343) | 待讲解 |

## tests/init_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `InitTest` | 类 | [34–886](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L34-L886) | 待讲解 |
| `InitTest.test_lang_extract_as_lx_extract` | 函数/方法 | [37–156](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L37-L156) | 待讲解 |
| `InitTest.test_extract_legacy_yaml_format_without_fences` | 函数/方法 | [158–201](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L158-L201) | 待讲解 |
| `InitTest.test_extract_legacy_yaml_format_without_fences.FakeModel` | 类 | [165–174](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L165-L174) | 待讲解 |
| `InitTest.test_extract_legacy_yaml_format_without_fences.FakeModel.infer` | 函数/方法 | [167–174](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L167-L174) | 待讲解 |
| `InitTest.test_extract_without_prompt_description_omits_it_from_prompt` | 函数/方法 | [203–231](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L203-L231) | 待讲解 |
| `InitTest.test_extract_without_prompt_description_omits_it_from_prompt.FakeModel` | 类 | [206–215](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L206-L215) | 待讲解 |
| `InitTest.test_extract_without_prompt_description_omits_it_from_prompt.FakeModel.infer` | 函数/方法 | [208–215](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L208-L215) | 待讲解 |
| `InitTest.test_extract_resolver_params_alignment_passthrough` | 函数/方法 | [233–278](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L233-L278) | 待讲解 |
| `InitTest.test_extract_resolver_params_suppress_parse_errors` | 函数/方法 | [280–323](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L280-L323) | 待讲解 |
| `InitTest.test_extract_suppress_parse_errors_routing` | 函数/方法 | [325–374](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L325-L374) | 待讲解 |
| `InitTest.test_extract_resolver_params_none_handling` | 函数/方法 | [376–428](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L376-L428) | 待讲解 |
| `InitTest.test_extract_resolver_params_typo_error` | 函数/方法 | [430–459](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L430-L459) | 待讲解 |
| `InitTest.test_extract_resolver_params_docs_path_passthrough` | 函数/方法 | [461–504](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L461-L504) | 待讲解 |
| `InitTest.test_extract_resolver_params_none_threshold` | 函数/方法 | [506–545](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L506-L545) | 待讲解 |
| `InitTest.test_extract_custom_params_reach_inference` | 函数/方法 | [547–593](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L547-L593) | 待讲解 |
| `InitTest.test_extract_with_custom_tokenizer` | 函数/方法 | [595–656](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L595-L656) | 待讲解 |
| `InitTest.test_extract_with_custom_tokenizer.mock_tokenize` | 函数/方法 | [609–631](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L609-L631) | 待讲解 |
| `InitTest.test_data_module_exports_via_compatibility_shim` | 函数/方法 | [658–675](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L658-L675) | 待讲解 |
| `InitTest.test_tokenizer_module_exports_via_compatibility_shim` | 函数/方法 | [677–698](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L677-L698) | 待讲解 |
| `InitTest.test_show_progress_controls_progress_bar` | 函数/方法 | [700–778](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L700-L778) | 待讲解 |
| `InitTest.test_schema_validation_warning_issued` | 函数/方法 | [780–830](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L780-L830) | 待讲解 |
| `InitTest.test_schema_validation_warning_issued.validate_format_side_effect` | 函数/方法 | [791–792](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L791-L792) | 待讲解 |
| `InitTest.test_gemini_schema_deprecation_warning` | 函数/方法 | [832–886](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L832-L886) | 待讲解 |
| `AnnotationSuppressParseErrorsTest` | 类 | [889–1016](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L889-L1016) | 待讲解 |
| `AnnotationSuppressParseErrorsTest.setUp` | 函数/方法 | [892–918](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L892-L918) | 待讲解 |
| `AnnotationSuppressParseErrorsTest.test_suppress_parse_errors_not_sent_to_infer` | 函数/方法 | [920–934](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L920-L934) | 待讲解 |
| `AnnotationSuppressParseErrorsTest.test_alignment_params_reach_resolver_not_infer` | 函数/方法 | [936–960](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L936-L960) | 待讲解 |
| `AnnotationSuppressParseErrorsTest.test_warning_excludes_raw_chunk_text` | 函数/方法 | [962–981](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L962-L981) | 待讲解 |
| `AnnotationSuppressParseErrorsTest.test_mixed_chunks_valid_extractions_survive` | 函数/方法 | [983–1003](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L983-L1003) | 待讲解 |
| `AnnotationSuppressParseErrorsTest.test_mixed_chunks_raises_when_suppression_disabled` | 函数/方法 | [1005–1016](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L1005-L1016) | 待讲解 |
| `FetchUrlsOptInTest` | 类 | [1019–1059](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L1019-L1059) | 待讲解 |
| `FetchUrlsOptInTest.setUp` | 函数/方法 | [1022–1029](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L1022-L1029) | 待讲解 |
| `FetchUrlsOptInTest._extract` | 函数/方法 | [1031–1040](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L1031-L1040) | 待讲解 |
| `FetchUrlsOptInTest.test_url_is_not_fetched_by_default` | 函数/方法 | [1042–1050](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L1042-L1050) | 待讲解 |
| `FetchUrlsOptInTest.test_fetch_urls_true_invokes_downloader` | 函数/方法 | [1052–1059](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/init_test.py#L1052-L1059) | 待讲解 |

## tests/io_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_ClosingTracker` | 类 | [30–40](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L30-L40) | 待讲解 |
| `_ClosingTracker.__init__` | 函数/方法 | [32–34](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L32-L34) | 待讲解 |
| `_ClosingTracker.update` | 函数/方法 | [36–37](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L36-L37) | 待讲解 |
| `_ClosingTracker.close` | 函数/方法 | [39–40](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L39-L40) | 待讲解 |
| `IoTest` | 类 | [43–152](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L43-L152) | 待讲解 |
| `IoTest.test_save_annotated_documents_closes_progress_bar_on_generator_error` | 函数/方法 | [45–70](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L45-L70) | 待讲解 |
| `IoTest.test_save_annotated_documents_closes_progress_bar_on_generator_error.annotated_documents` | 函数/方法 | [50–54](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L50-L54) | 待讲解 |
| `IoTest.test_download_text_from_url_closes_progress_bar_on_stream_error` | 函数/方法 | [72–104](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L72-L104) | 待讲解 |
| `IoTest.test_download_text_from_url_closes_progress_bar_on_stream_error.broken_iter_content` | 函数/方法 | [83–86](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L83-L86) | 待讲解 |
| `IoTest.test_load_annotated_documents_closes_progress_bar_on_malformed_line` | 函数/方法 | [106–124](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L106-L124) | 待讲解 |
| `IoTest.test_load_annotated_documents_closes_progress_bar_on_generator_close` | 函数/方法 | [126–152](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L126-L152) | 待讲解 |
| `_load_csv` | 函数/方法 | [155–161](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L155-L161) | 待讲解 |
| `DatasetTest` | 类 | [164–206](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L164-L206) | 待讲解 |
| `DatasetTest.test_load_preserves_literal_strings` | 函数/方法 | [166–179](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L166-L179) | 待讲解 |
| `DatasetTest.test_load_reads_empty_or_missing_text_as_empty_string` | 函数/方法 | [181–191](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L181-L191) | 待讲解 |
| `DatasetTest.test_load_generates_unique_document_ids_for_empty_ids` | 函数/方法 | [193–199](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L193-L199) | 待讲解 |
| `DatasetTest.test_load_with_custom_delimiter_preserves_literal_strings` | 函数/方法 | [201–206](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/io_test.py#L201-L206) | 待讲解 |

## tests/openai_batch_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_FakeFiles` | 类 | [32–50](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L32-L50) | 待讲解 |
| `_FakeFiles.__init__` | 函数/方法 | [34–37](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L34-L37) | 待讲解 |
| `_FakeFiles.create` | 函数/方法 | [39–41](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L39-L41) | 待讲解 |
| `_FakeFiles.content` | 函数/方法 | [43–44](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L43-L44) | 待讲解 |
| `_FakeFiles.delete` | 函数/方法 | [46–47](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L46-L47) | 待讲解 |
| `_FakeFiles.set_content` | 函数/方法 | [49–50](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L49-L50) | 待讲解 |
| `_FakeBatches` | 类 | [53–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L53-L73) | 待讲解 |
| `_FakeBatches.__init__` | 函数/方法 | [55–58](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L55-L58) | 待讲解 |
| `_FakeBatches.create` | 函数/方法 | [60–62](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L60-L62) | 待讲解 |
| `_FakeBatches.retrieve` | 函数/方法 | [64–67](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L64-L67) | 待讲解 |
| `_FakeBatches.cancel` | 函数/方法 | [69–70](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L69-L70) | 待讲解 |
| `_FakeBatches.push_retrieve` | 函数/方法 | [72–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L72-L73) | 待讲解 |
| `_FakeClient` | 类 | [76–80](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L76-L80) | 待讲解 |
| `_FakeClient.__init__` | 函数/方法 | [78–80](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L78-L80) | 待讲解 |
| `_StatusError` | 类 | [83–87](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L83-L87) | 待讲解 |
| `_StatusError.__init__` | 函数/方法 | [85–87](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L85-L87) | 待讲解 |
| `_make_output_line` | 函数/方法 | [90–103](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L90-L103) | 待讲解 |
| `_make_error_line` | 函数/方法 | [106–117](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L106-L117) | 待讲解 |
| `_make_refusal_line` | 函数/方法 | [120–133](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L120-L133) | 待讲解 |
| `OpenAIBatchConfigTest` | 类 | [136–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L136-L175) | 待讲解 |
| `OpenAIBatchConfigTest.test_default_timeout_covers_completion_window` | 函数/方法 | [138–141](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L138-L141) | 待讲解 |
| `OpenAIBatchConfigTest.test_from_dict_accepts_batch_config` | 函数/方法 | [143–146](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L143-L146) | 待讲解 |
| `OpenAIBatchConfigTest.test_from_dict_accepts_boolean_shorthand` | 函数/方法 | [148–153](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L148-L153) | 待讲解 |
| `OpenAIBatchConfigTest.test_from_dict_rejects_invalid_type` | 函数/方法 | [155–157](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L155-L157) | 待讲解 |
| `OpenAIBatchConfigTest.test_infer_batch_rejects_invalid_batch_size` | 函数/方法 | [159–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L159-L175) | 待讲解 |
| `OpenAIBatchHelperTest` | 类 | [178–835](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L178-L835) | 待讲解 |
| `OpenAIBatchHelperTest.test_orders_results_by_custom_id` | 函数/方法 | [180–218](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L180-L218) | 待讲解 |
| `OpenAIBatchHelperTest.test_splits_jobs_by_batch_size` | 函数/方法 | [220–281](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L220-L281) | 待讲解 |
| `OpenAIBatchHelperTest.test_metadata_and_job_create_hook_are_used` | 函数/方法 | [283–316](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L283-L316) | 待讲解 |
| `OpenAIBatchHelperTest.test_item_error_raises` | 函数/方法 | [318–352](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L318-L352) | 待讲解 |
| `OpenAIBatchHelperTest.test_job_create_failure_deletes_uploaded_input_file` | 函数/方法 | [354–380](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L354-L380) | 待讲解 |
| `OpenAIBatchHelperTest.test_failed_job_reports_errors_field` | 函数/方法 | [382–412](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L382-L412) | 待讲解 |
| `OpenAIBatchHelperTest.test_terminal_job_status_reports_status` | 函数/方法 | [414–444](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L414-L444) | 待讲解 |
| `OpenAIBatchHelperTest.test_completed_job_missing_output_reports_custom_id` | 函数/方法 | [446–474](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L446-L474) | 待讲解 |
| `OpenAIBatchHelperTest.test_completed_job_without_output_or_error_file_raises` | 函数/方法 | [476–501](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L476-L501) | 待讲解 |
| `OpenAIBatchHelperTest.test_unexpected_custom_id_logs_warning` | 函数/方法 | [503–534](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L503-L534) | 待讲解 |
| `OpenAIBatchHelperTest.test_output_download_retries_transient_forbidden` | 函数/方法 | [536–577](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L536-L577) | 待讲解 |
| `OpenAIBatchHelperTest.test_output_download_forbidden_mentions_files_permission` | 函数/方法 | [579–616](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L579-L616) | 待讲解 |
| `OpenAIBatchHelperTest.test_completed_job_reads_error_file` | 函数/方法 | [618–653](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L618-L653) | 待讲解 |
| `OpenAIBatchHelperTest.test_non_2xx_response_raises_item_error` | 函数/方法 | [655–685](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L655-L685) | 待讲解 |
| `OpenAIBatchHelperTest.test_refusal_message_is_reported` | 函数/方法 | [687–717](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L687-L717) | 待讲解 |
| `OpenAIBatchHelperTest.test_timeout_cancels_job` | 函数/方法 | [719–745](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L719-L745) | 待讲解 |
| `OpenAIBatchHelperTest.test_default_completion_window_is_sent` | 函数/方法 | [747–780](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L747-L780) | 待讲解 |
| `OpenAIBatchHelperTest.test_unsupported_completion_window_raises` | 函数/方法 | [782–784](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L782-L784) | 待讲解 |
| `OpenAIBatchHelperTest.test_truncated_batch_item_is_kept_and_logged` | 函数/方法 | [786–811](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L786-L811) | 待讲解 |
| `OpenAIBatchHelperTest.test_complete_batch_item_is_not_logged` | 函数/方法 | [813–835](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/openai_batch_test.py#L813-L835) | 待讲解 |

## tests/plugins_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `AvailableProvidersTest` | 类 | [29–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/plugins_test.py#L29-L73) | 待讲解 |
| `AvailableProvidersTest.setUp` | 函数/方法 | [31–51](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/plugins_test.py#L31-L51) | 待讲解 |
| `AvailableProvidersTest.test_builtins_take_precedence_by_default` | 函数/方法 | [53–57](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/plugins_test.py#L53-L57) | 待讲解 |
| `AvailableProvidersTest.test_allow_override_lets_third_party_replace_builtin` | 函数/方法 | [59–63](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/plugins_test.py#L59-L63) | 待讲解 |
| `AvailableProvidersTest.test_excluding_optional_omits_optional_builtin` | 函数/方法 | [65–68](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/plugins_test.py#L65-L68) | 待讲解 |
| `AvailableProvidersTest.test_third_party_alias_for_builtin_class_is_kept` | 函数/方法 | [70–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/plugins_test.py#L70-L73) | 待讲解 |

## tests/pr_security_check_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_load_check` | 函数/方法 | [37–45](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L37-L45) | 待讲解 |
| `_j` | 函数/方法 | [51–53](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L51-L53) | 待讲解 |
| `_new_file_diff` | 函数/方法 | [64–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L64-L73) | 待讲解 |
| `_binary_diff` | 函数/方法 | [76–81](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L76-L81) | 待讲解 |
| `_findings` | 函数/方法 | [84–85](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L84-L85) | 待讲解 |
| `_rules` | 函数/方法 | [88–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L88-L91) | 待讲解 |
| `CheckFilesTest` | 类 | [128–243](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L128-L243) | 待讲解 |
| `CheckFilesTest.test_import_time_egress_is_high` | 函数/方法 | [130–136](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L130-L136) | 待讲解 |
| `CheckFilesTest.test_privileged_workflow_change_is_high_for_non_maintainer` | 函数/方法 | [138–144](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L138-L144) | 待讲解 |
| `CheckFilesTest.test_maintainer_may_change_workflows_and_build_config` | 函数/方法 | [146–150](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L146-L150) | 待讲解 |
| `CheckFilesTest.test_provider_reading_key_and_calling_api_is_medium` | 函数/方法 | [152–163](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L152-L163) | 待讲解 |
| `CheckFilesTest.test_hidden_characters` | 函数/方法 | [165–210](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L165-L210) | 待讲解 |
| `CheckFilesTest.test_bom_on_new_code_file` | 函数/方法 | [212–214](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L212-L214) | 待讲解 |
| `CheckFilesTest.test_decoded_execution_is_high` | 函数/方法 | [216–232](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L216-L232) | 待讲解 |
| `CheckFilesTest.test_regex_compile_with_base64_import_is_not_flagged` | 函数/方法 | [234–239](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L234-L239) | 待讲解 |
| `CheckFilesTest.test_crlf_churn_is_low` | 函数/方法 | [241–243](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L241-L243) | 待讲解 |
| `FalsePositiveTest` | 类 | [246–308](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L246-L308) | 待讲解 |
| `FalsePositiveTest.test_docs_curl_example_with_api_key_is_medium` | 函数/方法 | [249–256](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L249-L256) | 待讲解 |
| `FalsePositiveTest.test_basic_auth_provider_is_medium` | 函数/方法 | [258–274](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L258-L274) | 待讲解 |
| `FalsePositiveTest.test_mocked_client_in_test_file_is_not_high` | 函数/方法 | [276–286](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L276-L286) | 待讲解 |
| `FalsePositiveTest.test_local_curl_in_workflow_from_non_maintainer_is_medium` | 函数/方法 | [288–298](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L288-L298) | 待讲解 |
| `FalsePositiveTest.test_private_ip_is_ignored` | 函数/方法 | [300–304](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L300-L304) | 待讲解 |
| `FalsePositiveTest.test_version_bump_from_contributor_is_medium` | 函数/方法 | [306–308](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L306-L308) | 待讲解 |
| `CoverageRegressionTest` | 类 | [311–517](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L311-L517) | 待讲解 |
| `CoverageRegressionTest.test_environment_dump_to_network_in_conftest_is_high` | 函数/方法 | [314–324](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L314-L324) | 待讲解 |
| `CoverageRegressionTest.test_bare_environ_import_and_aliased_requests_is_high` | 函数/方法 | [326–336](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L326-L336) | 待讲解 |
| `CoverageRegressionTest.test_split_across_files_is_high` | 函数/方法 | [338–345](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L338-L345) | 待讲解 |
| `CoverageRegressionTest.test_split_across_files_with_only_an_import_is_not_high` | 函数/方法 | [347–351](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L347-L351) | 待讲解 |
| `CoverageRegressionTest.test_network_calls_versus_exception_constructors` | 函数/方法 | [353–368](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L353-L368) | 待讲解 |
| `CoverageRegressionTest.test_comma_import_of_network_library_is_detected` | 函数/方法 | [370–379](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L370-L379) | 待讲解 |
| `CoverageRegressionTest.test_path_containing_diff_marker_is_parsed_and_flagged` | 函数/方法 | [381–387](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L381-L387) | 待讲解 |
| `CoverageRegressionTest.test_check_source_does_not_count_as_environment_copy` | 函数/方法 | [389–394](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L389-L394) | 待讲解 |
| `CoverageRegressionTest.test_quoted_workflow_path_is_still_checked` | 函数/方法 | [396–409](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L396-L409) | 待讲解 |
| `CoverageRegressionTest.test_paths_are_parsed_from_every_header_form` | 函数/方法 | [411–465](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L411-L465) | 待讲解 |
| `CoverageRegressionTest.test_workflow_renamed_or_deleted_by_contributor_is_flagged` | 函数/方法 | [467–483](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L467-L483) | 待讲解 |
| `CoverageRegressionTest.test_added_lines_starting_with_plus_signs_are_kept` | 函数/方法 | [485–495](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L485-L495) | 待讲解 |
| `CoverageRegressionTest.test_binary_code_file_is_high_and_loadable_binary_is_medium` | 函数/方法 | [497–502](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L497-L502) | 待讲解 |
| `CoverageRegressionTest.test_symlink_is_medium` | 函数/方法 | [504–506](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L504-L506) | 待讲解 |
| `CoverageRegressionTest.test_remote_dependency_from_contributor_is_high` | 函数/方法 | [508–513](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L508-L513) | 待讲解 |
| `CoverageRegressionTest.test_gitattributes_change_is_medium` | 函数/方法 | [515–517](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L515-L517) | 待讲解 |
| `CheckMetadataTest` | 类 | [520–576](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L520-L576) | 待讲解 |
| `CheckMetadataTest.test_command_substitution_in_description_is_high` | 函数/方法 | [522–525](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L522-L525) | 待讲解 |
| `CheckMetadataTest.test_absolute_tool_path_is_still_high` | 函数/方法 | [527–530](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L527-L530) | 待讲解 |
| `CheckMetadataTest.test_hidden_comment_payload_with_raw_ip` | 函数/方法 | [532–545](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L532-L545) | 待讲解 |
| `CheckMetadataTest.test_backtick_command_in_hidden_comment_is_medium` | 函数/方法 | [547–550](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L547-L550) | 待讲解 |
| `CheckMetadataTest.test_ordinary_descriptions_are_not_high_or_medium` | 函数/方法 | [552–576](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L552-L576) | 待讲解 |
| `ReportTest` | 类 | [579–612](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L579-L612) | 待讲解 |
| `ReportTest.test_excerpt_is_inert_and_visible` | 函数/方法 | [581–587](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L581-L587) | 待讲解 |
| `ReportTest.test_detailed_summary_renders_crafted_path_inert` | 函数/方法 | [589–594](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L589-L594) | 待讲解 |
| `ReportTest.test_brief_summary_has_counts_and_repro_only` | 函数/方法 | [596–604](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L596-L604) | 待讲解 |
| `ReportTest.test_check_and_tests_do_not_flag_themselves` | 函数/方法 | [606–612](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L606-L612) | 待讲解 |
| `MainTest` | 类 | [615–685](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L615-L685) | 待讲解 |
| `MainTest.setUp` | 函数/方法 | [617–622](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L617-L622) | 待讲解 |
| `MainTest._git` | 函数/方法 | [624–631](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L624-L631) | 待讲解 |
| `MainTest._commit` | 函数/方法 | [633–648](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L633-L648) | 待讲解 |
| `MainTest.test_main_writes_reports_and_fails_on_high` | 函数/方法 | [650–685](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/pr_security_check_test.py#L650-L685) | 待讲解 |

## tests/progress_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `ProgressTest` | 类 | [25–79](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/progress_test.py#L25-L79) | 待讲解 |
| `ProgressTest.test_download_progress_bar` | 函数/方法 | [27–35](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/progress_test.py#L27-L35) | 待讲解 |
| `ProgressTest.test_extraction_progress_bar` | 函数/方法 | [37–45](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/progress_test.py#L37-L45) | 待讲解 |
| `ProgressTest.test_save_load_progress_bars` | 函数/方法 | [47–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/progress_test.py#L47-L55) | 待讲解 |
| `ProgressTest.test_model_info_extraction` | 函数/方法 | [57–66](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/progress_test.py#L57-L66) | 待讲解 |
| `ProgressTest.test_formatting_functions` | 函数/方法 | [68–79](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/progress_test.py#L68-L79) | 待讲解 |

## tests/prompt_validation_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `PromptAlignmentValidationTest` | 类 | [28–394](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L28-L394) | 待讲解 |
| `PromptAlignmentValidationTest.test_alignment_detection` | 函数/方法 | [30–97](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L30-L97) | 待讲解 |
| `PromptAlignmentValidationTest.test_multiple_extractions_per_example` | 函数/方法 | [99–158](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L99-L158) | 待讲解 |
| `PromptAlignmentValidationTest.test_validation_levels_that_dont_raise` | 函数/方法 | [160–196](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L160-L196) | 待讲解 |
| `PromptAlignmentValidationTest.test_error_mode_raises_appropriately` | 函数/方法 | [198–245](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L198-L245) | 待讲解 |
| `PromptAlignmentValidationTest.test_empty_examples_produces_empty_report` | 函数/方法 | [247–252](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L247-L252) | 待讲解 |
| `PromptAlignmentValidationTest.test_multiple_examples_preserve_indices` | 函数/方法 | [254–306](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L254-L306) | 待讲解 |
| `PromptAlignmentValidationTest.test_validation_does_not_mutate_input` | 函数/方法 | [308–330](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L308-L330) | 待讲解 |
| `PromptAlignmentValidationTest.test_alignment_policies` | 函数/方法 | [332–394](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L332-L394) | 待讲解 |
| `ExtractIntegrationTest` | 类 | [397–425](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L397-L425) | 待讲解 |
| `ExtractIntegrationTest.test_extract_validates_in_error_mode` | 函数/方法 | [400–425](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L400-L425) | 待讲解 |
| `AlgorithmPolicyIntegrationTest` | 类 | [428–471](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L428-L471) | 待讲解 |
| `AlgorithmPolicyIntegrationTest.test_lcs_accepts_short_source_legacy_rejects` | 函数/方法 | [431–471](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L431-L471) | 待讲解 |
| `LegacyDeprecationWarningTest` | 类 | [474–532](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L474-L532) | 待讲解 |
| `LegacyDeprecationWarningTest._make_examples` | 函数/方法 | [477–489](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L477-L489) | 待讲解 |
| `LegacyDeprecationWarningTest.test_legacy_emits_warning_via_extract` | 函数/方法 | [491–512](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L491-L512) | 待讲解 |
| `LegacyDeprecationWarningTest.test_lcs_default_does_not_warn` | 函数/方法 | [514–532](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompt_validation_test.py#L514-L532) | 待讲解 |

## tests/prompting_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `QAPromptGeneratorTest` | 类 | [25–399](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L25-L399) | 待讲解 |
| `QAPromptGeneratorTest.test_generate_prompt` | 函数/方法 | [27–108](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L27-L108) | 待讲解 |
| `QAPromptGeneratorTest.test_format_example` | 函数/方法 | [110–399](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L110-L399) | 待讲解 |
| `PromptBuilderTest` | 类 | [402–456](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L402-L456) | 待讲解 |
| `PromptBuilderTest._create_generator` | 函数/方法 | [405–430](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L405-L430) | 待讲解 |
| `PromptBuilderTest.test_build_prompt_renders_chunk_text` | 函数/方法 | [432–443](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L432-L443) | 待讲解 |
| `PromptBuilderTest.test_build_prompt_includes_additional_context` | 函数/方法 | [445–456](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L445-L456) | 待讲解 |
| `ContextAwarePromptBuilderTest` | 类 | [459–591](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L459-L591) | 待讲解 |
| `ContextAwarePromptBuilderTest._create_generator` | 函数/方法 | [462–487](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L462-L487) | 待讲解 |
| `ContextAwarePromptBuilderTest.test_context_window_chars_property` | 函数/方法 | [489–499](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L489-L499) | 待讲解 |
| `ContextAwarePromptBuilderTest.test_first_chunk_has_no_previous_context` | 函数/方法 | [501–515](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L501-L515) | 待讲解 |
| `ContextAwarePromptBuilderTest.test_second_chunk_includes_previous_context` | 函数/方法 | [517–532](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L517-L532) | 待讲解 |
| `ContextAwarePromptBuilderTest.test_context_disabled_when_none` | 函数/方法 | [534–548](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L534-L548) | 待讲解 |
| `ContextAwarePromptBuilderTest.test_context_isolated_per_document` | 函数/方法 | [550–572](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L550-L572) | 待讲解 |
| `ContextAwarePromptBuilderTest.test_combines_previous_context_with_additional_context` | 函数/方法 | [574–591](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/prompting_test.py#L574-L591) | 待讲解 |

## tests/provider_plugin_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_create_mock_entry_points` | 函数/方法 | [40–58](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L40-L58) | 待讲解 |
| `_create_mock_entry_points.MockEntryPoints` | 类 | [50–56](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L50-L56) | 待讲解 |
| `_create_mock_entry_points.MockEntryPoints.select` | 函数/方法 | [53–56](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L53-L56) | 待讲解 |
| `PluginSmokeTest` | 类 | [61–394](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L61-L394) | 待讲解 |
| `PluginSmokeTest.setUp` | 函数/方法 | [64–71](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L64-L71) | 待讲解 |
| `PluginSmokeTest.test_plugin_discovery_and_usage` | 函数/方法 | [73–119](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L73-L119) | 待讲解 |
| `PluginSmokeTest.test_plugin_discovery_and_usage._ep_load` | 函数/方法 | [80–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L80-L91) | 待讲解 |
| `PluginSmokeTest.test_plugin_discovery_and_usage._ep_load.PluginProvider` | 类 | [81–89](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L81-L89) | 待讲解 |
| `PluginSmokeTest.test_plugin_discovery_and_usage._ep_load.PluginProvider.__init__` | 函数/方法 | [84–86](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L84-L86) | 待讲解 |
| `PluginSmokeTest.test_plugin_discovery_and_usage._ep_load.PluginProvider.infer` | 函数/方法 | [88–89](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L88-L89) | 待讲解 |
| `PluginSmokeTest.test_plugin_disabled_by_env_var` | 函数/方法 | [121–129](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L121-L129) | 待讲解 |
| `PluginSmokeTest.test_handles_import_errors_gracefully` | 函数/方法 | [131–163](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L131-L163) | 待讲解 |
| `PluginSmokeTest.test_handles_import_errors_gracefully._bad_load` | 函数/方法 | [134–135](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L134-L135) | 待讲解 |
| `PluginSmokeTest.test_load_plugins_once_is_idempotent` | 函数/方法 | [165–192](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L165-L192) | 待讲解 |
| `PluginSmokeTest.test_load_plugins_once_is_idempotent._ep_load` | 函数/方法 | [168–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L168-L175) | 待讲解 |
| `PluginSmokeTest.test_load_plugins_once_is_idempotent._ep_load.Plugin` | 类 | [169–173](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L169-L173) | 待讲解 |
| `PluginSmokeTest.test_load_plugins_once_is_idempotent._ep_load.Plugin.infer` | 函数/方法 | [172–173](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L172-L173) | 待讲解 |
| `PluginSmokeTest.test_non_subclass_entry_point_does_not_crash` | 函数/方法 | [194–225](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L194-L225) | 待讲解 |
| `PluginSmokeTest.test_non_subclass_entry_point_does_not_crash.NotAProvider` | 类 | [197–198](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L197-L198) | 待讲解 |
| `PluginSmokeTest.test_plugin_priority_override_core_provider` | 函数/方法 | [227–260](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L227-L260) | 待讲解 |
| `PluginSmokeTest.test_plugin_priority_override_core_provider._ep_load` | 函数/方法 | [233–240](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L233-L240) | 待讲解 |
| `PluginSmokeTest.test_plugin_priority_override_core_provider._ep_load.OverrideGemini` | 类 | [234–238](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L234-L238) | 待讲解 |
| `PluginSmokeTest.test_plugin_priority_override_core_provider._ep_load.OverrideGemini.infer` | 函数/方法 | [237–238](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L237-L238) | 待讲解 |
| `PluginSmokeTest.test_resolve_provider_for_plugin` | 函数/方法 | [262–296](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L262-L296) | 待讲解 |
| `PluginSmokeTest.test_resolve_provider_for_plugin._ep_load` | 函数/方法 | [268–275](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L268-L275) | 待讲解 |
| `PluginSmokeTest.test_resolve_provider_for_plugin._ep_load.ResolveMePlease` | 类 | [269–273](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L269-L273) | 待讲解 |
| `PluginSmokeTest.test_resolve_provider_for_plugin._ep_load.ResolveMePlease.infer` | 函数/方法 | [272–273](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L272-L273) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema` | 函数/方法 | [298–394](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L298-L394) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema.TestPluginSchema` | 类 | [301–316](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L301-L316) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema.TestPluginSchema.__init__` | 函数/方法 | [304–305](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L304-L305) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema.TestPluginSchema.from_examples` | 函数/方法 | [307–309](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L307-L309) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema.TestPluginSchema.to_provider_config` | 函数/方法 | [311–312](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L311-L312) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema.TestPluginSchema.requires_raw_output` | 函数/方法 | [314–316](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L314-L316) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema._ep_load` | 函数/方法 | [318–339](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L318-L339) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema._ep_load.SchemaTestProvider` | 类 | [319–337](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L319-L337) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema._ep_load.SchemaTestProvider.__init__` | 函数/方法 | [322–325](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L322-L325) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema._ep_load.SchemaTestProvider.get_schema_class` | 函数/方法 | [327–329](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L327-L329) | 待讲解 |
| `PluginSmokeTest.test_plugin_with_custom_schema._ep_load.SchemaTestProvider.infer` | 函数/方法 | [331–337](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L331-L337) | 待讲解 |
| `PluginE2ETest` | 类 | [397–705](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L397-L705) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e` | 函数/方法 | [404–503](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L404-L503) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e.TestPluginSchema` | 类 | [407–422](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L407-L422) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e.TestPluginSchema.__init__` | 函数/方法 | [410–411](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L410-L411) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e.TestPluginSchema.from_examples` | 函数/方法 | [413–415](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L413-L415) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e.TestPluginSchema.to_provider_config` | 函数/方法 | [417–418](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L417-L418) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e.TestPluginSchema.requires_raw_output` | 函数/方法 | [420–422](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L420-L422) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e._ep_load` | 函数/方法 | [424–448](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L424-L448) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e._ep_load.SchemaE2EProvider` | 类 | [425–446](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L425-L446) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e._ep_load.SchemaE2EProvider.__init__` | 函数/方法 | [428–431](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L428-L431) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e._ep_load.SchemaE2EProvider.get_schema_class` | 函数/方法 | [433–435](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L433-L435) | 待讲解 |
| `PluginE2ETest.test_plugin_with_schema_e2e._ep_load.SchemaE2EProvider.infer` | 函数/方法 | [437–446](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L437-L446) | 待讲解 |
| `PluginE2ETest.test_pip_install_discovery_and_cleanup` | 函数/方法 | [505–705](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_plugin_test.py#L505-L705) | 待讲解 |

## tests/provider_schema_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `ProviderSchemaDiscoveryTest` | 类 | [36–64](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L36-L64) | 待讲解 |
| `ProviderSchemaDiscoveryTest.test_gemini_returns_gemini_schema` | 函数/方法 | [39–46](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L39-L46) | 待讲解 |
| `ProviderSchemaDiscoveryTest.test_ollama_returns_format_mode_schema` | 函数/方法 | [48–55](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L48-L55) | 待讲解 |
| `ProviderSchemaDiscoveryTest.test_openai_returns_openai_schema` | 函数/方法 | [57–64](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L57-L64) | 待讲解 |
| `FormatModeSchemaTest` | 类 | [67–149](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L67-L149) | 待讲解 |
| `FormatModeSchemaTest.test_from_examples_ignores_examples` | 函数/方法 | [70–90](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L70-L90) | 待讲解 |
| `FormatModeSchemaTest.test_to_provider_config_returns_format` | 函数/方法 | [92–103](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L92-L103) | 待讲解 |
| `FormatModeSchemaTest.test_requires_raw_output_returns_true` | 函数/方法 | [105–113](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L105-L113) | 待讲解 |
| `FormatModeSchemaTest.test_different_examples_same_output` | 函数/方法 | [115–149](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L115-L149) | 待讲解 |
| `OllamaFormatParameterTest` | 类 | [152–316](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L152-L316) | 待讲解 |
| `OllamaFormatParameterTest.test_ollama_json_format_in_request_payload` | 函数/方法 | [155–181](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L155-L181) | 待讲解 |
| `OllamaFormatParameterTest.test_ollama_default_format_is_json` | 函数/方法 | [183–201](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L183-L201) | 待讲解 |
| `OllamaFormatParameterTest.test_extract_with_ollama_passes_json_format` | 函数/方法 | [203–259](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L203-L259) | 待讲解 |
| `OllamaFormatParameterTest.test_extract_with_ollama_passes_think_parameter` | 函数/方法 | [261–316](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L261-L316) | 待讲解 |
| `OllamaYAMLOverrideTest` | 类 | [319–426](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L319-L426) | 待讲解 |
| `OllamaYAMLOverrideTest.test_ollama_yaml_format_in_request_payload` | 函数/方法 | [322–341](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L322-L341) | 待讲解 |
| `OllamaYAMLOverrideTest.test_yaml_override_sets_fence_output_true` | 函数/方法 | [343–383](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L343-L383) | 待讲解 |
| `OllamaYAMLOverrideTest.test_json_format_keeps_fence_output_false` | 函数/方法 | [385–426](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L385-L426) | 待讲解 |
| `GeminiSchemaProviderIntegrationTest` | 类 | [429–608](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L429-L608) | 待讲解 |
| `GeminiSchemaProviderIntegrationTest.test_gemini_schema_to_provider_config` | 函数/方法 | [432–475](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L432-L475) | 待讲解 |
| `GeminiSchemaProviderIntegrationTest.test_gemini_requires_raw_output` | 函数/方法 | [477–484](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L477-L484) | 待讲解 |
| `GeminiSchemaProviderIntegrationTest.test_gemini_rejects_yaml_with_schema` | 函数/方法 | [486–519](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L486-L519) | 待讲解 |
| `GeminiSchemaProviderIntegrationTest.test_gemini_forwards_schema_to_genai_client` | 函数/方法 | [521–574](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L521-L574) | 待讲解 |
| `GeminiSchemaProviderIntegrationTest.test_gemini_doesnt_forward_non_api_kwargs` | 函数/方法 | [576–608](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L576-L608) | 待讲解 |
| `SchemaShimTest` | 类 | [611–642](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L611-L642) | 待讲解 |
| `SchemaShimTest.test_constraint_types_import` | 函数/方法 | [614–629](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L614-L629) | 待讲解 |
| `SchemaShimTest.test_provider_schema_imports` | 函数/方法 | [631–642](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L631-L642) | 待讲解 |
| `ApplyOutputSchemaTest` | 类 | [645–755](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L645-L755) | 待讲解 |
| `ApplyOutputSchemaTest.setUp` | 函数/方法 | [648–652](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L648-L652) | 待讲解 |
| `ApplyOutputSchemaTest.test_gemini_output_schema_reaches_generate_config` | 函数/方法 | [654–668](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L654-L668) | 待讲解 |
| `ApplyOutputSchemaTest.test_openai_output_schema_reaches_response_format` | 函数/方法 | [670–692](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L670-L692) | 待讲解 |
| `ApplyOutputSchemaTest.test_ollama_apply_output_schema_raises` | 函数/方法 | [694–700](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L694-L700) | 待讲解 |
| `ApplyOutputSchemaTest.test_apply_output_schema_rejects_conflicting_schema_kwargs` | 函数/方法 | [702–713](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L702-L713) | 待讲解 |
| `ApplyOutputSchemaTest.test_apply_output_schema_is_idempotent_for_same_schema` | 函数/方法 | [715–724](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L715-L724) | 待讲解 |
| `ApplyOutputSchemaTest.test_apply_output_schema_rejects_different_schema` | 函数/方法 | [726–739](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L726-L739) | 待讲解 |
| `ApplyOutputSchemaTest.test_apply_output_schema_rejects_constructor_gemini_schema` | 函数/方法 | [741–755](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L741-L755) | 待讲解 |
| `OpenAINoContentResponseTest` | 类 | [758–985](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L758-L985) | 待讲解 |
| `OpenAINoContentResponseTest.setUp` | 函数/方法 | [761–773](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L761-L773) | 待讲解 |
| `OpenAINoContentResponseTest.test_content_returns_scored_output` | 函数/方法 | [775–786](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L775-L786) | 待讲解 |
| `OpenAINoContentResponseTest.test_refusal_raises_with_refusal_message` | 函数/方法 | [788–802](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L788-L802) | 待讲解 |
| `OpenAINoContentResponseTest.test_refusal_with_content_still_raises` | 函数/方法 | [804–816](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L804-L816) | 待讲解 |
| `OpenAINoContentResponseTest.test_missing_content_raises_with_finish_reason` | 函数/方法 | [818–864](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L818-L864) | 待讲解 |
| `OpenAINoContentResponseTest.test_exhausted_budget_raises_with_hint` | 函数/方法 | [866–882](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L866-L882) | 待讲解 |
| `OpenAINoContentResponseTest.test_empty_content_with_normal_stop_passes_through` | 函数/方法 | [884–894](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L884-L894) | 待讲解 |
| `OpenAINoContentResponseTest.test_no_choices_raises` | 函数/方法 | [896–900](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L896-L900) | 待讲解 |
| `OpenAINoContentResponseTest.test_no_content_error_is_not_rewrapped` | 函数/方法 | [902–913](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L902-L913) | 待讲解 |
| `OpenAINoContentResponseTest.test_parallel_infer_preserves_no_content_error` | 函数/方法 | [915–932](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L915-L932) | 待讲解 |
| `OpenAINoContentResponseTest.test_extract_propagates_refusal` | 函数/方法 | [934–985](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/provider_schema_test.py#L934-L985) | 待讲解 |

## tests/registry_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `FakeProvider` | 类 | [36–43](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L36-L43) | 待讲解 |
| `FakeProvider.infer` | 函数/方法 | [39–40](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L39-L40) | 待讲解 |
| `FakeProvider.infer_batch` | 函数/方法 | [42–43](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L42-L43) | 待讲解 |
| `AnotherFakeProvider` | 类 | [46–53](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L46-L53) | 待讲解 |
| `AnotherFakeProvider.infer` | 函数/方法 | [49–50](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L49-L50) | 待讲解 |
| `AnotherFakeProvider.infer_batch` | 函数/方法 | [52–53](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L52-L53) | 待讲解 |
| `RegistryTest` | 类 | [56–263](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L56-L263) | 待讲解 |
| `RegistryTest.setUp` | 函数/方法 | [58–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L58-L60) | 待讲解 |
| `RegistryTest.tearDown` | 函数/方法 | [62–64](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L62-L64) | 待讲解 |
| `RegistryTest.test_register_decorator` | 函数/方法 | [66–74](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L66-L74) | 待讲解 |
| `RegistryTest.test_register_decorator.TestProvider` | 类 | [69–71](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L69-L71) | 待讲解 |
| `RegistryTest.test_register_lazy` | 函数/方法 | [76–82](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L76-L82) | 待讲解 |
| `RegistryTest.test_multiple_patterns` | 函数/方法 | [84–90](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L84-L90) | 待讲解 |
| `RegistryTest.test_priority_resolution` | 函数/方法 | [92–99](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L92-L99) | 待讲解 |
| `RegistryTest.test_no_provider_registered` | 函数/方法 | [101–107](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L101-L107) | 待讲解 |
| `RegistryTest.test_clear_registry` | 函数/方法 | [109–123](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L109-L123) | 待讲解 |
| `RegistryTest.test_load_builtins_once_deduplicates_and_recovers_after_clear` | 函数/方法 | [125–139](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L125-L139) | 待讲解 |
| `RegistryTest.test_openai_patterns_cover_reasoning_and_gpt35_models` | 函数/方法 | [141–147](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L141-L147) | 待讲解 |
| `RegistryTest.test_openai_patterns_cover_gpt6_models` | 函数/方法 | [149–155](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L149-L155) | 待讲解 |
| `RegistryTest.test_openai_patterns_do_not_capture_unrelated_o_prefix` | 函数/方法 | [157–161](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L157-L161) | 待讲解 |
| `RegistryTest.test_list_entries` | 函数/方法 | [163–179](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L163-L179) | 待讲解 |
| `RegistryTest.test_lazy_loading_defers_import` | 函数/方法 | [181–192](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L181-L192) | 待讲解 |
| `RegistryTest.test_regex_pattern_objects` | 函数/方法 | [194–206](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L194-L206) | 待讲解 |
| `RegistryTest.test_regex_pattern_objects.CustomProvider` | 类 | [198–200](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L198-L200) | 待讲解 |
| `RegistryTest.test_resolve_provider_by_name` | 函数/方法 | [208–221](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L208-L221) | 待讲解 |
| `RegistryTest.test_resolve_provider_by_name.TestProvider` | 类 | [211–213](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L211-L213) | 待讲解 |
| `RegistryTest.test_resolve_provider_not_found` | 函数/方法 | [223–227](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L223-L227) | 待讲解 |
| `RegistryTest.test_hf_style_model_id_patterns` | 函数/方法 | [229–263](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L229-L263) | 待讲解 |
| `RegistryTest.test_hf_style_model_id_patterns.TestHFProvider` | 类 | [236–248](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L236-L248) | 待讲解 |
| `RegistryTest.test_hf_style_model_id_patterns.TestHFProvider.infer` | 函数/方法 | [247–248](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L247-L248) | 待讲解 |
| `RouterCacheTest` | 类 | [266–310](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L266-L310) | 待讲解 |
| `RouterCacheTest.setUp` | 函数/方法 | [268–270](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L268-L270) | 待讲解 |
| `RouterCacheTest.tearDown` | 函数/方法 | [272–274](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L272-L274) | 待讲解 |
| `RouterCacheTest.test_repeated_resolve_is_served_from_cache` | 函数/方法 | [276–282](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L276-L282) | 待讲解 |
| `RouterCacheTest.test_resolve_sees_provider_registered_after_first_lookup` | 函数/方法 | [284–290](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L284-L290) | 待讲解 |
| `RouterCacheTest.test_resolve_provider_sees_provider_registered_after_first_lookup` | 函数/方法 | [292–299](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L292-L299) | 待讲解 |
| `RouterCacheTest.test_duplicate_registration_keeps_cache` | 函数/方法 | [301–310](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/registry_test.py#L301-L310) | 待讲解 |

## tests/resolver_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `assert_char_interval_match_source` | 函数/方法 | [28–57](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L28-L57) | 待讲解 |
| `ParserTest` | 类 | [60–146](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L60-L146) | 待讲解 |
| `ParserTest.test_parser_error_cases` | 函数/方法 | [62–146](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L62-L146) | 待讲解 |
| `ExtractOrderedEntitiesTest` | 类 | [149–630](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L149-L630) | 待讲解 |
| `ExtractOrderedEntitiesTest.test_extract_ordered_extractions_success` | 函数/方法 | [151–597](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L151-L597) | 待讲解 |
| `ExtractOrderedEntitiesTest.test_extract_ordered_extractions_exceptions` | 函数/方法 | [599–630](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L599-L630) | 待讲解 |
| `AlignEntitiesTest` | 类 | [633–1736](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L633-L1736) | 待讲解 |
| `AlignEntitiesTest.setUp` | 函数/方法 | [646–649](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L646-L649) | 待讲解 |
| `AlignEntitiesTest.test_extraction_alignment` | 函数/方法 | [651–1736](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L651-L1736) | 待讲解 |
| `ResolverTest` | 类 | [1739–2311](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1739-L2311) | 待讲解 |
| `ResolverTest.setUp` | 函数/方法 | [1808–1813](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1808-L1813) | 待讲解 |
| `ResolverTest.test_resolve_valid_inputs` | 函数/方法 | [1815–1893](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1815-L1893) | 待讲解 |
| `ResolverTest.test_handle_integer_extraction` | 函数/方法 | [1895–1917](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1895-L1917) | 待讲解 |
| `ResolverTest.test_resolve_empty_yaml` | 函数/方法 | [1919–1924](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1919-L1924) | 待讲解 |
| `ResolverTest.test_resolve_empty_yaml_without_suppress_parse_errors` | 函数/方法 | [1926–1929](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1926-L1929) | 待讲解 |
| `ResolverTest.test_resolve_and_align_skip_empty_extraction_values` | 函数/方法 | [1931–1955](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1931-L1955) | 待讲解 |
| `ResolverTest.test_resolve_and_align_only_empty_value_returns_no_extractions` | 函数/方法 | [1957–1977](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1957-L1977) | 待讲解 |
| `ResolverTest.test_resolve_parse_error_suppressed_logs_opt_out` | 函数/方法 | [1979–1991](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1979-L1991) | 待讲解 |
| `ResolverTest.test_resolve_schema_error_suppressed` | 函数/方法 | [1993–2023](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L1993-L2023) | 待讲解 |
| `ResolverTest.test_resolve_schema_error_raises_without_suppression` | 函数/方法 | [2025–2033](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2025-L2033) | 待讲解 |
| `ResolverTest.test_align_with_valid_chunk` | 函数/方法 | [2035–2080](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2035-L2080) | 待讲解 |
| `ResolverTest.test_align_with_chunk_starting_in_middle` | 函数/方法 | [2082–2132](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2082-L2132) | 待讲解 |
| `ResolverTest.test_align_with_no_extractions_in_chunk` | 函数/方法 | [2134–2155](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2134-L2155) | 待讲解 |
| `ResolverTest.test_align_successful` | 函数/方法 | [2157–2201](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2157-L2201) | 待讲解 |
| `ResolverTest.test_align_with_discontinuous_tokenized_text` | 函数/方法 | [2203–2242](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2203-L2242) | 待讲解 |
| `ResolverTest.test_align_with_discontinuous_tokenized_text_but_right_chunk` | 函数/方法 | [2244–2287](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2244-L2287) | 待讲解 |
| `ResolverTest.test_align_with_empty_annotated_extractions` | 函数/方法 | [2289–2311](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2289-L2311) | 待讲解 |
| `FenceFallbackTest` | 类 | [2314–2485](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2314-L2485) | 待讲解 |
| `FenceFallbackTest.test_parsing_scenarios` | 函数/方法 | [2317–2376](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2317-L2376) | 待讲解 |
| `FenceFallbackTest.test_fallback_preserves_content_integrity` | 函数/方法 | [2378–2425](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2378-L2425) | 待讲解 |
| `FenceFallbackTest.test_malformed_json_still_raises_error` | 函数/方法 | [2427–2439](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2427-L2439) | 待讲解 |
| `FenceFallbackTest.test_strict_fences_raises_on_missing_markers` | 函数/方法 | [2441–2453](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2441-L2453) | 待讲解 |
| `FenceFallbackTest.test_default_allows_fallback` | 函数/方法 | [2455–2465](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2455-L2465) | 待讲解 |
| `FenceFallbackTest.test_rejects_multiple_fenced_blocks` | 函数/方法 | [2467–2485](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2467-L2485) | 待讲解 |
| `FlexibleSchemaTest` | 类 | [2488–2568](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2488-L2568) | 待讲解 |
| `FlexibleSchemaTest.test_direct_list_format` | 函数/方法 | [2491–2505](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2491-L2505) | 待讲解 |
| `FlexibleSchemaTest.test_single_dict_as_extraction` | 函数/方法 | [2507–2517](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2507-L2517) | 待讲解 |
| `FlexibleSchemaTest.test_traditional_format_still_works` | 函数/方法 | [2519–2533](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2519-L2533) | 待讲解 |
| `FlexibleSchemaTest.test_lenient_mode_accepts_list` | 函数/方法 | [2535–2545](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2535-L2545) | 待讲解 |
| `FlexibleSchemaTest.test_flexible_with_attributes` | 函数/方法 | [2547–2568](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2547-L2568) | 待讲解 |
| `_entity` | 函数/方法 | [2571–2572](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2571-L2572) | 待讲解 |
| `MonotonicExactAlignmentTest` | 类 | [2575–2751](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2575-L2751) | 待讲解 |
| `MonotonicExactAlignmentTest.setUp` | 函数/方法 | [2597–2599](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2597-L2599) | 待讲解 |
| `MonotonicExactAlignmentTest._align` | 函数/方法 | [2601–2605](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2601-L2605) | 待讲解 |
| `MonotonicExactAlignmentTest.test_repeated_mention_aligns_to_successive_occurrences` | 函数/方法 | [2607–2617](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2607-L2617) | 待讲解 |
| `MonotonicExactAlignmentTest.test_clinical_note_duplicates_align_to_assessment_section` | 函数/方法 | [2619–2655](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2619-L2655) | 待讲解 |
| `MonotonicExactAlignmentTest.test_long_mention_seed_does_not_break_earlier_extractions` | 函数/方法 | [2657–2672](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2657-L2672) | 待讲解 |
| `MonotonicExactAlignmentTest.test_difflib_escape_hatch_preserves_legacy_behavior` | 函数/方法 | [2674–2689](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2674-L2689) | 待讲解 |
| `MonotonicExactAlignmentTest.test_duplicate_extractions_with_single_occurrence_share_span` | 函数/方法 | [2691–2701](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2691-L2701) | 待讲解 |
| `MonotonicExactAlignmentTest.test_out_of_order_extractions_fall_back_gracefully` | 函数/方法 | [2703–2715](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2703-L2715) | 待讲解 |
| `MonotonicExactAlignmentTest.test_exact_span_upgraded_over_competing_lesser_extraction` | 函数/方法 | [2717–2730](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2717-L2730) | 待讲解 |
| `MonotonicExactAlignmentTest.test_invalid_exact_alignment_algorithm_raises` | 函数/方法 | [2732–2740](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2732-L2740) | 待讲解 |
| `MonotonicExactAlignmentTest.test_offsets_applied_to_dp_matches` | 函数/方法 | [2742–2751](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/resolver_test.py#L2742-L2751) | 待讲解 |

## tests/schema_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_openai_extraction_items` | 函数/方法 | [37–38](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L37-L38) | 待讲解 |
| `_openai_variant` | 函数/方法 | [41–45](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L41-L45) | 待讲解 |
| `_openai_attribute_properties` | 函数/方法 | [48–51](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L48-L51) | 待讲解 |
| `BaseSchemaTest` | 类 | [54–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L54-L73) | 待讲解 |
| `BaseSchemaTest.test_abstract_methods_required` | 函数/方法 | [57–60](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L57-L60) | 待讲解 |
| `BaseSchemaTest.test_subclass_must_implement_all_methods` | 函数/方法 | [62–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L62-L73) | 待讲解 |
| `BaseSchemaTest.test_subclass_must_implement_all_methods.IncompleteSchema` | 类 | [66–70](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L66-L70) | 待讲解 |
| `BaseSchemaTest.test_subclass_must_implement_all_methods.IncompleteSchema.from_examples` | 函数/方法 | [68–70](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L68-L70) | 待讲解 |
| `BaseLanguageModelSchemaTest` | 类 | [76–106](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L76-L106) | 待讲解 |
| `BaseLanguageModelSchemaTest.test_get_schema_class_returns_none_by_default` | 函数/方法 | [79–87](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L79-L87) | 待讲解 |
| `BaseLanguageModelSchemaTest.test_get_schema_class_returns_none_by_default.TestModel` | 类 | [82–85](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L82-L85) | 待讲解 |
| `BaseLanguageModelSchemaTest.test_get_schema_class_returns_none_by_default.TestModel.infer` | 函数/方法 | [84–85](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L84-L85) | 待讲解 |
| `BaseLanguageModelSchemaTest.test_apply_schema_stores_instance` | 函数/方法 | [89–106](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L89-L106) | 待讲解 |
| `BaseLanguageModelSchemaTest.test_apply_schema_stores_instance.TestModel` | 类 | [92–95](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L92-L95) | 待讲解 |
| `BaseLanguageModelSchemaTest.test_apply_schema_stores_instance.TestModel.infer` | 函数/方法 | [94–95](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L94-L95) | 待讲解 |
| `GeminiSchemaTest` | 类 | [109–302](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L109-L302) | 待讲解 |
| `GeminiSchemaTest.test_from_examples_constructs_expected_schema` | 函数/方法 | [111–263](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L111-L263) | 待讲解 |
| `GeminiSchemaTest.test_to_provider_config_returns_response_schema` | 函数/方法 | [265–285](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L265-L285) | 待讲解 |
| `GeminiSchemaTest.test_requires_raw_output_returns_true` | 函数/方法 | [287–302](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L287-L302) | 待讲解 |
| `OpenAISchemaTest` | 类 | [305–676](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L305-L676) | 待讲解 |
| `OpenAISchemaTest.test_response_format_returns_json_schema_response_format` | 函数/方法 | [308–339](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L308-L339) | 待讲解 |
| `OpenAISchemaTest.test_to_provider_config_uses_provider_schema_hook` | 函数/方法 | [341–347](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L341-L347) | 待讲解 |
| `OpenAISchemaTest.test_from_examples_constructs_strict_openai_schema` | 函数/方法 | [349–445](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L349-L445) | 待讲解 |
| `OpenAISchemaTest.test_from_examples_preserves_list_attribute_schema` | 函数/方法 | [447–472](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L447-L472) | 待讲解 |
| `OpenAISchemaTest.test_from_examples_empty_examples_allow_empty_extraction_objects` | 函数/方法 | [474–486](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L474-L486) | 待讲解 |
| `OpenAISchemaTest.test_validate_format_rejects_yaml` | 函数/方法 | [488–497](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L488-L497) | 待讲解 |
| `OpenAISchemaTest.test_requires_raw_output_returns_true` | 函数/方法 | [499–503](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L499-L503) | 待讲解 |
| `OpenAISchemaTest.test_validate_format_warns_when_fences_enabled` | 函数/方法 | [505–516](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L505-L516) | 待讲解 |
| `OpenAISchemaTest.test_validate_format_warns_with_wrong_wrapper_key` | 函数/方法 | [518–531](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L518-L531) | 待讲解 |
| `OpenAISchemaTest.test_from_examples_preserves_scalar_attribute_types` | 函数/方法 | [533–568](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L533-L568) | 待讲解 |
| `OpenAISchemaTest.test_from_examples_preserves_mixed_numeric_attribute_types` | 函数/方法 | [570–619](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L570-L619) | 待讲解 |
| `OpenAISchemaTest.test_from_examples_allows_none_attribute_values` | 函数/方法 | [621–641](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L621-L641) | 待讲解 |
| `OpenAISchemaTest.test_from_examples_strict_false_emits_non_strict_response_format` | 函数/方法 | [643–647](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L643-L647) | 待讲解 |
| `OpenAISchemaTest.test_response_format_returns_isolated_schema_dict` | 函数/方法 | [649–658](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L649-L658) | 待讲解 |
| `OpenAISchemaTest.test_instance_is_frozen_and_dict_is_isolated` | 函数/方法 | [660–676](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L660-L676) | 待讲解 |
| `SchemaValidationTest` | 类 | [679–764](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L679-L764) | 待讲解 |
| `SchemaValidationTest._create_test_schema` | 函数/方法 | [682–695](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L682-L695) | 待讲解 |
| `SchemaValidationTest.test_gemini_validation` | 函数/方法 | [697–748](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L697-L748) | 待讲解 |
| `SchemaValidationTest.test_base_schema_no_validation` | 函数/方法 | [750–764](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L750-L764) | 待讲解 |
| `OutputSchemaHelperTest` | 类 | [767–846](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L767-L846) | 待讲解 |
| `OutputSchemaHelperTest.test_extraction_item_schema_without_attributes` | 函数/方法 | [770–781](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L770-L781) | 待讲解 |
| `OutputSchemaHelperTest.test_extraction_item_schema_with_attributes` | 函数/方法 | [783–800](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L783-L800) | 待讲解 |
| `OutputSchemaHelperTest.test_extractions_schema_wraps_item_schema` | 函数/方法 | [802–812](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L802-L812) | 待讲解 |
| `OutputSchemaHelperTest.test_extractions_schema_wraps_multiple_items_in_any_of` | 函数/方法 | [814–824](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L814-L824) | 待讲解 |
| `OutputSchemaHelperTest.test_builders_copy_input_schemas` | 函数/方法 | [826–837](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L826-L837) | 待讲解 |
| `OutputSchemaHelperTest.test_extraction_item_schema_rejects_invalid_class` | 函数/方法 | [839–846](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L839-L846) | 待讲解 |
| `OutputSchemaValidationTest` | 类 | [849–955](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L849-L955) | 待讲解 |
| `OutputSchemaValidationTest.test_accepts_envelope_and_returns_isolated_copy` | 函数/方法 | [852–861](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L852-L861) | 待讲解 |
| `OutputSchemaValidationTest.test_rejects_invalid_envelopes` | 函数/方法 | [863–955](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L863-L955) | 待讲解 |
| `FromSchemaDictTest` | 类 | [958–1011](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L958-L1011) | 待讲解 |
| `FromSchemaDictTest.setUp` | 函数/方法 | [961–970](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L961-L970) | 待讲解 |
| `FromSchemaDictTest.test_base_schema_rejects_user_schemas_by_default` | 函数/方法 | [972–974](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L972-L974) | 待讲解 |
| `FromSchemaDictTest.test_gemini_from_schema_dict_targets_json_schema_field` | 函数/方法 | [976–984](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L976-L984) | 待讲解 |
| `FromSchemaDictTest.test_gemini_from_schema_dict_validates_envelope` | 函数/方法 | [986–994](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L986-L994) | 待讲解 |
| `FromSchemaDictTest.test_openai_from_schema_dict_builds_response_format` | 函数/方法 | [996–1005](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L996-L1005) | 待讲解 |
| `FromSchemaDictTest.test_openai_from_schema_dict_rejects_invalid_schema_name` | 函数/方法 | [1007–1011](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/schema_test.py#L1007-L1011) | 待讲解 |

## tests/test_gemini_batch_api.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `create_mock_batch_job` | 函数/方法 | [35–47](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L35-L47) | 待讲解 |
| `_create_batch_response` | 函数/方法 | [50–59](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L50-L59) | 待讲解 |
| `_create_batch_error` | 函数/方法 | [62–67](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L62-L67) | 待讲解 |
| `TestGeminiBatchAPI` | 类 | [70–569](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L70-L569) | 待讲解 |
| `TestGeminiBatchAPI.setUp` | 函数/方法 | [73–80](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L73-L80) | 待讲解 |
| `TestGeminiBatchAPI.test_batch_routing_vertex` | 函数/方法 | [82–126](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L82-L126) | 待讲解 |
| `TestGeminiBatchAPI.test_realtime_when_disabled` | 函数/方法 | [128–151](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L128-L151) | 待讲解 |
| `TestGeminiBatchAPI.test_realtime_when_below_threshold` | 函数/方法 | [153–181](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L153-L181) | 待讲解 |
| `TestGeminiBatchAPI.test_batch_forwards_constructor_generation_params` | 函数/方法 | [183–231](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L183-L231) | 待讲解 |
| `TestGeminiBatchAPI.test_batch_schema_runtime_generation_params_override_constructor` | 函数/方法 | [233–314](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L233-L314) | 待讲解 |
| `TestGeminiBatchAPI.test_batch_runtime_none_clears_constructor_generation_params` | 函数/方法 | [316–353](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L316-L353) | 待讲解 |
| `TestGeminiBatchAPI.test_batch_runtime_none_clears_only_selected_generation_param` | 函数/方法 | [355–386](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L355-L386) | 待讲解 |
| `TestGeminiBatchAPI.test_batch_error_handling` | 函数/方法 | [388–409](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L388-L409) | 待讲解 |
| `TestGeminiBatchAPI.test_file_based_ordering` | 函数/方法 | [411–454](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L411-L454) | 待讲解 |
| `TestGeminiBatchAPI.test_max_prompts_per_job` | 函数/方法 | [456–536](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L456-L536) | 待讲解 |
| `TestGeminiBatchAPI.test_max_prompts_per_job.list_blobs_side_effect` | 函数/方法 | [498–505](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L498-L505) | 待讲解 |
| `TestGeminiBatchAPI.test_batch_item_error` | 函数/方法 | [538–569](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L538-L569) | 待讲解 |
| `BatchParseItemTest` | 类 | [572–899](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L572-L899) | 待讲解 |
| `BatchParseItemTest.test_vertex_status_error_raises` | 函数/方法 | [574–587](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L574-L587) | 待讲解 |
| `BatchParseItemTest.test_error_field_raises_with_provider` | 函数/方法 | [589–601](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L589-L601) | 待讲解 |
| `BatchParseItemTest.test_empty_vertex_status_preserves_text` | 函数/方法 | [603–610](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L603-L610) | 待讲解 |
| `BatchParseItemTest.test_ignored_vertex_status_preserves_indices` | 函数/方法 | [612–622](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L612-L622) | 待讲解 |
| `BatchParseItemTest.test_ignored_item_failure_is_logged` | 函数/方法 | [624–659](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L624-L659) | 待讲解 |
| `BatchParseItemTest.test_truncated_item_is_kept_and_logged` | 函数/方法 | [661–694](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L661-L694) | 待讲解 |
| `BatchParseItemTest.test_complete_item_is_not_logged` | 函数/方法 | [696–716](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L696-L716) | 待讲解 |
| `BatchParseItemTest.test_is_max_tokens_finish_accepts_finish_reason_enum` | 函数/方法 | [718–723](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L718-L723) | 待讲解 |
| `BatchParseItemTest.test_blocked_item_raises` | 函数/方法 | [725–806](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L725-L806) | 待讲解 |
| `BatchParseItemTest.test_no_valid_diagnostic_preserves_empty_output` | 函数/方法 | [808–869](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L808-L869) | 待讲解 |
| `BatchParseItemTest.test_nonempty_text_with_abnormal_finish_is_preserved` | 函数/方法 | [871–885](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L871-L885) | 待讲解 |
| `BatchParseItemTest.test_ignore_errors_preserves_other_rows_and_indices` | 函数/方法 | [887–899](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L887-L899) | 待讲解 |
| `BatchConfigValidationTest` | 类 | [902–914](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L902-L914) | 待讲解 |
| `BatchConfigValidationTest.test_validation_errors` | 函数/方法 | [905–914](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L905-L914) | 待讲解 |
| `EmptyAndPaddingTest` | 类 | [917–972](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L917-L972) | 待讲解 |
| `EmptyAndPaddingTest.test_empty_prompts_fast_path` | 函数/方法 | [920–936](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L920-L936) | 待讲解 |
| `EmptyAndPaddingTest.test_file_pad_to_expected_count` | 函数/方法 | [938–972](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L938-L972) | 待讲解 |
| `GCSBatchCachingTest` | 类 | [975–1377](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L975-L1377) | 待讲解 |
| `GCSBatchCachingTest.setUp` | 函数/方法 | [978–985](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L978-L985) | 待讲解 |
| `GCSBatchCachingTest.test_typed_config_serializes_to_uploaded_jsonl` | 函数/方法 | [987–1039](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L987-L1039) | 待讲解 |
| `GCSBatchCachingTest.test_typed_config_serializes_to_uploaded_jsonl.capture_upload` | 函数/方法 | [1009–1011](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1009-L1011) | 待讲解 |
| `GCSBatchCachingTest.test_cache_hashing_serializes_typed_thinking_config` | 函数/方法 | [1041–1052](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1041-L1052) | 待讲解 |
| `GCSBatchCachingTest.test_infer_batch_preserves_positional_project_location` | 函数/方法 | [1054–1083](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1054-L1083) | 待讲解 |
| `GCSBatchCachingTest.test_cache_hit_skips_inference` | 函数/方法 | [1085–1129](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1085-L1129) | 待讲解 |
| `GCSBatchCachingTest.test_strict_blocked_item_raises_before_cache_write` | 函数/方法 | [1131–1174](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1131-L1174) | 待讲解 |
| `GCSBatchCachingTest.test_strict_mode_rechecks_only_empty_cache_entries` | 函数/方法 | [1176–1215](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1176-L1215) | 待讲解 |
| `GCSBatchCachingTest.test_partial_cache_hit` | 函数/方法 | [1217–1286](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1217-L1286) | 待讲解 |
| `GCSBatchCachingTest.test_partial_cache_hit.get_blob` | 函数/方法 | [1239–1242](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1239-L1242) | 待讲解 |
| `GCSBatchCachingTest.test_project_passed_to_storage_client` | 函数/方法 | [1288–1343](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1288-L1343) | 待讲解 |
| `GCSBatchCachingTest.test_cache_hashing_stability` | 函数/方法 | [1345–1350](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1345-L1350) | 待讲解 |
| `GCSBatchCachingTest.test_cache_hashing_serializes_enum_and_dataclass` | 函数/方法 | [1352–1377](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1352-L1377) | 待讲解 |
| `GCSBatchCachingTest.test_cache_hashing_serializes_enum_and_dataclass._SafetyLevel` | 类 | [1355–1356](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1355-L1356) | 待讲解 |
| `GCSBatchCachingTest.test_cache_hashing_serializes_enum_and_dataclass._SafetySetting` | 类 | [1358–1361](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1358-L1361) | 待讲解 |
| `BatchOutputSchemaRequestTest` | 类 | [1380–1554](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1380-L1554) | 待讲解 |
| `BatchOutputSchemaRequestTest.test_build_request_omits_generation_config_without_values` | 函数/方法 | [1383–1386](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1383-L1386) | 待讲解 |
| `BatchOutputSchemaRequestTest.test_build_request_lowers_json_schema_config` | 函数/方法 | [1388–1402](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1388-L1402) | 待讲解 |
| `BatchOutputSchemaRequestTest.test_build_request_lowers_response_schema_config` | 函数/方法 | [1404–1416](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1404-L1416) | 待讲解 |
| `BatchOutputSchemaRequestTest.test_batch_with_output_schema_uses_json_schema_field` | 函数/方法 | [1418–1475](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1418-L1475) | 待讲解 |
| `BatchOutputSchemaRequestTest.test_build_request_places_tools_at_top_level` | 函数/方法 | [1477–1493](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1477-L1493) | 待讲解 |
| `BatchOutputSchemaRequestTest.test_build_request_lifts_tools_out_of_gen_config` | 函数/方法 | [1495–1504](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1495-L1504) | 待讲解 |
| `BatchOutputSchemaRequestTest.test_batch_places_tools_at_top_level_and_forwards_thinking_config` | 函数/方法 | [1506–1554](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1506-L1554) | 待讲解 |
| `BatchToolEncodingTest` | 类 | [1557–1635](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1557-L1635) | 待讲解 |
| `BatchToolEncodingTest.test_default_google_search_gets_empty_exclusion_list` | 函数/方法 | [1560–1582](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1560-L1582) | 待讲解 |
| `BatchToolEncodingTest.test_configured_google_search_is_unchanged` | 函数/方法 | [1584–1589](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1584-L1589) | 待讲解 |
| `BatchToolEncodingTest.test_caller_tools_are_not_modified` | 函数/方法 | [1591–1596](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1591-L1596) | 待讲解 |
| `BatchToolEncodingTest.test_other_empty_tool_config_is_rejected` | 函数/方法 | [1598–1614](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1598-L1614) | 待讲解 |
| `BatchToolEncodingTest.test_rejected_tool_fails_before_upload` | 函数/方法 | [1616–1635](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1616-L1635) | 待讲解 |
| `BatchHttpLoggerSuppressionTest` | 类 | [1638–1714](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1638-L1714) | 待讲解 |
| `BatchHttpLoggerSuppressionTest.setUp` | 函数/方法 | [1641–1662](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1641-L1662) | 待讲解 |
| `BatchHttpLoggerSuppressionTest._get_clean_logger` | 函数/方法 | [1664–1672](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1664-L1672) | 待讲解 |
| `BatchHttpLoggerSuppressionTest.test_infer_batch_sets_transport_logger_to_warning` | 函数/方法 | [1674–1694](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1674-L1694) | 待讲解 |
| `BatchHttpLoggerSuppressionTest.test_infer_batch_disables_httpx_logger` | 函数/方法 | [1696–1714](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_gemini_batch_api.py#L1696-L1714) | 待讲解 |

## tests/test_kwargs_passthrough.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_configure_openai_mock` | 函数/方法 | [33–43](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L33-L43) | 待讲解 |
| `_condition_examples` | 函数/方法 | [46–58](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L46-L58) | 待讲解 |
| `TestOpenAIBatchKwargsPassthrough` | 类 | [61–168](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L61-L168) | 待讲解 |
| `TestOpenAIBatchKwargsPassthrough.test_infer_batch_reuses_structured_output_params` | 函数/方法 | [64–94](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L64-L94) | 待讲解 |
| `TestOpenAIBatchKwargsPassthrough.test_infer_batch_rejects_invalid_batch_size` | 函数/方法 | [96–108](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L96-L108) | 待讲解 |
| `TestOpenAIBatchKwargsPassthrough.test_infer_propagates_batch_size_config_error` | 函数/方法 | [110–128](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L110-L128) | 待讲解 |
| `TestOpenAIBatchKwargsPassthrough.test_batch_config_does_not_leak_to_chat_completions` | 函数/方法 | [130–147](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L130-L147) | 待讲解 |
| `TestOpenAIBatchKwargsPassthrough.test_batch_mode_logs_when_below_threshold` | 函数/方法 | [149–168](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L149-L168) | 待讲解 |
| `TestOpenAIKwargsPassthrough` | 类 | [171–531](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L171-L531) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_reasoning_effort_passed_as_top_level` | 函数/方法 | [174–189](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L174-L189) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_runtime_reasoning_effort_override` | 函数/方法 | [191–205](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L191-L205) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_runtime_kwargs_override_stored` | 函数/方法 | [207–228](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L207-L228) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_falsy_values_preserved` | 函数/方法 | [230–251](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L230-L251) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_reasoning_effort_not_nested` | 函数/方法 | [253–268](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L253-L268) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_custom_response_format` | 函数/方法 | [270–292](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L270-L292) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_schema_response_format_passed_to_chat_completion` | 函数/方法 | [294–328](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L294-L328) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_runtime_response_format_overrides_schema` | 函数/方法 | [330–355](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L330-L355) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_apply_schema_rejects_non_openai_schema` | 函数/方法 | [357–374](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L357-L374) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_apply_schema_rejection_preserves_prior_schema` | 函数/方法 | [376–397](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L376-L397) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_apply_schema_none_clears_response_format` | 函数/方法 | [399–419](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L399-L419) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_factory_schema_clear_removes_response_format` | 函数/方法 | [421–448](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L421-L448) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_apply_schema_none_preserves_explicit_fence_output` | 函数/方法 | [450–465](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L450-L465) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_apply_schema_rejects_yaml_format` | 函数/方法 | [467–484](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L467-L484) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_constructor_schema_populates_public_schema` | 函数/方法 | [486–499](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L486-L499) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_inference_preserves_schema_config_error` | 函数/方法 | [501–516](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L501-L516) | 待讲解 |
| `TestOpenAIKwargsPassthrough.test_reasoning_not_in_chat_completions` | 函数/方法 | [518–531](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L518-L531) | 待讲解 |
| `TestOllamaAuthSupport` | 类 | [534–791](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L534-L791) | 待讲解 |
| `TestOllamaAuthSupport.test_api_key_in_authorization_header` | 函数/方法 | [537–557](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L537-L557) | 待讲解 |
| `TestOllamaAuthSupport.test_custom_auth_header_name` | 函数/方法 | [559–579](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L559-L579) | 待讲解 |
| `TestOllamaAuthSupport.test_pass_through_kwargs` | 函数/方法 | [581–606](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L581-L606) | 待讲解 |
| `TestOllamaAuthSupport.test_api_key_redacted_in_repr` | 函数/方法 | [608–619](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L608-L619) | 待讲解 |
| `TestOllamaAuthSupport.test_localhost_auth_warning_but_still_works` | 函数/方法 | [621–645](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L621-L645) | 待讲解 |
| `TestOllamaAuthSupport.test_runtime_kwargs_override` | 函数/方法 | [647–668](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L647-L668) | 待讲解 |
| `TestOllamaAuthSupport.test_localhost_detection` | 函数/方法 | [670–701](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L670-L701) | 待讲解 |
| `TestOllamaAuthSupport.test_format_none_not_in_payload` | 函数/方法 | [703–726](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L703-L726) | 待讲解 |
| `TestOllamaAuthSupport.test_reserved_kwargs_not_in_options` | 函数/方法 | [728–754](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L728-L754) | 待讲解 |
| `TestOllamaAuthSupport.test_max_workers_not_in_options` | 函数/方法 | [756–767](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L756-L767) | 待讲解 |
| `TestOllamaAuthSupport.test_api_key_without_localhost_warning` | 函数/方法 | [769–791](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_kwargs_passthrough.py#L769-L791) | 待讲解 |

## tests/test_live_api.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `has_vertex_ai_credentials` | 函数/方法 | [63–71](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L63-L71) | 待讲解 |
| `retry_on_transient_errors` | 函数/方法 | [127–168](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L127-L168) | 待讲解 |
| `retry_on_transient_errors.decorator` | 函数/方法 | [135–166](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L135-L166) | 待讲解 |
| `retry_on_transient_errors.decorator.wrapper` | 函数/方法 | [136–164](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L136-L164) | 待讲解 |
| `add_delay_between_tests` | 函数/方法 | [171–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L171-L175) | 待讲解 |
| `get_basic_medication_examples` | 函数/方法 | [178–204](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L178-L204) | 待讲解 |
| `get_relationship_examples` | 函数/方法 | [207–255](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L207-L255) | 待讲解 |
| `extract_by_class` | 函数/方法 | [258–267](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L258-L267) | 待讲解 |
| `assert_extractions_contain` | 函数/方法 | [270–281](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L270-L281) | 待讲解 |
| `assert_valid_char_intervals` | 函数/方法 | [284–307](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L284-L307) | 待讲解 |
| `TestLiveAPIGemini` | 类 | [310–741](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L310-L741) | 待讲解 |
| `TestLiveAPIGemini.test_medication_extraction` | 函数/方法 | [313–374](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L313-L374) | 待讲解 |
| `TestLiveAPIGemini.test_multilingual_medication_extraction` | 函数/方法 | [376–425](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L376-L425) | 待讲解 |
| `TestLiveAPIGemini.test_explicit_provider_gemini` | 函数/方法 | [427–454](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L427-L454) | 待讲解 |
| `TestLiveAPIGemini.test_medication_relationship_extraction` | 函数/方法 | [456–516](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L456-L516) | 待讲解 |
| `TestLiveAPIGemini.test_batch_extraction_vertex_gcs` | 函数/方法 | [518–627](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L518-L627) | 待讲解 |
| `TestLiveAPIGemini.test_batch_caching_live` | 函数/方法 | [629–698](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L629-L698) | 待讲解 |
| `TestLiveAPIGemini.test_batch_tools_thinking_and_cache_live` | 函数/方法 | [700–741](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L700-L741) | 待讲解 |
| `TestCrossChunkContext` | 类 | [744–816](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L744-L816) | 待讲解 |
| `TestCrossChunkContext.test_context_window_extracts_from_both_chunks` | 函数/方法 | [747–816](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L747-L816) | 待讲解 |
| `TestLiveAPIOpenAI` | 类 | [819–1147](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L819-L1147) | 待讲解 |
| `TestLiveAPIOpenAI.test_batch_extraction_uses_openai_batch_api` | 函数/方法 | [822–878](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L822-L878) | 待讲解 |
| `TestLiveAPIOpenAI.test_medication_extraction` | 函数/方法 | [880–942](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L880-L942) | 待讲解 |
| `TestLiveAPIOpenAI.test_medication_extraction_with_schema_constraints` | 函数/方法 | [944–1022](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L944-L1022) | 待讲解 |
| `TestLiveAPIOpenAI.test_explicit_provider_selection` | 函数/方法 | [1024–1056](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L1024-L1056) | 待讲解 |
| `TestLiveAPIOpenAI.test_medication_relationship_extraction` | 函数/方法 | [1058–1119](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L1058-L1119) | 待讲解 |
| `TestLiveAPIOpenAI.test_reasoning_effort_passthrough` | 函数/方法 | [1121–1147](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L1121-L1147) | 待讲解 |
| `TestLiveAPIOutputSchema` | 类 | [1150–1222](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L1150-L1222) | 待讲解 |
| `TestLiveAPIOutputSchema._assert_schema_constrained_extractions` | 函数/方法 | [1184–1192](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L1184-L1192) | 待讲解 |
| `TestLiveAPIOutputSchema.test_gemini_extract_with_output_schema` | 函数/方法 | [1194–1207](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L1194-L1207) | 待讲解 |
| `TestLiveAPIOutputSchema.test_openai_extract_with_output_schema` | 函数/方法 | [1209–1222](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_live_api.py#L1209-L1222) | 待讲解 |

## tests/test_ollama_integration.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_ollama_available` | 函数/方法 | [26–30](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_ollama_integration.py#L26-L30) | 待讲解 |
| `test_ollama_provider_via_model_config_must_be_first_test` | 函数/方法 | [33–91](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_ollama_integration.py#L33-L91) | 待讲解 |
| `test_ollama_extraction` | 函数/方法 | [94–135](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_ollama_integration.py#L94-L135) | 待讲解 |
| `test_ollama_extraction_with_fence_fallback` | 函数/方法 | [138–175](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_ollama_integration.py#L138-L175) | 待讲解 |
| `_model_available` | 函数/方法 | [178–187](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_ollama_integration.py#L178-L187) | 待讲解 |
| `test_gpt_oss_20b_extraction` | 函数/方法 | [190–230](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_ollama_integration.py#L190-L230) | 待讲解 |
| `test_deepseek_r1_extraction` | 函数/方法 | [233–271](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/test_ollama_integration.py#L233-L271) | 待讲解 |

## tests/tokenizer_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `TokenizerTest` | 类 | [23–225](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L23-L225) | 待讲解 |
| `TokenizerTest.assertTokenListEqual` | 函数/方法 | [26–43](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L26-L43) | 待讲解 |
| `TokenizerTest.test_tokenize_various_inputs` | 函数/方法 | [45–156](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L45-L156) | 待讲解 |
| `TokenizerTest.test_first_token_after_newline_flag` | 函数/方法 | [158–195](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L158-L195) | 待讲解 |
| `TokenizerTest.test_performance_optimization_no_crash` | 函数/方法 | [197–211](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L197-L211) | 待讲解 |
| `TokenizerTest.test_underscore_handling` | 函数/方法 | [213–225](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L213-L225) | 待讲解 |
| `UnicodeTokenizerTest` | 类 | [228–626](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L228-L626) | 待讲解 |
| `UnicodeTokenizerTest.assertTokenListEqual` | 函数/方法 | [231–248](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L231-L248) | 待讲解 |
| `UnicodeTokenizerTest.test_tokenize_various_inputs` | 函数/方法 | [250–302](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L250-L302) | 待讲解 |
| `UnicodeTokenizerTest.test_special_unicode_and_punctuation_handling` | 函数/方法 | [304–375](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L304-L375) | 待讲解 |
| `UnicodeTokenizerTest.test_first_token_after_newline_parity` | 函数/方法 | [377–389](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L377-L389) | 待讲解 |
| `UnicodeTokenizerTest.test_expanded_cjk_detection` | 函数/方法 | [391–399](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L391-L399) | 待讲解 |
| `UnicodeTokenizerTest.test_mixed_script_and_emoji` | 函数/方法 | [401–425](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L401-L425) | 待讲解 |
| `UnicodeTokenizerTest.test_script_boundary_grouping` | 函数/方法 | [427–449](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L427-L449) | 待讲解 |
| `UnicodeTokenizerTest.test_non_spaced_scripts_no_grouping` | 函数/方法 | [451–460](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L451-L460) | 待讲解 |
| `UnicodeTokenizerTest.test_cjk_detection_regex` | 函数/方法 | [462–470](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L462-L470) | 待讲解 |
| `UnicodeTokenizerTest.test_newline_simplification` | 函数/方法 | [472–480](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L472-L480) | 待讲解 |
| `UnicodeTokenizerTest.test_newline_simplification_start` | 函数/方法 | [482–489](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L482-L489) | 待讲解 |
| `UnicodeTokenizerTest.test_mixed_line_endings` | 函数/方法 | [491–499](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L491-L499) | 待讲解 |
| `UnicodeTokenizerTest.test_mixed_uncommon_scripts_no_grouping` | 函数/方法 | [501–516](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L501-L516) | 待讲解 |
| `UnicodeTokenizerTest.test_unknown_script_merging_edge_case` | 函数/方法 | [518–526](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L518-L526) | 待讲解 |
| `UnicodeTokenizerTest.test_find_sentence_range_empty_input` | 函数/方法 | [528–531](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L528-L531) | 待讲解 |
| `UnicodeTokenizerTest.test_normalization_indices_match_input` | 函数/方法 | [533–544](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L533-L544) | 待讲解 |
| `UnicodeTokenizerTest.test_acronym_inconsistency` | 函数/方法 | [546–557](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L546-L557) | 待讲解 |
| `UnicodeTokenizerTest.test_consecutive_punctuation_grouping` | 函数/方法 | [559–570](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L559-L570) | 待讲解 |
| `UnicodeTokenizerTest.test_punctuation_merging_identical_only` | 函数/方法 | [572–598](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L572-L598) | 待讲解 |
| `UnicodeTokenizerTest.test_distinct_unknown_scripts_do_not_merge` | 函数/方法 | [600–612](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L600-L612) | 待讲解 |
| `UnicodeTokenizerTest.test_identical_unknown_scripts_merge` | 函数/方法 | [614–626](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L614-L626) | 待讲解 |
| `ExceptionTest` | 类 | [629–681](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L629-L681) | 待讲解 |
| `ExceptionTest.test_invalid_token_interval_errors` | 函数/方法 | [632–660](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L632-L660) | 待讲解 |
| `ExceptionTest.test_sentence_range_errors` | 函数/方法 | [662–681](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L662-L681) | 待讲解 |
| `NegativeTestCases` | 类 | [684–799](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L684-L799) | 待讲解 |
| `NegativeTestCases.test_invalid_and_edge_case_unicode` | 函数/方法 | [687–775](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L687-L775) | 待讲解 |
| `NegativeTestCases.test_empty_string_edge_case` | 函数/方法 | [777–783](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L777-L783) | 待讲解 |
| `NegativeTestCases.test_whitespace_only_string` | 函数/方法 | [785–799](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L785-L799) | 待讲解 |
| `TokensTextTest` | 类 | [802–869](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L802-L869) | 待讲解 |
| `TokensTextTest.test_valid_intervals` | 函数/方法 | [806–841](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L806-L841) | 待讲解 |
| `TokensTextTest.test_invalid_intervals` | 函数/方法 | [843–869](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L843-L869) | 待讲解 |
| `SentenceRangeTest` | 类 | [872–1012](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L872-L1012) | 待讲解 |
| `SentenceRangeTest.test_partial_sentence_range` | 函数/方法 | [874–907](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L874-L907) | 待讲解 |
| `SentenceRangeTest.test_full_sentence_range` | 函数/方法 | [909–922](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L909-L922) | 待讲解 |
| `SentenceRangeTest.test_invalid_start_pos` | 函数/方法 | [924–940](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L924-L940) | 待讲解 |
| `SentenceRangeTest.test_sentence_boundary_with_quote` | 函数/方法 | [942–947](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L942-L947) | 待讲解 |
| `SentenceRangeTest.test_sentence_splitting_permissive` | 函数/方法 | [949–967](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L949-L967) | 待讲解 |
| `SentenceRangeTest.test_unicode_sentence_boundaries` | 函数/方法 | [969–983](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L969-L983) | 待讲解 |
| `SentenceRangeTest.test_configurable_sentence_splitting` | 函数/方法 | [985–1012](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/tokenizer_test.py#L985-L1012) | 待讲解 |

## tests/visualization_test.py

| 符号 | 类别 | 源码 | 教材 |
|---|---|---|---|
| `_markup_document` | 函数/方法 | [33–46](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L33-L46) | 待讲解 |
| `VisualizationTest` | 类 | [49–220](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L49-L220) | 待讲解 |
| `VisualizationTest.test_assign_colors_basic_assignment` | 函数/方法 | [51–73](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L51-L73) | 待讲解 |
| `VisualizationTest.test_build_highlighted_text_single_span_correct_html` | 函数/方法 | [75–94](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L75-L94) | 待讲解 |
| `VisualizationTest.test_build_highlighted_text_escapes_html_in_text_and_tooltip` | 函数/方法 | [96–119](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L96-L119) | 待讲解 |
| `VisualizationTest.test_visualize_basic_document_renders_correctly` | 函数/方法 | [121–161](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L121-L161) | 待讲解 |
| `VisualizationTest.test_visualize_no_extractions_renders_text_and_empty_legend` | 函数/方法 | [163–178](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L163-L178) | 待讲解 |
| `VisualizationTest.test_to_script_safe_json_escapes_markup_and_line_separators` | 函数/方法 | [180–191](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L180-L191) | 待讲解 |
| `VisualizationTest.test_visualize_markup_in_extraction_does_not_end_script_early` | 函数/方法 | [193–207](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L193-L207) | 待讲解 |
| `VisualizationTest.test_visualize_script_safe_json_round_trips_extraction_values` | 函数/方法 | [209–220](https://github.com/google/langextract/blob/b3499195e5ee711d3bb7da5178ba40456046f5f3/tests/visualization_test.py#L209-L220) | 待讲解 |
