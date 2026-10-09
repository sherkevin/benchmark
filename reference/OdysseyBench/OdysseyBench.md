# OdysseyBench 怎么测（目标维持 / 长期记忆）

**一句话：把完成一个任务所需的信息打散、埋在跨多天的对话历史里，让 agent 必须从长期上下文中回忆并整合关键信息，才能完成当前的办公操作。**

它考察长程的另一维度——不是步数多，而是"**时间跨度 + 上下文碎片化**"下能不能不遗忘、能回忆。

## 一个真实 case（task_1-1 的一个子任务，见 `case_示例.json`）

- **用户**：Bob，日期 2020-05-01。
- **子任务**：`Add a meeting to Bob's calendar at 5/17/2024 10:30–11:00 a.m`。
- **触发句** `query_sentence`：`Add the meeting to Bob's calendar.`
- 会议的具体细节埋在多日 `chat histories` 里，需回忆后才能填对。

## 判分机制（怎么算对）

- **分解式评价**：每个长程任务拆成若干 `subtasks`，每个 subtask 带独立 `evaluation` 字段（期望的文件状态）。
- 本例 `evaluation = evaluate_contain`：检查生成的 `./calendar/Bob.ics` 是否包含 `DTSTART:20240517T103000` / `Meeting` / `DTEND:20240517T110000`。
- 子任务即中间卡点，全 hard（文件状态比对），强调"**信息是否被正确回忆并使用**"。

## 属于哪种考法

**终态文件状态比对（分子任务）**：卡点 = 每个子任务的期望文件状态；长程性来自跨多日的上下文碎片化，考的是长期记忆与信息整合——目标维持里"不遗忘"的一面。

> 完整数据：[`reference/OdysseyBench/`](./)（`task_1-1/subtasks_plus/*.json` 子任务、`chat_histories_plus/` 多日会话）
