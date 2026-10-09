# Cross-method fairness audit v1

日期：2026-09-30  
状态：只读合同审查；没有重跑、调参或修改任何 receipt。

## 结论

五条当前 unified receipt 在输入 cohort、provider menu、outer folds、seed slots、delay、selected-only feedback、feedback clock 和 unknown 处理上完全一致。它们因此可以共享**协议层**，但不能直接用 raw accuracy 做一条总排序：feature 维度相同不代表表示和 warm-start 相同，PILOT 还使用了独立的 projection/provider prior。

- 允许的 primary comparison：每个方法自己的 `online − frozen` paired effect。
- 暂不允许的 comparison：把 MIRA、Greedy、PILOT、BayesianRouter、Matched 的 raw accuracy 混成一个 superiority 排名，或把同一 64 维写成“同一初始化”。
- cost 结论边界：这些都是 cached replay，每个 molecule 只记一次 selected provider call；没有真实 provider 价格、token 或 wall-clock 服务成本，不能声称 cost-aware superiority。

## 证据

机器审计：`experiments/logs/task49_cross_method_fairness_audit_20260930/audit_v1.json`。它只读取各 receipt 的 config/summary/verification/manifest，未读取或改写 raw events。

| 方法 | feature strata | warm-start | online update | verifier |
|---|---|---|---|---|
| MIRA | BT+CLS hidden-64 | fit-only direct-prior snapshot | selected-only structured reliability/failure state | verified |
| Matched Online-SGD | BT+CLS hidden-64 | common-direct-prior；Gaussian state 以 prior mean 为中心 | selected-only Gaussian rank-one | verified |
| native-BT BayesianRouter | recovered BT+CLS hidden-64 | native recovered `score.weight`；prior variance 0.02 | selected-only Gaussian rank-one/TS action | verified |
| Greedy LinUCB | BT+CLS hidden-64 | fit-only ridge provider mean；初始 covariance 为 identity | selected-only Gaussian rank-one/UCB | verified |
| PILOT-adapted | PILOT projected-64 | fit-only projection/provider vectors；fit provider precision | selected-only Gaussian rank-one/cosine-UCB | verified |

五条 receipt 的 input SHA 都是 `a73f1cf…e3bdf`，provider 数均为 33，fold/seed/delay 均为 `5/2/{0,1,4}`，selected-only、feedback due tick 和 unknown semantics 一致。完整值和每条 receipt 的 config/summary/verifier/events hash 见机器 JSON。

另对 10 个 `(fold, seed)` sealed snapshot 做了逐数组核对：MIRA 与 Matched 的 direct prior 最大绝对差为 `0`，evaluation feature 最大绝对差为 `0`；Greedy 与 native-BT BayesianRouter 的 evaluation feature 也分别与 Matched 最大绝对差为 `0`。但是 Greedy 的 fit-only ridge prior 与 Matched direct prior 最大绝对差为 `0.7531`，native Bayesian 使用的是 `score.weight` 与独立 prior variance。因而当前真正的 common warm-start stratum 只有 MIRA/Matched；Greedy/native 只能作为同表示、不同初始化的对照，PILOT 单独成组。

## 对主表的约束

1. 主表应先按 feature/warm-start strata 分组，再在组内展示 paired online−frozen；MIRA/Matched 可作为 strict common-prior 组，Greedy/native-BT 作为 same-feature/different-prior 组，PILOT 单独标为 adapted baseline。
2. Best Fixed 是共同 input 上的静态 anchor，不能被当成在线方法的同初始化对照。
3. 如果后续要声称跨方法 superiority，必须重新登记 common-64/common-prior 合同，统一 prior 来源、初始化 covariance、update budget、exploration rule 和 cost ledger；不能事后从现有 receipt 拼接。
4. unified receipt 的 `verified` 只说明 action/state/feedback provenance 可重放，不自动证明 provider training membership、endpoint assay equivalence 或 confirmation 泛化。

## Runtime / call accounting

只读 runtime 收据：`experiments/logs/task49_cross_method_fairness_audit_20260930/runtime_audit_v1.json`。每条 stream 都是一 molecule 一次 selected cached provider call，feedback 数与 decision 数守恒；但 MIRA 收据包含四个方法和两个 event scenario，不能用总计数直接与两方法 baseline 比较。按 method ID 的 selection/update p50/p95 已逐项保存，例如 relative-reliability 的 selection/update p50 为 `58.9/41.6 μs`，matched Online-SGD 为 `8.25/8.75 μs`，Greedy Online 为 `151.0/22.0 μs`，PILOT Online 为 `152.0/20.7 μs`，native BayesianRouter 为 `214.2/63.0 μs`。

这些数字只描述当前 Python/cache replay 的策略计算开销；不同 runner、NumPy 路径和硬件环境未做 runtime normalization，也没有真实 provider 金钱/token 价格。因此它们不能被写成 deployment cost 或 cost-aware superiority，只能作为后续统一成本账本的起点。

## 对验收的影响

这次审计关闭了“PILOT 尚无统一 full receipt”的工程缺口，但没有关闭公平性 gate。Gate B 仍 HOLD，原因集中在 provider lineage、feature/warm-start 公平、统一预算/成本账本和 confirmation。下一步先把这些合同写成可执行的 common-stratum 方案，再决定是否值得增加动态事件压力实验。

## 2026-09-30 common-stratum contract

已将下一次公平比较的执行合同固定在
`task/task49-pharma-benchmark-baseline-freeze/common_stratum_contract_v1.md`：同一 BT+CLS hidden-64 query、同一 fit-only provider prior `$W_0$`、同一初始 covariance、同一 selected-only feedback budget，并把 Frozen、Matched SGD、Greedy LinUCB、BayesianRouter 和 MIRA 的差异限制在各自的 update/exploration rule。PILOT 继续作为 adapted/out-of-stratum baseline 单独报告。

该文件是执行前合同，不把现有异质 prior receipt 追溯改成 strict common-stratum 结果。下一步仍需先做 2-stream smoke 和独立 verifier，再决定是否跑 60-stream full receipt；Gate B 暂不变。

### 2-stream smoke 实际收口

已完成 fold0/seed0、10 个 evaluation 分子、delay=0 的 common-prior smoke：Matched frozen/online、Greedy frozen/online、Bayesian frozen/online 和 MIRA relative reliability 共 7 条流，210 条 decision/terminal/feedback 事件。所有方法使用同一个 sealed validation-selected direct neural `$W_0$`，特征均为同一 BT+CLS hidden-64；独立 verifier 通过，收据：`experiments/logs/task49_common_stratum_smoke_20260930/v3/verification.json`。

这次 smoke 还暴露并保留了一个真实口径问题：旧 Greedy 代码的 fit-only ridge prior 与 sealed direct neural prior 最大绝对差为 `0.5402275611`，不能把二者叫作同一个 warm-start。失败现场在 `.../v1/failure_summary.json`；修正后的 v3 明确写入 `prior_source=sealed_validation_selected_direct_neural_head`。这一步只证明共同输入/反馈协议可以运行，不提供效果或 superiority 结论；60-stream full receipt 仍未跑，Gate B 继续 HOLD。
