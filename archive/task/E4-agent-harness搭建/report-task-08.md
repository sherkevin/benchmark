# report-task-08：H2 的 llm_call 证据流（逐次 usage）

> task_id=25 / E4-08 · 收口报告。问题：E6-07 抽验 H2 三道 llm_calls=0、token 全 0，三层矩阵第三层（H1 vs H2）成本维度不可比。核心精神：**拿不到的字段写 null 并注明原因，不填 0；不用单元级总数假装逐次累加值**。

## 判断结论（第一产出）

**能拿到逐次 usage，但有开关前提**：claude CLI 的 stream-json 默认输出里逐次 usage 是 **0 占位**，只有会话结束 result 事件给总数；加上 `--include-partial-messages` 开关后，流中每个 LLM 回合的 `stream_event.message_delta` 携带**该回合真实 usage**（input/output/cache_read/cache_creation 四项）。已按"能拿到 → 补流"路径落地，同题重跑实测 14/14 对账全过、两路交叉校验一致。

## 三问回答

### ① CLI 到底给了什么（全部盘上实证，非推测）

**存量 raw（无开关）——逐次不可用**，扫 task-05/06/07 全部 3 条 H2 raw 流：

| raw 流 | assistant 事件数 | 带 usage 数 | 非零数 | result 会话级 usage |
|---|---|---|---|---|
| case_co_drug_01（task-07 轮） | 139 | 139 | **0** | input=10,134 / output=13,721 等七项齐 |
| case_co_drug_01（t07pre 轮） | 368 | 368 | **0** | 同上齐全 |
| case_dh_drug_01 | 323 | 323 | 3（同一时刻重复 3 条，不可靠） | 齐全 |

assistant 事件 usage 实况片段（每条都是 0 占位）：
```json
"usage": {"input_tokens": 0, "output_tokens": 0}
```
结论：**token 只存在于会话结束的 result 事件（整会话颗粒度）**；assistant 层 usage 字段存在但恒 0（dh 流 3 条非零为同一 message 重复事件，进一步证明该层不可作取数源）。

**加 `--include-partial-messages` 后——逐次可用**（最小探针 `/tmp/t08_probe_partial.jsonl` + 同题重跑 raw 双实证）。流中新增 `stream_event`，每回合结束时的 `message_delta` 带真实 usage：
```json
{"type": "stream_event", "event": {"type": "message_delta",
  "delta": {"stop_reason": "tool_use"},
  "usage": {"output_tokens": 270, "input_tokens": 10031,
            "cache_read_input_tokens": 0, "cache_creation_input_tokens": 21946}}}
```
颗粒度：**每个 LLM 回合一条**（重跑实测 26 回合 = 26 条 message_delta），不是整会话。

### ② 补流的代价（改哪里、回归风险）

| 落点 | 修改 | 性质 |
|---|---|---|
| `t05_h2_run_cli.py` claude 分支 | 命令行加 `--include-partial-messages` | **记录面**：只使 stdout 多出 stream_event 事件种类，不改工具调用/参数打包/模型行为 |
| `src/benchmark/cli_trace_adapter.py` | 新增 `_llm_call_from_delta()`；convert_cli_stream 解析 message_start（跟模型名）/message_delta（产 llm_call，与 tool_call 共享 seq 时序交织） | 只消费 CLI 已写出的流事件 |
| `t05_adapter_reconcile.py` | R1/R3-R9 按 type=='tool_call' 过滤（events 现混有 llm_call）；新增 R10a/b/c（条数/逐条/交叉校验）；存量流无 stream_event → INFO 跳过不算失败 | 对账脚本只读 |
| `dag_poc/trace_schema.json` | compatibility 新增 `h2_llm_call_degraded` 登记 H2 降级形态 | 文档 |
| `t06_token_summary.py` | H2 口径改为 llm_call 逐次累加；拿不到的 run 七项写 null；run_metadata 降为交叉校验参考列 | 汇总脚本 |
| `t05_h2_run_cli.py` 三处兼容 | ok 补齐跳过 llm_call、判分投影过滤加 type 条件、R7 校验改 e.get('tool') | 防 KeyError |

**降级形态（拿不到的字段写 null 并注明原因，不填 0）**：request=null（CLI 流不暴露逐次请求 payload）、timestamp=null（stream_event 外层无 timestamp，时序由 seq 保证）、latency_ms=null（无逐次耗时）、attempt=null（CLI 内部重试不可见，写 0 会误读为首发成功）、reasoning_tokens=null（CLI 只给 system.thinking_tokens 估算值，非账单字段）。逐项原因已登记 trace_schema.json `h2_llm_call_degraded`。

**回归风险评估（owner 最关心，实测兜底）**：
- 存量流零影响：无 stream_event → 不产 llm_call → 转换结果与 task-07 逐字等价（存量 raw 复跑对账 11 PASS/0 FAIL，R10 如实 INFO 跳过）；
- 同题重跑（case_co_drug_01×claude，316s，5/5 PASS exit 0）：tool_call 对账 57/57、R7 单层、收卷 submit_answer accepted、判分 exit 0（best=z000608，与上轮 dc 不同属模型答案波动，如实登记——管道行为未变）；
- 判分投影仍为纯 tool_call（evaluate 输入契约不变），llm_call 只进 _full 证据流（与 H1 双轨语义一致）；
- 封堵三件套（task-06）与 R7（task-07）全部在位并通过。

### ③ 封顶影响面（虽已补流，存量与边界如实列清）

**补流只对未来新跑生效，存量 H2 产物无法追溯**（raw 里没有 stream_event，不可重算）：

| 存量产物 | 成本列处理 |
|---|---|
| case_dh_drug_01×claude、case_co_drug_01×qodercli、task-08 前的 co×claude 各轮 | token 七项 **null（拿不到）**，汇总脚本已实测输出 null 并注明原因，不回填会话级总数 |

- 结果表：上述 run 的 prompt/completion/total/cached/reasoning/llm_calls/重试 列空（null），表注"H2 成本维度仅 task-08 后新跑可比"；
- 论文限定句建议：「H1 vs H2 的成本对比仅对双方均有逐次证据流的 run 成立；早期 H2 run 因 CLI 未开 partial-messages，成本维度记 null 不参与对比」；
- run_metadata 会话级总数（result.usage/modelUsage）保留在 `_response.json` 作交叉校验参考，**不进逐次汇总**。

## 交叉校验（验收 §7 预设复核路径）

两路独立取数：路 A=raw 中 message_delta 逐条累加（API 协议层事件）；路 B=result 事件会话级 usage（CLI 会话统计层）。R10c 实测四项一致：
```text
input=10,181 / output=11,747 / cache_read=1,038,550 / cache_creation=51,210
```
说明：两路来自同一底层 API 调用的两个独立聚合点（流式协议事件 vs 会话结束统计），取数路径相互独立、非同一字段拷贝——与 PR-55「把总数当逐次」性质不同。若未来 CLI 聚合口径变化致两路不等，R10c 会 FAIL 报出而非静默。

复跑命令：
```bash
./.venv/bin/python task/E4-agent-harness搭建/t05_adapter_reconcile.py task/E4-agent-harness搭建/traces_new/case_co_drug_01__claude_raw_stream.jsonl   # 14 PASS
./.venv/bin/python task/E4-agent-harness搭建/t06_token_summary.py   # H2 逐次累加 + 存量 null
```

## 环境

实验后已杀自起 :9000（复测 lsof 为空）；备份四件套 `*_t08pre.*` 留档；重跑输出 t08_rerun_output.txt。禁改清单（含 E6 的 src/eval_matrix/**）零触碰，E6 汇总口径知会见 e6_notice_task08.md。
