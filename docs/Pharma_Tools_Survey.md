# 制药预测模型工具清单 v4（一行一工具，性质类型逐点，入口存活已核、功能未跑）

调研与核实日期：2026-09-03。五轮工作合并：轮 1 七路检索；轮 2 并行线六轮 API 交叉核验（证据 `tools/step1_survey_evidence/`）；轮 3 九路逐行开页核实；轮 4 按 ADR-0001 拆版本行；轮 5 工程师入口存活检查（PyPI JSON 26 包、GitHub API 60 仓库、HTTP 141 URL，原始结果 `tools/step1_survey_evidence/availability_2026-09-03/`）+ 我方复核（4 URL、2 repo、3 PyPI 包一致；并查出该批 PyPI 结果有 6 例假阳性，见附录三第 24 条）。

**本文档的核验边界（先读这一段）**：全表核过的是"文献是否真实存在"与"获取入口是否存活"两件事。**没有任何工具被安装或运行，没有任何分子被送进任何工具算过一次，因此表里没有一条预测值、也没有一次功能验证。** ✓ 只表示"能拿到"，不表示"跑得通"。

## 一、口径

**一行一工具，版本即身份**：工具同一性 = 名称 + 模型权重 + 训练数据 + 端点定义（ADR-0001，owner 裁定）。版本升级涉及模型重训或训练数据更换的，新旧版本是两个工具、各占一行；不改变预测值的补丁（如 ADMET-AI v2.0.0→v2.0.1 的 MPS→CPU 检查点修复）属同一工具，写在该行版本栏内。

**九列固定表头**：①工具（含版本）；②是什么/有什么用；③输入；④输出性质类型（逐点列出，按类别分组；端点特别多的平台在单元格内给"分类×数量+全部端点名"，原始逐点清单见附录六）；⑤发布时间（首发+当前版本与日期）；⑥发布方（论文一作/维护团队/厂商，以论文署名与托管域名为据）；⑦权威性（A=Nature/Science/Cell 系或官方权威机构；B=领域主流期刊 Bioinformatics/NAR/JCIM/J Cheminform/J Med Chem/Brief Bioinform/Nat 子刊/PNAS；C=预印本/技术报告；V=商业厂商自证）；⑧可用性（含义见下）；⑨来源文献（DOI/官方页）。

**⑧可用性列的准确含义**：记录**获取入口是否存活**，不是功能测试。✓ 的证据只有三类——PyPI JSON 返回 200 且带真实版本号、GitHub API 确认仓库存在、目标 URL 返回 200 且页面标题匹配。△=商业需 license／本环境网络拦截／通道未实测；✗=入口 404 或无现役入口，移附录五。功能可跑性须在 Step 1 建环境时逐个实跑，待办见附录七。

**两个已知的假阳性陷阱**（本轮实查到的，后续复用脚本必须避开）：其一，PyPI 的 404 响应体是 `{"message": "Not Found"}`，用 `grep "404 Not Found"` 判断会把不存在的包记成 200——轮 5 脚本因此产生 6 例假阳性；正确做法是取 HTTP 状态码或检查 JSON 里有无 `info` 字段。其二，本机公司代理对被拦域名返回 **HTTP 200 但内容是拦截页**（长度恒为 1461 字节），已确认 6 个 URL 属此类：alphafoldserver.com、drugdesigndata.org、torchdrug.ai、daylight.com/clogp、simulationsplus.com/gastroplus、molinspiration.com。

**可用性优先**：正文只留 ✓ 与 △；✗ 与"仅方法论文、无可用实现"的一律移附录五——调研用不起来的工具没有意义。

**性质类型为什么逐点列**：工具输出的性质清单决定它能进哪个冲突组（同性质才可比）。"41 个端点"这种总数没法分类讨论，必须落到端点名。

## 二、总体盘点

| 类别 | 正文条目 | ✓ 入口存活 | △ 受限/商业/待补测 | 出局（附录五） |
| --- | --- | --- | --- | --- |
| §三 ADMET 综合平台 | 17 | 14 | 3 | 3（ADMETlab 2.0、admetSAR 1.0、ADMETboost） |
| §四 BBB 渗透专项 | 16（端点 8+专用 8） | 15 | 1 | 1（LightBBB，服务器 404） |
| §五 物化性质 | 26 | 17 | 9 | 3（CLogP、miLogP、Epik 经典版） |
| §六 亲和力与对接 | 27 | 19 | 8 | 2（Vina 1.0 原版、X-Score） |
| §七 蛋白结构 | 25 | 23 | 2 | 0 |
| §八 毒性 | 20 | 11 | 9 | 0 |
| §九 成药性/可开发性 | 11 | 6 | 5 | 3（SAPS、DI、Zyggregator：仅方法无实现） |
| §十 分子生成与逆合成 | 7 | 5 | 2 | 0 |
| §十一 PBPK | 4 | 2 | 2 | 0 |
| §十二 蛋白设计 | 3 | 0 | 3（repo 待补测） | 0 |
| §十三 Agent 框架与基准 | 5 | 4 | 1 | 0 |
| §十四 框架与权威数据库 | 24 | 23 | 1 | 0 |
| 合计 | 185 | **139** | 46 | 12 |

> 139 是"入口能拿到"的条目数，其中**经过功能实跑的为 0**。

## 三、ADMET 综合平台

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ADMET-AI v1 系（在用 1.4.0） | Chemprop 多任务模型批量预测 ADMET+物化，提案 §1.3 在用版 | SMILES/CSV（≤1000/批） | 共 49 列 = 41 模型端点 + 8 物化直算。物化 8：MW、logP、HBA、HBD、Lipinski、QED、立体中心数、TPSA；吸收 8：人肠吸收 HIA、口服生物利用度、水溶性（AqSolDB）、亲脂性（AZ）、水合自由能（FreeSolv）、Caco-2 通透、PAMPA、P-gp 抑制；分布 3：BBB 渗透（Martins）、血浆蛋白结合 PPBR、稳态分布容积 VDss；排泄 3：半衰期、肝细胞清除率、微粒体清除率；代谢 8：CYP1A2/2C19/2C9/2D6/3A4 抑制 + CYP2C9/2D6/3A4 底物；毒性 19：hERG、临床毒性 ClinTox、AMES 致突变、DILI、致癌（Lagunin）、急性毒性 LD50、皮肤反应、NR-AR、NR-AR-LBD、NR-AhR、芳香化酶、NR-ER、NR-ER-LBD、PPARγ、SR-ARE、SR-ATAD5、SR-HSE、SR-MMP、SR-p53。每值附 DrugBank 已批准药物参考百分位 | 首发 1.0.0（2023-12-04）；1.4.0（2025-09-16） | Stanford（Swanson/Durrant 等，论文）；web 由 Greenstone Bio 托管 | B（Bioinformatics 2024） | ✓ pip：`admet-ai` 200（可指定 ==1.4.0；依赖 torch==2.5.0）；web admet.ai.greenstonebio.com 本环境 000 | Swanson et al., Bioinformatics 2024, [10.1093/bioinformatics/btae416](https://doi.org/10.1093/bioinformatics/btae416) |
| ADMET-AI v2（当前 2.0.1） | 同名同接口，但模型权重整体重训（Chemprop v1→v2、训练数据换 PyTDC 1.1.15），官方声明预测值不等于 v1——按 ADR-0001 是另一个工具 | 同上 | 共 52 列 = 41 模型端点（名单同 v1，权重重训）+ 11 直算列（v1 的 8 物化 + 新增 PAINS 警示、BRENK 警示、NIH 警示 3 列） | v2.0.0（2026-02-17）→ v2.0.1（2026-02-22，补丁：检查点 MPS→CPU 修 Linux/CUDA、移除 TDC 可选依赖） | 同上 | B（v1 论文；v2 无独立论文，权威来源=官方 release notes+仓库内置端点表，ADR-0001） | ✓ pip：`admet-ai==2.0.1` 200（2026-02-22，依赖 chemprop>=2.2.2）；GitHub swansonk14/admet_ai exists | 官方 release notes（api.github.com）；[PyPI](https://pypi.org/pypi/admet-ai/json) |
| ADMETlab 3.0 | 综合 ADMET+成药性平台，119 端点，带决策支持与 API | SMILES/SDF/CSV | 119 端点 = 物化 21 + 药化 20 + 吸收 9 + 分布 9 + 代谢 14 + 排泄 2 + 毒性 36 + 毒性基团规则 8；实现为 77 个 DMPNN 模型（18 回归+59 分类）+ 34 个 RDKit 直算 + 8 条药物规则。论文点名端点：pKa(acid/base)、熔点、沸点、口服生物利用度 F50%、PAMPA、CYP 抑制、人肝微粒体稳定性、肾毒性、神经毒性、耳毒性、血液毒性、遗传毒性、hERG、RPMI-8226/A549/Hek293 细胞毒、Caco-2、logD7.4、VDss、半衰期；完整清单在官网 Help 与 Supplementary S1（附录六） | 论文 2024（在线平台无版本号） | 中南大学 CBDD 组（scbdd.com；Fu L 等） | B（NAR 2024） | ✓ web 200：admetlab3.scbdd.com（标题"ADMETlab 3.0"）；不可本地、不可锁版 | Fu L et al., NAR 2024, [10.1093/nar/gkae236](https://doi.org/10.1093/nar/gkae236) |
| admetSAR 2.0 | QSAR 端点预测+相似结构检索+结构优化建议 | SMILES/手绘结构 | 端点覆盖吸收/分布/代谢/排泄/毒性各类（2.0 版清单官方页未逐项列出；3.0 版全清单见下一行与附录六） | 2018 在线/2019 见刊 | 华东理工大学 LMMD（ecust.edu.cn；Yang H 等） | B（Bioinformatics 2019） | ✓ web 200：lmmd.ecust.edu.cn/admetsar2/ | Yang H et al., Bioinformatics 2019, [10.1093/bioinformatics/bty707](https://doi.org/10.1093/bioinformatics/bty707) |
| admetSAR 3.0 | 同工具线当前版，116 个端点（官网端点页实测可数） | SMILES/手绘 | 官网端点页逐点 116 项：物化 13（MW、HBA、HBD、TPSA、原子数、环数、可旋转键、杂原子数、QED、SlogP、logP-CLMGraph、logS、pKa）；类药规则 3（Lipinski、Pfizer、GSK）；吸收 5（Caco-2 回归/分类、HIA、MDCK、口服 F）；分布 13（BBB、OATP1B1/2B1/1B3、OCT1/OCT2、BCRP、BSEP、MATE1、Pgp 抑制、Pgp 底物、PPB、VDss）；代谢 15（CYP1A2/2C9/2C19/2D6/3A4/2B6 抑制 6 + 同 6 亚型底物 + HLM、RLM、UGT 底物）；排泄 4（血浆清除率 CLp、肾清除率 CLr、T1/2、MRT）；人体毒性 30（神经毒、DILI、hERG×5 浓度档、呼吸毒、肾毒、眼腐蚀/刺激、皮肤腐蚀/刺激/致敏、急性经皮毒、Ames、小鼠/大鼠/啮类致癌 ×5、微核、生殖毒、线粒体毒、溶血毒、重复给药毒、急性经口毒 c/LD50、FDAMDD c/值）；EDCs/Tox21 通路 14（AR、ER、AR-LBD、ER-LBD、芳香化酶、AhR、ARE、ATAD5、HSE、p53、PPARγ、MMP、TR、GR）；生态毒 16（蜜蜂、鸟类×2、水生×9、四膜虫×2、BCF×2、生物降解）；光毒 3。注：论文称 119，官网端点页实数 116，两口径并存（附录六） | 2024 | 华东理工大学 LMMD（Gu Y 等） | B（NAR 2024） | ✓ web 200：lmmd.ecust.edu.cn/admetsar3/ 与端点页（67KB，实测解析） | Gu Y et al., NAR 2024, [10.1093/nar/gkae298](https://doi.org/10.1093/nar/gkae298) |
| DeepPurpose v0.1.5 | DTI/性质预测工具箱，15+ 编码器可组合；BBB 需在 TDC BBBP 上自训练（官方无现成 BBBP 权重） | SMILES+蛋白序列（DTI）；SMILES（性质） | DTI 模式：亲和力回归（Kd/Ki/IC50）+ 二分类；性质模式：用户指定数据集（BBBP 二分类等）单端点训练。官方预训练仅 DTI（BindingDB/DAVIS/KIBA） | v0.0.3（2020-12-25）→ v0.1.5（2021-10-26，已停更） | Kexin Huang 等（GitHub kexinhuang12345，Harvard/TDC 生态） | B（Bioinformatics 2020） | ✓ pip：`deeppurpose` 200（0.1.5）；GitHub exists | Huang K et al., Bioinformatics 2020, [10.1093/bioinformatics/btaa1005](https://doi.org/10.1093/bioinformatics/btaa1005) |
| pkCSM | 图签名（graph-based signatures）PK/毒性多端点预测 | SMILES/SDF | 30 个预测器（14 回归+16 分类）逐点：吸收 7（水溶性、Caco-2、人肠吸收、皮肤通透、P-gp 底物、P-gp 抑制 I/II）；分布 4（VDss、血浆游离分数 Fu、BBB 透过〔logBB 回归〕、CNS 通透）；代谢 7（CYP1A2/2C19/2C9/2D6/3A4 抑制、CYP2D6/3A4 底物）；排泄 2（总清除率、OCT2 肾转运）；毒性 10（hERG I/II、AMES、肝毒、皮肤致敏、人最大耐受剂量 MRTD、大鼠急性经口 LD50、大鼠慢性 LOAEL、四膜虫毒性、黑头呆鱼 LC50） | 2015（平台无版本号） | 昆士兰大学 biosig 实验室（uq.edu.au；Pires/Blundell/Ascher） | B（J Med Chem 2015） | ✓ web 200：biosig.lab.uq.edu.au/pkcsm/（prediction 页亦 200） | Pires et al., J Med Chem 2015, [10.1021/acs.jmedchem.5b00104](https://doi.org/10.1021/acs.jmedchem.5b00104) |
| Deep-PK | pkCSM 同组深度学习版，端点扩至 73 | 分子结构 | 73 端点 = 64 ADMET（吸收 8、分布 5、代谢 13、排泄 3、毒性 35）+ 9 一般物化；任务类型 49 分类+24 回归；数据源整合 ADMETlab 2.0、Interpretable-ADMET、toxCSM、pkCSM 四家。论文点名：NR-AR、NR-ER、口服生物利用度 F30%、OCT2 肾排泄；完整清单在 Supplementary S1（附录六） | 2024 | 昆士兰大学 biosig（Myung/Ascher 等） | B（NAR 2024） | ✓ web 200：biosig.lab.uq.edu.au/deeppk/ | Myung Y et al., NAR 2024, [10.1093/nar/gkae254](https://doi.org/10.1093/nar/gkae254) |
| SwissADME | 物化+药代+类药性+药化友好性一体化 web，含 BOILED-Egg 图 | SMILES（批量） | 约 30 项输出逐点：物化（MW、摩尔折射率 MR、TPSA、特定原子计数）；亲脂性 6（iLOGP、XLOGP3、WLOGP、MLOGP、SILICOS-IT、五者共识均值）；水溶性（ESOL logS、Ali logS、SILICOS-IT logS + mol/l、mg/ml 换算 + 六级定性）；药代 9（皮肤透过 logKp、HIA、BBB〔BOILED-Egg〕、P-gp 底物、CYP1A2/2C19/2C9/2D6/3A4 抑制〔6 个 SVM〕）；类药性 6（Lipinski、Ghose、Veber、Egan、Muegge、生物利用度评分）；药化友好 4（PAINS 481 片段、Brenk 105 基团、leadlikeness、SA 合成可及性）；另有 Bioavailability Radar 六轴与 2D 结构 | 2017 | SIB 瑞士生物信息学研究所（Daina/Zoete） | B（Sci Rep 2017） | ✓ web 200：swissadme.ch | Daina A et al., Sci Rep 2017, [10.1038/srep42717](https://doi.org/10.1038/srep42717) |
| vNN-ADMET | 可变最近邻（vNN）分类模型组，带适用域声明 | SMILES | 15 个模型逐点：细胞毒性、AMES 致突变、hERG 心脏毒、DILI、微粒体稳定性、DDI/CYP 相关等（15 模型全名单见官网，需登录） | 2017 | Schyman 等（bhsai.org 托管） | B（Front Pharmacol 2017） | ✓ web 200：vnnadmet.bhsai.org（登录页） | Schyman P et al., Front Pharmacol 2017, [10.3389/fphar.2017.00889](https://doi.org/10.3389/fphar.2017.00889) |
| PKSmart 3.0.1 | 开源体内 PK 参数回归 | SMILES | 3 端点逐点：清除率 CL、稳态分布容积 Vdss、游离分数 fup（派生 t½ 等按 repo 口径） | 2024（bioRxiv）；当前 3.0.1（2025-09-29，PyPI） | Srijit Seal 等（GitHub srijitseal） | B（J Cheminform 2025） | ✓ pip：`pksmart` 200（3.0.1，2025-09-29，本轮复核一致）；GitHub exists | Seal S et al., J Cheminform 2025, [10.1186/s13321-025-01066-5](https://doi.org/10.1186/s13321-025-01066-5) |
| OpenADMET | OMSF 开源 ADMET 模型训练/推理框架，配社区盲测（ExpansionRx 等） | SMILES | 框架性质：可训练任意 ADMET 端点；官方发布模型含 CYP/PXR 诱导等（HuggingFace 模型库为准） | 2024 启动 | OMSF（openadmet.org） | B（Nat Commun 2026 "Mapping the avoid-ome"，实测页面 200；轮 3 曾误撤该引用，本轮恢复） | ✓ GitHub OpenADMET/openadmet-models exists（源码 `pip install -e .`；PyPI 无 openadmet / openadmet-models / openadmet-toolkit，三名均 404，本轮复核）；openadmet.org 200 | [Nat Commun 2026, s41467-026-73410-8](https://www.nature.com/articles/s41467-026-73410-8)（实测 200） |
| XenoSite | CYP450 代谢位点（SoM）预测，含 UGT 与反应性模型族 | SMILES | 原子级代谢位点概率逐点：P450 位点、UGT 位点、反应性代谢物位点（模型族以站点为准） | 2013（模型）/2015（服务器） | Zaretzki/Matlock 等（GitHub swamidass） | B（JCIM 2013 + Bioinformatics 2015） | ✓ web 200：xenosite.org；GitHub swamidass/xenosite 可本地 | Zaretzki J et al., JCIM 2013, [10.1021/ci400518g](https://doi.org/10.1021/ci400518g)；Matlock MK et al., Bioinformatics 2015, [btu761](https://doi.org/10.1093/bioinformatics/btu761) |
| OCHEM v4.3.202 | 在线 QSAR 建模环境：自建模型+内置数百 ADMET/理化模型，承办 Tox24/EUOS25 盲测 | SMILES/SDF | 内置模型覆盖 logP、溶解度、AMES、hERG、熔点等（站内模型库为准，注册用户可查全清单）；另托管实测数据源 | 2011；平台 v4.3.202（页脚） | Tetko IV 等（ochem.eu） | B（J Comput Aided Mol Des 2011） | ✓ web 200：ochem.eu + docs.ochem.eu | Sushko I et al., [10.1007/s10822-011-9440-2](https://doi.org/10.1007/s10822-011-9440-2) |
| QikProp（Release 2026-3） | 商业快速 ADME/物化性质筛选 | 3D 结构（Maestro 内） | 50+ 描述符与 ADME 性质（厂商口径 12+ 关键项逐点：QPlogP、QPlogS、Caco-2、BBB、CYP 相关、Rule-of-5 等；完整清单随 Suite 文档） | Release 2026-3（2026-07-31） | Schrödinger | V（产品页，无独立方法文献） | △ 商业 license；产品页 200（软件本体需 license 实测） | [产品页](https://www.schrodinger.com/platform/products/qikprop/) |
| ACD/Percepta | 商业理化+ADME/Tox 套件（Classic+GALAS 双算法） | 结构文件 | logP/logD、pKa、溶解度、CYP、毒性模块等（厂商模块清单为准） | 版本官方未注明（帮助站点 v2021 文档） | ACD/Labs | V | △ 商业；acdlabs.com 本环境 000（需换网络） | [产品页](https://www.acdlabs.com/products/percepta-platform/) |
| ADMET Predictor | 商业大规模 ADMET 模型库（厂商称 1000+ 模型） | 结构 | 吸收/分布/代谢/排泄/毒性全类端点（厂商模块清单为准） | 版本官方未注明 | Simulations Plus | V | △ 商业需 license；产品页 200（软件本体未验证） | [产品页](https://www.simulationsplus.com/software/admetpredictor/) |

> 出局（附录五）：ADMETlab 2.0（被 3.0 取代，旧入口未实测）、admetSAR 1.0（2012，无现役入口）、ADMETboost（admetboost.scbdd.com 实测 000，同域 admetlab3 为 200，疑似下线）。

## 四、BBB 渗透专项

### 4.1 通用平台的 BBB 端点（端点即工具）

| 工具（端点） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ADMET-AI bBBB（v1/v2 各一） | 41 端点中的血脑屏障渗透端点；v1 与 v2 权重不同=两个工具 | SMILES | 1 项：BBB 渗透（BBB_Martins，分类概率+DrugBank 百分位） | 随 §三 两版本行 | 同 §三 | B | ✓ pip（同 §三） | 同 §三 |
| DeepPurpose BBBP 自训练 | 在 TDC BBBP 上自训练的二分类模型（官方无现成权重；提案 §1.3 三模型即此产物） | SMILES | 1 项：BBBP 二分类（可换 CNN/Morgan/MPNN 三编码器=三个模型） | v0.1.5（2021-10-26） | 同 §三 DeepPurpose | B | ✓ pip 200；复现须记录训练配置 | 同 §三 |
| ADMETlab 3.0 BBB | 119 端点中的 BBB 端点 | SMILES | 1 项：BBB 渗透分类 | 2024 | 中南大学 CBDD | B | ✓ web 200 | 同 §三 |
| pkCSM BBB | 30 端点中的 BBB 端点，为 logBB 回归（非二分类） | SMILES | 1 项：logBB 回归值 | 2015 | 昆士兰大学 biosig | B | ✓ web 200 | 同 §三 |
| admetSAR 3.0 BBB | 116 端点中的 BBB+/BBB− 分类端点 | SMILES/手绘 | 1 项：BBB 分类 | 2024 | 华东理工 LMMD | B | ✓ web 200 | 同 §三 |
| SwissADME BOILED-Egg | 物理规则法（WLOGP×TPSA 二维图），非 ML | SMILES | 2 项：BBB permeation（蛋黄区）+ GI 吸收（蛋白区），并标 P-gp 底物着色 | 方法 2016/平台 2017 | SIB（Daina/Zoete） | B（ChemMedChem 2016） | ✓ web 200 | Daina & Zoete, [10.1002/cmdc.201600182](https://doi.org/10.1002/cmdc.201600182) |
| PreADMET BBB | 早期 ADME web 服务的 BBB 二分类端点 | SMILES | 1 项：BBB 渗透分类 | 端点页 2015 | BMDRC（webservice.bmdrc.org） | C（端点页无独立文献，平台有历史文献） | ✓ web 200（主站与 BBB 端点页均 200，推翻轮 3 的 403 记录） | [端点页](https://preadmet.webservice.bmdrc.org/2015/01/28/blood-brain-barrier-penetration/) |
| VEGA BBB | Benfenati 组 QSAR 模型族中的 BBB/logBB 模型 | SMILES | 1 项：BBB 渗透分类（VEGA 桌面版另有 logBB 模型） | 首发年份官方未注明 | 米兰大学 Benfenati 组（ddl.unimi.it） | B（模型族被 ECHA 引用；QSAR without borders 综述 Chem Soc Rev 2020） | ✓ web 200：vegaol/bbbp.htm（标题"BBB predictor"）+ vegahub.eu 200 + vegaol 根 200（推翻轮 3 的 403 记录）；桌面版可下载 | [模型页](https://www.ddl.unimi.it/vegaol/bbbp.htm) |

### 4.2 BBB 专用工具

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DeepB3 | 三路融合（文本+分子图+描述符）深度学习 BBB 二分类 | SMILES | 1 项：BBB 二分类 | 2022 | GreatChenLab（cbcb.cdutcm.edu.cn） | B（Brief Bioinform 2022） | ✓ web 200 + GitHub GreatChenLab/Deep-B3 exists（锁 commit） | Tang Q et al., [10.1093/bib/bbac357](https://doi.org/10.1093/bib/bbac357) |
| B3clf（Zenodo v0.0.1） | B3DB 官方配套分类器，XGBoost 等 4 算法×24 模型可选，带不确定性输出 | SMILES/SDF | 1 项：BBB 二分类概率+标签（24 个候选模型） | 论文 2021；归档 v0.0.1（2023-09-12） | theochem 组织（B3DB 团队） | B（Sci Data 2021） | ✓ GitHub theochem/B3clf exists（pushed 2025-08-12，本轮复核）；Zenodo DOI 10.5281/zenodo.8336769 可锁版 | Meng et al., [10.1038/s41597-021-01069-5](https://doi.org/10.1038/s41597-021-01069-5) |
| graphB3 | 可解释图学习 BBB 二分类，带 GNNExplainer 原子级解释 | SMILES CSV | 1 项：BBB 二分类 + 可解释性高亮 | 2025 | dhanjal-lab（IIIT Delhi，webs.iiitd.edu.in） | B（Brief Bioinform 2025） | ✓ web 200（标题 graphB³）+ GitHub dhanjal-lab/graphB3 exists（pushed 2025-04-07，本轮复核） | Kumar S et al., [10.1093/bib/bbaf679](https://doi.org/10.1093/bib/bbaf679) |
| BBBper | 随机森林 BBB 二分类 web（自称 97% 准确率） | 理化参数 | 1 项：BBB 渗透分类 | 2026 | MDU（bbbper.mdu.ac.in；Pawan Kumar 等） | B（CNS Neurol Disord Drug Targets 2026） | ✓ web 200：bbbper.mdu.ac.in | [10.2174/0118715273328174241007060331](https://doi.org/10.2174/0118715273328174241007060331) |
| LogBB_Pred | LightGBM 定量 logBB 回归（R²=0.61，913 化合物训练） | SMILES | 1 项：logBB 回归值（注意与二分类工具口径不同） | 2023 | Shaker B 等（ssbio.cau.ac.kr） | B（Bioinformatics 2023） | ✓ web 200：ssbio.cau.ac.kr/software/logbb_pred/（同站 LightBBB 页已 404，此端点存活） | Shaker B et al., [10.1093/bioinformatics/btad577](https://doi.org/10.1093/bioinformatics/btad577) |
| LGBM-BBB | LightGBM BBB 二分类 web-server（预印本） | SMILES | 1 项：BBBP+/− 二分类 | 预印本 v1（2026-06-10，Under Review） | Yadav V 等（GitHub Rajnishphe） | C（预印本，无同行评审） | △ GitHub 仓库未在工程师 60 仓实测清单内，需补核 | [10.21203/rs.3.rs-10069539/v1](https://doi.org/10.21203/rs.3.rs-10069539/v1) |
| DeePred-BBB | DNN（Keras/TF）BBB 二分类 | SMILES 特征化 | 1 项：BBB 二分类 | 2022 | Kumar R 等（GitHub 12rajnish） | B（Front Neurosci 2022） | ✓ GitHub 12rajnish/DeePred-BBB exists（锁 commit） | [10.3389/fnins.2022.858126](https://doi.org/10.3389/fnins.2022.858126) |
| SAI BBB Predictor v3.0.0 | 多任务图注意力网络，单 SMILES 同测 12 个 CNS 药代端点 | SMILES | 12 项 CNS 端点（含 BBB 渗透；其余为 CNS 药代类端点，全名单随 Zenodo 包 CSV） | v3.0.0（2026-03-24） | Gunanathan K & Sivaramakrishnan V（Zenodo） | C（Zenodo 存档，无期刊） | ✓ Zenodo 200（DOI 10.5281/zenodo.19205279 锁版；数据+权重+脚本齐全） | [Zenodo](https://zenodo.org/records/19205279) |

> 出局（附录五）：LightBBB——论文在（Bioinformatics 2021, PMID 33112379），但唯一入口 ssbio.cau.ac.kr/software/bbb 实测 404，无官方代码仓库，当前不可用。同站 LogBB_Pred 存活，说明是单页下线而非整站消失。
> BBB 背景文献：[多工具对同一分子矛盾实证, Front Pharmacol 2025](https://doi.org/10.3389/fphar.2025.1616144)；[BBB 预测模型综述, PMC11949286](https://pmc.ncbi.nlm.nih.gov/articles/PMC11949286/)。

## 五、物化性质

### 5.1 logP（辛醇/水分配）

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XLogP3 | 原子贡献+片段加和 logP，PubChem 内置标准值 | SMILES/CID | 1 项：logP（87 种原子/基团类型+2 校正项，训练集 8,199） | 方法固定 2007 | PubChem/NCBI（Cheng T 等） | A（官方权威库内置） | ✓ PubChem PUG REST API 轮 3 实测调用成功 | Cheng T et al., JCIM 2007, [10.1021/ci700257y](https://doi.org/10.1021/ci700257y) |
| ALOGPS 2.1 | 联想神经网络（ASNN）logP+logS 双输出 | SMILES/结构 | 2 项：logP、logS（训练锚 PHYSPROP） | 方法固定 2002 | Tetko IV 等（VCCLAB） | B（JCICS 2002） | ✓ web 200：vcclab.org/lab/alogps | Tetko IV et al., JCICS 2002, [10.1021/ci025515j](https://doi.org/10.1021/ci025515j) |
| MLOGP | 拓扑法 logP（13 描述符线性回归），SwissADME 内置 | SMILES | 1 项：logP | 方法固定 1992 | Moriguchi I 等；经 SIB SwissADME 分发 | B（Chem Pharm Bull 1992） | ✓ 经 swissadme.ch（200） | Moriguchi I et al., [10.1248/cpb.40.127](https://doi.org/10.1248/cpb.40.127) |
| JPlogP | 可扩展原子类型加和+共识模型 logP，已并入 CDK 描述符 | 分子结构 | 1 项：logP（测试锚 Martel 707 实测集） | 论文 2018；2018-12-15 入 CDK | Plante J & Werner S；CDK 社区维护 | B（J Cheminform 2018） | △ CDK 库（JPlogPDescriptor）未在本轮 60 仓实测清单，Maven/GitHub 公开发布 | Plante J et al., [10.1186/s13321-018-0316-5](https://doi.org/10.1186/s13321-018-0316-5) |
| WLOGP | Wildman-Crippen 纯原子贡献 logP，SwissADME 内置 | SMILES | 1 项：logP | 方法固定 1999 | Wildman & Crippen；经 SIB 分发 | B（JCICS 1999） | ✓ 经 swissadme.ch（200） | Wildman SA et al., [10.1021/ci990307l](https://doi.org/10.1021/ci990307l) |
| SILICOS-IT logP | 混合法（27 片段+7 拓扑描述符），SwissADME 内置；filter-it 开源 | SMILES | 1 项：logP | filter-it 1.0.2 | SILICOS-IT（比利时）；经 SIB 分发 | C（无独立期刊论文，经 SwissADME 论文核实） | ✓ 经 swissadme.ch（200）；filter-it repo 未实测 | SwissADME 论文（Sci Rep 2017） |
| KowWIN v4.11（EPI Suite） | EPA 监管用片段法 log Kow 估算 | SMILES/结构 | 1 项：log Kow | v4.11（EPA 页面） | 美国 EPA（Meylan/Howard 方法） | A（监管机构官方软件） | △ epa.gov 下载页本环境 000（官方软件实际可下载，需换网络） | EPA 官方页 |
| ACD/Percepta LogP | 商业双通道（Classic+GALAS）logP/logD | 结构文件 | 2 项：logP、logD | 版本官方未注明 | ACD/Labs | V | △ 商业；acdlabs.com 本环境 000 | 产品页 |
| ChemAxon cxcalc logP | 商业 CLI 计算 logP/logD | SMILES/SDF | 2 项：logP、logD | 版本官方未注明 | ChemAxon（Certara 旗下） | V | △ 商业；docs.chemaxon.com 200（文档可达，软件需 license） | 官方文档 |
| QikProp QPlogP | 商业套件内 logP 项 | 3D 结构 | 2 项：QPlogP、QPlogS（套件 50+ 描述符之二） | Release 2026-3（2026-07-31） | Schrödinger | V | △ 商业；产品页 200 | 产品页 |

### 5.2 水溶性

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ESOL | 9 描述符线性回归 logS，溶解度领域最常用基线（无官方软件，经 SwissADME/RDKit 生态实现） | 结构描述符（clogP、MW、芳香原子比例、可旋转键） | 1 项：logS（训练集 2,874 条实测） | 方法固定 2004 | Delaney JS | B（JCICS 2004） | ✓ 经 swissadme.ch（200）；MoleculeNet/DeepChem 转载实现 | Delaney JS, JCICS 2004, [10.1021/ci034243x](https://doi.org/10.1021/ci034243x) |
| AqSolDB | 9,982 化合物水溶性参考数据集（9 源整合，带 G1-G5 可靠性分组），亦是多个模型的训练底座 | —（数据集） | 1 项：logS 实测值+可靠性标签 | 2019 定库 | Sorkun MC 等（GitHub mcsorkun） | B（Sci Data 2019） | ✓ GitHub mcsorkun/AqSolDB exists | Sorkun MC et al., [10.1038/s41597-019-0151-1](https://doi.org/10.1038/s41597-019-0151-1) |
| SolTranNet 1.0.0 | Molecule Attention Transformer（仅 3,393 参数）预测 logS | SMILES | 1 项：logS（AqSolDB 3 折 scaffold CV RMSE 1.459） | 1.0.0（2021-03-17，PyPI 唯一版本） | Francoeur PG 等（gnina 组） | B（JCIM 2021） | ✓ pip：`soltrannet` 200（1.0.0） | Francoeur PG et al., [10.1021/acs.jcim.1c00331](https://doi.org/10.1021/acs.jcim.1c00331) |
| SolPredictor | 残差门控图神经网络（RGNN）logS | SMILES | 1 项：logS（10 折 CV R²=0.79±0.02） | 2024 | Ahmad W 等（GitHub waqarahmadm019） | B（Int J Mol Sci 2024） | ✓ GitHub exists | Ahmad W et al., [10.3390/ijms25020715](https://doi.org/10.3390/ijms25020715) |
| Rowan 3.1.14 | 商业云 API：logP 三方法+水/多溶剂溶解度带不确定度 | SMILES | 4 项：logP（Chemprop/Crippen/COSMO-RS 三通道）、水溶解度 log(mol/L)+不确定度 | 0.0.1（2023-11-20）→3.1.14（2026-08-29，PyPI） | Rowan Technologies | V（官方文档，无期刊） | ✓ pip：`rowan-python` 200（3.1.14，本轮复核一致）+ web 200；云端计算需 API key | 官方文档 |

### 5.3 pKa

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| QupKake | GNN+GFN2-xTB 半经验 QM 混合，逐位点 micro-pKa | SMILES/SDF/CSV | 2 项：逐位点 micro-pKa、微观质子化态（输出带标注 SDF） | 首发 2023 | Abarbanel & Hutchison（GitHub Shualdon） | B（JCTC 2024） | ✓ GitHub Shualdon/QupKake exists | Abarbanel & Hutchison, JCTC 2024, [10.1021/acs.jctc.4c00328](https://doi.org/10.1021/acs.jctc.4c00328) |
| MoKa 5.1.0 | GRID 分子相互作用场经验模型 pKa+物种分布 | 结构文件 | 2 项：可电离位点 pKa、给定 pH 物种分布（训练 >25,000） | 首发约 2007；当前 5.1.0 | Molecular Discovery（Milletti 等） | V+B（JCIM 2007 方法文献） | ✓ 商业；moldiscovery.com 200 | Milletti et al., JCIM 2007 |
| Marvin pKa | 偏电荷+增量式 QSPR，micro/macro pKa | 分子 | 4 项：micro/macro pKa、主要微观物种、等电点、pH 分布图 | 版本官方未注明 | ChemAxon | V | △ 商业；docs.chemaxon.com 200 | 官方文档 |
| Epik 7 | 原子级图卷积 NN 集成，质子化态/互变异构+pKa（替代早期 QM 流程） | 2D/3D 结构 | 3 项：质子化态与互变异构枚举、各态 pKa、自由能排序（训练 >42,000 实验 pKa） | 2023（Epik 7） | Schrödinger（Johnston 等） | V+B（JCTC 2023） | △ 商业；pubmed 37023332 页 200 | Johnston et al., JCTC 2023, [10.1021/acs.jctc.3c00044](https://doi.org/10.1021/acs.jctc.3c00044) |
| ACD/Percepta pKa | Hammett 型/QSPR+结构库，全位点表观 pKa | 结构 | 2 项：各位点表观 pKa（最酸/最碱）、电离曲线（训练 >26,000） | 版本官方未注明 | ACD/Labs | V | △ 商业；acdlabs.com 本环境 000 | 产品页 |
| OpenEye Quacpac | 规则+经验模型，pKa/质子化态/互变体+电荷（AM1-BCC/MMFF94） | 分子 | 3 项：pKa、质子化态/互变异构枚举、原子电荷 | 版本官方未注明 | OpenEye | V | △ 商业；eyesopen.com 本环境 000 | 产品页 |
| Uni-pKa | Uni-Mol 预训练-微调 NN 自由能预测器+微观态枚举，热力学一致 macro-pKa | SMILES | 1 项：macro-pKa（经微观态枚举重构质子化平衡） | 2024 | 深势科技 dptech-corp（Luo W 等） | B（JACS Au 2024） | ✓ GitHub dptech-corp/Uni-pKa exists | Luo W et al., JACS Au 2024, [10.1021/jacsau.4c00271](https://doi.org/10.1021/jacsau.4c00271) |

### 5.4 其他物化

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TPSA（RDKit CalcTPSA） | Ertl 片段加和法拓扑极性表面积（BOILED-Egg 输入之一） | mol/SMILES | 1 项：TPSA（Å²） | 方法 2000；随 RDKit 滚动 | RDKit 社区（Landrum）；方法 Ertl 等 | B（J Med Chem 2000） | ✓ pip：`rdkit` 200（2026.3.5） | Ertl P et al., [10.1021/jm000942e](https://doi.org/10.1021/jm000942e) |
| 熔点/热解点模型（OCHEM） | 13 套描述符（>70 万维）+共识 ML，药物样空间误差约 32°C | 结构 | 3 项：熔点回归、热分解点回归、"分解 vs 正常熔化"分类（约 30 万专利数据点） | 2016 | Tetko/Lowe/Williams（OCHEM 平台） | B（J Cheminform 2016） | ✓ 经 ochem.eu（200） | Tetko IV et al., [10.1186/s13321-016-0113-y](https://doi.org/10.1186/s13321-016-0113-y) |
| QED（RDKit） | 8 描述符加权类药性评分 | SMILES | 1 项：QED（0-1） | 2012 | RDKit 内置；方法 Bickerton 等 | A（Nat Chem 2012） | ✓ pip：`rdkit` 200 | Bickerton et al., Nat Chem 2012, [10.1038/nchem.1243](https://doi.org/10.1038/nchem.1243) |
| SA score（RDKit contrib） | 片段贡献+分子复杂度合成可及性评分 | SMILES | 1 项：SA score（1 易–10 难） | 2009 | RDKit contrib；方法 Ertl & Schuffenhauer | B（J Cheminform 2009） | ✓ pip：`rdkit` 200 | Ertl & Schuffenhauer, [10.1186/1758-2946-1-8](https://doi.org/10.1186/1758-2946-1-8) |

> 出局（附录五）：CLogP（BioByte，biobyte.com 拦截且无存档）、miLogP（Molinspiration，域名拦截）、Epik 经典版（被 Epik 7 取代，无法独立获得）。

## 六、亲和力与对接

### 6.1 亲和力/DTI 回归（序列/图输入）

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DeepDTA | 双塔 CNN 亲和力回归，DTI 领域最常用基线 | SMILES+蛋白序列 | 1 项：结合亲和力回归值（Ki/Kd；训练锚 DAVIS、KIBA） | 2018 | Öztürk H 等（GitHub hkmztrk） | B（Bioinformatics 2018） | ✓ GitHub hkmztrk/DeepDTA exists（锁 commit） | Öztürk et al., [10.1093/bioinformatics/bty593](https://doi.org/10.1093/bioinformatics/bty593) |
| WideDTA | DeepDTA 加文本词特征（结构域/motif）的宽线性变体 | SMILES+序列+文本词 | 1 项：亲和力回归（锚 KIBA） | 2019 | Öztürk H 等 | C（arXiv 预印本） | △ 官方代码托管页未实测 | Öztürk et al., [arXiv:1902.04166](https://arxiv.org/abs/1902.04166) |
| GraphDTA | 分子图 GNN（GAT/GCN/GIN）+序列亲和力回归 | 分子图+蛋白序列 | 1 项：亲和力回归（锚 DAVIS、KIBA） | 2020/2021 | Nguyen T 等（GitHub thinng） | B（Bioinformatics 2021） | ✓ GitHub thinng/GraphDTA exists | Nguyen et al., [10.1093/bioinformatics/btaa921](https://doi.org/10.1093/bioinformatics/btaa921) |
| DeepAffinity | 统一 RNN-CNN+注意力，半监督预训练，IC50/Ki 回归 | SMILES+序列 | 1 项：IC50/Ki 回归（pKd/pKi） | 2019 | Shen-Lab（Karimi M 等） | B（Bioinformatics 2019） | ✓ GitHub Shen-Lab/DeepAffinity exists | Karimi et al., Bioinformatics 2019 |
| TankBind | 三角感知 NN，结合位点+pose+亲和力三输出 | 蛋白结构+配体 | 2 项：结合 pose、亲和力打分（锚 PDBbind） | 2022 | Lu W 等（GitHub luwei0917） | A（NeurIPS 2022） | ✓ GitHub luwei0917/TankBind exists | Lu et al., NeurIPS 2022 |
| DeepPurpose（DTI 模式） | 15+ 编码器自由组合的 DTI 回归/二分类 | SMILES+序列 | 2 项：亲和力回归、DTI 二分类（官方预训练锚 BindingDB/DAVIS/KIBA） | v0.1.5（2021-10-26） | Kexin Huang 等 | B（Bioinformatics 2020） | ✓ pip：`deeppurpose` 200 | Huang K et al., [10.1093/bioinformatics/btaa1005](https://doi.org/10.1093/bioinformatics/btaa1005) |

### 6.2 对接与打分（结构输入）

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| AutoDock Vina 1.2.7 | 经验打分+局部优化对接，1.2 系列加新对接方法/扩展力场/Python 绑定 | 受体 PDBQT+配体 | 2 项：结合 pose（多构象）、经验结合能 kcal/mol | v1.2.0 论文 2021；当前 v1.2.7（2025-02-26，GitHub API） | Scripps（ccsb-scripps；Trott/Eberhardt 等） | A+B（J Comput Chem 2010 原版；JCIM 2021） | ✓ pip：`vina` 200（1.2.7）；GitHub exists | Eberhardt et al., JCIM 2021, [10.1021/acs.jcim.1c00203](https://doi.org/10.1021/acs.jcim.1c00203) |
| Smina | Vina 分支，打分项可定制、能量最小化增强 | 受体+配体 | 2 项：pose、自定义打分项（锚 CSAR 2011） | 2013；最新文件 2021-12-01 | Koes D 等（SourceForge） | B（JCIM 2013） | ✓ SourceForge 文件页轮 3 实测（smina.osx.12） | Koes et al., [10.1021/ci300604z](https://doi.org/10.1021/ci300604z) |
| QuickVina 2 | Vina 启发式加速版 | 同 Vina | 2 项：pose、打分 | 2015 | Alhossary M 等（NTU；qvina.github.io） | B（Bioinformatics 2015） | ✓ web 200：qvina.github.io | Alhossary et al., [10.1093/bioinformatics/btv082](https://doi.org/10.1093/bioinformatics/btv082) |
| Gnina 1.3.3 | 对接+CNN 重打分，输出传统分与学习分双通道 | 受体+配体 | 3 项：pose、Vina 经验分、CNN 亲和力/pose 分 | v1.0 论文 2021；当前 v1.3.3（2026-06-29） | gnina 组（McNutt/Koes 等） | B（J Cheminform 2021/2025） | ✓ GitHub gnina/gnina exists；Docker 可锁版 | McNutt et al., [10.1186/s13321-021-00522-2](https://doi.org/10.1186/s13321-021-00522-2) |
| DiffDock | SE(3) 扩散生成盲对接+置信度 | 蛋白 PDB+配体 SMILES | 3 项：多 pose、扩散置信度（c>0 可信）、（DiffDock-L 扩展） | 2022；ICLR 2023 | Corso G 等（GitHub gcorso） | A（ICLR 2023） | ✓ GitHub gcorso/DiffDock exists（git clone+conda 安装；**PyPI 无 diffdock/DiffDock 包，404，本轮复核**）；ICLR 页 200 | Corso et al., ICLR 2023 |
| Pafnucy | 3D 体素化+3D CNN 复合物亲和力打分 | 复合物 mol2/SDF | 1 项：pK 亲和力打分（锚 PDBbind） | 2018 | cheminfIBB（Stepniewska-Dziubinska 等） | B（Bioinformatics 2018） | ✓ GitLab cheminfIBB/pafnucy 轮 3 开页 | [10.1093/bioinformatics/bty374](https://doi.org/10.1093/bioinformatics/bty374) |
| OnionNet | 多层分子间接触计数 2D CNN 亲和力回归 | 复合物结构 | 1 项：亲和力回归（锚 PDBbind v2016） | 2019 | Zheng Z 等（GitHub zhenglz） | B（ACS Omega 2019） | ✓ GitHub zhenglz/onionnet exists | Zheng et al., [10.1021/acsomega.9b01997](https://doi.org/10.1021/acsomega.9b01997) |
| OnionNet-2 | 残基-原子接触壳层特征 CNN 回归 | 复合物结构 | 1 项：亲和力回归（锚 PDBbind v2019、CASF-2016/2013、CSAR NRC-HiQ） | 2021 | Wang Z 等（GitHub zchwang） | B（Front Chem 2021） | ✓ GitHub zchwang/OnionNet-2 exists | Wang Z et al., [10.3389/fchem.2021.753002](https://doi.org/10.3389/fchem.2021.753002) |
| RF-Score | 复合物原子对计数特征+随机森林亲和力打分 | 复合物结构 | 1 项：亲和力打分 | 2010 | Ballester & Mitchell | B（Bioinformatics 2010） | △ 论文随附可执行，托管页未实测 | Ballester & Mitchell, [10.1093/bioinformatics/btq112](https://doi.org/10.1093/bioinformatics/btq112) |
| RF-Score-VS | RF-Score 特征体系用于化合物库虚拟筛选排序 | 复合物结构 | 1 项：筛选排序分 | 2017 | Wójcikowski 等（ODDT） | B（Sci Rep 2017） | ✓ GitHub oddt/rfscorevs exists | Wójcikowski et al., [10.1038/srep46710](https://doi.org/10.1038/srep46710) |
| NNScore 1.0 | Vina 描述符+前馈 NN 打分函数首版 | 复合物 | 1 项：结合打分 | 2010 | Durrant & McCammon（durrantlab） | B（JCIM 2010） | ✓ GitHub durrantlab/nnscore1 exists | Durrant & McCammon, JCIM 2010, [10.1021/ci100244v](https://doi.org/10.1021/ci100244v) |
| NNScore 2.0 | 描述符扩展版打分函数 | 复合物 | 1 项：结合打分 | 2011 | Durrant & McCammon | B（JCIM 2011） | △ 2.0 专用 repo 未实测 | [10.1021/ci2003889](https://doi.org/10.1021/ci2003889) |
| rDock | 开源对接（GA/MC/SA 三种搜索，打分可定制） | 受体+配体 | 2 项：pose、打分 | 2014-04-10 | Ruiz-Carmona 等 | B（PLOS Comput Biol 2014） | △ 开源本地，repo 未在本轮实测清单 | [10.1371/journal.pcbi.1003571](https://doi.org/10.1371/journal.pcbi.1003571) |
| KDEEP | 3D CNN 复合物绝对亲和力（pK）回归 | 复合物 | 1 项：pK 回归 | 2018 | Jiménez-García 等 | B（JCIM 2018） | △ 论文代码，托管页未实测 | [10.1021/acs.jcim.7b00650](https://doi.org/10.1021/acs.jcim.7b00650) |
| FABind | 几何深度学习柔性蛋白-配体结合预测 | 蛋白+配体 | 2 项：结合构象、亲和力 | 2023（NeurIPS，repo 2023-10-10） | QizhiPei 等 | A（NeurIPS 2023） | ✓ GitHub QizhiPei/FABind exists（MIT） | [arXiv:2310.06763](https://arxiv.org/abs/2310.06763) |
| EquiBind | SE(3) 等变 GNN 刚性盲对接 | 蛋白+配体 | 1 项：结合 pose | 2022 | Stark H 等 | C（arXiv 2022） | ✓ GitHub HannesStark/EquiBind exists（MIT） | [arXiv:2202.05146](https://arxiv.org/abs/2202.05146) |
| Uni-Mol Docking | 深势 Uni-Mol 系 3D 预训练对接+亲和力 | 蛋白口袋+配体 | 2 项：pose、亲和力 | 2024（arXiv）；LNCS 2026 | 深势科技 dptech-corp | C→B（arXiv+LNCS 2026） | ✓ GitHub dptech-corp/Uni-Mol exists | [arXiv:2405.11769](https://arxiv.org/abs/2405.11769) |
| Vina-GPU 2.0 | OpenCL GPU 加速 Vina | 受体+配体 | 2 项：pose、打分（同 Vina） | 2023（repo 2022-09-19） | 南京邮电 DeltaGroupNJUPT | B（JCIM 2023） | ✓ GitHub DeltaGroupNJUPT/Vina-GPU-2.0 exists | [10.1021/acs.jcim.2c01504](https://doi.org/10.1021/acs.jcim.2c01504) |
| RosettaLigand | Rosetta 全柔性受体-配体对接 | 受体+配体 | 2 项：pose、Rosetta 能量分 | 方法 2006；标准化流程 2013 | RosettaCommons（Combs 等） | B（Nat Protoc 2013） | △ 学术免费 license（非开源，需申请） | Combs et al., [10.1038/nprot.2013.074](https://doi.org/10.1038/nprot.2013.074) |
| Glide | 商业系统搜索+经验打分对接（SP/XP） | 受体网格+配体 | 2 项：pose、GlideScore | 首发 2004；随 Suite 年度更新 | Schrödinger（Friesner 等） | V+B（J Med Chem 2004） | △ 商业；产品页 200 | Friesner et al., J Med Chem 2004 |
| Gold | 商业遗传算法柔性对接 | 受体+配体 | 2 项：pose、四种打分（GoldScore/ChemScore/ASP/ChemPLP） | 首发约 1997；随 CSD 年度发布 | CCDC（Jones 等） | V+B（J Mol Biol 1997） | △ 商业；ccdc.cam.ac.uk 200 | Jones et al., [10.1006/jmbi.1996.0897](https://doi.org/10.1006/jmbi.1996.0897) |

> 出局（附录五）：AutoDock Vina 1.0 原版（2010，文献基准版本，现役下载通道未实测，使用一律以 1.2.7 行）、X-Score（2002，原版分发渠道未核实）。跨节：Boltz-2 亲和力头、Chai-1、NeuralPLexer 见 §七；CASF-2016/DUD-E 见 §十四。

## 七、蛋白结构与折叠

### 7.1 单链/复合物折叠

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| AlphaFold2 v2.3.2 | Evoformer+结构模块，单链与多聚体结构预测的行业基准 | 序列+MSA | 2 项：PDB 坐标、逐残基置信度 pLDDT（Multimer 加 PAE/ipTM） | 2021；v2.3.2（2023-04-05） | Google DeepMind | A（Nature 2021） | ✓ GitHub google-deepmind/alphafold exists；权重 Apache 2.0 | Jumper et al., [10.1038/s41586-021-03819-2](https://doi.org/10.1038/s41586-021-03819-2) |
| AlphaFold3 v3.0.4 | 扩散模型全分子复合物（蛋白/核酸/配体/离子） | 序列+SMILES | 3 项：复合物坐标、置信度、配体 pose | 2024-05；repo v3.0.0（2024-11-11）→v3.0.4（2025-07-28） | Google DeepMind | A（Nature 2024） | ✓ GitHub google-deepmind/alphafold3 exists（推理码开源，权重向 Google 申请、非商用）；web alphafoldserver.com 本环境域名拦截 | Abramson et al., [10.1038/s41586-024-07487-w](https://doi.org/10.1038/s41586-024-07487-w) |
| AlphaFold-Multimer | AF2 的蛋白复合物扩展 | 多序列+MSA | 2 项：复合物坐标、pTM/ipTM | bioRxiv 2021-10（v2 2022-03） | Google DeepMind（Evans 等） | C（bioRxiv） | ✓ 随 AF2/ColabFold 代码库 | Evans et al., [10.1101/2021.10.04.463034](https://doi.org/10.1101/2021.10.04.463034) |
| OpenFold v2.2.0 | AF2 的 PyTorch 可训练复现 | 序列+MSA | 2 项：坐标、pLDDT | 2021；v2.2.0（2024-04-26） | OpenFold Consortium（aqlaboratory） | B（Nat Methods 2024） | ✓ pip：`openfold` 200；GitHub aqlaboratory/openfold exists | Ahdritz et al., Nat Methods 2024 |
| OpenFold3 | AF3 完全开源复现（权重 Apache 2.0） | 序列+SMILES | 3 项：复合物坐标、置信度、配体 pose | 2025 公开预览 | OpenFold Consortium | C（无独立论文） | ✓ GitHub aqlaboratory/openfold-3 exists | 引用 OpenFold3 team + Abramson et al. |
| ESMFold | ESM-2 语言模型+折叠头，免 MSA 单序列快速折叠 | 单序列 | 2 项：坐标、pLDDT | 2022 | Meta AI（facebookresearch） | A（Science 2023） | ✓ pip：`fair-esm` 200（2.0.0）；GitHub exists | Lin et al., [10.1126/science.ade2574](https://doi.org/10.1126/science.ade2574) |
| OmegaFold | OmegaPLM+Geoformer，单序列折叠（孤儿蛋白适用） | FASTA | 2 项：坐标、置信度 | 2022 | HeliXon（HeliXonProtein） | C（bioRxiv） | ✓ GitHub HeliXonProtein/OmegaFold exists（源码安装；**PyPI 无 omegafold/OmegaFold 包，404，本轮复核**） | Wu et al., [10.1101/2022.07.21.500999](https://doi.org/10.1101/2022.07.21.500999) |
| RoseTTAFold（原版） | 三轨道神经网络结构预测 | 序列+MSA | 2 项：坐标、置信度 | 2021 | RosettaCommons/Baker lab | A（Science 2021） | ✓ GitHub RosettaCommons/RoseTTAFold exists | Baek et al., [10.1126/science.abj8754](https://doi.org/10.1126/science.abj8754) |
| RoseTTAFold2 | 三轨网络+SE(3)-Transformer，单链/多聚体/环肽 | FASTA | 2 项：坐标、B 因子置信度 | 2023 | Baker lab（uw-ipd） | B（Nat Methods 2024） | ✓ GitHub uw-ipd/RoseTTAFold2 exists | Baek et al., Nat Methods 2024 |
| RoseTTAFold3（RF3） | AF3 类全分子共折叠（RosettaCommons 官方版） | 序列+SMILES | 3 项：复合物坐标、置信度、配体 pose | v0.1.8（2025-12-09）→v0.1.9（2025-12-21） | RosettaCommons | C（官方未注明期刊） | ✓ GitHub RosettaCommons/RF3 exists；Azure 在线入口见 Microsoft Research 页（200） | Microsoft Research 页 |
| ColabFold v1.6.2 | AF2/Multimer 易用封装+MMseqs2 加速 MSA | 序列 | 2 项：坐标、pLDDT | 2021；v1.6.2（GitHub 2025-07-14 / PyPI 上传 2026-07-14，两口径） | Mirdita/sokrypton 等 | B（Nat Methods 2022） | ✓ pip：`colabfold` 200（1.6.2）；GitHub exists | Mirdita et al., [10.1038/s41592-022-01488-1](https://doi.org/10.1038/s41592-022-01488-1) |
| HelixFold3 | 百度 AF3 级全分子预测（代码 CC BY-NC-SA，非商用） | 序列+SMILES | 3 项：复合物坐标、置信度、配体 pose | 2024-08（arXiv v1；v3 2024-12）；PaddleHelix 内 HelixFold3.2（2025-07-23） | 百度 PaddlePaddle | C→B（arXiv 技术报告；HelixFold3 后续有 Nat Mach Intell） | ✓ GitHub PaddlePaddle/PaddleHelix exists；paddlehelix.baidu.com 200 | Liu et al., [arXiv:2408.16975](https://arxiv.org/abs/2408.16975) |
| Uni-Fold | 深势 AF2 类开源复现与扩展 | 序列+MSA | 2 项：坐标、pLDDT | 2022 | 深势科技 dptech-corp | C（bioRxiv） | ✓ GitHub dptech-corp/Uni-Fold exists（Apache-2.0） | [10.1101/2022.08.04.502811](https://doi.org/10.1101/2022.08.04.502811) |
| Protenix v0.5 | 字节 AF3 复现（权重 Apache 2.0） | 序列+SMILES | 3 项：复合物坐标、置信度、配体 pose | 2025（v0.5 对应技术报告） | 字节跳动 bytedance | C（技术报告随 repo） | ✓ GitHub bytedance/Protenix exists | Protenix team 技术报告 |
| Protenix v2 | 同工具线重训/升级当前版（repo 自述 "Toward High-Accuracy…"） | 同上 | 同上 | v2（2026-04-08） | 字节跳动 bytedance | C（技术报告随 repo） | ✓ 同上 | 同上 |
| Boltz-1 | MIT 全开放 AF3 级共折叠 | 序列+SMILES | 3 项：坐标、pLDDT、置信度 | 2024-11 | MIT（jwohlwend 等） | C（bioRxiv） | ✓ pip：`boltz` 200；GitHub jwohlwend/boltz exists | Wohlwend et al., [10.1101/2024.11.19.624167](https://doi.org/10.1101/2024.11.19.624167) |
| Boltz-2 v2.2.1 | Boltz 系加亲和力预测头（结构+亲和力联合） | 序列+SMILES | 4 项：坐标、置信度、亲和力回归（Kd/Ki/IC50）、pose | 2025-06；v2.2.1（2025-09-08） | MIT+Recursion | C（bioRxiv） | ✓ pip：`boltz` 200（2.2.1）；boltz.bio/boltz2 200 | Passaro et al., [10.1101/2025.06.14.659707](https://doi.org/10.1101/2025.06.14.659707) |
| Chai-1 v0.6.1 | Chai Discovery 多模态全分子复合物 foundation model | 序列+SMILES+修饰残基 | 3 项：坐标、置信度、配体 pose | 2024-09；v0.6.1（2025-03-18） | Chai Discovery | C（bioRxiv 技术报告） | ✓ pip：`chai-lab` 200（0.6.1）；GitHub chaidiscovery/chai-lab exists | [10.1101/2024.10.10.615955](https://doi.org/10.1101/2024.10.10.615955) |

### 7.2 抗体/免疫结构

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| IgFold v0.4.0 | antiBERTy 预训练+图网络+AF2 式精修的抗体 Fv 结构预测 | VH/VL 序列 | 1 项：抗体 Fv 结构（PDB） | 2022；v0.4.0（2023-09-03，PyPI） | JHU Graylab | B（Nat Commun 2023） | ✓ pip：`igfold` 200（0.4.0）；GitHub Graylab/IgFold exists | Ruffolo et al., [10.1038/s41467-023-38063-x](https://doi.org/10.1038/s41467-023-38063-x) |
| ABodyBuilder2 | ImmuneBuilder 套件抗体（Fab/Fv）结构预测，比 AF2 快百倍 | VH/VL 序列 | 1 项：抗体结构+置信度 | 2022 首发 | Oxford OPIG（oxpig；Abanades 等） | B（Commun Biol 2023） | ✓ pip：`ImmuneBuilder` 200（**v1.2**，本轮按正确大小写复核成功；小写 immunobuilder 在 PyPI JSON API 下返 404）；GitHub oxpig/ImmuneBuilder exists | Abanades et al., [10.1038/s42003-023-04927-7](https://doi.org/10.1038/s42003-023-04927-7) |
| NanoBodyBuilder2 | 套件内纳米抗体（单域）结构预测 | VHH 序列 | 1 项：纳米抗体结构+置信度 | 2022（随套件） | Oxford OPIG | C（独立论文官方未注明，套件仓库为准） | ✓ 同套件 pip/GitHub | 同套件仓库 |
| TCRBuilder2 | 套件内 T 细胞受体结构预测 | TCR α/β 序列 | 1 项：TCR 结构+置信度 | 2022（随套件） | Oxford OPIG | C（同上） | ✓ 同套件 | 同套件仓库 |
| DeepAb | 可解释深度学习抗体结构预测 | VH/VL | 1 项：抗体结构+注意力可解释性 | 2022-02 | 论文团队（Patterns） | B（Patterns 2022） | △ Web+论文代码，本轮未实测 | [10.1016/j.patter.2021.100406](https://doi.org/10.1016/j.patter.2021.100406) |
| Chai-2 | 零样本抗体/结合蛋白设计（24 孔板湿实验验证） | 抗原结构/序列 | 2 项：抗体序列设计、结构 | 2025-06/07 | Chai Discovery | C（bioRxiv 技术报告） | △ 封闭 web，本轮未实测 | [10.1101/2025.07.05.663018](https://doi.org/10.1101/2025.07.05.663018) |

### 7.3 蛋白-配体共折叠

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| NeuralPLexer | 多尺度扩散生成蛋白-配体共折叠（可盲对接） | 蛋白序列/PDB+配体 SMILES/SDF | 2 项：复合物坐标、状态特异构象 | 2023（预印本）；2024 发表 | Qiao Z 等 | B（Nat Mach Intell 2024；另一 DOI 经 CrossRef 核实为 Umol 论文，已排除） | ✓ GitHub zrqiao/NeuralPLexer exists；权重 Zenodo（CC BY-NC-SA） | Qiao et al., [10.1038/s42256-024-00792-z](https://doi.org/10.1038/s42256-024-00792-z) |

> 横评锚点：[FoldBench, Nat Commun 2025](https://doi.org/10.1038/s41467-025-67127-3) 横评 AF3、Boltz-1/2、Chai-1、HelixFold3、Protenix、OpenFold3-preview、RF3 八工具（抗体-抗原接口成功率 18%–48%）。

## 八、毒性

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ProTox-II | 多端点毒性 webserver | 化合物 ID/SMILES | 3 类：急性毒性、器官毒性、致突变等端点概率+分类 | 2018 | Charité（Banerjee 等） | B（NAR 2018） | △ 现役入口为 protox3（200）；ProTox-II 独立入口未实测 | Banerjee et al., NAR 2018, [10.1093/nar/gky318](https://doi.org/10.1093/nar/gky318) |
| ProTox 3.0 | 61 个模型的多端点毒性平台 | 化合物 ID/SMILES | 61 项分七类逐点：急性毒性 1（经口 LD50 mg/kg+GHS 六级+置信度）；器官毒性 5（肝、神经、肾、心脏 hERG、呼吸）；毒理端点 8（致癌、致突变、细胞毒、免疫毒、BBB 透过、生态毒、临床毒、营养毒）；Tox21 AOP 通路 12（核受体 7+应激反应 5，论文点名 NR-AR/NR-AR-LBD/NR-ER/NR-ER-LBD）；分子启动事件 MIE 14（THRα、THRβ、TTR、RYR、GABAR、NMDAR、AMPAR、KAR、AChE、CAR、PXR、NADHOX、VGSC、NIS）；代谢 6（CYP1A2/2C19/2C9/2D6/3A4/2E1）；毒性靶点 15（Novartis 体外安全 panel，论文未逐点名）。其中 28 个新增、33 个沿用 ProTox-II 更新 | 2024 | Charité（Banerjee 等） | B（NAR 2024） | ✓ web 200：tox.charite.de/protox3/ | Banerjee et al., NAR 2024, [10.1093/nar/gkae303](https://doi.org/10.1093/nar/gkae303) |
| DeepTox | Tox21 挑战赛冠军多任务 DNN | SMILES | 12 项：Tox21 全部 12 终点毒性概率 | 2016 | JKU Linz（Mayr 等） | B（Front Environ Sci 2016） | ✓ web 200：bioinf.jku.at/research/DeepTox/；代码 GitHub 可本地 | Mayr et al., [10.3389/fenvs.2015.00080](https://doi.org/10.3389/fenvs.2015.00080) |
| Pred-hERG 5.0 | hERG 通道阻断 QSAR/ML 分类 | SMILES | 1 项：hERG 阻断分类+概率 | 2015；现版 5.0 | LabMol（Braga 等） | B（Mol Inform 2015） | ✓ web 200：predherg.labmol.com.br | Braga et al., [10.1002/minf.201500040](https://doi.org/10.1002/minf.201500040) |
| DeephERG | 深度学习药物心脏毒性预测 | SMILES | 1 项：hERG/心脏毒性分类 | 2019-03 | Cai 等 | B（JCIM 2019，本轮 Crossref 抽查命中） | △ Web+论文代码，本轮未实测 | Cai et al., JCIM 2019, [10.1021/acs.jcim.8b00769](https://doi.org/10.1021/acs.jcim.8b00769) |
| CardioTox net | hERG 阻断深度学习元特征集成 | SMILES | 1 项：hERG 阻断分类 | 2021 | Karim 等 | B（J Cheminform 2021，本轮 Crossref 抽查命中） | △ 论文代码，托管页未实测 | Karim et al., [10.1186/s13321-021-00541-z](https://doi.org/10.1186/s13321-021-00541-z) |
| vNN-ADMET（毒性端点） | 可变最近邻分类+适用域 | SMILES | 15 项中的毒性子集：细胞毒、AMES、hERG、DILI 等（全 15 模型见 §三行） | 2017 | Schyman 等（bhsai.org） | B（Front Pharmacol 2017） | ✓ web 200（需登录） | 同 §三 |
| LAZAR（gem v1.2.1） | 开源预测毒理学框架，lazy SAR+read-across，带置信度 | SMILES | 多端点 QSAR：致突变、致癌等（模块可扩展；JRC QSARDB/QMRF 存档） | 2013；gem v1.2.1（2019-08-29） | in-silico.ch（Maunz 等） | B（Front Pharmacol 2013） | ✓ OpenRiskNet 页 200；`gem install lazar` 可本地锁版 | Maunz et al., [10.3389/fphar.2013.00038](https://doi.org/10.3389/fphar.2013.00038) |
| DEREK Nexus 6.4.2 | 商业专家规则系统（结构警示知识库） | 结构 | 多端点规则推断：致突变、致癌（含效力分级）、皮肤致敏等（知识库 Lhasa 专有） | 最新 6.4.2（2025-06-26） | Lhasa Limited | V（厂商知识库） | △ 商业 license；lhasalimited.org 200 | Lhasa 官方发版新闻 |
| CASE Ultra | 商业碎片 QSAR 专家系统 | 结构 | 多端点：Ames、致癌、hERG、肝毒等+适用域 | 版本官方未注明 | MultiCASE | V | △ 商业；multicase.com 403（存在性经 CrossRef 确认） | Saiakhov et al., Mol Inform 2013, [10.1002/minf.201200081](https://doi.org/10.1002/minf.201200081) |
| DeepChem 2.8.0（Tox21 基线） | 分子 ML 全栈库内置 Tox21/ClinTox 基线模型 | SMILES/分子图 | 12 项：Tox21 全终点预测分数（另 ClinTox、ToxCast loader） | 2015 起；2.8.0（2024-04-02） | DeepChem 社区（Ramsundar 等） | B（MoleculeNet, Chem Sci 2018） | ✓ pip：`deepchem` 200；GitHub exists | Wu et al., [10.1039/C7SC02664A](https://doi.org/10.1039/C7SC02664A) |
| DILIPredictor（dilipred 4.0.9） | 人 DILI 肝损伤风险 scikit-learn ML（锁 sklearn 1.2.0） | SMILES | 1 项：DILI 风险概率 | 2024；4.0.9（2025-07-30） | Seal S 等（GitHub srijitseal） | C→B（bioRxiv 2024；CRT 2024 出处待确认） | ✓ pip：`dilipred` 200（4.0.9）；GitHub exists | Seal et al., [10.1101/2024.01.10.575128](https://doi.org/10.1101/2024.01.10.575128) |
| eTOXlab | eTOX 项目生产环境 QSAR 建模框架 | 结构 | QSAR 端点（致突变等，模型库可扩展） | 2015 | phi-grib（UPF） | B（J Cheminform 2015） | ✓ GitHub phi-grib/eTOXlab exists（GPL3） | Carrió et al., [10.1186/s13321-015-0058-6](https://doi.org/10.1186/s13321-015-0058-6) |
| Pred-Skin | 皮肤致敏 QSAR/ML 网页 | SMILES | 1 项：皮肤致敏分类/概率 | 2017 | LabMol（Braga 等） | B（JCIM 2017） | △ web 403（反爬），需人工复核 | Braga et al., JCIM 2017, [10.1021/acs.jcim.7b00194](https://doi.org/10.1021/acs.jcim.7b00194) |
| SkinSensPred | 迁移学习 DNN 人体皮肤致敏 | SMILES | 1 项：皮肤致敏概率 | 2022 | NHRI 台湾（cwtung 组） | B（IJERPH 2022） | △ cwtung.nhri.edu.tw 本环境 000 | Wang et al., [10.3390/ijerph191912856](https://doi.org/10.3390/ijerph191912856) |
| EPA TEST v5.1.2 | EPA 官方免费 QSAR 桌面软件 | SMILES/绘图 | 6 类：大鼠经口 LD50、Ames、发育毒性、黑头呆鱼/溞 LC50、四膜虫 IGC50、物性 | 最新 5.1.2 | 美国 EPA | A（监管机构官方） | △ epa.gov 下载页本环境 000（官方软件实际可下载） | EPA 官方页 |
| OpenTox | REST API 预测毒理学开源框架/社区 | —（框架） | 框架：聚合多模型毒性预测服务 | 2008/2012 | OpenTox 社区（Hardy 等） | B（J Biomed Semantics 2012） | △ 框架多服务部署，单一入口未实测 | Hardy et al., [10.1186/2041-1480-3-S1-S7](https://doi.org/10.1186/2041-1480-3-S1-S7) |
| Toxtree v3.1.0.1851 | JRC 规则型毒理决策树（Cramer 分类等），Java GUI | 结构 | 规则决策树端点：Cramer 分类、致突变警示等（按所选决策树） | v3.1.0.1851（2021-11-17） | JRC EURL ECVAM（SourceForge 维护） | A（欧盟监管机构官方） | ✓ SourceForge 实测（本轮抽查：v3.1.0.1851） | 引 Benfenati et al., Chem Soc Rev 2020, [10.1039/d0cs00098a](https://doi.org/10.1039/d0cs00098a) |
| VEGA QSAR | Benfenati 组免费 QSAR 模型套件（含 BBB、致突变等） | SMILES | 模型套件多端点：BBB、致突变（Ames）、水生毒等（各模型独立） | 首发年份官方未注明 | 米兰大学 Benfenati 组 | B（模型族被 ECHA 引用） | ✓ web 200：vegahub.eu + ddl.unimi.it/vegaol/；桌面版下载 | 官方站 |
| OECD QSAR Toolbox v4.9 | OECD/JRC 监管级 QSAR+read-across 平台 | 结构 | 监管评估端点：read-across、profiler 分类、模拟实验数据填补（平台性工具，非单一模型） | 当前 4.9 | OECD + ECHA | A（监管官方） | ✓ web 200：qsartoolbox.org（本轮抽查版本 4.9） | OECD 官方站 |

## 九、成药性/可开发性

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| CamSol | 物理化学评分函数：蛋白溶解度谱+增溶突变设计 | 序列（可加结构） | 2 项：逐残基溶解度谱、增溶突变排序 | 2015 | 剑桥大学 Vendruscolo 组 | B（J Mol Biol 2015） | ✓ web 200（需免费注册，学术免费） | Sormanni et al., [10.1016/j.jmb.2014.09.026](https://doi.org/10.1016/j.jmb.2014.09.026) |
| TANGO | 统计力学模型预测 β-折叠聚集区 | 序列 | 1 项：逐段聚集倾向（APR） | 2004 | CRG（Fernandez-Escamilla 等） | A（Nat Biotechnol 2004） | ✓ web 200：tango.crg.es | [10.1038/nbt1012](https://doi.org/10.1038/nbt1012) |
| AGGRESCAN | 实验标度的序列聚集热点评分 | 蛋白序列 | 1 项：逐段聚集倾向（a4v 分值） | 2007 | 华沙大学（Conchillo-Solé 等） | B（BMC Bioinformatics 2007） | △ biocomp.chem.uw.edu.pl 本环境 000 | [10.1186/1471-2105-8-65](https://doi.org/10.1186/1471-2105-8-65) |
| AGGRESCAN3D（A3D） | 结构聚集热点+虚拟突变 | PDB 结构 | 2 项：聚集热点残基、增溶突变建议 | 2015 | 华沙大学（Zambrano 等） | B（NAR 2015） | △ 同上 000 | Zambrano et al., NAR 2015, gkv359 |
| AGGRESCAN3D 2.0 | A3D 升级版，提供 standalone 本地部署 | PDB/AFDB 结构 | 2 项：同 A3D | 2019 | 华沙大学（Kuriata 等） | B（NAR 2019） | △ web 000；standalone 渠道未实测 | Kuriata et al., NAR 2019, gkz321 |
| Aggrescan4D（A4D） | pH 依赖聚集预测+自动增溶突变设计，集成约 50 万条预计算库与 AlphaFold DB 检索 | PDB/序列 | 3 项：pH 依赖聚集倾向、突变设计、结构聚集热点 | NAR 2024-07 在线 | 华沙大学（Bárcenas 等） | B（NAR 2024） | △ biocomp/a4d 本环境 000 | Bárcenas et al., NAR 2024, gkae382 |
| Solubis | TANGO 聚集谱×FoldX 稳定性修正的突变设计 | 序列/结构 | 2 项：聚集谱、稳定性-聚集折衷突变排序 | 2016 | Switch Lab（Van Durme 等） | B（PEDS 2016） | ✓ web 200：solubis.switchlab.org（SolubiS 1.0） | [10.1093/protein/gzw019](https://doi.org/10.1093/protein/gzw019) |
| TAP（Therapeutic Antibody Profiler） | 抗体可开发性五指标旗标评估（与临床抗体分布对比） | VH/VL 序列 | 5 项：CDR 总长、表面疏水性 PSH、CDR 正电荷、CDR 负电荷、轻重链电荷不对称 | 2019 | Oxford OPIG（Raybould 等） | A（PNAS 2019） | ✓ web 200：opig.stats.ox.ac.uk（SAbPred/TAP） | Raybould et al., [10.1073/pnas.1810576116](https://doi.org/10.1073/pnas.1810576116) |
| NetMHCpan 4.1 | 神经网络肽-MHC I 结合/呈递预测（免疫原性端点） | 肽段/FASTA/自定义 MHC | 2 项：EL likelihood（呈递）、BA 亲和力（训练 >100 万条 BA+EL、约 4.3 万 IEDB+5.2 千 CEDAR 表位） | 2020 | DTU Health Tech（Reynisson 等） | B（NAR 2020） | ✓ web 200 + 下载可本地（学术许可） | Reynisson et al., [10.1093/nar/gkaa379](https://doi.org/10.1093/nar/gkaa379) |
| NetMHCpan 4.2 | 现行版（扩展 BA+EL 训练集） | 同上 | 同上 | 服务页 2026-07-06 更新 | DTU Health Tech | B（以服务页为准，无独立论文） | ✓ web 200 + 下载可本地 | DTU 服务页 |
| EpiMatrix | 商业矩阵式 T 细胞表位/免疫原性评分 | 蛋白序列（九肽窗口） | 2 项：MHC II 结合评分、EpiBars 表位 | 商业无公开版本号 | EpiVax | V | △ 商业；epivax.com 本环境拦截 | De Groot & Martin, Clin Immunol 2009, [10.1016/j.clim.2009.01.009](https://doi.org/10.1016/j.clim.2009.01.009) |

> 出局（附录五）：SAPS、Developability Index、Zyggregator——仅方法论文、无公开实现（历史服务器 404），不可用。

## 十、分子生成与逆合成

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| REINVENT | RNN 从头分子生成+迁移学习 | 训练语料（SMILES）+打分配置 | 非性质预测：输出新分子 SMILES（按目标打分优化） | 2017 | MolecularAI（Engkvist 等，AstraZeneca 生态） | B（J Cheminform 2017） | ✓ GitHub MolecularAI/Reinvent exists（Apache-2.0） | [10.1186/s13321-017-0235-x](https://doi.org/10.1186/s13321-017-0235-x) |
| REINVENT 2.0 | 生成模型扩展（generative Turing network 等） | 同上 | 非性质预测：新分子 SMILES | 2020-10-29 | MolecularAI | B（JCIM 2020） | ✓ 同仓库生态 | [10.1021/acs.jcim.0c00915](https://doi.org/10.1021/acs.jcim.0c00915) |
| Reinvent 4 | 模块化生成平台（多目标/骨架跃迁） | SMILES+多目标打分组件 | 非性质预测：新分子 SMILES（多目标 Pareto） | 2024 | MolecularAI（Loeffler 等） | B（J Cheminform 2024，本轮 Crossref 抽查命中） | ✓ GitHub MolecularAI/REINVENT4 exists（源码安装；**PyPI 无 reinvent4/reinvent 包，404，本轮复核**） | Loeffler et al., [10.1186/s13321-024-00812-5](https://doi.org/10.1186/s13321-024-00812-5) |
| DrugEx v3 | 图 Transformer+RL 骨架约束生成 | SMILES/骨架约束 | 非性质预测：新分子 SMILES（骨架约束下） | 2023-02-20 | 论文团队 | B（J Cheminform 2023） | △ **PyPI 无 drugex/drugex-mol 包，404（本轮复核）**；GitHub repo 未在实测清单，待补测（同组 qsprpred 3.2.1 确在 PyPI，但非同一工具） | [10.1186/s13321-023-00694-z](https://doi.org/10.1186/s13321-023-00694-z) |
| MolDQN | 深度强化学习分子性质优化 | 起始分子+目标性质 | 非性质预测：优化后分子 SMILES | 2019-07-24 | 论文团队 | B（Sci Rep 2019） | △ 论文代码，托管页未实测 | [10.1038/s41598-019-47148-x](https://doi.org/10.1038/s41598-019-47148-x) |
| AiZynthFinder 4.4.1 | 逆合成规划（模板+蒙特卡洛树搜索） | 目标分子 SMILES | 非性质预测：合成路线（可排序） | 2020-11-17；当前 4.4.1（2025-12-09，PyPI） | MolecularAI | B（J Cheminform 2020） | ✓ pip：`aizynthfinder` 200（4.4.1）；GitHub exists | [10.1186/s13321-020-00472-1](https://doi.org/10.1186/s13321-020-00472-1) |
| molbloom 3.2.0 | 逆合成快速过滤库（USPTO 布隆过滤器） | 反应 SMILES | 非性质预测：反应可行性二值过滤 | 当前 3.2.0（2026-03-23，PyPI） | whitead（Andrew White） | C（独立论文官方未注明） | ✓ pip：`molbloom` 200（3.2.0）；GitHub whitead/molbloom exists | GitHub |

## 十一、PBPK

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| PK-Sim | 全身生理药代（PBPK）建模平台 | 化合物理化参数+给药方案+人群 | 非单点性质：药时曲线及 PK 参数（Cmax、AUC、t½、组织分布等，按模型配置输出） | 2003-09 首发；2017 开源 | Open Systems Pharmacology（OSP；Willmann 等） | B（BIOSILICO 2003，本轮 Crossref 抽查命中） | ✓ GitHub Open-Systems-Pharmacology/PK-Sim exists；OSP 官网 200 | Willmann et al., [10.1016/s1478-5382(03)02342-4](https://doi.org/10.1016/s1478-5382(03)02342-4) |
| MoBi | 机制式系统药代建模（PK-Sim 同套件，可建任意机制模型） | 模型文件+参数 | 非单点性质：任意机制模型的浓度-时间输出 | 2017 开源 | OSP | C（独立论文官方未注明） | ✓ OSP 官网 200；GitHub 同组织 | OSP 官网 |
| Simcyp Simulator | 商业群体 PBPK（人群变异模拟） | 化合物+人群+给药 | 群体 PK 参数分布（Cmax/AUC/清除率等） | 版本官方未注明 | Certara | V | △ 商业；certara.com 本环境 000 | 产品页 |
| GastroPlus | 商业吸收/PK 仿真（ACAT 模型） | 化合物+剂型+生理参数 | 吸收/PK 参数（Fa、Cmax、AUC 等） | 版本官方未注明 | Simulations Plus | V | △ 商业；产品页本环境域名拦截 | 产品页 |

## 十二、蛋白设计

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ProteinMPNN | 蛋白逆折叠序列设计（给定骨架设计序列） | PDB 骨架 | 非性质预测：设计序列（多条采样+打分） | 2022-10-07 | Baker lab（Dauparas 等） | A（Science 2022，本轮 Crossref 抽查命中 21 作者） | △ GitHub 开源权重开放；repo 未在本轮 60 仓实测清单，待补测 | Dauparas et al., [10.1126/science.add2187](https://doi.org/10.1126/science.add2187) |
| LigandMPNN | 配体/原子上下文条件化序列设计 | PDB 骨架+配体 | 非性质预测：设计序列（配体感知） | 预印本 2023-12-22；Nat Methods 2025-04 | Baker lab | B（Nat Methods 2025） | △ 同上待补测 | [10.1038/s41592-025-02626-1](https://doi.org/10.1038/s41592-025-02626-1) |
| RFdiffusion | 扩散模型蛋白骨架从头设计 | 条件（对称性/结合位点等） | 非性质预测：设计骨架 PDB | 2023-07-11 | Baker lab/RosettaCommons | A（Nature 2023） | △ 同上待补测 | [10.1038/s41586-023-06415-8](https://doi.org/10.1038/s41586-023-06415-8) |

## 十三、Agent 框架与基准

| 工具（版本） | 是什么/有什么用 | 输入 | 输出性质类型（逐点） | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ChemCrow | LLM+18 个化学工具调用的 Agent 框架 | 自然语言任务 | 非性质预测：工具调用轨迹+任务结果 | NMI 2024-05-08 | ur-whitelab（Bran/Cox/Schwaller 等） | A（Nat Mach Intell 2024，本轮 Crossref 抽查命中） | ✓ GitHub ur-whitelab/chemcrow-public exists | Bran et al., [10.1038/s42256-024-00832-8](https://doi.org/10.1038/s42256-024-00832-8) |
| MolClaw | 药物分子评估/筛选/优化自治 Agent（分层技能） | 分子+任务 | 非性质预测：评估/筛选/优化任务结果 | arXiv 2026-04（v2 2026-04-02） | InternScience | C（arXiv） | ✓ GitHub InternScience/MolClaw exists（MIT） | [arXiv:2604.21937](https://arxiv.org/abs/2604.21937) |
| TxBench-PP | 小分子临床前药理毒理 Agent 基准 | 任务集 | 非性质预测：Agent 任务完成率基准 | arXiv 2026-06 | latchbio | C（arXiv） | ✓ GitHub latchbio/txbench-pp exists | [arXiv:2606.19245](https://arxiv.org/abs/2606.19245) |
| PoseBusters | 对接 pose 物理合理性检验基准（8 项物理检查） | 对接 pose | 检验类：pose 合理性通过率（键长/碰撞/手性等检查项） | 2023 | 论文团队（Buttle 等） | B（Chem Sci 2023） | △ pip 通道未在本轮实测清单 | [10.1039/d3sc04185a](https://doi.org/10.1039/d3sc04185a) |
| FoldBench | 结构预测工具统一横评基准 | 各工具预测结构 | 检验类：接口成功率等横评指标 | 2025 | BEAM-Labs | B（Nat Commun 2025） | ✓ GitHub BEAM-Labs/FoldBench（轮 3 开页） | [10.1038/s41467-025-67127-3](https://doi.org/10.1038/s41467-025-67127-3) |

## 十四、框架与权威数据库

### 14.1 框架/工具库

| 资源（版本） | 是什么/有什么用 | 输入 | 输出性质类型 | 发布时间 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RDKit 2026.3.5 | 化学信息学底座：分子读写/描述符/指纹/子结构，事实标准 | SMILES/SDF/mol | 框架：数百种描述符与指纹（本清单 TPSA/QED/SA score 等行即其内置） | GitHub Release_2026_03_6（2026-08-28）；PyPI 2026.3.5（2026-08-03） | RDKit 社区（Landrum） | A（事实标准） | ✓ pip：`rdkit` 200；GitHub rdkit/rdkit exists | rdkit.org |
| Open Babel 3.2.1 | 110+ 分子格式互转与分子操作 | 各化学文件格式 | 框架：格式转换+力场优化+基础描述符 | 3.2.1（2026-07-11，GitHub API 轮 3 实测） | Open Babel 社区（O'Boyle 等） | B（J Cheminform 2011） | ✓ pip `openbabel`；GitHub API 实测 | [10.1186/1758-2946-3-33](https://doi.org/10.1186/1758-2946-3-33) |
| DeepChem 2.8.0 | 分子 ML 全栈：MoleculeNet 加载器、模型、拆分 | SMILES/图 | 框架：内置 Tox21/ESOL/BBBP 等数据集与基线模型 | 2.8.0（2024-04-02） | DeepChem 社区 | B（MoleculeNet 论文） | ✓ pip：`deepchem` 200；GitHub exists | Wu et al., Chem Sci 2018 |
| PyTDC 1.1.15 | TDC 统一数据接口+benchmark+leaderboard | 数据集名 | 框架：22 个学习任务族统一加载与评测 | PyPI 1.1.15（2025-03-31） | Harvard（mims-harvard） | B（Nat Chem Biol 2022） | ✓ pip：PyTDC；tdcommons.ai 200 | Huang et al., [10.1038/s41589-021-00903-2](https://doi.org/10.1038/s41589-021-00903-2) |
| MoleculeNet | 17 个分子性质数据集+统一拆分与指标 | —（数据集族） | 数据集：QM/物化/生理/毒性各族（BBBP、Tox21、ESOL 等出处） | 定格 2018 | Wu Z 等（Stanford） | A（Chem Sci 2018） | ✓ 随 DeepChem 加载；论文页 PMC 200 | [10.1039/C7SC02664A](https://doi.org/10.1039/C7SC02664A) |
| OCHEM v4.3.202 | 在线 QSAR 建模环境+化合物数据库 | SMILES/SDF | 框架+内置模型：可训练任意端点；内置数百模型 | 平台 v4.3.202 | Tetko IV 等 | B（JCAMD 2011） | ✓ web 200：ochem.eu + docs.ochem.eu | [10.1007/s10822-011-9440-2](https://doi.org/10.1007/s10822-011-9440-2) |
| PaddleHelix（HelixFold3.2） | 百度生物计算平台：HelixFold+药物/疫苗工具链 | 序列/结构 | 框架：结构预测+分子性质多模型 | HelixFold3.2（2025-07-23） | 百度 | C→B（HelixFold3 后续 Nat Mach Intell） | ✓ GitHub PaddlePaddle/PaddleHelix exists；站点 200；许可 CC BY-NC-SA（商用受限） | 官网 |
| TorchDrug 0.2.1 | PyTorch 分子/蛋白图学习平台 | 图数据 | 框架：性质预测/生成模型库 | 0.2.1（此后停更） | DeepGraphLearning（Zhu 等） | C（arXiv） | ✓ pip：`torchdrug` 200；torchdrug.ai 域名拦截 | [arXiv:2202.08320](https://arxiv.org/abs/2202.08320) |
| Uni-Mol（Uni-Mol2） | 3D 分子表征预训练：性质预测/构象/对接 | 3D 构象+SMILES | 框架：性质预测头（下游任务微调） | Uni-Mol2（2024-10-10 更新） | 深势科技 | A（ICLR 2023）+B（Nat Commun 2024） | ✓ GitHub dptech-corp/Uni-Mol exists | Zhou et al., ICLR 2023；Lu et al., Nat Commun 2024 |
| chemprop v2.3.1 | 消息传递神经网络库（ADMET-AI 底座） | SMILES/图 | 框架：回归/分类模型训练 | PyPI 首发 2020-07-25；v2.3.1（2026-08-04） | chemprop 团队（Yang/Durrant 等） | B（JCIM 2019） | ✓ pip：`chemprop` 200（2.3.1） | [10.1021/acs.jcim.9b00237](https://doi.org/10.1021/acs.jcim.9b00237) |
| meeko v0.8.0 | Vina 1.2 生态配体准备工具 | 分子结构 | 框架：PDBQT 准备/修饰 | PyPI 首发 2021-02-17；v0.8.0（2026-08-27） | forlilab（Sanner 等） | C（独立论文官方未注明） | ✓ pip：`meeko` 200；GitHub forlilab/meeko exists | GitHub |

### 14.2 权威数据库（实验金标准）

| 资源（版本） | 是什么/有什么用 | 内容/规模 | 输出性质类型 | 当前版本 | 发布方 | 权威性 | 可用性（入口存活） | 来源文献 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ChEMBL 37 | 实测生物活性权威库（亲和力金标准首选） | 292 万化合物、2,453 万活性值、197 万 assay、1.86 万靶点 | 实测：Ki/Kd/IC50/EC50 等活性值+assay 元数据 | ChEMBL 37（文件 2026-05-29） | EMBL-EBI | A | ✓ ftp.ebi.ac.uk release notes 轮 3 开页 | Zdrazil et al., NAR 2024, [10.1093/nar/gkad1004](https://doi.org/10.1093/nar/gkad1004) |
| BindingDB | 蛋白-小分子结合实测库 | 320 万条数据、140 万化合物、1.15 万靶点（首页原文） | 实测：Ki/Kd/IC50+条件（pH/温度） | 滚动更新 | Gilson 等（UCSD） | A（NAR） | ✓ web 200：bindingdb.org | Gilson et al., NAR 2016, [10.1093/nar/gkv1072](https://doi.org/10.1093/nar/gkv1072) |
| PubChem BioAssay | 高通量筛选原始结果库 | 1.67 万 bioassay、2.95 亿活性值（全库 1.19 亿化合物） | 实测：assay outcome（活性/非活性/不确定） | 滚动更新 | NCBI/NLM | A | ✓ PUG-REST API；论文页 PMC11701573 200 | Kim et al., NAR 2025, [10.1093/nar/gkae1059](https://doi.org/10.1093/nar/gkae1059) |
| PDBbind v2020 | 复合物结构+实测亲和力配对精选集 | 通用集 19,443 复合物（refined/core 为子集） | 实测：Kd/Ki+对应结构 | v2020（最后公开版，此后转 PDBbind+） | PDBbind 团队 | A（Bioinformatics） | ✓ web 200：pdbbind-plus.org.cn（旧域名 pdbbind.org.cn 已劫持，勿引用） | Liu et al., [10.1093/bioinformatics/btu626](https://doi.org/10.1093/bioinformatics/btu626) |
| DUD-E | 对接虚拟筛选 decoy 基准 | 102 靶点、22,886 活性、每活性 50 decoy | 基准：活性/诱饵标签 | 定格 2012 | UCSF（docking.org） | B（J Med Chem 2012） | ✓ web 200：dude.docking.org | Mysinger et al., [10.1021/jm300687e](https://doi.org/10.1021/jm300687e) |
| Tox21/ToxCast | qHTS 毒性筛选数据+2014 挑战赛 | 约 10K 化合物库、70+ assay | 实测：qHTS 剂量响应曲线/AC50 | 定格 2014-15 | NCATS+EPA | A（官方联盟） | ✓ web 200：tox21.gov/data-and-tools | Tice et al., EHP 2013 |
| DrugBank 5.1.13 | 药物综合知识库 | 公开 XML 全库约 195 MB | 注释：药物-靶点/适应证/性质 | 公开版 5.1.13（2025-01-02）；商业 6.x | Wishart 等 | A（NAR） | △ go.drugbank.com/releases 本环境 000；学术下载暂停中；许可非自由再分发 | Wishart et al., NAR 2018, [10.1093/nar/gkx1037](https://doi.org/10.1093/nar/gkx1037) |
| BBBP 原集 | BBB 二分类经典数据集（注意两口径） | MoleculeNet 论文口径 2,053（Table 1）；TDC BBB_Martins 口径 1,975（metadata.py）；两口径差 78，引用必须注明 | 实测：BBB 透/不透二分类（Martins 等 2012 汇编） | 静态 | Martins IF 等（原始）；MoleculeNet/TDC（转载） | B（JCIM 2012） | ✓ 经 DeepChem/PyTDC 加载（tdcommons 200） | Martins et al., JCIM 2012, [10.1021/ci2005159](https://doi.org/10.1021/ci2005159) |
| B3DB | BBB 渗透数据库（分类+连续值双口径） | 7,982 化合物带标签+1,058 条 logBB 实测值，50 个来源（README 原文） | 实测：BBB 二分类标签、logBB 连续值 | 定格 2021 首发 | theochem（B3DB 团队） | B（Sci Data 2021） | ✓ GitHub theochem/B3DB exists | Meng et al., [10.1038/s41597-021-01069-5](https://doi.org/10.1038/s41597-021-01069-5) |
| AqSolDB | 水溶性参考数据集（带可靠性分层） | 9,982 唯一化合物，9 源整合，G1-G5 可靠性分组 | 实测：logS+可靠性标签 | 定格 2019-08-08 | Sorkun MC 等 | B（Sci Data 2019） | ✓ GitHub mcsorkun/AqSolDB exists | [10.1038/s41597-019-0151-1](https://doi.org/10.1038/s41597-019-0151-1) |
| TDC ADMET 家族 | 单性质 ADMET benchmark 组 | 18 ADME+4 Tox=22 个数据集（BBB_Martins、Lipophilicity_AstraZeneca、Solubility_AqSolDB、CYP×4 等） | 实测/汇编：各数据集性质标签+统一拆分与指标 | 随 PyTDC 1.1.15 | Harvard TDC | B（Nat Chem Biol 2022） | ✓ tdcommons.ai/single_pred_tasks/adme/ 200 | 同 PyTDC |
| ProteinGym v1.3 | 突变效应（DMS）基准 | 替换集 217 assay/约 270 万突变；indel 集 74 assay/约 30 万突变 | 实测：DMS 适应度分值 | v1.3 | Notin 等（OATML） | A（NeurIPS 2023 D&B） | ✓ web 200：proteingym.org | Notin et al., NeurIPS 2023 |
| CACHE / SAMPL / D3R | 三个前瞻盲测挑战入口 | CACHE（hit-finding 实验盲测）；SAMPL9 进行中；D3R 定格 GC4 | 盲测：亲和力/logD/pKa/pose 等前瞻真值 | 持续/定格 | SGC / NIH 资助 / UCSD | A（官方挑战） | ✓ cache-challenge.org 200；samplchallenges.org 200；△ drugdesigndata.org 域名拦截 | Ackloo et al., Nat Rev Chem 2022；Parks et al. 2020 |

## 十五、金标准配置建议（按性质）

- **BBB**：B3DB（7,982 带标签+1,058 条连续 logBB，可解二值退化风险）为主金标准的迁移**属六项待裁之一（已附推荐、owner 未裁）**，未裁前不作为既成事实；BBBP 引用必须注明口径（MoleculeNet 2,053 / TDC 1,975，差 78 个分子）；沿用既有 BBB 真值污染核查流程（TDC 标签自相矛盾记录）。
- **logP**：TDC Lipophilicity_AstraZeneca；SAMPL6/7 盲测值作外部校验；Mannhold 9.6 万分子比较集作方法间分歧的现成证据。
- **溶解度**：AqSolDB（按 G1-G5 可靠性分层使用）。
- **毒性**：Tox21/ToxCast 经 PubChem 取原始 qHTS 测定值；ClinTox 作临床级对照。
- **亲和力**：ChEMBL 37 + BindingDB 交叉；结构层 PDBbind v2020；CASF-2016 作打分函数评测协议。
- **突变效应**：ProteinGym（注意同源污染子集）。

## 十六、对 Step 1 的选型建议（只列入口存活工具；功能均待实跑验证）

1. **首选冲突组（BBB）**：本地可锁版 8 行——ADMET-AI v1 系 1.4.0 与 ADMET-AI v2 2.0.1（pip 两版并存，权重不同=零训练成本的真实版本漂移冲突对，迁移前逐分子比对）、DeepPurpose（pip，BBBP 须自训练并记录配置）、B3clf（Zenodo v0.0.1）、SAI v3（Zenodo DOI）、DeepB3/DeePred-BBB/graphB3（GitHub 锁 commit）。web 快照组 8 行——ADMETlab 3.0、admetSAR 3.0、pkCSM（logBB 回归）、SwissADME（BOILED-Egg）、PreADMET、VEGA BBB、BBBper、LogBB_Pred（logBB 回归）。计分口径：二分类与 logBB 回归分开。LGBM-BBB（△，预印本）不入主组。
2. **次选组（logP）**：✓ 可用组合——XLogP3（PubChem API 快照）、ALOGPS 2.1、MLOGP/WLOGP/SILICOS-IT（经 SwissADME 一次调用得三值）；候选待补测——JPlogP（CDK）、KowWIN（EPA 下载页需换网络）；商业对照——ACD/Percepta、ChemAxon、QikProp。
3. **毒性组**：本地基线 DeepChem/Tox21 + dilipred + LAZAR + eTOXlab；web 对照 ProTox 3.0、Pred-hERG、VEGA、Toxtree、OECD Toolbox（后三者监管级）。
4. **亲和力组**：回归 5 模型同锚 DAVIS/KIBA（DeepDTA、GraphDTA、DeepAffinity、TankBind、DeepPurpose）；对接组 Vina 1.2.7/Smina/QVina2/Gnina 1.3.3 同源打分 + Pafnucy/OnionNet/OnionNet-2/RF-Score-VS 异源 pK，统一 pose 生成器；DiffDock/FABind/EquiBind 作生成式 pose 组。
5. **结构组**：FoldBench 横评八工具全部 ✓（AF3 走 GitHub 推理码+权重申请）；Boltz-2 因带亲和力头可跨 §六 使用。
6. **web/商业工具策略**：固化输入输出快照+访问日期+版本入 benchmark 记录；商业端（QikProp、Percepta、Glide、Gold、DEREK、CASE Ultra、Simcyp、GastroPlus、Epik、MoKa、Marvin、Quacpac、EpiMatrix、Rowan）仅作对照通道。
7. **生成/逆合成、PBPK、蛋白设计、Agent 层**：REINVENT 系+DrugEx（✓）；AiZynthFinder 4.4.1 vs molbloom 3.2.0（✓）；PK-Sim/MoBi（✓）对 Simcyp/GastroPlus（△商业）；ProteinMPNN/LigandMPNN/RFdiffusion（△ repo 待补测，补测后即可用）；ChemCrow（✓）作工具调用框架，MolClaw/TxBench-PP/PoseBusters/FoldBench 为现成基准。

## 十七、关键参考文献

1. Swanson K, et al. ADMET-AI. Bioinformatics 2024. [DOI](https://doi.org/10.1093/bioinformatics/btae416)
2. Huang K, et al. DeepPurpose. Bioinformatics 2020. [DOI](https://doi.org/10.1093/bioinformatics/btaa1005)
3. Pires DEV, et al. pkCSM. J Med Chem 2015. [DOI](https://doi.org/10.1021/acs.jmedchem.5b00104)
4. Myung Y, et al. Deep-PK. NAR 2024. [DOI](https://doi.org/10.1093/nar/gkae254)
5. Daina A, Zoete V. BOILED-Egg. ChemMedChem 2016. [PMID 27218427](https://pubmed.ncbi.nlm.nih.gov/27218427/)
6. Daina A, et al. SwissADME. Sci Rep 2017. [DOI](https://doi.org/10.1038/srep42717)
7. Fu L, et al. ADMETlab 3.0. NAR 2024. [DOI](https://doi.org/10.1093/nar/gkae236)
8. Gu Y, et al. admetSAR 3.0. NAR 2024. [DOI](https://doi.org/10.1093/nar/gkae298)
9. Banerjee P, et al. ProTox 3.0. NAR 2024. [DOI](https://doi.org/10.1093/nar/gkae303)
10. Mannhold R, et al. LogP 方法比较（>96,000 化合物）. J Pharm Sci 2009. [DOI](https://doi.org/10.1002/jps.21494)
11. Işık M, et al. SAMPL6 logP Challenge. 2020. [DOI](https://doi.org/10.1007/s10822-020-00295-0)
12. Meng F, et al. B3DB. Sci Data 2021. [DOI](https://doi.org/10.1038/s41597-021-01069-5)
13. Martins IF, et al. BBBP 原始集. JCIM 2012. [DOI](https://doi.org/10.1021/ci2005159)
14. BBB 预测工具筛查实证. Front Pharmacol 2025. [DOI](https://doi.org/10.3389/fphar.2025.1616144)
15. BBB 渗透预测模型综述. [PMC11949286](https://pmc.ncbi.nlm.nih.gov/articles/PMC11949286/)
16. Shaker B, et al. LightBBB. Bioinformatics 2021. [PMID 33112379](https://pubmed.ncbi.nlm.nih.gov/33112379/)
17. Sorkun MC, et al. AqSolDB. Sci Data 2019. [DOI](https://doi.org/10.1038/s41597-019-0151-1)
18. Wu Z, et al. MoleculeNet. Chem Sci 2018. [DOI](https://doi.org/10.1039/C7SC02664A)
19. Huang K, et al. TDC. Nat Chem Biol 2022. [DOI](https://doi.org/10.1038/s41589-021-00903-2)
20. Jumper J, et al. AlphaFold2. Nature 2021. [DOI](https://doi.org/10.1038/s41586-021-03819-2)
21. Abramson J, et al. AlphaFold3. Nature 2024. [DOI](https://doi.org/10.1038/s41586-024-07487-w)
22. FoldBench. Nat Commun 2025. [DOI](https://doi.org/10.1038/s41467-025-67127-3)
23. Passaro S, et al. Boltz-2. bioRxiv 2025. [DOI](https://doi.org/10.1101/2025.06.14.659707)
24. Zdrazil B, et al. ChEMBL update. NAR 2024. [DOI](https://doi.org/10.1093/nar/gkad1004)
25. OpenADMET（avoid-ome）. Nat Commun 2026. [s41467-026-73410-8](https://www.nature.com/articles/s41467-026-73410-8)

## 附录一：五轮核验方法与证据位置

- 轮 1：7 路并行检索（WebSearch+页面复核），初稿 8 类 138 行。
- 轮 2（并行线工程师）：6 轮权威 API 交叉核验（Crossref、OpenAlex、PubMed eutils、bioRxiv API、PyPI JSON、GitHub API），扩展至 12 类；脚本与原始返回存 `tools/step1_survey_evidence/`。
- 轮 3：9 路逐行开页核实 + 11 项 API 抽查。
- 轮 4：按 ADR-0001"一版本一行"拆分组合并行。
- 轮 5（本次）：工程师可用性实测（PyPI JSON 26 包、GitHub API 60 仓库、HTTP 141 URL，存 `availability_2026-09-03/`）+ 我方抽查复核（pksmart/rowan-python PyPI、4 个 web URL、2 个 GitHub repo，全部与工程师结果一致）；ADMET-AI v1/v2 端点清单取自仓库内置 CSV（admet_v1/v2_endpoints.csv）；ADMETlab/Deep-PK/pkCSM/ProTox/SwissADME 端点清单解析自工程师下载的论文 XML；admetSAR 3.0 端点清单解析自官网端点页 HTML（实测 116 项）。
- 轮 5 复核发现并修正轮 5 自身的 6 例 PyPI 假阳性（脚本 404 判定缺陷，详见口径节与附录三第 24 条）；本轮另确认 6 个 URL 为公司代理"伪 200"拦截页。
- **全程零功能实跑**：本文档未安装、未运行任何工具，未产出任何预测值。功能验证待办见附录七。
- ADR 对齐：工具同一性=名称+权重+训练数据+端点定义（`docs/adr/0001-tool-identity-version-lineages.md`，Accepted）。

## 附录二：检索未证实剔除（历轮累计）

ProADMET、pkADME、BBB-Predictor（Wang）、BoB、CBBBP、BrainP、Dragnet、SANN、ProTox3B、T20、SFvCSP、AqSolPred（Crossref/OpenAlex/PubMed 零命中）、CLogP（BioByte，官网拦截无存档）、miLogP（Molinspiration，域名拦截）、DataWarrior 熔点条目（官方功能清单无此模型，软件本体真实存在 v6.1.0）、AqueousSol（PyPI 404，AqSolDB 数据集本体保留）。

## 附录三：更正记录（相对各前稿）

1. SwissADME 出处：Front Pharmacol → **Sci Rep 2017**（PMID 28256516）。
2. pkCSM 出处：NAR 2015 → **J Med Chem 2015**。
3. LightBBB 刊号：btab699 → **btaa918**。
4. NeuralPLexer 期刊：定夺 **Nat Mach Intell 2024**；另一 DOI 为 Umol 论文，排除。
5. AlphaFold3：推理代码已开源（v3.0.0→v3.0.4），权重申请制——"仅 server"作废。
6. AutoDock Vina v1.2.7 日期：2026-02 → **2025-02-26**（GitHub API）。
7. RosettaLigand 出处：PLOS ONE → **Nat Protoc 2013**。
8. TAP 出处：期刊未核实 → **PNAS 2019**。
9. DeepPurpose：**官方预训练库无 BBBP 权重**，提案 §1.3 三模型为自训练。
10. BBBP 分子数：定为 **MoleculeNet 2,053 / TDC 1,975**（"1,974"系讹误）。
11. DrugBank 公开版：5.1.22 → **5.1.13（2025-01-02）**，学术下载暂停。
12. PyTDC：**1.1.15（2025-03-31）**；TDC 论文补 Nat Chem Biol 2022。
13. TDC ADMET 家族：21 → **22（18 ADME+4 Tox）**；ProteinGym 补 indel 集；PubChem 按 NAR 2025 口径。
14. Deep-PK 端点数：72 → **73**（PubMed 摘要原文）。
15. 轮 3 端点/版本补齐 15 项（admetSAR 3.0 DOI、DeepTox DOI、Epik 期刊 JCTC、ESOL 期刊 JCICS 等）。
16. 粒度重排（轮 4）：按"一版本一行"拆分 13 组合并行。
17. **OpenADMET 引用恢复**：轮 3 曾撤销其 Nat Commun 2026 引用；轮 5 实测 nature.com/articles/s41467-026-73410-8 返回 200（标题"Mapping the avoid-ome…"），引用恢复。
18. **可用性反转两处**：VEGA（vegahub.eu/vegaol/bbbp.htm 均 200）与 PreADMET（主站+BBB 端点页 200）——轮 3 的 403 系本环境反爬，工具实际在线。
19. **LightBBB 确认下线**：ssbio.cau.ac.kr/software/bbb 实测 404（同站 logbb_pred 为 200），移附录五。
20. 版本更新（工程师 PyPI 实测）：PKSmart **3.0.1（2025-09-29）**、AiZynthFinder **4.4.1（2025-12-09）**、molbloom **3.2.0（2026-03-23）**、chemprop **2.3.1（2026-08-04）**、meeko **0.8.0（2026-08-27）**。
21. admetSAR 3.0 端点数两口径：**论文 119 / 官网端点页实测 116**，引用需注明。
22. ADMET-AI 口径对齐：README"41 端点"指模型端点；仓库端点表实测 **v1 输出 49 列（8 直算+41 模型）、v2 输出 52 列（11 直算+41 模型）**。
23. ColabFold v1.6.2 日期两口径：GitHub release 2025-07-14 / PyPI 上传 2026-07-14。

24. **轮 5 PyPI 结果 6 例假阳性已修正**：脚本用 `grep "404 Not Found"` 判断包是否存在，而 PyPI 实际返回 `{"message": "Not Found"}`，导致 6 个不存在的包被记成"200/版本字段空"。本轮按 HTTP 状态码重查：`diffdock`、`reinvent4`、`drugex`、`omegafold`、`openadmet` 均 **404**（这些工具的正确获取方式是 GitHub 源码，非 pip）；`immunobuilder` 小写在 JSON API 下 404，正确名 **`ImmuneBuilder`（v1.2，200）**。据此 DiffDock/Reinvent 4/OmegaFold/OpenADMET 的证据改为 GitHub 仓库，**DrugEx v3 降为 △**。
25. **ADMET Predictor 降为 △**：此前凭"产品页 200"标 ✓，与其余商业软件口径不一致；产品页可达不等于软件可用，改 △。
26. **术语更正**：⑧列名由"实测可用"改为"入口存活"，标题与首段加核验边界声明——此前的"实测可用"会被读成功能测试，属过度声称。✓ 总数相应由 141 修为 **139**。

## 附录七：功能实跑待办（Step 1 建环境时执行，当前全部未做）

| 优先级 | 待办 | 验收标准 |
| --- | --- | --- |
| P0 | BBB 主线 8 个本地工具装环境实跑（ADMET-AI 1.4.0/2.0.1、DeepPurpose、B3clf、SAI v3、DeepB3、DeePred-BBB、graphB3） | 同一批 ≥10 个分子（含提案 §1.3 六个展示分子）各出一次预测值，落盘 run receipt（输入/输出/版本/时间戳） |
| P0 | ADMET-AI 1.4.0 vs 2.0.1 逐分子比对 | 给出两版预测值差异分布，确认是否构成可用的版本漂移冲突对 |
| P1 | BBB 主线 8 个 web 工具走一遍真实提交（非仅首页 200） | 每个工具对同一分子取回一个真实预测值+页面快照 |
| P1 | 蛋白设计三件套与 18 个 △ 开源工具补 GitHub/PyPI 存活检查 | 用修正后的判定逻辑（HTTP 状态码）重跑，出新表 |
| P2 | ADMETlab 3.0 与 Deep-PK 的 Supplementary S1 补抓 | 端点逐点清单补全，替换附录六 6.7/6.8 的分类计数 |
| P2 | 6 个"伪 200"域名换网络复核 | 确认工具本体在线与否，更新 △/✗ |

## 附录四：范例条目——ADMET-AI v2 深度解读（"一版本一行"的示范）

**它是什么**：pip 包（`pip install admet-ai==2.0.1`），输入 SMILES，输出 52 列性质预测（41 个模型端点+11 个直算/警示列），每个模型端点值附 DrugBank 已批准药物参考百分位。

**模型机制（一个分子走一遍）**：① SMILES 解析成分子图（原子=节点、键=边）；② Chemprop v2 的有向消息传递神经网络让每个原子多轮聚合邻居信息，把局部化学环境编码进原子向量；③ 整图向量进回归/分类两个多任务头，一次前向输出全部端点；④ 每个值对照随包 DrugBank 参考分布给百分位。

**版本史（GitHub releases notes + PyPI JSON 实测，8 版）**：1.0.0（2023-12-04，无模型文件）→ 1.1.0（多任务模型）→ 1.2.0（DrugBank 参考数据，百分位上线）→ 1.3.0/1.3.1（论文评审改进）→ 1.4.0（2025-09-16，固定 torch==2.5.0，提案在用）→ 2.0.0（2026-02-17，**权重换 Chemprop v2、训练数据换 PyTDC 1.1.15 重训**，官方声明预测值不等于 v1）→ 2.0.1（2026-02-22，检查点 MPS→CPU 修 Linux/CUDA，补丁）。

**为什么 v1 与 v2 是两个工具**：接口、端点、输入输出全同，但权重与训练数据都换了，同一分子输出可以不同——这正是 ADR-0001 的判据，也是零训练成本的天然冲突对。提案 §1.3 既有测量全部产自 v1 系，迁移前须逐分子比对。

**锁版**：`admet-ai==2.0.1` + `chemprop>=2.2.2` + torch 版本，三元组入记录才算锁定。

**边界**：DrugBank 百分位是参照系不是金标准，判对错仍以实验测定值（B3DB/BBBP）为准。

## 附录五：当前不可用/出局清单（正文不收录）

| 工具 | 出局原因（实测证据） | 恢复路径 |
| --- | --- | --- |
| LightBBB | 唯一入口 ssbio.cau.ac.kr/software/bbb 实测 **404**；无官方代码仓库 | 联系作者（Shaker B）获取代码后恢复 |
| ADMETboost | admetboost.scbdd.com 实测 **000**（同域 admetlab3 为 200，疑似下线） | 人工浏览器复核；若确认下线则永久剔除 |
| ADMETlab 2.0 | 被 3.0 取代，旧入口未实测 | 如需历史对照，向中南大学 CBDD 组确认 2.0 入口 |
| admetSAR 1.0 | 2012 版无现役入口（2.0/3.0 均 200） | 历史对照可引论文，不作工具行 |
| Epik 经典版 | 被 Epik 7（ML）取代，无法独立获得 | 商业套件内确认是否保留经典模式 |
| AutoDock Vina 1.0 | 现役下载通道未实测（1.2.7 pip/GitHub 均 ✓） | SourceForge 历史版本可下载后恢复为对照行 |
| X-Score | 2002 原版分发渠道未核实 | 找到官方分发页后恢复 |
| CLogP / miLogP | 官网本环境拦截且无存档，五要素未完成核实 | 人工浏览器复核 biobyte.com / molinspiration.com |
| SAPS / Developability Index / Zyggregator | 仅方法论文，无公开实现（Zyggregator 历史服务器 404） | 自行按论文实现后方可入表 |
| DataWarrior 熔点条目 | 官方功能清单无熔点模型（软件本体 v6.1.0 真实存在） | 无需恢复（条目本身不成立） |
| AqueousSol | PyPI 404、无权威仓库（AqSolDB 数据集本体保留） | 无需恢复 |
| 轮 1 未证实种子 12 个 | Crossref/OpenAlex/PubMed 零命中（见附录二） | 提供可核实出处后恢复 |

## 附录六：多端点平台输出性质全清单（逐点）

### 6.1 ADMET-AI v1 系——输出 49 列（仓库端点表 CSV 原文）

- 物化直算 8：Molecular Weight、LogP、Hydrogen Bond Acceptors、Hydrogen Bond Donors、Lipinski Rule of 5、QED、Stereo Centers、TPSA
- 吸收 8：Human Intestinal Absorption（HIA_Hou，分类）、Oral Bioavailability（Bioavailability_Ma，分类）、Aqueous Solubility（Solubility_AqSolDB，回归）、Lipophilicity（Lipophilicity_AstraZeneca，回归）、Hydration Free Energy（FreeSolv，回归）、Cell Effective Permeability（Caco2_Wang，回归）、PAMPA Permeability（PAMPA_NCATS，分类）、P-glycoprotein Inhibition（Pgp_Broccatelli，分类）
- 分布 3：Blood-Brain Barrier Penetration（BBB_Martins，分类）、Plasma Protein Binding Rate（PPBR_AZ，回归）、Volume of Distribution at Steady State（VDss_Lombardo，回归）
- 排泄 3：Half Life（Half_Life_Obach）、Drug Clearance-Hepatocyte（AZ）、Drug Clearance-Microsome（AZ），均回归
- 代谢 8：CYP1A2/2C19/2C9/2D6/3A4 Inhibition（Veith，分类）+ CYP2C9/2D6/3A4 Substrate（CarbonMangels，分类）
- 毒性 19：hERG Blocking、Clinical Toxicity（ClinTox）、Mutagenicity（AMES）、Drug Induced Liver Injury（DILI）、Carcinogenicity（Lagunin）、Acute Toxicity LD50（Zhu，回归）、Skin Reaction、NR-AR、NR-AR-LBD、NR-AhR、NR-Aromatase、NR-ER、NR-ER-LBD、NR-PPAR-gamma、SR-ARE、SR-ATAD5、SR-HSE、SR-MMP、SR-p53（除 LD50 外均分类）

### 6.2 ADMET-AI v2——输出 52 列

= v1 的 49 列 + 3 个新增直算警示列：**PAINS Alert、BRENK Alert、NIH Alert**；41 个模型端点名单不变，但权重为 Chemprop v2 重训（ADR-0001：另一工具）。

### 6.3 pkCSM——30 个预测器（论文 Table 1/2，14 回归+16 分类）

- 吸收 7：water solubility、Caco-2 permeability、human intestinal absorption、skin permeability、P-gp substrate、P-gp inhibitor I、P-gp inhibitor II
- 分布 4：VDss、human plasma unbound fraction（Fu）、blood-brain barrier penetration、CNS penetration
- 代谢 7：CYP1A2/2C19/2C9/2D6/3A4 inhibition、CYP2D6 substrate、CYP3A4 substrate
- 排泄 2：total clearance、renal OCT2
- 毒性 10：hERG I、hERG II、Ames mutagenicity、hepatotoxicity、skin sensitization、human maximum recommended therapeutic dose（MRTD）、rat acute oral toxicity（LD50）、rat chronic toxicity（LOAEL）、T. pyriformis toxicity、fathead minnow LC50

### 6.4 SwissADME——约 30 项输出（论文各输出面板）

- 物化：MW、摩尔折射率 MR、TPSA、特定原子类型计数
- 亲脂性 6：iLOGP、XLOGP3、WLOGP、MLOGP、SILICOS-IT、consensus log Po/w（五者均值）
- 水溶性：ESOL logS、Ali logS、SILICOS-IT logS + mol/l 与 mg/ml 换算 + 六级定性（insoluble→highly soluble）
- 药代 9：log Kp（皮肤透过，Potts-Guy）、HIA、BBB（BOILED-Egg）、P-gp 底物（BOILED-Egg）、CYP1A2/2C19/2C9/2D6/3A4 抑制（6 个 SVM）
- 类药性 6：Lipinski、Ghose、Veber、Egan、Muegge、Bioavailability Score（11/17/56/85% 四档）
- 药化友好 4：PAINS（481 片段）、Brenk（105 基团）、Leadlikeness、SA score（1-10）
- 图形：Bioavailability Radar 六轴 + BOILED-Egg 图

### 6.5 ProTox 3.0——61 个模型（七类）

急性毒性 1（LD50 mg/kg+GHS 六级+置信度）；器官毒性 5（肝、神经、肾、心脏 hERG、呼吸）；毒理端点 8（致癌、致突变、细胞毒、免疫毒、BBB、生态毒、临床毒、营养毒）；Tox21 AOP 通路 12（核受体 7+应激反应 5；论文点名 NR-AR、NR-AR-LBD、NR-ER、NR-ER-LBD）；分子启动事件 14（THRα、THRβ、TTR、RYR、GABAR、NMDAR、AMPAR、KAR、AChE、CAR、PXR、NADHOX、VGSC、NIS）；代谢 6（CYP1A2、2C19、2C9、2D6、3A4、2E1）；毒性靶点 15（Novartis 体外安全 panel，论文未逐点名）。28 个新增、33 个沿用 ProTox-II 更新。

### 6.6 admetSAR 3.0——官网端点页实测 116 项（论文口径 119）

- 物化 13：MW、HBA、HBD、TPSA、原子数、环数、可旋转键数、杂原子数、QED、SlogP（RDKit）、logP（CLMGraph）、logS、pKa
- 类药规则 3：Lipinski、Pfizer、GSK
- 吸收 5：Caco-2（回归/分类）、HIA、MDCK、口服生物利用度 F
- 分布 13：BBB、OATP1B1、OATP2B1、OATP1B3、OCT1、OCT2、BCRP、BSEP、MATE1、Pgp 抑制、Pgp 底物、PPB、VDss
- 代谢 15：CYP1A2/2C9/2C19/2D6/3A4/2B6 抑制（6）+ 同 6 亚型底物 + HLM、RLM、UGT 底物
- 排泄 4：CLp、CLr、T1/2、MRT
- 人体毒性 30：神经毒、DILI、hERG（1/10/30μM 及两档组合共 5）、呼吸毒、肾毒、眼腐蚀、眼刺激、皮肤腐蚀、皮肤刺激、皮肤致敏、急性经皮毒、Ames、小鼠致癌（c/TD50）、大鼠致癌（c/TD50）、啮类致癌、微核、生殖毒、线粒体毒、溶血毒、重复给药毒、急性经口毒（c/LD50）、FDAMDD（c/值）
- EDCs/Tox21 通路 14：AR、ER、AR-LBD、ER-LBD、Aromatase、AhR、ARE、ATAD5、HSE、p53、PPARγ、MMP、TR、GR
- 生态毒 16：蜜蜂、鸟类（Colinus virginanus、Anas platyrhynchos）、水生（P. subcapitata、Crustaceans、D. magna、Fish、Fathead minnow、Bluegill sunfish、Rainbow trout、Sheepshead minnow、T. pyriformis c/pIGC50）、BCF（c/值）、生物降解性
- 光毒 3：光诱导毒性、光刺激、光过敏

### 6.7 ADMETlab 3.0——119 端点（分类计数，论文未附逐点表）

物化 21 + 药化 20 + 吸收 9 + 分布 9 + 代谢 14 + 排泄 2 + 毒性 36 + 毒性基团规则 8 = 119；实现 = 77 个 DMPNN 模型（18 回归+59 分类）+ 34 个 RDKit 直算 + 8 条药物规则。论文点名：pKa(acid/base)、熔点、沸点、F50%、PAMPA、CYP 抑制、HLM 稳定性、肾/神经/耳/血液/遗传毒性、hERG、RPMI-8226/A549/Hek293 细胞毒、Caco-2、logD7.4、VDss、半衰期。**逐点全清单在论文 Supplementary Table S1（未随 XML 下载）与官网 Help 节，需补抓后方可逐点核对。**

### 6.8 Deep-PK——73 端点（分类计数）

64 ADMET（吸收 8、分布 5、代谢 13、排泄 3、毒性 35）+ 9 一般物化；49 分类+24 回归；数据源 ADMETlab 2.0、Interpretable-ADMET、toxCSM、pkCSM。论文点名：NR-AR、NR-ER、oral bioavailability 30%、renal excretion（OCT2）。**逐点全清单在 Supplementary S1（gkae254 附件 docx，未下载），需补抓。**
