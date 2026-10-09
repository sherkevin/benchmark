# Stage-B scientific submission gate v2

日期：2026-10-06

## 判定

在声明范围“公开 TDC BBB truth、四个 checkpoint-bound fresh providers、一次调用、selected-only delayed feedback、延迟 `{0,1,4}`，并把 correctness drift 明确标为 controlled stress”内，判定仍为 **PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE**。

本版本补上了方法一致性缺口：此前自然 fresh 收据含 MIRA-Relative（已停止的旧版本），压力收据含 MIRA-Context（当前候选）。新增自然 fresh MIRA-Context 收据使用完全相同的 157 个候选、5 折、2 seed、3 delay 和事件合同：270 条 stream，独立回放 `270/270` 通过。MIRA-Context 为 89.81%，Frozen 为 89.81%，paired delta `0.00 pp`，CI `[-1.24,+1.40]`；动作发生 164 次变化，其中 82 次 rescued、82 次 harmed。

## 仍成立的边界

- stationary fresh menu 上没有自然 online gain；这保留为稳定性/无损边界，不能写成 generic improvement。
- controlled stress 上 MIRA-Context 相对 Frozen 为 `+14.18 pp`，CI `[+9.61,+19.03]`；这是预登记 evaluator-side label flip 的机制压力，不是自然部署漂移。
- post-call verifier/refusal、端到端 Agent、hERG/AMES 和 full 33-provider model-fresh 仍不属于本次 gate。

## 证据

- fresh natural MIRA-Context：`experiments/logs/task49_provider_fresh_context_natural_20261006/v1/`
- fresh natural report：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_context_natural_v1/report.md`
- controlled stress：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v1/`
- v1 scorecard：`task/task49-pharma-benchmark-baseline-freeze/stage_b_scientific_submission_gate_v1/`
