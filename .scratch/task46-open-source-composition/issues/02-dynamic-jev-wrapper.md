# 02：动态 Jev 与在线后验适配

Type: prototype
Status: open
Blocked by: 01-adapter-contract

## Question

如何把 AnyJev/Laya 的 typed decision 与 River/contextualbandits/bayesianbandits 接到同一事件日志，支持 select、verify、final 三类 decision，并记录 head/posterior version？

## Acceptance

- AnyJev `observe`、LinUCB、LinTS 和 posterior wrapper 使用同一 interface。
- 未调用工具不产生负标签；延迟真值只更新实际动作。
- 只做 fixture/回放合同测试；真实医药学习等待 Task38/P02 放行。
