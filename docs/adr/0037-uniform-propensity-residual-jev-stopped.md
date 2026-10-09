# ADR-0037：统一随机探索的 propensity residual JEV 停止

- 状态：Accepted
- 日期：2026-09-27
- 依据：`task/task49-pharma-benchmark-baseline-freeze/propensity_residual_jev_v1/report.md`

## 背景

selected-only 反馈可能有选择偏差。PCR-JEV 用固定 20% 的均匀随机探索取得正 propensity，再用冻结 correctness probability 的 residual 做 provider 级 ridge 校正。

## 证据

5 folds × 2 seeds × delay `0/1/4` 的 mean stream accuracy 为 Frozen calibrated `0.8831`、同探索 Audit-Frozen `0.8748`、PCR-JEV `0.8493`。PCR-JEV 相对同探索对照三个 delay 都为负。

## 决策

停止该候选，不调探索率、ridge 或校正幅度，不申请远程任务。保留 propensity 和延迟更新的实现思路，但不把它写成动态 JEV 主方法。

