# Propensity-corrected residual JEV v1：CPU 证伪结果

日期：2026-09-27

## 结果

使用同一份已恢复的 Task49 development fit snapshots，5 folds × 2 seeds × delay `0/1/4`，没有新工具调用或训练：

| 方法 | 三种 delay 的 mean stream accuracy |
| --- | ---: |
| Frozen calibrated | 0.8831 |
| Audit-Frozen（同样 20% 探索） | 0.8748 |
| PCR-JEV | 0.8493 |

PCR-JEV 相对 Audit-Frozen 的 paired delta 为：delay 0 `-0.0263`、delay 1 `-0.0257`、delay 4 `-0.0243`。三个 delay 都低于同探索对照，也都低于 Frozen。

原始逐流证据：`experiments/logs/task49_propensity_residual_jev_20260927/results_v1.json`。

## 裁定

停止统一随机探索的 propensity residual JEV。它支付了明显探索成本，且 ridge residual 校正继续把选择推坏。这个结果不支持继续调 $ε$、ridge 或校正幅度，也不申请 A800。

## 可保留的东西

Horvitz–Thompson residual、已知 propensity、延迟队列和 O(1) 状态更新可以作为后续方法的实现部件；当前数据不支持把“全题随机探索 + provider 全局 residual”作为主方法。

