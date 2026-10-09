# Matched effect matrix v3：fold3/seed0 receipt report

范围：五折 v3 的第四条 fold receipt；CPU、144 evaluation rows、3 methods × 3 delays；status0-only；不是 confirmation，也不是统计结论。

## Receipt

- Raw receipt：`experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold3_seed0/`。
- 9 streams、3,888 events；加强后的独立 verifier `verification_v2.json` 为 `verified`。
- 事件包含 pending queue、release ordinal 与 released feedback IDs；raw manifest 记录所有文件 hash。
- fold3 使用 status0-only fit-state 合同；无 status2 放宽。

## Paired readout

| Delay | MIRA | Independent | NoUpdate | MIRA vs Independent | MIRA vs NoUpdate |
|---:|---:|---:|---:|---:|---:|
| 0 | 131/144 | 131/144 | 129/144 | 0 / 0 | 4 / 2 |
| 1 | 131/144 | 131/144 | 129/144 | 0 / 0 | 4 / 2 |
| 4 | 131/144 | 131/144 | 129/144 | 0 / 0 | 4 / 2 |

## Mechanism checks

- MIRA family-transfer events by delay: D0=17, D1=17, D4=17.
- Independent provider-local updates by delay: D0=80, D1=79, D4=79.
- All 144 selected returns were runtime-valid in all nine streams.

## Interpretation

fold3 相对 Independent 没有 rescued/harmed；相对 NoUpdate 在三个 delay 都是 4/2，净增 2 条。这个 fold 只说明当前开发回放中的配对差异，不能单独支持论文 claim；必须和 fold0–2、fold4 一起按 fold 计算不确定性。delay4 仍保留为预注册压力条件。
