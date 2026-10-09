# Margin-triggered propensity residual JEV v1：CPU 证伪结果

日期：2026-09-27

## 结果

在同一 5 folds × 2 seeds × delay `0/1/4` development 回放中，用 outer-fold fit 区的中位 top-two margin 定义模糊题，模糊题才以 $ε=0.5$ 探索：

| 方法 | mean stream accuracy |
| --- | ---: |
| Frozen | 0.8831 |
| Margin-Audit-Frozen | 0.8658 |
| Margin-PCR-JEV | 0.8616 |

Margin-PCR-JEV 相对同探索 Margin-Audit-Frozen 在 delay 0/1/4 都是 `-0.00416`。三个 delay 的结果完全没有超过同探索对照，也没有超过 Frozen。

原始逐流证据：`experiments/logs/task49_margin_propensity_jev_20260927/results_v1.json`。

## 裁定

停止 margin-triggered propensity residual 这条线。把探索集中到低 margin 题，仍不足以让 selected-only provider residual 产生净收益；不调门限或探索率，不申请 A800。

