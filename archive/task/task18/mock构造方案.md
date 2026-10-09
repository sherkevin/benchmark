# task18 · mock 构造方案（字段映射表）

> **用途**：把已拉取的权威库落成 `mock_db.json` 的操作手册——**每个 mock 字段的真值取自哪个库的哪一列、怎么算、provenance 怎么写**。
>
> **本 task 不改 `mock_db.json`**；本文是后续"改 mock_db"系列 task（含暂停中的 task13）的执行依据。
>
> 状态：2026-08-01 ｜ 上游：[`数据字典.md`](数据字典.md)（逐库字段实读）· `task/task12/权威数据源与数据缺口指南.md` §4（五条硬规则 + 9 步顺序）

---

## ★ 结论先行：能立刻落地的三件，卡住的两件

| # | 动作 | 数据是否就位 | 对应指南 §4.3 | 优先级 |
|---|---|---|---|---|
| 1 | **修 15 条 SMILES 错配 + 解除 aa/bh 重复 + 新增 inchikey/pubchem_cid** | ✅ **PubChem 40/40 已落盘** | 步骤 1（🔴 L1） | **最高，可立即执行（= task13）** |
| 2 | **补 11 个 logS 空格** | ✅ **AqSolDB 9,982 条已落盘** | 步骤 2（🟠 L2） | 高 |
| 3 | **`structures` 空壳 → 真 CIF；晶体池 17 → 5,771** | ✅ **3DSC 5,771×92 已落盘** | 步骤 5–6（🔴 L2 / 🟡 L3） | 高 |
| 4 | 补 36 个非 logS 的 ADMET 空格（bbb/herg/caco2/logD/half_life） | ❌ **TDC 未拉** | 步骤 2 剩余 | 中（需先拉 TDC） |
| 5 | `targets` 扩容 + `predict_binding` 换实测值 | ❌ **UniProt/PDB/BindingDB 未拉** | 步骤 3–4 | 中 |

---

## 1. 药物线字段映射

### 1.1 身份层（`molecule_search[]`）——**唯一取源：PubChem**

依据指南 §4.1 规则 3「身份与性质分离取源」。现有 15 条错配的根因正是身份与性质混在一处手工搬运。

| mock 字段 | 来源 | 取值方式 | 备注 |
|---|---|---|---|
| `molecule_search[].smiles` | `PubChem/identity_40mols.json` → `ConnectivitySMILES` | 按真名（`_mapping_scoring_only.admet` 的 value）直查替换 | ⚠️ API 实际返回字段名是 `ConnectivitySMILES`，非请求的 `CanonicalSMILES` |
| `molecule_search[].inchikey` | 同上 → `InChIKey` | **新增字段** | 比 SMILES 稳定（SMILES 有多种等价写法） |
| `molecule_search[].pubchem_cid` | 同上 → `CID` | **新增字段** | 身份锚，可回查 PubChem |
| `molecule_search[].name` | 不变（匿名 ID） | — | 匿名化不破坏（真名仍只在 `_mapping_scoring_only`） |

**入库门（指南 §4.1 规则 4）**：每条入库时用 RDKit 算分子式，与 PubChem 的 `MolecularFormula` 比对，不一致则拒收。**已验证该门有效**——正是它精确识别出 15 条真错配（另 18 条仅书写风格差异、7 条立体化学差异，不算错）。

### 1.2 性质层（`admet[]`）——按性质分源

| mock 字段 | 来源库 | 来源列 | 状态 |
|---|---|---|---|
| `logS` | **AqSolDB** | `Solubility`（logS，范围 -13.17~2.14） | ✅ 数据就位，可补 11 个空格 |
| `bbb` | TDC ADMET (BBB_Martins) | 二分类标签 | ❌ 待拉 TDC |
| `herg` | TDC ADMET (hERG) | 二分类标签 | ❌ 待拉 TDC |
| `caco2` | TDC ADMET (Caco2_Wang) | 回归值（log cm/s） | ❌ 待拉 TDC |
| `logD` | TDC / 实测库 | — | ❌ 待拉；**AqSolDB 的 `MolLogP` 是 RDKit 计算值，不可当真值** |
| `half_life_days` | TDC ADMET (Half_Life_Obach) | 回归值 | ❌ 待拉 TDC |

**匹配键**：AqSolDB 用 `InChIKey` 与 PubChem 身份对齐（不用 name，避免同名异构体）。当同一分子多条记录时，取 `Occurrences` 高、`SD` 小者，并在 provenance 记录所选 `ID`。

> **重要事实**：mock 现有的 ADMET **性质值本身是正确的**（task13 已逐值核验 6 分子共 24 项全部一致），错的只有 SMILES 一列。所以补 TDC 的目的是**填空格**，不是纠错。

---

## 2. 材料线字段映射

### 2.1 `crystals[]` 与 `structures[]` ——主源 3DSC

| mock 字段 | 来源列 | 取值方式 | 备注 |
|---|---|---|---|
| `crystals[].tc` | `tc` | 直接取（K） | 5,771 条全非空，范围 0.0–132.9 |
| `crystals[].structure_in_db` | 由 `cif` 非空推导 | `cif` 有内容即 true | 3DSC 本身就是"有结构"的子集 |
| `structures[].cif` | `cif` | 直接取完整 CIF 文本 | **补空壳的关键**，同时修 `preview_structure_file` 恒返 P6/mmm 的假实现 |
| 材料真名（评分侧） | `formula_sc` / `pretty_formula_2` | 双列交叉校验 | 写入 `_mapping_scoring_only.crystals` |
| MP 溯源锚 | `material_id_2` | 记入 provenance | 可回查 Materials Project（即使本环境 403） |
| 分层/抽样标签 | `sc_class` | 按族抽样 | 防污染 + 题目分层 |

### 2.2 一个必须自行标注的字段（不能从 3DSC 直接取）

**`crystals[].condition`（常压/高压）——3DSC 没有这一列。**

这是我方 open 类陷阱题的核心字段（`xd`=LaH10 的 tc=250K 但只在 ~170GPa 成立）。3DSC 未提供压力条件列，因此：

- **必须按材料族 + 文献人工标注**，不可从 3DSC 推导；
- 已知高压相（氢化物超导如 LaH10/H3S）需显式标 `high_pressure(~xxxGPa)`；
- **标注来源必须写入 provenance**，不能默认 ambient。

> 这是本轮拉数发现的**方案级缺口**：原以为 3DSC 能提供全部材料线字段，实测它不含压力条件。

---

## 3. provenance 书写口径（承指南 §4.2）

每条记录带三元组 `{source_db, source_url, source_paper_doi}`；case 级写法：

| case 类型 | provenance 写法 |
|---|---|
| 药物 case（身份） | 「分子身份取自 PubChem（CID/InChIKey/ConnectivitySMILES），公共领域」 |
| 药物 case（logS） | 「水溶解度取自 AqSolDB（Sci Data 6:143, 2019, DOI 10.1038/s41597-019-0151-1）」 |
| 药物 case（其他 ADMET） | 「基于 TDC ADMET benchmark 的 BBB 渗透性与 hERG 毒性数据集」（待 TDC 拉取后填具体子集名） |
| 材料 case | 「基于 3DSC 超导体结构-Tc 数据集（Sci Data 10:816, 2023, DOI 10.1038/s41597-023-02721-y），上游为 NIMS SuperCon 与 Materials Project」——**双标上游** |
| 材料 case（condition 字段） | 「压力条件为人工标注，来源：<文献>」——**必须显式说明非数据库直取** |

---

## 4. 执行顺序建议（按数据就位程度重排指南 §4.3）

| 序 | 动作 | 数据 | 归属 task |
|---|---|---|---|
| **1** | 修 15 条 SMILES + 解 aa/bh 重复 + 加 inchikey/pubchem_cid，跑 `verify_snapshot_identity.py --strict` 退出码转 0 | ✅ PubChem 就位 | **task13（可立即恢复）** |
| **2** | 补 11 个 logS 空格（按 InChIKey 匹配 AqSolDB） | ✅ AqSolDB 就位 | 新 task |
| **3** | `structures` 填真 CIF + 晶体池扩容 + **人工标注 condition** | ✅ 3DSC 就位 | 新 task |
| 4 | 拉 TDC ADMET，补剩余 36 个空格 | ❌ 待拉 | 新 task |
| 5 | 拉 UniProt/PDB/BindingDB，扩 `targets`、换 `predict_binding` 硬编码 | ❌ 待拉 | 新 task |

**注意**：步骤 1 的数据已完全就位，且 `verify_snapshot_identity.py` 入库门已存在（T-45 交付）——**task13 现在没有任何阻塞**。
