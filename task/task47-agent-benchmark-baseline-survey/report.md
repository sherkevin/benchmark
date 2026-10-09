# Task47 报告摘要

本轮调研覆盖 AgentBench、τ-bench、τ²-bench、ToolSandbox、AgentDojo、AppWorld、TravelPlanner、OSWorld、MedAgentBench、MINT、AgentBoard、LLMRouterBench、ToolBench-X、ToolLLM、Reflexion、Voyager、PILOT 和 BaRP。

## 判断

高质量 Agent benchmark 论文的共同结构是：受控可执行环境、独立或程序化 evaluator、任务/难度/泄漏控制、强 baseline 矩阵、过程与终局诊断、公开复现材料。方法论文若复用公共 benchmark，则必须在同信息和同预算下比较固定最强、经典方法、oracle 和强近邻，并做消融、漂移或 unseen 泛化。

PTRB-v0 目前是基于 Task36 真实医药回放资产拟构建的领域 benchmark 草案，不是已有社区标准。B0–B8 是待实现的候选 baseline 矩阵，不是已完成结果。最近直接相邻工作 ToolBench-X 已经覆盖可恢复的工具环境不可靠性，因此我们需要把差异压缩到真实医药 provider、延迟 selected-only feedback、动态 Jev 在线更新和主 Agent 复核/拒答。

## 推荐主实验

四个 slice：S1 工具选择、S2 结果复核、S3 作答/拒答、S4 在线适应。主指标同时报告 joint success、final error、unsafe answer、verification F1、coverage-risk、pass^k、累计 regret/utility、调用成本和延迟。主 baseline 包括 random、fixed incumbent、static Laya/AnyJev、full-information oracle、LinUCB、Linear TS、AnyJev observe、dynamic Jev + posterior、base Agent/static Jev/dynamic module，以及 always/never/random verify 和 always-abstain 边界。

## 复现与边界

ToolBench-X 目前是 2026 arXiv 预印本；其数据许可证、任务/工具复用条件需要在外部实验前另行核验。MedAgentBench 是医疗领域 benchmark 的可复用发表范式，但它的 FHIR/EHR 任务不能替代 PTRB 的医药预测工具真值。
