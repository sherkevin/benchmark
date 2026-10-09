# Post-call verification/refusal on an existing BBB holdout v1

日期：2026-10-08（2026-10-08 correction amendment）
状态：**完成 secondary/post-hoc analysis；不满足 prospective untouched-confirmation gate。**

## 运行范围

这轮复用了此前已经检查过的 157 个 BBB 留出分子。它们与 development fit/validation rows 按 scaffold 分离，但不能再称为“另一套未触碰的 fresh confirmation”。provider checkpoint、fit-only provider 选择和阈值在本次 replay 前已冻结；本轮没有改 provider、split、阈值或 policy，也没有新增 provider/API/GPU 调用。

五个 development scaffold folds 各自选择一个 fit-only provider 和置信度阈值；四个 checkpoint-bound provider 为 `ecfp_logreg`、`ecfp_mlp`、`ecfp_random_forest`、`graph_message_passing`。confirmation truth 不进入策略计算和 policy receipt；runner 在同一进程加载 truth，当前证据不等于进程级隔离。每个 fold 都评估 always-accept、confidence-refuse、same-coverage random 和 always-refuse；本轮没有第二 provider escalation 或 online update。

## 结果

| policy | coverage | accepted accuracy | wrong | refusal | calls | $U(0.25,0.01)$ |
|---|---:|---:|---:|---:|---:|---:|
| always-accept | 100.00% | 88.15% | 93 | 0 | 785 | -0.1285 |
| confidence-refuse | 84.46% | **94.57%** | **36** | 122 | 785 | **-0.0947** |
| same-coverage random$^\dagger$ | 84.46% | 88.69% | 75 | 122 | 785 | -0.1444 |
| always-refuse | 0.00% | — | 0 | 785 | 785 | -0.2600 |

$n=785$ 表示 5 folds × 157 rows；同一个 confirmation molecule 会在五个 fold policy 中重复出现，因此不能当作 785 个独立分子。confidence-refuse 相对 same-coverage random 的 pooled accepted-accuracy 差为 **+5.88 pp**；五折都为正。按 124 个 scaffold cluster、10,000 次 bootstrap（seed 20261008），该差值的 95% CI 为 **[+2.77,+9.29] pp**。相对 always-accept 的差值为 +6.42 pp，CI 为 **[+3.34,+9.72] pp**。

效用定义为 $U=-(W+\rho R+\kappa C)/N$，其中 $W$ 是 accepted wrong，$R$ 是 refusal，$C$ 是 provider calls，$N$ 是 evaluation rows。固定 $(\rho,\kappa)=(0.25,0.01)$ 时，confidence-refuse 相对 always-accept 的点改善为 +0.0338，按同一 scaffold bootstrap 的 95% CI 为 **[+0.0088,+0.0609]**；相对 same-coverage random 的点改善为 +0.0497，CI 为 **[+0.0292,+0.0718]**。这些区间是对现有留出集的事后不确定性量化，不是新的 prospective estimate。

$^\dagger$ `same-coverage random` 是 evaluator-side conditional null：先读取 confidence gate 在完整 cohort 中接受的数量，再随机选取相同数量的位置。因此它用于 matched selection comparison，不是可部署的 online policy，也不提供随机种子稳健性估计。

## 证据完整性与修正

- policy receipt 3,140 行，evaluator ledger 3,140 行；每个 fold/policy/entity key 唯一，policy/evaluator 字段逐行绑定。
- evaluator 对 accepted 行用 sealed manifest truth 重算 correctness；refusal 行的 `answer_correct` 保持 null，不能进入 accepted-accuracy 分母。
- policy receipt 不含 truth、answer_correct、terminal、scaffold 或未选 provider 输出。corrected import-free audit 重新检查概率有限性、calls/status、动作、truth binding、重复 key 和完整的 3,140+3,140 行，并输出 `analysis_audit_v3/analysis_audit_v2.json`。
- 增强 verifier 已独立验证全部 3,140+3,140 行、232 个 fit/aggregate 字段和输入 hash；26 个测试通过，其中包括 22 个 corruption cases。实际 fit 阈值的 tie-inclusive 接受数为 `[355,386,393,386,384]`，与原结果一致，没有发现真实 tie 计数错误。错误预期曾导致首轮测试失败，保存在 `strict_audit_v3/runs/001`，修正测试后的通过结果在 `runs/002`。
- 原 v1 report 中把 random comparison 错用了 always-accept 的 CI；原文件保留为 `report_v1_original.md`，本报告使用正确区间。

## 原始运行的局限与失败留痕

- `smoke_v1/` 目录存在但没有完整运行日志；中断前上下文记录它因错误的 fit+eval 行数断言失败，该原因是回顾性记录，没有同期错误日志可核对。
- `smoke_v2/result.json` 的 always-accept 是 0/80 correct；该轮 evaluator 比较值类型错误，结果作废，原目录保留。后来的 smoke_v3 为 72/80，full_v1 使用修正 evaluator；全量 truth binding 另由独立 verifier 核对。
- full_v1 的配置和结果在运行末尾落盘；未满足原协议的运行前配置/流式日志要求，且缺少当时执行源文件的独立 hash。这是历史 provenance 缺口，不能由当前源码 hash 追溯填造。新增 analysis_audit_v3 在重算前保存 source snapshot、输入 hash、命令、软件环境和参数；它证明本次重算，不证明旧运行源文件完全可追溯。
- 原分析增强版首次尝试因 refusal correctness=null 和 terminal 字段名处理不当停止；修正后才得到 audit_v2 的结果。该错误来自分析器，不修改原始 policy/evaluator 文件。

## 判定与主稿边界

这轮不能更新“prospective fresh confirmation”或 E6 untouched-confirmation 的通过状态。它可以作为一个次级 post-call observation：在 provider 已返回概率后，fit-only confidence gate 在已有留出 cohort 上把低置信返回拒答，并在该 cohort 上显示更高 accepted accuracy 与固定 utility。它不证明 MIRA-Relay 的 online gain、自然 provider drift、第二 provider escalation 净收益、hERG/AMES 泛化，或端到端 LLM agent 最终答案提升。正文若保留该结果，必须使用“existing held-out cohort secondary analysis”的表述。

## 文件

- 执行前协议：本目录 `experiment_card.md`（原协议状态保留；本次修正见 `correction_amendment_20261008.md`）。
- 原始报告快照：`report_v1_original.md`。
- policy receipt：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/policy_receipt.jsonl`。
- evaluator ledger：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/evaluator_ledger.jsonl`。
- 原始聚合：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/result.json`。
- 修正分析：`experiments/logs/task49_postcall_fresh_confirmation_20261008/analysis_audit_v3/analysis_audit_v2.json`。
- 首轮独立核验：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/verification_v2.json`；增强校验器的结果写入 `strict_audit_v3/`。

- 完整独立核验与失败记录：`experiments/logs/task49_postcall_fresh_confirmation_20261008/strict_audit_v3/report.md`。
