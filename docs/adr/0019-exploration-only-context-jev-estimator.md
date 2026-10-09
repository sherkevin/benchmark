# ADR-0019：下一版 context JEV 只用随机探索样本更新

状态：Superseded by ADR-0020

日期：2026-09-25

## 背景

ADR-0018 的 cell-provider HT/Freedman 状态在 synthetic coverage 上保持了保守覆盖，但在 1000 episode、调用成本 `0.1` 的量级 gate 一次也没有打开。原因是 deterministic overlap floor 把未选中 provider 的最坏方差带进每个 cell，半径太宽。

我们随后实现了 exploration-only 状态：只把随机探索分支实际抽到的 provider gain 放进 bounded-mean 状态，利用分支的结果不进入估计。4-cell synthetic coverage 仍为 `1.0`，并且 gate 能根据成本打开；BBB 回放中相对 random/no-update 在 `0/0.05/0.1` 三个成本点均有更低 net utility。

## 决定

下一版 pilot 优先采用 exploration-only context JEV：

1. context 仍固定为 `TPSA > 60` × `aromatic_ring_count > 0` 四个 cell。
2. 每个 cell-provider 维护随机探索样本数和 bounded mean。
3. 只在探索 coin 命中且 provider 被随机选中时更新状态；exploit 结果只用于终局评分和日志。
4. gate 使用 alpha-spent Hoeffding confidence sequence，LCB 超过调用成本才允许利用分支。
5. 所有动作仍记录完整 mixture propensity；propensity 用于审计和后续 IPW 对照，不把 exploitation 观测混入当前 certified mean。
6. 先在 synthetic potential-gain 和冻结 BBB replay 上做 pilot，再决定是否进入新数据的 fresh confirmation。

## 理由

这样把“用于学习的随机证据”和“用于执行的自适应动作”分开，避免为了降低方差而重新使用有选择偏差的 exploitation 结果。实现状态更小，实时更新只需累加一个 cell-provider 的 count/sum，适合后续接入 JEV backbone 作为冷启动模块。

## 后果与边界

- 估计目标是随机探索采样下的 context-provider gain；正式论文必须明确条件独立和目标总体定义，不能自动把它写成所有自适应动作的无偏估计。
- 探索成本由 `epsilon` 付出；如果 cell 稀疏，gate 会保持关闭。
- 这条决议不等于最终方法冻结。若 fresh scaffold 上没有收益，必须回到 context 定义或数据池审查，不能直接加大 backbone。

## 证据

- [AAMAS2027_BBB_Context_Exploration_Only_Evaluation_Report_v1.md](../../research/AAMAS2027_BBB_Context_Exploration_Only_Evaluation_Report_v1.md)
- [context_jev_exploration_only_synthetic_v1_20260925](../../experiments/logs/context_jev_exploration_only_synthetic_v1_20260925/)
- [0018](0018-context-conditioned-cell-jev-mvp.md)（已被本决议取代）
