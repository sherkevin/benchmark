# Matched effect matrix v3：fold2/seed0 receipt report

范围：五折 v3 的第三条 fold receipt；CPU、144 evaluation rows、3 methods × 3 delays；显式保留 status2 数值驻点策略；不是 confirmation，也不是统计结论。

## Receipt

- Raw receipt：`experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold2_seed0/`。
- 9 streams、3,888 events；加强后的独立 verifier `verification_v2.json` 为 `verified`。
- 事件包含 pending queue、release ordinal 与 released feedback IDs；raw manifest 记录所有文件 hash。
- fit-state 的原始 SciPy status=2 / success=false 保留在 sealed config；仅按预登记的 numerically stationary status2 policy 运行，未改写为 status0。

## Paired readout

| Delay | MIRA | Independent | NoUpdate | MIRA vs Independent | MIRA vs NoUpdate |
|---:|---:|---:|---:|---:|---:|
| 0 | 130/144 | 130/144 | 129/144 | 0 / 0 | 2 / 1 |
| 1 | 130/144 | 130/144 | 129/144 | 0 / 0 | 2 / 1 |
| 4 | 130/144 | 130/144 | 129/144 | 0 / 0 | 2 / 1 |

## Mechanism checks

- MIRA family-transfer events by delay: D0=16, D1=16, D4=15.
- Independent provider-local updates by delay: D0=98, D1=99, D4=102.
- All 144 selected returns were runtime-valid in all nine streams.

## Interpretation

fold2 相对 Independent 没有 rescued/harmed；相对 NoUpdate 在三个 delay 都是 2/1，净增 1 条。这个方向与 fold0/1 的小幅信号部分一致，但仍不能当作跨折效果；必须完成 fold3/4 后以 fold 为单位计算不确定性。delay4 不能删除。该 receipt 只用于预注册的 development gate，不进入 confirmation 或论文结果表。
