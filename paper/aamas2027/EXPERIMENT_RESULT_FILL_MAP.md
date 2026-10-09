# Experiment result fill map

状态：内部结果已回填稿；2026-10-06 更新。

这份文件把论文中的每个结果占位符绑定到一个可恢复的实验收据。占位符只能由真实 raw JSONL、配置/输入/source hash、独立 recovery 和作者复核填入。开发诊断、手工抄数和预期值都不能直接回填。

## 回填顺序

1. 先关闭方法与公平性门禁：MIRA-Relay state-to-action stop test、ProbeNoUpdate/ProbeIndependentResidual/MIRA 同 schedule receipt、独立 verifier/recovery。
2. 再完成 E1--E5 的统一 receipt 和 `results.json`；每个数字同时保留分母、scaffold-cluster 区间和 failure/unknown/refusal 分母。
3. 最后执行冻结 confirmation E6。E6 的标签不能回流到 epoch、阈值、菜单、provider family 或论文 claim 的选择。
4. 按下面的映射逐表填入，运行全文 lint、独立重编译和匿名性检查；删除 pre-results banner 后才允许进入投稿门禁。

## 论文占位符映射

| 位置 | 需要填入的字段 | 唯一证据来源 | 验收条件 |
|---|---|---|---|
| Abstract | E2a MIRA-Relay system success；最强 matched control；E3 rescued/harmed | E2a/E3 final `results.json` + recovery report | 同 receipt、同预算、paired scaffold-cluster CI；不能用开发诊断 |
| Table `tab:baselines` | baseline 的 sealed-fit-state、feature/prior、license/audit 状态 | `baseline_freeze` manifest、各 baseline source/checkpoint hash | adapted 与 strict 行分开；Oracle 不进入 deployable ranking |
| Table `tab:matrix` | E1--E6 状态：`CLOSED`、`BLOCKED` 或带 receipt 的结果路径 | 每个 `task/<task>/report.md` 与 preregistration | 状态和结果文件一一对应；E4c/E6 无冻结 trace 时保留 `BLOCKED` |
| Table `tab:main-results` | `N`, calls, system success, paired Δ/95% CI, Fail/U/R, update p95 | E2a/E2b/E2c raw ledger、independent replayer | N 按 scored decisions；calls 含所有调用；严格行使用同 receipt Frozen control |
| Main comparison paragraph | MIRA-Relay success、Frozen success、paired interval、extra calls、strongest matched online control | 与 `tab:main-results` 同一 `results.json` | 文字与表格机器可比；若 gate 失败，改写为边界结论，不删除失败 |
| Table `tab:mechanism-results` | probes、post-release changes、rescued/harmed、paired Δ | E3 receipt + state-hash/action-change recovery | direct probes 排除在 changes 外；ProbeNoUpdate 是机制 control；报告 family permutation |
| Table `tab:voi-results` | calls、refusal rate、system utility、rescued/harmed | E4b one-step evidence receipt | 所有 planner 共用 first-call output、call/refusal cost、evidence budget |
| Error and Cost Analysis | family/endpoint/scaffold 错误、both-correct/wrong、switch-away、calls、p50/p95、state bytes、recovery time | E3/E5 raw event ledger + cost report | runtime error、chemical wrong、unknown、refusal 分开计数；不把 Python replay time写成 provider cost |
| E6 confirmation paragraph | confirmation primary metric、paired interval、冻结菜单/阈值/epoch 记录 | confirmation task report + sealed manifest | confirmation 标签只用于最终评估；无调参、无反馈回流；否则明确 `BLOCKED` |

## 证据文件最小字段

每个收据至少包含 `experiment_id`、源代码 commit/tree hash、输入 manifest hash、配置和 seed、GPU/软件环境、逐事件 `molecule/provider/action/read_set/status/output_label/reward/release_tick/state_hash`、聚合结果、cluster bootstrap 或 paired CI、失败列表和 recovery 版本。状态为 `BLOCKED` 的实验仍需保存阻塞原因和未满足的门禁。

## 当前明确不能回填的数值

已有 Best Fixed、Oracle、ContextProbe 和 Matched Online-SGD 数字属于开发诊断，不能填入主结果表或摘要。它们只能留在内部诊断记录中，直到相同 benchmark contract、同 receipt、同预算和独立 recovery 的正式实验完成。


## 2026-10-06 回填状态

| 位置 | 当前来源 | 状态 |
|---|---|---|
| Abstract / main comparison | `provider_fresh_context_natural_v1/report.md` + `provider_fresh_drift_stress_v1/report.md` | 已回填；natural null 与 controlled positive 分开表述 |
| `tab:main-results` | `experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/` | 已回填；Random/Best Fixed/Frozen/Relative/Context/SGD/LinUCB/Bayesian/Oracle 同一 fresh 分母 |
| `tab:mechanism-results` | `experiments/logs/task49_provider_fresh_drift_stress_20261006/v2_statehash_run2/` | 已回填；rescue/harm 和 scaffold CI 保留 |
| `tab:voi-results` | E4b receipt | 明确 OUT OF SCOPE，保留空值，不伪造 utility |
| E6 | `stage_b_scientific_submission_gate_v3/` | 在声明的一次调用范围内 PASS；post-call、hERG/AMES、full 33-provider model-fresh 仍不在范围 |

当前主稿不再依赖 `TBD` 数值。旧的回填顺序和占位说明保留作为历史流程记录，以上表格是当前生效的结果映射。


### 2026-10-06 state-hash closure

MIRA-Context 的关键在线计数器已进入 serialized state/hash；自然与 controlled receipts 按同一实现重跑并通过 contract/replay verification。验证器独立于 aggregation，但复用 runner primitives，因此正文不称其为第二套独立实现。
