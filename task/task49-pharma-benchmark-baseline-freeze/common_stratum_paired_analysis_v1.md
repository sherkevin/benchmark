# Common-stratum molecule-paired analysis

状态：描述性分析，不给 pooled significance claim。

原始汇总：`experiments/logs/task49_common_stratum_paired_20260930/v1.json`。每条 stream 内先按 molecule 配对，再计算 online−Frozen 或 online−no-update 的平均差；没有把 method/seed/delay 的重复 cell 当成独立样本。

| 对比 | streams | 平均 stream 差 | 正差 stream 数 | 最小 / 最大 |
|---|---:|---:|---:|---:|
| Matched SGD − Frozen | 30 | −0.00946 | 7/30 | −0.04167 / 0.01389 |
| Greedy online − Frozen | 30 | −0.01430 | 7/30 | −0.06250 / 0.04861 |
| Bayesian online − Frozen | 30 | −0.03623 | 2/30 | −0.08276 / 0.00694 |
| MIRA − Frozen | 30 | +0.00115 | 9/30 | −0.00694 / 0.01379 |
| Greedy online − Greedy no-update | 30 | −0.01430 | 7/30 | −0.06250 / 0.04861 |
| Bayesian online − Bayesian no-update | 30 | −0.01845 | 5/30 | −0.06897 / 0.02069 |

这说明当前回放中，MIRA 的差异接近零，三个普通在线更新基线没有稳定的正差；Gaussian 方法的 online−no-update 也偏负。这个结果只能作为诊断：Gaussian 先验尺度、更新目标、provider failure 暴露和固定分子顺序仍是可能解释，不能拿它证明 MIRA 已经有效或普通基线无效。

验收上，这一步关闭了“把 21,660 条 decision 当独立样本”的明显错误，但还没关闭正式统计门：需要按 scaffold 聚类、预先固定 fold-level randomization/cluster bootstrap，并补 provider-family macro 与 valid-return conditional correctness。
