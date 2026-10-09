# Task48 报告摘要

方法论论文通常复用公开 benchmark，或复用公开任务并自行生成候选模型的输出/分数/成本 panel；只有 benchmark/evaluator 本身是贡献时，才走全新环境路线。PILOT、BaRP、Cascade Routing 直接使用 RouterBench；PAK-UCB、NeMoS 使用公开任务和 evaluator 并定义 model arrival/replay；FrugalGPT 使用公开任务加作者采集的 API 输出。

推荐组合：RouterBench 作为主 routing panel；LLMRouterBench 固定子集作为现代外部复核；BigCodeBench/HumanEval-X/Sudoku 作为确定性 evaluator 和 model-arrival 泛化；CRITIC 使用的 AmbigNQ/TriviaQA/HotpotQA 作为 Agent 复核/拒答主 sanity，ToolSandbox 或 τ²-bench 二选一做交互补充；BBB/Task36 作为真实医药应用案例，不作为唯一公开 benchmark。

PTRB-v0 暂不承担主 benchmark 贡献。我们只新增 selected-only delayed replay、arrival/drift protocol、动态 Jev 适配和医药 evaluator 胶水。
