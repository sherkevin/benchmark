# task18 — 完成汇报

## 一、结论先行

1. **三个库完整落盘，数据底座已就位**：3DSC（5,771 材料 × 92 列，含 Tc 与真实 CIF）、AqSolDB（9,982 分子 logS）、PubChem（40/40 分子身份）。全部通过完整性校验（字节数 = 远端 Content-Length）并留 md5。
2. **最重要的产出：task13 的阻塞已解除。** PubChem 40/40 身份全获取，用 RDKit 规范化精确比对后，**真错配 15 条，与 task11 早先查出的"15/40"精确吻合**；`aa`(Metoprolol) 与 `bh`(Propranolol) 的 SMILES 在权威源里确实不同，**重复可解除**。
3. **发现一个方案级缺口（原以为不存在）**：**3DSC 不含"常压/高压"列**。而 `condition` 是我方 open 类陷阱题的核心字段（LaH10 的 tc=250K 只在 ~170GPa 成立），**必须人工按文献标注，不能从 3DSC 推导**。
4. **实测纠正一个 API 认知**：PubChem 请求 `CanonicalSMILES`，**实际返回字段名是 `ConnectivitySMILES`**——写脚本不能假设字段名与请求名一致。
5. **UCI SuperCon 未拉全**（zip 2.4MB、EOCD 校验失败，站点响应慢）。它属 L3（池子 5.7k→21k），**不阻塞主线**。

## 二、做了什么

| 子目标 | 完成情况 |
|---|---|
| 1 拉取脚本 | ✅ `src/data_download/fetch_authoritative_sources.py`：断点续传 + 远端 Content-Length 校验 + zip 结构校验 + MANIFEST（url/时间/字节/行数/md5/完整性） |
| 2 A 档全量落盘 | ✅ 3DSC、AqSolDB 完整（**3/4**，UCI 未全） |
| 3 B 档子集落盘 | ✅ PubChem 40/40；BindingDB/UniProt/PDB/ChEMBL 本轮未拉（见遗留） |
| 4 数据字典 | ✅ `数据字典.md`：逐库字段/量纲/缺失/可映射性质，含 API 改名与读取陷阱 |
| 5 构造方案 | ✅ `mock构造方案.md`：字段映射表 + provenance 口径 + 按数据就位程度重排的执行顺序 |
| 6 账本 | ✅ 见 §五 |

## 三、关键实测数据

| 库 | 规模 | 完整性 | md5 | 关键字段 |
|---|---|---|---|---|
| 3DSC | 5,771 × 92 | ✅ 9,457,406 = 远端 | `321116ee…4416` | `tc`（0–132.9K 全非空）、`cif`（完整结构）、`material_id_2`（MP 溯源）、`sc_class` |
| AqSolDB | 9,982 × 26 | ✅ 3,750,252 = 远端 | `3edc4468…3e90` | `Solubility`（logS -13.17~2.14 全非空）、`SD`、`Occurrences` |
| PubChem | 40/40 | ✅ 逐条 200 | — | `CID`、`ConnectivitySMILES`、`InChIKey`、`MolecularFormula` |
| UCI SuperCon | ❌ 2.4MB | ❌ zip 损坏 | — | 未解压 |

### 40 分子身份核验（RDKit 规范化精确比对）

| 类别 | 数量 |
|---|---|
| 规范化后一致（仅书写风格差异，如芳香小写 vs Kekulé） | 18 |
| 分子式相同、写法不同（立体化学/互变异构） | 7 |
| **分子式不同＝真错配** | **15**（与 task11 结论吻合） |
| mock SMILES 非法 | 0 |

## 四、证据在哪

| 内容 | 位置 |
|---|---|
| 原始库 + MANIFEST | `dag_poc/data/sources/{3DSC,AqSolDB,PubChem,UCI_SuperCon}/`（**gitignore 不入库**） |
| 拉取脚本 | `src/data_download/fetch_authoritative_sources.py` |
| 逐库字段解读 | `task/task18/数据字典.md` |
| 字段映射表与执行顺序 | `task/task18/mock构造方案.md` |

## 五、遗留

| # | 遗留 | 处置 |
|---|---|---|
| 1 | **UCI SuperCon zip 不完整** | L3 不阻塞；需要时改用作者 repo `github.com/khamidieh/predict_tc` |
| 2 | **TDC ADMET 未拉** | 影响 36 个非 logS 空格；mock 现有性质值本身正确（只缺不错），优先级低于身份修复 |
| 3 | **BindingDB / UniProt / PDB 未拉** | 服务 `targets` 扩容与 `predict_binding` 换实测值（指南 §4.3 步骤 3–4） |
| 4 | **`condition` 字段无数据源** | 3DSC 不含压力条件列，**必须人工按文献标注并写入 provenance**（本轮新发现） |
| 5 | **网络不稳导致下载易截断** | 脚本已内置断点续传 + Content-Length 校验；zip 类需额外结构校验（本轮 UCI 即因此被识别为损坏） |

## 六、下一步（数据已就位，无阻塞）

按 `mock构造方案.md` §4 的重排顺序：

1. **恢复 task13**：修 15 条 SMILES + 解 aa/bh 重复 + 加 `inchikey`/`pubchem_cid`，跑 `verify_snapshot_identity.py --strict` 至退出码 0。**PubChem 数据已就位，无任何阻塞。**
2. 补 11 个 logS 空格（AqSolDB 按 InChIKey 匹配）。
3. `structures` 填真 CIF + 晶体池扩容 + 人工标注 `condition`。
