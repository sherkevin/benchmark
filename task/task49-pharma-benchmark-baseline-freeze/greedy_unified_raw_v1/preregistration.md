# Greedy LinUCB unified raw effect v1

日期：2026-09-30  
状态：预注册；仅 CPU development baseline qualification，不是 Proposed 方法、A800 或 confirmation。

## 目的

把 Greedy LinUCB 作为当前医药直接工具选择任务中的现代在线近邻，放到与 MIRA/native Bayesian/matched 共用的输入、表示、菜单和 delayed selected-only feedback 合同下。旧 Greedy receipt 使用 P-C1 metadata 版本，保留为历史负证据；本批不把旧结果改写成新结果。

## 固定输入与矩阵

- 输入：`fingerprint_diagnostic_v1/inputs.json`，SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 722 个分子、33 个具体 provider、5 个 scaffold-disjoint outer folds；evaluation 顺序不打乱。
- 表示：同当前 unified receipts 的 fit-only BT/CLS encoder hidden-64；checkpoint、source snapshot 和每个 fit prior 都复制或 hash 到 receipt。
- Greedy prior：每个 outer fold 只用 fit 区的 hidden-64 和完整 `system_success` panel 做 ridge provider mean，`lambda_fit=1`；evaluation 标签不可见。
- 方法：`greedy_online`（selected-only delayed reward 后做 rank-one Gaussian ridge update）与 `greedy_frozen`（同一 prior、同一 feature、消费反馈但不改模型状态）。
- LinUCB：`alpha=0.5`，regularization=1，observation noise variance=1；provider menu 全部 33 个，tie rule 为 menu 中第一个最大分数。
- 矩阵：2 methods × 5 folds × 2 deterministic protocol slots × delays `{0,1,4}` = 60 streams；seed slot 不是独立随机重复。

## 信息与反馈边界

选择前只读当前 molecule hidden feature、当前 menu 和已释放的 selected-only feedback。未选 provider output、truth、future feedback 不得进入 policy。终止后只把 selected provider 的 `system_success` 作为 reward；unknown 停止 batch；failure reward=0。feedback release tick 固定为 `decision_tick + 1 + delay`。

## 主指标与停止线

每条 stream 记录 selected correctness accuracy、累计错误、反馈数量、选择/更新 p50/p95、状态内存和逐事件 raw receipt。主比较是同 `(fold, seed, delay)` 的 `greedy_online - greedy_frozen` paired difference，另报 fit-only Best Fixed 作为控制。

这批只做 baseline 资格，不预设 Greedy 会改善。若 online 在至少两个 delay 没有正均值，或多数 fold 不为正，保留为负 baseline；不调 alpha、lambda、顺序或数据筛选来追求正数。任何 input/menu/read-set/provider binding/state hash/replay verifier 失败，都标为 engineering failure，不解释 accuracy。

## 证据要求

receipt 必须复制 exact inputs、记录 declared 40-char commit、immutable source hashes、checkpoint/feature/prior hashes，并含 decision/terminal/feedback 的 menu、read-set、released IDs、provider binding、due/release tick、version、state hashes。独立 NumPy verifier 不导入 runner 或 policy，重算 prior、UCB action、selected reward、延迟 release 和 rank-one state transition；至少一次 menu/state tamper 必须被拒绝。

通过只说明 Greedy 具备统一主表的可审计资格，不证明 Proposed 方法有效，也不关闭 provider lineage、endpoint equivalence、成本公平或 confirmation。
