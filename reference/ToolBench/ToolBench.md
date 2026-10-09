# ToolBench (ToolLLM) 怎么测（用对工具）

**一句话：给 agent 一个任务和一大堆候选 API，让它在一个任务里连续选、调、串多个真实工具去解决；再用 Pass Rate（完成没）+ Win Rate（比参考答案谁更好）打分。**

这是本类里最"长程"的工具调用评测——一个任务要连着调好几个 API。

## 一个真实 case（G2 同类多工具，见 [`reference/ToolBench/case_示例.json`](./case_示例.json)）

- **任务**：我在做电商，需要集成包裹跟踪——给我 shipment ID `6045e2f44e1b233199a5e77a` 的跟踪数据；另外帮我查 SQUAKE 认证系统的健康状态。
- **候选工具（api_list）**：Transportistas de Argentina、Create Container Tracking、Turkey Postal Codes、SQUAKE……（每个都带参数与说明）。
- agent 要自己判断：用哪几个（这里≈ Create Container Tracking + SQUAKE）、参数怎么填、按什么顺序调、把结果串起来回答。

## 判分机制（怎么算对）

ToolEval（仿 AlpacaEval 的机器评审）两个指标：

- **Pass Rate**：在有限 API 调用次数内**完成任务的比例**。
- **Win Rate**：把 agent 的解法与参考答案（ChatGPT-ReACT）成对比较，由 ChatGPT 按预设标准判谁更好，胜出比例即 Win Rate。
- **注意**：判定用的是 **LLM 评审（ChatGPT）**，不是可执行 hard 断言——这是它和本类另外两个（SUPER 正则、τ-bench 数据库终态）的关键区别。好处是能直接评"工具选调得好不好"，代价是判据非硬。

## 属于哪种考法

**专门的工具调用能力评测**：给候选工具集，直接考"该不该调、选哪个、参数对没、多步怎么串"；G2/G3 的长链多工具最贴长程。判分是 LLM 评审（软），若要接入 hard 体系可换成 BFCL 式的 AST / 执行匹配。

> 完整数据：[`reference/ToolBench/`](./)（`G1/G2/G3_query.json` 三档 指令、`example_answer_G2_10.json` 真实 DFSDT 解法树）｜ 论文 arXiv:2307.16789（ICLR 2024）
