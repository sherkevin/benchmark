# Stage-B scientific submission gate v3

日期：2026-10-06

## 判定

在声明范围“公开 TDC BBB truth、四个 checkpoint-bound fresh providers、一次调用、selected-only delayed feedback、延迟 `{0,1,4}`，并把 correctness drift 明确标为 controlled stress”内，判定仍为 **PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE**。

本版本修复了审计链的一致性问题：MIRA-Context 的 `fit_rates`、`successes`、`counts` 和 `prior_count` 已纳入序列化状态。自然 fresh 重跑 270 条 stream，controlled stress 主收据 120 条 stream，并补充 30 条 BayesianRouter stress comparator；合计 150 条 stress stream，contract/replay verification 均全通过。

自然 fresh：MIRA-Context `89.81%`，Frozen `89.81%`，paired delta `0.00 pp`，CI `[-1.24,+1.40]`；164 次动作变化，82 rescued、82 harmed。

Controlled stress：MIRA-Context `87.56%`，Frozen `73.38%`，paired delta `+14.18 pp`，CI `[+9.61,+19.03]`。

补充的 BayesianRouter stress comparator 为 `80.28%`，相对 Frozen `+6.90 pp`，CI `[+3.50,+10.41]`；它没有改变 MIRA 的参数或主估计量，只用于证明受控压力表没有遗漏现代 adapted baseline。

## 边界

- stationary fresh menu 上没有自然 online gain；不能写成 generic improvement。
- controlled stress 是预登记 evaluator-side label flip 的机制压力，不是自然部署漂移。
- 当前 verifier 是 contract/replay verifier，复用 runner primitives，独立于 aggregation；不宣称第二套独立实现。
- post-call verifier/refusal、端到端 Agent、hERG/AMES 和 full 33-provider model-fresh 仍不属于本次 gate。

## 证据

- natural receipt：`experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/`
- natural verification：`experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/verification.json`
- natural aggregation：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_context_natural_v1/aggregation_v2_run/`
- stress receipt：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v2_statehash_run2/`
- stress verification：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v2_statehash_run2/verification.json`
- stress aggregation：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_context_natural_v1/stress_aggregation_v2_run/`
- BayesianRouter stress receipt：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v3_bayesian_statehash_run1/`
- BayesianRouter stress report/aggregation：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_drift_stress_v2_bayesian/`
