# Science Agent 领域 Benchmark 构建调研

> 调研日期：2026-07 ｜ 方法：多渠道并行检索（arXiv / 官方仓库 / 综述）交叉核验。
> 目的：梳理"科学智能体（science agent）"benchmark 都有哪些、**怎么构建的**（谁出题、金标准怎么来、怎么判分、人力 vs 自动）——与本项目"过程量 hard checkpoint"需求对齐。
> 每条结论附来源；不确定的数量以"约"标注。
> **2026-07-27 更新（全覆盖版）**：补入分子发现 oracle 系（Tartarus/TDC+PMO/GuacaMol/MOSES/TOMG-Bench/PoseBusters/PoseBench）与 2026 新发（MADE、SciAgentArena）——至此覆盖本项目调研过的**全部** science 相关 benchmark；标 ★落盘 的均有深度 case 文档与真实采样，见 `reference/<同名>/`。

---

## 一、总览：按领域分九类

| 类 | 代表 benchmark | 一句话 |
|---|---|---|
| 0 聚合套件/综述 | **AstaBench**、A Survey of LLM-based Scientific Agents | 把多个科学 benchmark 打包统一评测 |
| 1 数据驱动发现 | **DiscoveryBench**、**ScienceAgentBench**、DSBench | 给数据+任务，agent 写分析代码/找假设 |
| 2 科研编程/复现 | **SciCode**、**CORE-Bench** | 把科研计算/论文复现拆成可测子任务 |
| 3 ML/AI 研究工程 | **MLAgentBench**、MLE-bench、RE-Bench、PaperBench | agent 端到端做 ML 实验/复现论文 |
| 4 化学/分子发现 | **ChemBench**、ChemCrow★落盘、**ChemToolAgent**★落盘、**TOMG-Bench**★落盘、**TDC+PMO**★落盘、**GuacaMol**★落盘、**MOSES**★落盘、**SciAgentArena(dd)**★落盘 | 化学知识/工具调用/分子生成 oracle 反查 |
| 4b 结构/对接终态反查 | **PoseBusters**★落盘、**PoseBench**★落盘 | 最终 3D 构象的物理/化学合理性接口反查 |
| 5 生物/生信 | **LAB-Bench**、**BixBench**、**BiomniBench**、BioPlanner、BioAgent Bench、Biomni★落盘 | 生信工作流复现/协议规划/生物医学 agent |
| 6 材料 | **LLM4Mat-bench**、CrystalGym、**Tartarus**★落盘、**MADE**★落盘 | 晶体属性预测/材料发现/闭环发现环境 |
| 7 科学理解/推理/长上下文 | **CURIE**、SciBench、SciAssess、GPQA、OlympiadBench | 长文档理解、专业推理、信息抽取 |
| 8 端到端自动研究 | AI Scientist、ResearchBench、PaperArena | 从想法到论文的全流程 |

---

## 二、逐个详解（含构建方式）

### 类 1 · 数据驱动发现

**DiscoveryBench**（AI2，arXiv:2407.01725）
- 是什么：264 个数据驱动发现任务，覆盖社会学、生物、经济等 6 域。
- 怎么构建：**从已发表论文里反向抽取真实科研工作流**——把论文里"用某数据集验证某假设"的过程，整理成 (数据集 + 任务描述 + 金标准假设) 三元组；另有一部分合成任务。**人力**：研究者读论文、提炼假设、整理数据（一次性）。
- 怎么判分：金标准是"目标假设"（变量 + 变量间关系 + 上下文）。评测把 agent 产出的假设按 facet（context/variables/relationship）分解，与金标准逐面比对——**含 LLM 辅助的假设匹配**（非纯 hard）。

**ScienceAgentBench**（OSU，arXiv:2410.05080）
- 是什么：102 个数据分析任务，来自 44 篇论文、4 学科（生信/计算化学/地理/心理神经）。
- 怎么构建：专家从论文提取任务 + 写 gold 分析程序 + 定义任务专属成功标准（论文附录 C）。**人力**：领域专家标注。
- 怎么判分：多维——**VER**（代码能否跑通，纯 hard）+ **SR**（结果达标否，对照 gold）+ **CBS**（代码相似度）。

**DSBench**（arXiv:2409.07703）
- 540 个数据科学任务（466 ModelOff 问答 + 74 Kaggle 建模）；官方答案/相对性能判分，**终态无中间卡点**。

### 类 2 · 科研编程/复现

**SciCode**（arXiv:2407.13168，NeurIPS 2024）
- 是什么：80 主问题 → 338 子问题，覆盖物理/数学/材料/生物/化学 16 子域。
- 怎么构建（我们已深挖）：**科学家（≥2 位 PhD）手工**拆子问题、写金标准解、设计断言测试；**target 金标准数值由跑金标准解自动生成→存 H5**；判分器是通用 harness（写一次）；GPT-4 仅当红队查假阳性、不参与出题。
- 怎么判分：子问题通过率（中间卡点）+ 主问题解决率。**纯 hard（Python 断言）**。

**CORE-Bench**（arXiv:2409.11363）
- 是什么：基于真实论文代码胶囊（code capsules）的**计算可复现性**任务，考 agent 能否装环境、跑代码、复现论文报告的数值结果。
- 怎么构建：**harvest**——从已发表、附代码与数据的论文里取现成 capsule，任务=复现其结果。金标准=论文报告的数值。判分：数值比对（hard）。

### 类 3 · ML/AI 研究工程

**MLAgentBench**（Stanford，arXiv:2310.03302）
- 13 个端到端 ML 实验任务（从 CIFAR-10 提点到 BabyLM）。构建：研究者选定任务+数据+起始代码。判分：**相对基线的性能提升 + 成功率**，执行式（hard）。

**MLE-bench**（OpenAI）/ **RE-Bench**（METR）/ **PaperBench**（OpenAI）
- 见项目 `数据集分类与详解.md`：MLE-bench=75 Kaggle 竞赛终态奖牌；RE-Bench=7 环境连续 scoring（随时可测，hard）；PaperBench=20 论文复现、8316 叶子 rubric 树（LLM judge）。

### 类 4 · 化学

**ChemBench**（arXiv:2404.01475，lamalab）
- 是什么：2700+ 化学问答，覆盖广泛主题，含专家对照（部分题顶尖模型超过专家）。
- 怎么构建：从教材/考试/数据库/专家**多源整理**成 MCQ+数值题；**人力**：化学家审校。
- 怎么判分：MCQ/数值**自动比对**（hard），无中间卡点（知识问答型，非长程 agent）。

**ChemCrow**（Nature Machine Intelligence 2024）★落盘 `reference/ChemCrow/`
- 化学工具增强 agent（合成规划、分子性质等）；偏"用工具做对"的能力展示 + 专家评估，非标准化大规模判分 benchmark。
- ★补充实证（论文摘要原文）：**"GPT-4 as an evaluator cannot distinguish between clearly wrong GPT-4 completions and ChemCrow's performance"**——LLM judge 失灵的直接证据，支撑本项目 hard 路线。

**ChemToolAgent**（OSU-NLP，arXiv:2411.07228）★落盘 `reference/ChemToolAgent/`
- ReAct 多步化学 agent，研究"配工具到底有没有用"（有/无工具答题率对比）；工具含 UniMol 性质预测、名称转换、分子描述。
- 判分：终答正确性比对（hard）；工具调用只做可执行性校验（`_call_tool`：工具名合法+执行不报错），**不判语义正确性**——过程无 hard 卡点。

**TOMG-Bench**（arXiv:2412.14642）★落盘 `reference/TOMG-Bench/`
- 是什么：文本指令开放式分子生成（MolEdit/MolOpt/MolCustom 三族），直接测 LLM 本身（25 个模型）。
- 怎么构建：从 Zinc-250K 采样真实分子，用 RDKit 反推出题（每子任务 5000）。
- 怎么判分：**RDKit 接口反查**——validity（合法性=存在性）+ accuracy（原子/基团/属性是否达标）+ similarity/novelty，纯 hard；但**单轮 chat 式**（给指令→出分子→事后反查），agent 全程不调工具、非长程。

**TDC + PMO**（TDC: NeurIPS 2021 D&B；PMO: NeurIPS 2022）★落盘 `reference/TDC_PMO/`
- 是什么：药物发现 oracle 库（`oracle(smiles)→分数`）；PMO 把 23 个 oracle 包成"1 万次调用预算"的优化任务。
- ★判分分两档（真实代码 `oracle.py`）：**A 类确定性公式**（QED/SA/logP，RDKit 现算，纯 hard）；**B 类 ML 预测器**（DRD2/GSK3β/JNK3 活性、hERG/ClinTox 毒性——确定但有模型误差，非物理真值）。

**GuacaMol / MOSES**（arXiv:1811.09621 / Front. Pharmacol. 2020）★落盘 `reference/GuacaMol/`、`reference/MOSES/`
- GuacaMol：目标导向优化（Tanimoto 相似度/MPO 多目标/同分异构约束）+ 分布学习，全 RDKit 确定性打分（hard、终态）；
- MOSES：生成分子集的分布指标（valid/unique/novelty/FCD/SNN/IntDiv），hard 但对象是分布非单任务。

**SciAgentArena（Agent4Science·dd）**（arXiv:2606.12736，2026）★落盘 `reference/SciAgentArena/`
- 是什么：跨域科学 agent 竞技场（约 200 任务，分步验证）；dd 药物域 79 任务：agent 写并执行 Python 代码，框架受控跑代码判分。
- ★判分链 `executability→validity→correctness→strategic_success`（分层 hard，无 LLM judge，离线执行）；**C3 为预算制 oracle 分子优化**（QED/logP/DRD2，超预算抛异常）；C4 毒性 SMARTS 警示。
- 差异：任务是单轮代码生成-执行，不追踪长程轨迹逐步正确性。

### 类 4b · 结构/对接终态反查

**PoseBusters**（arXiv:2308.05777，Chem. Sci. 2024）★落盘 `reference/PoseBusters/`
- 是什么：对 AI 对接/构象生成方法**最终输出的 3D 构象**跑 RDKit 物理/化学检查（键长键角界限/立体冲突/环平坦/能量比/手性保持）。
- 怎么判分：逐项布尔 + 通过率，纯 hard 纯终态；实测发现"AI 方法常生成物理无效构象"（论文标题即结论）。

**PoseBench**（Nature Machine Intelligence）★落盘 `reference/PoseBench/`
- 是什么：蛋白-配体结构预测综合评测（11 方法 × 4 数据集含 CASP15 盲测）；
- 判分：RMSD（vs 实验结构）+ **内置 PoseBusters rescoring**（配置文件实证）；质量门=无晶体接触靶点清单（防污染）。

### 类 5 · 生物/生信

**LAB-Bench**（FutureHouse）
- 生物文献理解、图表解读、协议、数据库/序列操作等 QA；金标准=标准答案，MCQ 自动判分。

**BixBench**（FutureHouse，2025）
- 是什么：真实生信数据分析任务。
- 怎么构建：**由资深生物信息学家复现已发表研究里的数据分析**，把复现过程整理成任务（含数据 + 开放问答/MCQ）。**人力**：生信专家复现+标注。
- 怎么判分：开放答案 + MCQ，对照专家复现结果。

**BiomniBench**（Stanford Biomni 团队，biorxiv 2026）★过程级
- 是什么：**过程级**评测生物 agent——不只看终答，评"分析过程"。
- 怎么构建/判分：**6 维 rubric 打分**——data handling / method selection / statistical rigor / biological interpretation / scientific reasoning / （+1 维）。判定含 rubric + LLM（非纯 hard），但"给过程打分"的思路与我们高度同源。

**BioPlanner**（arXiv:2310.10632）
- 生物实验**协议规划**：给目标，agent 生成实验步骤序列，与金标准协议按步骤/伪代码比对。构建：从已发表协议整理。

**BioAgent Bench**（arXiv:2601.21800）
- 端到端生信工作流评测套件，覆盖多类真实任务。

### 类 6 · 材料

**LLM4Mat-bench**（npj Comput. Mater. 2025）
- 是什么：材料**属性预测**基准，用晶体的多种表示（成分/CIF/描述）输入。
- 怎么构建：**harvest**——从 Materials Project 等公开数据库取材料+实测/计算属性当金标准。判分：数值回归指标（hard），无中间卡点。

**CrystalGym**（AI4Mat-ICLR-2025）
- 用 RL 环境做材料发现；连续奖励（能量等）。

**Tartarus**（arXiv:2209.12487，NeurIPS 2023 D&B）★落盘 `reference/Tartarus/`
- 是什么：逆向分子设计 benchmark——产出 SMILES，**用物理模拟当 oracle 打分**，材料+化学+医药全覆盖（OLED 能隙/光伏 PCE/三靶点对接/反应活化能+SAScore/PAINS 过滤）。
- 怎么构建：无标准答案（化学空间太大）——目标=可计算优化目标，起始池=GDB-13/HCE 公开库，靶点=PDB 真实蛋白；判分器=5 个物理模拟脚本（写一次全体复用）。
- 怎么判分：**物理模拟确定性计算（hard）**，但纯终态（只评最终分子）；自带基线是遗传算法（solver 无关）。

**MADE**（arXiv:2601.20996，ICML 2026）★落盘 `reference/MADE/`
- 是什么：**闭环材料发现环境**——agent 在受限 oracle 预算下迭代"提议结构→能量反馈→改进"，形式化为相图凸包上找热力学稳定化合物。
- 怎么构建：环境（ConvexHullEnvironment）+ oracle（ORB/MACE 形成能）+ 组件化 agent（generator/filter/planner/scorer，含 56KB LLM ReAct 编排器）。
- 怎么判分：**发现效率**（AF/EF/AUDC/mSUN，相对基线多快多省找到稳定材料），oracle 为物理量（hard）；记录完整 episode 轨迹但**不在中间 checkpoint 判过程对错**——与本项目差异化最直接（同域同闭环，它评效率、我们评过程正确性）。

### 类 7 · 科学理解/推理/长上下文

**CURIE**（Google，arXiv:2503.13517）
- 是什么：多任务科学**长上下文理解/推理/信息抽取**，10 个任务、约 429 篇研究文档、6 域。
- 怎么构建：**从真实论文抽取** + 专家标注金标准（结构化抽取/推理答案）。
- 怎么判分：混合——结构化抽取用程序化指标，开放答案用 LLM judge。

**SciBench / SciAssess / GPQA / OlympiadBench / SciEval**
- 科学考题/文献评估型 QA，多为专家或考试题整理，MCQ/数值自动判分——测"知识与推理"，非长程 agent 过程。

### 类 8 · 端到端自动研究

**AI Scientist**（Sakana）/ **ResearchBench** / **PaperArena** / **ResearchClawBench**（arXiv:2606.07591，40 任务 10 域）
- 从想法生成→实验→写论文的全流程。判分多依赖 **LLM/评审式**（同行评审模拟），过程判定弱、主观性强。

### 类 0 · 聚合套件

**AstaBench**（AI2，arXiv:2510.21652）
- 是什么：**科学 agent 评测套件**，2400+ 样例、11 个 benchmark，首轮评了 57 个 agent / 22 种架构。
- 价值：目前最全的"科学研究能力"holistic 评测；把文献检索、编码、数据分析等多类任务统一到一个成本感知框架下。

---

## 三、按"构建方式"横切（本项目最关心）

| 构建范式 | 谁出题/金标准怎么来 | 判分 | 代表 |
|---|---|---|---|
| **① 从已发表论文反向抽取** | 研究者读论文，提炼任务+金标准假设/结果 | 面向假设/结果比对（常含 LLM） | DiscoveryBench、ScienceAgentBench、CURIE、BixBench、BioPlanner |
| **② 专家手工出题+写测试** | 领域 PhD 拆分+写 gold+设计测试；target 跑 gold 自动生成 | 可执行断言（hard） | SciCode、ScienceAgentBench(VER) |
| **③ harvest 现成** | 直接用公开数据库/竞赛/论文代码胶囊 | 数值/测试比对（hard） | CORE-Bench、MLE-bench、LLM4Mat-bench、DSBench |
| **④ 环境+连续 scoring** | 作者写 scoring 函数/RL 环境 | 连续分，随时可测（hard） | MLAgentBench、RE-Bench、CrystalGym |
| **⑤ rubric/过程级打分** | 专家定多维 rubric | rubric+LLM judge（非纯 hard） | PaperBench、BiomniBench |
| **⑥ 考题/QA 整理** | 教材/考试/专家多源整理 | MCQ/数值自动（hard，但无过程） | ChemBench、LAB-Bench、GPQA、SciBench |
| **⑦ oracle 终态反查（分子/构象）** | 性质 oracle=物理模拟/化学公式/ML 预测器；金标准=oracle 现算 | oracle 确定性打分（hard，纯终态） | Tartarus、TDC+PMO、GuacaMol、MOSES、TOMG-Bench、PoseBusters、PoseBench |
| **⑧ 闭环环境+效率指标** | 环境+oracle 预算+基线算法 | 发现效率（AF/EF/AUDC，hard，评效率非过程对错） | MADE |

**三个关键观察（对我们）**：
1. **science agent benchmark 的金标准，最主流来源是"从已发表论文反向抽取"**（范式①）——因为科学任务的"正确答案"往往就藏在论文的方法与结果里，抗污染靠新论文/隐藏测试集。这与本项目"用文献已知值当 target"的思路一致。
2. **真正做"过程级 hard 判分"的极少**：SciCode（子问题断言）、ScienceAgentBench 的 VER、MLAgentBench/RE-Bench（可执行 scoring）是少数 hard 的；**BiomniBench 明确做过程级评测但用 rubric+LLM（soft）**——说明"科学过程量 + hard 判定"仍是稀缺，正是本项目空白。
3. **化学/材料/QA 类多为终态 MCQ/数值**（知识测试），不涉长程过程；长程过程集中在"数据驱动发现 / ML 研究工程 / 端到端研究"三类。

---

## 四、来源依据（均经检索核验）

- 综述：A Survey of LLM-based Scientific Agents (arXiv:2503.24047)；HKUST-KnowComp/Awesome-LLM-Scientific-Discovery；awesome-scientific-llm-benchmarks
- DiscoveryBench arXiv:2407.01725 (allenai/discoverybench)｜ScienceAgentBench arXiv:2410.05080｜DSBench arXiv:2409.07703
- SciCode arXiv:2407.13168｜CORE-Bench arXiv:2409.11363
- MLAgentBench arXiv:2310.03302｜MLE-bench (OpenAI)｜RE-Bench arXiv:2411.15114｜PaperBench arXiv:2504.01848
- ChemBench arXiv:2404.01475｜ChemCrow (Nat. Mach. Intell. 2024)
- LAB-Bench (FutureHouse)｜BixBench (FutureHouse 2025)｜BiomniBench (biorxiv 2026)｜BioPlanner arXiv:2310.10632｜BioAgent Bench arXiv:2601.21800
- LLM4Mat-bench (npj Comput. Mater. 2025)｜CrystalGym (AI4Mat-ICLR-2025)
- CURIE arXiv:2503.13517 (google/curie)｜AstaBench arXiv:2510.21652 (allenai/asta-bench)
- Tartarus arXiv:2209.12487 (aspuru-guzik-group/Tartarus)｜TDC (mims-harvard/TDC)｜PMO (NeurIPS 2022)｜GuacaMol arXiv:1811.09621｜MOSES (molecularsets/moses)
- TOMG-Bench arXiv:2412.14642 (phenixace/tomg-bench)｜ChemToolAgent arXiv:2411.07228 (OSU-NLP-Group/ChemToolAgent)
- PoseBusters arXiv:2308.05777 (maabuu/posebusters)｜PoseBench (BioinfoMachineLearning/PoseBench)
- MADE arXiv:2601.20996 (diffractivelabs/MADE)｜SciAgentArena arXiv:2606.12736 (HelloWorldLTY/SciAgentArena)

> 注（2026-07-27）：标 ★落盘 的 10 个已在 `reference/` 完成真实采样 + 深度 case 文档（含真实数据、判分脚本与机制拆解）；未落盘的（DiscoveryBench/BiomniBench/CORE-Bench 等）如需同规格处理可继续。
