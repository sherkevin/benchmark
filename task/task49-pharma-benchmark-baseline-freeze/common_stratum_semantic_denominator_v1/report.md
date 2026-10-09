# Common-stratum semantic denominator audit v1

日期：2026-10-01  
状态：只读审计，已完成；不改变方法、benchmark 或 confirmation 状态。

## 目的

这一步只回答一个分母问题：已验证的 common-stratum full receipt 是否真的包含
`attempt / scored / runtime failure / unknown / refusal` 这些类别，还是只有 pooled
accuracy。它不估计效果，也不把 semantic smoke 的 fixture 当成主实验结果。

## 输入与复核边界

- 原始收据：`experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/events.jsonl`
- 收据已有独立验证：`verification_v1.json`，180 streams、77,976 events。
- 审计脚本：`src/probe_headless/analyze_common_stratum_semantic_denominators_v1.py`
- 机器结果：`denominators.json`（原始机器输出副本；日志目录保留同名收据）
- 原始 events SHA-256：由机器结果中的 `events_sha256` 固定。

脚本不导入 runner，不读取输入 truth，不补造缺失事件；它要求每个 decision 恰好有一个
terminal 和一个 feedback。`unknown` 只按显式的 null `chemical_correct` 或
`feedback_status ∈ {unknown, unscorable}` 计数；`refused` 只按显式 refusal action
或 refusal feedback status 计数，不把 provider failure 推断成拒答。

## 结果

| method | attempts/calls | scored | runtime failure | unknown | refused | correct | wrong |
|---|---:|---:|---:|---:|---:|---:|---:|
| Frozen direct | 4,332 | 4,299 | 33 | 0 | 0 | 3,906 | 393 |
| Matched Online-SGD | 4,332 | 4,297 | 35 | 0 | 0 | 3,865 | 432 |
| Greedy | 4,332 | 4,309 | 23 | 0 | 0 | 3,844 | 465 |
| Bayesian | 4,332 | 4,307 | 25 | 0 | 0 | 3,749 | 558 |
| MIRA-relative | 4,332 | 4,299 | 33 | 0 | 0 | 3,911 | 388 |
| MIRA-FamilyGuard | 4,332 | 4,322 | 10 | 0 | 0 | 3,932 | 390 |

每个方法 3 个 delay、5 个 fold、2 个 seed 各有 1,444 次 attempt；每个 attempt
都有一次 terminal 和一次 feedback。按 delay 的明细写在机器 JSON 中，未把不同 delay
当成独立 molecule 样本。

## 解释

1. common-stratum 的 unknown 和 refused 均为 0，说明这份主回放没有测试 unknown
   censoring 或主动 refusal。它不是“方法不会遇到 unknown/refusal”的证据。
2. runtime failure 被保留在 attempts 分母内；failure 不携带 chemical correctness。
3. `refused=0` 与 `unknown=0` 是输入覆盖范围的结果，不能补足 E5 的 refusal utility、
   accepted/refused-rate 对照或五折 verifier full table。
4. 这份审计关闭了“主表是否漏报 failure/unknown/refusal 字段”的疑问，但没有关闭
   Gate C：统一 E1--E5 矩阵、baseline 公平合同、paired uncertainty 和 confirmation
   仍未完成。

## 下一步

把这份分母结果作为统一 E1--E5 主表的 common-stratum 行；另列 family-outage 的
受控事件行和 semantic smoke 的语义行，不把三者合并成一个 accuracy 分数。若继续做
refusal 效果，必须先固定 utility 和 refusal rate 对照，再登记独立的 fit-only 五折实验。
