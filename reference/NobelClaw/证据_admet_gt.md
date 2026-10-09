# 药物 ADMET 预测 — Ground-Truth 数据调研记录

> 目的：为「AI 科学 agent 评测 benchmark（ADMET 方向）」准备一批**已知真值**的小分子题目，
> 真值全部来自公认的公开实验数据集（TDC ADMET group / AqSolDB 等），可追溯到原始论文。
> 所有分子的 canonical SMILES 与身份均经 **PubChem InChIKey 二次核验**；所有真值均来自**实际下载的数据集行**，
> 非凭记忆。抓取时间：2026-06-03。
>
> 环境备注：本机 `raw.githubusercontent.com` 被墙（HTTP 000），但 `pubchem.ncbi.nlm.nih.gov`、
> `tdcommons.ai`、`api.github.com`、`dataverse.harvard.edu`、`cdn.jsdelivr.net` 均可达。
> TDC 数据集经 Harvard Dataverse `api/access/datafile/<id>` 直接拉取；AqSolDB 经 GitHub API raw-media 拉取。

---

## 0. 结论速览

- **canonical GT 源 = Therapeutics Data Commons (TDC) 的 ADMET Benchmark Group**（22 个数据集，覆盖 A/D/M/E/T 全部环节）+ **AqSolDB**（溶解度的事实标准）。两者都把更早的原始实验数据集（Martins BBBP、AstraZeneca logD、Wang Caco-2、Veith CYP、Obach/Lombardo PK、Hansen Ames、Karim hERG…）做了清洗与统一接口。
- 我们直接下载了 8 个端点数据集 + 全量 AqSolDB（9 982 行），从中**精选 14 个知名上市药**，每个都有 1 个以上端点的真值（多数分子横跨 4–5 个端点），可支撑比较型 / 分类型 / 数值容差型三类打分。
- **污染风险高**（全是名药，LLM 大概率背过）→ 评测应以**比较题 + 分类题**为主，数值题给容差带，并辅以「同分子多端点一致性」「打散匿名 SMILES 输入」等缓解手段（见 §5–§6）。

---

## 1. Canonical GT 数据集总览

### 1.1 Therapeutics Data Commons — ADMET Benchmark Group（主索引）

- 主页：<https://tdcommons.ai/benchmark/admet_group/overview/>
- 数据托管：Harvard Dataverse，按 `name2id` 下载，`https://dataverse.harvard.edu/api/access/datafile/<id>`（文件为 tab 分隔，列 `Drug_ID / Drug(SMILES) / Y`）。
- 论文（原始引用）：
  - Huang K., Fu T., Gao W., et al. *Therapeutics Data Commons: Machine Learning Datasets and Tasks for Drug Discovery and Development.* **NeurIPS 2021 Datasets & Benchmarks**. arXiv:2102.09548.
  - Huang K., Velez-Arce A., et al. *Artificial intelligence foundation for therapeutic science.* **Nature Chemical Biology** 18, 1033–1036 (2022). DOI: 10.1038/s41589-022-01131-2.

22 个 ADMET 数据集（单位 / 规模 / 任务 / 指标，摘自 TDC overview 实抓文本）：

| 类别 | 数据集 | 单位 | 规模 | 任务 | 指标 | TDC 名 / Dataverse id |
|---|---|---|---|---|---|---|
| Absorption | Caco-2 (Wang) | cm/s (log) | 906 | 回归 | MAE | `caco2_wang` / 4259569 |
| Absorption | HIA (Hou) | % | 578 | 二分类 | AUROC | `hia_hou` / 4259591 |
| Absorption | Pgp (Broccatelli) | % | 1 212 | 二分类 | AUROC | `pgp_broccatelli` |
| Absorption | Bioavailability (Ma) | % | 640 | 二分类 | AUROC | `bioavailability_ma` / 4259567 |
| Absorption | **Lipophilicity (AstraZeneca)** | logD7.4 | 4 200 | 回归 | MAE | `lipophilicity_astrazeneca` / 4259595 |
| Absorption | **Solubility (AqSolDB)** | log mol/L | 9 982 | 回归 | MAE | `solubility_aqsoldb` / 4259610 |
| Distribution | **BBB (Martins)** | 类别 | 1 975 | 二分类 | AUROC | `bbb_martins` / 4259566 |
| Distribution | PPBR (AZ) | % bound | 1 797 | 回归 | MAE | `ppbr_az` / 6413140 |
| Distribution | VDss (Lombardo) | L/kg | 1 130 | 回归 | Spearman | `vdss_lombardo` / 4267387 |
| Metabolism | CYP2C9/2D6/3A4 inhib (Veith) | 类别 | ~12k 各 | 二分类 | AUPRC | `cyp3a4_veith` / 4259582 等 |
| Metabolism | CYP2C9/2D6/3A4 substrate (CarbonMangels) | 类别 | ~666 | 二分类 | AUPRC/AUROC | `cyp*_substrate_carbonmangels` |
| Excretion | **Half-Life (Obach)** | hr | 667 | 回归 | Spearman | `half_life_obach` / 4266799 |
| Excretion | CL-Hepatocyte / CL-Microsome (AZ) | uL/min/1e6 / mL/min/g | 1 020 / 1 102 | 回归 | Spearman | `clearance_*_az` |
| Toxicity | LD50 (Zhu) | log(1/(mol/kg)) | 7 385 | 回归 | MAE | `ld50_zhu` |
| Toxicity | **hERG** | 类别 | 648 | 二分类 | AUROC | `herg` / 4259588 |
| Toxicity | **Ames** | 类别 | 7 255 | 二分类 | AUROC | `ames` / 4259564 |
| Toxicity | DILI | 类别 | 475 | 二分类 | AUROC | `dili` |

> 划分规则：TDC 统一用 **scaffold split**，留 20% 作 test。分类用 AUROC/AUPRC，回归多用 MAE，受结构外因素影响大的（VDss、半衰期、清除率）用 Spearman。

### 1.2 AqSolDB（溶解度事实标准，TDC `solubility_aqsoldb` 的上游）

- 仓库：<https://github.com/mcsorkun/AqSolDB>（文件 `results/data_curated.csv`，9 982 行，列含 `Name, InChI, InChIKey, SMILES, Solubility(=logS, log mol/L), SD, Occurrences, Group`）。
- 论文：Sorkun M.C., Khetan A., Er S. *AqSolDB, a curated reference set of aqueous solubility and 2D descriptors for a diverse set of compounds.* **Scientific Data** 6, 143 (2019). DOI: 10.1038/s41597-019-0151-1.
- **可信度分组很有用**：`Group` G1–G5 表示同一分子被多少独立来源覆盖；**G5 = 多来源、SD 小，最可靠**；G1 = 单来源。`SD` 是多来源 logS 的标准差，可直接当「实验变异度」用于设容差带。

### 1.3 各端点的原始（一手）实验论文

| 端点 | 原始论文 | DOI |
|---|---|---|
| 溶解度 logS | Sorkun 2019, *Sci Data* (AqSolDB) | 10.1038/s41597-019-0151-1 |
| BBB ± | Martins I.F. et al. *A Bayesian Approach to in Silico Blood-Brain Barrier Penetration Modeling*, *JCIM* 2012 | 10.1021/ci300124c |
| Caco-2 | Wang N.-N. et al. *ADME … Prediction of Caco-2 Cell Permeability …*, *JCIM* 2016 | 10.1021/acs.jcim.5b00642 |
| logD7.4 | AstraZeneca 实验 DMPK 数据，存于 ChEMBL（公开 deposited dataset，TDC 自 ChEMBL 提取） | (ChEMBL deposited) |
| hERG | Karim A. et al. *CardioTox net: a robust predictor for hERG channel blockade …*, *J Cheminform* 2021 | 10.1186/s13321-021-00541-z |
| CYP 抑制 | Veith H. et al. *Comprehensive characterization of cytochrome P450 isozyme selectivity …*, *Nat Biotechnol* 2009 | 10.1038/nbt.1581 |
| 半衰期 / VDss | Obach R.S., Lombardo F., Waters N.J. *Trend Analysis of a Database of Intravenous PK Parameters in Humans for 1352 Drug Compounds*, *DMD* 2008 | 10.1124/dmd.108.020479 |
| Ames 致突变 | Hansen K. et al. *Benchmark Data Set for in Silico Prediction of Ames Mutagenicity*, *JCIM* 2009 | 10.1021/ci900161g |

---

## 2. 精选分子 × 真值表（全部已 PubChem InChIKey 核验）

> 单位说明：**logS** = log10(溶解度 mol/L)（越大越易溶）；**logD7.4** = AstraZeneca pH7.4 分配比（越大越亲脂）；
> **Caco-2** = log10(Papp, cm/s)（越大越易透过，一般 > -5.0 视为高渗透）；**BBB** 1=能过血脑屏障(+)/0=不能(-)；
> **hERG** 1=阻断/0=不阻断；**Half-life** = 人体半衰期(hr)。每个值后括注来源数据集。
> SMILES 给的是 **PubChem canonical（已核 InChIKey）**；数据集内 SMILES 形式可能略异（盐/质子化/立体），但身份一致。

### 2.1 分子身份（canonical SMILES + InChIKey）

| 名称 | PubChem CID | InChIKey | Canonical SMILES |
|---|---|---|---|
| Caffeine | 2519 | RYYVLZVUVIJVGH-UHFFFAOYSA-N | `CN1C=NC2=C1C(=O)N(C(=O)N2C)C` |
| Warfarin | 54678486 | PJVWKTKQMONHTI-UHFFFAOYSA-N | `CC(=O)CC(c1ccccc1)C2=C(O)c3ccccc3OC2=O` |
| Verapamil | 2520 | SGTNSNPWRIOYBX-UHFFFAOYSA-N | `CC(C)C(CCCN(C)CCc1ccc(OC)c(OC)c1)(C#N)c1ccc(OC)c(OC)c1` |
| Propranolol | 4946 | AQHHHDLHHXJYJD-UHFFFAOYSA-N | `CC(C)NCC(O)COc1cccc2ccccc12` |
| Haloperidol | 3559 | LNEPOXFFQSENCJ-UHFFFAOYSA-N | `O=C(CCCN1CCC(O)(c2ccc(Cl)cc2)CC1)c1ccc(F)cc1` |
| Quinidine | 441074 | LOUPRKONTZGTKE-LHHVKLHASA-N | `C=C[C@H]1CN2CC[C@H]1C[C@@H]2[C@@H](O)c1ccnc2ccc(OC)cc12` |
| Diazepam | 3016 | AAOVKJBEBIDNHE-UHFFFAOYSA-N | `CN1C(=O)CN=C(c2ccccc2)c2cc(Cl)ccc21` |
| Imipramine | 3696 | BCGWQEUPMDMJNV-UHFFFAOYSA-N | `CN(C)CCCN1c2ccccc2CCc2ccccc21` |
| Clozapine | 135398737 | QZUDBNBUXVUHMW-UHFFFAOYSA-N | `CN1CCN(C2=Nc3cc(Cl)ccc3Nc3ccccc32)CC1` |
| Risperidone | 5073 | RAPZEAPATHNIPO-UHFFFAOYSA-N | `Cc1nc2n(c(=O)c1CCN1CCC(c3noc4cc(F)ccc34)CC1)CCCC2` |
| Theophylline | 2153 | ZFXYFBGIUFBOJW-UHFFFAOYSA-N | `Cn1c(=O)c2[nH]cnc2n(C)c1=O` |
| Phenytoin | 1775 | CXOFVDLJLONNDW-UHFFFAOYSA-N | `O=C1NC(=O)C(c2ccccc2)(c2ccccc2)N1` |
| Sulfamethoxazole | 5329 | JLKIGFTWXXRPMT-UHFFFAOYSA-N | `Cc1cc(NS(=O)(=O)c2ccc(N)cc2)no1` |
| Testosterone | 6013 | MUMGGOZAMZWBJJ-DYKIIFRCSA-N | `C[C@]12CC[C@H]3[C@H]([C@@H]1CC[C@@H]2O)CCC4=CC(=O)CC[C@]34C` |

### 2.2 真值矩阵（— 表示该端点未取该分子；空格代表数据集中无此名行）

| 名称 | logS (AqSolDB) | logD7.4 (AZ) | Caco-2 logPapp (Wang) | BBB (Martins) | hERG | Half-life hr (Obach) |
|---|---|---|---|---|---|---|
| Caffeine | **-0.91** (G5, SD 0.03) | **-0.03** | **-4.34** | **1 (+)** | 1* | **4.9** |
| Warfarin | **-4.78** (G5, SD 0.45) | — | — | **0 (-)** | — | — |
| Verapamil | **-3.98** (G1) | — | **-4.58** | **0 (-)** | **1** | — |
| Propranolol | **-3.62** (G5, SD 0.21) | — | — | **0 (-)** † | 1* | **3.4** |
| Haloperidol | — | **2.67** | **-4.71** | **1 (+)** | **1** | **35.0** |
| Quinidine | — | **2.21** | **-4.69** | **1 (+)** | **1** | **6.6** |
| Diazepam | — | **2.69** | — | **1 (+)** | — | **42.0** |
| Imipramine | **-4.19** (G1) | **2.49** | **-4.85** | **1 (+)** | **1** | **16.0** |
| Clozapine | — | **3.08** | **-4.51** | **1 (+)** | **1** | **10.0** |
| Risperidone | — | **2.05** | — | **1 (+)** | **1** | **3.2** |
| Theophylline | — | **-0.09** | **-4.35** | **0 (-)** | **0** | **7.2** |
| Phenytoin | **-3.99** (G5, SD 0.09) | — | — | **1 (+)** | **0** | — |
| Sulfamethoxazole | **-2.62** (G5, SD 0.25) | — | — | — | — | — |
| Testosterone | **-4.02** (G5, SD 0.20) | — | — | — | — | — |

附加高对比端点（毒性 / 经典阻断剂，便于出鲜明的分类/比较题）：

| 名称 | InChIKey | hERG | BBB | 说明 |
|---|---|---|---|---|
| Terfenadine | GUGOEEXESWIERI-UHFFFAOYSA-N | **1 (阻断)** | 0 (-) | 经典 hERG 阻断剂，曾因此撤市 |
| Astemizole | GXDALQBWZGODGZ-UHFFFAOYSA-N | **1 (阻断)** | 0 (-) | 同上，强 hERG 阻断 |
| Sotalol | ZBMZVLHSJCTVON-UHFFFAOYSA-N | **0 (不阻断)** | 1 (+) | hERG 阴性对照（与上面两者对比鲜明）|
| Chlorpromazine | ZPEIMTDSQAKGNT-UHFFFAOYSA-N | **1** | 1 (+) | logS=-5.07 (AqSolDB G5) |
| Metoprolol | IUBSYMUCCVWXPE-UHFFFAOYSA-N | **0** | 1 (+) | Caco-2=-4.35 |
| Digoxin | LTMHDMANZUZIPE-PUGKRICDSA-N | **1** | 0 (-) | Caco-2=-5.63（低渗透，P-gp 底物）|

\* **hERG 标注注意**：TDC `herg` 把 caffeine、propranolol 也标为 1（阻断）。这是该数据集二值化阈值/来源使然——它们在多数文献里是**弱/边缘**阻断。出题时**优先用对比鲜明的强阻断剂（terfenadine/astemizole/cisapride）vs 明确阴性（sotalol/phenytoin/theophylline）**，不要拿 caffeine/propranolol 当 hERG 题目。

† **BBB 标签噪声**：Martins 数据集里 propranolol 同时出现两行——`"Propanolol"`(盐型 `.[Cl]`，Y=1) 与 `propranolol`(Y=0)，互相矛盾，是已知的 BBBP 标签噪声。propranolol 实际**能过血脑屏障(BBB+)**（中枢副作用即证）。**出 BBB 题时避开 propranolol**，改用无争议者：caffeine/diazepam/imipramine/haloperidol(均 +) vs warfarin/theophylline/atenolol(均 -)。

> 以上每个数值都来自实际下载的数据集对应行；身份经 PubChem InChIKey 比对一致（logS/logD/half-life 的数据集 SMILES 与 PubChem 同 InChIKey skeleton 验证通过）。

### 2.3 关于 CYP3A4 抑制（为何未进主表）

`cyp3a4_veith`（Veith 2009，PubChem BioAssay 高通量筛）规模 ~12k，但**绝大多数是匿名/非著名化合物**：我们用分子式签名与 InChIKey 检索 ketoconazole / clotrimazole / miconazole / itraconazole / nicardipine / nifedipine / erythromycin / ritonavir 等典型 CYP3A4 抑制剂，**全部不在数据集中**。clozapine、quinidine、theophylline 在集中且标 0（非抑制）。
→ CYP3A4 端点**不适合用著名药出干净的真值题**；如需 CYP 题，建议用比较型并明确写「依据 Veith CYP3A4 数据集」，或换 CYP 底物（CarbonMangels）数据集。本表保留一个可用例：**Clozapine → CYP3A4 抑制 = 否(0)**（已 InChIKey 核验）。

---

## 3. 数据来源与可达性（复现实操）

```
# TDC 数据集（tab：Drug_ID / Drug=SMILES / Y）
curl -sL "https://dataverse.harvard.edu/api/access/datafile/4259566"   # bbb_martins
curl -sL "https://dataverse.harvard.edu/api/access/datafile/4259588"   # herg
curl -sL "https://dataverse.harvard.edu/api/access/datafile/4259569"   # caco2_wang
curl -sL "https://dataverse.harvard.edu/api/access/datafile/4259595"   # lipophilicity_astrazeneca (logD7.4)
curl -sL "https://dataverse.harvard.edu/api/access/datafile/4266799"   # half_life_obach
curl -sL "https://dataverse.harvard.edu/api/access/datafile/4259564"   # ames
curl -sL "https://dataverse.harvard.edu/api/access/datafile/4259582"   # cyp3a4_veith

# AqSolDB 全量（raw.githubusercontent 被墙，改走 GitHub API raw-media）
curl -sL -H "Accept: application/vnd.github.raw+json" \
  "https://api.github.com/repos/mcsorkun/AqSolDB/contents/results/data_curated.csv?ref=master"

# SMILES → InChIKey 核验（PubChem PUG REST，可达）
curl -sL "https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/name/<name>/property/SMILES,InChIKey/JSON"
curl -sL --data-urlencode "smiles=<SMILES>" \
  "https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/smiles/property/InChIKey/JSON"
```

---

## 4. 各端点的实验变异度（决定容差带）

| 端点 | 典型实验/数据集内变异度 | 建议容差/打分 |
|---|---|---|
| logS（溶解度）| AqSolDB 同分子多来源 SD 常 0.2–0.6 log，跨实验室可达 ~0.7 log | 数值题 **±0.5–0.7 log**；优先比较题 |
| logD7.4 | shake-flask 实验室间 ~0.3–0.5 log | **±0.5 log** |
| Caco-2 logPapp | 不同实验室绝对值差异大（细胞批次/方法）| 数值题慎用；用**高/低渗透分类**（阈值 ~ -5.0 ~ -5.5 log cm/s）或比较题 |
| PPBR(%) | 高结合区(>99%)对小数极敏感 | 用区间（低/中/高）或 ±5–10% |
| 半衰期 | 个体差异/给药途径致跨度极大，TDC 用 **Spearman**（只看排序）| **不要**比绝对值；用**比较题**「A vs B 谁半衰期长」，或数量级分类 |
| VDss / 清除率 | 同样高变异，TDC 用 Spearman | 比较题为主 |
| BBB / hERG / Ames / CYP | 本就是分类标签 | **直接分类打分**（注意标签噪声，见 §2.2 注）|

---

## 5. 推荐出题框架（打分干净）

**(a) 比较题（最稳，抗污染、抗实验变异）**
- 「下列两药，哪个**水溶性更高**？ (A) Warfarin (B) Caffeine」→ GT：Caffeine（logS -0.91 vs -4.78，差距大、跨容差稳）。
- 「哪个**人体半衰期更长**？ (A) Diazepam (B) Risperidone」→ GT：Diazepam（42 vs 3.2 hr）。
- 出题守则：选 GT 差距 **> 2× 端点容差** 的配对（如 logS 差 > 1.0、half-life 差 > 3×），避免落在实验误差带内。

**(b) 分类题（二分类端点天然适配）**
- 「Terfenadine 是否阻断 hERG 钾通道？(yes/no)」→ GT：yes（hERG=1）。
- 「Caffeine 能否穿过血脑屏障？(BBB+/BBB-)」→ GT：BBB+（=1）。
- 守则：避开 §2.2 标注的噪声/边缘项（hERG 的 caffeine/propranolol、BBB 的 propranolol）。

**(c) 数值带容差题（仅用于变异小的端点）**
- 「预测 Sulfamethoxazole 的 aqueous logS（log mol/L），±0.5 内算对」→ GT：-2.62 (AqSolDB G5, SD 0.25)。
- 「预测 Haloperidol 的 logD7.4，±0.5 内算对」→ GT：2.67 (AstraZeneca)。
- 守则：只对 logS/logD 用数值题，且**优先选 AqSolDB G5（多来源、SD 小）的分子**；半衰期/Caco-2/VDss 不出绝对数值题。

**(d) 一致性/组合题（提高对 agent 的区分度，难记忆）**
- 给 3–4 个分子，要求**按某端点排序**（如按 logS 升序）→ 用 Spearman/Kendall 对排序打分（与 TDC 半衰期/VDss 用 Spearman 同理），实验变异对排序更鲁棒。

---

## 6. 污染（contamination）风险与缓解

**风险**：本表全是经典上市药，LLM 训练语料里大量出现其 ADMET 性质 → agent 可能**靠记忆而非推理/调工具**答对，高估真实能力。

**缓解手段**（建议组合使用）：
1. **输入用 SMILES/InChIKey 而非药名**，并对 SMILES 做**随机原子重排（非 canonical）**，迫使 agent 识别结构而非检索名字。
2. **以比较/排序题为主**：即便记得单个值，跨分子的相对关系更依赖真实理解；且对实验变异鲁棒。
3. **混入冷门分子**：从同数据集抽 G1/低 occurrence 或匿名 ChEMBL 行（如 lipophilicity_az、cyp_veith 里大量非著名结构）做对照组，比较 agent 在「名药 vs 冷门」上的正确率差，差值即记忆污染的量度。
4. **要求给出依据/单位/数据集**：让 agent 说明数值来源与端点定义；只背了数字、说不清端点/单位的，扣分。
5. **数值题给容差而非精确匹配**，避免「背到小数」的虚高，同时也容下真正算对但实验值有偏的情况。
6. **过程量评分**（与本仓库 PRD 主张一致）：记录 agent 是否真的调用了性质预测/检索工具、推理链是否自洽，而不仅看最终答案对错——这是对抗记忆污染的根本手段。

---

## 7. 不确定 / 需注意项（诚实标注）

- **propranolol 的 BBB 标签**在 Martins 数据集里自相矛盾（盐型行=1 / 中性行=0）；真值应为 **BBB+**。出题避开它。
- **hERG 二值标签依赖阈值**：caffeine、propranolol 被标 1 属边缘判定，不代表强阻断。强阻断的干净例：terfenadine/astemizole/cisapride/haloperidol/quinidine。
- **半衰期/VDss/清除率**实验变异极大，TDC 官方就用 Spearman——**严禁出绝对数值题**，只出比较/排序。
- **CYP3A4_Veith** 不含主流 CYP3A4 抑制剂著名药（已检索确认），不适合用名药出干净真值题。
- AstraZeneca logD 数据**无独立期刊论文**，是 AZ 向 ChEMBL 公开 deposit 的实验 DMPK 数据集；引用时标 ChEMBL + TDC 即可。
- naproxen 在 AqSolDB 有两行（G3 logS=-3.29 多来源 / 单来源行 -4.50），用时取 **G3 多来源值 -3.29**。
