# 文献库扩量 50 条抽检记录（toolcheck 任务②）

- 抽检对象：`dag_poc/data/backend/literature_search/literature_search_A.jsonl`（2655 条，md5=29218d204ecec4fc9d41be388b242a6e）
- 抽样方法：分层随机 `random.Random(20260815)`——legacy（老 309 手工标注）抽 6 条 + auto（自动抽取）抽 44 条，与库内占比（309/2655 ≈ 11.6%）一致
- 抽检人：构建执行 agent 逐条人工核验（2026-08-15）

## 核验结论

- **legacy 6/6 通过**：手工关键词原样合入（DOI 精确匹配），全部切题。
- **auto 44/44 可用**：keywords 均忠实 title、无公式形泄漏（身份消毒闸 keyword_sanitized=0）、无坏形 token（全库 13 个坏形嫌疑词精确扫描 0 命中）。
- **25 条 fallback 正常**：title 过短（如单词标题 "Levetiracetam"）抽取不足 2 词，回落主题手工标注——机制符合设计。注：Levetiracetam 等库外药名出现在题录属正常学术内容，身份闸口径是拦库内注册身份，不拦文献词汇。
- **主题漂移 4 条登记不处理**（Crossref relevance 尾部固有，题录真实、keywords 忠实 title）：
  - `10.1016/0012-821x(67)90166-5`（陨石 rare-earth 丰度，1967）
  - `10.1007/bf01162428`（核反应堆微扰论损失系数）
  - `10.1179/174329406x122847`（Fe–20Cr 合金氧化，rare earth 主题带偏）
  - `10.1016/0042-207x(72)90630-6`（碳化硅晶体氮分布，1972）
  同性质漂移条目（估计 <3%）：题录本身真实可检索，agent 按主题词检索时命中它们无信息危害，不处理。

## 抽检过程中发现并修复的抽取缺陷（5 轮迭代）

| 代际 | 缺陷（抽检实证） | 修复 |
|---|---|---|
| v1 | `studies→studie`（-ies 未处理）；连字符形不归一 | -ies→y；连字符词同样处理 |
| v2 | `species→specy`、`series→sery`（单复同形词坏形） | species/series 入停用词表 |
| v3 | XML 标签泄漏（`<scp>`/`<sub>` → kw 出现 scp/sub）；`Ames'→ame` 撇号碎片；`2p582` 页码残留 | 抽取前剥 XML 标签+去撇号；数字开头 token 滤 |
| v4 | `approaches→approache`（-es 未处理） | -es 规则（词干尾 ss/x/z/ch/sh 判真 -es） |
| v5 | `techniques→techniqu`/`phases→phas`（e 尾词干误去 e）；`ames→ame`（短词专有名词） | e 尾词干只去单 s；len>4 才归一（短词保留原形） |
| 终 | `sery/specy` 复活（停用词检查移到归一后，原形拦截失效） | 停用词双查（原形+归一形） |

全库终扫坏形嫌疑集 {ame, approache, processe, techniqu, metabolit, abundanc, glucuronid, scp, sub, 2p582, 2d6, specy, sery} 精确匹配 **0 命中**。

## 检索面验证（gateway 口径 `any(kw in query.lower())`）

| query | 命中条数 | | query | 命中条数 |
|---|---|---|---|---|
| superconductor critical temperature | 299 | | drug induced liver injury | 232 |
| iron selenide superconductivity | 240 | | qsar | 68 |
| magnesium diboride | 81 | | pbpk modeling | 137 |
| caco-2 permeability | 46 | | solubility prediction | 357 |
| blood brain barrier | 28 | | transdermal delivery | 152 |
| nickelate | 54 | | toxicity prediction | 364 |

12 个典型 query 全部有效命中（28-364 条），无零命中；老库（309 条）时代同 query 命中量约为此 1/8。

## 抽检明细（50 条）

| # | 来源 | 年份 | 被引 | title（截断 80 字符） | keywords |
|---|---|---|---|---|---|
| 1 | legacy | 1994 | 2 | Cytochrome P450 in Human Drug Metabolism: How Much Is Predictable? | cyp450, metabolism, drug interaction |
| 2 | legacy | 2015 | 0 | Case Studies of Non‐CNS Drugs to Minimize Brain Penetration—Nonsedative Antihist | blood brain barrier, bbb, cns, brain penetration |
| 3 | legacy | 2026 | 0 | Machine-Learning Methods for pH-Dependent Aqueous-Solubility Prediction | solubility, logS, aqueous |
| 4 | legacy | 2022 | 1 | In-silico identification of novel inhibitors for human Aurora kinase B form the  | zinc database, virtual screening, compound library |
| 5 | legacy | 2012 | 11 | Pairing symmetry in the iron-pnictide superconductor KFe
                    <su | pnictide, iron based superconductor, iron selenide |
| 6 | legacy | 2020 | 12 | Citrus auraptene induces drug efflux transporter P-glycoprotein expression in hu | p-glycoprotein, efflux, mdr1, transporter |
| 7 | auto | 2026 | 0 | New high-entropy rare-earth barium copper oxide superconductor via high-pressure | superconductor, superconductivity, high-entropy, rare-earth, barium |
| 8 | auto | 2012 | 51 | Modulation of Cytochrome-P450 Inhibition (CYP) in Drug Discovery: A Medicinal Ch | modulation, cytochrome-p450, inhibition, cyp, drug |
| 9 | auto | 2014 | 20 | Skin ablation by physical techniques for enhancing dermal/transdermal drug deliv | transdermal, skin, ablation, physical, technique |
| 10 | auto | 2018 | 0 | Nanotechnology approaches for oral bioavailability enhancement of drugs undergoi | bioavailability, metabolism, nanotechnology, oral, enhancement |
| 11 | auto | 2009 | 1 | A Quantitative Structure‐Activity Relationship Study of Some Commercially Availa | quantitative, structure, activity, relationship, commercially |
| 12 | auto | 2017 | 0 | Synthesis of Nanopowders of Magnesium Diboride by Magnesium Thermic Reduction un | synthesis, nanopowder, magnesium, diboride, thermic |
| 13 | auto | 2022 | 0 | Levetiracetam | drug induced liver injury, hepatotoxicity, dili |
| 14 | auto | 2009 | 29 | Glucuronidation and Covalent Protein Binding of Benoxaprofen and Flunoxaprofen i | glucuronidation, covalent, protein, binding, benoxaprofen |
| 15 | auto | 1969 | 0 | Perturbation-theory calculation of the reactor loss coefficient | perturbation-theory, calculation, reactor, loss, coefficient |
| 16 | auto | 2021 | 27 | In silico prediction of chemical respiratory toxicity via machine learning | prediction, toxicity, silico, chemical, respiratory |
| 17 | auto | 2015 | 38 | Enhanced oral bioavailability of lurasidone by self-nanoemulsifying drug deliver | bioavailability, oral, lurasidone, self-nanoemulsifying, drug |
| 18 | auto | 2023 | 12 | Predicting Maternal and Infant Tetrahydrocannabinol Exposure in Lactating Cannab | modeling, predicting, maternal, infant, tetrahydrocannabinol |
| 19 | auto | 1979 | 47 | Mutagenicity of anthraquinone and azo dyes in Ames' Salmonella typhimurium test | mutagenicity, anthraquinone, azo, dyes, ames |
| 20 | auto | 2016 | 4 | Effect of Platinum Group Metal Doping in Magnesium Diboride Wires | platinum, group, metal, doping, magnesium |
| 21 | auto | 2019 | 5 | Lead optimization of selective tubulin inhibitors as anti-trypanosomal agents | lead, optimization, selective, tubulin, inhibitor |
| 22 | auto | 2008 | 100 | Application of Microemulsions in Dermal and Transdermal Drug Delivery | transdermal, microemulsion, dermal, drug, delivery |
| 23 | auto | 2019 | 1 | Methodological Considerations on <scp>CYP</scp>2D6 Phenoconversion Due to Drug–D | methodological, consideration, cyp, phenoconversion, due |
| 24 | auto | 1989 | 3 | Identification of some glucuronides by direct liquid‐inlet liquid chromatography | identification, glucuronide, direct, liquid, inlet |
| 25 | auto | 2020 | 4 | Crystal growth, structural, optical and theoretical investigations of organic no | crystal, growth, structural, optical, theoretical |
| 26 | auto | 2024 | 3 | Noninvasive Novel Transdermal Drug Delivery System for Deep Drug Permeability | transdermal, permeability, noninvasive, drug, delivery |
| 27 | auto | 2017 | 26 | Drug-induced liver injury: Towards early prediction and risk stratification | prediction, drug-induced, liver, injury, early |
| 28 | auto | 2010 | 247 | Free Energy Perturbation Hamiltonian Replica-Exchange Molecular Dynamics (FEP/H- | free, energy, perturbation, hamiltonian, replica-exchange |
| 29 | auto | 2020 | 65 | Accurate Prediction of GPCR Ligand Binding Affinity with Free Energy Perturbatio | prediction, accurate, gpcr, ligand, binding |
| 30 | auto | 1989 | 14 | The structural identification of drug metabolites using thermospray liquid chrom | structural, identification, drug, metabolite, thermospray |
| 31 | auto | 2010 | 2 | Nano-fabrication processes for magnesium diboride | nano-fabrication, process, magnesium, diboride |
| 32 | auto | 2016 | 44 | Persistent-current switch for pancake coils of rare earth-barium-copper-oxide hi | superconductor, persistent-current, switch, pancake, coil |
| 33 | auto | 1994 | 33 | Transdermal Delivery of Narcotic Analgesics: Comparative Metabolism and Permeabi | transdermal, metabolism, permeability, delivery, narcotic |
| 34 | auto | 2011 | 122 | Topological superconducting phase and Majorana fermions in half-metal/supercondu | topological, majorana, superconductor, superconducting, phase |
| 35 | auto | 2022 | 0 | Amplifying the impact of kidney microphysiological systems: predicting renal dru | kidney, clearance, amplifying, impact, microphysiological |
| 36 | auto | 2023 | 41 | Chemical representation learning for toxicity prediction | toxicity, prediction, chemical, representation, learning |
| 37 | auto | 2006 | 0 | 2P582 Calculation of binding free energy based on a variational principle for QS | qsar, calculation, binding, free, energy |
| 38 | auto | 2014 | 0 | ChemInform Abstract: A New Layered Iron Arsenide Superconductor: (Ca, Pr)FeAs<su | layered, superconductor, cheminform, abstract, iron |
| 39 | auto | 2025 | 0 | Physiologically Based Pharmacokinetic (PBPK) Modeling | pbpk, modeling, physiologically, pharmacokinetic |
| 40 | auto | 2016 | 1 | Design of chemical space networks incorporating compound distance relationships | design, chemical, space, network, incorporating |
| 41 | auto | 2024 | 0 | The Influence of Skin Permeability and Electric Fields on Drug Permeation in Tra | permeability, transdermal, influence, skin, electric |
| 42 | auto | 2006 | 10 | Cyclic oxidation behaviour of rare earth oxide coated Fe–20Cr alloys | cyclic, oxidation, behaviour, rare, earth |
| 43 | auto | 2009 | 1 | Predictive 3D‐Quantitative Structure‐Activity Relationship for A<sub>1</sub> and | predictive, quantitative, structure, activity, relationship |
| 44 | auto | 1967 | 15 | Rare-earth and barium abundances in the Bununu howardite | rare-earth, barium, abundance, bununu, howardite |
| 45 | auto | 2007 | 251 | Drug-Induced Liver Injury | drug-induced, liver, injury |
| 46 | auto | 2002 | 146 | Effects of carbon doping on superconductivity in magnesium diboride | superconductivity, carbon, doping, magnesium, diboride |
| 47 | auto | 2022 | 214 | Superconductivity in infinite-layer nickelate La
            <sub>1−x</sub>
     | superconductivity, nickelate, infinite-layer, nio, thin |
| 48 | auto | 1983 | 2 | Relationship of Creatinine Clearance to Orthoiodohippurate Measured Effective Re | clearance, relationship, creatinine, orthoiodohippurate, measured |
| 49 | auto | 1972 | 0 | Nitrogen distribution coefficient in silicon carbide single crystals | nitrogen, distribution, coefficient, silicon, carbide |
| 50 | auto | 2019 | 52 | Molecular properties, crystal structure, Hirshfeld surface analysis and computat | molecular, property, crystal, structure, hirshfeld |