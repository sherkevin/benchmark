# Matched effect matrix v3：fold4/seed0 receipt report

范围：五折 v3 的第五条 fold receipt；CPU、144 evaluation rows、3 methods × 3 delays；status0-only；不是 confirmation，也不是统计结论。

## Receipt

- Raw receipt：`experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold4_seed0/`。
- 9 streams、3,888 events；最新独立 verifier `verification_latest.json` 为 `verified`。
- 第一次 verifier 发现 runner 的阈值计数与旧核验器精确浮点计数不一致，原始拒绝收据保留在 `verification_rejected_summary_mismatch.json`；修正核验器后对同一 raw JSONL 重读通过，没有重跑 replay。
- 事件包含 pending queue、release ordinal 与 released feedback IDs；raw manifest 记录所有文件 hash。

## Paired readout

| Delay | MIRA | Independent | NoUpdate | MIRA vs Independent | MIRA vs NoUpdate |
|---:|---:|---:|---:|---:|---:|
| 0 | 128/144 | 128/144 | 128/144 | 0 / 0 | 1 / 1 |
| 1 | 128/144 | 128/144 | 128/144 | 0 / 0 | 1 / 1 |
| 4 | 128/144 | 128/144 | 128/144 | 0 / 0 | 1 / 1 |

## Mechanism checks

- MIRA family-transfer events by delay: D0=14, D1=14, D4=14.
- Independent provider-local updates by delay: D0=111, D1=111, D4=116.
- All 144 selected returns were runtime-valid in all nine streams.

## Interpretation

fold4 相对 Independent 没有 rescued/harmed；相对 NoUpdate 三个 delay 都是 1/1，净增 0。这个 fold 没有提供正向 paired signal。它与 fold0–3 一起进入跨折不确定性分析；delay4 保留为预注册压力条件。
