# ADR-0041：PILOT-adapted selected-only 更新不作为动态主方法

- 状态：Accepted
- 日期：2026-09-27
- 依据：`task/task49-pharma-benchmark-baseline-freeze/pilot_adapted_development_v1/report.md`

## 背景

PILOT 是当前任务必须补齐的现代在线 baseline。此前仓库只有 cosine/UCB/ridge online kernel 和随机 fixture prior，不能称为完整适配。

## 证据

在 5-fold scaffold split、2 个 fit seed、delay `0/1/4` 的 60 条真实 development 流上，PILOT-adapted Frozen 为 `0.882964`；Online 分别为 `0.882271/0.880194/0.885042`。Online−Frozen 分别为 `-0.000704/-0.002773/+0.002074`，每个 delay 只有 `4/10` paired 流为正；Best Fixed 为 `0.915512`。逐事件日志、输入 hash、prior 和独立恢复均通过。

## 决策

将 PILOT-adapted 保留为现代 baseline，不把普通 cosine/UCB/ridge 更新写成动态 JEV 主方法，不为它单独申请 A800 完整任务。

## 后果

方法主线必须提供超出“离线 preference prior + selected-arm ridge”的新信息或新安全机制。后续若 Proposed 只超过 PILOT 而不能超过 matched controls 或 Best Fixed，不能据此宣称实时学习成立。
