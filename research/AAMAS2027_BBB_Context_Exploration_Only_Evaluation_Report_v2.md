# BBB context exploration-only JEV 回放结果 v2

日期：2026-09-25

## 这次改了什么

v1 的 gate 不能作为统计安全结果：gain 在 $[-1,1]$，原 Hoeffding 半径少了范围因子 2；同一 batch 内重复查门也没有被 alpha spending 覆盖。本版做了两处修正：

1. 使用 $r=\min(2,2\sqrt{\log(2/\delta_\ell)/(2n)})$，LCB 下截到 $-1$；
2. 每 64 个 episode 在 batch 起点计算一次 gate，并冻结到下一个 batch；探索 coin 和 provider 抽样仍逐 episode 进行。

## 结果

同一预先固定的 722 个 resolved BBB 分子、同一 Bemis–Murcko scaffold split、20 seeds、4 个 synthetic call cost 下，修正后的 gate 在所有成本点都没有打开：

| call cost | accuracy | calls / episode | net utility | gate open |
|---:|---:|---:|---:|---:|
| 0.00 | 0.7745 | 1.7948 | 0.2255 | 0 / 2120 |
| 0.05 | 0.7745 | 1.7948 | 0.3152 | 0 / 2120 |
| 0.10 | 0.7745 | 1.7948 | 0.4050 | 0 / 2120 |
| 0.20 | 0.7745 | 1.7948 | 0.5844 | 0 / 2120 |

这与 random/no-update control 相同，说明短校准流、4-cell 分层和 $\delta=0.05$ 下，严格门没有足够证据支持利用分支。这个负结果是统计修正后的真实行为，不能通过把半径缩回 v1 的错误版本来改善。

## 为什么门必然很难打开

第 $\ell$ 个 look 要求 $\widehat\mu-r>c$。若真实增益约为 $0.3$、成本为 $0.1$，即使样本均值没有误差，也需要 $r<0.2$。修正后的半径要求：

$n>\frac{2\log(2/\delta_\ell)}{(0.2)^2}$。

在第 10 个 look、$C=4$、$K=2$、$\delta=0.05$ 时，$\delta_{10}\approx3.8\times10^{-6}$，所以需要 $n>658$ 个同一 cell/provider 的随机探索样本。当前 616 条 calibration 流中每个 cell 最多约 154 条，结构上达不到这个支持量。这解释了 gate 全关，不是实现偶然。

对应的 20-seed 原始记录、配置和汇总在：

`experiments/logs/bbb_context_exploration_only_retrospective_v3_20260925/`（commit `2f42546c` 的可复现重跑）

## 统计范围

探索-only 均值在固定 calibration 流上对 $\mu^{\mathrm{cal}}_{c,a}$ 条件无偏；它不自动覆盖 scaffold-heldout 流。BBB 这里仍是 post-freeze retrospective utility evaluation，没有新的 provider snapshot，也没有 model-fresh 分子池。

有限总体前缀审计在 500 seeds、1024 episode、每 64 episode 一次 look 下覆盖了 63,706/63,706 个有足够 support 的检查；原始产物为：

`experiments/logs/exploration_only_finite_population_prefix_audit_v3_20260925/`（commit `2f42546c` 的可复现重跑）

完整目标、证明条件和不主张的内容见：

`research/AAMAS2027_ExplorationOnly_JEV_Statistical_Contract_v1.md`

## 当前判断

探索-only 方案现在是一个统计边界清楚、可以运行的校准 gate 原型，但还不是能在当前 BBB 规模上产生利用动作的最终方法。下一步要解决的是“如何在不破坏 coverage 的前提下提高有效样本量”，候选方向是减少 context 分层、预先注册的 provider pooling/层次模型，或把问题改成固定 horizon 的决策评估；不能再手工放宽置信半径。
