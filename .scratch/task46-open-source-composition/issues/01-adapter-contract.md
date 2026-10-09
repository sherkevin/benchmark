# 01：provider adapter 与延迟反馈合同

Type: task
Status: open
Blocked by:

## Question

如何把 Task36 replay 和未来 tau2 pharma domain 统一成 `ToolAdapter`、`PendingFeedback`、`Trace` 接口，并保证 selected-only、candidate mask、真值延迟和版本 hash 可审计？

## Acceptance

- 固定 JSON schema 和最小 Python protocol。
- 合同测试覆盖未选工具不更新、重复调用被屏蔽、pending 到达后只更新一次、日志保留 propensity。
- 不调用预测服务、不生成新科学结果。
