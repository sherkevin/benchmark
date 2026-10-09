# ChemCrow — 采样说明（深度版）

## 论文与来源
- 论文：*ChemCrow: Augmenting Large-Language Models with Chemistry Tools*（Bran, Cox, Schilter, Baldassari, White, Schwaller；arXiv:2304.05376，1000+ 引；Nat. Mach. Intell. 2024）
- 代码：https://github.com/ur-whitelab/chemcrow-public ｜ 全量运行轨迹：ur-whitelab/chemcrow-runs ｜ 本地采样：gh raw，2026-07

## 它是什么
化学 agent 鼻祖：GPT-4 + **18 个专家设计的化学工具**，自主完成有机合成、药物发现、材料设计。论文原话：*"autonomously planned and executed the syntheses of an insect repellent, three organocatalysts, and guided the discovery of a novel chromophore."*

## 一个完整 case（真实，论文）
长程任务例：**"规划并执行一种驱虫剂的合成"**。agent 需多步：把需求转成目标分子 → 查分子/性质 → 查安全（是否受控/易爆）→ 逆合成规划路线 → 给出可执行方案。这是真正的**多步、跨阶段、自调工具**的长程任务。

## agent 怎么调工具（真实代码 `agent_chemcrow.py`）
langchain 的 `RetryAgentExecutor` + `ChatZeroShotAgent`（ReAct 式），`max_iterations=40`：agent 最多 40 轮"想→选工具→调用→看结果→再想"，最后给答案。工具由 `make_tools()` 注册（`agent_tools_registry.py`）。

## ★ 机制一：调工具时怎么判"对不对"？
和 ChemToolAgent 同类——**只做可执行性校验，不做语义正确性校验**：
- 工具名必须是注册的 18 个之一（langchain 框架约束）；
- 工具能跑出结果（如 RDKit 能解析该 SMILES、RXN4Chem API 返回了产物）；
- **不判**"这步该不该用这个工具""输入是否恰当""输出是否推进任务"——全靠 agent 的 ReAct 推理自我驱动。
- 中间**无 hard checkpoint**；完整工具调用轨迹存在 chemcrow-runs，但事后不逐步判对错。

## ★ 机制二：专家评判时，LLM 是怎么判的？（这是 ChemCrow 评测的核心，也是它的"自曝短板"）
论文评测用**两条腿**：**LLM 评判（GPT-4-as-judge）+ 人类专家评判**，对 agent 的最终答案/方案打分（正确性、是否真完成任务）。

**关键发现（论文摘要原文）**：
> *"Surprisingly, we find that **GPT-4 as an evaluator cannot distinguish between clearly wrong GPT-4 completions and ChemCrow's performance**."*

翻译：让 GPT-4 当裁判时，它**分不清"明显错误的裸 GPT-4 回答"和"ChemCrow（带工具）的正确表现"**——即 **LLM 评判不可靠**，会把错的也判成对的。所以论文最终**以人类化学专家的评判为准**（专家能看出 ChemCrow 因调了工具而更准确，GPT-4 裁判看不出）。

**这对我们项目的直接启示（极重要）**：
- 它**实证了"用 LLM 当 judge 评科学任务不可靠"**——这正是我们坚持 **hard checkpoint（可执行断言/数值比对）而非 LLM judge** 的强力旁证；
- ChemCrow 的失败模式（LLM 裁判失灵）= 我们要规避的；我们的 hard oracle 判分恰好补上这个洞。

## 真实工具清单（`make_tools()`，就是"反查接口"）
- **安全/毒性**：`ExplosiveCheck`、`ControlChemCheck`、`SimilarControlChemCheck`、`SafetySummary`
- **存在性/新颖**：`PatentCheck`（是否已被专利/已知）
- **可合成性**：`RXNPredict`（产物预测）、`RXNRetrosynthesis`（逆合成路线）
- **性质/结构**：`SMILES2Weight`、`FuncGroups`、`MolSimilarity`（RDKit）
- **名称/检索**：`Query2SMILES`、`Query2CAS`、`SMILES2Name`、`Scholar2ResultLLM`、`WebSearch`、`GetMoleculePrice`、`python_repl`

## 对我们的价值与差距
- **价值**：最权威的"agent 多步调化学工具（含毒性/存在性/合成反查）做长程任务"范式；并**用实验证明了 LLM-judge 不可靠**（支撑我们走 hard 路线）。
- **差距**：判分靠人类专家（不可规模化）或失灵的 LLM-judge；**无 hard checkpoint、不评过程**。我们要做的=把它的工具调用轨迹配上**可执行的 hard 中间卡点**，替代不可靠的 LLM/专家评判。

## 采样内容
`agent_chemcrow.py`（agent 主循环）、`agent_tools_registry.py`（18 工具注册）、`agent_prompts.py`（ReAct 提示）、`tool_rdkit.py`/`tool_safety.py`/`tool_reactions.py`/`tool_converters.py`（真实工具实现）、`example_tasks_test_agent.py`、`README.md`。
