# Action-supported dynamic event recovery v2

日期：2026-10-01  
状态：运行前登记；development only，confirmation closed。

## 1. 唯一问题

`dynamic_scope=feedback-adaptive`。本轮只研究 selected-only 延迟反馈释放后，内部状态和后续动作是否改变；不声明 provider/version 漂移、工具到达/退出或 environment-nonstationary。

在同一条 selected-only、延迟反馈流中，收到 provider 的 runtime failure 后，MIRA 是否能在不破坏 stationary 行为的前提下恢复；一次偶发失败和持续失败是否应产生不同的动作响应？

这不是新的 benchmark、不是自然 drift 证据，也不把固定池回放改称生产环境。它只检验 MIRA 动态事件接口是否有可识别的恢复行为。

## 2. 固定输入和方法

- 输入：已封存的 BBB development `fingerprint_diagnostic_v1/inputs.json`、同一 33-provider menu、同一五个 scaffold evaluation folds、同一 20 个 fit/feature snapshots。
- 方法：`frozen`、`streak_only`、`relative_reliability`、`matched_sgd`。前两者是稳定性/机制控制，后两者是候选与最近邻。Greedy/Bayesian 的 Gaussian reward-scale 语义尚未进入 strict main stratum，保持 adapted comparator 单列。
- delays：`{0,1,4}`；seed：`{0,1}`；事件 tick：每个 fold evaluation 顺序的 `floor(n/2)`。
- no-event control：复用同 method/fold/seed/delay 的既有统一 receipt，不根据事件结果重排。
- 所有参数、输入、snapshot、代码 commit 和 target manifest 在运行前冻结；不使用 confirmation 标签调参。

## 3. 事件协议

每个 method/fold/seed/delay 先由 event 前 no-event prefix 运行至 event tick，把该方法实际选择的 provider 写入 sealed target manifest。target 只由 prefix 的 selected-only feedback 决定。

- `transient_failure`：target 在 event tick 只返回一次 runtime failure，之后恢复原缓存输出；
- `persistent_failure`：target 从 event tick 起每次被选中都返回 runtime failure；
- policy 看不到 event flag、真值、未选 provider 输出或完整 panel；
- runtime failure 的 `system_success=0`、`chemical_correct=null`，只进入 failure channel，不伪造化学标签；
- feedback 在 `release_tick=decision_tick+1+delay` exactly once 释放；未释放 feedback 不改变状态。

如果 event tick 的实际 action 与 target manifest 不一致、事件改变未选输出/menu/truth/features、或 feedback 时钟/状态恢复失败，整批只记 engineering failure，不解释效果。

## 4. 主估计量

所有 event stream 与同坐标 no-event stream 配对，先按 stream 计算，再按 5 个 scaffold folds 聚类 bootstrap；不把重复 molecule/method/seed/delay cell 当独立样本。

1. **注入 terminal loss**：event tick 被强制失败造成的一次性损失，单独报告；不当作恢复失败。
2. **反馈盲区**：`event_tick+1` 到 `release_tick-1` 的配对差值；delay=0 记 `NA`。
3. **release 后恢复**：从 `release_tick` 到 stream 结束的配对 correctness 差值，这是 persistent 的主指标。
4. **target exposure**：failure release 后继续选择 target 的比例。
5. **release-to-switch**：首次不再选择 target 的 tick，未切换右删失。
6. **成本**：selection/update p50/p95、state bytes、failure/unknown/refusal/call 账本。

主要报告 method 内 event−no-event；跨方法只比较同一 estimand 的 paired delta、exposure 和成本，不比较不同 target provider 的 raw post accuracy。

## 5. 证伪条件

- `relative_reliability` 在 transient 的 release 后恢复段相对 no-event 在至少两个 delay 低于 `-1 pp`：保守偶发故障假设不成立；
- persistent 中 relative 的 release 后恢复在至少两个 delay 比 `streak_only` 低超过 `1 pp`：relative factor 没有保留 hard failure recovery；
- relative 的 persistent target exposure 比 matched 高至少 `5 pp`，且 matched 的 release 后恢复高至少 `1 pp`：当前缺少独立 runtime-failure response channel，不能用 chemical correctness ledger 解释；
- 任何 provenance、信息隔离、事件守恒或独立 recovery 失败：整批不解释。

即使上述条件均不触发，也只进入下一轮方法审查，不打开 confirmation，不自动申请 A800。只有 event recovery、baseline 合同和成本预算同时通过，才考虑远端完整主矩阵。
