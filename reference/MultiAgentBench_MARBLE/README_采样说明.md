# MultiAgentBench (MARBLE) — 采样说明

## 论文与来源
- 论文：*MultiAgentBench: Evaluating the Collaboration and Competition of LLM Agents*（arXiv:2503.01935，ACL 2025）
- 代码/数据仓库：https://github.com/ulab-uiuc/MARBLE（MARBLE = 配套多智能体协同引擎）
- 本地采样下载通道：GitHub Contents API（gh CLI 认证），2026-07 抓取

## Benchmark 是什么
**面向多智能体系统（MAS）的协作/竞争评测**，覆盖 6 类场景：research（科研提案协作）、coding（软件开发协作）、database、minecraft、bargaining（谈判）、werewolf（狼人杀）。支持 star/tree/graph/chain 多种协同拓扑与不同协调协议的对比。

## checkpoint 机制（milestone-based KPI）
论文的核心评测创新是 **milestone（里程碑）驱动的 KPI**：
- 为每个任务定义一组里程碑（如 coding 场景："完成需求分析"→"接口设计"→"核心模块实现"→"联调通过"）；
- **Task Score**：里程碑达成情况（LLM judge + 规则混合判定）；
- **Coordination Score**：通信质量与规划质量（milestone 对齐度）；
- 论文报告 milestone 达成率能显著区分不同协同拓扑（graph 拓扑在 research 场景最优）。
这是"**多智能体 + 里程碑卡点**"最直接的公开范式，与用户定义的"多智能体长任务中的中间卡点"高度对口。

## 采样内容
- `research_sample_3cases.jsonl`：3 条 research 协作任务完整配置（`agents`（各自 profile）、`coordinate_mode`（协同拓扑）、`relationships`、`metrics`（评价维度）、`task`（协作任务书）、`output` 格式约束）。
- `coding_sample_2cases.jsonl`：2 条 coding 协作任务配置。
- `REPO_README.md`：官方说明。

## 对我们的参考价值
唯一同时具备"多智能体 + milestone 中间卡点 + 协同质量度量"三要素的公开 benchmark；其 milestone/KPI 定义方式（场景化任务书内嵌评价维度）可直接借鉴，但其判定依赖 LLM judge，客观性弱于 TheAgentCompany 的程序化 checkpoint。
