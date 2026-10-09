# ADR-0038：margin-triggered propensity residual JEV 停止

- 状态：Accepted
- 日期：2026-09-27
- 依据：`task/task49-pharma-benchmark-baseline-freeze/margin_propensity_jev_v1/report.md`

## 背景

统一探索的 PCR-JEV 把探索成本支付在所有题上。第二个候选只在冻结 top-two correctness score 接近的题上探索和更新，并用 fit 区中位 margin 固定模糊题区域。

## 证据

5 folds × 2 seeds × delay `0/1/4` 的 mean stream accuracy 为 Frozen `0.8831`、Margin-Audit-Frozen `0.8658`、Margin-PCR-JEV `0.8616`。PCR 相对同探索对照在三个 delay 都为 `-0.00416`。

## 决策

停止该候选，不调 margin 门限或探索率，不申请远程任务。低 margin 探索和 residual 校正不能在当前数据上形成主方法。

