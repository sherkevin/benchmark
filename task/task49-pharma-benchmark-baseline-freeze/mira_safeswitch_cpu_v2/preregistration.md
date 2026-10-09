# MIRA-SafeSwitch CPU replay v2 预注册

日期：2026-09-29
状态：运行前；只用于 development，不打开 confirmation。

## 只改什么

相对 v1，只把 runtime failure 的阻断规则从“一次失败即阻断”改为“同一 provider 连续两次 selected runtime failure 才阻断”。一次成功的 selected output 会把该 provider 的 failure streak 清零。Beta error bound、fit-only direct prior、事件注入、输入、fold、seed slot、delay、方法对照和日志格式全部保持不变。

## 固定实验

- 5 scaffold folds × 2 deterministic slots；场景 `no_event` 与既有 `controlled_event`；方法 Frozen direct、MIRA-SafeSwitch、matched Online-SGD；delay `{0,1,4}`。
- MIRA：`delta=.05`、`n_min=20`、`tau_fail=.20`、`runtime_failure_streak=2`。
- 不读未选输出、当前真值、post-call confidence 或 version flag；只在 release tick 更新。
- GPU/API 新预测均为 0；confirmation 关闭。

## 预先固定的判据

1. 独立 replayer 必须逐事件重现 MIRA 的 masked score、provider、version、streak、block reason 和 state hash；否则整批作废。
2. no-event：三个 delay 的 MIRA accuracy 相对 Frozen 的配对点差值都不得低于 `-0.010`（-1.0pp ROPE）；超出任一 delay 即停止该 v2。
3. controlled-event：至少两个 delay 的事件后 suffix accuracy 相对 Frozen 提升 `>= +0.010`，且第一次合法 target failure release 后 target selection rate `<= 0.02`；否则停止该 v2。
4. event 前 accuracy 相对 Frozen 不得低于 `-0.010`；收益不能来自额外调用次数、未选标签或 future information。

结果只回答“连续失败门槛能否减少瞬时误阻断，同时保留受控退化恢复”。不把正控外推成自然 drift，也不据此宣称所有 provider adaptation 已解决。
