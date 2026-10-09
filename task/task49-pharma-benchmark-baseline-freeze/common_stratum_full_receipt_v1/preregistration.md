# Common-stratum full receipt v1

日期：2026-09-30  
状态：执行前登记；不打开 confirmation。

## 目的和可反驳主张

这批实验只检验：在相同冻结 BT+CLS 表示、相同 fit-only provider prior、相同菜单和相同 selected-only delayed feedback 下，动态更新器是否能被公平、可重放地比较。它不预设 MIRA、Matched Online-SGD、Greedy LinUCB 或 BayesianRouter 会超过 Frozen。

主要可反驳主张：所有方法都遵守同一 decision→terminal→delayed release 协议，failure 进入分母，未选工具输出不进入 policy；若任一独立 recovery 失败，整批只记为工程失败，不解释准确率。效果表只在收据完整后报告，主比较为每个动态方法相对共享 Frozen 的 molecule-paired `online−frozen`。

## 固定输入

- BBB development inputs：`fingerprint_diagnostic_v1/inputs.json`，SHA=`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 5 个 scaffold-disjoint outer folds × 2 个已封存 seed slots。
- 每个 `(fold, seed)` 绑定 common-stratum manifest 中的 BT+CLS encoder checkpoint 和 `33×64` direct prior `$W_0$`；所有方法在该 cohort 内共享 `$W_0$`。
- provider menu：33 个具体 provider；每题最多一次 selected provider call。
- delay：`{0,1,4}`；release tick=`decision_tick+1+delay`。
- unknown truth：停止该预声明 stream，不产生 numeric reward；development cohort 不含 unresolved rows。运行失败保留在分母，`system_success=0`、`chemical_correct=null`。

## 方法矩阵

一次 receipt 运行共享 Frozen 控制与四个动态方法，共 `5×2×3×5=150` method-streams：

1. `frozen_direct`：只读 `$W_0$`，反馈只推进协议时钟。
2. `matched_online_sgd`：选中 provider 的 logistic head 按固定学习率 `0.05` 更新。
3. `greedy_online`：共同数值 prior 下的 disjoint LinUCB，`alpha=0.5`；Gaussian covariance `Σ0=0.02I`，observation noise variance `1.0`，因此 prior precision 固定为 `50.0`。由于 `direct_prior` 源自 BCE/logit head，这一方法在未提供 reward-scale prior 前只作为 adapted/out-of-stratum comparator，不进入严格 common-prior 结论。
4. `bayesian_online`：共同 prior/covariance 下的 projected Gaussian Thompson sampling，prior variance `0.02`、noise variance `1.0`。
5. `mira_relative_reliability`：固定 `delta=0.05`、`n_min=20`、`tau_fail=0.20`、runtime failure streak=2 的可靠性调整。

所有 Bayesian stream 使用固定且按 fold/seed/delay 区分的随机种子 `100000 + 1000*fold + 100*seed + delay`；不能把同一个随机序列复用于不同 fold 或 delay。

Frozen 在每个 `(fold, seed, delay)` 只运行一次，作为所有动态方法的共享配对控制；总事件数由实际 evaluation cohort 计数决定，不用填充样本。

## 记录与指标

每个 event 记录 stream key、query、menu、selected provider、selected cache output、state hash、版本、due/release tick、selection/update wall time 和 state bytes。独立 verifier 重算 cache output、terminal reward、延迟和事件守恒。

汇总报告：pooled accuracy、每 fold/seed/delay 的 accuracy、failure rate、累计错误、每方法相对 Frozen 的 molecule-paired difference；以 scaffold cluster bootstrap 给出描述性 95% 区间。另报 selection/update p50/p95、调用数、更新数和峰值状态 bytes。缓存 replay 只允许报告策略计算开销，不写真实 provider 成本优势。

## 停止和解释规则

- 输入、manifest、source commit/hash、selected output、事件顺序或独立 recovery 任一失败：不报告效果，修复后新建 receipt。
- 结果为负或区间跨过 0 时保留负结果/证据不足，不调参、不换 split、不删除 failure。
- 该 receipt 只负责 Gate B 的 common-stratum 公平性和可审计性；不能关闭 provider training lineage、hERG/AMES qualification 或 confirmation gate。
