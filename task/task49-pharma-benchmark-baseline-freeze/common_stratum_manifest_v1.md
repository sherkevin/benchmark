# Common-stratum 输入 manifest v1

日期：2026-09-30  
状态：**已封存并独立核验；尚未运行 full effect receipt。**

## 目的

把 strict common-stratum 的 fold/seed、encoder checkpoint、direct provider prior、数据分区和反馈语义固定下来。这个 manifest 只登记已经产生的 fit snapshot，不重新训练模型，也不读取 confirmation。

机器文件：`experiments/logs/task49_common_stratum_manifest_20260930/v2/manifest.json`  
manifest SHA-256：`a9232a4003fc02f6a1d7fa7092aee63aa642b71c1ef33397149de72fa6cd2fe7`  
独立 verifier：`experiments/logs/task49_common_stratum_manifest_20260930/v2/verification.json`，状态 `verified`。

## 输入覆盖

- BBB development inputs SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 33 个具体 provider。
- 5 个 scaffold outer folds × 2 个 seed slots，共 10 个 snapshot。
- 每个 snapshot 都绑定 `q_prior`、`provider_prior`、`direct_prior` 三个实际存在的数组；direct prior 形状为 `33×64`，所有值有限。
- 每个 snapshot 绑定对应 BT+CLS encoder checkpoint 的文件 hash；verifier 会重新读取 checkpoint 并校验 hash。
- 每个 fold 的 fit、validation、evaluation 索引由 verifier 检查为两两不交且完整覆盖 722 条 development 分子。

## 统一协议

- 表示：冻结 BT+CLS encoder，query dimension=64。
- warm start：每个 `(fold, seed)` 使用该 snapshot 的同一个 `direct_prior`；同一 stream 内所有 strict 方法共享它。
- covariance：`Σ0 = 0.02 I`；observation noise variance=`1.0`；Greedy alpha=`0.5`；Matched Online-SGD lr=`0.05`。
- 每个 decision 一次 provider call；每个 release 一次状态更新；不看未选 provider 的输出。
- delay 为 `{0,1,4}`，release tick=`decision_tick + 1 + delay`。
- runtime failure：`system_success=0`、numeric reward=`0`、`chemical_correct=null`、feedback status=`failure`。
- valid return：reward 使用 truth-judged `system_success`，`chemical_correct` 只在 evaluator 内记录，feedback status=`scored`。
- unknown truth：停止该 stream，不产生 numeric reward，记录 censoring；failure 仍计入分母。
- confirmation 保持关闭。

## 语义冒烟

manifest 之后的真实缓存语义冒烟已通过：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_feedback_semantics_smoke_v1.md`。它覆盖一个真实运行失败和一个 unresolved reference，只验证 failure denominator 与 censor/stop 语义，不产生效果估计。

## 验收边界

这份 manifest 解决的是“共同输入到底是不是同一份”的问题，不代表任何方法已经优于 Frozen，也不替代 full receipt 的事件守恒、paired uncertainty、调用/更新时间和成本账本。语义冒烟通过后，下一步是用它做包含完整事件账本的 2-stream smoke，再做 60-stream full receipt；如果某个方法无法在这份 prior/covariance 下表达，就标为 adapted/out-of-stratum，不改写旧结果。
