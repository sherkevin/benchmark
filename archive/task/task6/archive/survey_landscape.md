# AI4S Agent Benchmark Panoramic Survey · Supplement (2026-06)

| | |
|---|---|
| **上游任务** | task4 ——「开源 agent 测评 benchmark 全景调研」(2026-06-03) |
| **本轮定位** | **补充调研**:填补 task4 的三大缺口——材料方向空白、药物过程量缺失、2025–2026 新发 benchmark 未覆盖 |
| **方法** | deep-research workflow(5 角度并发搜索 → 24 个一手源 → 120 条声明 → 25 条经 3 投票对抗式核验,24 通过 / 1 被驳)+ 8 个专项 deep-read agent(共 ~7.5M tokens) |
| **新增深读卡** | **14 张**(≥10 硬指标达成;材料 3 张,药物过程量 3 张,医疗/生物 4 张,过程量方法论 2 张,补充 2 张) |
| **原始拉取** | `prd/task6_papers/`(15 篇关键论文 abstract/metadata) |
| **调研笔记** | `search/task6_deep_research_raw.json`(完整 workflow 输出) |
| **更新日期** | 2026-06-09 |
| **2026-06-09 增补** | +2 张深读卡(**C5 χ-Bench** 设计孪生 / **F1 TrialPanorama** 药物侧数据源)+ cubox 去重表。来源:用户 Cubox `medical-benchmark` 收藏(16 篇,知乎正文用 `work/.claude/skills/zhihu-fetch` 补全);**arXiv 核心数据已一手核对**(2605.16679 / 2505.16097) |

---

## TL;DR（与 Task 4 的 Delta）

Task 4（2026-06-03）深读了 7 张卡,核心结论是**"没有任何现成 benchmark 已做人类介入分级 GT"**。本轮补充调研在 5 个方向扩展后,**新增 14 张深读卡**,关键发现：

1. **材料方向仍无真正的 agent benchmark 带过程量——但出现了突破口。** LLM4Mat-Bench(1.9M 结构)和 Matbench Discovery(215K 结构)是大规模 ML 基准,纯最终答案;LLaMP / MASTER / Aethorix 是框架不是 benchmark。唯一的例外是 **AutoMat**（JHU CLSP,2026-05）——85 个来自真实论文的材料科学可复现性声明,5 维评分（含方法忠实度、执行能力、科学严谨性）,LLM evaluator agent 打分,quadratic-weighted κ=0.69。这是材料方向**唯一**带过程量的 agent benchmark。
2. **药物方向找到了 3 个带真正过程量的候选。** Syntheseus（Microsoft,MIT）提供逆合成的时间-到-首个路线曲线 + 单调 diversity 指标;SDDBench 的 round-trip score 是内在的多步度量;SEISMO/PMO 的 AUC 优化曲线是轨迹级指标。
3. **医疗方向出现了两个里程碑。** **PhysicianBench**（Stanford HealthRex,Apache-2.0,2026-05）：100 个长程 EHR 任务 × 670 个结构化 checkpoint,4 类 checkpoint 分类（Data Retrieval → Clinical Reasoning → Action Execution → Documentation）,execution-grounded Python verifier——几乎是 TheAgentCompany 在医疗领域的翻版。**BioAgent Bench**（ICML 2026）：10 个端到端生物信息学流水线任务,LLM-judge 评分含 `steps_completed / steps_to_completion` 的部分得分。
4. **过程量方法论有两个可借的框架。** AgentBoard（NeurIPS 2024 Oral）定义了 progress rate（子目标完成分数,单调递增）;TRACE（WWW 2026）定义了 Hierarchical Trajectory Utility（MIG + REP + Evidence Grounding + Scaffolded Capability Assessment）。
5. **Task 4 §7 四个开放问题全部有了结论。** BioProt 的 pyproject.toml 声明 MIT;MedAgentBench refsol.py 的 Box 链接已失效(404);TheAgentCompany 的 partial credit formula 在医疗方向的唯一后继是 PhysicianBench（但它用了更简单的 binary pass@1 而非加权部分得分）。
6. **(2026-06-09 增补)出现了一个与我们设计"孪生"的现成 benchmark:χ-Bench。** χ-Bench(arXiv:2605.16679,actAVA + Eric Xing/Koyejo/Xiong 等)把我们 task6 的两条核心设计独立落地了:**① 三步造 case**(终态反向生成 + per-stage rubric → 人工走查当 GT → 多人审核)= 我们「分类→找点→合成 trace」;**② 复合验证器**(确定性合约 + 多票 LLM judge,AND 二值奖励)= 我们「硬校验 + AI 判官」分档。30 个前沿 agent 最佳仅 pass@1 28%、pass^3 无一过 20%、单 session 跌到 3.8%。详见 **C5**。它**佐证了我们方法的合理性**(这些不是我们独创,已有 published 先例),但它**无 graded human intervention 且是医疗运营(保险/审查)非 drug/materials discovery** —— 我们的独有 delta 仍成立。同批 cubox 收藏还带来药物侧数据源 **TrialPanorama**(详见 F1)。

---

## 第 1 部分 · 深读卡（14 张新增,按方向分组）

### ——————————————————————————
### A. 材料方向（3 张）
### ——————————————————————————

### A1. ⭐⭐⭐ AutoMat（材料方向唯一的 agent benchmark,带过程量）

**Paper / Code / Data Source:**
- arXiv:2605.00803 · https://github.com/JHU-CLSP/AutoMat · https://hf.co/datasets/jhu-clsp/AutoMat
- 主页: https://jhu-clsp.github.io/AutoMat

**Time / Team:** 2026-05, Johns Hopkins University CLSP (Ziyang Huang, Yi Cao, Ali K. Shargh 等; Daniel Khashabi, Benjamin Van Durme 共同指导)

**Field 1 · Data Scale:**
- 85 个来自真实材料科学论文的可量化声明（claims）
- 4 个子领域：统计/ML 方法、密度泛函理论(DFT)、分子动力学(MD)、离散位错动力学(DDD)
- 3 种复现类型（按难度递增）：from-artifact interpretation → from-artifact reproduction → from-paper reproduction（后者成功率接近 0）
- 5 种验证类型：标量匹配、比较/排序、结构/定性、范围/边界、图形复现
- **任务长度：端到端多步**,每个任务需要读论文 → 设置环境 → 在 HPC 上运行模拟(最长 72 小时) → 后处理 → 产出证据,是真正的长程任务

**Field 2 · GT Format:** 包含过程量。GT = **SME（高年资博士生/博后）撰写的逐步复现步骤** + 预期中间结果 + 参考代码。完整执行轨迹（日志、命令、中间输出、生成文件）被保存,可离线分析。

**Field 3 · How Process is Measured:**
- **LLM evaluator agent**（不是固定 prompt,而是可以导航 artifact 目录、检查文件的 agent）
- **5 维评分,每维 1-5 分**：Methodological Fidelity / Execution Competence / Result Accuracy / Completeness / Scientific Rigor
- 每个维度生成 2-5 个 claim-specific sub-dimensions
- **总分 5 分制**：5=完全复现, 4=大部分复现(成功阈值), 3=部分, 2=大部分失败, 1=失败
- **有部分得分** ✅
- **κ-calibration**: quadratic-weighted κ=0.69（substantial agreement）vs 40 个盲评 SME 评估,within-1-point accuracy=80%
- 6 类失败模式分类：Procedural Incompleteness / Methodological Deviation / Resource & Execution Failure / Overconfident Assessment / Circular Reasoning / Under-training

**Field 4 · Retrofit Path:**
- **R (Retrofit): Medium cost.** SME 撰写的逐步复现步骤已是有序步骤序列,可以从中提取 checkpoint。完整执行轨迹可回溯定义 checkpoint。但需要将 LLM-judge 的 holistic 评分拆解为 discrete pass/fail checkpoints。
- **B (Borrow):** 5 维评分 rubric / LLM evaluator agent 模式 / 3 种复现类型的难度梯度 / 6 类失败分类法 / judge calibration 协议(分层抽样+盲评+κ) / 任务打包格式 `(claim, paper, metadata, artifacts)` / 完整 prompt 公开(Appendix D)

**License:** ⚠️ 无 LICENSE 文件（GitHub 和论文均未声明）,需联系作者澄清

**Distance to Our Goal:** **Same quadrant** — 材料科学 + agent + 过程量 + 多步长程,是我们体系的直接近邻;缺少"人类介入分级"但最容易加。

---

### A2. LLM4Mat-Bench（最大规模材料 LLM 基准,纯最终答案）

**Paper / Code / Data Source:**
- arXiv:2411.00177 · https://github.com/vertaix/LLM4Mat-Bench

**Time / Team:** 2024-10, Princeton Vertaix + U Toronto (Andre Rubungo, Kangming Li 等; Adji Bousso Dieng); NeurIPS 2024 AI4Mat Workshop

**Field 1 · Data Scale:**
- ~1.9M 晶体结构-性质对,来自 10 个公开数据源(OQMD 964K, JARVIS-QETB 624K, GNoME 376K 等)
- 45 个不同性质,65 个 property-dataset 任务(60 回归 + 5 分类)
- 3 种输入模态：composition / CIF / crystal text description
- 固定 80/10/10 splits
- **任务长度：单步**（给定材料表示 → 预测数值性质）

**Field 2 · GT Format:** **纯最终答案。** GT = DFT 计算的单个数值(如带隙 eV、形成能、体模量)或二元分类标签。无过程/轨迹/中间计算。

**Field 3 · How Process is Measured:** N/A — 无过程量。回归用 MAD:MAE ratio,分类用 AUC。

**Field 4 · Retrofit Path:**
- **Unusable as agent benchmark.** 这是 LLM/ML 性质预测基准,评测的是模型能否从文本/CIF 预测数值,没有 agent loop、工具调用或多步推理。
- **B (Borrow):** 1.9M 结构-性质对可作为材料方向 agent 任务的 seed 数据源（agent 任务可以是"查询 Materials Project 后预测此材料的带隙"）;MAD:MAE 评分方法可用于回归型 checkpoint。

**License:** ⚠️ 无 LICENSE 文件（GitHub API 返回 null）;底层数据源各有自己的 license。

**Distance to Our Goal:** **Far-field** — ML 性质预测,非 agent,无过程量;但数据可作 seed。

---

### A3. Matbench Discovery（材料稳定性预测标准 ML 基准）

**Paper / Code / Data Source:**
- arXiv:2308.14920 · https://github.com/janosh/matbench-discovery · https://matbench-discovery.materialsproject.org

**Time / Team:** 2023-08, Cambridge + LBNL + UC Berkeley (Janosh Riebesell, Anubhav Jain, Kristin Persson 等); 在线排行榜持续更新

**Field 1 · Data Scale:**
- 训练集：Materials Project v2022.10.28,~154K 晶体（含弛豫轨迹、能量、力、应力、磁矩）
- 测试集：WBM dataset,**215,488 个 unique-prototype 结构**（元素替换 + DFT 弛豫生成）
- 正标签(稳定)：32,942/215,488（~15.3% prevalence）
- 已 benchmark 13+ 模型（EquiformerV2+DeNS → Voronoi RF）
- **任务：单一任务**——从未弛豫结构预测热力学稳定性(凸包距离)

**Field 2 · GT Format:** **纯最终答案。** GT = DFT PBE 形成能 + 凸包距离(二元：hull distance ≤ 0 = 稳定)。无过程量。

**Field 3 · How Process is Measured:** N/A。分类指标: F1(主排名)、DAF(发现加速因子)、Precision/Recall。回归指标: MAE/RMSE/R²。

**Field 4 · Retrofit Path:**
- **Unusable as agent benchmark.** ML 能量模型基准,评测 GNN / 通用势场 / 随机森林,不涉及 LLM 或 agent。
- **B (Borrow):** DAF(Discovery Acceleration Factor)概念——precision / prevalence,衡量模型筛选比随机快多少——可用于衡量 agent 的筛选效率。在线排行榜基础设施可参考。

**License:** **MIT** ✅

**Distance to Our Goal:** **Far-field** — 纯 ML 模型评测,非 agent,无过程量;DAF 概念可借。

---

### ——————————————————————————
### B. 药物发现过程量方向（3 张）
### ——————————————————————————

### B1. ⭐⭐⭐ Syntheseus（逆合成基准库,带真正过程量）

**Paper / Code / Data Source:**
- arXiv:2310.19796 · https://github.com/microsoft/syntheseus · Faraday Discussions (2024)

**Time / Team:** 2023-10, Microsoft Research (Krzysztof Maziarz, Austin Tripp, Marwin Segler 等)

**Field 1 · Data Scale:**
- 单步：USPTO-50K(~50K 反应,标准 split,~5K 测试分子)
- 多步：**190 个困难目标分子**(Retro* Hard set) + **23,081,629 个建筑块**(eMolecules)
- 搜索深度：MCTS 最大 20,Retro* 最大 10,每分子 10 分钟壁钟时间限制
- 8 个单步模型(Chemformer, GLN, LocalRetro 等) × 多种搜索算法(MCTS, Retro*, PDVN, BFS 等)
- **任务长度：含单步和多步**;多步路线可达 ~20 步反应深度

**Field 2 · GT Format:** 单步 GT = 已知反应(精确 SMILES 匹配);多步**无 GT 路线**——成功 = 找到任一完整路线(从目标到可购买建筑块)。包含过程量:
- **何时**找到第一条解(壁钟时间、模型调用次数)
- **多少**不重叠(多样)路线随时间累积
- 时间-到-首个解的分布(中位数、百分位)

**Field 3 · How Process is Measured:**
- **Top-k accuracy** (k=1,5,10,50) + MRR(单步)
- **Solve rate / time-to-first-solution curves**(多步,按壁钟时间和模型调用次数追踪）
- **Route diversity via packing number (#Circles)**——不重叠路线数,**单调**指标(找更多路线不会降低 diversity 分数)
- **无部分得分**：每分子 binary(solved / not solved)
- 路线质量**未自动评分**——论文明确指出需要专家化学家 A/B testing 或前向预测可行性模型

**Field 4 · Retrofit Path:**
- **R (Retrofit): Medium cost.** 190 个目标分子的多步搜索轨迹天然包含步骤序列(递归分解到建筑块)。可以为每个目标定义中间 checkpoint(如"找到第一个有效分解"、"路线深度 ≤ N"、"使用建筑块 ≤ 预算")。缺少的是路线质量的自动评分。
- **B (Borrow):** time-to-first-solution 曲线 + 单调 diversity 指标(packing number)是直接可借的过程量模板;best-practice enforcement（caching, dedup, canonicalization）可当标准化评测基础设施;模型无关 + 算法无关的接口设计。

**License:** **MIT** ✅ (Copyright Microsoft Corporation)

**Distance to Our Goal:** **Adjacent quadrant** — 逆合成是药物发现核心,有时间序列过程量,但不是 agent-level 的工具调用/求助轨迹;是算法级的搜索性能追踪。

---

### B2. SDDBench（合成可行性驱动的药物设计基准,多步 round-trip score）

**Paper / Code / Data Source:**
- OpenReview: https://openreview.net/pdf/17364749b85ea281d969a63610fedf8e8b4e2dc4.pdf
- Under review at ICLR 2025(匿名作者,无 arXiv,无公开代码）

**Time / Team:** 2024/2025, 匿名(双盲审稿中)

**Field 1 · Data Scale:**
- ~916K 清洗/去重反应(USPTO, Lowe 2014)
- 反应网络 → 107,354 个分子有提取的合成路线(107,154 train / 100 val / 100 test)
- SBDD 评测分子：10,000 个(100 蛋白 × 100 分子,CrossDocked 数据集)
- 逆合成规划最大深度 15
- 评测 7 个 SBDD 模型
- **任务长度：内在多步**（逆合成规划 → 前向反应预测 → 相似度计算）

**Field 2 · GT Format:** 包含过程量（内在的）。round-trip score `S(m) = Sim(m, f(g(m)))` 是三阶段流水线：
1. 逆合成规划 g(m)：预测合成路线(树状反应序列)
2. 前向复现 f(·)：从叶节点出发材料逐步前向预测,重构目标分子 m'
3. Tanimoto 相似度：比较 m 与 m'

每个数据点含目标分子及其所有关联合成路线作为 GT。

**Field 3 · How Process is Measured:**
- **Round-trip score**：连续值 [0,1],1.0 = 前向模型完美重构(路线可行的强证据)
- 实际使用中以阈值 1.0 二值化
- 精度 81.5%, 召回率 64.7%, F1 72.0%
- 误差在任何步骤都会累积,使其**真正对过程敏感**
- 但最终输出是**每分子一个标量**,不是轨迹级指标

**Field 4 · Retrofit Path:**
- **B (Borrow):** round-trip score 思路（逆合成→前向→比较）可直接用于我们评测 agent 产出的合成路线的可行性;multi-step process-sensitivity 是天然的过程度量模板。
- **Unusable for direct retrofit:** 仍在匿名审稿,无公开代码/数据,无法 fork 或重发布。若后续公开,retrofit cost = Low(路线 GT 已有）。

**License:** ⚠️ 无(匿名审稿中,无公开代码)

**Distance to Our Goal:** **Adjacent quadrant** — 药物合成可行性 + 多步过程量;但是分子级度量非 agent 轨迹级,且当前不可获取。

---

### B3. SEISMO / PMO（轨迹级 AUC 分子优化基准）

**Paper / Code / Data Source:**
- SEISMO: arXiv:2602.00663 · https://github.com/FabianKruger/molecule-optimization-agent
- PMO 基准: arXiv:2206.12411 · https://github.com/wenhao-gao/mol_opt (MIT, 233 stars)

**Time / Team:** SEISMO: 2026-01, Helmholtz Munich / TU Munich (Fabian Krüger, Andrea Hunklinger 等); PMO: 2022, MIT/GT (Wenhao Gao, Connor Coley)

**Field 1 · Data Scale:**
- PMO: **23 个单目标标量优化任务**(QED, DRD2, GSK3β, JNK3, 19 个 Guacamol oracle),oracle score 归一化到 [0,1]
- 标准 oracle 预算：10,000 次调用(SEISMO 只用 50 次)
- 数据要求：仅 ZINC 250K
- SEISMO 评测了 REINVENT, Graph-GA, Augmented Memory, MolCRAFT, DrugAgent 等基线
- **任务长度：多步序列优化**（每次 oracle 调用后根据全部历史提出下一个分子）

**Field 2 · GT Format:** 包含过程量（轨迹级）。核心指标：
- **优化曲线** `s_t = max_{i≤t} F(m_i)`,追踪到第 t 次 oracle 调用时找到的最好分子的 score
- **AUC** = 曲线下面积 / oracle 预算,值域 [0,1]
- **两个达到相同最终分数但速度不同的方法 AUC 不同** → 这是真正的轨迹级指标

**Field 3 · How Process is Measured:**
- AUC Top-K（K=10 in PMO, K=1 in SEISMO）
- SEISMO Sum Top-1 AUC(50 calls) = 19.85 vs best baseline 7.06 (2.8x)
- 轨迹感知设计：每次 oracle 调用后,LLM 的上下文包含所有先前 (molecule, score) 对
- SEISMO 还评测了 explanatory feedback (SHAP attributions) 对效率的提升
- **无 partial credit 概念**（AUC 本身是连续值）

**Field 4 · Retrofit Path:**
- **B (Borrow):** AUC 优化曲线模板——"两个方法到达相同最终分数但 AUC 不同"这一洞察直接可用于我们的 agent 评测（agent A 在第 3 步找到正确中间物 vs agent B 在第 10 步找到 → AUC 不同）;trajectory conditioning 设计模式（将全部历史放入 LLM context）可借。PMO 的 23 个 oracle 任务可作为药物方向 agent 任务的 seed。
- **Unusable for direct data retrofit:** PMO 任务是优化 oracle,不是"科学任务"——没有论文背景、没有实验设计、没有逆合成路线。

**License:** **MIT** ✅ (两个仓库均为 MIT)

**Distance to Our Goal:** **Adjacent quadrant** — 轨迹级度量 + 药物相关 oracle;但是优化 sample efficiency 度量,非 agent 工具调用/求助/介入度量。

---

### ——————————————————————————
### C. 医疗/生物 2025–2026 新发（4 张）
### ——————————————————————————

### C1. ⭐⭐⭐ PhysicianBench（医疗方向里程碑——670 checkpoint 的 EHR agent 基准）

**Paper / Code / Data Source:**
- arXiv:2605.02240 · https://github.com/HealthRex/PhysicianBench · https://healthrex.github.io/PhysicianBench/

**Time / Team:** 2026-05, Stanford HealthRex (Ruoqi Liu, Jonathan H. Chen 等; 11 名医生标注者)

**Field 1 · Data Scale:**
- **100 个长程任务**,改编自真实一级护理-专科会诊病例
- **670 个结构化 checkpoint**
- 21 个亚专科,组织为 8 大专科组(心脏、内分泌、消化、免疫/感染、精神/神经、血液/肿瘤、肾泌、肺/其他)
- 4 种工作流类型 × 14 种细分子类型(Diagnosis & Interpretation 13, Medication Prescribing 26, Treatment Planning 27, Workup & Risk Stratification 34)
- **平均每任务 27 个工具调用**
- EHR 环境：FHIR R4 标准,HAPI FHIR JPA Server,**来自 STARR（Stanford 真实脱敏临床数据仓库）**的患者记录(加扰动)
- 14 种工具：9 个 GET(Patient, Condition, Observation 等) + 4 个 POST(MedicationRequest, ServiceRequest 等) + 1 个 write_file
- **任务长度：端到端长程**

**Field 2 · GT Format:** **包含过程量** ✅。4 类 checkpoint 分类镜像临床决策流水线：
1. **Data Retrieval** — agent 是否查询了正确的 FHIR 资源并提取了关键数据点
2. **Clinical Reasoning** — 4 个子类型：计算临床评分、解释/综合发现、做出临床判断、应用临床标准
3. **Action Execution** — agent 是否创建了适当的 FHIR 资源(MedicationRequest, ServiceRequest 等)
4. **Documentation** — 书面输出是否完整、临床合理、遵循指南、无不安全建议

**Field 3 · How Process is Measured:** 3 种 grader 类型：
- **Code graders**（确定性 Python）: 查询 FHIR server 验证 agent 创建的资源,检查类型/状态/编码值/剂量参数,日期过滤区分 agent 创建 vs 预存数据
- **Hybrid graders**（确定性 + LLM extraction）: GT 从 FHIR 数据确定性计算(如 CHA₂DS₂-VASc),LLM 从 agent 自由文本中提取值,然后确定性比较
- **LLM-judge graders**: 含结构化 rubric,**三级评分：pass / partial / fail**
- **主指标：Pass@1**（binary,所有 checkpoint 通过才算成功）
- 还报告 Pass@3, Pass^3(一致性),#Turns(效率)
- **注意：主指标是 binary,但 checkpoint 级粒度在错误分析中使用**——checkpoint taxonomy 用于诊断而非产出连续分数

**Field 4 · Retrofit Path:**
- **R (Retrofit): Low cost.** 670 个 checkpoint 已是结构化的,checkpoint 分类已有(4 类)。只需要：(a) 把 LLM-judge 的 pass/partial/fail 聚合为加权部分得分(类似 TheAgentCompany 公式);(b) 加入"agent 主动求助"checkpoint(类似 TAC 的 Collaboration 类别)。EHR 环境已 Docker 化,API 标准化。**这是我们发现的最低成本 retrofit 目标。**
- **B (Borrow):** 4 类 checkpoint taxonomy / 3 种 grader 类型 / FHIR-based 执行验证 / 从真实会诊病例构建任务的协议 / 医生多轮标注流水线

**License:** **Apache-2.0** ✅

**Distance to Our Goal:** **Same quadrant** — 医疗 agent + 结构化 checkpoint + execution-grounded verification + 长程任务。唯一差距：主指标是 binary pass@1 而非加权部分得分,且无"人类介入分级"。

---

### C2. ⭐⭐ BioAgent Bench（带过程量的生物信息学 agent 基准,ICML 2026）

**Paper / Code / Data Source:**
- arXiv:2601.21800 · https://github.com/bioagent-bench/bioagent-bench

**Time / Team:** 2026-01 (v3 2026-05), Croatian team (Dionizije Fa, Marko Culjak 等; EU NextGenerationEU 资助); **ICML 2026**

**Field 1 · Data Scale:**
- **10 个端到端生物信息学流水线任务**,7 个领域(bulk RNA-seq, single-cell RNA-seq, comparative genomics, variant calling, metagenomics 等)
- 每任务需链接多个命令行生物信息工具,运行时 < 4h, ≤ 48GB RAM
- 4 个任务有 binary verifiable GT(cystic-fibrosis, giab, transcript-quant, viral-metagenomics),6 个允许多条有效路径
- **任务长度：端到端多步流水线**

**Field 2 · GT Format:** **包含过程量** ✅。LLM grader 产出 5 个字段：
- `steps_completed`: agent 可证明完成的流水线步骤数
- `steps_to_completion`: 估计完成任务所需的总步骤数
- `final_result_reached`: boolean
- `results_match`: 任务特定正确性标志
- `f1_score`: F1(仅 variant calling 任务)
- **主指标：Completion rate (%)** = steps_completed / steps_to_completion

**Field 3 · How Process is Measured:**
- **LLM-judge (GPT-5.1)**: 接收输入/参考数据路径、预期结果(CSV/TSV GT)、agent 结果、agent 轨迹(文件夹/文件路径)
- **有部分得分** ✅: steps_completed / steps_to_completion 给出分数进度
- **无 κ-calibration**: 论文承认 LLM 评分依赖 rubric 措辞和 trace 详细度
- 鲁棒性：Jaccard Index (mean 0.43) for categorical, Pearson (mean 0.73) for numerical, 4 次重复试验

**Field 4 · Retrofit Path:**
- **R (Retrofit): Low-Medium cost.** 任务已是多步流水线,steps_completed 已跟踪进度,完整轨迹(messages, tool calls, intermediate steps)已记录。可以：(a) 定义 per-step checkpoint 而非只用 LLM 估计步骤数;(b) 加入确定性 Python grader 替换部分 LLM-judge;(c) 加入"agent 主动求助"checkpoint。
- **B (Borrow):** steps_completed / steps_to_completion 的进度度量模式;LLM grader 的 rubric 设计;多条有效路径的处理方式。

**License:** ⚠️ 无 LICENSE 文件

**Distance to Our Goal:** **Adjacent quadrant** — 生物信息学 agent + 过程量 + 多步;但规模小(10 任务),LLM-judge 而非确定性评分,非药物/材料方向。

---

### C3. BioXArena（76 个 BioML 任务,最终输出但承诺释放轨迹）

**Paper / Code / Data Source:**
- arXiv:2605.15766 · https://github.com/mbzuai-ai4bio/BioXArena

**Time / Team:** 2026-05, MBZUAI + 中科院 + CMU (Loka Li, Le Song 等); 预印本(69 页)

**Field 1 · Data Scale:**
- **76 个端到端任务**,9 个领域(sequence, single-cell, structure, network biology, chemical biology, perturbation dynamics, phenotype-disease, imaging, text-integrated)
- 60.5% 多模态,92.1% 多源
- 104.08 GiB 数据;来自 40+ primary biomedical sources
- 11 种 agent 配置在 2h 单 GPU 环境中评测
- **任务长度：端到端 ML 模型构建**（数据加载 → 特征工程 → 模型训练 → 预测提交）

**Field 2 · GT Format:** **纯最终答案**,但记录丰富元数据。评测 submission.csv 预测结果 vs 隐藏测试标签。指标: Pearson/Spearman/ROC-AUC/AUPRC/Accuracy/Macro-F1/C-index/Mean Dice/TM-score 等,归一化到 [0,1]。
- 记录但**不评分**：submission status, evaluator success, raw metric, normalized score, runtime, token usage, API-cost

**Field 3 · How Process is Measured:**
- **确定性 Python graders**(sklearn 指标函数)
- **无部分得分**：未产出有效 submission.csv = 0 分(836 次 agent-task 运行中 111 次评测失败)
- 3 次运行鲁棒性分析(GPT-5.4: 63.7±0.1)

**Field 4 · Retrofit Path:**
- **R (Retrofit): Medium cost.** 承诺释放 agent traces(代码生成尝试、执行输出、错误、修复迭代)。一旦可用,可回溯加过程量评分(尝试次数、错误恢复模式、代码质量等)。当前纯输出评测。
- **B (Borrow):** 9 领域分类法;确定性 Python graders;归一化到 [0,1] 统一评分框架;76 个任务可作 bio agent seed。

**License:** ⚠️ 无 LICENSE 文件; Ethics statement 声称 "derived from public, de-identified, or controlled-access resources whose terms permit academic non-commercial benchmark use"

**Distance to Our Goal:** **Adjacent quadrant** — Bio agent + 大规模 + 9 领域;但纯最终输出,缺过程量;待轨迹释放后可 retrofit。

---

### C4. MedHELM（医疗 LLM 综合评测框架,可借 LLM-jury 方法论）

**Paper / Code / Data Source:**
- arXiv:2505.23802 · https://github.com/stanford-crfm/helm (Apache-2.0) · https://crfm.stanford.edu/helm/medhelm/

**Time / Team:** 2025-05, Stanford CRFM + School of Medicine + Microsoft (Nigam Shah, Percy Liang 等; 29 名临床医生参与)

**Field 1 · Data Scale:**
- 5 类别, 22 子类别, 121 任务的临床医生验证分类法
- **35 个 benchmark**(17 existing, 5 reformulated, 13 new; 12 个 EHR-based)
- 14 公开, 7 gated, 14 private
- 9 个前沿 LLM 评测

**Field 2 · GT Format:** **混合**。22 个 closed-ended benchmark = 最终答案(exact match/accuracy/F1);13 个 open-ended benchmark = 自由文本 + rubric-based LLM-jury。**不评 agent 轨迹或工具调用。**

**Field 3 · How Process is Measured:**
- Closed-ended: exact match, MedCalc accuracy, EHRSQL execution accuracy
- Open-ended: **LLM-jury** — 3 judges (GPT-4o, Claude 3.7 Sonnet, LLaMA 3.3 70B), 3 axes (Accuracy, Completeness, Clarity), 1-5 Likert
- ICC = 0.47, 超过平均临床医生间一致性 (ICC = 0.43)

**Field 4 · Retrofit Path:**
- **B (Borrow):** LLM-jury 方法论(3 diverse providers × 3 axes, validated vs clinician agreement);临床医生验证的 121 任务分类法;cost-performance 分析框架。
- **Unusable as agent benchmark:** 纯 LLM 评测,无 agent loop/tool use/trajectory。

**License:** **Apache-2.0** ✅ (HELM 框架)

**Distance to Our Goal:** **Far-field** — LLM 评测非 agent;但 LLM-jury 和分类法可借。

---

### C5. ⭐⭐⭐ χ-Bench / CHI-Bench（医疗运营长程 agent,与我们设计"孪生")

> 📥 **来源:用户 Cubox `medical-benchmark` 收藏(知乎魔搭社区 + 小红书,2026-06-09)。** 非本轮 deep-research 产出。**arXiv 核心数据已一手核对**(2605.16679v2);验证器内部机制(复合验证器、三步造 case、失败分类)为知乎 digest 转述,🟡 论文正文未亲核,⏳ 列 P0 待补。

**Paper / Code / Data Source:**
- arXiv:2605.16679 · https://github.com/actava-ai/chi-bench · https://huggingface.co/datasets/actava/chi-bench · 排行榜 https://actava.ai/benchmarks

**Time / Team:** 2026-05, actAVA.ai + Johns Hopkins / Stanford / CMU / 牛津 等(Haolin Chen 等;Eric P. Xing, Sanmi Koyejo, Caiming Xiong, Kun Zhang, Philip S. Yu 共同参与)

**Field 1 · Data Scale:**(arXiv 一手)
- **75 个真实长程任务**,3 域:provider prior authorization (PA) / payer utilization management (UM) / care management (CM)
- 每任务跨 **4–6 阶段、~60–80 步**(知乎),不可逆 handoff
- 高保真模拟器:**20 个医疗 App,经 87 个 MCP 工具暴露**(⚠️ 知乎帖写"21 App / 200+ 工具",**以 arXiv 为准**)
- **1,290+ 文档**的 managed-care operations handbook(以渐进披露的 Skill 形式组织,顶层按角色路由)
- **任务长度:端到端长程**

**Field 2 · GT Format:** **包含过程量** ✅。三步流水线造 case(知乎 digest):Step1 从终态反向生成 chart 规格 + **锚定政策条文的 per-stage rubric**(Claude Opus 4.7 结构化采样)→ Step2 标注员在真实 χ-World UI 端到端走查,录 trajectory/DB 状态/handoff 当 ground truth → Step3 ≥1 临床工作者 + 5 作者审核 + 残留 PHI 扫描。从 523 候选筛出 75。

**Field 3 · How Process is Measured:**
- **复合验证器**(知乎 digest):**确定性合约(deterministic contract)+ 多票 LLM judge(pinned Claude Opus 4.7,严格多数投票)**,两层 AND 输出二值奖励;可全量审计世界状态 / event log / 多轮对话
- 指标:pass@1 / pass@3 / **pass^3**(同任务 3 次全过)
- 结果(arXiv 一手):30 个 agent harness/model 配置,**最佳 28.0%**;pass^3 **无一过 20%**;单 session 跑全部任务跌到 **3.8%**;端到端双 agent(provider–payer)协作 0%
- 失败两级归因(知乎):临床推理误判 35.4% / 跳必需步 23.3% / 死循环超时 15.6% / 政策字面误读 13.2% / 工具格式错 10.7%;CM 域特有 **illegitimate consent**(反复劝说直到患者改口同意,违反 autonomy-first)→ 说明"完成度"单一指标不足以衡量安全

**Field 4 · Retrofit Path:**
- **B (Borrow): 高价值** —— 它把我们 task6 的两条核心设计独立落地了:(a) **三步造 case = 我们「分类→找点→合成 trace」**;(b) **复合验证器(确定性合约 + 多票 LLM judge)= 我们「硬校验 + AI 判官」分档**。可直接借:per-stage rubric 锚定规则条文、复合验证器结构、**pass^3 可靠性指标**、失败两级分类法、illegitimate-consent 这类"安全 cp"。
- **R (Retrofit): 待评估** —— 开源带数据集(HF / ModelScope),但任务是医疗运营(保险授权/利用率审查/护理管理),**非药物/材料 discovery**;宜作"过程量评测工程范式"参考,数据本身不直接 retrofit 进我们的科学发现题。

**License:** 代码 Apache-2.0(知乎称)· ⏳ repo/dataset 待亲核

**Distance to Our Goal:** **Same quadrant(方法论孪生)** —— 医疗运营 agent + 结构化 per-stage checkpoint + 复合验证器 + 长程不可逆。**这是目前找到的与我们设计最像的现成 benchmark**。关键差距:**① 无 graded human intervention(L0–L3)**;**② 域是医疗运营非 drug/materials discovery** —— 我们的独有 delta(分级介入 + 药物/材料发现 + 可复现)仍然成立。**重要含义:复合验证器与 per-stage rubric 不再是我们的"独创",已有 published 先例 → 论文里这两点改为"采用业界已验证范式"而非"首创",把首创 claim 收敛到"分级介入 + AI4S discovery + 可复现"。**

---

### ——————————————————————————
### D. 过程量方法论（2 张,借方法不借数据）
### ——————————————————————————

### D1. ⭐⭐⭐ AgentBoard（progress rate 指标,NeurIPS 2024 Oral）

**Paper / Code / Data Source:**
- arXiv:2401.13178 · NeurIPS 2024 Oral, Datasets & Benchmarks Track

**Time / Team:** 2024-01, HKU + SJTU + Tsinghua (Chang Ma, Junxian He 等)

**Field 1 · Data Scale:**
- **9 个任务** × 4 类别(Embodied AI: AlfWorld/ScienceWorld/BabyAI; Game: Jericho/PDDL; Web: WebShop/WebArena; Tool: Tool-Query/Tool-Operation)
- **1013 个示例环境**,每个含人工标注的子目标序列
- 多轮交互,部分可观测环境

**Field 2 · GT Format:** 每个任务实例有 goal g 及其分解为 **K 个子目标序列** [g₁, ..., g_K]。每个子目标 g_i 关联标记状态,用**正则匹配函数** f(·, g_i) → {0,1} 判断是否完成。

**Field 3 · How Process is Measured:**
- **Progress Rate** r_t = max_{i≤t}(1/K · Σ f(s_i, g_k)):
  - 子目标完成分数,在历史上取 max → **单调递增**
  - 连续匹配变体 r_t^match 用于状态相似度任务
  - 初始 0,完成 1
- **人工验证**: Pearson ρ > 0.95 with human progress judgments, Fleiss' κ substantial agreement
- 分析维度: grounding accuracy, hard/easy breakdown, sub-skill scoring (memory, planning, grounding, world modeling, self-reflection, spatial navigation)

**Field 4 · Retrofit Path:**
- **B (Borrow):** Progress rate 公式直接可借——把科学任务目标拆为有序子目标序列,每个子目标有 binary 匹配函数,进度 = 完成分数(在历史上取 max 保证单调)。dual-mode(连续匹配 vs 子目标 binary)模式适配定量任务(对接分数) vs 程序任务(完成步骤)。POMDP 形式化框架。人工验证协议(60 轨迹 × 3 模型 × 4 评分者)。
- **Unusable for data:** 9 个任务是通用 agent 任务,非 AI4S。

**License:** Code: **Apache-2.0** ✅; Data: **GPL-2.0**

**Distance to Our Goal:** **Adjacent quadrant (methodology)** — 通用 agent 过程量评测方法论,直接可移植到 AI4S;数据本身不可用。

---

### D2. ⭐⭐⭐ TRACE（轨迹感知综合评测框架,WWW 2026）

**Paper / Code / Data Source:**
- arXiv:2602.21230 · WWW 2026 (ACM Web Conference)

**Time / Team:** 2026-02, CUHK (Yanyu Chen, Irwin King 等)

**Field 1 · Data Scale:**
- **DeepResearch-Bench**: 650 个任务(TRACE-Core 500 + TRACE-Robustness 100 + TRACE-Scaffolding 50)
- 每个任务含 oracle trajectory (H_gt) + 战略性嵌入的"信息陷阱"
- 可控复杂度
- 还评测 BrowseComp-en (123 tasks) 和 GAIA text-only (103 tasks)

**Field 2 · GT Format:** 每个任务有 GT answer A_gt + **oracle trajectory H_gt**。GT format 包含过程量——oracle trajectory 可分割为不同比例的提示(用于 scaffolded capability assessment)。

**Field 3 · How Process is Measured:**

**Hierarchical Trajectory Utility U(H)** = I(correct) · E(H)^w_E · C(H)^w_C:
- **I(·)**: 正确性门控——答案错 = 0 分
- **E(H) = Process Efficiency**: R_C(T) / J(H),含 Redundant Exploration Penalty (REP) 和 Marginal Information Gain (MIG)
  - MIG g_t = max(0, cos(Φ(o_t), Φ(A_gt)) - sup_{i<t} cos(Φ(o_i), Φ(A_gt)))
  - REP 惩罚连续无信息动作
- **C(H) = Cognitive Quality**: β·G_E + (1-β)·R_R
  - G_E = Evidence Grounding: NLI 检查每个 atomic claim 是否有证据支持(几何平均 → 单个无根据声明毁全分)
  - R_R = Reasoning Robustness: 从信息陷阱恢复的延迟指数衰减
- **Scaffolded Capability Assessment (λ_min)**:
  - 给 agent oracle trajectory 的前 λ 比例作为提示
  - 找到使成功率 ≥ θ_succ 的最小 λ
  - λ_min 越低 = agent 内在能力越强（类比 Vygotsky 的"最近发展区"）

**Field 4 · Retrofit Path:**
- **B (Borrow):** 极高借鉴价值——
  - Hierarchical Utility Function 架构(正确性门控 × 效率 × 质量的乘法结构)
  - MIG 概念(每步动作是否带来新的相关信息 → 适用于文献搜索、数据库查询、计算实验)
  - REP 概念(惩罚 agent "空转" → 检测重复运行相同对接配置)
  - Evidence Grounding via NLI 分解(科学声明必须有证据支持)
  - 信息陷阱 + 恢复延迟(检测 agent 遇到矛盾数据/撤回论文/混淆实验结果后的恢复能力)
  - Scaffolded Capability Assessment(给部分实验协议/中间结果作为提示,衡量 agent 需要多少引导)
  - 几何平均惩罚模式(单个灾难性失败主导总分 → 适用于科学领域的幻觉检测)

**License:** CC license (paper metadata); 代码/benchmark 可用性未明确

**Distance to Our Goal:** **Adjacent quadrant (methodology)** — 通用 deep research agent 过程量评测方法论;Scaffolded Capability Assessment 几乎就是我们"人类介入分级"的理论化身。

---

### ——————————————————————————
### E. 补充卡（2 张,借方法论）
### ——————————————————————————

### E1. HealthBench Professional（医生 rubric 设计标杆）

**Paper / Code / Data Source:**
- arXiv:2604.27470 · 数据: https://openaipublic.blob.core.windows.net/simple-evals/healthbench_professional/assets.zip

**Time / Team:** 2026-04, OpenAI (Rebecca Soskin Hicks 等; 190 名医生,50 国,28 专科)

**Field 1 · Data Scale:** 525 个任务(从 15,079 候选中筛选),3 use cases(care consult, writing/documentation, medical research);~1/3 red teaming(对抗);difficulty enriched ~3.5x。

**Field 2 · GT Format:** **无最终答案 GT**。每个示例有**医生撰写的 rubric criteria**(分值 -10 到 +10,负分惩罚有害行为)。评测系统输出(含工具使用的 ChatGPT for Clinicians),但**不评工具使用轨迹本身**——黑盒评最终响应。

**Field 3 · How Process is Measured:** Model-based grader (GPT-5.4 low reasoning) 逐条评 rubric,s_i = Σ(p_c · 1[c met]) / Σ(p_c for p_c>0),length-adjusted,clip [0,1]。配对 t 检验 + Holm 校正。人类基线：specialist-matched physician responses。

**Field 4 · Retrofit Path:**
- **B (Borrow):** 3-stage physician rubric design methodology (author → review → adjudicate, 3+ physicians per example);length adjustment methodology;difficulty stratification via Likert + adversarial sampling;system vs model evaluation distinction;red teaming taxonomy。
- **Unusable as agent benchmark:** 评最终响应,非 agent 轨迹。

**License:** 数据公开发布,但"We request that you do not reveal examples in plain text online"(防污染);无显式 license 名称。

**Distance to Our Goal:** **Far-field** — LLM 系统评测;但 physician rubric 设计方法论是构建医疗 checkpoint 的金标准参考。

---

### E2. MEDIC（临床 LLM 安全-效用评测,含 CEF 参考免方法）

**Paper / Code / Data Source:**
- arXiv:2409.07314 · https://huggingface.co/spaces/m42-health/MEDIC-Benchmark

**Time / Team:** 2024-09 (v2 2026-01), M42 Health, Abu Dhabi (Praveenkumar Kanithi 等)

**Field 1 · Data Scale:** 5 评测维度, ~18 个 benchmark/dataset, 14+ 模型, ~31,000 pairwise comparisons。

**Field 2 · GT Format:** **混合**: 静态知识 = final answer accuracy; 功能任务(EHRSQL, MedCalc) = 执行精度; 生成任务 = Cross-Examination Framework (CEF); 开放式 = pairwise Elo; 安全 = refusal rate + error detection accuracy (multi-stage)。

**Field 3 · How Process is Measured:**
- **CEF (Cross-Examination Framework)**: 参考免评测——从源文本生成 boolean 验证问题,然后查询模型输出验证:Coverage / Conformity / Consistency / Conciseness。CEF 与 ROUGE/BLEU/BERTScore 近零相关。
- **MEDEC 三阶段错误评测**: detect → localize → correct
- **知识-执行差距**: 静态 MCQ 不预测功能执行表现
- **被动安全 vs 主动安全**: 高 refusal rate ≠ 能检测文档错误
- inter-judge Spearman ρ > 0.98

**Field 4 · Retrofit Path:**
- **B (Borrow):** CEF 是参考免生成评测方法,直接可用于评测 agent 生成的科学报告(无需 gold output);知识-执行差距分析方法论;被动/主动安全区分;MEDEC 三阶段错误评测管线。
- **Unusable as agent benchmark:** LLM 评测,含 proto-agentic 功能任务但非多步 agent。

**License:** HuggingFace leaderboard 公开; 无显式 framework license。

**Distance to Our Goal:** **Far-field** — LLM 评测;CEF 和安全分析方法论可借。

---

### ——————————————————————————
### F. cubox / 知乎收藏补充（2026-06-09,来源:用户 Cubox `medical-benchmark` 标签）
### ——————————————————————————

> 用户 Cubox `medical-benchmark` 标签 16 篇(知乎正文 Cubox 解析不出,用 `work/.claude/skills/zhihu-fetch` 按 URL 补全),去重约 13 个 benchmark。其中 **χ-Bench 已升为 C5 深读卡**(设计孪生)。**TrialPanorama** 是药物/临床试验侧新数据源,单列(F1);其余多为终答型 QA 或已在 task4 覆盖,列去重表(F2)。

### F1. TrialPanorama（药物/临床试验侧数据源,终答型 benchmark）

> 📥 来源:Cubox/小红书(UIUC 智疗研究所帖)。**arXiv 已一手核对**(2505.16097v2)。

**Paper:** arXiv:2505.16097(v2 2025-12)· UIUC + NIH(Zifeng Wang, Jiacheng Lin, Zhiyong Lu, Jimeng Sun 等)

**是什么(arXiv 一手):** 聚合 **15 个全球注册库的 1.6M 条临床试验记录**,关联生物医学本体与文献;构建 **152K 训练/测试样本**,覆盖 **8 个任务**——系统综述 3(study search / study screening / evidence summarization)+ 试验设计 5(arm design / eligibility criteria design / endpoint selection / sample size estimation / trial completion assessment)。通用 LLM 在临床推理上能力有限;作者基于它训的 8B 模型(SFT+RL)在 8 任务全面超过 70B 通用模型。

**GT Format:** 纯最终答案(LLM benchmark,**非 agent 轨迹,无过程量**)。

**对我们:** **B / seed —— 药物侧目前最缺的临床试验数据源。** 8 个任务可作药物 agent 任务 seed(如"给定适应症设计入排标准 → 我们再加 per-step checkpoint");"试验完成评估 / 样本量估算"是天然的长程子任务雏形。**不可直接 retrofit**(终答型,需自加 checkpoint)。

**Distance to Our Goal:** **Adjacent(数据源)** —— 药物/临床试验,大规模,但非 agent、无过程量。

### F2. 其余 cubox 条目（去重处置）

| 条目 | 是什么 | 处置 |
|---|---|---|
| **AgentClinic** | 多 agent 临床模拟(患者/医生/测量/调解),9 专科 7 语言,顺序决策+工具,USMLE+MIMIC-IV+NEJM | 已是 task4 baseline(`inventory.md` [^18]);知乎全文确认"顺序决策格式使 MedQA 准确率掉到原 1/10",可作"为什么必须评过程量"的论据 |
| **MedAgentBench** | Stanford,300 任务 FHIR 虚拟 EHR,Claude3.5 最佳 69.67%,**50%+ 失败是格式不合规** | 已是 task4 baseline([^17]);印证"结构/语法硬校验"形态 |
| **AI Hospital / MVME** | 多 agent 医患模拟,按症状采集/检查推荐/诊断三视图打分(ACL/COLING 2025) | 新增;有中间环节打分雏形,价值中等,暂不建卡 |
| **MedAgentsBench** | arXiv:2503.07459,7 数据集多步临床推理,DeepSeek R1/o3 领先 | 新增;终答型推理,论文 baseline 备选 |
| **CLIMB** | MIT/Harvard/Stanford,451 万患者 / 19TB 多模态临床(github DDVD233/climb) | 新增;多模态数据源,非 agent,与我们 drug/materials 弱相关 |
| **MedEvalKit** | 阿里达摩 Lingshu 统一医疗评测框架(arXiv:2506.07044) | 新增;可借统一 runner 结构,归 B |
| **DAS red-teaming** | arXiv:2508.00923,动态/自动/系统化红队,4 安全轴 | 新增;安全评测方法可借(对接 χ-Bench 的"安全 cp") |
| **MedXpertQA** | 专家级医疗推理 QA,最新模型 text 子集 <70 | 新增;终答型难度池 |
| 医疗 LLM benchmark 综述 | arXiv:2410.21348,MIMIC/BioASQ/PubMedQA/CheXpert 盘点 | 新增;综述,索引用 |

---

## 第 2 部分 · Task 4 §7 开放问题的回答

### Q1. BioProt 数据 License 能否重发布？

**结论：事实上可以(MIT),但存在文档缺口。**

- GitHub 仓库 `bioplanner/bioplanner` 根目录**无 LICENSE 文件**（GitHub API 返回 `license: null`）
- 但 **`pyproject.toml` 显式声明 `license = "MIT"`** — 这在 Python 打包生态中具有法律意义
- Issue #3（"Add repository license",2024-06-21 提交,倾向 MIT）于 **2026-02-08 被关闭**,无可见评论。关闭暗示维护者认为已解决(pyproject.toml 声明先于 issue)
- 未发现 Samuel Rodriques / FutureHouse 的公开声明
- 所有 4 个 fork 均 `license: None`
- arXiv 论文 CC-BY 4.0 仅覆盖论文文本,不覆盖代码/数据

**操作建议：** pyproject.toml 的 MIT 声明给出了合理的法律基础。但最佳实践是**联系作者请求添加正式 LICENSE 文件**,尤其在商业使用前。**可以在学术范围内使用,风险较低。**

### Q2. MedAgentBench refsol.py 访问条件

**结论：MIT 许可,但 Box 链接已失效(404)。无 IRB/DUA 要求。**

- 仓库 **MIT 许可** (Copyright 2025 Stanford Machine Learning Group)
- README 指引用户从 Stanford Box 下载 `refsol.py`: `https://stanfordmedicine.box.com/s/fizv0unyjgkb1r3a83rfn5p3dc673uho`
- **该 Box 链接返回 HTTP 404** — 文件已不可访问
- **无 DUA、IRB 批准或数据使用协议** — refsol.py 曾作为简单公开下载链接提供,无访问控制
- 底层患者数据来自 Docker 镜像中的 **Synthea 合成**患者数据 — 无真实患者数据,无 IRB 要求
- Issue #1：HuggingFace 提出托管数据集,作者未回应
- Issue #6：外部团队使用 Gemini 3 Flash + Claude Haiku 4.5 实现 100% 成功率 — 确认基准被外部使用

**操作建议：** 需要联系作者(Yixing Jiang 等 Stanford)获取可用链接,或检查是否已迁移。

### Q3. 2025–2026 未浮出的医疗/临床 benchmark

**已在 Angle 3 覆盖。关键新发现：**

| Benchmark | 类型 | 过程量? | 状态 |
|---|---|---|---|
| **PhysicianBench** (2605.02240) | EHR agent, 670 checkpoints | ✅ | 最重要新发现 |
| **BioAgent Bench** (2601.21800) | Bioinformatics agent, 10 tasks | ✅ | ICML 2026 |
| **BioXArena** (2605.15766) | BioML agent, 76 tasks | ❌ (承诺释放轨迹) | 预印本 |
| **MedHELM** (2505.23802) | Medical LLM eval, 35 benchmarks | ❌ | Stanford |
| **HealthBench Professional** (2604.27470) | Clinical LLM eval, 525 tasks | ❌ | OpenAI |
| **MEDIC** (2409.07314) | Clinical LLM safety eval | ❌ | M42 Health |
| **AgentMD** (2402.13225) | Clinical calculator agent, 2164 tools | ❌ | NIH/NLM |
| **EHRAgent** (2401.07128) | EHR tabular reasoning agent | ❌ | EMNLP 2024 |

**Task 4 中确实遗漏了 PhysicianBench 和 BioAgent Bench** — 这两个是 2026 年初发布的,任务 4 运行时(2026-06-03)可能尚未出现在搜索结果中。PhysicianBench 是该方向的重大进展。

### Q4. TheAgentCompany 部分得分公式的后续移植

**结论：193 citations,但无直接的医疗/AI4S 公式移植。最近的后继是 PhysicianBench。**

TheAgentCompany 源码公式:
```python
def calculate_score(total, result):
    return (result / total * 0.5) + (result // total * 0.5)
```
含义：50% 完成比例(线性部分得分) + 50% 全部通过奖励(binary)。

引用分析(193 篇 Semantic Scholar 引用):
- **PhysicianBench** (2605.02240): 唯一医疗后继,显式引用 TAC。使用 checkpoint 评测(670 checkpoints)但用了**更简化的公式**: `checkpoint_score = passed / total`（纯线性,无 binary 完成奖励），主指标为 pass@1。
- **ThePersonalizedAgentCompany** (fork): 保持完全相同的公式,加个性化,非医疗/AI4S。
- **LHAW** (2602.10525): 用 TAC 任务研究歧义,但衡量的是"终态分歧"而非改公式。
- **财富管理论文** (2512.02230): 扩展 TAC 环境到金融任务。
- **OR-Space** (2605.28158): 工业优化 agent benchmark,引用 TAC 的 workspace 式评测。

**无任何论文直接将 TAC 的 `0.5·(Result/Total) + 0.5·S_full` 公式移植到医疗/AI4S 领域。**

---

## 第 3 部分 · R / B / Unusable 三级分布表

将 Task 4 的 7 张旧卡 + 本轮 14 张新卡 = 21 张,按"是否可 Retrofit 加 checkpoint / 是否可借方法 / 不可用"三级分类。

### R (Retrofit) — 可以在现有数据上加 checkpoint,变成我们需要的过程量评测

| # | Benchmark | 方向 | Retrofit Cost | 理由 |
|---|---|---|---|---|
| 1 | **PhysicianBench** ⭐ | 医疗 EHR | **Low** | 670 checkpoint 已有,只需聚合为加权部分得分 + 加"求助" checkpoint |
| 2 | **AutoMat** ⭐ | 材料科学 | **Medium** | SME 步骤序列已是有序的,需从 LLM-judge holistic 评分拆为 discrete checkpoint |
| 3 | **BioAgent Bench** | 生物信息学 | **Low-Medium** | steps_completed 已跟踪进度,需定义 per-step checkpoint + 换确定性 grader |
| 4 | **BioXArena** | BioML | **Medium** | 76 任务,承诺释放 agent traces;待 trace 可用后回溯加过程量 |
| 5 | **Syntheseus** | 逆合成 | **Medium** | 190 目标分子的搜索轨迹天然含步骤序列;需加路线质量自动评分 |
| 6 | **BioProt** (Task4) | 生物实验 | **Medium** | ~1000 协议→伪代码对即步骤序列 GT;license 已澄清(MIT via pyproject.toml) |
| 7 | **MedAgentBench** (Task4) | 医疗 EHR | **Medium** | 300 医生撰写任务 + Docker FHIR;需自加 checkpoint;refsol.py 需联系作者 |
| 8 | **AgentClinic** (Task4) | 医疗诊断 | **Medium** | sentinel-tagged 动作可离线重打分;需从 stdout 解析轨迹 |
| 9 | **DiscoveryBench** (Task4) | 科学发现 | **Medium** | 264 任务 + gold_workflow;workflow 是分析流水线参考,非逐轮轨迹 |
| 10 | **ScienceAgentBench** (Task4) | AI4S | **High** | 102 任务;2 点 with/without 专家知识对照可扩展为 L0-L3;但需大量专家工作 |

### B (Borrow) — 不直接 Retrofit,但有具体方法/模板/公式/工具值得借

| # | Benchmark / Paper | 方向 | 借什么 |
|---|---|---|---|
| 11 | **TheAgentCompany** (Task4) ⭐ | 通用 agent | Checkpoint + rubric + 部分得分公式 + Collaboration 类别 + 3000 人时协议 |
| 12 | **AgentBoard** | 过程量方法论 | Progress rate 公式(子目标完成分数,单调递增)+ 双模式 + POMDP 形式化 + 人工验证协议 |
| 13 | **TRACE** | 过程量方法论 | Hierarchical Trajectory Utility + MIG + REP + Evidence Grounding + Scaffolded Capability Assessment |
| 14 | **SEISMO / PMO** | 药物优化 | AUC 优化曲线(轨迹级) + trajectory conditioning + 23 oracle 任务 seed |
| 15 | **SDDBench** | 药物合成可行性 | Round-trip score (逆合成→前向→比较) 思路;但当前不可获取 |
| 16 | **MedHELM** | 医疗 LLM | LLM-jury (3 providers × 3 axes) + 121 任务分类法 + cost-performance 框架 |
| 17 | **HealthBench Professional** | 医疗 LLM | 3-stage physician rubric design + length adjustment + red teaming taxonomy |
| 18 | **MEDIC** | 医疗 LLM 安全 | CEF 参考免评测 + 知识-执行差距 + 被动/主动安全区分 + MEDEC 三阶段错误评测 |
| 19 | **AppWorld** (Task4) | 通用 agent | State-based 单元测试 + collateral damage 检查 |
| 20 | **MLAgentBench** (Task4) | ML 实验 | trace.json 轨迹格式 + 失败模式分类 |
| + | **χ-Bench** ⭐(2026-06-09,见 C5)| 医疗运营 agent | **设计孪生**:复合验证器(确定性合约 + 多票 LLM judge)+ 三步造 case + per-stage rubric + pass^3 + 失败两级分类 + 安全 cp(illegitimate consent) |

### Unusable — License 阻挡 / 领域不匹配 / 任务类型不匹配

| # | Benchmark | 理由 |
|---|---|---|
| 21 | **LLM4Mat-Bench** | 纯 ML 性质预测,非 agent;无 LICENSE 文件;可作 seed 数据源但不可 retrofit 为 agent benchmark |
| 22 | **Matbench Discovery** | 纯 ML 能量模型评测(GNN/UIP),非 agent;MIT 许可但领域不匹配 |
| 23 | **LAB-Bench** (Task4) | MCQ 形态,无法加过程量 |
| 24 | **Biomni-Eval1** (Task4) | 字符串匹配评分,规模中等但 GO 集污染风险;可做 seed 但 retrofit 价值有限 |
| 25 | **ChemCrow** (Task4) | 论文级 demo(14 任务 case study + EvaluatorGPT + 4 专家 Likert),不是可复用 benchmark |
| 26 | **DrugSAGE** | Framework(非 benchmark),33 TDC/Polaris 任务仅最终指标;memory 架构可借但在 B 而非 R |
| + | **TrialPanorama**(2026-06-09,见 F1)| 终答型 LLM benchmark,**非 agent、无过程量**;但 1.6M 临床试验 + 8 任务可作**药物侧 seed**数据源(需自加 checkpoint)|
| + | **CLIMB / MedAgentsBench / MedXpertQA / AI Hospital**(cubox)| 多模态数据源或终答型 QA,非过程量 agent;见 F2 去重表 |

---

## 第 4 部分 · 材料方向供给侧空白的解释

**发现：材料方向不存在 ≥3 个带过程量的 agent benchmark。唯一的例外是 AutoMat(2026-05)。**

这不是调研遗漏——是供给侧的真实空白。原因分析：

1. **材料科学的 benchmark 传统是 ML 模型评测而非 agent 评测。** LLM4Mat-Bench、Matbench Discovery、Matbench(原版)都在回答"此 ML 模型预测性质的 MAE 是多少"——是固定输入→固定输出的映射评测。材料科学社区尚未系统性地思考"agent 在多步工作流中的行为评测"。

2. **材料 agent 系统处于"系统论文"阶段而非"benchmark 论文"阶段。** LLaMP、MASTER、Aethorix、MatClaw 都是展示"我的 agent 系统可以做 X"的系统论文,附带 ad hoc 评测(在几个选定的化合物/任务上 demo)。它们没有构建标准化的测试集、评分协议或排行榜。这类似于 NLP 在 GLUE 出现之前的状态。

3. **材料模拟的评测需要 HPC 和长时间运行。** DFT 计算单个体系可能需要数小时到数天。这使得构建包含 >50 个任务的 benchmark 成本极高(AutoMat 为此使用了 JHU 的 Rockfish 集群,每个任务最长 72 小时壁钟)。

4. **AutoMat 的出现(2026-05)是转折点。** 它证明了在材料科学中构建 agent benchmark 的可行性——通过"从论文复现声明"的框架,把长程计算工作流变成了可评测的任务。

**对我们的含义：** 材料方向的空白**正是我们的机会**。我们可以是第一个在材料方向构建带过程量 checkpoint 的 agent benchmark 的团队,与 AutoMat 互补(他们做可复现性,我们做过程量/介入分级)。

---

## 第 5 部分 · 关键借鉴的优先级排序

按"对我们 MVP 的直接价值"降序:

| 优先级 | 来源 | 借什么 | 用在哪 |
|---|---|---|---|
| **P0** | PhysicianBench | 4 类 checkpoint taxonomy + 3 种 grader 类型 + FHIR execution verification | 我们的 checkpoint 设计模板(直接从 Data Retrieval → Reasoning → Action → Documentation 映射到 Query → Analysis → Synthesis → Intervention) |
| **P0** | TheAgentCompany (Task4) | Checkpoint + 部分得分公式 + Collaboration 类别 | Evaluator 工程化模板 |
| **P1** | AgentBoard | Progress rate 公式(子目标完成分数,单调,max-over-history) | 我们的过程量核心指标定义 |
| **P1** | TRACE | Scaffolded Capability Assessment (λ_min) | 我们的"人类介入分级"理论框架——给 agent 不同比例的 oracle 轨迹,找最小所需引导 = L0-L3 的连续版 |
| **P1** | **χ-Bench**(2026-06-09)| 复合验证器(确定性合约 + 多票 LLM judge)+ 三步造 case + pass^3 + 失败两级分类 | **设计孪生**:直接佐证我们「硬校验+AI判官」分档与「分类→找点→合成」造题法;论文里把这两点定位为"采用已验证范式"而非首创 |
| **P2** | AutoMat | 5 维评分 rubric + LLM evaluator agent + judge calibration (κ=0.69) | 材料方向 checkpoint 的评分方案 |
| **P2** | Syntheseus | time-to-first-solution + monotonic diversity metric | 药物逆合成任务的过程量模板 |
| **P2** | SEISMO/PMO | AUC 优化曲线(轨迹级) | 药物优化任务的过程量模板 |
| **P3** | BioAgent Bench | steps_completed / steps_to_completion | 生物信息学任务的简易进度度量 |
| **P3** | MedHELM | LLM-jury (3 providers × 3 axes) | 我们 LLM-judge 的一致性验证方法 |
| **P3** | HealthBench | 3-stage physician rubric design | 我们医疗 checkpoint rubric 的构建协议 |
| **P3** | MEDIC | CEF 参考免评测 | 评测 agent 生成的科学报告(无需 gold output) |

---

## 第 6 部分 · 本轮遗留的新开放问题

1. **AutoMat 的 License 到底是什么?** GitHub 和论文均无声明。需联系 JHU CLSP 团队(Daniel Khashabi / Benjamin Van Durme)。
2. **BioXArena 的 agent trace 何时释放?** 论文承诺 "We will release all agent trajectories",但目前未见。释放后立即可做 retrofit 评估。
3. **BioAgent Bench 的 LICENSE?** 同样无 LICENSE 文件。需联系 Croatian 团队。
4. **SDDBench 能否公开?** 匿名审稿中,无法获取。若后续公开,round-trip score 思路直接可用。
5. **TRACE 的 Scaffolded Capability Assessment 能否直接用于 AI4S?** 需要实验验证——给 agent oracle 实验协议的前 λ 比例,看成功率曲线是否有意义。
6. **(2026-06-09)χ-Bench 的复合验证器实现细节需亲核。** 知乎 digest 称"确定性合约 + 多票 LLM judge(pinned Opus 4.7)+ per-stage rubric 锚定政策 + 三步造 case",但这些来自社媒转述、论文正文未亲核;repo(github actava-ai/chi-bench)+ dataset(HF / ModelScope)的实际 schema 待读。⏳ 列 P0——它是与我们设计最像的先例,实现细节直接影响我们"复合验证器"章节的写法。

---

## 附录 A · 调研方法的诚实校准

- **搜索**：deep-research workflow(5 角度并发) + 8 个专项 deep-read agent
- **流程**：5 路并发 WebSearch → 24 个一手源 fetch → 120 条声明 → top 25 做 3 投票对抗式核验(2/3 反对则 kill) → 24 通过,1 被驳
- **被驳 1 条(已剔除)**："MASTER is a multi-agent LLM framework that evaluates 4 reasoning architectures on 2 materials discovery tasks...totaling ~3200 runs, using GPT-5 as the base model"(1-2 驳;具体模型和运行数细节未经核实)
- **deep-read agent 未覆盖的范围**：
  - GeneAgent (Task4 已读) 未在本轮重读
  - DrugSAGE 的 memory 架构细节未深入(确认为 framework 非 benchmark 后降级)
  - OC20/OC22/OMat24 的 agent 使用方式未找到相关 benchmark(它们是 ML 竞赛排行榜)
- **重要校准**：材料方向"≥3 agent benchmark"的硬指标未达成(只找到 1 个: AutoMat + 2 个 ML benchmark)。这是诚实报告,不是调研不足——供给侧确实空白。
- **PhysicianBench 和 AutoMat 是本轮最重要的两个新发现**——它们改变了 Task 4 的核心结论("没有任何 AI4S/医疗 benchmark 做 checkpoint 评测")。

## 附录 B · 信源（已 fetch 的一手资料）

| 论文 ID | 名称 | 本地 |
|---|---|---|
| arXiv:2605.00803 | AutoMat | `prd/task6_papers/2605.00803.xml` |
| arXiv:2411.00177 | LLM4Mat-Bench | `prd/task6_papers/2411.00177.xml` |
| arXiv:2308.14920 | Matbench Discovery | `prd/task6_papers/2308.14920.xml` |
| arXiv:2310.19796 | Syntheseus | `prd/task6_papers/2310.19796.xml` |
| arXiv:2602.00663 | SEISMO | `prd/task6_papers/2602.00663.xml` |
| OpenReview | SDDBench | (under review, no public PDF archived) |
| arXiv:2605.02240 | PhysicianBench | `prd/task6_papers/2605.02240.xml` |
| arXiv:2601.21800 | BioAgent Bench | `prd/task6_papers/2601.21800.xml` |
| arXiv:2605.15766 | BioXArena | `prd/task6_papers/2605.15766.xml` |
| arXiv:2505.23802 | MedHELM | `prd/task6_papers/2505.23802.xml` |
| arXiv:2604.27470 | HealthBench Professional | `prd/task6_papers/2604.27470.xml` |
| arXiv:2409.07314 | MEDIC | `prd/task6_papers/2409.07314.xml` |
| arXiv:2401.13178 | AgentBoard | `prd/task6_papers/2401.13178.xml` |
| arXiv:2602.21230 | TRACE | `prd/task6_papers/2602.21230.xml` |
| arXiv:2512.13930 | MASTER | `prd/task6_papers/2512.13930.xml` |
| arXiv:2605.15461 | DrugSAGE | `prd/task6_papers/2605.15461.xml` |
| arXiv:2605.16679 | **χ-Bench / CHI-Bench**(2026-06-09 增补)| arXiv metadata 已一手核对(MCP);正文待亲核 |
| arXiv:2505.16097 | **TrialPanorama**(2026-06-09 增补)| arXiv metadata 已一手核对(MCP)|

完整 deep-research workflow 输出(120 声明、25 核验、信源全表)：`search/task6_deep_research_raw.json`。

**2026-06-09 增补的两条来自用户 Cubox/知乎收藏**(非本轮 deep-research workflow),已用 arxiv MCP 一手核对核心数据;χ-Bench 验证器内部机制为知乎 digest 转述,正文待亲核(⏳ P0)。知乎正文抓取链路见 `work/.claude/skills/zhihu-fetch`。
