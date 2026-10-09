# Family-outage smoke v1

日期：2026-10-01

## 目的

检查 family-outage 全量回执前的两个硬条件：

1. exposed 流确实只在封存的 target family 上注入 failure，并按 `decision_tick + 1 + delay` 释放；
2. unexposed 流不注入任何反馈，和对应 no-event control 的 action、terminal、feedback、版本和状态语义完全一致。

这不是效果实验，也不打开 confirmation。

## 配置

- run：`family_outage_v1_smoke_v6`
- 4 streams：fold0 / seed0 / delay4；`relative_reliability` 和 `matched_sgd`；transient 与 persistent 各一条。
- 共同 target manifest：`family_outage_v1_target_manifest_v6.json`，manifest self-hash：`bc3df04fd2a7040684c9d9c99f3cff9f8bb2077227e6668d6f397e3df0094382`。
- event receipt code commit：`d93b5a5cb1a3e128886336c7b41df3446ec4eda1`。
- 原始事件：`events.jsonl`，SHA-256 记录在 verifier receipt 中。

## 验收结果

独立复核器没有导入 family-outage runner，直接从 JSONL 重建 stream identity 和反馈队列。

| 项目 | 结果 |
|---|---:|
| streams | 4 / 4 |
| exposed | 2 |
| unexposed | 2 |
| injected target-family failures | 12 |
| selected-only / exactly-once feedback | 通过 |
| manifest entry 与 event record 绑定 | 通过 |
| event entity / target action 绑定 | 通过 |
| unexposed full semantic equality | 通过 |
| replay verifier | `verified` |

机器收据：

`experiments/logs/task49_mira_event_replay_cpu_20261001/family_outage_v1_smoke_v6/verification_v2.json`

分析器也已在这 4 条流上 dry-run，通过输入、manifest、target verification、replay verification、control receipt 和 family registry 的 hash 绑定。因为 smoke 只有 delay4 且该坐标不在预封存的 `S=23` 中，`n_support_pairs=0` 是预期结果，不能据此做跨方法效果判断。

分析收据：

`experiments/logs/task49_mira_event_replay_cpu_20261001/family_outage_v1_smoke_v6/metrics_v1.json`

## 当前结论

family-outage 的运行协议和分析入口已经具备跑 240 条 CPU receipt 的条件。全量结果仍只能在独立 verifier 通过后读取；主估计量是全 30 坐标 ITT 和封存共同支持集 `S` 的 paired delta，exposure 与 leave-family latency 作为配套指标。该协议只支持 `feedback-adaptive` controlled family stress，不支持自然 provider drift、version shift 或 provider-level superiority。
