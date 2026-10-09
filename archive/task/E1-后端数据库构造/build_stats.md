# E1 · Task #7 — 逐 tool 子库构造与入库校验门 · 构造统计（build_stats）

> 执行人：E1-数据线｜执行日期：2026-08-06｜施工依据：`后端库规格契约.md` v1.0（唯一依据）
> 构造脚本：`src/data_build/`｜落盘位置：`dag_poc/data/backend/`（已向监工报备）
> 硬红线自查：未真实计算任何性质（全部性质值来自落盘数据源，无数据记"待补/未知"）；未引 DFT 预测值（JARVIS 未使用）；A 侧记录无一来自 B 侧文件（一侧一文件）；每条记录 provenance 三元组齐全（断言二逐行强制）。

## 1. 构造脚本清单与子库落盘清单

### 1.1 脚本清单（src/data_build/）

| 脚本 | 职责 | 对应契约 |
|---|---|---|
| `common.py` | 共用：provenance 注入、JSONL 确定性写出、A/B 分文件、MANIFEST、化学式解析/掩码 | §1 §2 §4 |
| `ingest_gate.py` | 入库校验门三道断言 + `--selftest` 回归 | §5 |
| `condition_annotations.py` | 3DSC condition 人工标注表（DOI 逐条经 CrossRef 核验） | §4.3 |
| `build_search_candidates.py` | 候选池 A 侧 + 实体注册表 + B 侧空占位 | §3.2 |
| `build_get_entity.py` | 身份档 A 侧（消费实体注册表，与候选池 1:1）+ B 侧空占位 | §3.3 |
| `build_get_property.py` | 性质行 A 侧（一行一 (实体,性质) 对）+ B 侧空占位 | §3.4 |
| `build_literature_search.py` | CrossRef 真实题录冻结快照（缓存可离线重跑） | §3.1 |
| `build_check_molecule_exists.py` | 仅 B 侧双键集（本阶段空占位，字段白名单守门） | §3.5 |
| `finalize_backend.py` | 全量落盘复核（三断言重放 + A/B 隔离断言）+ 根 MANIFEST | §5 §2.3 |

### 1.2 子库落盘清单（实测）

| 子库 | 文件 | 记录数 | 体积（du 实测见 §5） |
|---|---|---:|---|
| literature_search | `literature_search_A.jsonl`（无 B 侧） | **321**（契约下限 ≥300 ✓） | 200K |
| search_candidates | `search_candidates_A.jsonl` | **80,712** | 39M |
| search_candidates | `search_candidates_B.jsonl` | 0（空占位+说明） | — |
| get_entity | `get_entity_A.jsonl` | **80,712**（与候选池 1:1 ✓） | 36M |
| get_entity | `get_entity_B.jsonl` | 0（空占位+说明） | — |
| get_property | `get_property_A.jsonl` | **27,618** | 9.3M |
| get_property | `get_property_B.jsonl` | 0（空占位+说明） | — |
| check_molecule_exists | `check_molecule_exists_B.jsonl`（仅 B 侧） | 0（空占位+说明） | — |

**A 池构成**（search_candidates_A / get_entity_A 同集）：
- PubChem 40 分子身份档：40（ID 沿用 mock_db aa..ej，真名只存 provenance 之外不暴露）；
- ZINC 药物分子池：**74,901**（磁盘实存 100,044 行 → 唯一 SMILES 串 86,161 → 非法拒收 232 → canonical 去重 11,028 → 保留 74,901；契约下限 ≥10,000 ✓。源文件截断问题见 §6.2）；
- 3DSC 材料池：**5,771**（全量过门，0 拒收）。

**get_property_A 分行统计**（一行一 (entity_id, prop) 对）：

| prop | 行数 | 来源 |
|---|---:|---|
| bbb | 757 | TDC_ADMET:BBB_Martins |
| herg | 104 | TDC_ADMET:hERG |
| caco2 | 235 | TDC_ADMET:Caco2_Wang |
| half_life | 523 | TDC_ADMET:Half_Life_Obach（hr） |
| logD | 604 | TDC_ADMET:Lipophilicity_AstraZeneca（实验 logD7.4，构造期锁定来源） |
| logS | 2,311 | AqSolDB（多源取 Occurrences 高、SD 小者） |
| tc / family / structure_in_db / condition | 5,771 ×4 | 3DSC / 3DSC / cif 列非空推导 / **人工标注** |

药物线命中 3,427 个 A 池分子（与 TDC/AqSolDB 按最大片段 canonical SMILES 匹配）；未命中实体不出行（契约"缺失即无此行"）。

## 2. 入库校验门三道断言执行结果

### 2.1 回归自测（契约 §5 第 2 条要求，`ingest_gate.py --selftest`）

- 断言一正向：现有 40 分子全部通过（真值表从 `identity_40mols.json` 动态载入）✓
- 断言一负向：注入 1 条已知错配（aa 旧 SMILES，取自 `task/task13/修复明细.json` 的 old 字段）必被拒收 ✓（`C16H21NO2 vs C15H25NO3`）
- 断言二负向：占位符 provenance 被拒收 ✓；断言三：ChEMBL 记录命中 SA 红线 ✓

### 2.2 构造期过门统计（每行写入前过门）

| 子库 | 过门通过 | 拒收 | 拒收原因 |
|---|---:|---:|---|
| search_candidates | 80,712 | 232 | 全部为断言一「SMILES 非法（RDKit 无法解析）」——ZINC 源中的金属配合物/脏行（如 `[Fe+3]` 卟啉盐、孤立引号行） |
| get_entity | 80,712 | 0 | — |
| get_property | 27,618 | 0 | — |
| literature_search | 321 | 0 | — |
| check_molecule_exists | 0 | 0 | 本阶段无 gt 集 |

断言三（SA 红线）：五库整库级检查 ChEMBL/SIDER 记录数均 = 0 ✓（ChEMBL 仅以 ID+配方存在于 sources，未入任何子库）。

### 2.3 落盘后全量复核（finalize_backend.py，从文件读回重放）

8 个 jsonl 文件逐行重放断言一/二 + 整库断言三 + check_molecule_exists 三字段白名单：**全部通过，复核拒收 0**。A/B 隔离断言（A 侧返回对象 ∩ B 侧独有 id = ∅）：B 侧 id 集合大小 0、A 侧泄漏 0 ✓（B 非空后该检查按一般形式自动生效）。

拒收清单落盘：`src/data_build/_build_state/rejections_search_candidates.jsonl`（232 条，含行摘要/断言编号/原因）。

## 3. condition 标注策略与示例

**策略**（契约 §4.3 红线：不得从 3DSC 推导、未标注记"未知"不默认 ambient）：
1. `condition_annotations.py` 维护「化学式 → (condition 值, 文献 DOI)」人工标注表，**所有 DOI 于构造期逐条经 CrossRef API 核验（返回 200 且标题匹配）**；未过核验的条目（YH6/YH9/CaH6/ThH10）以 `enabled=False` 冻结占位，启用前必须补核验；
2. 每行 3DSC 记录按 formula_sc 查表：命中 → 写死格式值 + `manual_annotation` provenance（文献、标注人、日期入 note）；未命中 → 值记 `"未知"`；
3. 实测结果：5,771 条中 **16 条命中**（PdH_x 14 条 ambient、ThH_x 若干、H4Si1 high_pressure），**5,755 条记"未知"**。

**标注示例**（high_pressure 类，格式即契约 §4.3 写死口径）：

```json
{"entity_id": "c04173", "prop": "condition", "value": "high_pressure(~96-120GPa)",
 "provenance": {"source_db": "manual_annotation",
  "source_url": "https://doi.org/10.1103/PhysRevLett.101.077002",
  "source_paper_doi": "10.1103/PhysRevLett.101.077002",
  "note": "压力条件为人工标注（非数据库直取），依据文献：Eremets et al., PRL 101, 2008（SiH4 高压超导，Tc~17 K @ 96–120 GPa）；标注人=E1-数据线，日期=2026-08-06"}}
```

**LaH10 类说明**：LaH10/H3S/YH6 等高压氢化物**不在当前 A 池**（3DSC_MP 5,771 条最高 Tc 132.9 K，真含氢仅 24 行；UCI/JARVIS 锚点亦无——见 §6.4）。标注表已预先冻结 LaH10（`high_pressure(~170GPa)`，核验 DOI `10.1038/s41586-019-1201-8`）与 H3S（`high_pressure(~155GPa)`，`10.1038/nature14964`）两条，E5 造 open case gt 实体入池时即自动命中，无需二次标注。

## 4. "待补"项清单（不伪造，按契约如实留白）

| # | 待补项 | 原因与补齐路径 |
|---|---|---|
| 1 | `binding_affinity` 性质行 | 数据源未锁定（契约 §3.4 注明待 UniProt/PDB/BindingDB 另立项），本阶段不出行 |
| 2 | ZINC 分子的 `filter_props`（mw/logp/tpsa） | 落盘 ZINC 子集按许可只含 ID+SMILES（无描述符列）；硬红线禁止计算补齐 → 置 null；补齐需重拉 Zinc-250K 含属性列版本 |
| 3 | 晶体 `cif` 正文 | 落盘 3DSC_MP.csv 的 cif 列是文件路径引用非 CIF 正文（5,771/5,771 实测）；补齐需重拉 aimat-lab/3DSC 仓库 cif 目录 |
| 4 | 晶体 `atom_count` | 无坐标正文无法计数（契约允许 null，随 3 补齐） |
| 5 | 四个 B 侧文件（search_candidates_B / get_entity_B / get_property_B / check_molecule_exists_B） | 本阶段无造题 gt 集，空文件+MANIFEST 说明占位；随 E5 造题冻结增长（每 case ≥1），不得编造 gt |
| 6 | 5,755 条晶体 `condition`="未知" | 未完成逐条文献标注（红线不默认 ambient）；后续按材料族+文献补标 |
| 7 | ZINC 源文件截断补齐 | 磁盘文件 4.66MB/100,044 行 < MANIFEST 声明 9.34MB/200,394 行，需重拉后增量入库 |

## 5. 体积实测（du -sh，2026-08-06）

| 对象 | 实测 |
|---|---|
| `dag_poc/data/backend/` 总量 | **85 MB**（预算 ≤10 GB，富余两个数量级 ✓） |
| search_candidates/ | 39M |
| get_entity/ | 36M |
| get_property/ | 9.3M |
| literature_search/ | 200K |
| check_molecule_exists/ | 4.0K |
| `src/data_build/_build_state/`（构造中间态：注册表/拒收清单/CrossRef 缓存） | 44M（不入 backend 冻结范围） |

根 MANIFEST：`dag_poc/data/backend/MANIFEST.json`（逐文件 md5/bytes/行数/生成时间，189,363 条记录 / 87,673,249 bytes 冻结）。

## 6. 意外发现（施工期实测，均已留痕）

1. **契约 §4.3 示例的 LaH10 DOI 是错的**：`10.1038/s41586-019-1410-1` 经 CrossRef 核验指向白血病免疫学论文；真实 LaH10 论文为 `10.1038/s41586-019-1201-8`（Nature 572, 2019，"Superconductivity at 250 K in lanthanum hydride under high pressures"，另有 Somayazulu PRL `10.1103/PhysRevLett.122.027001` 佐证）。本构造采用核验后 DOI；**建议契约 v1.1 修订 §4.3 示例**。
2. **ZINC 落盘文件截断**：磁盘实存 4,660,898 bytes / 100,044 数据行，仅为其 MANIFEST 声明（9,340,459 / 200,394）的一半；文件在行边界处干净截断，本次以磁盘实存为准入库（74,901 条仍满足契约下限），补齐需重拉。
3. **ZINC 源文件第 986 行存在孤立引号脏行**：用 csv 模块读取会被引号状态机吞掉后续约 8 万行（首轮构造只读到 20,599 行即此因）；已改逐行字面解析，该行按非法 SMILES 拒收留痕。
4. **落盘 3DSC_MP.csv 的 cif 列是文件路径而非 CIF 正文**（41,283 物理行的真因是 doi_bibtex_2 多行 bibtex）；task18 数据字典「cif 列可直接补 structures」的预期需改为重拉 3DSC 仓库 cif 目录。
5. **知名高压氢化物（LaH10/H3S 等）不在任何已落盘材料库**：3DSC_MP 最高 Tc 132.9 K（Hg 系铜氧化物）、真含氢仅 24 行（PdH/ThH/SiH，最高 17 K）；UCI unique_m.csv 仅 2 行 H2S；JARVIS 实验锚点亦无。open 类高压陷阱题的 gt 实体需 E5 另行冻结入池（标注表已预置）。
6. **3DSC 晶体断言一口径经全量实测校准**：formula_sc（掺杂组分）对 orig_formula_cif（母相超胞）绝对计量不可比，改用元素分数占比重合度 ≥0.6；pretty_formula_2 有 1 行为 'NM'（MP 无约化式标注）跳过子校验——校准后 5,771/5,771 通过。
