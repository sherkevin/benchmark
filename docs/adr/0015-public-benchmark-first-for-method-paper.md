# ADR-0015：方法论论文优先复用公开 benchmark

日期：2026-09-24

状态：Superseded by ADR-0021（2026-09-26）。本文件保留当时公开 benchmark 优先路线的调研历史，不再作为当前主 benchmark 决策。

## 背景

当前论文的中心贡献是动态工具适应、实时学习的动态 Jev 和主 Agent 的复核/拒答能力，属于方法论论文。上一轮把 PTRB-v0 作为可能的主 benchmark，混淆了 benchmark 论文与方法论文的发表路径。

调研 PILOT、BaRP、RouteLLM、Cascade Routing、FrugalGPT、PAK-UCB 和 NeMoS 后发现：方法论文普遍复用公开任务或公开 routing panel；作者自行生成候选模型输出、成本和 replay 日志是允许的。τ-bench、ToolSandbox、AgentDojo 和 AppWorld 这类全新环境主要把 benchmark/evaluator 本身作为贡献。

## 决定

1. 主论文优先使用公开 benchmark，不从零构造新的 Agent 社区 benchmark。
2. **RouterBench** 作为首个主 routing panel；**LLMRouterBench 固定子集**作为现代外部复核候选。
3. 使用 PAK-UCB 风格的公开任务/evaluator（优先 BigCodeBench、HumanEval-X、Sudoku）验证 model arrival、drift 和动态选择泛化。
4. 使用 CRITIC 的公开任务（AmbigNQ、TriviaQA、HotpotQA）作为 Agent 复核/拒答主 sanity；ToolSandbox 或 τ²-bench 二选一做 stateful interaction 补充，不把它们的总分与 routing regret 合并成一个排行榜。BFCL/API-Bank 只作工具调用格式与参数 sanity。
5. BBB/Task36 保留为真实医药应用案例。PTRB-v0 只有在未来完成独立 evaluator、公开 split 和发布审计后，才可作为单独 benchmark 贡献。
6. 我们新增的是 selected-only delayed feedback、arrival/drift protocol、动态 Jev 适配和医药 evaluator 胶水，不把这些包装成新的 benchmark 数据集。

## 影响

- 论文可与 PILOT、BaRP、Cascade Routing 在相同公开 routing panel 上直接比较。
- BBB 的不可公开部分不会阻断方法论文的公共可复现性，但 BBB 结果必须标注为 application case study。
- 需要先完成 RouterBench adapter、信息权限测试和固定 split，再申请真实医药批次。
- 如果公开 benchmark 上方法不成立，应收窄贡献，不用 BBB 单点结果补救。

## 相关文件

- [方法论文公开 benchmark 调研](../../research/Method_Paper_Public_Benchmark_Survey_20260924.md)
- [Task48 报告](../../task/task48-method-public-benchmark-selection/report.md)
- [ADR-0014：benchmark 来源与外部角色](0014-benchmark-provenance-and-external-role.md)
