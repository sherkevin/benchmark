# Reward-ridge Gaussian baseline semantic smoke v1

日期：2026-10-01  
状态：已完成；协议通过，尚无效果结论。

## 收据

- 运行目录：`experiments/logs/task49_reward_ridge_gaussian_smoke_20261001/v2/`
- 声明代码 commit：`ea1b8430620ef1bfe4ec4db33606f885a00b5aa`
- 独立 verifier：`src/probe_headless/verify_greedy_unified_raw_v1.py`
- verifier 状态：`verified`，2 streams、20 decisions、20 terminals、20 feedbacks。
- 唯一 run id：`task49_reward_ridge_gaussian_smoke_20261001_v2`

初次 v1 收据也保留在同一日志目录，但沿用了旧 Greedy run id，作为 provenance engineering
failure，不纳入结果。修复后 v2 使用唯一 run id 重跑并通过同一独立 verifier。

## 固定的 prior 语义

| 项目 | 值 |
|---|---|
| feature | sealed BT+CLS hidden-64，fold 0 / seed 0 |
| fit target | fit rows 的 binary `system_success` |
| `lambda_fit` | 1.0 |
| Gaussian posterior precision | 50.0 |
| initial covariance | `0.02 I` |
| observation noise | 1.0 |
| LinUCB alpha | 0.5 |
| cohort | evaluation 前 10 个分子，固定顺序 |
| delay | 0 |

`lambda_fit` 与 posterior precision 已分别记录在 `config.json`，没有把它们合成一个
regularization。Gaussian score 没有隐式 clipping。独立 verifier 重算了 fit-only prior、
初始 covariance、UCB score、selected provider、延迟队列、rank-one 更新和 state hash。

## 观察到的结果

Frozen 和 Online 两条 stream 各有 10 次调用，均为 8/10 joint-success；这是 smoke cohort
上的协议输出，不能当作方法效果或 superiority。两者的差异只用于检查 online 版本确实
消费了 10 条 selected feedback（最终 version=10），Frozen 保持 version=0。

## 结论与下一步

这次 smoke 证明 reward-ridge Gaussian 对照可以在同一 selected-only contract 下表达，
并且它的拟合正则与 posterior precision 没有混淆。它还没有证明该 baseline 在全开发集上
优于或劣于 MIRA，也没有关闭 Gate B。

下一步按相同合同登记 60-stream full receipt；在 full receipt 完成前，以 Matched Online-SGD
作为合法 logistic 强对照，本 smoke 只作为 Gaussian baseline 的接口证据。
