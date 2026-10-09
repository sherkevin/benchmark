# MIRA veto ablation v1

日期：2026-09-30
状态：运行前；CPU development mechanism audit only。

## 问题

MIRA-SafeSwitch v2 同时有两个 veto：selected successful returns 上的 Beta error upper bound，以及 selected runtime failures 上的 consecutive-failure streak。v2 的 controlled-event positive signal 不能直接归因其中一个机制；本轮只拆这个机制，不调阈值、不重训、不打开 confirmation。

## 固定输入与对照

- 复用 `fingerprint_diagnostic_v1/inputs.json`、run_v2_fixed 的 `fit_snapshots` 与 `feature_snapshots`、同一 fold/seed/episode order、同一 evaluator、同一 event tick 和 delay `{0,1,4}`。
- 5 scaffold folds × 2 protocol slots × 2 scenarios（`no_event`/`controlled_event`）× 3 delays。
- 四个机制条件：`frozen`、`streak_only`（Beta 禁用）、`beta_only`（runtime streak 禁用）、`full`（MIRA v2）；保留 `matched_sgd` 作为强同合同外部对照。
- `streak_only` 用 `n_min=10^9, runtime_failure_streak=2`；`beta_only` 用 `n_min=20, runtime_failure_streak=10^9`；`full` 用 v2 的 `n_min=20, tau_fail=.20, delta=.05, runtime_failure_streak=2`。这些是机制开关，不是调参搜索。

## 必须记录与复核

每条 stream 记录 overall/pre/post/first-release-after accuracy、target exposure、target failure、首次 block tick/reason、pending release clock、decision/terminal/feedback 数。独立 verifier 需要拒绝 event ID、due/release、provider、版本或 action 篡改；full 条件还必须与 `run_v2_fixed` 的对应 action/version/block state 逐事件一致。

## 判别问题和停止条件

1. no-event：四条件相对 Frozen 的 paired delta 与 block reason，确认静态损失是否来自 Beta veto；超过既有 `-1pp` ROPE 只作为机制诊断，不据此调阈值。
2. controlled-event：只在 event 前目标未被 Beta block 且目标确实产生 failure release 的 stream 上，比较 first-release-after suffix；event tick 到 release tick 的 pending window 单独报告。
3. 计算 interaction `U_full-U_beta_only-U_streak_only+U_frozen`，但不把它当最终统计显著性。
4. 若 full 与旧 v2 逐事件不一致，整批只作 engineering failure；若任何条件偷读未选输出、当前 truth 或 future feedback，整批作废。

本轮结果不能证明 MIRA 超过 Best Fixed、Matched Online-SGD 或 modern router；它只回答 MIRA v2 的机制来源和下一轮该修哪里。
