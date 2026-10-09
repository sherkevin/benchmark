# Agent 论文中的 benchmark 与 baseline：调研、审计与 PTRB-v0 方案

日期：2026-09-24
范围：工具调用、交互 Agent、动态路由、可靠性/拒答，以及医疗 Agent benchmark。
证据原则：优先使用论文原文、官方项目页和官方代码；最新工作若只有 arXiv，明确标为预印本。

**选型更正**：本项目当前是方法论论文。后续 benchmark 组织以 [Task48 方法论文公开 benchmark 调研](Method_Paper_Public_Benchmark_Survey_20260924.md) 和 [ADR-0015](../docs/adr/0015-public-benchmark-first-for-method-paper.md) 为准：公开 RouterBench/公开任务是主证据，BBB/PTRB-v0 是应用案例或后续 benchmark 候选，不作为唯一主 benchmark。

## 结论先行

我们现在**有一个领域 benchmark 的候选设计和真实回放资产**，还没有一个可以在论文中直接称为“公开 benchmark”的完成品。

- Task36 提供真实的医药工具结果、具体模型版本、BBB 回放和信息隔离接口。它是数据与环境资产，不等于 benchmark。
- PTRB-v0 已经写出 episode、T0–T3 任务、数据切分、延迟反馈和评分草案，因此可以称为“本项目拟构建的领域 benchmark”或“project-defined domain benchmark”。它还没有完成公开测试集、独立 evaluator、外部复核和正式结果。
- B0–B8 是候选 baseline 矩阵，不是已经运行并冻结的 baseline 结果。当前最缺的是同一信息条件、同一预算和同一日志合同下的可复现实装。
- 论文不能把“正常返回但可能不可靠”写成首次发现。2026 年的 ToolBench-X 已经直接评估 specification drift、invocation error、execution failure、output drift 和 cross-source conflict。我们的可识别增量应放在真实医药 provider 回放、选择性且延迟的反馈、动态 Jev 的在线适应，以及主 Agent 的验证/拒答行为。

因此，当前最稳妥的投稿定位是：**方法论文为主，问题驱动的医药领域 benchmark 为实验载体；只有在 PTRB-v0 完成发布和独立复核后，才把 benchmark 本身列为次要贡献。**

## 1. 别人是怎样把 Agent 工作发表成论文的

Agent 论文大致有两种发表范式。两者都需要 benchmark 和 baseline，但承担的科学任务不同。

### 1.1 Benchmark 论文

这类论文的核心贡献是一个可复用的评测环境，而不是某个模型分数。代表性工作有 AgentBench、τ-bench、ToolSandbox、AgentDojo、AppWorld、TravelPlanner、OSWorld 和 MedAgentBench。它们通常同时交付：

1. 一个现有 benchmark 没有覆盖的明确缺口，例如用户和 Agent 双方都能改变世界状态、工具有隐式状态依赖、工具环境会发生可恢复故障，或医疗任务必须访问结构化病历。
2. 一个可执行环境。工具、初始状态、任务和终止条件都能被程序加载；同一个任务可以重复运行。
3. 一个尽量独立于被测模型的 evaluator。数据库终态、状态断言、milestone、unit test 或约束检查优先于“让另一个 LLM 评判答案”。
4. 一组能证明 benchmark 有区分度的 baseline。至少包括多个模型/Agent、简单策略、标准 Agent 方法和必要的上界或 oracle。
5. 诊断分析，而不只是一个总分。论文要说明失败来自工具选择、参数、状态跟踪、规划、通信、恢复还是最终作答。
6. 可复现材料。代码、任务生成器、版本、运行协议、split、随机种子和测试集保护方式都要能被复核。

τ-bench 的做法很典型：它把领域数据库、API、政策和任务拆成模块，用用户模拟器驱动多轮对话，用最终数据库状态做确定性判分，并提出 `pass^k` 衡量同一任务多次运行的一致性。作者还用多轮模型矩阵、Function Calling/ReAct/Act 对照、成本分析和失败归因证明 benchmark 不是一组静态问答题。

ToolSandbox 把难点进一步拆成 state dependency、canonicalization、insufficient information、multiple tool call 和 multiple user turn；它用 stateful tool、on-policy 用户模拟器和 milestone/minefield DAG 对中间轨迹与终局同时评分。其论文没有只报告“是否成功”，而是报告不同能力切片和工具 schema 扰动下的差异。

AgentDojo 的贡献是可扩展的动态环境，而不是固定攻击列表。它把 97 个 user task、629 个 security case、utility function 和 security function 组合起来，同时报告 benign utility、utility under attack 和 targeted attack success rate。这个结构说明安全/可靠性论文必须同时报告“任务是否完成”和“是否产生危险副作用”。

AppWorld 展示了自建 benchmark 如何达到发表门槛：9 个日常应用、457 个 API、750 个任务；每个任务有 Supervisor、初始状态、任务指令和 state-based unit tests；Test-C 留出至少一个未见应用，防止模型只记住 API；baseline 同时包含 ReAct、Plan-and-Execute、FullCode+Reflexion、并行函数调用、ToolLLaMA 和 CodeAct，并做 oracle API 对照。

MedAgentBench 对我们尤其重要。它不是把通用医疗问答换成一个新名字，而是提供了 300 个临床医生编写的任务、100 个患者、超过 70 万个数据元素和 FHIR 交互环境，并公开实现。它证明医疗领域 benchmark 可以发表，但前提是任务、环境、评价协议和模型矩阵都能独立运行；“有一些医疗数据”本身不构成 benchmark 贡献。

### 1.2 方法论文

方法论文通常复用已有 benchmark，贡献在于新的 Agent、路由器、学习规则或反馈机制。典型结构是：

- 在一个或多个公共 benchmark 上固定任务、工具池、模型版本、提示和预算；
- 用随机/多数、固定最强模型、标准 ReAct/Function Calling、经典算法或公开强方法做 baseline；
- 证明新方法在同一信息条件下改善了任务成功率、成本、可靠性或泛化；
- 用消融拆开每个组件的作用，并在 unseen task、unseen tool、漂移、预算变化或多次重复运行下验证；
- 说明反馈从哪里来、更新何时发生，以及方法是否偷看了未执行动作的结果。

PILOT、BaRP 和 LLMRouterBench 所代表的 routing 工作把“每个 query 选择哪个模型”写成 contextual bandit 或成本质量权衡问题。它们会同时报告 random、best single、offline/full-information 方法、经典 contextual bandit 和 oracle，并使用统一的模型池、数据字段、split 和成本统计。LLMRouterBench 的公开框架有 21 个数据集、33 个模型、10 种 routing baseline，主表明确区分 Random、Best Single、Oracle、准确率和成本 Pareto 指标。这个套路直接告诉我们：如果把动态 Jev 写成方法，必须把“静态最强模型”和“看完整反馈的上界”放进主表，而不是只和随机策略比较。

Reflexion 和 Voyager 则说明“反馈驱动的适应”不能只靠最终分数来证明。它们报告多轮学习曲线、跨任务/新环境泛化、反馈来源消融和长期进度指标。对我们来说，动态 Jev 需要显示从冷启动到稳定阶段的曲线、漂移恢复时间和每次反馈后的参数版本，而不能只在最后一个测试集上报告一次平均准确率。

## 2. 代表性工作如何安排 benchmark、baseline 和 evaluator

| 工作 | 发表形态 | benchmark 由谁提供 | 核心 evaluator/指标 | baseline 组织方式 | 对我们的直接启示 |
| --- | --- | --- | --- | --- | --- |
| [AgentBench](https://arxiv.org/abs/2308.03688) | benchmark | 新建 8 个交互环境并整合 3 个已有环境 | 多环境任务成功、失败类型 | 27 个 API/开源模型，统一协议，错误分析 | 需要跨工具族的聚合协议和失败 taxonomy |
| [τ-bench](https://arxiv.org/abs/2406.12045) | benchmark | 新建 retail/airline 数据库、API、政策和用户模拟器 | 数据库终态、`pass^1`/`pass^k`、成本 | Function Calling、ReAct、Act，多模型矩阵 | 同一任务重复运行，报告可靠性曲线；终态 evaluator 要独立 |
| [τ²-bench](https://arxiv.org/abs/2506.07982) | benchmark | 新建 dual-control telecom 环境和组合任务生成器 | assertion-based task success、single/dual-control 对照 | Default、No-user、Oracle-plan，按任务复杂度分析 | 把 Agent 的取证动作与环境/用户的动作分开归因 |
| [ToolSandbox](https://arxiv.org/abs/2408.04682) / [代码](https://github.com/apple/aiml-research/ToolSandbox) | benchmark | 新建 34 个工具、1032 个 stateful 场景 | milestone/minefield DAG、类别分数、schema 扰动 | 12 个开闭源模型，统一最小提示，ReAct 消融 | 中间证据、状态一致性和 insufficient-information 必须单独计分 |
| [AgentDojo](https://arxiv.org/abs/2406.13352) / [代码](https://github.com/ethz-spylab/agentdojo) | benchmark/安全 | 新建 4 个应用环境、97 user tasks、629 security cases | utility、utility under attack、attack success | 多 Agent、攻击、检测器/防御器，报告 Pareto | 可靠性要同时报告完成率和危险副作用率 |
| [AppWorld](https://arxiv.org/abs/2407.18901) | benchmark | 新建 9 个应用、457 API、750 个任务 | state-based unit tests、task/scenario completion、collateral damage | ReAct、PlanExec、FullCode+Reflexion、IPFunCall、ToolLLaMA、CodeAct | `train/dev/test-normal/test-challenge` 和 unseen-app split 值得直接借用 |
| [TravelPlanner](https://arxiv.org/abs/2402.01622) | benchmark | 新建旅行 sandbox、1225 queries、约 400 万条工具数据 | delivery、commonsense/hard constraint pass、final pass | greedy、ReAct、Reflexion、不同模型/规划策略 | 复杂度轴、约束级指标和规则 evaluator 比单一总分更有解释力 |
| [OSWorld](https://arxiv.org/abs/2404.07972) | benchmark | 新建跨 OS 的真实可执行桌面环境 | execution-based evaluator、人工 sanity check | 多个 VLM/LLM、提示/GUI 方法、人类基线 | 发布初始状态和执行 evaluator，避免只靠 LLM judge |
| [MedAgentBench](https://arxiv.org/abs/2501.14654) / [NEJM AI](https://doi.org/10.1056/AIdbp2500144) | 医疗 benchmark | 新建 FHIR EHR、100 患者、300 临床任务 | 环境状态/任务完成，10 类任务 | 10 个模型，公开运行代码 | 医药 benchmark 要有真实域协议、可运行环境和公开任务 |
| [MINT](https://arxiv.org/abs/2309.10691) | 方法/评测 | 复用既有 code、QA、ALFWorld 数据，构造多轮交互 | 原任务成功率、轮次增益、反馈增益 | 20 个开闭源模型，多轮 tool/feedback 设置 | 反馈方法需区分“解决任务能力”和“提供反馈能力” |
| [AgentBoard](https://arxiv.org/abs/2401.13178) | 统一评测框架 | 复用 embodied/game/web/tool 任务 | success + progress rate | ReAct 等提示和多任务分析 | 终局失败也要报告逐步进度和已完成证据 |
| [LLMRouterBench](https://arxiv.org/abs/2601.07206) / [代码](https://github.com/ynulihao/LLMRouterBench) | routing benchmark/框架 | 21+ 数据集、33 模型、400K+ instances | AvgAcc、Gain、Gap-to-Oracle、成本 Pareto、latency | 10 个 routing 算法，Random/Best Single/Oracle | 统一数据字段、模型池、成本和 oracle 是 routing 论文的最低基线纪律 |
| [ToolBench-X](https://arxiv.org/abs/2606.25819) / [代码](https://github.com/Foreverskyou/ToolBench-X) | 最新直接相邻预印本 | 1106 个可执行任务，7 topics、84 subtopics，5 类可恢复 hazard | clean/exception accuracy、hazard/recovery 分析 | 12 个模型、no-hint、hint、test-time scaling、clean oracle | 我们不能再声称“首次研究不可靠工具”；必须正面比较并突出在线适应与医药真值 |

这张表揭示一个稳定模式：能发表的 benchmark 并不是“题目数量多”，而是**任务状态、反馈、判分、split、baseline 和失败分析形成闭环**。能发表的方法论文也不是“在一个自建数据集上比随机高”，而是**同信息、同预算、强近邻、消融和泛化证据形成闭环**。

## 3. 对我们当前 benchmark 的审计

### 3.1 我们已经有什么

我们有三类可以复用的资产：

1. **真实领域资产**：Task36 的 BBB 具体模型/版本结果、provider replay、错误与互补性分析、信息隔离接口。
2. **问题协议**：PTRB-v0 草案已经定义 episode、T0–T3、延迟反馈、selected-only 约束、propensity/OPE 记录和 JEV/模块/Agent 三层指标。
3. **开源拼装路线**：Laya/AnyJev、Bayesianbandits、River/contextualbandits/MABWiser、tau²/Inspect、JevBenchmarks/OBP 的候选接口已经有来源和许可证快照。

这足以支撑“我们知道要构建什么 benchmark 和 baseline”，也足以开始合同适配；它还不足以支撑“benchmark 已完成”或“动态 Jev 已经有效”。

### 3.2 我们还缺什么

| 发表门槛 | 当前状态 | 关闭条件 |
| --- | --- | --- |
| 公开可复现环境 | Task36 是仓内回放资产，PTRB-v0 仍是草案 | 独立 clone 能加载固定 episode、工具版本和 evaluator |
| 可判定 evaluator | 已有 BBB 真值/回放判分，但尚未形成统一多层 scorer | 同时输出 tool correctness、verification、answer/abstain、side effect 和 cost |
| 任务难度与覆盖 | 有真实分歧和回放样本，尚未按任务模板/证据风险/漂移冻结切片 | 发布 S1–S4 统计，报告每类样本数、难度和最小支持集 |
| 无泄漏 split | 有信息隔离接口，尚未完成公开 train/dev/test 与新 provider/tool split | 分子 scaffold、任务模板、provider/version、时间/漂移至少一项真正留出，并做审计 |
| 动态性 | 有题内/题间更新设计，尚无真实在线曲线 | 预先定义冷启动、反馈到达、漂移和恢复窗口，保存每个 head/posterior 版本 |
| baseline 结果 | B0–B8 主要是候选清单 | 同一 runner、feature、菜单、预算和日志合同下全部可运行 |
| 外部可比性 | 有开源路线，尚未在外部环境跑 transfer/sanity | 至少完成 ToolBench-X/ToolSandbox/τ²-bench 之一的结构化对照，或明确声明不做泛化并解释原因 |
| 发布与独立复核 | 尚未公开 PTRB-v0 和独立 test evaluator | 发布任务生成/固定 test、版本锁、运行脚本、样例日志和 evaluator 复核报告 |

所以，回答“我们有没有自己的 benchmark”：**有自己的 benchmark 设计和数据资产，没有完成可发表的公开 benchmark。**回答“我们有没有 baseline”：**有候选矩阵，没有完成冻结和实证的 baseline 套件。**

## 4. PTRB-v0 应该怎样收紧

PTRB-v0 不应该试图复制通用 Agent benchmark 的全部能力。它的第一原则是：**在真实医药工具会正常返回、但证据可能错误、过时、冲突或不足的情况下，测 Agent 如何选择工具、验证证据、继续调用、作答或拒答，并测这些决策能否用延迟反馈在线改进。**

### 4.1 四个可独立验证的 slice

| Slice | 要回答的问题 | 最小任务形式 | 主指标 |
| --- | --- | --- | --- |
| S1 tool selection | 对当前分子和功能，是否选到合适的具体 provider/version | 候选模型菜单 + 只能看到已选输出 | selected-tool correctness、regret、propensity |
| S2 result verification | 返回格式正常但语义可能错误时，是否触发验证并识别风险 | 首个输出 + 可选第二证据/复核工具 | verification precision/recall、AUROC、ECE、错误拦截率、误报率 |
| S3 abstention/escalation | 证据不足或冲突时，是否安全停止、拒答或转人工 | 输出、风险状态、预算、终止动作 | selective risk@coverage、危险作答率、拒答 precision/recall |
| S4 online adaptation | provider 可靠性、成本或延迟变化后，是否从真实反馈中恢复 | 按时间到达的 episode + 延迟标签 | cumulative utility/regret、cold-start 曲线、drift recovery time、后期 calibration |

每条轨迹至少保存：`episode_id`、分子/功能 key、候选菜单及版本、实际调用、propensity、原始工具输出、Jev 建议、Agent 实际动作、验证证据、最终答案/拒答、goal state、side effect、成本、延迟、反馈到达时间和 head/posterior 版本。

### 4.2 外部 benchmark 的角色

- **ToolBench-X** 是最近的直接相邻工作，用来检验我们是否真的超过了静态 hazard/recovery 评测。它是 2026 arXiv 预印本，数据许可还需要人工确认；不应直接把其数据打包进我们的发布物。可以引用其五类 hazard、运行其公开代码，或只借其结构化对照表。
- **ToolSandbox** 用于验证 stateful、intermediate milestone、insufficient-information 和任意轨迹 scorer 的通用接口。
- **τ²-bench** 用于验证多轮用户/Agent 双控制和任务复杂度轴；它不能替代医药真值。
- **LLMRouterBench** 用于校验 routing baseline 和 cost/quality/oracle 报告纪律；它是静态多模型 routing，不能直接回答延迟 selected-only feedback。
- **MedAgentBench** 用来说明医疗 Agent 的公开环境和真实域 benchmark 标准；它评估 FHIR/EHR 任务，不替代医药预测工具可靠性问题。

这些外部 benchmark 不应合并成一个总排行榜。PTRB-v0 的主结论只能由医药真值和医药 evaluator 支撑，外部环境只回答接口迁移或行为 sanity。

## 5. baseline 必须怎么冻结

baseline 要按信息条件分层，而不是把不同问题的算法混在一张表里。

### 5.1 模块层

| 层级 | baseline | 作用 | 信息合同 |
| --- | --- | --- | --- |
| B0 | random / majority | 最低可用线 | 只能使用任务定义 |
| B1 | fixed best / fixed incumbent | 静态部署线 | 只用 train/dev 统计，test 不看真值 |
| B2 | static Laya / static AnyJev | 冷启动 typed decision | 冻结表示/权重，不做在线更新 |
| B3 | full-information supervised head/oracle | 机会上界 | 可看训练期完整标签，只作诊断，不当在线方法 |
| B4 | LinUCB | 标准 contextual bandit | 每步只收到 selected arm 的合法反馈 |
| B5 | Linear Thompson Sampling | Bayesian exploration 对照 | 与 B4 使用完全相同 feature、预算、菜单和 feedback queue |
| B6 | AnyJev `observe` | 最近的动态 typed head | 明确其全量 observation 假设，与 selected-only wrapper 分开报告 |
| B7 | dynamic Jev + posterior | 当前主方法候选 | 预训练表示/冷启动先验 + selected-only 延迟更新 + typed feedback |

### 5.2 Agent 层

至少要有 `base tool-calling/ReAct`、`base + static Jev`、`base + dynamic module` 三个版本，并固定底座模型、提示、最大动作数、调用预算和温度。为了让复核/拒答结论可解释，必须加入规则边界：`never-verify`、`always-verify`、`random-verify`、confidence threshold、`always-abstain`。

### 5.3 公平性约束

1. 在线方法只看实际执行动作的反馈；不能用未调用工具的结果填 0，也不能把完整缓存标签伪装成 bandit feedback。
2. 所有方法共享候选菜单、特征版本、随机种子、调用预算和日志格式。
3. 任何 abstention 结果都同时报告 coverage-risk 曲线和 abstention cost；`always-abstain` 必须在表中作为边界。
4. 动态方法必须与 no-update、no-verifier、no-abstention、no-drift 消融比较，避免把更多调用或更高预算误读成算法收益。
5. 结果至少给 paired bootstrap/置信区间、独立重复和按 slice 的分数；不能只报全体平均。

## 6. 论文主结果应该长什么样

如果 PTRB-v0 进入主论文，主表建议固定为以下列：

| 方法 | joint success | final error | unsafe answer | verification F1 | coverage-risk | pass^k | calls/episode | cost/latency |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |

再配三类图：

1. **在线曲线**：前 10/50/100 个 episode 的 cumulative utility/regret、冷启动到稳定阶段的 calibration。
2. **质量—风险—成本曲线**：coverage-risk 和 API/token cost 的 Pareto frontier，包含 always-verify、never-verify、always-abstain。
3. **漂移恢复图**：provider reliability/cost/latency 变化后，动态 Jev 的恢复时间和静态方法的残余风险。

附录必须给：模型/提示/版本、任务模板和 split、每个 slice 的样本数、每次反馈到达时间、propensity、失败日志、evaluator 单元测试、人工抽检/一致性和 OPE 支持范围。

## 7. 现在应该怎样定位论文

当前不要同时声称“新 benchmark、新 Jev、新 bandit、新 Agent 训练”四个同等贡献。现有工作已经分别覆盖了通用交互 benchmark、stateful tool benchmark、unreliable-tool hazard benchmark 和静态 LLM routing。更可识别的中心命题是：

> 在真实医药 provider 会产生正常格式但可能错误或冲突的结果时，带有预训练 typed prior 的动态 Jev 能否在只观察实际调用结果、且反馈延迟到达的条件下，在线学习何时选工具、何时验证以及何时拒答，从而降低最终错误和危险作答，同时控制调用成本？

这个命题需要三条分别可推翻的证据：

1. 动态 Jev 相比 static Laya/AnyJev 和标准 bandit，确实从冷启动和漂移中更快恢复；
2. 动态工具适应模块确实增加了验证收益，而不是只增加调用次数；
3. 主 Agent 接收结构化状态后，最终错误、危险作答和拒答质量改善，且 improvement 不能由 provider selection accuracy 代替。

只要其中一条没有证据，就收窄论文主张，不把整个系统包装成一个已经完成的端到端 Agent 学习方法。

## 8. 可关闭的工作项

建议新增 Task47，按以下顺序关闭：

1. **邻域定位**：把 ToolBench-X、ToolSandbox、τ²-bench、LLMRouterBench、MedAgentBench 的任务、evaluator、baseline、许可证和我们差异写进 related-work matrix。
2. **PTRB-v0 合同**：冻结 S1–S4、episode schema、goal/side-effect evaluator、预算和反馈时钟。
3. **split 与泄漏审计**：按分子 scaffold、任务模板、provider/version、时间/漂移选择留出，写出不可见 test 保护方案。
4. **baseline runner**：在同一日志合同下实现 B0–B7、Agent 三版本和五个规则边界，先跑静态 fixture。
5. **最小公开包**：发布样例任务、固定 test evaluator、版本锁、运行脚本、样例轨迹和复现实验说明。
6. **小批科学验证**：满足 Task38/P02 条件后，先跑冷启动、selected-only 在线曲线、验证收益和 Agent 端到端三张表；结果不支持时撤回相应主张。

## 9. 来源

- Liu et al., *AgentBench: Evaluating LLMs as Agents*, https://arxiv.org/abs/2308.03688
- Yao et al., *τ-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains*, https://arxiv.org/abs/2406.12045
- Barres et al., *τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment*, https://arxiv.org/abs/2506.07982
- Lu et al., *ToolSandbox: A Stateful, Conversational, Interactive Evaluation Benchmark for LLM Tool Use Capabilities*, https://arxiv.org/abs/2408.04682
- Debenedetti et al., *AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents*, https://arxiv.org/abs/2406.13352
- Trivedi et al., *AppWorld: A Controllable World of Apps and People for Benchmarking Interactive Coding Agents*, https://arxiv.org/abs/2407.18901
- Xie et al., *TravelPlanner: A Benchmark for Real-World Planning with Language Agents*, https://arxiv.org/abs/2402.01622
- Xie et al., *OSWorld: Benchmarking Multimodal Agents for Open-Ended Tasks in Real Computer Environments*, https://arxiv.org/abs/2404.07972
- Jiang et al., *MedAgentBench: A Realistic Virtual EHR Environment to Benchmark Medical LLM Agents*, https://arxiv.org/abs/2501.14654; official code: https://github.com/stanfordmlgroup/MedAgentBench
- Wang et al., *MINT: Evaluating LLMs in Multi-turn Interaction with Tools and Language Feedback*, https://arxiv.org/abs/2309.10691
- Ma et al., *AgentBoard: An Analytical Evaluation Board of Multi-turn LLM Agents*, https://arxiv.org/abs/2401.13178
- Li et al., *LLMRouterBench: A Massive Benchmark and Unified Framework for LLM Routing*, https://arxiv.org/abs/2601.07206; official code: https://github.com/ynulihao/LLMRouterBench
- Tian et al., *Beyond Function Calling: Benchmarking Tool-Using Agents under Tool-Environment Unreliability*（2026 arXiv preprint）, https://arxiv.org/abs/2606.25819; official code: https://github.com/Foreverskyou/ToolBench-X
- Qin et al., *ToolLLM: Facilitating Large Language Models to Master 16000+ Real-world APIs*, https://arxiv.org/abs/2307.16789
- Shinn et al., *Reflexion: Language Agents with Verbal Reinforcement Learning*, https://arxiv.org/abs/2303.11366
- Wang et al., *Voyager: An Open-Ended Embodied Agent with Large Language Models*, https://arxiv.org/abs/2305.16291
- PILOT, *Adaptive LLM Routing under Budget Constraints*, https://aclanthology.org/2025.findings-emnlp.1301/
- BaRP, *Learning to Route LLMs from Bandit Feedback: One Policy, Many Trade-offs*, https://arxiv.org/abs/2510.07429

## 当前判断的边界

本文档是研究审计和执行建议，不把 PTRB-v0 或任何动态 Jev 结果写成已经完成的实验。Task38 的外部资格、P02 反馈协议和现有训练暂停仍然有效。ToolBench-X 的数据许可证和外部运行环境在正式复用前必须单独核验。
