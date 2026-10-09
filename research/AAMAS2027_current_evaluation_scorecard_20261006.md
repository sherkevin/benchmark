# AAMAS 2027 当前验收核对表增量（2026-10-06）

性质：内部开发审查增量；不是 AAMAS 官方评分，也不保证录用。基线分数沿用
[`AAMAS2027_current_evaluation_scorecard_20261004.md`](AAMAS2027_current_evaluation_scorecard_20261004.md)。

## 本次完成的事情

完成 `role_qualified_main_table_v1`：从已封存且独立核验的 BBB CPU receipts 生成按角色、scope、attempt 分母和失败语义分开的机器表，并对 Best Fixed/Oracle 做同输入 fold evaluation 的 exact control join。

- 430 条 stream/control rows，16 个 scope-method aggregates；
- 5 个 fold 的 Best Fixed 和 Oracle 控制，共 10 个连接检查，全部 `PASS`；
- ordinary `no_event`、`controlled_event`、`family_outage` 分开，禁止跨 scope 汇总；
- `unknown=0/refused=0` 原样保留为观察事实，并显式标记“不能据此宣称拒答有效”；
- Random、paired scaffold uncertainty、统一 state bytes 和 E3 full mechanism table 仍 `MISSING`。

证据：

- [`report.md`](../task/task49-pharma-benchmark-baseline-freeze/role_qualified_main_table_v1/report.md)
- [`result.json`](../task/task49-pharma-benchmark-baseline-freeze/role_qualified_main_table_v1/result.json)
- [`control_join.json`](../task/task49-pharma-benchmark-baseline-freeze/role_qualified_main_table_v1/control_join.json)

## 当前数字边界

| 控制/方法 | scope | pooled accuracy | 解释 |
|---|---|---:|---|
| Best Fixed | exact fold control | 0.915512 | fit-only incumbent |
| Oracle | exact fold control | 0.997230 | evaluator-only upper bound |
| Frozen | no-event | 0.901662 | strict control |
| Matched Online-SGD | no-event | 0.892198 | strict online control |
| MIRA relative | no-event | 0.902816 | development candidate |
| MIRA relative | controlled-event | 0.892428 | controlled stress |
| FamilyGuard | family-outage | 0.907664 | separate controlled stress |

这些描述性数字不替代已有 scaffold-paired CI；没有把它们写成最终论文结果。

## 验收分数

| 项目 | 之前 | 本次 | 状态 |
|---|---:|---:|---|
| 故事线与创新 | 47.5/100 | 47.5/100 | FAIL |
| 方法论 | 37/60 | 37/60 | HOLD/FAIL |
| Benchmark + Baseline | 41/60 | 41/60 | HOLD |
| Gate A：Benchmark | 11/16 | 11/16 | HOLD |
| Gate B：Baseline | 11/16 | 11/16 | HOLD |
| Gate C：矩阵与结果 | 10/16 | 10/16 | HOLD |

分数不变是有意的：本次关闭的是证据组织和 E1 exact join，不是新的性能提升或 confirmation。按标准，E1–E5 仍有 partial 项，E6 继续关闭。

## 下一步

用这张 role-qualified 表固定普通任务机制诊断的输入与分母，只比较 Frozen、MIRA streak-only、MIRA relative、Matched Online-SGD，并同时报告 no-event、transient failure、persistent failure 的 recovery、target exposure 和 harmed/rescued。若机制表仍没有超过最强合法 matched control，则停止 superiority wording；若 E1–E5 仍有 partial，不开 A800 confirmation。

## E3 机制消融收口

另将已核验的 `mira_veto_ablation_20260930/run_v1` 整理为
`task/task49-pharma-benchmark-baseline-freeze/mechanism_ablation_matrix_v1/`：300 条
stream、43,320 decisions/terminals/feedback，包含 Frozen、streak-only、beta-only、full
MIRA、Matched Online-SGD 五个变体。它在同 schedule 下关闭了 full、remove-correctness-gate、
remove-runtime-channel 和 remove-state 四个 E3 子轴。

结果定位了当前实现问题：runtime-failure streak 是 controlled-event 的主要恢复通道；
beta-only 不能处理 runtime failure，且造成 no-event 静态损失；full MIRA 三个 delay 都
低于 Matched Online-SGD。由此保留 `availability-hard / correctness-soft` 修复方向，
不把该机制诊断写成 superiority。

interface-null、independent-residual、family-permutation 和 paired scaffold CI 仍缺，
所以 Gate C 仍为 `10/16 HOLD`，confirmation 继续关闭。

## E3 coverage v2

`mechanism_ablation_matrix_v2` 将 v1 veto ablation 与 common-stratum
FamilyGuard/relative receipt 连接，但保留 scope 分离。由此，当前主方法的 correctness
ledger、runtime-failure ledger、family hazard state 和全状态去除均有对应对照；Matched
Online-SGD 作为 strict residual-learning 最近邻控制。`family_permutation` 仍是可选敏感性
缺口，paired uncertainty 不由该聚合器重新生成。

这使 E3 的状态从“机制去除不完整”收敛为“核心去除已覆盖、仍有边界敏感性和统一 CI 待补”。
总 Gate C 不上调：E1 Random exact stream、全 baseline 的统一 cost/state/refusal、E5 utility
分母和 E6 confirmation 仍不满足标准。

## E1 Random exact join

完整 Random development receipt（30 streams，独立 verifier `verified`）已加入
`role_qualified_main_table_v1`。每个 fold 的 Best Fixed/Oracle 与 common-stratum 48 条
stream、Random 6 条 stream 通过同一 evaluation attempt denominator 连接，10 个检查全部
`PASS`。Random pooled accuracy=`0.844875`，不使用解析期望值替代事件收据。

这关闭了 E1 的 Random 控制缺口，但 Random timing p95、全 baseline cost/state/refusal 和
paired scaffold uncertainty 仍缺，因此 Gate C 仍 `10/16 HOLD`。

## E5 accounting closure

`e5_unified_accounting_v1` 已把 common-stratum、family-outage、Random 和 post-call utility
收据统一到 attempts/calls、terminal、feedback、valid return、chemical correctness、runtime
failure、unknown、refused、cost/state 和 recovery 字段。它明确禁止把 runtime failure 记成
chemical wrong，也禁止把 post-call refusal 回填为 pre-call selector 效果。

这是 E5 accounting 子项的真实收口，仍不等于 E5 全部完成：Random timing、全 baseline state/cost、
unknown censoring、refusal utility 和 paired scaffold CI 尚缺。Gate C 保持 `10/16 HOLD`。

## Paired uncertainty matrix

`paired_uncertainty_matrix_v1` 收集了 common-stratum、controlled family-outage 和 post-call
utility 的已预计算 paired/bootstrap 区间，共 15 行，保留各自 estimand、单位、分母和 seed。
它补上了 C3/C4 的不确定性入口：Matched−Frozen 区间为 `[-2.2906,+0.2189] pp`，
MIRA-relative−Frozen 为 `[-0.3728,+0.8063] pp`，FamilyGuard−Frozen 为
`[-0.6204,+1.5988] pp`。

仍有 baseline-specific CI、Random CI、统一 cost/state/unknown/refusal 字段和 E6 缺口，
所以 Gate C 暂不改分数。下一步将这张表与 E5 accounting、role-qualified 主表合成最终
E1–E5 coverage matrix，重新判断 C4 是否达到单项 3 分。


## 2026-10-06 Stage-A 复核更新

完成两项只读证据补齐：

- `baseline_contract_v3` 将六个主方法的菜单、特征、初始化、selected-only 延迟反馈、选择/更新成本、状态字节、raw receipt 和 recovery 绑定到同一比较权限；Greedy/Bayesian 的状态字节来自已核验 common-stratum family receipt，不能与不兼容的 Replay/PILOT/static audit-only 行混排。
- `paired_uncertainty_matrix_v2` 新增 Random–Frozen 的 scaffold 成对区间，点估计 `-5.676 pp`，95% CI `[-7.587,-3.822] pp`；没有从 pooled accuracy 推造区间。

按“固定已部署 BBB 菜单上的 feedback-adaptive operational routing”范围重审，Stage-A scorecard 为：Gate A `13/16`、Gate B `12/16`、Gate C `12/16`。E1–E5 已有覆盖入口，E3 的边界敏感性、普通 cohort 的 unknown/refusal 零观察、provider model-fresh 不可证明和 E6 confirmation 都仍作为限制保留。该分数不等于 AAMAS 官方评分，也不等于最终效果通过；它只表示可以进入一次冻结的 E6 confirmation。

下一步：生成 provenance-locked E6 freeze receipt，确认只读评测不回流标签；完成后再决定正文 headline 数字和 claim 强度。
