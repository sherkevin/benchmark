# Cross-method fairness audit v1

日期：2026-09-30  
状态：只读合同审查；不重跑、不调参、不打开 confirmation。

## 目的

核对当前可运行主 baseline 与 MIRA unified receipts 是否可以放进同一张 superiority 排名表。审查维度固定为：输入/menu/fold/seed/delay、feature representation、fit-only warm-start、selected-only feedback/update budget、provider-call/cost accounting 和 receipt completeness。

## 收据范围

- MIRA：`experiments/logs/task49_mira_unified_raw_20260930/full_v1/`
- Matched Online-SGD：`experiments/logs/task49_matched_sgd_unified_raw_20260930/full_v2/`
- native-BT BayesianRouter：`experiments/logs/task49_bayesianrouter_native_bt_raw_20260930/full_v1/`
- Greedy LinUCB：`experiments/logs/task49_greedy_unified_raw_20260930/full_v1/`
- PILOT-adapted：`experiments/logs/task49_pilot_unified_raw_20260930/full_v1/`

只读取各 receipt 的 config/summary/verification/manifest；不读取或修改 raw events，不生成新的效果数字。

## 判定规则

1. `input_sha256`、provider roster、fold/seed/delay、selected-only、feedback clock、unknown semantics 全部一致，才记为 shared protocol。
2. feature 表示和 warm-start 必须分别列出；即便维度相同，也不能把不同 encoder/prior 当作同一算法输入。
3. online−frozen 是每个方法内部的 primary paired effect；跨方法 raw accuracy 只有在 feature/warm-start/update/cost 都匹配时才允许排序。
4. 记录 baseline 是否有统一逐事件 receipt 和独立 verifier；contract 完整不等于 scientific comparability。
5. 任何缺少真实 provider inference cost 的回放只能报告 cached call count 和 policy update latency，不能声称 cost-aware superiority。
