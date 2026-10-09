# Task48：方法论文公开 benchmark 选型

## 目标

以方法论论文为前提，调研 Agent、工具调用、在线路由和动态模型选择工作实际复用的 benchmark，确认公开性、复现条件和可迁移性，冻结我们的公开 benchmark 组合。

## 研究问题

1. 方法论文通常复用公开 benchmark，还是自建 benchmark？
2. 公开 benchmark 的“公开”具体覆盖任务、evaluator、模型输出、成本和环境中的哪些层？
3. 哪些 benchmark 最适合动态 Jev、selected-only delayed feedback 和 Agent 复核/拒答？
4. BBB/Task36 应作为主 benchmark、公开 benchmark 的补充，还是应用案例？

## 预期产物

- `research/Method_Paper_Public_Benchmark_Survey_20260924.md`
- 公开性分层和方法论文先例表
- RouterBench、LLMRouterBench、PAK-UCB 任务、ToolSandbox/τ²-bench 的选型建议
- 与现有 PTRB-v0/BBB 定位的修正

## 边界

本任务只做文献、代码、数据公开性和 benchmark 选型审计，不启动训练、新预测、真实 fit/update 或 Agent 科学批次。
