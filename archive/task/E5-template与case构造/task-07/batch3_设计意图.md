# task-07 批次③ 设计意图文档（co_drug 30 道）

批次③：手写 constrained_optimization 药物侧 30 道，原地覆盖
case_co_drug_01~30.json；lineage="handwritten-batch3"。
四步节奏第 1 步产物：逐道四栏（领域/约束/说明/题面）+ §3.2 相容性自答 + §5 文献。

## 0. 批级设计决策（人定）与口径偏离记录

1. **类型2 形态**：全批带 objectives（28 道 logS:max、1 道 logD:max、
   1 道 half_life:max），dk 含 Objective 条 + 平手/唯一性表述条；query 由
   build_type2_query 组装（A 段 + Scoring 段 + 工具段 + 交付段）。
2. **格位来源（preflight 实测：候选 84 / 入选 30 / 判废 44 / 无解 10）**：
   - 30 格全部避开 18 禁入格——含本批专项要求的旧 co_drug 最后 4 格
     (0,-6)/(0,-5)/(0,1)/(1,0)，preflight 保留即时断言；本批写完 18 格清零。
   - 30 格全部避开 cs_drug 30 占格（药物形状格；co_material/dh_material
     材料格形状互异天然不撞；dh_drug 无 grid_key）。
   - 30 格批内互异；gt 均不在活跃排除集（cs 60 + co_material 30 + dh 30 +
     spare_dh_drug 3 = 123 个）。
   - **旧格换人策略**（旧 co_drug 30 gt 不在活跃排除集但软提示回避面貌雷同）：
     ①窄原子带把旧 gt 原子数排在带外；②窄 logS 窗把旧 gt 值排出窗外；
     ③稀疏性质（bbb/logD/half_life）fail-closed——旧 co gt 多为裸 logS
     实体，加稀疏性质约束即被天然剔除（cell_probe 实证）。最终 30 道 gt
     全部换人，无一道沿用旧 co gt。
3. **目标量口径**：logD/half_life 目标量一律叠 logS 约束（grid_key 契约
   v1.0 要求 gt 有 logS，批①教训）；C01=logD:max ∧ logD∈[0,2.5] ∧ logS≥−2.5；
   D01=half_life:max ∧ hl≥6 ∧ logS≥−2.5。
4. **候选池**：26 道在 [5,200]；**4 道池<5 为默认值偏离**，工程师裁定入选
   （规程 §8，deviation_note 三要素随题落盘）：
   | case | label | 格 | 池 | 偏离理由摘要 |
   |---|---|---|---|---|
   | 10 | L01 | (6,-2) | 2 | bbb==0 ∧ logS 窗 ∧ 30~34 原子三者交集库内天然稀疏 |
   | 21 | I11 | (9,-1) | 4 | 45~49 原子 bRo5 尺寸带内保 −1 档 logS 者稀少 |
   | 26 | I16 | (11,-1) | 3 | 55~59 原子超大尺寸带内保 −1 档 logS 者极少 |
   | 30 | L05 | (13,-6) | 4 | 65~74 原子 + −6 档 logS 双重极端组合 |
   四道科学问题均真实（外排避脑/大分子可溶/极难溶），非硬凑。
5. **atom_count 三件套全落**（批①修复轮教训）：原子窗同时入 constraints
   （atom_count >= / <=）、dk structural 条、s_feasible.criteria.structural，
   三处同口径；下限全部 ≥10。element_count 零使用（owner 拍定药物侧禁用）。
6. **opener 分配**：TYPE2_OPENERS 已扩 38→68（批③开工第一步完成，task-05
   builder 冻结禁重跑，只追加不改既有语义）；本批 30 道逐一使用新增段
   索引 38~67，30 种两词模式两两互异，M9a 变体率 30/30 ≥ 0.3 稳过。
7. **题面互异**：与 cs 60 + 现役 co_material 30 + dh 30 并排比对——cs 批为
   类型1 动词骨架（Identify/Find/…），本批为类型2 opener 骨架且每道
   项目叙事、尺寸带、窗口均不同；领域名全部"具体项目场景"级。
8. **snapshot**：BackendReader/load_entities+load_properties 实算注入，
   禁硬编码；write 脚本运行时打印实算数核对。
9. **文献**：沿用批①已 Crossref 核实 P 系列按科学问题选配，新增 3 篇
   （P24 bRo5 / P25 logD / P26 代谢稳定性），DOI 均 2026-08-09 经
   api.crossref.org 核实；gt 为 masked 实体无真实化合物身份，文献挂
   科学问题本身而非化合物类别，从机制上规避批②"挂错化合物类别"坑。
   provenance 不写作者名（防 KNOWN_REAL_NAMES 擦边）。

## 1. 文献清单（P1~P23 批①已核；P24~P26 本批 2026-08-09 Crossref 核实）

| 代号 | 文献 | DOI |
|---|---|---|
| P1 | Moving beyond Rules: The Development of a Central Nervous System Multiparameter Optimization (CNS MPO) Approach, ACS Chem. Neurosci. (2010) | 10.1021/cn100008c |
| P2 | Central Nervous System Multiparameter Optimization Desirability: Application in Drug Discovery, ACS Chem. Neurosci. (2016) | 10.1021/acschemneuro.6b00029 |
| P3 | Blood Brain Barrier Permeability Prediction Using Machine Learning Techniques: An Update, Curr. Pharm. Biotechnol. (2019) | 10.2174/1389201020666190821145346 |
| P4 | Physiologically Based Pharmacokinetic Modeling 1: Predicting the Tissue Distribution of Moderate-to-Strong Bases, J. Pharm. Sci. (2005) | 10.1002/jps.20322 |
| P5 | A Theoretical Basis for a Biopharmaceutic Drug Classification, Pharm. Res. (1995) | 10.1023/a:1016212804288 |
| P6 | Polar Molecular Surface Properties Predict the Intestinal Absorption of Drugs in Humans, Pharm. Res. (1997) | 10.1023/a:1012188625088 |
| P7 | Findings of the Second Challenge to Predict Aqueous Solubility, J. Chem. Inf. Model. (2020) | 10.1021/acs.jcim.0c00701 |
| P8 | ESOL: Estimating Aqueous Solubility Directly from Molecular Structure, J. Chem. Inf. Comput. Sci. (2004) | 10.1021/ci034243x |
| P9 | AqSolDB, a curated reference set of aqueous solubility and 2D descriptors, Sci. Data (2019) | 10.1038/s41597-019-0151-1 |
| P10 | Solubility and partitioning I: solubility of nonelectrolytes in water, J. Pharm. Sci. (1980) | 10.1002/jps.2600690814 |
| P11 | Estimation of aqueous solubility for a diverse set of organic compounds based on molecular topological indices, J. Chem. Inf. Comput. Sci. (2000) | 10.1021/ci9901338 |
| P12 | Therapeutics Data Commons (TDC), arXiv (2021)（已核实；修复轮后本批不再引用，case 25 改挂 P18，见 §6） | arXiv:2102.09548 |
| P13 | The solubility database (Wiki-pS0) of the pKa and intrinsic solubility compendium, ADMET & DMPK (2020) | 10.5599/admet.766 |
| P14 | Experimental and computational approaches to estimate solubility and permeability in drug discovery and development settings, Adv. Drug Deliv. Rev. (1997) | 10.1016/s0169-409x(96)00423-1 |
| P15 | Pharmaceutical profiling in drug discovery, Drug Discov. Today (2003) | 10.1016/s1359-6446(03)02649-7 |
| P16 | Prediction of Physicochemical Parameters by Atomic Contributions, J. Chem. Inf. Comput. Sci. (1999) | 10.1021/ci990307l |
| P17 | Brief overview of solubility methods: Recent trends in equilibrium solubility measurement and prediction, Drug Discov. Today Technol. (2018) | 10.1016/j.ddtec.2018.06.001 |
| P18 | Predicting Aqueous Solubility: The Role of Crystallinity, Curr. Drug Metab. (2009) | 10.2174/138920009790820110 |
| P19 | Handbook of Aqueous Solubility Data, CRC Press (2003) | 10.1201/9780203490396 |
| P22 | Prediction of human pharmacokinetics in drug discovery, Drug Metab. Pharmacokinet. (2017) | 10.1016/j.dmpk.2016.10.087 |
| P23 | Evaluation of Dynamic Polar Molecular Surface Area as Predictor of Drug Absorption, J. Med. Chem. (1998) | 10.1021/jm980313t |
| P24 | Oral Druggable Space beyond the Rule of 5: Insights from Drugs and Clinical Candidates, Chem. Biol. (2014) | 10.1016/j.chembiol.2014.08.013 |
| P25 | LogD: Lipophilicity for ionisable compounds, Chemosphere (2008) | 10.1016/j.chemosphere.2008.04.074 |
| P26 | Metabolic screening in vitro: metabolic stability, CYP inhibition and induction, Drug Discov. Today Technol. (2004) | 10.1016/j.ddtec.2004.10.008 |

## 2. 逐道设计（30 道）

格式：领域名 ｜约束组合（含原子带三件套）｜§3.2 相容性自答 ｜题面草案
（opener 索引 + 正文要点，build_type2_query 组装）｜文献。
机器栏（gt/池/格）由 task07_solve.py 回填，不在本文档预填。
case 编号按原子带升序排布。

### 01 口服 FBDD 项目的片段命中可溶性排序（label A04）
- 约束：logS∈[−0.5,0.5] ∧ atom∈[10,14]；目标 logS:max。格 (2,0)。
- 自答：10~14 原子片段配 logS≈0 窗口——片段库标配画像，分子大小×溶解度
  相容（小片段天然易溶，窗内取最可溶者现实存在）。
- 题面：opener 38（Between the）｜fragment hits of an oral FBDD campaign，
  小尺寸+窗内溶解度，挑最可溶。｜P8。
### 02 外周疼痛项目的低中枢暴露片段发现（label G01）
- 约束：logS∈[−1.5,−0.5] ∧ atom∈[10,14]；目标 logS:max。格 (2,−1)。
- 自答：小片段、−1 档 logS、外周靶点起点——尺寸×溶解度相容；脂溶性温和
  （logS 负值偏低亲脂），BBB 被动渗透倾向低，与外周定位一致。
- 题面：opener 39（Inside the）｜peripheral pain program 的 fragment
  starting points，避开中枢暴露。｜P14。
### 03 吸入给药项目的窄窗小分子吸入候选（label F01）
- 约束：logS∈[−4.5,−3.5] ∧ atom∈[13,14]；目标 logS:max。格 (2,−4)。
- 自答：吸入候选需低水溶以延长肺部滞留，小尺寸配 −4 档 logS——尺寸×
  溶解度相容（现实中吸入皮质激素即低溶小/中分子画像）。
- 题面：opener 40（Throughout the）｜inhaled retention 优先的小分子。｜P17。
### 04 肺局部给药项目的极低溶小分子（label F02）
- 约束：logS∈[−5.5,−4.5] ∧ atom∈[12,13]；目标 logS:max。格 (2,−5)。
- 自答：12~13 原子却 −5 档 logS 属"反直觉但真实"画像（高晶格能/少极性
  基团），尺寸×溶解度对可共存，库内 7 个合格者佐证。
- 题面：opener 41（Against the）｜lung-localized、systemic 暴露最小化。｜P18。
### 05 外用皮肤科项目的适度可溶小先导（label J02）
- 约束：logS∈[−0.7,−0.51] ∧ atom∈[15,19]；目标 logS:max。格 (3,−1)。
- 自答：外用候选需角质层分配而不过度水溶，窄窗 −0.6 档配 lead-like 尺寸，
  现实中存在（外用 NSAID 画像）。
- 题面：opener 42（Along the）｜topical dermatology lead 的窄窗可溶性排序。｜P16。
### 06 口服溶液剂项目的 BCS 高溶带片段级候选（label A06）
- 约束：logS∈[−0.5,0.5] ∧ atom∈[15,19]；目标 logS:max。格 (3,0)。
- 自答：lead-like 尺寸 + BCS 高溶门槛带——分子大小×溶解度相容，现实主流。
- 题面：opener 43（Amid the）｜oral solution formulation 候选排序。｜P5。
### 07 抗感染项目的低溶 lead-like 窗口候选（label F03）
- 约束：logS∈[−4.5,−3.5] ∧ atom∈[16,19]；目标 logS:max。格 (3,−4)。
- 自答：抗感染 lead 常牺牲溶解度换靶点亲和力，−4 档配 lead-like 尺寸现实
  存在；尺寸×溶解度相容。
- 题面：opener 44（Beyond the）｜anti-infective lead 窗内取最可溶。｜P7。
### 08 口服吸收甜区项目的中尺寸候选排序（label F04）
- 约束：logS∈[−4.5,−3.5] ∧ atom∈[20,23]；目标 logS:max。格 (4,−4)。
- 自答：20~23 原子 + −4 档 logS 为典型"溶解度待优化先导"画像，现实大量
  存在；尺寸×溶解度相容。
- 题面：opener 45（Beneath the）｜oral sweet-spot profiling 的低溶档排序。｜P15。
### 09 口服分布优化项目的 logD 甜区候选（label C01）
- 约束：logD∈[0,2.5] ∧ logS≥−2.5 ∧ atom∈[20,29]；目标 logD:max。格 (4,−2)。
- 自答：logD 甜区 0~2.5 叠加可溶性下限——脂溶性×吸收分布相容（logD 1~3
  为口服吸收甜区），logS≥−2.5 保证不落入难溶区，两对均不矛盾。
- 题面：opener 46（Beside the）｜distribution-balancing 项目挑 logD 最高的
  合格者。｜P25。
### 10 外周限制项目的避脑候选排序（label L01，池<5 偏离）
- 约束：bbb==0 ∧ logS∈[−2.5,−1.5] ∧ atom∈[30,34]；目标 logS:max。格 (6,−2)。
- 自答：刻意不入脑的外周靶点设计 + 保适度溶解——脂溶性×BBB 相容（bbb==0
  常由极性/外排决定而非单纯低脂溶），尺寸×溶解度相容；池仅 2 为库内天然
  稀疏（三条件交集），偏离已按 §8 带 deviation_note。
- 题面：opener 47（Down the）｜peripherally restricted 项目避 CNS 副作用。｜P4。
### 11 悬浮剂型分诊的难溶带中尺寸候选（label M05）
- 约束：logS∈[−6.5,−5.5] ∧ atom∈[20,24]；目标 logS:max。格 (4,−6)。
- 自答：−6 档 logS 配 20~24 原子=难溶固形风险样本——分子大小×溶解度
  相容（现实中难溶先导常见此画像），用于悬浮剂型评估。
- 题面：opener 48（Over the）｜suspension formulation 的难溶档排序。｜P19。
### 12 先导优化项目的 25~29 原子中溶窗口候选（label A10）
- 约束：logS∈[−3.5,−2.5] ∧ atom∈[25,29]；目标 logS:max。格 (5,−3)。
- 自答：中尺寸 + −3 档 logS=lead optimization 起点常见画像，现实存在。
- 题面：opener 49（Under the）｜lead optimization 队列窗内择优。｜P9。
### 13 长效给药项目的代谢稳定性排序（label D01）
- 约束：half_life≥6 ∧ logS≥−2.5 ∧ atom∈[20,29]；目标 half_life:max。格 (5,−2)。
- 自答：代谢半衰期≥6 h 且溶解度下限宽松——尺寸×溶解度相容，长半衰期与
  中尺寸分子共存现实成立（代谢稳定性主要取决于代谢软位点而非尺寸）。
- 题面：opener 50（Via the）｜once-daily 长效目标，挑半衰期最长合格者。｜P26。
### 14 中尺寸高溶档候选排序（label F06）
- 约束：logS∈[−0.5,0.5] ∧ atom∈[27,29]；目标 logS:max。格 (5,0)。
- 自答：27~29 原子仍保 logS≈0 需足量极性基团，现实中存在（多杂原子骨架）。
- 题面：opener 51（With the）｜solubility-anchored mid-size 排序。｜P10。
### 15 高溶带 30~34 原子候选（label F13）
- 约束：logS∈[0.5,0.7] ∧ atom∈[30,34]；目标 logS:max。格 (6,1)。
- 自答：30+ 原子仍 logS>0.5=极易溶大分子，需强极性/离子化基团，现实中
  存在（氨基糖苷类画像）；尺寸×溶解度在强极性补偿下相容。
- 题面：opener 52（Per the）｜high-sol band 的大尺寸排序。｜P13。
### 16 口服大分子边缘区的温和溶解度候选（label F08）
- 约束：logS∈[−1.5,−0.5] ∧ atom∈[37,39]；目标 logS:max。格 (7,−1)。
- 自答：37~39 原子保 −1 档 logS——尺寸×溶解度在极性骨架下相容，现实中为
  口服吸收边缘区画像。
- 题面：opener 53（Following the）｜absorption boundary 排序。｜P23。
### 17 抗感染大尺寸先导的中溶窗口（label F09）
- 约束：logS∈[−3.5,−2.5] ∧ atom∈[35,38]；目标 logS:max。格 (7,−3)。
- 自答：大环/多环抗感染骨架常见 −3 档 logS 配 35~38 原子，现实存在。
- 题面：opener 54（Respecting the）｜anti-infective macrocycle 排序。｜P11。
### 18 外周阿片项目的避脑中尺寸候选（label B05）
- 约束：bbb==0 ∧ logS∈[−4.5,−3.5] ∧ atom∈[35,44]；目标 logS:max。格 (8,−4)。
- 自答：外周阿片经典策略=BBB 排除 + 保留药效团，低溶档配中大尺寸现实
  存在（外周限制阿片拮抗剂画像）；脂溶性×BBB 相容（bbb==0 由季铵/高
  极性实现而非低脂溶）。
- 题面：opener 55（Regarding the）｜peripheral opioid 项目避中枢。｜P1。
  P1（CNS MPO）作为反向参照使用：该打分定义入脑方向，本题反向引用其
  BBB 维度（bbb==0 要求避脑），provenance 已随题注明。
### 19 透皮储库项目的低溶缓释候选（label H19）
- 约束：logS∈[−1.5,−0.5] ∧ atom∈[40,44]；目标 logS:max。格 (8,−1)。
- 自答：透皮候选需脂水分配平衡，−1 档 logS 配 40~44 原子现实存在；
  尺寸×溶解度相容。
- 题面：opener 56（Concerning the）｜transdermal reservoir 排序。｜P6。
### 20 难溶档大分子制剂分诊（label H09）
- 约束：logS∈[−5.5,−4.5] ∧ atom∈[40,44]；目标 logS:max。格 (8,−5)。
- 自答：大尺寸 + −5 档 logS=制剂分诊典型难溶样本，分子大小×溶解度相容
  （疏水骨架放大效应）。
- 题面：opener 57（Touching the）｜formulation triage 的难溶排序。｜P17。
### 21 bRo5 口服空间的 45~49 原子可溶候选（label I11，池<5 偏离）
- 约束：logS∈[−1.5,−0.5] ∧ atom∈[45,49]；目标 logS:max。格 (9,−1)。
- 自答：beyond-Rule-of-5 尺寸仍保 −1 档 logS——分子大小×溶解度在环化/
  分子内氢键屏蔽策略下现实成立（bRo5 口服药如环孢素画像）；池 4 为库内
  天然稀疏，偏离已带 deviation_note。
- 题面：opener 58（Pondering the）｜bRo5 oral space 可溶性排序。｜P24。
### 22 组织分布研究的极难溶大分子（label E06）
- 约束：logS∈[−6.5,−5.5] ∧ atom∈[45,49]；目标 logS:max。格 (9,−6)。
- 自答：45~49 原子 + −6 档 logS=组织蓄积风险评审样本，现实存在（高
  亲脂大分子）；尺寸×溶解度相容。
- 题面：opener 59（Balancing the）｜tissue-distribution 评审排序。｜P22。
### 23 大口服候选的中溶窗口（label I13）
- 约束：logS∈[−3.5,−2.5] ∧ atom∈[50,54]；目标 logS:max。格 (10,−3)。
- 自答：50~54 原子保 −3 档 logS——bRo5 边缘口服画像，尺寸×溶解度在
  极性补偿下相容。
- 题面：opener 60（Judging the）｜large-oral 队列排序。｜P24。
### 24 难溶窗大分子的蓄积风险评审（label M06）
- 约束：logS∈[−5.5,−4.5] ∧ atom∈[50,54]；目标 logS:max。格 (10,−5)。
- 自答：大尺寸 + −5 档 logS 为蓄积风险评审典型样本，现实存在。
- 题面：opener 61（Measuring the）｜accumulation-risk 排序。｜P19。
  （修复轮：领域名去掉 lipophilic 定性词——约束未叠 logD，弱宣称改按
  约束实际覆盖表述，见 §6。）
### 25 混悬长效注射的极难溶大分子（label G04）
- 约束：logS∈[−6.5,−5.5] ∧ atom∈[50,54]；目标 logS:max。格 (10,−6)。
- 自答：−6 档 logS 配 50~54 原子——混悬注射缓释设计刻意求极低溶，现实
  策略成立；尺寸×溶解度相容。
- 题面：opener 62（Grading the）｜depot suspension 排序。｜P18。
  （修复轮：P12 为 ML 数据集论文、与药剂学科学问题弱相关，改挂 P18
  结晶度×溶解度视角，见 §6。）
### 26 bRo5 超大分子的可溶性极限（label I16，池<5 偏离）
- 约束：logS∈[−1.5,−0.5] ∧ atom∈[55,59]；目标 logS:max。格 (11,−1)。
- 自答：55~59 原子仍保 −1 档 logS 属 bRo5 可溶性极限样本，现实中大环
  多极性侧链可实现；池 3 为库内天然稀疏，偏离已带 deviation_note。
- 题面：opener 63（Ordering the）｜bRo5 solubility-limit 排序。｜P24。
### 27 大环口服项目的低溶档排序（label G13）
- 约束：logS∈[−4.5,−3.5] ∧ atom∈[55,59]；目标 logS:max。格 (11,−4)。
- 自答：大环骨架 −4 档 logS 配 55~59 原子，现实存在（大环抗生素画像）；
  尺寸×溶解度相容。
- 题面：opener 64（Pitting the）｜macrocyclic oral 项目排序。｜P13。
### 28 大环骨架的难溶窗排序（label G05）
- 约束：logS∈[−5.5,−4.5] ∧ atom∈[55,59]；目标 logS:max。格 (11,−5)。
- 自答：大尺寸大环 −5 档 logS，现实存在；尺寸×溶解度相容。
- 题面：opener 65（Charting the）｜难溶窗 macrocycle 排序。｜P16。
  （修复轮：领域名去掉 lipophilic 定性词——约束未叠 logD，弱宣称改按
  约束实际覆盖表述，见 §6。）
### 29 超大尺寸难溶档候选（label G07）
- 约束：logS∈[−5.5,−4.5] ∧ atom∈[60,64]；目标 logS:max。格 (12,−5)。
- 自答：60~64 原子 + −5 档 logS=超大尺寸难溶样本，现实存在（新模态
  大分子）；尺寸×溶解度相容。
- 题面：opener 66（Mapping the）｜new-modality 难溶排序。｜P22。
### 30 巨型分子极难溶极限样本（label L05，池<5 偏离）
- 约束：logS∈[−6.5,−5.5] ∧ atom∈[65,74]；目标 logS:max。格 (13,−6)。
- 自答：65~74 原子 + −6 档 logS=尺寸×溶解度双极端，库内仅 4 个合格者，
  属物理化学极限研究真实样本；偏离已带 deviation_note。
- 题面：opener 67（Cataloguing the）｜size-solubility limit 排序。｜P18。

## 3. 判废与回调记录（实时维护）

preflight 四数口径（2026-08-09 定稿）：候选 84 / 入选 30 / 判废(issues) 44
/ 无解 10。
- 判废主因：cs_drug 占格（H10/H11/H13~H18/I04/I05/L10/M01 等）、与已选同格
  （F05/F21/H01/I02）、gt=旧 co 软提示换人（F10/H04/H12/I08/I14/I15/L02~
  L04/L07/L08/L11）、池<5 未获裁定（F07/H02/H06~H08/I03/I06/I07/I09/I10/
  L09/M02/M07/M08/M11/M12）。
- 无解名单：H03/H05/I01/I12/L06/L12/M03/M04/M09/M10（多为高溶档配大
  原子带，库内真空，如实记录不硬凑）。
- 回调记录：第五轮编辑曾误删 I16/I17 候选行，已补回 I16（(11,−1) 偏离
  入选），I17 不需要；无其他回调。

## 4. 与活跃语料的互异核对（题面级）

- 与 cs_drug 60 道（含 cs_material）：骨架类型不同（类型1 动词 vs 类型2
  opener+Scoring），且每道项目叙事、尺寸带、窗口参数不同。
- 与现役 co_material 30：领域侧不同（药物 vs 超导晶体），性质空间无交集。
- 与现役 dh 30：dh 为发现题骨架，无重叠。
- 批内 30 道：opener 两词模式互异（38~67 段），领域名逐道专属。

## 5. 文献出处说明

新增 3 篇均 2026-08-09 经 api.crossref.org works 接口核实：
- P24 Oral Druggable Space beyond the Rule of 5（Chem. Biol. 2014）→ 挂
  bRo5 尺寸带题目（21/23/26）；
- P25 LogD: Lipophilicity for ionisable compounds（Chemosphere 2008）→
  挂唯一 logD 目标题（09）；
- P26 Metabolic screening in vitro（DDT: Technologies 2004）→ 挂唯一
  half_life 目标题（13）。
复用篇按科学问题选配，每篇至多挂 3 道。gt 实体为 masked formula，无真实
化合物身份，文献挂"本题科学问题"本身，不声称 gt 属于文献中的化合物类别。
（修复轮后 P18 挂 04/25/30 三道，仍在 ≤3 上限内。）

## 6. 修复轮记录（2026-08-10，三维评审提示级 6 条，原子提交收口）

只动人定栏文字与文档，机器栏（constraints/gt/grid_key/s_feasible/
objectives）零改动；case 24/28 重跑 lint 确认 violations==0，solve 无需
重跑（机器栏不变）。
1. §0.4 偏离表编号勘误："case 22（I11）/case 27（I16）"为误写，改回
case 21 / case 26，与 §2 逐道节、账本、case 文件 deviation_note 三处
权威记录对齐。
2. case 11 标题勘误：原"儿科口服液项目的高溶带中尺寸候选"与约束
logS∈[−6.5,−5.5] 及落盘 domain（suspension formulation study in the
poorly soluble band）相反，改为"悬浮剂型分诊的难溶带中尺寸候选"
（case 文件 domain/query 本就与约束一致，无需改）。
3. case 24/28 弱化领域名：约束未叠 logD，"lipophilic"定性词属弱宣称；
不改机器栏（不叠 logD 约束），domain/query 去掉 lipophilic 改为约束实际
覆盖的表述（24：large molecules … accumulation-risk assessment within a
poorly soluble window；28：macrocycles ranked within a poorly soluble
window）；dk 本就无 lipophilic 字样。
4. 文献弱相关两处：case 25（depot suspension 极难溶）从 P12（TDC ML 数据
集论文）改挂 P18（结晶度×溶解度，贴药剂学问题）；case 18 保留 P1
（CNS MPO）作反向参照，provenance 与本文档均注明"该打分定义入脑方向，
本题反向引用其 BBB 维度"。两篇均为已核实真实文献（§1 表）。
5. 同步改动：task07_batch3_write.py CASES 表人定栏文本（domain_plural/
ref）同步勘误，保持生成器与产物一致；落盘器不重跑。
