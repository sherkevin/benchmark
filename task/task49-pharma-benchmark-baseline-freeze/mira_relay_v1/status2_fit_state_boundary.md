# Status2 fit-state 边界说明

状态：候选边界（只读审查，2026-10-03）；不修改当前生效的 preregistration、serializer 或 replay runner。

## 结论

fold0–4 的 trust-exact candidate 均通过独立 verifier。fold2 的 SciPy 原始返回码是 `2`，`scipy_success=false`，但在预先约定的数值条件下是有限的 stationary candidate：梯度无穷范数为 `2.2856494087675205e-7 <= 1e-6`，Hessian 对称误差为 `2.2737367544323206e-13 <= 1e-10`。因此 fold2 可以作为**显式标注的 status2 候选 fit state**保存和审计，但不能被静默改名为 `accepted` 或 `success`。

当前 fit-state v2 合同仍然是 status0-only。默认 serializer 拒绝 `numerically_stationary_status2`，这是有意的边界，避免把优化器的非成功返回误写成普通成功状态。

## 五折审查证据

| fold | candidate status | SciPy status | `scipy_success` | gradient $\|g\|_\infty$ | Hessian symmetry error | verifier |
|---:|---|---:|:---:|---:|---:|---|
| 0 | `accepted` | 0 | true | $2.33\times10^{-9}$ | $1.14\times10^{-13}$ | verified |
| 1 | `accepted` | 0 | true | $2.45\times10^{-8}$ | $2.27\times10^{-13}$ | verified |
| 2 | `numerically_stationary_status2` | 2 | false | $2.29\times10^{-7}$ | $2.27\times10^{-13}$ | verified |
| 3 | `accepted` | 0 | true | $1.19\times10^{-7}$ | $1.14\times10^{-13}$ | verified |
| 4 | `accepted` | 0 | true | $1.30\times10^{-10}$ | $1.14\times10^{-13}$ | verified |

原始 receipt 位于 `experiments/logs/task49_mira_relay_fit_diagnostic_20261003/trust_candidate_crossfold_v1/fold{0..4}_seed0/candidate_receipt.json`；汇总及状态保留在同目录 `crossfold.jsonl`。独立 verifier 重算 objective、gradient、analytic Hessian、参数哈希、输入/源码绑定和有限性；fold2 的 `status2` 没有被重写。

## 如果以后需要封装 status2

必须新增显式配置开关（例如 `allow_status2=false`，只有调用方明确置为 true 才允许）或独立的 status2 fit-state contract。启用时，receipt 和 state config 至少保留：

1. `candidate_status: numerically_stationary_status2`；
2. `scipy_status: 2` 与 `scipy_success: false`；
3. `status2_allowed: true`，以及 status2 的梯度、Hessian、finite 条件；
4. 完整输入、源码、参数和 canonical state hash；
5. `optimizer_status_policy: status0_or_numerically_stationary_status2`。

独立 verifier 必须分别验证这些原始字段和数值条件，不能只检查 state hash。回放 receipt 中可以将运行时 `error` 归一为 `failure`，但不得改变 fit candidate 的 `status2` 语义。

## 当前边界与后果

- 当前生效策略：`optimizer_status_policy: status0_only`；现有 v2 state pack 不接收 fold2。
- fold2 的候选参数可继续保留作审计和后续敏感性分析，但尚未授权任何效果矩阵、confirmation set、preregistration 更新、Nebula/GPU 回放或论文 claim。
- 若要在五折效果矩阵中使用 fold2，先单独记录并确认新的合同/决议，再以 status2 policy 生成独立 state pack 和 replay；不得把 status0 与 status2 混成一个未标注的 fit-state 集合。

这份说明只界定封装与审计边界，不改变既有实验结论。
