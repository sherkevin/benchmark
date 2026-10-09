# 知会 E6 — task-06 结论：H2 内置工具泄漏已堵（解除你 task-05 §1 前置第 8 项）

> 发出方：E4（agent-harness 线，task_id=19）。时间：2026-08-08。
> 对应你 `task/E6-三层矩阵评测/task-05*.md` §1 前置表第 8 行：
> 「H2 内置工具泄漏 | ⏳ E4 报 fixed 跑泄漏 12 条，已派 E4-06 堵；若开跑前未堵，如实标注受影响的格位」。

## 结论（一句话）

**前置第 8 项可以解除**：重清点后 task-05 登记的泄漏实质为 0（无一条触达 dag_poc/data、无一条联网），且封堵已升级为三层运行时禁止并同题重跑验证通过。H2 半边矩阵不需要标注受影响格位。

## 三层事实

1. **重清点订正（t06_leak_inventory.py，可复跑）**：task-05 两条 `_full` 流共 50 条非契约工具事件 = 46 条拒绝信封（信息零返回）+ 4 条 CLI 内部 todo（TaskCreate/TaskUpdate，不触数据不联网）。**实质泄漏 0 条**。原报「Bash 实读工作区」系更早 bypassPermissions 探测轮的事，现行参数组合下不存在。
2. **封堵升级（t05_h2_run_cli.py claude 分支）**：① --disallowed-tools 补齐 Task*/MCP 资源工具；② --settings permissions.deny；③ PreToolUse hook 白名单闸门（只放行 mcp__lingshu-mock__*）。三层各自独立冒烟实测生效（证据 t06_smoke_stream*.jsonl）。
3. **同题重跑验证（case_co_drug_01×claude，998s，4/4 PASS）**：MCP 资源工具/Task* 尝试归零；残留 6 条 Read/Bash 尝试全部被拒（信息零返回），逐条举证见 `t06_leak_evidence.md`。

## 你开跑前要知道的两件事

- **H2 基线配置**：矩阵跑 H2 请沿用现行 `t05_h2_run_cli.py` 的 claude 分支参数（三层封堵在内）；复跑命令与产物结构见 `report-task-06.md` §五交付物索引。
- **排期提醒**：模型偶发尝试 Read/Bash（被拒、耗轮次），H2 实测耗时波动大（692s / 998s 同题两次），建议按 open×H2 1313s 的上限量级留排期余量（task-05 已单独知会过）。

## 证据与复跑入口

| 材料 | 位点 |
|---|---|
| 收口报告 | `task/E4-agent-harness搭建/report-task-06.md` |
| 逐条举证清单 | `task/E4-agent-harness搭建/t06_leak_evidence.md`（含重跑残留 6 条） |
| 清点脚本 | `./.venv/bin/python task/E4-agent-harness搭建/t06_leak_inventory.py` |
| token/日志汇总 | `./.venv/bin/python task/E4-agent-harness搭建/t06_token_summary.py`（§7 硬门禁产物） |
