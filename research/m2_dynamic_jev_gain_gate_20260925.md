# M2 gain gate 最小 runner 记录

**日期：** 2026-09-25

## 目的

在 M1 的随机 probe 和 delayed verification-gain 状态之上，加一个最小的在线决策门：只有当前估计的追加收益高于调用成本时，利用分支才会继续调用；探索分支仍以显式概率随机 probe，保证后续还能学习。

## 决策合同

对剩余 provider 菜单 $C$，令当前状态给出的最大估计为：

$$
\hat g^*(h)=\max_{p\in C}\hat g(h,p)
$$

以概率 $\epsilon$ 均匀 probe 一个 provider；以概率 $1-\epsilon$，当 $\hat g^*(h)>c_{call}$ 时调用最大估计 provider，否则 STOP。日志同时记录 probe 和 STOP 的 propensity：

- probe provider $p$ 在探索分支的概率为 $\epsilon/|C|$；如果它也是利用分支的最佳 provider，则再加上 $1-\epsilon$；
- gate 关闭时 STOP 的概率为 $1-\epsilon$。

冷启动时没有已释放的 gain estimate，利用分支默认 STOP，只靠探索分支获得首批 label。这一点是有意的，避免把未知 provider 当成零收益。

## 实现

- `src/probe_headless/m2_dynamic_jev.py`
- `src/probe_headless/tests/test_dynamic_jev_m2.py`
- 复用 M1 的 `PendingVerificationFeedback`、`VerificationFeedbackQueue` 和 `VerificationGainState`；没有改 M0/M1。
- 当前只有 gain gate，没有 risk gate、历史内容 encoder、VOI 校准或 backbone fine-tune。

## 验证

合同回归：M0 + M1 + M2 + Task36 replay 共 `109 passed`。

真实 Task36 frozen replay v2：

- 6 个 episode、CNN 固定首 provider；
- 3 个候选 provider，探索概率 `0.8`，delay `1`；
- 4 次 probe、2 次 STOP；probe propensity `0.4`，STOP propensity `0.2`；
- 4 条 scored feedback exactly once release，gain `[-1, 0, 0, 0]`；
- 队列 flush 后为空。

日志：`experiments/logs/m2_dynamic_jev_smoke_20260925_v2_{config,raw,events,summary}.{json,jsonl}`。

另外跑了 v3 检查延迟状态是否真的被后续选择读到：6 个 episode、探索概率 `0.5`、delay `1`，3 次 probe、3 次 STOP；其中第 6 次选择在 learner version `2` 时进入利用分支，gate open，propensity `0.75`。v3 日志为 `experiments/logs/m2_dynamic_jev_smoke_20260925_v3_{config,raw,events,summary}.{json,jsonl}`。

这只是合同 smoke。它没有证明 gain gate 比 always-verify、never-verify 或其他 baseline 更好，也没有估计准确率提升。

## 下一步

先做离线 replay 对照：固定同一批随机 seed，比较 always-verify、never-verify、random probe 和 M2 gain gate 的最终错误、调用次数和净效用；之后再把 risk gate 单独加进来。
