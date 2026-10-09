# Task31 Failure Mode Analysis

## Purpose

Analyze why frozen baselines are insufficient before defining Ours.

The goal is not to claim existing methods fail universally, but to identify the mismatch between their optimization objective and the selective challenger onboarding setting.

## 1. Objective Mismatch

Most bandit/routing methods optimize accumulated reward:

maximize:

sum_t r(x_t,a_t)

However, challenger onboarding requires identifying contexts where:

Δ(x)=r(x,a_c)-r(x,a_i)>0

The objective is region identification rather than global arm selection.

## 2. Sparse Challenger Advantage

A newly arriving tool may only outperform the incumbent on a subset:

X+={x|Δ(x)>0}

When X+ is small, aggressive exploration creates unnecessary adoption cost.

The key challenge is high precision discovery of X+.

## 3. Cold-start Uncertainty

The challenger has no historical deployment record.

Therefore the system must learn:

- whether the challenger is useful;
- where it is useful;
- how confident the decision is.

Feedback itself has cost because wrong adoption can reduce performance.

## 4. Transfer Limitation

Historical tools provide useful information, but challenger capability boundaries may differ from existing tools.

Direct transfer of old routing knowledge may miss new positive regions.

## 5. Preliminary Research Gap

Existing methods answer:

"Which available action should be selected to maximize reward?"

The unresolved setting is:

"Given a new action with limited observations, where should it replace the incumbent?"

## Next

Use these failure modes to define the precise Ours problem formulation.

## 2026-09-22 — 解释边界更正，不修改冻结协议

上文“已有方法最大化 reward，我们识别 region”的对比不能单独证明 novelty，也不能证明方法必然失败：contextual pure exploration、threshold identification、selective prediction 等已有相应目标；在给定均值下，二元最优 reward policy 本身就是在 challenger 相对均值为正时采用它。

本文件列的是待解释的现象/假设，而非已经排除整个方法族的结论。Task30 特定适配与参数的负结果，只支持该适配在当前环境中的实测不足。Task32/34 后续还有历史信息隔离更正，必须引用各自最新 report，不能沿用原先过强的“已否定”。

数学定义的两类目标——realized reward-matrix 差值与 stochastic conditional-mean 差值——以及 hindsight oracle 不自动等于可学习 opportunity 的反例，见 `../task32-AAMAS2027-problem-formulation/estimand_audit.md`。本次不改变 Task31 benchmark、split、incumbent、反馈语义或指标。
