# AAMAS2027 Evaluation Protocol

> 2026-09-22 范围更正：下文 frozen-v1 仅描述当时的通用新工具接入实验，不是当前医药具体模型选择协议。现目标与待办见 [论文主线与进度](../docs/AAMAS2027_论文主线与进度.md) 和 [任务清单](../docs/任务清单.md)。保留原记录，不据此自动重开实验或修改真实结果。

> Status: frozen-v1
> Purpose: freeze evaluation protocol before designing Ours.

## 1. Unified Problem Definition

All benchmarks are mapped into selective challenger onboarding.

Given:

- context/instance x in X;
- available tools A;
- incumbent tool a_i;
- arriving challenger tool a_c;
- reward function r(x,a).

The target quantity is:

Δ(x)=r(x,a_c)-r(x,a_i)

The system should identify contexts where Δ(x)>0 using limited feedback.

## 2. Required Benchmark Properties

Each benchmark must provide:

1. multiple candidate tools/algorithms/models;
2. context-dependent performance variation;
3. per-instance reward comparison;
4. simulation of new challenger arrival;
5. limited feedback onboarding setting.

## 3. Main Benchmarks

### LLMRouterBench

Role:
modern model/tool routing.

Mapping:
- x: user query;
- a: candidate LLM/tool;
- r: task quality metric.

### ASlib SAT12-ALL

Role:
classical algorithm selection.

Mapping:
- x: problem instance features;
- a: solver algorithm;
- r: solver performance.

### BBB

Role:
scientific black-box tool selection stress test.

Mapping:
- x: scientific task instance;
- a: tool;
- r: evaluation score.

## 4. Metrics

Primary:

- cumulative regret;
- challenger precision among adopted cases;
- positive-region identification quality.

Safety:

- incumbent degradation;
- unnecessary challenger usage.

## 5. Protocol Freeze Requirements

Before Ours:

- freeze splits;
- freeze incumbent construction;
- freeze challenger arrival protocol;
- freeze baseline list.

## 6. Split Protocol

The evaluation should avoid random aggregate comparison only. Each benchmark is transformed into a temporal challenger onboarding setting.

### Train / Validation

Used for:
- learning context representation;
- estimating incumbent weakness regions;
- hyperparameter selection.

### Test

Used for:
- unseen contexts;
- unseen challenger arrival events;
- final regret measurement.

The test set must not expose challenger labels during decision making.

## 7. Challenger Arrival Simulation

For each evaluation episode:

1. construct incumbent set A_i;
2. introduce challenger a_c;
3. provide limited observation budget B;
4. decide whether to adopt challenger for each context region;
5. evaluate accumulated reward.

The key difficulty is not identifying the globally strongest algorithm, but discovering where a new challenger has local advantage.

## 8. Baseline Freeze

Main comparison families:

### Contextual Bandits

- LinUCB
- LinTS

Purpose:
standard context-aware exploration.

### Adaptive Selection Methods

- ACTS
- RFF-UCB

Purpose:
non-linear context-performance relationship modeling.

### Preference / Pareto Style Methods

- ParetoBandit-style methods

Purpose:
multiple-objective tool selection and trade-off analysis.

Appendix baselines:

- UCB1
- Beta-TS
- conservative variants

These methods are not the main contribution comparison because they lack explicit context modeling or challenger-aware formulation.

## 9. Failure Mode Analysis

The evaluation should report not only average reward but why selection fails.

Required cases:

1. False adoption:
challenger selected although Δ(x)<0.

2. Missed opportunity:
challenger ignored although Δ(x)>0.

3. Cold-start failure:
insufficient observations for reliable challenger evaluation.

4. Distribution shift:
new contexts differ from historical observations.
