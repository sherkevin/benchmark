# AgentBoard 怎么测（反馈应变）

**一句话：让 agent 在环境里多轮行动，用正则匹配"环境每一轮返回的观测文本"，看它达成了几个子目标——即使没完成整体任务，也能给出过程分。**

## 一个真实 case（ALFWorld 具身任务）

- **任务目标**：`look at bowl under the desklamp.`（把碗拿到台灯下看）
- **子目标（3 条正则，逐步递进）**：
  - Subgoal 1（看见碗）：`^(?=.* you see)(?=.*a bowl \d+)`
  - Subgoal 2（捡起碗）：`You pick up the bowl \d+`
  - Subgoal 3（找到台灯）：`^(?=.* you see)(?=.*a desklamp)`

（完整 3 条 case 见 [`reference/AgentBoard/case_示例.jsonl`](./case_示例.jsonl)）

## 判分机制（怎么算对）

- **Progress Rate（进展率）**：每匹配中一个子目标 +1/n，取 agent 达成的**最大子目标比例**；正则匹配的是环境客观返回的观测文本，非 agent 自述。
- 覆盖 **9 个环境**（具身 AlfWorld/ScienceWorld/BabyAI、游戏 Jericho/PDDL、网页 WebShop/WebArena、工具 Tool-Query/Tool-Operation）。
- 子目标模板按"任务族"复用（如 134 个 alfworld 任务仅约 6 种骨架），非逐条人工。

## 属于哪种考法

**subgoal 正则匹配环境反馈 → 进展率**：把"有没有在朝目标推进"量化成达成子目标的比例，是过程量评测的代表指标。

> 完整数据：[`reference/AgentBoard/`](./)（`alfworld/`、`pddl/`、`tool-query/` 的 `test.jsonl`）
