# matrix_runs —— 三层矩阵跑测落盘契约（task-05 §7）

> 本目录由 `src/eval_matrix/matrix_runner.py` 生成、`matrix_summarize.py` 汇总。
> 判分脚本与后来人只按本契约取数，不要猜层级。

> ## ⚠️ 操作规范（task-06 新定，2026-08-09）：读 `summary.json` 之前必须先重跑一次汇总脚本
>
> `./.venv/bin/python -m src.eval_matrix.matrix_summarize`
>
> `summary.json` 只是某次汇总的快照（教训：PR-52“H2 未进汇总”是误报，
> 实为凌晨快照时 H2 还没落盘）。不重跑直接读 = 读历史状态。

## 1. 目录结构（按四元组可定位）

```
matrix_runs/<模型名（/换_）>/<harness>/<case_id>/round_<NN>/
  ├── <case_id>__<model_tag>.jsonl          # 判分投影（纯 tool_call，与 evaluate.py 输入契约同形）
  ├── <case_id>__<model_tag>_full.jsonl     # 全量证据流（tool_call + llm_call 交织，含 type 字段）
  ├── <case_id>__<model_tag>_response.json  # 答卷 + submission_channel + token_usage 汇总
  └── run_meta.json                         # 单元状态（断点续跑判重依据）
matrix_runs/manifest.jsonl                  # 追加式总台账（每单元完成追加一行，PR-35 抗还原留痕）
matrix_runs/_server/<case_id>.p<port>.log   # 该 case 的 mock_server stdout/stderr（并发下按端口区分）
matrix_runs/_server/mcp_config_p<port>.json # H2 用 MCP 接入配置（指向对应端口）
matrix_runs/summary.json                    # matrix_summarize.py 的七维度汇总输出
matrix_runs/_batch_H1.log / _batch_H2.log   # 全量批次 nohup 主日志（matrix_launch.py 启动）
```

- 断点续跑判重：`run_meta.json.status == "done"` 的四元组 (模型, harness, case, round) 不重跑；`failed`（网络/限流）重启重跑覆盖。模型答错不算失败，照实记录。
- 所有落盘为 tmp+rename 原子写；本仓非 git 仓库，目录在 E6 写作区内。

## 2. usage / finish_reason 嵌套层级（监工实测教训：不在顶层）

全量流 `_full.jsonl` 每行一个事件。**llm_call 事件**（`"type": "llm_call"`）内：

| 字段 | 真实路径（事件为 `e`） |
|---|---|
| usage 总量 | `e.response.usage.{prompt_tokens, completion_tokens, total_tokens}` |
| 推理 token | `e.response.usage.completion_tokens_details.reasoning_tokens`（推理模型才有，可能缺省） |
| 缓存 token | `e.response.usage.prompt_tokens_details.cached_tokens`；idealab 另有**等价**兼容字段 `e.response.usage.cacheReadInputTokensCompatible`（同一个数的两种写法，逐条相等已实证）。**取数取两者 max，绝不相加**（task-06 §1：相加会翻倍，曾致占比 186.7%） |
| finish_reason | `e.response.choices[0].finish_reason` |
| 完整请求 | `e.request`（model/messages/tools/temperature/max_tokens 原样） |
| 索引与留痕 | `e.seq` / `e.attempt` / `e.timestamp` / `e.latency_ms` / `e.code_version` / `e.case_id` |

即：**usage/finish_reason 都嵌在 `response` 里，不是事件顶层**（与监工 dogfooding
试跑时 `call_log[0].response` 的教训同源）。汇总脚本 `matrix_summarize.py` 的
`_find_usage()` 做防御性双层探测（顶层 → response 下），契约以本表为准。

## 2b. H2 单元落盘契约（H1 差异点）

H2 走 claude CLI（E4 t05_h2_run_cli 同口径 + task-06 三层封堵），单元目录比 H1 多：

```
round_NN/
  ├── <case_id>__<model_tag>_raw_stream.jsonl  # CLI 原始 stream-json（对账基准，含 result 事件）
  └── _claude_config/                          # per-单元隔离配置目录（含鉴权 env 的
                                               # settings.json 用完即删，残留目录为空）
```

- **H2 trace 无 llm_call 事件**（adapter 只产 tool_call）：token 取 raw stream
  最后一次 `result` 事件的 `usage`（E4 对账 R8 口径），写入
  `run_meta.token_usage`（`source=raw_stream.result.usage(...)`）；
  `matrix_summarize.py` 对 H2 单元回落读 run_meta，不依赖 _full 流的 llm_call。
- **H2 prompt 口径更正（task-06 §1，2026-08-09）**：result.usage 的
  `input_tokens` 只是未命中缓存的增量（qwen 单元实测仅 162），全量
  prompt = `input_tokens + cache_read_input_tokens + cache_creation_input_tokens`；
  cached_tokens = `cache_read_input_tokens` 单字段。旧口径写过的两个
  H2 单元已按本口径从 raw_stream 重取修正（原值存
  `run_meta.token_usage.prev_token_usage` 留痕）。
- **缓存取数纪律（task-06 §1.3 + §6 返工 PR-55，owner 批准）**：
  **唯一取数路径 = 逐次累加 llm_call 的 usage**；run_meta.token_usage 是
  汇总字段，**绝不参与累加**，仅供单元级查询与交叉校验。H2 单元无
  llm_call 证据流 → token 不进 totals（汇总产物有 token_coverage_note
  如实标注）。教训：上轮曾把 H2 回落当“允许的第二取数路径”，run_meta
  值进总数致 cached 5,486,024/prompt 7,055,095/占比 0.7776（分子分母
  同时多加一份，占比落进 0~1 肉眼不可见）；正确值 cached 3,047,040 /
  prompt 4,363,389 / 占比 **0.6983**（监工逐次累加基准，已对上）。
  三条自动断言：①每单元 cached ≤ prompt（越界报错）；②单元级与
  run_meta 逐项对账（两边不同源：逐次累加 vs run_meta 字段；H2 无
  累加值不参与，不许自己对自己）；③独立复扫（另一实现重扫全部
  trace 重算四项总量 vs 汇总输出，拦双路径叠加）。负向测试已验证：
  故意注入双路径叠加，断言③确实报错。单测见
  `src/eval_matrix/test_matrix_summarize.py`（8 项）。
- `response.json` 另含 `run_metadata`（num_turns/total_cost_usd/duration_ms）与
  `cli_returncode`；H2 的 steps=num_turns、tool_calls_total=adapter 事件数。
- **环境隔离（冒烟实证教训）**：用户 ~/.claude 的插件（ponytail SessionStart
  hook）会注入 persona 导致模型不调 submit_answer 空卷；故 H2 用
  CLAUDE_CONFIG_DIR 指向 per-单元干净配置目录（仅拷鉴权 env，不带
  hooks/plugins）。
- 已知格位级不可用（留痕不静默改口径）：moonshot/kimi-k3 在 H2 anthropic
  协议通道 400 提供方错误（最小复现确证，非框架问题）；glm dh 在 H1
  ReadTimeout 连灭（见 progress.md 阻塞表）。

## 2c. 并发跑批契约（task-05 §3.1）

- `--workers N`：cell 级线程池，每单元独占端口 `--base-port + slot`，自起停
  本 case 的 mock server；H1/H2 两进程并行时用不同 base-port（9000/9100）。
- H1 单元在独立子进程（`_h1worker`）执行：harness 侧 verify_case_channel
  读本进程 env，子进程隔离避免多线程全局 env 串扰。
- 残留回收只杀占用目标端口的 mock_server（lsof 定位），不碰其他 worker。
- manifest 追加带线程锁；halt_on_empty 置位后未启动单元不再开跑。

## 3. run_meta.json 字段

`model / harness / case_id / round / template_type（type1/type2/open）/ status
（done/failed）/ started_at / finished_at / elapsed_s / steps / tool_calls_total /
tool_calls_ok / submission_channel / answer_empty（空卷标记）/ token_usage
（prompt/completion/total 分列 + llm_calls）/ trace_path / full_trace_path /
response_path / code_version`；failed 时附 `error`。

## 4. 空卷防线

`answer_empty=true` 或 `submission_channel=text_fallback` 的单元一律留痕不掩盖；
试跑用 `--halt-on-empty` 发现即停批。汇总的 `totals.empty_answer` 计数可查。
试跑实证（2026-08-08）：qwen3.7-max × case_dh_drug_01 空卷 1 份，根因是模型
submit_answer 的 payload 缺 open 类必填字段 `mol`（SCHEMA_VIOLATION 拒收后
未再修正），非交卷断链；全量流内 submit 尝试原样可查。

## 4b. 跑批防御（试跑实证后加固）

- 启动前检查 9000 端口：残留 mock_server 自动回收，非本框架进程占用拒绝启动
  （教训：残留占端口会让 readiness 误判就绪，中途拒连造成空卷假象）。
- harness 侧 `verify_case_channel` 校验 runner 本进程环境变量，open 类
  fail-closed：框架每 case 同步注入 DAGPOC_CASE_ID/DAGPOC_CASE_KIND。
- `llm_chat` 已知缺口（src/benchmark 不在本任务写作区，未改）：重试仅覆盖
  HTTP 状态码，requests 网络异常（ReadTimeout）不重试直接抛。试跑中 glm-5.2
  首调用大 prompt 多次 ReadTimeout 即此缺口 + 该模型首 token 慢叠加。

## 5. 成本口径

`matrix_summarize.py` 的 `PRICE_TABLE`：三付费模型单价当前为 None（仓库内无官方
计费表，来源栏如实标「待核实」），DogFooding 单价 0（ADR-0019）。核到实价只改
该表重跑即可复现。

注：`cached_tokens` 为各次调用缓存读量的累加（含 glm 兼容字段），同一前缀
多次命中会重复计，可大于 prompt_tokens 总和（试跑实测比值 1.45），命中率
仅作缓存行为参考、不当成本折扣用；计费若分缓存价，需另按供应商口径折算。
