# task-07 批次① 设计意图文档（cs_drug 30 道）

> **本文档先于 case JSON 落盘**（规程四步节奏第 1 步）。设计意图由工程师（Lee）人定；
> 脚本（task07_solve.py）只负责算 gt/s_feasible/grid_key/唯一性，不产生任何约束或题面。
> 生成时间 2026-08-09；依据：手写case操作规程-v1.md、容量探针报告.md（药物 128 可用格）、
> task07_forbidden_cells.py（药物 14 禁入格 = cs_drug 10 + co_drug 4，本批 cs_drug 须避开全部 14 格）、task07_batch1_terrain_probe.py /
> task07_batch1_preflight.py 实测底账。

## 0. 批级设计决策（人定）

1. **格位来源**：30 格全部取自主流区（abin 2~10，即原子数 10~54；logS −7~1），
   避开药物 14 禁入格（cs_drug 10 + co_drug 4）与 co 60 道/dh 30 道占用的格及 gt（活跃语料口径，terrain_probe 实测）。
   不选成员数为 1 的极端角落格（占比 <1%），无一格需要专业理由豁免。
2. **性质使用**：logS 全批使用（grid_key 契约本身要求 gt 有 logS）；bbb 用 4 道；
   **logD/half_life 本批不用**——preflight 实测：带 logD/half_life 标注的实体高度集中在
   co 批已占的 ~10 个格内，全部候选组合要么撞占格、要么池 ≤2（见 preflight C/D/E 组记录），
   强行出题只能靠 deviation_note 兜底，不符"质量标杆"定位；归批次③④前再评估。
   这正是规程 §7"稀疏性质别叠太多条"的实测体现。
3. **平手规则**：全批类型1 多成员格统一"取 logS 最高者"（最易溶者优先）——
   药物化学含义：同等满足窗口约束时，溶解度裕量最大的先导化合物开发风险最低。
   该句写入每题 domain_knowledge 第 3 件，机器核以 `--tiebreak logS:max` 传入。
4. **element_count 零使用**（owner 拍定：药物侧禁用）。
5. **atom_count 全部 ≥10**（§3.1 硬线），下限带最低取 10。
6. **候选池**：最小 3（case 28），最大 130（case 13），全部在 [3,200]；
   无一道触发 deviation_note（批级零偏离）。
7. **文献**：17+ 篇真实文献（DOI 全部经 Crossref API 核实），每篇至多挂 2 道且按
   本题科学问题选；provenance 字符串不写作者名（防与 KNOWN_REAL_NAMES 清单擦边）。
8. **题面**：30 道互异；开头两词取自 TYPE1_OPENERS 池且 30 道互不重复（M9a 要求 ≥9 种）；
   英文骨架、30~120 词、D 段字段名 candidates/ranking/answer 压尾单句收尾、含
   domain knowledge 引用句（M1/M4/M5/M7/M8）。

## 1. 文献清单（DOI 均经 api.crossref.org 核实，2026-08-09）

| 代号 | 文献 | DOI |
|---|---|---|
| P1 | Moving beyond Rules: The Development of a Central Nervous System Multiparameter Optimization (CNS MPO) Approach, ACS Chem. Neurosci. (2010) | 10.1021/cn100008c |
| P2 | Central Nervous System Multiparameter Optimization Desirability: Application in Drug Discovery, ACS Chem. Neurosci. (2016) | 10.1021/acschemneuro.6b00029 |
| P3 | Blood Brain Barrier Permeability Prediction Using Machine Learning Techniques: An Update, Curr. Pharm. Biotechnol. (2019) | 10.2174/1389201020666190821145346 |
| P4 | Physiologically Based Pharmacokinetic Modeling 1: Predicting the Tissue Distribution of Moderate-to-Strong Bases, J. Pharm. Sci. (2005) | 10.1002/jps.20322 |
| P5 | A Theoretical Basis for a Biopharmaceutic Drug Classification: The Correlation of in Vitro Drug Product Dissolution Rates and in Vivo Bioavailability, Pharm. Res. (1995) | 10.1023/a:1016212804288 |
| P6 | Polar Molecular Surface Properties Predict the Intestinal Absorption of Drugs in Humans, Pharm. Res. (1997) | 10.1023/a:1012188625088 |
| P7 | Findings of the Second Challenge to Predict Aqueous Solubility, J. Chem. Inf. Model. (2020) | 10.1021/acs.jcim.0c00701 |
| P8 | ESOL: Estimating Aqueous Solubility Directly from Molecular Structure, J. Chem. Inf. Comput. Sci. (2004) | 10.1021/ci034243x（批次①前已核实） |
| P9 | AqSolDB, a curated reference set of aqueous solubility and 2D descriptors for a diverse set of compounds, Sci. Data (2019) | 10.1038/s41597-019-0151-1（已核实） |
| P10 | Yalkowsky & Valvani, Solubility and partitioning I: solubility of nonelectrolytes in water, J. Pharm. Sci. (1980) | 10.1002/jps.2600690814（已核实） |
| P11 | Huuskonen, Estimation of aqueous solubility for a diverse set of organic compounds based on molecular topological indices, J. Chem. Inf. Comput. Sci. (2000) | 10.1021/ci9901338（已核实） |
| P12 | Therapeutics Data Commons (TDC), arXiv (2021) | arXiv:2102.09548（已核实） |
| P13 | Avdeef, The solubility database (Wiki-pS0) of the pKa and intrinsic solubility compendium, ADMET & DMPK (2020) | 10.5599/admet.766（已核实） |
| P14 | Experimental and computational approaches to estimate solubility and permeability in drug discovery and development settings, Adv. Drug Deliv. Rev. (1997) | 10.1016/s0169-409x(96)00423-1 |
| P15 | Pharmaceutical profiling in drug discovery, Drug Discov. Today (2003) | 10.1016/s1359-6446(03)02649-7 |
| P16 | Prediction of Physicochemical Parameters by Atomic Contributions, J. Chem. Inf. Comput. Sci. (1999) | 10.1021/ci990307l |
| P17 | Brief overview of solubility methods: Recent trends in equilibrium solubility measurement and prediction, Drug Discov. Today Technol. (2018) | 10.1016/j.ddtec.2018.06.001 |
| P18 | Predicting Aqueous Solubility: The Role of Crystallinity, Curr. Drug Metab. (2009) | 10.2174/138920009790820110 |
| P19 | Handbook of Aqueous Solubility Data, CRC Press (2003) | 10.1201/9780203490396 |
| P21 | DrugBank 3.0: a comprehensive resource for 'Omics' research on drugs, Nucleic Acids Res. (2011) | 10.1093/nar/gkq1126 |
| P22 | Prediction of human pharmacokinetics in drug discovery, Drug Metab. Pharmacokinet. (2017) | 10.1016/j.dmpk.2016.10.087 |
| P23 | Evaluation of Dynamic Polar Molecular Surface Area as Predictor of Drug Absorption: Comparison with Other Computational and Experimental Predictors, J. Med. Chem. (1998) | 10.1021/jm980313t（T43 修复轮核实） |

## 2. 逐道设计（30 道）

格式：领域名 ｜约束组合 ｜§3.2 相容性自答 ｜题面草案（英文）开头两词 ｜文献。
机器栏（gt/池/格）由 task07_solve.py 回填，不在本文档预填。

### 01 水溶性小分子中枢神经系统（CNS）候选
- 约束：bbb==1 ∧ logS∈[−0.5,1.5] ∧ atom_count∈[10,14]；tiebreak logS:max。格 (2,1)。
- 相容性自答：符合的是"小分子、能过血脑屏障、且水溶性好"的 CNS 候选——现实中确有此类
  （小分子极性适中即可被动扩散入脑），尺寸×溶解度、脂溶性×BBB 两对均不矛盾。
- 题面开头：Identify a …（CNS 项目筛可溶性脑渗透小分子）｜P1。

### 02 血脑屏障穿透型 CNS 候选（小到中等尺寸）
- 约束：bbb==1 ∧ logS≥−5.0 ∧ atom_count∈[15,24]；tiebreak logS:max。格 (3,1)。
- 自答：中等尺寸、脑渗透、溶解度下限宽松的 CNS 化合物——典型 CNS MPO 空间，现实存在。
- 题面开头：Find a …｜P2。

### 03 血脑屏障穿透型 CNS 候选（中等尺寸）
- 约束：bbb==1 ∧ logS≥−4.0 ∧ atom_count∈[25,34]；tiebreak logS:max。格 (5,1)。
- 自答：25~34 原子仍能入脑者须脂溶性与极性平衡良好，现实中为成熟 CNS 药物尺寸带。
- 题面开头：Locate a …｜P3。

### 04 规避中枢暴露的外周作用候选（BBB 外排偏好）
- 约束：bbb==0 ∧ logS≥−3.0 ∧ atom_count∈[30,44]；tiebreak logS:max。格 (6,−1)。
- 自答：设计成不入脑的外周靶点药物（避免中枢副作用），需保留足够溶解度——现实的
  peripherally restricted 设计策略，两对相容性均成立。
- 题面开头：Select a …｜P4。

### 05 高水溶性口服给药候选（可溶液剂潜力）
- 约束：logS∈[0.5,1.5] ∧ atom_count∈[20,29]；tiebreak logS:max。格 (4,1)。
- 自答：logS>0.5 的药物尺寸分子极易溶，现实对应 BCS 高溶类/可口服液体制剂候选。
- 题面开头：Pinpoint a …｜P7。

### 06 生物药剂学分类（BCS）高溶带口服候选
- 约束：logS∈[−0.5,0.5] ∧ atom_count∈[20,29]；tiebreak logS:max。格 (4,0)。
- 自答：中等尺寸、摩尔溶解度达 BCS 高溶门槛附近的口服化合物——现实主流。
- 题面开头：Retrieve a …｜P5。

### 07 中等溶解度的类药中等尺寸分子
- 约束：logS∈[−0.5,0.5] ∧ atom_count∈[30,39]；tiebreak logS:max。格 (6,0)。
- 自答：30~39 原子仍保持 logS≈0 需足够极性基团，现实中存在（多杂原子骨架）。
- 题面开头：Screen for a …｜P15。

### 08 片段筛选起点：中等溶解度的小分子片段
- 约束：logS∈[−3.5,−2.5] ∧ atom_count∈[10,14]；tiebreak logS:max。格 (2,−3)。
- 自答：FBDD 片段典型尺寸（10~14 原子）配中等溶解度——片段库标配，尺寸×溶解度相容。
- 题面开头：Search for a …｜P14。

### 09 中等溶解度窗口的 lead-like 化合物
- 约束：logS∈[−3.2,−2.6] ∧ atom_count∈[15,19]；tiebreak logS:max。格 (3,−3)。
- 自答：lead-like 尺寸带配 −3 档 logS，是优化起点的常见画像，现实存在。
- 题面开头：Pick a …｜P8。

### 10 中等溶解度口服先导化合物（20~24 原子）
- 约束：logS∈[−3.5,−2.5] ∧ atom_count∈[20,24]；tiebreak logS:max。格 (4,−3)。
- 自答：经典口服先导的尺寸/溶解度组合，吸收-溶解度权衡的现实平衡点。
- 题面开头：Single out a …｜P6。

### 11 中等溶解度中大型先导（30~34 原子）
- 约束：logS∈[−3.5,−2.5] ∧ atom_count∈[30,34]；tiebreak logS:max。格 (6,−3)。
- 自答：分子变大后仍守住 −3 档溶解度需控制脂溶性增长，现实可行（beyond-Ro5 前哨）。
- 题面开头：Detect a …｜P16。

### 12 中等溶解度窗口的大分子（45~49 原子）
- 约束：logS∈[−3.5,−2.5] ∧ atom_count∈[45,49]；tiebreak logS:max。格 (9,−3)。
- 自答：45~49 原子仍维持微摩尔级溶解度的大环/降解剂类分子，现实中是新兴模态的画像。
- 题面开头：Uncover a …｜P22。

### 13 溶解度良好的类药小分子（口服吸收甜区）
- 约束：logS∈[−2.5,−1.5] ∧ atom_count∈[15,19]；tiebreak logS:max。格 (3,−2)。
- 自答：小尺寸+−2 档 logS = 口服吸收甜区，现实中最常见的类药画像之一。
- 题面开头：Track down a …｜P9。

### 14 高溶解度小中分子（20~24 原子）
- 约束：logS∈[−1.5,−0.5] ∧ atom_count∈[20,24]；tiebreak logS:max。格 (4,−1)。
- 自答：极性取代充分的小中分子，水溶性好，现实常见。
- 题面开头：Spot a …｜P11。

### 15 高溶解度中尺寸分子（25~29 原子）
- 约束：logS∈[−1.5,−0.5] ∧ atom_count∈[25,29]；tiebreak logS:max。格 (5,−1)。
- 自答：中尺寸保 −1 档溶解度，电离基团或极性杂环支撑，现实存在。
- 题面开头：Isolate a …｜P10。

### 16 溶解度较好的大分子（35~39 原子）
- 约束：logS∈[−2.5,−1.5] ∧ atom_count∈[35,39]；tiebreak logS:max。格 (7,−2)。
- 自答：大分子靠多极性基团守住 −2 档溶解度——现实对应多官能团化口服药。
- 题面开头：Determine the …｜P17。

### 17 口服吸收边缘区的大分子（40~44 原子）
- 约束：logS∈[−2.5,−1.5] ∧ atom_count∈[40,44]；tiebreak logS:max。格 (8,−2)。
- 自答：40~44 原子恰在被动口服吸收的尺寸边缘，−2 档溶解度是可达的极限画像。
- 题面开头：Discover a …｜P23（T43 修复轮换贴题：动态极性表面积-口服吸收边界方向）。

### 18 需溶解度优化的疏水性先导化合物
- 约束：logS∈[−4.5,−3.5] ∧ atom_count∈[25,29]；tiebreak logS:max。格 (5,−4)。
- 自答：活性驱动下脂溶性爬升的先导，−4 档溶解度提示需晶型/盐型或结构优化，现实常态。
- 题面开头：Seek out a …｜P18。

### 19 低溶解度溶出限速候选（30~34 原子）
- 约束：logS∈[−5.5,−4.5] ∧ atom_count∈[30,34]；tiebreak logS:max。格 (6,−4)。
- 自答：高脂溶低溶画像、溶出限速吸收，现实大量存在（约束只覆盖溶解度侧，
  不宣称渗透性或 BCS 分类——T43 修复轮删"高渗"断言）。
- 题面开头：Hunt down a …｜P19。

### 20 脂溶性低溶解度较大分子（35~39 原子）
- 约束：logS∈[−5.5,−4.5] ∧ atom_count∈[35,39]；tiebreak logS:max。格 (7,−4)。
- 自答：大疏水骨架+−5 档 logS，通用溶解度方程（脂溶性×熔点）可解释，现实存在。
- 题面开头：Unearth a …｜P10。

### 21 难溶脂溶性实体（窄窗口，30~34 原子）
- 约束：logS∈[−5.4,−4.6] ∧ atom_count∈[30,34]；tiebreak logS:max。格 (6,−5)。
- 自答：与 19 同带但窗口收窄，考察对 logS 阈值的精查能力；画像同 19（低溶
  解度溶出限速，不宣称 BCS 分类）。
- 题面开头：Recognize a …｜P16。

### 22 低水溶性中尺寸药物候选（25~29 原子，固形风险）
- 约束：logS∈[−5.5,−4.5] ∧ atom_count∈[25,29]；tiebreak logS:max。格 (5,−5)。
- 自答：中尺寸低溶多因高熔点刚性骨架，现实中与晶型强相关。
- 题面开头：Characterize a …｜P17。

### 23 疏水性小分子（15~19 原子）
- 约束：logS∈[−5.5,−4.5] ∧ atom_count∈[15,19]；tiebreak logS:max。格 (3,−5)。
- 自答：小而难溶是少数派但真实存在——刚性稠环/卤素取代的高熔点小分子；尺寸×溶解度
  组合少见但不矛盾（正是本题考点）。
- 题面开头：Specify a …｜P18。

### 24 近乎不溶的实体（25~29 原子）
- 约束：logS∈[−6.5,−5.5] ∧ atom_count∈[25,29]；tiebreak logS:max。格 (5,−6)。
- 自答：−6 档溶解度=制剂挑战级，现实对应高亲脂高熔点化合物。
- 题面开头：Nominate a …｜P12。

### 25 不溶性大脂溶性分子（35~39 原子）
- 约束：logS∈[−6.5,−5.5] ∧ atom_count∈[35,39]；tiebreak logS:max。格 (7,−6)。
- 自答：大疏水表面+几乎不溶，现实存在（脂库蓄积倾向分子）。
- 题面开头：Designate a …｜P19。

### 26 不溶性大分子（40~44 原子）
- 约束：logS∈[−6.5,−5.5] ∧ atom_count∈[40,44]；tiebreak logS:max。格 (8,−6)。
- 自答：40+ 原子不溶分子的 PK 常为吸收限速，现实对应难溶大分子候选。
- 题面开头：Flag a …｜P22。

### 27 极难溶实体（30~39 原子）
- 约束：logS∈[−7.5,−6.5] ∧ atom_count∈[30,39]；tiebreak logS:max。格 (6,−7)。
- 自答：−7 档=极限制剂挑战（纳米晶/无定形分散体才可能救），现实存在但开发风险高。
- 题面开头：Verify a …｜P13。

### 28 极难溶大分子（40~44 原子）
- 约束：logS∈[−7.5,−6.5] ∧ atom_count∈[40,44]；tiebreak logS:max。格 (8,−7)。
- 自答：同 27 画像、尺寸上移；池恰好 3（类型1 下限），无需偏离默认值。
- 题面开头：Establish a …｜P21。

### 29 极难溶超大分子（45~49 原子）
- 约束：logS∈[−7.5,−6.5] ∧ atom_count∈[45,49]；tiebreak logS:max。格 (9,−7)。
- 自答：超大疏水分子极难溶是物理必然（表面积↑、晶格能↑），现实存在。
- 题面开头：Report a …｜P14。

### 30 极难溶巨型分子（50~54 原子）
- 约束：logS∈[−7.5,−6.5] ∧ atom_count∈[50,54]；tiebreak logS:max。格 (10,−7)。
- 自答：本批尺寸上限带，−7 档溶解度与巨型疏水表面自洽。
- 题面开头：Name a …｜P16。

## 3. 判废与回调记录（实时维护）

| 轮次 | 事件 | 处置 |
|---|---|---|
| preflight 轮1 | C/D 组（logD/half_life 单性质+tiebreak 目标量）gt 无 logS → grid_key 契约断裂 | 全部叠加 logS 约束（契约发现，非题面改动） |
| preflight 轮1-7 | 104 候选组合：30 入选；66 判废（撞 co 占格/禁入格/不唯一/池过小）——A01/A02/A06/A07/A09/A12/A15/A16/A22/A23/A25/A33、B02/B05~B13、C01~C12、D01~D11/D13/D15、E01~E05/E07~E15/E19/E20/E22~E24；8 无解——A28/C13/D12/D14/E06/E17/E18/E21 | 判废/无解换组合重配（详见 task07_batch1_preflight.py 输出留痕与 task07_batch1_ledger.json preflight_accounting）。注：早期文档曾写判废 46/47，出处为早期轮次粗口径且漏列无解类与部分编号，作废，精确口径以此为准（T43 Ryan P2） |
| preflight 轮7 | A38/A39 入选；最终 30 格零冲突 | 采纳 |

| 落盘/lint 轮1 | case 22 target_domain 含 crystal 被域推断误判 material（G1）；case 24 arXiv provenance 无 DOI/URL（M10） | 领域名改 "solid-form" 表述并补 drug 关键词；arXiv 改 https://arxiv.org/abs/ 链接（题面/约束不变，机器栏不变） |
| T43 修复轮 | P1-1 case 19/21 领域名 BCS-II 超宣称；P1-2 atom_count 窗口未落 constraints；P2-1 snapshot 失真；P2-4 dk "not via the property interface" 失真；P2-5 case 17 文献不贴题 | 19/21 领域名降维为同义专业表述（措辞互异）；atom_count 三件套入 constraints；snapshot 改 BackendReader 实算（92,153 实体/240,467 性质行）；dk 改中性"可读实体记录或经性质接口核验"；case 17 换 P23（PSA-口服吸收边界）。全量重跑 solve：合格集/gt/池规模/grid_key 30/30 零差异；lint strict 0 违规 |
