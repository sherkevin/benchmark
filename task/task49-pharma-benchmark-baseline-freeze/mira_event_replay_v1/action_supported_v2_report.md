# Action-supported dynamic event recovery v2

日期：2026-10-01
状态：CPU development receipt 已完成；协议和指标核验通过；不进入 confirmation。

## 1. 实验和证据

本轮的动态范围固定为 `feedback-adaptive`：事件只改变 selected provider 的 delayed runtime-failure feedback，观察 release 后的状态和下一次 action；没有 provider arrival/retire、version drift 或自然环境非平稳事件。

本轮按运行前登记的 [v2 preregistration](action_supported_v2_preregistration.md) 执行。输入、snapshots、菜单、fold/seed/delay 和方法不变；唯一变化是使用 method-conditioned event target 和预登记的 v2 recovery estimands。

- target manifest：`experiments/logs/task49_mira_event_replay_cpu_20261001/action_supported_v4/target_manifest.json`，120 entries，独立结构与 provenance verifier `verified`；manifest self-hash=`06fade1ee298d68942665107f98064105f75904e9bd8c85b73b198f1e7c6f764`。
- event receipt：`experiments/logs/task49_mira_event_replay_cpu_20261001/action_supported_v2_full_v2/run/`。
- 规模：2 kernels × 4 methods × 5 folds × 2 seeds × 3 delays = 240 streams；34,656 decisions、104,208 event rows。
- schema/evaluator verifier：240/240 streams `verified`；relative-reliability 独立 action/state verifier：60/60 streams `verified`。
- v2 metric analyzer：240 method/event/control pairs，20,000 次按 fold 聚类 paired bootstrap，`metrics_v2_checked.json` 状态 `verified_metric_v2`；新增 event/control 在事件 tick 前的 provider、version、state hash、released feedback identity、menu 和 control-valid-terminal 检查。
- target binding：独立 verifier `action_supported_v4/verification.json` 状态 `verified`；manifest self-hash、manifest file hash、input hash、snapshot roster、实际共享 source hashes 和 declared commit 已与 full receipt 做 post-hoc 交叉核对，见 `action_supported_v2_full_v2/receipt_binding_posthoc_v2.json`。旧 binding 文件保留作历史记录。
- provenance warning：这条已完成 raw receipt 的历史 `run_id` 仍是 `task49_mira_action_supported_event_replay_unified_raw_20260930_v1`，与目录日期不一致；没有覆盖原始 receipt，后续 runner 已改为强制传入唯一 `run_id`。
- 首次短命令的目录曾被误记为半截工程失败；后续现场显示它最终写出了一个完整的重复 receipt。该目录不作为结果来源，原始记录和更正分别保留在 `action_supported_v2_full_v1/failed_attempt.json` 与 `completion_correction.json`；所有指标只来自 `action_supported_v2_full_v2/run/`。

## 2. 主要结果

表中 `Delta_adapt` 是 failure feedback release 后相对于同坐标 no-event stream 的配对 correctness 差值；`E_target` 是 release 后继续选择故障 target 的比例。点估计和 95% fold-cluster bootstrap 区间来自机器结果。

| scenario / method | delay | Delta_adapt | E_target |
|---|---:|---:|---:|
| transient / Frozen | 0/1/4 | 0 / 0 / 0 pp | 65.0 / 64.9 / 65.3% |
| transient / streak-only | 0/1/4 | 0 / 0 / 0 pp | 65.0 / 64.9 / 65.3% |
| transient / relative | 0/1/4 | 0 / 0 / 0 pp | 66.4 / 64.4 / 76.3% |
| transient / matched SGD | 0/1/4 | -0.98 / +0.99 / +1.33 pp | 0.8 / 8.0 / 9.4% |
| persistent / Frozen | 0/1/4 | -59.93 / -59.78 / -60.07 pp | 65.0 / 64.9 / 65.3% |
| persistent / streak-only | 0/1/4 | -1.54 / -1.13 / -1.48 pp | 1.4 / 1.0 / 1.6% |
| persistent / relative | 0/1/4 | -0.98 / -0.85 / -0.89 pp | 1.4 / 1.3 / 1.3% |
| persistent / matched SGD | 0/1/4 | -0.84 / +0.43 / +0.89 pp | 0.6 / 0.7 / 0.6% |

注入 terminal loss 单独记录；它不被解释为更新能力。delay=0 没有 blind window，按预注册记为 `NA`。persistent relative 在三个 delay 都比 streak-only 的 release 后恢复更好，但仍没有超过 matched SGD；relative 的 target exposure 约比 matched 高 0.6–0.8 个百分点，未达到预登记的 5 pp 缺口阈值。

## 3. 这轮真正说明了什么

1. **feedback-adaptive 协议成立**：selected-only delayed release、failure channel、release 后 state update 和 next-action 都能在 240 条 stream 中运行并恢复。
2. **偶发失败没有造成额外长期损失**：Frozen、streak-only、relative 的 transient `Delta_adapt` 都为 0；event tick 的一次性失败被单独拆出。
3. **持续失败能被动态方法处理**：relative 和 streak-only 都把 target exposure 从约 65% 降到约 1–2%，persistent recovery 明显优于 Frozen。
4. **relative 还没有击败最近邻**：matched SGD 在这批 persistent event 上恢复更快，relative 的优势是更保守的 stationary 行为，而不是已证明的 post-event superiority。

## 4. 验收边界

这轮补上了 M10 的 feedback-adaptive 事件证据、E4 的 failure/delay stress 和 M13 的 transient/persistent 机制分解入口。它仍然不能关闭 Gate C，原因有三点：

- target 是 method-conditioned incumbent，不同方法承受的 provider 不同；不能把跨方法 raw post accuracy 当共同干预排名；
- target family 仍高度集中：120 个 target 中 106 个来自 `b3clf`、10 个 `lgbm_bbb`、4 个 `admetlab`，且 30 个 fold/seed/delay 坐标没有四方法共同 target；只读分层见 [family stratification](action_supported_family_stratification_v1.md)，它没有把 method-conditioned target 变成共同干预；
- verifier 已独立核对 event schema 和 relative state；target prefix 的独立策略重放、common Frozen-incumbent secondary、propensity/探索和 queue restart 压力还没有形成最终主表。

因此本轮结论是 **dynamic mechanism diagnostic passed, paper effect claim not passed**。下一步不是申请 A800 或打开 confirmation，而是补一个共同 Frozen-incumbent secondary 和 target-family 分层，并把 baseline fairness contract 冻结后再决定是否需要远程主矩阵。
