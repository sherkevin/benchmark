# 测试 0001 — API 能否触发「分阶段/不分阶段」分叉

## 目的

task5 的前提是必须触发智能体的「分阶段(并行) vs 不分阶段(单步)」选择,否则测的不是这两种模式。浏览器手动操作太慢且易卡死,需确认能否纯用 API 复现这个分叉,并摸清确切的请求/SSE 契约。

## 方法

逆向 `~/work/nobelclaw` 后端 `server/routes/chat.py`,再用 curl 实跑验证。入口:`POST /api/v1/projects/{pid}/sessions/{sid}/chat`,返回 SSE 流(`event:` 行 + `data:` JSON 行)。

## 结果 — 确认的 API 契约

**ChatInput 请求体**:`{message: str, deep_thinking: bool, confirmed_tasks: list[dict]|None}`。

**分叉点**:首发 `{message: 原题, deep_thinking: true}` 后,若平台判定任务复杂,流里会出现 `event: decomposition`,其 `data.tasks` 是拆好的子任务列表 —— **这就是浏览器里弹出的「分阶段/不分阶段」选择框**。后端路由据此二选一:

- **分阶段(并行)**:回发 `{message:"", confirmed_tasks: <那批 tasks>}` → 路由判定 `confirmed_tasks is not None` → 走 `run_parallel_agents`(多个子 agent 并行 + 末尾 synthesizer 汇总)。
- **不分阶段(单步)**:不回 confirmed_tasks,正常发 `{message, deep_thinking:false}` → 走 `run_agent_with_tasks`(单 agent 顺序执行)。本实验在题尾附 `[请作为单一顺序步骤执行，不要拆分并行子任务]` 以稳定压制拆分。

事件词表两模式不同:单步用顶层 `tool_call/tool_result/message`;并行用 `agent_start/agent_tool_call/agent_message/agent_end` + 末尾 `synthesis_start`。

## 结论

API 路径成立,分叉可控,无需浏览器。但有一个关键发现影响所有后续实验:

**是否触发分解是随机的。** 同一题不同次发送,有时回 `decomposition`、有时直接答完。初始发送 12 次里仅 8 次触发;AD-P1 两次都没触发。因此「必须先触发选择」这个前提不是每次都满足,实验时需重试到触发为止。能触发 `decomposition` 的是「whole-question 多阶段 pipeline」题(见 `../questions.md`),原子题/简单比较题平台会直接答、不分叉。

运行器固化在 `../runner.py`。
