# Matched effect matrix v3：fold0/seed0 receipt report

范围：五折 v3 的第一条 fold receipt；CPU、145 evaluation rows、3 methods × 3 delays；不是 confirmation，也不是统计结论。

## Receipt

- Raw receipt：`experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold0_seed0_r2/`。
- 9 streams、3,915 events；独立 verifier `verification.json` 为 `verified`。
- 事件新增 pending queue、release ordinal 与 released feedback IDs；raw manifest 记录所有文件 hash。
- 第一次启动只完成 precompute、无 replay 事件，保留在 `v3_fold0_seed0/` 并标记 aborted；本报告不使用它。

## Paired readout

| Delay | MIRA | Independent | NoUpdate | MIRA vs Independent | MIRA vs NoUpdate |
|---:|---:|---:|---:|---:|---:|
| 0 | 127/145 | 126/145 | 126/145 | 1 / 0 | 2 / 1 |
| 1 | 127/145 | 126/145 | 126/145 | 1 / 0 | 2 / 1 |
| 4 | 126/145 | 126/145 | 126/145 | 0 / 0 | 2 / 2 |

## Interpretation

这个 fold 的数字与 v2 一致：delay 0/1 相对两个对照各净 +1，delay 4 持平。v3 新增的是 queue 证据和独立 schema 核验，不是新的效果样本。它不能证明跨折稳定性；其余四折必须按同一合同完成后才能计算 fold-level paired uncertainty。
