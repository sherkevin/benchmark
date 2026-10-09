# Fresh mechanism ablation v2 — result report

日期：2026-10-07

## 状态

v2 已完成 natural 与 controlled-drift 两个 CPU replay。两者都使用同一份
sealed provider prediction、confirmation manifest 和五折 fit-prior cache；没有
重新调用 provider、LLM 或 Nebula GPU。代码和运行契约绑定到 commit
`a389bd2aa1ac6452f3766fe44944f00fbd720977`。原始 receipt 保留在
`experiments/logs/task49_fresh_mechanism_ablation_v2_20261007/`，本报告和
`artifact_manifest.json` 是可提交的紧凑索引。

## 运行和复核

每个条件包含 5 folds × 2 个 deterministic schedule replicas × 3 delays
($D\in\{0,1,4\}$) × 3 methods，共 90 streams、14,130 decisions 和 42,390
event records。两个 replica 是固定执行表的重复观测，不是随机 seed 稳健性
实验。

三臂为 Frozen/no-update、MIRA-Relay/Local 和 fixed cyclic permutation credit。
所有方法的 cold-start score 是 $q_a^0(x)=\sigma(w_a^\top x)$；Local 将源
provider 的 selected-only residual 增量写入源槽，permutation 将同一个源增量写入
固定目标槽。构造案例通过：无反馈时三臂 score/action 完全相同；单个 reward
mutation 的 source counters 相同，而 Local 与 permutation 的 destination residual
分别落在预期槽位。

两个条件的 independent verifier 均通过：90 streams、14,130 decisions、56,520
state-hash checks、14,130 score checks、最大 score 重算误差 `0.0`，所有
release 前逐事件 score/argmax 与 Frozen 一致。verifier 还检查了输入文件和实现
文件 SHA、严格的 `release_tick = decision_tick + 1 + delay`、同 tick ordinal
顺序、selected output/truth 绑定和 source-delta 更新。将第一条 feedback 的
`release_tick` 加 1 后，verifier 以 `ReplayError` 拒绝；失败现场和摘要保留。

## 唯一主估计量

对每个 stream $s$ 和 scaffold $g$，先计算 scaffold 内 selected correctness
$m_{s,g}$；对配对方法差值取 stream 均值，再对 124 个 scaffold 等权平均：

$$\hat\Delta=|G|^{-1}\sum_{g\in G}|S_g|^{-1}
\sum_{s\in S_g}(m^{\mathrm{method}}_{s,g}-m^{\mathrm{Frozen}}_{s,g}).$$

95% 区间对同一个 scaffold 差值向量做固定 seed 的 10,000 次 bootstrap。下面的
区间和点估计使用同一 estimand；pooled event 差值只作为描述性计数保留。

## 结果

| condition | contrast | scaffold mean Δ (pp) | 95% CI (pp) | rescued / harmed | action changes |
|---|---|---:|---:|---:|---:|
| natural | Local − Frozen | +0.10 | [−1.08, +1.42] | 82 / 82 | 1,656 |
| natural | permutation − Frozen | +0.41 | [−0.47, +1.32] | 70 / 62 | 1,258 |
| natural | Local − permutation | −0.30 | [−1.42, +0.76] | 106 / 114 | 2,174 |
| controlled drift | Local − Frozen | +14.40 | [+9.79, +19.23] | 780 / 112 | 1,884 |
| controlled drift | permutation − Frozen | −1.27 | [−3.66, +0.95] | 206 / 252 | 1,326 |
| controlled drift | Local − permutation | +15.67 | [+10.65, +20.74] | 874 / 160 | 2,550 |

Delay-specific pooled accuracies in the controlled condition are Frozen
`0.7338/0.7338/0.7338`, Local `0.8701/0.8790/0.8777`, and permutation
`0.7159/0.7210/0.7350` for $D=0/1/4$. These are descriptive schedule-stratified
values, not separate confidence intervals.

Natural streams therefore do not show a statistically separated stationary gain:
the Local point estimate is near zero and its interval crosses zero, while the
fixed permutation is numerically slightly higher. Under the declared evaluator-side
drift, Local recovers correctness and the fixed wrong-credit control does not. This
supports the narrower mechanism statement that source-aligned released feedback is
useful for the declared drift replay; it does not establish a universal natural-data
improvement, family sharing, verification/refusal, or end-to-end agent gain.

## Evidence paths

- natural receipt: `experiments/logs/task49_fresh_mechanism_ablation_v2_20261007/natural_v2/`
- controlled receipt: `experiments/logs/task49_fresh_mechanism_ablation_v2_20261007/controlled_v2/`
- independent verification: sibling `*_v2_verification/` directories
- analysis: sibling `*_v2_analysis.json` files
- expected rejection: `mutation_rejection.json`
- pre-run design and reviewer: `experiment_card.md`, `pre_run_review_20261007.md`

这份报告不把 v2 结果自动写入主稿；先用它重跑三份验收标准，再决定主稿中是否
加入一行 mechanism control。v1 Shared-Credit 的结果和解释继续保持撤回状态。
