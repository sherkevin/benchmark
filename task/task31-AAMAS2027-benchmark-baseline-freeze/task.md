# task31 — AAMAS2027 Benchmark & Baseline Freeze Review

> 状态：✅ 已完成
> 目标：在设计 Ours 前冻结 benchmark 与 baseline 协议。

## 动机

Task30 已完成主要 nearest baseline closure。下一步不是继续无边界扩展实验，而是系统审查：

1. benchmark 是否覆盖问题定义；
2. baseline 是否覆盖已有方法空间；
3. reviewer 是否可能提出遗漏。

## 工作范围

### Benchmark

审查：
- LLMRouterBench
- ASlib SAT12-ALL
- BBB
- Synthetic controlled benchmark
- 其他候选 routing / algorithm selection benchmark

### Baseline

审查：
- incumbent-only
- classical contextual bandits
- residual/treatment effect methods
- nonlinear routing
- transfer methods
- newcomer onboarding methods

## 输出

- research/AAMAS2027_Benchmark_Selection_Review.md
- research/AAMAS2027_Baseline_Selection_Review.md
- report.md

完成后冻结 T3 evaluation protocol，再进入 Ours。
