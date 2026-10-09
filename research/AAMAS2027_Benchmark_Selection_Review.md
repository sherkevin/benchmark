# AAMAS2027 Benchmark Selection Review（历史候选）

> 状态：已被 [ADR-0004](../docs/决议/0004-主Benchmark收敛为TDC与Task36医药工具路由.md) 替代，不再作为当前执行依据。
> 目的：冻结 Selective Contextual Onboarding of Newly Arriving Challenger Tools 的 benchmark 协议。

> 说明：本文保留早期 LLMRouterBench + ASlib SAT12-ALL + BBB 方案，供追溯。当前主任务已经收敛为 TDC truth + Task36 医药 provider routing；如需复用本文候选，只能另设 external transfer/appendix 实验。

## Selection Criteria

Benchmark must support:

1. multiple competing tools/models/algorithms;
2. context-dependent specialization;
3. counterfactual reward matrix R(context, tool);
4. frozen incumbent vs arriving challenger protocol;
5. few-shot feedback evaluation;
6. regret / IOU measurement.

## Current Candidates

## LLMRouterBench

Role: main modern routing benchmark.

Strengths:
- query-level model performance;
- direct LLM/tool routing interpretation;
- natural newcomer model scenario.

Risk:
- reviewer may view it as ordinary model selection.

Decision: keep as main benchmark.

## ASlib SAT12-ALL

Role: classical algorithm selection benchmark.

Strengths:
- established algorithm selection setting;
- instance -> algorithm mapping is mathematically aligned.

Risk:
- older benchmark;
- not directly agent/tool oriented.

Decision: keep as main benchmark to avoid LLM-only claim.

## BBB

Role: scientific tool routing stress test.

Strengths:
- closer to black-box scientific tools;
- exposes sparse beneficial-region difficulty.

Risk:
- less universally known.

Decision: keep as hard stress test.

## Synthetic Controlled Benchmark

Role: mechanism validation appendix.

Need control:
- incumbent strength;
- challenger positive-region sparsity;
- feedback budget.

Key variable:

rho = P(r_new(x) > r_inc(x))

Decision: recommended for appendix, not replacement of real benchmarks.

## Historical Recommendation（已废止）

Main:
1. LLMRouterBench
2. ASlib SAT12-ALL
3. BBB

Appendix:
4. Synthetic sparse-advantage benchmark

This combination covers modern routing, classical selection, and scientific tool scenarios.
