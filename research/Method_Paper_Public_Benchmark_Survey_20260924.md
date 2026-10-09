# 方法论论文如何选择公开 benchmark：Agent、工具调用与在线路由调研

日期：2026-09-24
研究对象：动态工具适应、Jev/模型路由、工具调用可靠性、Agent 复核与拒答。
本报告修正上一轮偏差：当前论文按**方法论论文**组织，公开 benchmark 是主证据；PTRB-v0/BBB 只作为领域应用或补充案例，暂不作为唯一主 benchmark。

## 结论

### 1. 方法论文通常复用公开 benchmark

近期方法论文大致采用三种模式：

| 模式 | 具体做法 | 代表工作 | 对我们的意义 |
|---|---|---|---|
| A. 公开 routing panel | 任务、候选模型输出、分数和成本已整理好，方法做 offline replay 或 selected-arm replay | RouterBench、PILOT、BaRP、Cascade Routing、LLMRouterBench | 最适合检验动态 Jev 和在线选择算法，复现成本最低 |
| B. 公开任务 + 自行生成输出 panel | 任务和 evaluator 公开，作者调用候选模型/工具生成输出、分数和成本，再做 replay 或在线生成 | FrugalGPT、PAK-UCB、NeMoS | 最适合加入 provider 到达、漂移和延迟反馈，但必须公开模型版本、成本、输出和 evaluator |
| C. 全新环境/evaluator | 同时创造任务、工具、状态和判分器，把 benchmark 本身作为贡献 | τ-bench、ToolSandbox、AgentDojo、AppWorld | 这是 benchmark 论文的路线，普通方法论文不必承担这项工作 |

因此，答案不是“所有 benchmark 都公开”或“必须自己造 benchmark”：**公开任务是主流，候选模型输出 panel 可以由作者补采；只有当环境和 evaluator 本身是贡献时，才需要自建 benchmark。**

### 2. 我们不应把 PTRB-v0 作为唯一主 benchmark

Task36/BBB 仍然重要，但它是项目领域回放资产，当前不能替代公开 benchmark。若论文只在 BBB 上展示动态 Jev 有效，审稿人会担心：

- 方法是否只适配医药 provider 的特殊分布；
- 结果是否依赖不可公开的工具输出；
- 任务、模型、真值和反馈是否可被外部复核；
- 与已有 routing/bandit 方法的比较是否公平。

更稳的结构是：

1. **公开 routing panel 做主方法验证**；
2. **公开、可执行任务做第二个泛化验证**；
3. **BBB/Task36 做真实医药应用案例**；
4. 只公开我们新增的 delayed selected-only replay、arrival/drift protocol 和适配胶水，不把它包装成新的社区 benchmark。

## 方法论文的实际 benchmark 先例

| 方法论文 | 方法问题 | 实际 benchmark / 数据 | 公开性 | 评估方式 | 可借鉴点 |
|---|---|---|---|---|---|
| [ReAct](https://arxiv.org/abs/2210.03629) | 推理与行动交替 | HotpotQA、FEVER、ALFWorld、WebShop | 任务和环境公开；作者重建工具 wrapper | EM/accuracy、环境 success rate | 多种公开任务验证同一 Agent 原语，不依赖自造 benchmark |
| [Reflexion](https://arxiv.org/abs/2303.11366) / [代码](https://github.com/noahshinn/reflexion) | 语言反馈驱动的跨轮改进 | ALFWorld、HotPotQA、HumanEval、MBPP、LeetCodeHardGym | 任务公开；实现、prompt 和部分新环境公开 | success、EM、pass@1、unit test | 用学习曲线和失败反馈证明“在线改进”，不是只报终局分数 |
| [CRITIC](https://arxiv.org/abs/2305.11738) | 工具交互式验证和自我修正 | AmbigNQ、TriviaQA、HotpotQA，各抽取验证集样本 | 原始 QA 数据和工具接口公开；作者实现验证循环 | answer F1/accuracy、修正增益、工具反馈消融 | 与我们的“先有答案、再请求工具验证、再作答/拒答”最接近的 Agent 方法先例 |
| [ToolRL](https://arxiv.org/abs/2504.13958) | 工具调用奖励和 RL | BFCL-v3、API-Bank、Bamboogle | 任务/评估器公开，工具调用模型和训练轨迹由作者生成 | tool name/parameter correctness、任务 reward、泛化 | BFCL/API-Bank 是标准工具调用 sanity，不足以单独证明语义证据可靠 |
| [ToolLLM](https://arxiv.org/abs/2307.16789) / [代码](https://github.com/OpenBMB/ToolBench) | API 检索、规划和多工具调用 | 自建 ToolBench；用公开 APIBench 做 OOD | benchmark 数据和代码公开，但 RapidAPI/API 版本有复现依赖 | ToolEval pass/win、NDCG、AST accuracy、hallucination | 可以自建训练集，但必须再用外部公开数据做泛化；自建集不能是唯一证据 |
| [RouteLLM](https://arxiv.org/abs/2406.18665) / [代码](https://github.com/lm-sys/RouteLLM) | 偏好数据下的双模型路由 | Chatbot Arena、MMLU、MT-Bench、GSM8K | 公开任务和 Arena 数据；Djudge 是作者补充的模型输出/LLM judge 数据 | cost-performance、CPT、APGR、OOD model pair | 公开测试 benchmark + 自建训练/偏好日志是可接受组合 |
| [PILOT](https://aclanthology.org/2025.findings-emnlp.1301/) | 带预算的 selected-arm contextual bandit | RouterBench、MMLU、Chatbot Arena 子集 | RouterBench 任务、模型分数/成本和代码公开 | reward、regret、performance-cost curve | 与我们最接近的在线路由先例；明确区分全臂离线日志和 selected-arm 学习权限 |
| [BaRP](https://arxiv.org/abs/2510.07429) | bandit feedback 下的一策略多权衡路由 | RouterBench + GSM8K、MMLU、ARC-C、WinoGrande、NQ、MBPP、HellaSwag、HotpotQA | 公开任务和 replay 数据；方法论文承诺公开代码 | regret、OOD、cost-quality | 公开 routing panel + 未见任务 OOD，是动态方法的直接实验模板 |
| [Cascade Routing](https://proceedings.mlr.press/v267/dekoninck25a.html) / [代码](https://github.com/eth-sri/cascade-routing) | 多级模型级联与质量成本权衡 | RouterBench；SWE-Bench、Minerva Math、LiveCodeBench、ARC-C、MMLU-Pro、MixEval、GSM8K | 公开任务；部分模型输出需重新采集 | quality-cost AUC、cascade/threshold/oracle | 一个 routing panel 加 2–4 个公开任务族足以支撑方法论文 |
| [FrugalGPT](https://arxiv.org/abs/2305.05176) / [代码](https://github.com/stanford-futuredata/FrugalGPT) | 成本受限级联 | HEADLINES、OVERRULING、CoQA；作者收集 12 个 API 输出 | 原始任务公开，输出 panel 由作者生成并发布部分版本 | accuracy、cost、match-best、quality-cost curve | 公开任务 + 自行采集多模型输出是成熟先例，但必须锁定 API 版本和成本 |
| [PAK-UCB](https://proceedings.mlr.press/v267/hu25m.html) / [代码](https://github.com/yannxiaoyanhu/dgm-online-select) | 在线生成器选择和新模型到达 | BigCodeBench、HumanEval-X、Sudoku、MS-COCO 等公开任务/evaluator | 任务和 evaluator 公开；生成器输出可在线生成或预计算 replay | cumulative regret、best-fixed、arrival ablation | “新工具到达”实验可以不造 benchmark，只需公开 arrival protocol |
| [NeMoS](https://openreview.net/forum?id=CSjewjplO1) / [代码](https://github.com/julesdamidaux/nemos-tmlr) | 近邻 bandit + active feedback 的在线模型选择 | 4 个公开 prompt 数据集、6 个公开图像模型 | MIT 代码；支持预计算 replay 和真实在线生成 | regret、Outscore-to-Best、OPR、model add/remove | 最接近动态模型池；但 active query 常给全臂反馈，不能直接等同 selected-only |

这些先例显示：**方法论文的创新通常在学习协议、反馈权限、预算、漂移或理论，而不在于另造一套任务题。**

## 哪些 benchmark 真正“公开”

“公开”必须拆成四层，不能只看论文有没有 GitHub：

| 层级 | 公开内容 | 例子 | 复现风险 |
|---|---|---|---|
| L1 完整可执行 benchmark | 任务、环境、工具、evaluator、split、代码都公开 | ToolSandbox、τ²-bench、AgentDojo、AppWorld | 环境依赖、API key、版本变化 |
| L2 公开任务 + evaluator | 题目和判分公开，候选模型输出需重新生成 | PAK-UCB 的 BigCodeBench/HumanEval-X/Sudoku、ReAct 任务 | 模型版本、价格和服务端行为变化 |
| L3 公开 routing panel | query、候选模型输出、score、cost 已整理 | RouterBench、LLMRouterBench | panel 版本和授权、分数定义、API 输出时效 |
| L4 公开原始数据但无可复现实验面板 | 数据集公开，方法需要自己重建环境或工具结果 | ToolBench/API-Bank 的部分设置 | evaluator/外部 API 漂移，重建成本高 |

对动态 Jev，L3 和 L2 最有价值。L3 能直接做严格 replay；L2 能检验 arrival、漂移和真实 evaluator。L1 更适合 Agent 端到端 sanity，不适合直接替代 routing 方法 benchmark。

## 路由 benchmark 与 Agent 验证 benchmark 不能混为一类

我们的三个层次需要不同的公开证据：

| 层次 | 公开 benchmark 代表 | 能证明什么 | 不能证明什么 |
|---|---|---|---|
| 动态工具/模型选择 | RouterBench、LLMRouterBench | selected-arm 学习、成本/质量权衡、arrival/drift 适应 | 主 Agent 是否会理解和复核错误证据 |
| 工具交互和验证 | CRITIC 的 AmbigNQ/TriviaQA/HotpotQA；ToolSandbox | 调用工具获取反馈、验证初始答案、状态依赖和 insufficient information | 真实医药 provider 的化学真值 |
| 工具调用格式与参数 | BFCL-v3、API-Bank | tool name、参数和调用链正确性 | 工具返回的语义是否可靠、何时应拒答 |

所以不能用 RouterBench 的 routing reward 代替 Agent 复核指标，也不能用 BFCL 的函数调用准确率代替“正常返回但结果错误”的科学证据问题。

## 我们的 benchmark 选择

### 主 benchmark：RouterBench

优先使用 [RouterBench 官方仓库](https://github.com/withmartian/routerbench) 和 [公开 Hugging Face 数据](https://huggingface.co/datasets/withmartian/routerbench)，原因是：

- PILOT、BaRP、Cascade Routing 都以它为主要 routing 参照；
- query、候选模型结果、score 和 cost 已有统一格式；
- 可以严格模拟 selected-only feedback，而不必为每个 episode 重新调用所有候选模型；
- 有 Random、Best Single、Oracle 和多种 routing 结果可直接对照；
- 数据规模比更大版本更容易审计和复现。

使用方式：

1. 公开 panel 只用于建立任务上下文、oracle 和离线分析；
2. 在线 replay 每轮只把被选 arm 的 reward 放入 feedback queue；
3. feedback 延迟、arrival、drift 和预算由我们预注册；
4. 不把 panel 中未选 arm 的分数泄露给动态 Jev；
5. 另用全臂分数计算 oracle、regret 和事后诊断。

### 次 benchmark：LLMRouterBench 的固定子集

[LLMRouterBench](https://github.com/ynulihao/LLMRouterBench) 提供 33 个模型、21+ 数据集、10 种 routing baseline 和标准化 score/cost 字段，代码为 MIT，数据链接到公开 Hugging Face 数据集。它适合做现代外部复核，但完整 release 较大，包含 API 输出和版本依赖。

因此不把它和 RouterBench 全量同时作为首轮主表。首轮固定一个可审计子集，用来回答：动态 Jev 是否只在旧 RouterBench 上有效。若资源有限，只完成 RouterBench 也比同时堆两个未审计的大 benchmark 更好。

### 第二类公开任务：PAK-UCB 风格的确定性 evaluator

从 PAK-UCB 使用的公开任务中选择 2–3 个：

- BigCodeBench 或 HumanEval-X：代码执行结果可程序化判分；
- Sudoku：确定性约束满足；
- 可选 MS-COCO：若要测试多模态/生成器模型到达。

首轮建议只选代码和 Sudoku，避免为多模态环境增加不必要的工程变量。这里不复制 PAK-UCB 的算法，只复用“公开任务 + 候选模型输出 + model arrival + evaluator”的实验范式。

### Agent 验证主 sanity：CRITIC 公开任务；ToolSandbox 或 τ²-bench 作交互补充

**CRITIC 的公开任务应排在 Agent 主 sanity 的前面。**它直接从初始回答出发，调用外部工具得到验证反馈，再决定是否修正；使用的 AmbigNQ、TriviaQA、HotpotQA 都是公开数据，判分可以使用答案 F1/accuracy。我们可以保持任务和答案公开，只把“验证工具返回是否正确、是否继续调用、是否拒答”作为新增动作协议。

这比直接把一个 stateful tool benchmark 当作主 Agent 证据更贴近当前故事。

### Agent integration：ToolSandbox 或 τ²-bench，二选一

- [ToolSandbox](https://github.com/apple/ToolSandbox) 更适合我们的复核/拒答问题：state dependency、canonicalization、insufficient information、multiple tool calls 和 milestone evaluator 都与“看到结果后是否继续验证”相关。
- [τ²-bench](https://github.com/sierra-research/tau2-bench) 更适合多轮用户-Agent 双控制和组合任务，代码、任务和 assertion evaluator 公开。

它们只承担 Agent 行为 sanity check，不承担动态 Jev 的主算法排名。首选 ToolSandbox；如果其 RapidAPI/模型依赖无法锁定，则改用 τ²-bench 的固定 text split。

### BBB/Task36 的定位

BBB 保留为真实医药应用案例：

- 验证 provider 结果可能冲突、延迟反馈和领域真值下的方法行为；
- 说明动态 Jev 对真实科学工具是否有用；
- 不把 BBB 称为公开社区 benchmark；
- 若工具输出或真值不能完整公开，主表只能作为 application case study，不能承担唯一的可复现方法证据。

## 主 benchmark 与 baseline 的最小组合

### RouterBench 主表

1. Random；
2. Best Single / Fixed Incumbent；
3. Oracle；
4. LinUCB；
5. Linear Thompson Sampling；
6. 一个公开强近邻：PILOT 或 BaRP；
7. 我们的 dynamic Jev；
8. no-update、no-prior、no-drift 和 no-delay 消融。

### 公开任务泛化表

在 BigCodeBench/HumanEval-X/Sudoku 的固定公开 split 上，报告：

- Random、Best Fixed、LinUCB/TS、PAK-UCB 风格 arrival baseline、dynamic Jev；
- cumulative regret、best-fixed gap、model-arrival recovery time、cost/latency；
- 不同模型池、新模型加入和 prompt distribution drift。

### Agent 验证表

在 CRITIC 的公开 QA 任务上，报告：

- initial answer accuracy；
- verification-trigger precision/recall；
- corrected answer accuracy；
- unsafe answer rate；
- abstention coverage-risk；
- external tool calls/episode 和 latency。

在 ToolSandbox 的固定子集或 τ²-bench 的固定 split 上，作为交互补充报告：

- base Agent；
- base + static module；
- base + dynamic module；
- never-verify、always-verify、always-abstain；
- task success、verification precision/recall、unsafe action、coverage-risk、calls/episode。

## 最终决策建议

当前不做“从零构造一个新的 Agent benchmark”这条路线。执行顺序改为：

1. 先把 RouterBench adapter 和 selected-only delayed replay 跑通；
2. 复现 Random/Best Single/Oracle/LinUCB/TS，并接一个 PILOT 或 BaRP 强近邻；
3. 在 PAK-UCB 风格公开任务上做 model arrival 和 drift 复核；
4. 用 ToolSandbox 的小固定子集验证主 Agent 的复核/拒答接口；
5. 用 CRITIC 公开任务验证 Agent 的复核/拒答，再以 ToolSandbox 或 τ²-bench 做交互补充；
6. 最后把 BBB/Task36 作为真实医药 case study；
7. 只有当公开 benchmark 上的方法成立，才讨论是否把 PTRB-v0 单独发展成后续 benchmark 论文。

这条路线直接回答了“别人是否都自己造 benchmark”：**不是。方法论文大多复用公开 benchmark，自己补采模型输出和反馈协议；我们也应采用同样的公开 benchmark 优先路线。**

## 来源与代码

- RouterBench：论文 https://arxiv.org/abs/2403.12031；代码 https://github.com/withmartian/routerbench；数据 https://huggingface.co/datasets/withmartian/routerbench
- LLMRouterBench：论文 https://arxiv.org/abs/2601.07206；代码 https://github.com/ynulihao/LLMRouterBench；数据 https://huggingface.co/datasets/NPULH/LLMRouterBench
- PILOT：论文 https://aclanthology.org/2025.findings-emnlp.1301/
- BaRP：论文 https://arxiv.org/abs/2510.07429
- Cascade Routing：论文 https://proceedings.mlr.press/v267/dekoninck25a.html；代码 https://github.com/eth-sri/cascade-routing
- PAK-UCB：论文 https://proceedings.mlr.press/v267/hu25m.html；代码 https://github.com/yannxiaoyanhu/dgm-online-select
- NeMoS：论文 https://openreview.net/forum?id=CSjewjplO1；代码 https://github.com/julesdamidaux/nemos-tmlr
- ToolSandbox：论文 https://arxiv.org/abs/2408.04682；代码 https://github.com/apple/ToolSandbox
- τ²-bench：论文 https://arxiv.org/abs/2506.07982；代码 https://github.com/sierra-research/tau2-bench
- AgentDojo：论文 https://arxiv.org/abs/2406.13352；代码 https://github.com/ethz-spylab/agentdojo
- FrugalGPT：论文 https://arxiv.org/abs/2305.05176；代码 https://github.com/stanford-futuredata/FrugalGPT
- Reflexion：论文 https://arxiv.org/abs/2303.11366；代码 https://github.com/noahshinn/reflexion
- CRITIC：论文 https://arxiv.org/abs/2305.11738
- ToolRL：论文 https://arxiv.org/abs/2504.13958
- ToolLLM/ToolBench：论文 https://arxiv.org/abs/2307.16789；代码 https://github.com/OpenBMB/ToolBench
