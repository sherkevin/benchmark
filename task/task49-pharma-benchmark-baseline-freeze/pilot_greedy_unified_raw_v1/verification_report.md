# Greedy unified raw verifier v1 — smoke report

日期：2026-09-30

## 验证结果

- Receipt：`experiments/logs/task49_greedy_unified_raw_20260930/smoke_v1/`
- Declared commit：`63028b68cc00924a39f4f1b6279f025c8478673d`
- 规模：2 streams（fold0/seed0/delay0，Frozen + Online）、20 decisions/terminals/feedback。
- 结果：`verification.json` 为 `verified`。
- 独立重放：从 checkpoint 重建 hidden-64；从 fit 区重算 ridge prior；重算 Greedy LinUCB UCB 分数、argmax、延迟 selected-only feedback 和 rank-one state update。

## 篡改拒绝

`tamper_checks.json` 保留了两次失败验证：

1. 篡改 online feedback 的 `state_hash_after`：拒绝，报告 `feedback state after ... mismatch`。
2. 篡改 online decision 的 UCB score：拒绝，报告 `score replay mismatch`。

临时篡改目录未保留；原始 receipt 未改动。尚未运行 full effect，也未打开 confirmation。

## Full receipt（后续完成）

随后生成的 `experiments/logs/task49_greedy_unified_raw_20260930/full_v1/` 也由同一 verifier 独立重放：60 streams、8,664 decisions/terminals/feedback，状态 `verified`。聚合 selected `system_success` 为：

| delay | Greedy Online | Greedy Frozen |
| ---: | ---: | ---: |
| 0 | 0.851108 | 0.856648 |
| 1 | 0.844875 | 0.856648 |
| 4 | 0.862881 | 0.856648 |

这只是统一 receipt 和 baseline qualification 证据，不改变其开发阶段、负结果和 confirmation 关闭边界。
