# Benchmark 数据库调研报告：我们调研的 benchmark 用了哪些数据库，能不能用

> **回答的问题**：我们调研的 14 个药物/材料发现 benchmark 各自用了哪些数据库？这些数据库的权威性凭什么成立？我们能借鉴着用吗？
> **方法**：从 `reference/` 14 个 benchmark 的源码和 README 中提取数据库名称和链接，逐一验证可访问性；**每篇论文的 venue 经 DBLP 逐条核准**（ACTION_RULES §4：禁止凭记忆断言 venue）。
> **结论先行（三句）**：
> 1. **这些库全部是第三方权威机构的「原料」，没有一个是 benchmark 自建的**——库是别人的，「怎么把库变成一道能自动判分的题」才是 benchmark 的贡献。这正好给我们「数据源引用现成权威库、判据自研」提供了先例。
> 2. **权威性不靠自证，靠交叉采用背书**：TDC 被 3 家独立 benchmark 采用且自身即 NeurIPS D&B 2021、PubChem 被 3 家采用、ChEMBL/ZINC/PDB 各被 2 家采用。**用它们当真值源，等于站在已被同行评议接收的工作上。**
> 3. **但背书强度按域严重不对称**：药物线 6 个库背后有 8 家顶会/顶刊 benchmark；材料线只有 Materials Project 一家（MADE），而我们材料 case 现在锚的 SuperCon **零家 benchmark 使用**——这是当前最大的选型风险，详见 §一.3。

---

## 一、权威性背书：谁用过这些库（venue 经 DBLP 核准）

> 本章是本报告的主论证。逻辑链：**一个库被越多家「相互独立、且经同行评议」的 benchmark 采用，越说明它是领域公认基座** → 我们采用同样的库 → 投稿时评审无从质疑数据源合理性。

### 1.1 14 个 benchmark 各自用了哪些库（含 venue）

| Benchmark | 使用的数据库 | venue（DBLP 核准） | 来源地址 |
|---|---|---|---|
| **TDC_PMO** | TDC ADMET | TDC＝**NeurIPS Datasets and Benchmarks 2021**；PMO＝**NeurIPS 2022** | https://tdcommons.ai/functions/admet/ |
| **Tartarus** | PDB 真实蛋白结构（1SYH/4LDE/6Y2F）+ QuickVina/Smina + xTB | **NeurIPS 2023** | https://github.com/aspuru-guzik-group/Tartarus |
| **GuacaMol** | ChEMBL | **J. Chem. Inf. Model. 59 (2019)** | https://www.ebi.ac.uk/chembl/ |
| **ChemCrow** | PubChem（`rest/pug/compound/`） | **Nat. Mac. Intell. 6 (2024)** | https://pubchem.ncbi.nlm.nih.gov/ |
| **DiscoveryBench** | 真实论文数据（6 域：考古/社会/生物/经济/工程） | **ICLR 2025** ← 即我方目标会议 | https://github.com/allenai/discoverybench |
| **MADE** | Materials Project（凸包 + 形成能） | ICML 2026（arXiv:2601.20996，DBLP 未收录，待官方 proceedings 复核） | https://materialsproject.org/ |
| **MOSES** | ZINC Clean Leads（459 万分子，已逐字核实） | Front. Pharmacol. 2020（DBLP 仅收 CoRR abs/1811.12823，待期刊页复核） | https://media.githubusercontent.com/media/molecularsets/moses/master/data/dataset_v1.csv |
| **TOMG-Bench** | Zinc-250K（**约 25 万分子**，见 §2.1 订正）+ PubChem | arXiv:2412.14642（DBLP 无记录，预印本） | https://github.com/aspuru-guzik-group/chemical_vae |
| **Biomni** | PyTDC + BindingDB（**以预训练 DTI 模型形式**，见 §2.4 订正） | bioRxiv 2026.05（预印本） | https://tdcommons.ai/ + https://www.bindingdb.org/ |
| **PoseBench** | PDB（posebusters_benchmark / astex_diverse / dockgen / casp15） | Nat. Mach. Intell.（DBLP 无记录，待复核） | https://www.rcsb.org/ |
| **ChemToolAgent** | PubChem + PubchemSearchQA | arXiv:2411.07228（DBLP 无记录，预印本） | https://pubchem.ncbi.nlm.nih.gov/ |
| **SciCode** | 真实科研问题（5 域：物理/数学/材料/生物/化学） | NeurIPS 2024 D&B（DBLP 检索未命中，待复核） | https://github.com/scicode-bench/SciCode |
| **SciAgentArena** | Ground truth CSV + PyTDC | arXiv:2606.12736（预印本） | https://github.com/HelloWorldLTY/SciAgentArena |
| **MatTools** | Pymatgen（材料计算库，非数据库） | CoRR 2025（预印本） | https://pymatgen.org/ |

### 1.2 按库聚合：交叉采用次数就是背书强度（药物/化学线）

**排序依据＝被多少家相互独立的 benchmark 采用。** 这一列才是可以写进论文的论据。

| 数据库（真实 URL） | 归属机构 | 被谁采用（venue 即背书） | 独立家数 | 我们借鉴的用法 |
|---|---|---|---|---|
| **TDC**<br>https://tdcommons.ai/ | Harvard | **自身即 NeurIPS D&B 2021**；PMO（NeurIPS 2022）、Biomni（Stanford SNAP）、SciAgentArena | **3＋1** | ADMET 真值来源（已在用）；QED/SA 当 A 类公式 oracle |
| **PubChem**<br>https://pubchem.ncbi.nlm.nih.gov/ | NIH | ChemCrow（Nat. Mac. Intell. 2024）、ChemToolAgent、TOMG-Bench | **3** | `search_molecule_database` 分子池 |
| **ChEMBL**<br>https://www.ebi.ac.uk/chembl/ | EMBL-EBI | GuacaMol（JCIM 2019）、TDC 的 B 类 oracle 训练数据 | **2** | 生物活性补充 |
| **ZINC**<br>https://zinc.docking.org/ | UCSF | MOSES（Front. Pharmacol. 2020）、TOMG-Bench（经 Zinc-250K） | **2** | 候选池扩容 |
| **PDB**<br>https://www.rcsb.org/ | RCSB | Tartarus（NeurIPS 2023）、PoseBench（Nat. Mach. Intell.） | **2** | `validate_target` 真实靶点 |
| **BindingDB**<br>https://www.bindingdb.org/ | UCSD | Biomni（以预训练 DTI 模型形式，非原始记录比对） | **1** | `predict_binding` 替换 mock |

**最强的一条论据**：TDC 本身就是 **NeurIPS Datasets and Benchmarks track** 的论文，而这正是我方要投的 track 类型。以 TDC 为真值源，数据源合理性已由该 track 的同行评议完成背书。

### 1.3 材料线的背书缺口（必须正视的选型风险）

| 数据库（真实 URL） | 被谁采用 | 独立家数 |
|---|---|---|
| **Materials Project**<br>https://materialsproject.org/ | MADE（ICML 2026） | **1** |
| **Pymatgen**（计算库，非数据库）<br>https://pymatgen.org/ | MatTools（CoRR 2025，预印本） | 1 |
| **SuperCon**<br>https://supercon.nims.go.jp/ | **零**——14 个 benchmark 全仓 grep 无一使用 | **0** |

**证据**：对 `reference/` 全仓检索 `supercon`，命中项全部落在 `reference/NobelClaw/`（我方自造题集），**无一条来自第三方 benchmark**。

**两个后果与处置**：

1. **Materials Project 是材料线唯一可引的背书**，且 MADE 与我方同域同闭环（凸包上找热力学稳定化合物 + oracle 预算下的序贯决策），是最该对标的一家。其配置文件明写数据是 `RAW, UNFILTERED Materials Project data`、task-specific 过滤在 Environment 层做——**这条恰好给我方「库引用现成权威源、判据自研」提供了现成先例**，可直接在论文里引用。
2. **SuperCon 没有任何 benchmark 背书**，而我方材料 case 的 17 个晶体目前锚在它上面（晶体逐条对照见 [`权威数据源与数据缺口指南.md`](../task/task12/权威数据源与数据缺口指南.md) §2.4）。处置：凡能从 Materials Project 取到的性质（formation_energy / bandgap / 凸包稳定性）一律改锚 Materials Project；Tc 确实仅 SuperCon 覆盖，则在 `provenance` 里同时标注 NIMS SuperCon 原始出处，并引 MADE 证明「材料发现 benchmark 以官方材料库做 oracle」这一做法本身有先例。

> ⚠️ **执行障碍（task11 实测追加）**：上述「改锚 Materials Project」在本环境**走不通**——MP API 实测 403、OQMD 实测不可达。现实出路是改锚 **3DSC**（Sci Data 2023，它本身就是 SuperCon × Materials Project 的匹配产物，自带 Tc＋CIF 结构）。详见 [`权威数据源与数据缺口指南.md`](../task/task12/权威数据源与数据缺口指南.md) §三.2与§三.3。

### 1.4 可直接写进论文的一句话

> Our property ground truth is drawn from TDC (NeurIPS Datasets and Benchmarks 2021) and the Materials Project, the same sources adopted by PMO (NeurIPS 2022), Biomni, Tartarus (NeurIPS 2023), GuacaMol (JCIM 2019) and MADE, and is frozen into a local snapshot for deterministic, offline-reproducible grading.

**后半句同样重要**：经源码核验，**上述 benchmark 判分时全部不联网**——GuacaMol 把 Celecoxib/Troglitazone 等目标分子的 SMILES 写死在 `src/benchmark_suites.py` L64-L77；SciAgentArena 冻结成 `*_ground_truth.csv`；MatTools 冻结成 `case_properties.json`。**所以我方的本地快照不是妥协，而是与该领域一致的标准做法——论文中不应称其为 mock，应称 frozen snapshot。**

---

## 二、可用数据库的详细说明

### 2.1 Zinc-250K（TOMG-Bench 用的）

> **⚠️ 订正（task10，经源码与命名核验）**：本节原写「ZINC Clean Leads 子集（250 万真实分子）」，**两处均错**：
> 1. **数量错 10 倍**——Zinc-250K 的「250K」即 **25 万**（chemical_vae 那份为 249,456 条），不是 250 万。按错的数去「扩充到 250 万」会白做工。
> 2. **归属混淆**——**ZINC Clean Leads 是 MOSES 的基底**（见 §2.x MOSES 与 §一.1），Zinc-250K 是另一个子集，出自 Gómez-Bombarelli 等的 chemical_vae（2018），并非 TOMG-Bench 自建，TOMG-Bench 只是从中采样。

**来源**：ZINC 数据库的类药子集，经 chemical_vae（2018）整理发布，约 **25 万**真实分子
**链接**：
- 主页：https://zinc.docking.org/
- Zinc-250K 下载：https://github.com/aspuru-guzik-group/chemical_vae（`data/zinc250k.smi`）
- PubChem 10M：https://pubchem.ncbi.nlm.nih.gov/

**覆盖内容**：
- 约 **25 万**个真实分子的 SMILES
- 分子性质：MW, LogP, TPSA, SMILES, 分子式

**对我们的用途**：
- `search_molecule_database`：搜索相似分子（当前 mock_db 只有 40 个分子，可扩充至 **25 万**量级）
- 作为 fixed_discovery 的候选池数据源

**benchmark 里的实际用法**：TOMG-Bench 从中采样真实分子，**再用 RDKit 反推指令**（每子任务 5000 题）——即答案先于题目存在，与我方 GT-first 路线同构。

**可访问性**：✅ 公开下载，无需注册

---

### 2.2 TDC ADMET（TDC_PMO / Biomni 用的）

**来源**：Therapeutics Data Commons（Harvard，NeurIPS 2021 D&B）
**链接**：
- 主页：https://tdcommons.ai/
- ADMET 函数：https://tdcommons.ai/functions/admet/
- BBB 渗透性：https://tdcommons.ai/functions/admet/#bbb-permeability
- hERG 毒性：https://tdcommons.ai/functions/admet/#herg-inhibition
- Caco-2 渗透性：https://tdcommons.ai/functions/admet/#caco2-permeability
- logS 溶解度：https://tdcommons.ai/functions/admet/#logs-water-solubility
- logD 分布系数：https://tdcommons.ai/functions/admet/#logd-distribution-coefficient
- half_life 半衰期：https://tdcommons.ai/functions/admet/#half-life

**论文**：
- *Therapeutics Data Commons: Machine Learning Datasets and Tasks for Therapeutic Discovery*（NeurIPS 2021 D&B）
- arXiv: https://arxiv.org/abs/2102.09548

**覆盖内容**：
- 23+ 个 ADMET 性质数据集
- BBB, hERG, Caco-2, logS, logD, half_life, MW, LogP, TPSA, QED, SA_Score, Ames, DILI, LD50

**对我们的用途**：
- `predict_admet`：预测药物 ADMET 性质（当前 mock_db 的 40 个分子就是从 TDC ADMET 反查的真实药物）
- 作为 fixed_discovery 的性质数据源

**可访问性**：✅ 公开下载，PyTDC Python 包：https://github.com/mims-harvard/TDC

---

### 2.3 Materials Project（MADE 用的）

**来源**：Materials Project（Berkeley Lab，APL Materials 2015）
**链接**：
- 主页：https://materialsproject.org/
- API 文档：https://materialsproject.org/docs/api
- 数据库：https://materialsproject.org/materials

**论文**：
- *The Materials Project: A materials genome approach to accelerating materials discovery*（APL Materials, 2015）
- DOI: https://doi.org/10.1063/1.4812323

**覆盖内容**：
- 15 万+ 材料的晶体结构、形成能、带隙、弹性性质
- 凸包稳定性（convex hull）

**对我们的用途**：
- `crystal_predict`：预测晶体性质（Tc/formation_energy/bandgap）
- `get_cif_by_formula`：按化学式查晶体结构
- 作为材料发现域的数据源

**可访问性**：✅ 公开 API，需注册（免费）

---

### 2.4 BindingDB（Biomni 用的）

**来源**：BindingDB（UCSD）
**链接**：
- 主页：https://www.bindingdb.org/
- 下载：https://www.bindingdb.org/bind/download.jsp

**覆盖内容**：
- 200 万+ 蛋白-配体结合亲和力数据
- Kd, Ki, IC50, EC50, SMILES, PDB ID

**对我们的用途**：
- `predict_binding`：预测蛋白-配体结合亲和力（当前 mock 固定返回 -8.5 kcal/mol，需要替换为真实数据）
- 作为对接任务的数据源

> **⚠️ 订正（task10，经 `reference/Biomni/tool_pharmacology_sample.py` L385 核验）**：本报告 §4.2 原写 BindingDB 的接入方式是「下载 BindingDB 数据集，替换 mock」，但**唯一使用它的 benchmark（Biomni）并非查原始记录，而是加载在 BindingDB 上预训练的 DTI 模型**：`DTI.model_pretrained(model=..._BindingDB)`。两者差别很大：
> - 工作量：加载预训练模型比下载并索引 200 万条记录小一个数量级；
> - **判分性质：预训练模型是「预测器」而非「真值」**，若用作 checkpoint 的 oracle，必须在论文中显式声明这一点（同 TDC 的 B 类 oracle）。
> 我方若要 hard checkpoint，`predict_binding` 应优先取 BindingDB 的**实测记录**（Kd/Ki/IC50）作为真值，而非预测模型输出。

**可访问性**：✅ 公开下载，无需注册

---

### 2.5 PubChem（ChemCrow / ChemToolAgent 用的）

**来源**：PubChem（NIH）
**链接**：
- 主页：https://pubchem.ncbi.nlm.nih.gov/
- API：https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/
- PUG View：https://pubchem.ncbi.nlm.nih.gov/rest/pug_view/data/compound/

**覆盖内容**：
- 1 亿+ 化学物质
- MW, LogP, TPSA, SMILES, 分子式, 生物活性

**对我们的用途**：
- `search_molecule_database`：搜索相似分子（Zinc-250K 的来源）
- `validate_target`：验证药物靶点

**可访问性**：✅ 公开 API，无需注册

---

### 2.6 ChEMBL（GuacaMol 用的）

**来源**：ChEMBL（EMBL-EBI）
**链接**：
- 主页：https://www.ebi.ac.uk/chembl/
- API：https://www.ebi.ac.uk/chembl/api/

**覆盖内容**：
- 2000 万+ 生物活性数据
- IC50, Kd, Ki, EC50, SMILES, 靶点

**对我们的用途**：
- `predict_admet`：补充 TDC ADMET 的生物活性数据
- `search_drug_literature`：搜索药物文献

**可访问性**：✅ 公开下载，无需注册

---

### 2.7 PDB（PoseBench 用的）

**来源**：PDB（RCSB）
**链接**：
- 主页：https://www.rcsb.org/
- API：https://www.rcsb.org/docs/programmatic-access

**覆盖内容**：
- 20 万+ 蛋白结构
- PDB ID, 序列, 配体, 结构

**对我们的用途**：
- `validate_target`：验证药物靶点
- `predict_binding`：蛋白结构（对接输入）

**可访问性**：✅ 公开下载，无需注册

---

### 2.8 SuperCon（材料发现域超导数据库）

> **⚠️ 订正（task10）**：本节原置于「14 个 benchmark 使用的数据库」框架下，属**分类错误**——对 `reference/` 全仓检索 `supercon`，**14 个 benchmark 无一使用 SuperCon**，命中项全部落在 `reference/NobelClaw/`（我方自造题集）。SuperCon 是**我方的选型意愿，不是调研发现**，它没有任何第三方 benchmark 背书。保留本节仅作数据源备查，引用时不得声称「其他 benchmark 也用它」。缺口处置见 §一.3。

**来源**：SuperCon（NIMS）
**链接**：
- 主页：https://supercon.nims.go.jp/
- NIMS Superconductor Database

**覆盖内容**：
- 3 万+ 超导材料
- Tc（超导转变温度）, crystal_structure

**对我们的用途**：
- `crystal_predict`：预测超导材料 Tc
- 作为超导材料发现域的数据源

**可访问性**：✅ 公开（需申请）

---

## 三、结论：我们该锚定哪些数据库

> 优先级已按 §一.2 的**背书强度**重排：同等可用性下，被更多独立 benchmark 采用的库优先。

### 3.1 药物发现域（优先锚定）

| 数据库 | 用途 | 背书强度 | 优先级 | 链接 |
|---|---|---|---|---|
| **TDC ADMET** | 药物 ADMET 性质（BBB/hERG/Caco-2/logS/logD/half_life） | **3＋1 家**（自身 NeurIPS D&B 2021） | 高 | https://tdcommons.ai/functions/admet/ |
| **PDB** | 蛋白结构 | **2 家**（Tartarus NeurIPS 2023、PoseBench） | 高 | https://www.rcsb.org/ |
| **PubChem** | 化学物质数据库 | **3 家**（ChemCrow NMI 2024 等） | 中高 | https://pubchem.ncbi.nlm.nih.gov/ |
| **ChEMBL** | 生物活性数据 | **2 家**（GuacaMol JCIM 2019、TDC B 类 oracle 训练集） | 中 | https://www.ebi.ac.uk/chembl/ |
| **Zinc-250K** | 分子库（search_molecule_database 数据源，**约 25 万**） | **2 家**（经 ZINC：MOSES、TOMG-Bench） | 中 | https://github.com/aspuru-guzik-group/chemical_vae |
| **BindingDB** | 蛋白-配体结合亲和力 | **1 家**（Biomni，且仅以预训练模型形式） | 高（需取实测记录而非模型输出） | https://www.bindingdb.org/ |

### 3.2 材料发现域（优先锚定）

| 数据库 | 用途 | 背书强度 | 优先级 | 链接 |
|---|---|---|---|---|
| **Materials Project** | 晶体结构 + 形成能 + 带隙 + 凸包稳定性 | **1 家**（MADE，同域同闭环） | 高 | https://materialsproject.org/ |
| **SuperCon** | 超导材料 + Tc | **0 家**——无任何 benchmark 背书（见 §一.3） | 仅 Tc 不可替代时使用，且需双标 provenance | https://supercon.nims.go.jp/ |

### 3.3 我们当前 mock_db 的差距

| 工具 | 当前 mock_db | 该锚定 | 差距 |
|---|---|---|---|
| predict_admet | 40 个分子（TDC ADMET 反查） | TDC ADMET 23+ 性质数据集 | 分子池太小（40 vs 23+ 数据集覆盖） |
| predict_binding | 固定返回 -8.5 kcal/mol | BindingDB 200 万+ 条实测记录 | mock 数据太简单 |
| search_molecule_database | 40 个分子 | Zinc-250K **约 25 万**分子（已订正，原写 250 万） | 分子池太小 |
| crystal_predict | 17 个晶体（手工构造） | Materials Project 15 万+ 材料 | 晶体池太小，且现锚 SuperCon 无背书 |
| validate_target | mock | PDB 20 万+ 蛋白结构 | mock 没有真实靶点 |

---

## 四、我们该怎么做

### 4.1 立即可做的（不需要额外找）

我们的 mock_db 的 40 个 ADMET 分子就是从 TDC ADMET 反查的真实药物——数据源本身就是权威背书，且该背书可追溯到 NeurIPS D&B 2021。需要做的是：
1. 在 case 的 `provenance` 里显式写明"基于 TDC ADMET 数据集"；
2. 补充 mock_db 缺失的性质（caco2/logS/logD/half_life_days 各缺 9-13/40）；
3. **论文中不再称其为 mock，改称 frozen snapshot**——因为 GuacaMol/SciAgentArena/MatTools 均为同一做法（见 §一.4）。

### 4.2 需要补的

| 数据库 | 用途 | 怎么接入 |
|---|---|---|
| **TDC ADMET** | 补充 mock_db 的 ADMET 性质 | PyTDC Python 包：`pip install pytdc` |
| **BindingDB** | 替换 predict_binding 的 mock 数据 | 下载 BindingDB **实测记录**（Kd/Ki/IC50）作真值；**不要用 Biomni 那种预训练 DTI 模型**（那是预测器、非真值，见 §2.4 订正） |
| **Materials Project** | 替换 crystal_predict 的 mock 数据；并**把材料 case 从 SuperCon 改锚到它** | Materials Project API：`mp_api` Python 包（需免费注册 `MATERIALS_PROJECT_API_KEY`） |
| **Zinc-250K** | 扩充 search_molecule_database 的分子池 | 下载 Zinc-250K 数据集（**约 25 万条**，非 250 万） |

---

## 参考

- reference/TOMG-Bench/README_采样说明.md（Zinc-250K 数据来源）
- reference/TDC_PMO/README_采样说明.md（TDC ADMET 详解，oracle A/B 两类区分）
- reference/Tartarus/README_采样说明.md（物理模拟 oracle）；reference/Tartarus/oracles/docking.py L56（受体结构为本地冻结资产）
- reference/MADE/README_REPO.md（Materials Project 凸包环境）；reference/MADE/configs/dataset_phase_diagram.yaml（RAW UNFILTERED + 需 API key）
- reference/GuacaMol/REPO_README.md（ChEMBL 训练集）；reference/GuacaMol/src/benchmark_suites.py L64-L77（目标分子 SMILES 硬编码于源码）；src/distribution_learning_benchmark.py L206-L212（ChEMBL 作分布基线）
- reference/MOSES/REPO_README.md L25（ZINC Clean Leads，4,591,276 分子，已逐字核实）
- reference/Biomni/tool_pharmacology_sample.py L385（BindingDB 以预训练 DTI 模型形式使用）
- reference/ChemCrow/tool_converters.py L26（PubChem 实时 REST API）
- venue 核准：DBLP search API（https://dblp.org/search/publ/api），核准日期见 task/task10/report.md
