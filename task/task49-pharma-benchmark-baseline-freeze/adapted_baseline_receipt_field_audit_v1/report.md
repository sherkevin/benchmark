# Adapted baseline raw receipt field audit v1

- 日期：2026-10-04
- 范围：只读 schema 审计；不重跑、不排名、不读取 confirmation。
- 收据：`audit.json`
- 脚本：`src/probe_headless/audit_adapted_baseline_receipt_fields_v1.py`

## 结果

四条 adapted receipt（Greedy LinUCB、PILOT、BayesianRouter native-BT、Gaussian reward-ridge）
都满足：60 streams、8,664 decision、8,664 terminal、8,664 feedback，且独立 verifier
状态为 `verified`。每条 decision/terminal/feedback 都保存了 `state_hash_before/after`，
terminal/feedback 还保存了 runtime status、`system_success` 和 scored/failure feedback。

它们都没有保存 `state_bytes_before/after`，也没有保存逐事件 `unknown_reason` 或
`refusal_action`。因此当前成本表继续把 `state_bytes_p95` 和 refusal/unknown utility
相关字段标为 `MISSING`；不能从 hash、模型维度或 Python 对象大小推断这些数值。

## 对验收的影响

这次审计把“字段有没有被记录”与“字段是否缺失”分开了：adapted receipt 的事件完整性和
failure accounting 入口是存在的，但资源账本和 refusal/unknown 语义仍未闭合。它不增加
Gate B/C 分数，不把 adapted 方法移入 strict 主表，也不触发新的 GPU 实验。
