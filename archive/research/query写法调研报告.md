# 药物/材料发现 benchmark query 全文展示与写法分析

> **回答的问题**：别人的 query 到底是怎么写的？会不会被 judge"这不是专家常用的写法"？
> **方法**：从 `reference/` 63 个 benchmark 中筛出 18 个直接相关的（药物发现 8 个 + 材料发现 5 个 + 化学/科学 agent 5 个），逐一提取 query 原文。能拿到完整原文的列全文，拿不到的标注来源和原因。
> **结论先行**：**药物/材料发现 benchmark 的 query 都很短很精确**——TOMG-Bench 直接给原子数（"8 carbon atoms, 1 nitrogen atom, 3 oxygen atoms"），MADE 直接给目标属性（"NOVEL, UNIQUE, STABLE"），SciCode 直接给操作（"Write a Script to integrate the Berendsen thermostat"）。**没有一家写"为 xxx 寻找一个候选"这种考试题式的写法**。我们的 query 需要改成这种精确、简短的写法。

---

## 一、有完整 query 原文的 benchmark

### 1. TOMG-Bench（药物发现，LLM 分子生成）

**论文**：*TOMG-Bench: Evaluating LLMs on Text-based Open Molecule Generation*（arXiv:2412.14642）

**query 全文**（三类任务各列三条）：

**MolCustom（定制生成）**：
1. "Please generate a molecule with 8 carbon atoms, 1 nitrogen atoms, and 2 oxygen atoms."
2. "Please generate a molecule with 2 hydroxyl groups."
3. "Please generate a molecule with 1 single bond."

**MolEdit（编辑）**：
1. "Please remove a benzene_ring from the molecule O=C(Cc1ccc(Cl)cc1)NC[C@](O)(c1ccccc1)C(F)(F)F."
2. "Please add a hydroxyl group to the molecule CCO."

**MolOpt（优化）**：
1. "Please optimize the molecule CCO to have a higher LogP value."
2. "Please optimize the molecule CCO to have a higher MR value."
3. "Please optimize the molecule CCO to have a higher toxicity."

**写法特征**：
- **精确约束**：8 碳 1 氮 3 氧——原子级别精确，不是"找一个候选"；
- **操作明确**：generate / remove / optimize——告诉 agent 要做什么；
- **方向明确**：higher LogP / higher MR——告诉 agent 优化方向；
- **极短**：一句话，没有背景描述。

**判分**：RDKit 确定性计算（合法性→原子数比对→LogP 变化方向），无 LLM judge。

**专家背书**：从 Zinc-250K（250 万真实分子库）采样，用 RDKit 反推指令。数据源是权威的，不需要额外专家验证。

---

### 2. MADE（材料发现，闭环）

**论文**：*MADE: Benchmark Environments for Closed-Loop Materials Discovery*（Malik et al., Oxford；ICML 2026）

**query 全文**（YAML 配置原文）：

**版本 1（planner_llm.yaml）**：
> "You are a planner for a material discovery experiment. Your goal is to discover as many NOVEL, UNIQUE, STABLE (or metastable) structures as possible."

**版本 2（agent_llm_react_orchestrator.yaml）**：
> "You are an autonomous materials discovery agent."
> "OBJECTIVE: Find as many NOVEL, UNIQUE, STABLE (or metastable) structures as possible. Use the available tools, then select ONE composition + structure for oracle evaluation."

**写法特征**：
- **角色设定**：planner / autonomous agent——告诉 agent 它是谁；
- **宏观目标**：discover as many NOVEL, UNIQUE, STABLE structures as possible——不给固定路径；
- **约束内嵌**：NOVEL（新的）、UNIQUE（不重复）、STABLE（热力学稳定）——约束写在目标里；
- **交付物明确**：select ONE composition + structure for oracle evaluation——告诉 agent 要交什么。

**判分**：凸包环境 + ORB/MACE oracle（形成能），评"发现效率"（多快找到稳定材料）。

**专家背书**：凸包环境+相图来自材料科学标准数据库，领域公认。

---

### 3. ChemCrow（化学 agent，证明了 LLM judge 不可靠）

**论文**：*ChemCrow: Augmenting Large-Language Models with Chemistry Tools*（Bran et al.；Nat. Mach. Intell. 2024，1000+ 引）

**query 全文**（agent_prompts.py 原文）：

**系统提示**：
> "You are an expert chemist and your task is to respond to the question or plan a synthesis route, execute a synthesis, find a similar molecule, or modify a molecule."

**ReAct 提示**：
> "In this exercise you will assume the role of a scientific assistant. Your task is to answer the provided question as best as you can, based on the provided solution draft."

**写法特征**：
- **角色设定**：expert chemist / scientific assistant；
- **操作列表**：plan / execute / find / modify——告诉 agent 能做什么；
- **无具体约束**：query 本身是通用的，具体约束在任务实例里。

**判分**：**人类化学专家评判**——因为论文证明了 LLM judge 不可靠（"GPT-4 分不清错的和对的"）。

**专家背书**：18 个专家设计的化学工具 + 人类专家评判。

---

### 4. SciCode（科学编程，NeurIPS 2024）

**论文**：*SciCode: A Research Coding Benchmark Curated by Scientists*（NeurIPS D&B 2024）

**query 全文**（first_problem.jsonl 原文）：

**case 1**：
> "Write a Script to integrate the Berendsen thermalstat and barostat into molecular dynamics calculation through velocity Verlet algorithm. The particles are placed in a periodic cubic system, interacting with each other through truncated and shifted Lenard-Jones potential and force."

**写法特征**：
- **操作明确**：Write a Script to integrate——告诉 agent 要写代码；
- **技术细节精确**：Berendsen thermostat + barostat + velocity Verlet + Lenard-Jones potential——全是计算物理标准术语；
- **无背景描述**：不说"为什么"，直接说"做什么"。

**判分**：子问题断言（hard，可执行测试）。

**专家背书**：科学家标注的背景和金标准解答。

---

### 5. DiscoveryBench（数据驱动科学发现，AI2）

**论文**：*DiscoveryBench: Towards Data-Driven Discovery with Large Language Models*（AI2，arXiv:2407.01725）

**query 全文**（sample_task_archaeology/metadata_0.json 原文）：

**query**：
> "In which century did the axes become quantitatively most frequent?"

**domain_knowledge**：
> "1. Symbolic capital consists of Hatchet & Swords, Daggers, House Size. 2. Social Capital consists of Copper and Gold, Amber, Monument Count. 3. Cultural Capital consists of Diversity of Pottery form, Diversity of Pottery Decoration. 4. Economic Capital consists of Depot, Sickle, Axes & Celts, Monument Count..."

**写法特征**：
- **query 极短**：一句话问题——"In which century did the axes become quantitatively most frequent?"
- **领域知识单独给**：domain_knowledge 是一大段背景——"Symbolic capital consists of..."；
- **从论文反构造**：任务来自一篇真实考古学论文。

**判分**：GPT-4 假设匹配（soft，LLM judge）——这是它的短板，我们不学。

**专家背书**：从已发表论文反向抽取，金标准来自论文。

---

### 6. SciAgentArena（科学 agent，分步验证）

**论文**：*Benchmarking AI Agents for Addressing Scientific Challenges Across Scales*（Yale/Harvard 等 33 人，2026）

**query 全文**（C4 毒性筛查任务，agent_c4_tox.py 原文）：

> agent 从环境变量 `AGENT4S_INPUT_JSON` 读到任务载荷（含 `query_molecules_file_path`——一批待筛分子的 CSV），用 RDKit SMARTS 逐分子匹配毒性警示结构（nitroaromatic `[a][N+](=O)[O-]`、michael_acceptor、epoxide、quinone 等 7 类），输出严格的逐分子 JSON（含 alert_type/SMARTS/匹配原子索引/风险等级）到 stdout。

**写法特征**：
- **任务载荷**：给文件路径，agent 自己读；
- **交付物明确**：逐分子 JSON（含 alert_type/SMARTS/匹配原子索引/风险等级）；
- **判分四级链条**：executability → validity → correctness → strategic_success。

**专家背书**：ground truth CSV + 确定性判分。

---

## 二、query 为隐式（无自然语言）的 benchmark

以下 benchmark 不用自然语言 query，而是给打分函数/oracle，agent 直接优化：

| benchmark | 隐式 query 形式 | 说明 |
|---|---|---|
| **GuacaMol** | 打分函数（Tanimoto 相似度、MPO 多目标、Isomer 分子式、QED/logP/tPSA） | 无自然语言 query，agent 直接优化打分函数 |
| **Tartarus** | 物理模拟 oracle（QuickVina 对接、xTB 量子化学、反应活化能） | 无自然语言 query，agent 生成 SMILES，oracle 打分 |
| **MOSES** | 分布学习（validity、uniqueness、novelty、FCD、KL 散度） | 无自然语言 query，agent 学训练集分布 |
| **TDC_PMO** | 分子生成任务（docking score、QED、logP） | 无自然语言 query，agent 优化物化性质 |
| **PoseBench** | 对接姿态物理检查（RDKit 键长/键角/内部冲突） | 无自然语言 query，agent 生成 3D 构象，检查物理合理性 |
| **PoseBusters** | 对接姿态物理检查（能量比、平面度、分子间距离） | 无自然语言 query，检查 AI 对接方法生成的构象是否物理有效 |

**关键发现**：**这些 benchmark 根本不写自然语言 query**——它们直接给打分函数或物理模拟 oracle，agent 不需要理解题意，只需要优化目标函数。

**对我们的意义**：我们的 query 是"约束满足式"（给约束条件，agent 找满足的候选），这比隐式 query 更难——agent 需要理解约束、检索候选、查证性质、筛选排序。我们的 query 写法在复杂度上已经超过了这些 benchmark。

---

## 三、无可提取 query 的 benchmark

以下 benchmark 在本地文件中无可提取的 query 文本（纯数据/纯代码/纯 PDF，或 query 在 HF/OpenReview 在线平台上）：

| benchmark | 原因 |
|---|---|
| Biomni | query 在 HF 数据集页面（gated） |
| MatSciBench | 纯数据文件 |
| MatTools | 纯代码 |
| DiscoveryWorld | query 在 example_scorecard.json 里但只有一句 "follow the instructions of the other person in the room" |
| ICLR-* 系列 | query 在 OpenReview PDF 里 |
| ICRA-* 系列 | query 在 OpenReview PDF 里 |

---

## 四、我们的 query 会被 judge 吗？

### 当前 query 的问题

> "为中枢神经系统寻找一个候选：需要一个能进入大脑、对心脏安全、口服吸收好的药物。"

**会不会被 judge**：**会**。审稿人可能会问：
1. "这个 query 是专家写的吗？"——不是，是自造的；
2. "这个 query 的术语是领域标准吗？"——不完全是，"能进入大脑"不如"BBB 渗透性"专业；
3. "这个 query 的约束是精确的吗？"——不精确，没有数值阈值。

> **⚠️ 订正（task14，经 62 家 benchmark 逐条取证）**：上面第 1 条的回答「不是，是自造的」**本身没错，但紧接着得出的推论错了**。它在下文被用来论证「大家都不做专家验证」，而实测结果是：**至少 8 家 benchmark 明写了领域专家参与**（SciCode / ScienceAgentBench / NewtonBench / ChemCrow / TRAIL / Tool-Decathlon / RE-Bench / PaperBench）。所以「没有专家环节」是**我方的差距**，不是行业惯例。逐条原文证据见 [`数据集权威性总览.md`](../task/task14/数据集权威性总览.md) §四.2。

### 别人的 query 为什么不被 judge

| benchmark | 为什么不被 judge |
|---|---|
| TOMG-Bench | 数据从 Zinc-250K 采样（**约 25 万**真实分子库，原写 250 万错了 10 倍，task10 已订正），RDKit 确定性判分——数据源+判分器都是硬的 |
| MADE | 凸包环境+相图来自材料科学标准数据库——领域公认 |
| ChemCrow | 18 个专家设计的化学工具 + 人类专家评判——工具链和评判都是专家级的 |
| SciCode | 科学家标注的背景和金标准解答——标注者是专家 |
| SciAgentArena | ground truth CSV + 确定性判分——判分是硬的 |

> **⚠️ 订正（task14）**：本表自身就包含了两条专家参与的例子（ChemCrow、SciCode），**与上一节第 1 条的「不是专家写的」形成直接矛盾**。矛盾根源是把「我方的 query 自造」错推广成了「大家都自造」。正确的图景是：**他人解决「任务是否真实」有两条路**——① 让专家审任务的科学合理性；② 从真实来源反向抽取。我方只做了②，没做①。

**规律**：只要**数据源是权威数据库**（Zinc/TDC/真实蛋白结构/标准相图），**判分是确定性计算**（RDKit/物理模拟/子问题断言），就不需要额外的专家逐题验证——数据源和判分器本身就是专家背书。

> **⚠️ 订正（task14）**：这条「规律」**是我方的主张，不是从他人论文观察到的事实**，不得写成领域规律。反例：ScienceAgentBench 的数据源（同行评议论文）与判据（可执行脚本）都很硬，**但它仍然请了 9 位领域专家逐题验证、两轮把关、改了 41 条 rubric 并删了 3 题**——这直接反驳了「数据源硬就不需要专家验证」。

### 我们该怎么改

**我们的数据源已经是权威的**——TDC ADMET benchmark 的 BBB 渗透性、hERG 毒性、Caco-2 渗透性、logS、logD、half_life 数据集。我们的判分也是确定性的（checkpoint reward 可执行断言）。

**需要改的是 query 写法**：

| 当前 | 改进 | 参考 |
|---|---|---|
| "为中枢神经系统寻找一个候选" | "Find a small molecule with BBB ≥ 1, hERG = 0, Caco-2 > -5.0" | TOMG-Bench 精确约束 |
| "需要一个能进入大脑的药物" | "BBB 渗透性 ≥ 1（穿透血脑屏障）" | TDC ADMET 术语 |
| "对心脏安全" | "hERG 毒性 = 0（无心脏钾通道阻断）" | TDC ADMET 术语 |
| "口服吸收好" | "Caco-2 渗透性 > -5.0（口服生物利用度可接受）" | TDC ADMET 术语 |
| "给出最优答案" | "Return ranked candidates with property values" | SciAgentArena 交付物 |

**在 case 的 `provenance` 里写**"基于 TDC ADMET benchmark 的 BBB 渗透性与 hERG 毒性数据集"——这就是专家背书，不需要额外的专家验证。

---

## 五、结论

1. **药物/材料发现 benchmark 的 query 都很短很精确**——TOMG-Bench 给原子数，MADE 给目标属性，SciCode 给操作指令，没有"为 xxx 寻找一个候选"这种考试题式的写法；
2. **没有一家被 judge"这不是专家常用的写法"**——因为数据源和判分器本身就是专家背书；
3. **我们的 query 需要改成精确、简短的写法**——给精确数值约束（BBB≥1、hERG=0、Caco-2>-5.0），用 TDC ADMET 术语，在 provenance 里写数据源；
4. **我们不需要额外的专家验证**——TDC ADMET 数据集本身就是权威背书，跟 TOMG-Bench 用 Zinc-250K、MADE 用标准相图一样。

> **⚠️ 第 2 与第 4 条已被 task14 推翻，以下为现行结论**（证据：[`query构造与专业性保证.md`](query构造与专业性保证.md) §三 逐字引的 ScienceAgentBench 附录 G 问卷原文）：
>
> **第 2 条属论证偷换，且事实也错**。「没有一家被质疑」≠「没有一家做专家验证」。而且 **ScienceAgentBench 直接就在专家问卷里问了这件事**：它明写「题面是非专家写的，可能有**术语误用、表达生硬、不符合你们领域的科学语言**」，然后请专家**重写**——结果改了 **41 条**题面（占 102 题的 40%）、删了 3 道。所以「没有人在意 query 写法」是错的，**恰恰有人把它当成了核心环节**。
>
> **第 4 条结论不成立**。ScienceAgentBench 数据源是 44 篇同行评议论文、判据是可执行脚本（两者都比我方硬），**它仍然请了 9 位专家两轮把关**。所以「数据源权威就不需要专家验证」是错的。
>
> **现行结论：专家审核有三层，我方一层未做**——L1 题面措辞符合科学语言（ScienceAgentBench 请专家重写）、L2 任务科学合理性（9 位专家两轮）、L3 任务代表性（删了 3 题）。我方只做了**并行路线**（从真实数据源反查真值）。整改方案以 [`query构造与专业性保证.md`](query构造与专业性保证.md) 的四道关为准（该文早于本订正存在，且论证更完整）。

---

## 参考

- reference/TOMG-Bench/README_采样说明.md + task_readme/MolCustom.md + task_readme/MolOpt.md（精确约束写法）
- reference/MADE/README_采样说明.md + configs/agent_llm_react_orchestrator.yaml（目标导向写法）
- reference/ChemCrow/README_采样说明.md + agent_prompts.py（角色设定写法）
- reference/SciCode/README_采样说明.md + first_problem.jsonl（指令式写法）
- reference/DiscoveryBench/README_采样说明.md + sample_task_archaeology/metadata_0.json（数据驱动发现写法）
- reference/SciAgentArena/README_采样说明.md + dd_examples/agent_c4_tox.py（分步验证写法）
- reference/GuacaMol/README_采样说明.md（隐式打分函数）
- reference/Tartarus/README_采样说明.md（隐式物理模拟 oracle）
- governance/adr/0012-两类benchmark设计范式与query专家背书要求.md
