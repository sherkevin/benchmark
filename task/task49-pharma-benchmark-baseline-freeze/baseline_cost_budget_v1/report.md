# Baseline cost and budget contract v2

日期：2026-10-01  
状态：开发期只读审计；2026-10-02 修正 Replay 成本 scope 和 family registry 指针；不改变 benchmark、方法角色或 confirmation。

## 这份表解决什么问题

当前 baseline 的 accuracy 已经有多条收据，但它们的输入表示、warm start、似然、更新规则和运行成本并不完全相同。只把 accuracy 放在一张排名表里，会把方法差异、先验差异和实现成本混在一起。

本表只做一件事：逐方法把这些条件写出来。所有字段都来自已有 receipt/config/metrics；没有记录的字段保留为 `null`，并在 `missing_fields` 中列出。脚本不重跑策略，也不读取 confirmation。

机器结果：[`table.json`](table.json)  
生成器：[`build_baseline_cost_budget_table_v1.py`](../../../src/probe_headless/build_baseline_cost_budget_table_v1.py)  
输入固定为 722 个 BBB development molecule、33 个 provider、`input_sha256=a73f1c…`、delay `{0,1,4}`。

## v2 证据指针修复

成本数字仍由原 common-stratum metrics 和各自的成本收据生成，没有重算策略或替换结果。
为了让验收复核能直接找到当前严格 raw contract，Frozen 和 Matched Online-SGD 行现在同时
指向 `baseline_verifier_p0_repair_v2/report.md` 与 Matched `full_v6/verification_v2.json`。
BayesianRouter 行的 `evidence` 保留 native-BT prior 的原始收据；另在独立的
`audit_evidence` 字段附上 `provenance_repaired_v1/verification_v2.json` 作为
**direct-prior raw contract audit**。该旁证只证明事件合同可复核，不能把 direct prior 改称
native prior，也不改变任何分层或比较权限。

### Replay-SGD 成本 scope 修正（2026-10-02）

v2 重新生成了 Replay-SGD 的成本字段。旧版本把 `recovered_v2/events.jsonl` 中
Frozen、Online-SGD、Replay-SGD 三类共 90 条 stream 的事件一起计算 p95，却把分母写成
Replay 的 30 条 stream；这会把不同 scope 的时间混在同一行。当前表只筛选
`stream_suffix=replay_sgd_matched` 的 30 条 Replay stream，源回执仍明确记录为 90 条
stream。修正后的 Replay-only 成本为 selection p50/p95=`44,409/50,713.9 ns`，
update p50/p95=`127,304/149,029.45 ns`。旧 pooled 数字保留在历史回执中，不再作为当前
成本证据。机器表中的 `budget.receipt_scope` 记录了这两个分母。

## 比较分层

| stratum | 方法 | 允许的比较 |
|---|---|---|
| `strict_common_prior_logistic` | Frozen direct、Matched Online-SGD | 同一 hidden-64、fit-only direct prior、selected-only feedback 合同下的主控制比较 |
| `strict_common_prior_candidate` | MIRA-relative、MIRA-FamilyGuard | 和控制比较；这是候选方法，不是外部 baseline |
| `strict_gaussian_reward` | reward-ridge Gaussian | 自己的 Gaussian Frozen/Online 成对比较；不和 logistic raw accuracy 混排 |
| `adapted_gaussian_reward` | Greedy、BayesianRouter、PILOT-adapted | 各自 report paired effect；不能放进 strict 主排名 |
| `protocol_anchor` / `fit_only_static` / `privileged_evaluator` | Random、Best Fixed、Oracle | 任务锚点、强固定参照和 evaluator 上界 |
| `static_full_panel` | MLP BCE、BT+CLS | 静态 full-panel 路由诊断，不回答在线更新 |

这张分层不会因为排除某一方法而增加验收分数；它只是规定哪些数字可以直接比较。

## 已测的统一成本

下表是同一 common-stratum event receipt 中可直接比较的数值。时间是本机 Python/cache replay 的策略开销，单位 ns；不是线上 provider 服务延迟、token 或金钱成本。`state_bytes_p95` 是事件账本中的数值状态大小。

| method | decisions | feedback | selection p95 ns | update p95 ns | state bytes p95 | task success |
|---|---:|---:|---:|---:|---:|---:|
| Frozen direct | 4,332 | 4,332 | 36,435 | 2,000 | 16,896 | 0.901662 |
| Matched Online-SGD | 4,332 | 4,332 | 35,333 | 13,209 | 16,896 | 0.892198 |
| MIRA-relative | 4,332 | 4,332 | 85,018 | 47,750 | 17,721 | 0.902816 |
| MIRA-FamilyGuard | 4,332 | 4,332 | 118,685 | 91,079 | 17,945 | 0.907664 |
| Random | 4,332 | 4,332 | 80,227 | 1,393 | MISSING | 0.844971 |

上述 common-stratum 行来自 `common_stratum_family_guard_20261001/full_v1/metrics_v2_external_registry.json`。该 receipt 的 unknown/refusal 均为 0；这只说明该开发 cohort 没有覆盖这两类事件，不能当成 refusal 能力结果。

Replay-SGD 不属于上面 common-stratum 六方法的同源主表。它来自 90-stream 历史 matched
receipt，其中 Replay 子集为 30 streams；因此只报告 Replay 子集的成本，不把它与当前
common-direct-prior Online-SGD 做未经配对的效果排名。

## 其他 receipt 的成本

统一 raw receipt 中，adapted 方法的事件成本也被单独抽出，但由于 state bytes 没有写进这些 receipt，不能补写结构推测：

| method | selection p95 ns | update p95 ns | state bytes | 成本收据 |
|---|---:|---:|---|---|
| reward-ridge Gaussian Online | 161,083 | 26,583 | MISSING | `reward_ridge_gaussian_full_v1/analysis.json` |
| Greedy-adapted | 168,034 | 29,833 | MISSING | `greedy_unified_raw_20260930/full_v1/events.jsonl` |
| BayesianRouter native-BT | 243,204 | 78,579 | MISSING | `bayesianrouter_native_bt_raw_20260930/full_v1/events.jsonl` |
| PILOT-adapted | 176,685 | 27,560 | MISSING | `pilot_unified_raw_20260930/full_v1/events.jsonl` |
| Replay-SGD（历史 BT prior，30/90 streams） | 50,714 | 149,029 | MISSING | `matched_sgd_development_20260927/recovered_v2/events.jsonl` |

`reward-ridge` 的 Online−Frozen 为 `−0.1154 pp`，95% scaffold-cluster CI 为 `[-1.1242,+0.9707] pp`；它说明语义自洽的 Gaussian 更新在当前 development cohort 没有稳定在线收益。这个结果不能改写成 MIRA 结果，也不能和 strict logistic raw accuracy 混排。

## 预算和信息边界

当前已核对的共同预算是：每次 decision 最多调用一个 provider；feedback 只来自 selected provider，释放时间为 `decision_tick + 1 + delay`；未选 provider output、future feedback 和 confirmation truth 不进入 selector。`new_provider_calls=0`，所有结果都来自已有 cache/replay。

仍未闭合的字段必须显式保留：

- adapted receipt 的统一 state-memory 账本；
- Best Fixed、Oracle 的事件级 selection/update 成本；Random 的 selection/update 已有事件账本，state bytes 仍缺；
- 静态 MLP/BT+CLS 的在线 feedback 预算；
- 所有方法同一字段顺序、同一 timing instrumentation 和同一 refusal/unknown full table；
- provider 服务本身的 wall-clock、token、金钱成本。

## 对验收的影响

这份表关闭了“不同 baseline 角色混在一起、成本字段散落在多份报告里”的审计问题，并修正了 Replay-SGD 的 pooled-cost 账本错误，补强 Benchmark+Baseline 的可复核性和 E5 成本入口。但它没有伪造缺失字段，也没有关闭：

1. Gate A 的 endpoint qualification、provider training membership 和最终 confirmation lineage；
2. Gate B 的全方法统一 raw contract、Laya/GraphRouter 范围和跨 stratum likelihood 公平性；
3. Gate C 的 refusal utility、完整 E1–E5 冻结矩阵、paired uncertainty 和 confirmation。

所以 Gate A/B/C 继续保持 `11/16`、`11/16`、`10/16 HOLD`。下一步不应再堆普通 baseline；应先决定哪些缺失字段必须在最终主表中闭合，再登记一条有明确科学问题的 MIRA 修复实验。

2026-10-04 provenance 审计补充：旧 Replay 收据已经能把 feature（数值容差内）和历史
`bt_cls.score.weight` 绑定到具体 fold/seed checkpoint，并确认它与当前 common-direct-prior
不同；因此它被明确归入 separate control，而不是继续作为“尚未判定”的 strict 行。旧收据
没有 state bytes，成本表仍保留 `MISSING`，不回填。

## 独立稽核

`baseline_cost_budget_fairness_audit_v1/` 是另一位审查者对同一问题的只读复核，专门检查 Replay-SGD、GraphRouter、Laya 和缺失字段。当前生效的机器表是本目录的 `table.json`（`baseline_cost_budget_contract_v2`）；独立稽核只作为 review，不产生第二套生效标准。两份材料的共同结论是：缺失字段保留 `MISSING`，adapted 方法按自身 paired effect 报告，Gate B/C 不上调。

adapted unified receipt 目前也有完整的 attempts/failure 分母：Greedy `23/4,332`、BayesianRouter `25/4,332`、PILOT `30/4,332`，unknown/refusal 均为 0；它们的 `task_success` 仍留空，因为本表不把 adapted raw accuracy 跨 stratum 排名。

2026-10-04 adapted receipt field audit 进一步核对了原始事件：四条 receipt 均有完整的
8,664 decision/terminal/feedback 计数、state hash、runtime status 和 failure feedback，
独立 verifier 均为 `verified`；但没有 state bytes，也没有逐事件 `unknown_reason` 或
`refusal_action`。机器收据为
`task/task49-pharma-benchmark-baseline-freeze/adapted_baseline_receipt_field_audit_v1/audit.json`。
因此 `state_bytes_p95` 和 refusal/unknown utility 继续保持 `MISSING`，不从哈希或模型维度推断。
