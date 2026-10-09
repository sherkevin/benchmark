# E6 知会 — task-08：H2 llm_call 证据流已补齐（成本维度口径变更）

> 2026-08-10 · E4 → E6 · 关联：E6 登记 R3（H2 llm_calls=0/token 全 0）、PR-55 同源缺口。

## 结论一句话

H2（claude CLI）的逐次 LLM usage **可拿到**：CLI 加 `--include-partial-messages` 后每回合 `stream_event.message_delta` 带真实 usage；E4 已在 adapter 补 llm_call 证据流（与 H1 同结构），**task-08 之后的新 H2 跑成本维度可进汇总、可与 H1 对比**。

## 对你汇总口径的三点影响

1. **取数位置变了**：H2 token 不再取 `_response.json.run_metadata`（那是会话级总数，只作交叉校验列）；改为 `_full.jsonl` 的 llm_call 逐次累加（response.usage：input_tokens/output_tokens/cache_read_input_tokens/cache_creation_input_tokens）。E4 的 `t06_token_summary.py` 已按新口径实现，可参照。
2. **存量 H2 run 一律 null**：task-08 之前的 H2 产物（含 E6-07 抽验的那三道，若取自 E4 traces_new）raw 里没有 stream_event，逐次 usage 不可追溯重算——token 七项记 **null（拿不到）**，**不得以 run_metadata 会话级总数回填逐次字段**（那是 PR-55 的老路，owner 红线）。结果表这几列就让它空着。
3. **降级字段**：H2 llm_call 的 request/timestamp/latency_ms/attempt 为 null（CLI 不暴露，原因逐项登记在 dag_poc/trace_schema.json `compatibility.h2_llm_call_degraded`）；reasoning_tokens 也是 null（CLI 只给 thinking_tokens 估算值，非账单字段）。汇总遇到 null 请跳过不累加。

## 内置校验

对账脚本 `t05_adapter_reconcile.py` R10 已落盘：llm_call 逐次累加 vs result 会话级总数两路交叉校验（重跑实测四项一致）。你的汇总如与 E4 数字不符，先跑这条对账定位取数分歧。

## E4 不动你的代码

按纪律 E4 未触碰 src/eval_matrix/**；以上口径变更请你在矩阵汇总侧自行对齐。如需带开关重跑早期 H2 题以补成本列，链路脚本 t05_h2_run_cli.py 已就绪（封堵三件套 + R7 + partial-messages 全套在位）。
