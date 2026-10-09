# task31 report — Benchmark & Baseline Freeze Review

> 状态：✅ 已完成

## Completed

- Created benchmark selection review.
- Created baseline taxonomy review.
- Recorded current recommended evaluation protocol.

## Current conclusion

Existing task30 results show the main methodological gap is not generic routing. Existing methods already cover:

- exploration;
- contextual modeling;
- nonlinear routing;
- residual effect estimation;
- transfer;
- newcomer burn-in.

Benchmark/baseline freeze is required before defining Ours.

## Next

1. Expand review with external literature verification.
2. Decide final main/appendix benchmark split.
3. Decide final main/appendix baseline split.
4. Update TODO_LOG after completion.

## External verification progress

Completed first-pass literature verification.

- LLMRouterBench is consistent with the target setting of model/tool routing: query-level context, multiple candidate models, and per-instance performance differences.
- ASlib/SAT12-ALL is consistent with classical algorithm selection: instance features map to algorithm choices with heterogeneous performance.
- The combination provides coverage from modern LLM routing and established algorithm selection rather than relying on only one community.

## Updated decision

Current freeze remains:

Main benchmark:
1. LLMRouterBench
2. ASlib SAT12-ALL
3. BBB

Appendix:
- Synthetic sparse challenger advantage benchmark

Remaining work:
- freeze exact split/protocol/metrics;
- freeze final baseline table.
