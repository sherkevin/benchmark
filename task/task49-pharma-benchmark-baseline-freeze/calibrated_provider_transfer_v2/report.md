# Calibrated provider residual transfer v2：结果与裁定

## 这次修了什么

旧 provider-transfer 直接使用 BT raw score，残差单位不成立。本版先在每个 outer-fold fit 区做 per-provider affine logistic calibration，再用 fit-only correctness residual covariance 形成 provider transfer matrix；evaluation 只把已选 provider 的 delayed `system_success` 反馈写入 source residual mean。`permuted_transfer` 只打乱 transfer matrix 的目的 provider 行，保留相同反馈与计算预算。

## 结果

| 方法 | delay 0 | delay 1 | delay 4 |
| --- | ---: | ---: | ---: |
| Frozen calibrated | 0.8831 | 0.8831 | 0.8831 |
| Independent residual | 0.8762 | 0.8769 | 0.8630 |
| Calibrated transfer | 0.8755 | 0.8783 | 0.8817 |
| Permuted transfer | 0.8706 | 0.8720 | 0.8755 |

Transfer 相比 independent 的差值是 `-0.0007/+0.0014/+0.0187`，但三个 delay 都低于 Frozen（`-0.0076/-0.0048/-0.0014`）；delay 0 还低于 independent。真实 transfer 比置换版本高，说明 fit-only provider 关系确实带来信号，但它没有转化为稳定的直接选择收益。

## 裁定

按预登记停止 calibrated provider-transfer 主线，不调 shrinkage、clip 或矩阵稀疏度，不申请 A800，不打开 confirmation。保留结果作为“有 provider 关系但当前在线利用会伤害冷启动”的边界证据。

证据：`experiments/logs/task49_calibrated_provider_transfer_20260927/`；协议：`preregistration.md`；入口：`src/probe_headless/replay_pharma_calibrated_provider_transfer.py`。
