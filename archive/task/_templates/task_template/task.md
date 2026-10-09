# Task {{N}} — {{1 句话任务名}}

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | TASK-{{N}} / `task{{N}}/` |
| **类型** | {{Investigation / Implementation / Research / Decision / Experiment}} |
| **状态** | 🟢 Drafting goal / 🟡 Running / ✅ Done(完成后同步到 `report.md`)|
| **负责人** | {{name}} |
| **预计起止** | {{YYYY-MM-DD}} ~ {{YYYY-MM-DD}}({{N 天}}) |
| **上游来源** | {{PRD §X / parent block 路径}} |
| **下游消费方** | {{(可选)预计消费本 task 产出的 block 或后续 task}} |

---

## 一句话目标

> {{1 句话:要做什么 + 用来回答什么。例如:"调研 X 数据集能否改造成我们的 Y benchmark,产出可改造性评估报告。"}}

---

## 1. 背景与由来

{{1–2 段:为什么做这个 task?上游 PRD/block 的哪条需求驱动?为什么是现在做?}}

> *写作提示:能 1 句说清就别 1 段。指向上游文件即可,不复述其内容。*

---

## 2. 目标(GOAL)

> {{Specific, testable. 强制以这一句结尾:**"本 task 完成的标准 = ..."**(必须可被 `report.md` 的 goal 对照清单逐条勾验)}}

### 2.1 核心问题

{{1–3 个具体问题,task 完成时应能给出明确回答}}

### 2.2 子问题(可选)

1. {{...}}
2. {{...}}

---

## 3. 范围与非目标

- **范围内**:{{核心动作}}
- **非目标**:{{明确不做的;避免后期 scope creep 争议}}

---

## 4. 预期交付物

| 交付物 | 路径 | 性质 |
|---|---|---|
| 任务收尾 | `task{{N}}/result.md` | 必出 |
| 实验记录 | `task{{N}}/experiment/*.md` | 一次实验一篇(lite task 可省) |
| (可选)对外汇报 | `task{{N}}/block.md` | 仅当本 task 需单独上钉钉时 |
| {{产出 X}} | `task{{N}}/...` | {{文档/代码/数据}} |

---

## 5.(可选)停止准则 / 高置信判定

> 仅 Investigation / Research / Experiment 类 task 需要;Implementation / Decision 类跳过。

满足任一条件即停止:

1. {{条件 1 —— 通常基于覆盖度 + 一致性 + 机制性解释}}
2. {{条件 2 —— 出现明确可复现的失败模式,足以定性区分}}
3. {{条件 3 —— 时间硬约束:N 天后无论结论是否稳定都收尾,残余转下个 task}}

---

## 6. 约束与约定

- 调研报告平铺进 `research/`（一次调研的报告与中间材料并成一个文件夹）;本任务产出在 `task/task{{N}}/`。
- {{其他 task 特有的约定,例如:不臆造工具数值;真实数据需标注 ▣ vs 作者撰写 ▽;…}}

---

## 7.(可选)风险预判

| 风险 | 缓解 |
|---|---|
| {{...}} | {{...}} |

> *写作提示:能想到的 blocker 提前写;实际遇到的写到 `report.md` §4。*

---

## 已填好的示例(参考)

- Investigation:[`task2/goal.md`](../../../task2/task.md)
- Implementation:[`task3/goal.md`](../../../task3/task.md)
- Research:[`task4/goal.md`](../../../task4/task.md)
- Experiment:[`task1/goal.md`](../../../task1/task.md)

---

## 【固定条款】若本 task 涉及调用 LLM

**每次调用必须留完整 JSON log**（owner 要求，2026-08-07）。必留字段清单见 [`ACTION_RULES.md` §7](../../../ACTION_RULES.md)：
`request` 完整原样（model/temperature/max_tokens/messages/tools）+ `response` 完整原样（含 `usage`、`finish_reason`、完整 `message` 对象）+ `latency_ms`/`attempt`/`seq`/`timestamp`/`code_version`。

三条硬要求：① 原始响应原样保留，不得只存解析结果——解析逻辑会改、原始数据不会 ② 失败与重试也要落盘 ③ 落盘在自己 task 目录下，**不写进 `dag_poc/traces/`**（PR-35：该目录会被外部进程 git 还原，交付物会丢）。

temperature 统一调 `agent_harness.temperature_for(model)`——kimi-k3 传 0 返 HTTP 400 MPE-001（实测）。

