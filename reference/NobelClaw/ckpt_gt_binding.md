# ckpt → 可靠数据来源 · 逐条绑定表(5 步法 Step 4 正式交付)

| 字段 | 值 |
|---|---|
| **这文档干什么** | 把 6 道样板题的**每一个 ckpt**钉死到一个**可靠**数据来源:具体源 + 怎么拉 + license + **可靠性等级** + fallback。这是 Step 5(组装真题)直接要用的"配料表"。 |
| **上游** | `ckpt_master_list.md`(§3-D 源分组 + §7 spike 实测)· 4 路 GT spike 报告(2026-06-12) |
| **可靠性图例** | 🟢 稳(本地规则/解析 trace/在手论文/CC0 开放无 key)· 🟡 可用但有坑(ShareAlike / 需后筛翻页 / 语义需 κ)· 🔴 需人工 curation 或先解阻塞(逐题文献 / firewall) |

> **白话**:上一步只确认了"这些库有货";这一步是把**每个检查站到底去哪个库、用哪条命令拿、那库靠不靠谱**一条条写死,Step 5 才能照着批量造题。

---

## ⚠️ 读表前先懂:为什么"一个 ckpt 一个源" ≠ 数据量小

常见误会:看下面每个 ckpt 只对一个源,觉得"数据好像很薄"。其实是两件事:

- **一道题的一个检查点,本来就只该有一个权威答案** —— 这是对的,不是薄。
- **"数据量大"是跨题的,不是跨源的**:同一个库,给同一个检查点,在**几千道不同的题**上各给一个真值。
  - 例:caseD 的"命中稳定相 / Ehull"只写一个源「OQMD」,但 OQMD 有 **3万-6万个化学体系**——**换一个体系就是一道全新的题**,每道真值都从 OQMD 实拉。**一个源 = 这个检查点 × 几万道题**,不是只够一道。
  - 同理:ChEMBL ~1000-2000 靶点(case2)、Open Targets ~100-300(caseF)、USPTO+ZINC ~1万+(caseE)。

**两个例外要诚实说:**
1. **只有 case1 压一篇论文**——它是"手上现成、有真实发表全过程可对照"的 **premium 样板,不 scale**(没法造一千个 Elements-Zero)。"发现漏斗"形状要走量,得像 caseD 一样**在 OQMD 上重建**,不靠攒论文。**"把全过程都写出来的论文"全世界就几十篇级别,是真稀缺品。**
2. **人编 rubric(~15 个 ckpt)**确实不是外部真值、是我方判断;但它们是**每种形状 2-3 个通用模板(策略好不好 / 报告诚不诚实),写一次、所有同形状的题复用**,不是每题重编;且是**非承重**检查点——承重的科学检查点全部由大库锚定。

> **一句话:撑规模的是"大库锚定的科学检查点"(每形状几千-几万道);论文是少量 premium 样板,人编是少量复用模板。** 下面表里"服务哪些 ckpt"看的是"哪个检查点用哪个源",不是"这道题全部数据";真正的题量看 `ckpt_master_list.md` §7 与本节。

---

## 数据源地址簿(审核用 · 每个源:地址 + 说明 + 怎么拿 + license + 可靠性)

> 6 道题所有 ckpt 用到的**全部数据源**都在这。带 **✅实测** 的是 2026-06-12 spike **当场拉通过**的真实端点。下面 §1–§6 每道题的 ckpt 表里"可靠数据源"列即指向这里的编号/名字。

### 一、在线公开库(去网上拉真值)

**①  Nature「Elements Zero」论文**(case1 旗舰真值)
- 地址:仓库内 `nobelclaw/Nature__Elements_Zero.pdf`;过程数值提取版 `nobelclaw/pipline.md`
- 是什么:复刻超导发现全流程的论文,含每步中间真值(初筛 2319 / 三分类 158·385·981 / 锁定 Hf-Zr-Re·Zr-V-Re / 候选实测 Tc 如 ZrRe2=6.4K)
- 怎么拿:直接读仓库这两个文件 · license:论文(内部参考)· 可靠性:🟢 在手
- 服务:case1 cp2/3/6/9/10

**②  OQMD 开放量子材料库** ✅实测(caseD 主源)
- 地址:`https://oqmd.org/optimade/structures`(OPTIMADE);legacy `https://oqmd.org/oqmdapi/formationenergy`
- 是什么:140 万+材料的 DFT 生成能 / 凸包稳定性 / 结构
- 怎么拿:`?filter=elements HAS ALL "Ba","Ti","O" AND nelements=3&response_fields=_oqmd_delta_e,_oqmd_stability,_oqmd_spacegroup`(⚠️ `_oqmd_stability` 只能返回不能 filter→本地后筛;深翻页用返回里的 `links.next`,别用大 offset)
- license:CC-BY 4.0(引 Saal 2013 / Kirklin 2015)· 可靠性:🟡(无 key,实测可用,上述两个使用坑)
- 服务:caseD cp6/8/9

**③  Materials Project**(caseD 交叉验证·fallback)
- 地址:`https://api.materialsproject.org`(官方 client `mp-api`)
- 是什么:15 万+材料 DFT 性质(生成能/带隙/凸包)
- 怎么拿:需免费 API key;⚠️ 本环境 IP 被封(403),只用官方 client 偶尔交叉验证,**勿 scrape**
- license:CC-BY 4.0 · 可靠性:🔴 本环境封(用 ② OQMD 替)· 服务:caseD 交叉验证(非主源)

**④  ChEMBL** ✅实测(case2 主源)
- 地址:`https://www.ebi.ac.uk/chembl/api/data/`(REST)
- 是什么:EMBL-EBI 生物活性库,IC50/Ki/Kd + ADMET 类实测值
- 怎么拿:`activity.json?target_chembl_id=CHEMBL1784&standard_type__in=IC50,Ki,Kd`(GLP-1R=CHEMBL1784,由 UniProt P43220 解析;实测拉到 0.14nM→>10µM 梯度)
- license:⚠️ CC-BY-SA 3.0(ShareAlike → 只存 ChEMBL ID + 查询配方,**勿 bulk dump 整表**)· 可靠性:🟡(无 key,过墙)
- 服务:case2 cp4/5

**⑤  BindingDB**(case2 cp5 fallback,license 更干净)
- 地址:`https://bindingdb.org/rest/getLigandsByUniprots?uniprot=P43220`
- 是什么:结合力专库(Kd/Ki/IC50)· 怎么拿:REST;⚠️ 本环境防火墙挡,需申请加白 `bindingdb.org`(已记 todo_user)
- license:CC-BY 类(比 ④ 干净,无 SA)· 可靠性:🔴 本环境墙(先用 ④)· 服务:case2 cp5 备选

**⑥  UniProt**
- 地址:`https://rest.uniprot.org/uniprotkb/P43220.json` · 是什么:蛋白身份证/序列/结合口袋残基
- 怎么拿:REST,无 key · license:CC-BY 4.0 · 可靠性:🟢 · 服务:case2 cp1

**⑦  PubChem**(case2 cp4 备选,规避 ④ 的 ShareAlike)
- 地址:`https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/...` · 是什么:NCBI 化合物性质/生测
- 怎么拿:PUG-REST,无 key · license:美国公共领域(最干净)· 可靠性:🟢 · 服务:case2 cp4 备选

**⑧  Open Targets Platform** ✅实测(caseF 主源)
- 地址:`https://api.platform.opentargets.org/api/v4/graphql`(GraphQL POST)
- 是什么:靶点-疾病多维关联分(遗传/表达/通路/临床/文献)+ 安全(safetyLiabilities)
- 怎么拿:GraphQL query,如 ABL1(ENSG00000097007)× CML(EFO_0000339)取 overall score + datatypeScores + safetyLiabilities(实测 0.832 + 心脏毒性);⚠️ v26 的 Target 类型无 `knownDrugs` 字段,知名药走 drug/disease 节点
- license:CC0 · 可靠性:🟢 · 服务:caseF cp4/5/6/8;case3 主靶点辅助

**⑨  Reactome** ✅实测(case3 主源)
- 地址:`https://reactome.org/ContentService/`(REST)· 是什么:通路库,有向反应级联
- 怎么拿:`search/query?query=Dasatinib&species=Homo sapiens` 找药/通路;`data/pathway/{stId}/containedEvents` 取有向子事件(判信号轴方向)
- license:CC0(数据)· 可靠性:🟢 ·⚠️ 一律 Reactome,**别用 KEGG**(非公开)· 服务:case3 cp1/3/5

**⑩  SIDER**(caseF cp6 备选)
- 地址:`http://sideeffects.embl.de/`(批量 TSV `meddra_all_se.tsv`)· 是什么:1430 药 × 5880 副作用
- 怎么拿:`curl -A "Mozilla/5.0"` 或下 TSV(WebFetch 被挡)· license:⚠️ CC-BY-SA 4.0(SA,标注)· 可靠性:🟡 · 服务:caseF cp6 备选(主用 ⑧ safety)

**⑪  USPTO-50k 反应库** ✅实测(caseE 主源)
- 地址:`https://raw.githubusercontent.com/connorcoley/retrosim/master/retrosim/data/data_processed.csv`
- 是什么:5 万条带反应类(1-10)+ 原子映射的反应,逆合成标准库 · 怎么拿:下 CSV,本地用(判"反应有没有先例")
- license:公共领域(USPTO 专利文本)· 可靠性:🟢(下载即本地)· 服务:caseE cp6

**⑫  ORD 开放反应数据库**(caseE 增强,⑪ 的更全替代)
- 地址:`https://github.com/open-reaction-database/ord-data`;HF 镜像 `https://huggingface.co/datasets/open-reaction-database/ord-data`
- 是什么:200 万+反应(Protobuf)· 怎么拿:git/HF 下 + `ord-schema` 解析 `.pb.gz`
- license:数据 CC-BY-SA-4.0 / schema Apache-2.0 · 可靠性:🟢 · 服务:caseE cp6 增强

**⑬  ZINC 可购原料库** ✅实测(caseE 主源)
- 地址:`http://files.docking.org/catalogs/source/*.src.txt`(+ tranches `http://files.docking.org/2D/`)
- 是什么:各供应商可购分子(每行 `SMILES catalogID`),判"叶子原料能不能买到"
- 怎么拿:下静态文件 → 本地建 hash 查 membership(⚠️ 别用 `zinc20.docking.org` 网站 API,有验证码)
- license:免费(引 ZINC15/20);重发布只存 ID + 自算 SMILES · 可靠性:🟢(本地)· 服务:caseE cp4

**⑭  AiZynthFinder(参考实现,非数据源)**
- 地址:`https://github.com/MolecularAI/aizynthfinder` · 是什么:现成逆合成引擎,已实现"模板先例判定 + 可购库判定",可借其 stock/route 数据模型省工程
- license:MIT · 可靠性:🟢 · 服务:caseE cp4/6 工程参考

### 二、本地规则 / 工具(零外部依赖,免费无限)

- **⑮ pymatgen** — 本地库 · `Structure.from_str(text, fmt="cif")` 解析得通=合法晶体 · MIT · 🟢 · 服务 case1 cp7、caseD cp2
- **⑯ RDKit** — 本地库 · `Chem.MolFromSmiles` / 反应 SMARTS 模板匹配 · BSD · 🟢 · 服务 caseE cp2
- **⑰ 规则表(黑名单/守恒律)** — 内置常量:毒/放射元素黑名单(Be/Hg/Tl + 放射性)、质量/电荷平衡、预算上限、漏斗单调、树无环 · 🟢 · 服务 case1 cp5、caseD cp3/7、caseE cp5、各守恒 cp

### 三、内部(解析被测系统录像 / 我方撰写)

- **⑱ agent trace(运行录像)** — 解析 agent 跑题全过程,判内部一致(子集/连通/单调)+ 工具落地(覆盖/调用)· 🟢(我方解析)· 服务各 case 的 trace/internal cp
- **⑲ 人编 rubric + κ 校准** — 语义类(策略/报告诚实)无客观真值,我方手写评分清单 + LLM 判官(复用 POC4 llm_judge)+ κ 校准 · 🟡(需校准)· 服务各 case 策略/报告 cp

### 四、🔴 待人工挖(已隔离为可选加分项)

- **⑳ 逐题文献(PubMed/期刊)** — "严格命中已发表过程"类:case2 cp8(模拟 vs 实验)、case3 cp7(IC50/pCRKL 对照)、caseE cp8(已发表合成路线)。需人工翻论文锚一处真值;检索 `https://pubmed.ncbi.nlm.nih.gov/`。🔴 人工 curation · **不挂这几个 cp,trace 仍是完整一条**

---

## 0. 真值类型四分(决定可靠性)

| gt_class | 含义 | 可靠性 | 占比 |
|---|---|---|---|
| **rule** | 本地规则/科学定律(pymatgen/RDKit/黑名单/守恒) | 🟢 最稳(零外部依赖) | ~9 |
| **internal/trace** | 解析 agent 自己的录像(覆盖/连通/单调/子集) | 🟢 稳(只需解析 trace) | ~11 |
| **external-anchor** | 去公开库/论文取真值 | 🟢🟡🔴 视库而定 | ~20 |
| **semantic** | 人编细则 + LLM 判官 + κ 校准 | 🟡(需校准) | ~15 |

---

## 1. case1 超导发现(GT 锚:Nature「Elements Zero」论文,已在手)

| ckpt | gt_class | 可靠数据源 | 怎么拉 | 可靠性 |
|---|---|---|---|---|
| 1 策略 | semantic | 人编细则 | 我们写清单 + κ | 🟡 |
| 2 初筛数 2319 | external | 论文 SI / `matched_results_over4.csv` | 已在手 | 🟢 |
| 3 三分类 158/385/981 | external | 论文 §4.7 | 已在手 | 🟢 |
| 4 MgB2 置信 | external+真算 | 已知超导 + Elements 实测 0.992 | NobelClaw API(材料真算) | 🟢 |
| 5 毒/放射黑名单 | rule | 固定元素表 | 本地 | 🟢 |
| 6 锁定家族 | external | 论文附录图 E.2(Hf-Zr-Re/Zr-V-Re) | 已在手 | 🟢 |
| 7 结构合法 | rule | pymatgen `Structure.from_str` | 本地 | 🟢 |
| 8 生成∈家族 | internal | 解析生成组成 vs 上步锁定 | trace | 🟢 |
| 9 排序 | external | 论文 Tc 表 → Spearman | 已在手 | 🟢 |
| 10 命中真货 | external | 论文实验确认名单(ZrRe2 等) | 已在手 | 🟢 |
| 11 报告诚实 | semantic | trace 交叉验证 + 人编 + κ | trace+rubric | 🟡 |

> **case1 = 唯一满锚旗舰**(9/11 现成),论文真值全在手。

---

## 2. caseD 闭环优化(GT 锚:OQMD)

| ckpt | gt_class | 可靠数据源 | 怎么拉 | 可靠性 |
|---|---|---|---|---|
| 1 策略含闭环+停 | semantic | 人编细则 | 清单 + κ | 🟡 |
| 2 结构合法 | rule | pymatgen | 本地 | 🟢 |
| 3 预算守恒 | trace | 数 `crystal_generate` 调用 ≤ N | trace | 🟢 |
| 4 best 单调 | internal | 逐轮 best Ehull 序列非升 | trace | 🟢 |
| 5 评估覆盖全 | trace | 生成集 ⊆ 评估集 | trace | 🟢 |
| 6 Ehull 数值 | external | **OQMD** OPTIMADE | `filter=elements HAS ALL ... AND nelements=3`,`response_fields=_oqmd_delta_e,_oqmd_stability`;**stability 只能返回不能 filter→本地后筛** | 🟡 |
| 7 收敛即停 | trace+rule | 收敛后无多余 generate | trace | 🟢 |
| 8 命中稳定相 | external | **OQMD** 凸包(E_hull<0.001) | 同上 + 本地筛 stability | 🟡 |
| 9 排序 | external | OQMD Ehull → Spearman | 同上 | 🟡 |
| 10 报告诚实 | semantic | trace + 人编 + κ | trace+rubric | 🟡 |

> **fallback**:Materials Project 被 IP 封(403)+ 要 key → **主用 OQMD**(CC-BY 无 key,实测可用);MP 仅官方 `mp-api` client 偶尔交叉验证。深翻页用 `links.next` 不用大 offset。

---

## 3. case2 GLP-1R 设计(GT 锚:ChEMBL;BindingDB 待加白)

| ckpt | gt_class | 可靠数据源 | 怎么拉 | 可靠性 |
|---|---|---|---|---|
| 1 靶点合法 | external | UniProt P43220 / ChEMBL1784 | UniProt REST / ChEMBL REST | 🟢 |
| 2 目标对 | semantic | 人编细则 | 清单 + κ | 🟡 |
| 3 novelty 覆盖 | trace | check_novelty 覆盖全候选 | trace | 🟢 |
| 4 ADMET 数值 | external | **ChEMBL** assay(solubility/logP/CL) | `chembl/api/data/activity.json?...` | 🟡 ShareAlike |
| 5 Kd/IC50 数值 | external | **ChEMBL**(CHEMBL1784,实测 0.14nM→>10µM) | 同上;BindingDB=fallback(待加白) | 🟡 ShareAlike |
| 6 对接⊆ADMET过 | internal | 子集校验 | trace | 🟢 |
| 7 细胞反应方向 | external/semantic | 教科书机理(可转硬判符号) | 文献机理 + 人编 | 🟡 |
| 8 模拟 vs 实验 | external | **具体文献**(⬜ 逐题手挖) | 人工 curation | 🔴 |
| 9 漏斗守恒 | trace | 各阶段计数单调 | trace | 🟢 |
| 10 报告诚实 | semantic | trace + 人编 + κ | trace+rubric | 🟡 |

> **fallback**:ChEMBL 是 CC-BY-SA → **只存 ChEMBL ID + 查询配方,不 bulk dump**(规避 ShareAlike);或对 cp4 用 **PubChem**(美国公共领域)。BindingDB(更干净)待加白后替 cp5。

---

## 4. case3 Dasatinib 机理(GT 锚:Reactome)

| ckpt | gt_class | 可靠数据源 | 怎么拉 | 可靠性 |
|---|---|---|---|---|
| 1 主靶点 | external | Dasatinib 真实靶点(Reactome drug entity / DrugBank / ChEMBL) | Reactome `search/query=Dasatinib` | 🟢 |
| 2 机理方向 | semantic | 公认机理(文献)+ 人编 | rubric + κ | 🟡 |
| 3 扰动节点白名单 | external | **Reactome** 通路节点 | `ContentService` 取 pathway 节点集 | 🟢 CC0 |
| 4 耐药 T315I | external | 公认耐药突变(文献/COSMIC) | 关键词 + 清单(锚一处文献) | 🟡 |
| 5 信号方向 pCRKL/pSTAT5↓ | external | **Reactome** 有向级联 | `data/pathway/{id}/containedEvents` | 🟢 CC0 |
| 6 表型符号 | rule/external | 抑癌→增殖↓凋亡↑(公认) | 规则 | 🟢 |
| 7 模拟 vs 文献 | external | **具体文献**(⬜ 手挖 IC50/pCRKL) | 人工 curation | 🔴 |
| 8 因果链连通 | internal | 图连通校验 | trace | 🟢 |
| 9 诚实标注 | semantic | trace + 人编 + κ | trace+rubric | 🟡 |

> **fallback**:通路一律用 **Reactome(CC0),不用 KEGG**(非公开)。

---

## 5. caseE 逆合成(GT 锚:USPTO-50k + ZINC)

| ckpt | gt_class | 可靠数据源 | 怎么拉 | 可靠性 |
|---|---|---|---|---|
| 1 策略 | semantic | 人编细则 | rubric + κ | 🟡 |
| 2 反应步合法 | rule | RDKit 反应解析 | 本地 | 🟢 |
| 3 路径连通 | internal | 树图连通 | trace | 🟢 |
| 4 叶可达 | external | **ZINC** 可购集 | `files.docking.org/catalogs/source/*.src.txt` → 下载后本地 hash 查 | 🟢(本地) |
| 5 无退化环 | rule | 树查环 | 本地 | 🟢 |
| 6 反应有先例 | external | **USPTO-50k** 反应类/模板 | `retrosim/.../data_processed.csv`(5万,带类)→ 本地 | 🟢(本地·公共领域) |
| 7 路线长短 | external | 同类路线步数(⬜文献)/启发式 | 启发式优先 | 🟡 |
| 8 命中已发表路线 | external | **已发表全合成路线**(⬜ 手挖,~100-1000) | 人工 curation(可选,非load-bearing) | 🔴 |
| 9 报告诚实 | semantic | trace + 人编 + κ | trace+rubric | 🟡 |

> **fallback**:ZINC 用 `files.docking.org` 静态文件(**别用 zinc20 网站 API,有验证码**);可借 **AiZynthFinder** 的 stock/route 数据模型现成。cp8 是"严格命中"项、非必需,可只 curate 一小批。

---

## 6. caseF 靶点验证(GT 锚:Open Targets)

| ckpt | gt_class | 可靠数据源 | 怎么拉 | 可靠性 |
|---|---|---|---|---|
| 1 策略 | semantic | 人编细则 | rubric + κ | 🟡 |
| 2 证据覆盖 | trace | 必查清单 + 录像覆盖 | trace | 🟢 |
| 3 取证顺序 | rule | 信息价值排序(启发式) | 规则 | 🟢 |
| 4 遗传关联 | external | **Open Targets** `datatypeScores.genetic_association` | GraphQL POST | 🟢 CC0 |
| 5 表达/通路 | external | **Open Targets** datatypeScores | GraphQL | 🟢 CC0 |
| 6 安全查了没 | external+trace | **Open Targets** `safetyLiabilities` / SIDER | GraphQL(OT safety=CC0) | 🟢 |
| 7 结论-证据一致 | internal/rule | 综合证据档 vs 结论一致性 | trace+规则 | 🟢 |
| 8 结论命中真值 | external | **Open Targets** overall `score` | GraphQL | 🟢 CC0 |
| 9 报告诚实 | semantic | trace + 人编 + κ | trace+rubric | 🟡 |

> **caseF 可靠性最高**:核心科学 cp 全锚 Open Targets(CC0、无 key、GraphQL 实测可用)。SIDER(CC-BY-SA)仅 cp6 备选,记得 license 标注。

---

## 7. 可靠性总评 + 风险/fallback 汇总

| 等级 | 数量(近似) | 哪些 | 处置 |
|---|---:|---|---|
| 🟢 稳 | ~38 | 本地规则、解析 trace、在手论文、CC0 库(OT/Reactome/UniProt)、下载后本地(USPTO/ZINC) | 直接用 |
| 🟡 有坑可用 | ~16 | OQMD(后筛翻页)、ChEMBL(ShareAlike)、所有 semantic(需 κ) | 按下方坑表处理 |
| 🔴 需 curation/解阻塞 | ~4 | 逐题文献对照(case2-8 / case3-7 / caseE-8)+ BindingDB(firewall) | 见下 |

**坑 → 处置(这就是"可靠"的兑现)**:
1. **ChEMBL CC-BY-SA** → 只存 ID + 查询配方不 bulk dump;或 cp4 改 PubChem(公共领域)。
2. **OQMD** stability 不可 filter / 深 offset 超时 → 本地后筛 + `links.next` 翻页。
3. **Materials Project 403+key** → 不 scrape,主用 OQMD,MP 仅官方 client 交叉验证。
4. **BindingDB firewall** → 先用 ChEMBL;申请加白(已记 `todo_user`)。
5. **ZINC 验证码** → 用 `files.docking.org` 静态。
6. **KEGG 非公开** → 一律 Reactome。
7. **🔴 逐题文献对照(~3 个 ckpt)** → 这几个是"严格命中已发表过程"的加分项、**非 load-bearing**;先 curate 一小批(几道),其余题不挂这个 cp 也成立。**这是唯一需要人工、最不可靠的一块,已隔离成可选项。**
8. **semantic(~15)** → 复用 POC4 跑通的 llm_judge + κ 校准;能转硬判的转(机理方向→符号)。

---

## 8. 结论

- **除 ~3 个可选的"逐题文献"ckpt 外,所有 ckpt 都已绑定到可靠、可程序化获取的数据源**(本地规则/trace + CC0 开放库 + 在手论文 + 下载即本地)。
- 🔴 风险已隔离:firewall 有 fallback,文献类是非必需加分项。
- → **Step 4(找 ckpt 可靠数据源)到此真正完成**;Step 5(按 Σ 公式组装真题)的"配料表"齐了。
