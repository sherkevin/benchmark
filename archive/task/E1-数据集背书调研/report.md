# E1 · 步 1 — 数据集背书调研 · 收口报告（report.md）

> 任务：`task/E1-数据集背书调研/task.md`（goal.md v3.0 §6 步 1）｜执行：E1-数据线｜收口日期：2026-08-06
> 遵循 `ACTION_RULES.md` §4/§5：回答做了什么、结论是什么、证据在哪、遗留问题是什么。

---

## 一、核心结论（3 句）

1. **背书核验**：16 行数据集（task12 的 15 库口径全覆盖，SuperCon 的 UCI #464 衍生快照单列）经三口径逐库核验，**8 达标 / 8 未达标**，未达标原因全部是口径①（在案无"被用于构建 benchmark"记录），如实标注、不凑数。
2. **落盘完成度**：`dag_poc/data/sources/` 下 **11 库 MANIFEST 六要素齐全、complete 全 true**，完整性自检脚本全绿；当前环境不可达/受限的 4 库（MP/OQMD/MPDS/SuperCon NIMS）与注册制暂缓的 PDBbind 未落盘，与主表"取成品还是 raw"判定无矛盾。
3. **对下游的意义**：E1-后端数据库构造（步 4）可直接以达标库的冻结快照为真值底座；论文数据可信度论证须区分"benchmark 交叉采用背书"（达标 8 库）与"机构/期刊权威"（未达标 8 库）两类证据，材料线锚点 3DSC 属后者。

---

## 二、做了什么（按执行顺序，各带证据路径）

1. **认领 task**：`task.md` 表头更新为「🚧 进行中」（2026-08-06）。证据：`task/E1-数据集背书调研/task.md` 表头。
2. **三口径背书核验**：对 15 库（16 行）逐个核验口径①（被用于造 benchmark，证据回链 `research/benchmark数据库调研报告.md` §一交叉采用表）、口径②（venue+DOI/arXiv，转录 DBLP 核准结论）、口径③（公开 URL 当场实测）。证据：`数据集背书调研报告.md` §二主表（16 行 × 11 列）＋§三口径明细；reference/ 原文抽查 8 条（≥5）见附录 A，全部属实。
3. **可达性当场重测**：2026-08-06 15:12–15:16 CST，16 库 19 端点逐条 curl（未沿用 task12 历史结论，未下载任何数据）。证据：`reachability_retest_result.md`（原始记录＋补测）、`reachability_retest.sh`（脚本）。
4. **11 库落盘核验（只读引用）**：`dag_poc/data/sources/` 下 3DSC/AqSolDB/PubChem/TDC_ADMET/BindingDB/UniProt/PDB/ZINC/JARVIS/UCI_SuperCon/ChEMBL（ID-only）共 11 库 MANIFEST 六要素（url/时间/字节/行数/md5/完整性）齐全、complete 全 true。证据：各库 `MANIFEST.json`；自检脚本 `src/data_download/verify_sources_integrity.py`，输出 `src/data_download/integrity_check_output.txt`（[PASS]，complete=false 0 个）。
5. **报告终稿审校与收口**：通读主报告，补全缺失的「数据落盘状态」一节（报告 §六），其余核验结论未推翻、未改动。证据：`数据集背书调研报告.md`（终稿版头标注「1d 收口审校」）。

---

## 三、关键结论与证据

### 3.1 达标 8 库（清单＋背书来源）

| 库 | 背书来源（口径①） | 注记 |
|---|---|---|
| TDC ADMET | 自身即 benchmark（NeurIPS D&B 2021）；PMO、Biomni、SciAgentArena 采用 | 与我方目标 track 同型，权威性最高 |
| AqSolDB | 经 TDC 构建 logS 任务**间接背书** | 路径已如实注明 |
| PubChem | ChemCrow、ChemToolAgent、TOMG-Bench 采用（3 家） | NIH 官方＋公共领域 |
| ChEMBL | GuacaMol（训练集＋分布基线）、TDC B 类 oracle 预训练 | **CC-BY-SA 3.0，只存 ID＋查询配方**，不可 bulk dump |
| BindingDB | Biomni 采用（预训练 DTI 模型形式，research 报告 §2.4 订正） | **弱背书**：仅 1 家＋预印本＋模型形式 |
| PDB | Tartarus 对接 oracle、PoseBench 结构集（2 家） | PoseBench venue 待复核注记 |
| ZINC | MOSES benchmark 基底、TOMG-Bench 经 Zinc-250K（2 家） | MOSES venue 待期刊页复核注记 |
| Materials Project | MADE 采用（材料线**唯一** benchmark 背书） | ICML 2026 待官方 proceedings 复核；本环境 403 |

### 3.2 未达标 8 库（清单＋缺口径）

| 库 | 缺口径 | 一句话原因 |
|---|---|---|
| UniProt | ① | 在案 14 benchmark 调研范围内无"用于造 benchmark"记录 |
| PDBbind | ① | 同上；注册制另增落地成本，暂缓 |
| 3DSC | ① | 无第三方 benchmark 采用记录（权威性另有来源，见 §3.4） |
| SuperCon via UCI #464 | ① | Hamidieh 2018 为发布＋模型论文，非 benchmark 采用 |
| SuperCon (NIMS) | ①＋③ | 零家背书（research §1.3 全仓 grep 证据）＋主站 000 |
| OQMD | ① | 无第三方采用记录；可达性另判"不稳定" |
| MPDS | ① | 无背书记录；SID 注册制 |
| JARVIS | ① | Choudhary & Garrity 2022 为自身论文，无第三方采用 |

### 3.3 可达性与 task12 历史结论的差异（3 处）

1. **OQMD 翻转**：task12 记"000 不可达"，本次 3 连测 200/200/000，判"不稳定可达"——坐实已知坑 1"历史可达性结论会失效"，取数前必须重测。
2. **ChEMBL 旧端点迁移**：主站/status.json 均 200，但旧式 `/api/data/molecule/<ID>/JSON` 返回 404，旧查询配方需验证新路径。
3. **新增首测 3 库**（task12 未测）：PDBbind 200、MPDS 基础端点 200（取数仍需 SID）、JARVIS 200。

（MP 403、SuperCon NIMS 000 与 task12 一致，属预期内，不计差异。）

### 3.4 材料线锚点 3DSC 的背书属性如实界定

3DSC **未达背书标准**（口径①在案无第三方 benchmark 采用记录）。其权威性来自另一类证据：**Scientific Data 10:816 (2023) 同行评议＋MP/SuperCon 双上游 provenance**。这两类证据必须分开表述：论文中 3DSC 应写为"同行评议数据集，上游锚定 Materials Project（被 MADE 采用）与 NIMS SuperCon"，**不得**写成"被 benchmark 采用的库"。"不等于被 benchmark 采用"也不影响其作为材料线真值锚点的工程可用性（A/B 分层判定为"支持"，材料线首选）。

---

## 四、遗留与交接

1. **ChEMBL 旧 API 端点失效**：下游 E1-后端数据库构造若复用旧查询配方，取数前必须先验证新 API 路径（本 task 边界内未改任何代码，仅记录）。叠加 CC-BY-SA 3.0，维持"只存 ID＋查询配方"规则。
2. **BindingDB 弱背书需论文显式声明**：唯一在案背书来自预印本且为预训练模型形式使用；我方取 raw 实测记录的用法无第三方先例，论文中应显式声明该差异。
3. **get_property 数据源注意事项**：
   - `binding_affinity`：当前真值源为 BindingDB raw 实测记录（Kd/Ki/IC50），弱背书（见上）；PDBbind 为注册制备选，未落盘。
   - `logD`：TDC ADMET 落盘数据中该列存在缺失（入库门核验 ADMET 填充率 80.4%，缺 caco2/logS/logD 等列），补空格子需按 TDC 上游子集口径处理，不得编造。
4. **OQMD 不稳定**：若后续纳入取数链路，必须先当场重测并准备降级方案，不得依赖本次结论。
5. **本 task 未改任何共享文件的声明**：本次收口仅写 `task/E1-数据集背书调研/` 内三个文件（主报告修订、report.md、progress.md）；未写 `task/E1-后端数据库构造/**`、`src/**`、`dag_poc/**`、`TODO.md`、`problems.md`、`goal.md`、`governance/adr/**` 及其他 `task/E*/`；对 11 库 MANIFEST 与自检输出仅读取引用。
6. **TODO 账本同步触发**：本线已收口，请监工将 TODO.md T-60（数据集背书调研）置 ✅ 并回链本 report。

---

## 五、交付物清单（路径）

| # | 交付物 | 路径 |
|---|---|---|
| 1 | 背书调研报告终稿（主表 16 行 × 11 列＋三口径明细＋可达性对照＋A/B 分层＋落盘状态＋附录抽查 8 条） | `task/E1-数据集背书调研/数据集背书调研报告.md` |
| 2 | 可达性重测原始记录＋脚本 | `task/E1-数据集背书调研/reachability_retest_result.md`、`reachability_retest.sh` |
| 3 | 11 库落盘（MANIFEST 六要素齐全、complete 全 true） | `dag_poc/data/sources/{3DSC,AqSolDB,BindingDB,ChEMBL,JARVIS,PDB,PubChem,TDC_ADMET,UCI_SuperCon,UniProt,ZINC}/MANIFEST.json` |
| 4 | 完整性自检（全绿输出） | `src/data_download/verify_sources_integrity.py`、`src/data_download/integrity_check_output.txt` |
| 5 | 收口报告（本文件） | `task/E1-数据集背书调研/report.md` |
| 6 | 进度看板 | `task/E1-数据集背书调研/progress.md` |

---

## 监工验收意见（2026-08-06，实跑复核）

**结论：通过。**

| 验收项 | 实跑证据 |
|---|---|
| 11 库 MANIFEST 六要素齐全、`complete` 全 true | `python3 src/data_download/verify_sources_integrity.py` → **[PASS]，11/11 六要素齐全，complete=false 清单 0 个**，退出码 0；监工另独立聚合 11 个 MANIFEST，四项关键字段 4/4 命中、`complete` 全 True |
| 背书判定不凑数 | 自评 **8 达标 / 8 未达标**，未达标原因统一为口径①（在案无"被用于构建 benchmark"记录）并如实标注——**没有把"机构/期刊权威"混算成"benchmark 交叉采用背书"**，这个区分对论文的数据可信度论证是必要的 |
| 可达性未沿用历史结论 | 2026-08-06 15:12–15:16 CST 当场 curl 16 库 19 端点，脚本与原始记录均落盘（`reachability_retest.sh` / `reachability_retest_result.md`） |
| 不可达库的处理 | MP/OQMD/MPDS/SuperCon-NIMS 与注册制的 PDBbind 未落盘，**如实标注而非用替代源冒名充数** |

### 特别记功：查出并纠正了一个错误 DOI

`src/data_build/condition_annotations.py` 记录：契约 §4.3 示例中的 LaH10 DOI `10.1038/s41586-019-1410-1` 经 CrossRef 核验实为**另一篇论文（白血病免疫学）**，真实 LaH10 论文是 `10.1038/s41586-019-1201-8`。

**这是主动核验发现的**，不是被要求核的。同时它把标注红线写死为"**命中即标注，未命中一律记「未知」，不得默认 ambient**"——这条纪律正是后来监工在容量核算中发现"condition 覆盖率仅 0.3%"时，能确认那 5,755 条"未知"是**诚实留空而非错误标注**的原因。

### 遗留（转入 task-02，不影响本轮通过）

1. `family` 字段有两处拼接错误（`OxideHeavy_fermion` 4 条、`Heavy_fermionChevrel` 7 条）；
2. `condition` 标注覆盖 16/5,771 = 0.3%，是"常压超导"科学卖点的前置条件。
