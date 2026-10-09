# 后端库配置报告 · get_entity

| 字段 | 值 |
|---|---|
| 工具规范名 | `get_entity` |
| alias | `get_cif_by_formula`、`get_or_generate_cif`、`download_poscar`、`preview_structure_file`、`poscar_to_cif`（5 个旧结构文件类工具收敛为本工具，按 `alias_map` 归一） |
| source_tier | A |
| 预算 | free（不计预算；录像仍记 lookup 行） |
| 后端来源决策 | **自建**（调研报告 §3 决策表行 3：无现成「匿名 ID → 结构身份」后端） |
| 契约依据 | `后端库规格契约.md` v1.1 §3.3、§6.4 |
| 支撑 checkpoint | cp2→cp3 衔接（每个可检索实体必须可取件，否则断链） |

## 1. 数据数量（实测）

| 侧 | 文件 | 记录数 | 体积 |
|---|---|---:|---|
| A 侧 | `dag_poc/data/backend/get_entity/get_entity_A.jsonl` | **80,712**（与 search_candidates_A 同集 1:1 ✓） | 37,797,793 bytes（du 36M） |
| B 侧 | `dag_poc/data/backend/get_entity/get_entity_B.jsonl` | **0**（空占位 + MANIFEST 说明） | 0 B |

构成与 search_candidates_A 完全同集：PubChem 40 + ZINC 74,901 + 3DSC 5,771。入库过门 80,712、拒收 0。

## 2. 功能

按 ID 查询单个实体的结构/身份可见字段（存在性确认），**不返回任何性质数值**。旧结构文件类工具（CIF/POSCAR 取件与预览）收敛为本工具 + get_property（crystal 线性质）；AI 生成类入口（get_or_generate_cif 的 generate 分支等）在 v1 不提供——agent 的候选只能来自检索与推理编辑。完整结构（SMILES/CIF 正文）永不经工具返回，agent 输出只见 masked_structure。

## 3. 调用方式（契约 §6.4 读取接口签名）

```
load:   get_entity/get_entity_A.jsonl → dict[id → record]
get_entity(id) → {id, entity_type, structure{formula, masked_structure, atom_count}}
```

- `id` 不在 A 侧 dict 返 `UNKNOWN_ENTITY`（不区分「不存在」与「不在本源」，防源探测）；
- B 侧文件不在 agent 会话进程加载（评分侧另行持有）。

## 4. 出入参含义（对照 tools_schema.json）

**入参**：

| 参数 | 类型 | 必填 | 含义 |
|---|---|---|---|
| `id` | string | 是 | 实体 ID（search_candidates 返回的匿名 ID） |

**出参**：

| 键 | 类型 | 含义 |
|---|---|---|
| `id` | string | 回显 |
| `entity_type` | string | enum: molecule / crystal |
| `structure.formula` | string\|null | 匿名域化学式 |
| `structure.masked_structure` | string\|null | masked SMILES/CIF 键；掩码规则不可逆（契约前提） |
| `structure.atom_count` | integer\|null | 原子数（晶体线当前待补，见 §7） |

**错误码**：`INVALID_ARGUMENT` / `UNKNOWN_ENTITY`。

## 5. 子库记录字段与 provenance 写法

**A 侧字段**：`id` / `entity_type` / `formula` / `masked_structure` / `atom_count` / `inchikey`（仅评分侧/校验门用，不进 agent 可见输出）/ `cif`（晶体线完整 CIF 文本，agent 不可见）/ `provenance`。

**B 侧字段**（待 E5 冻结）：同 A 侧字段集 + `case_ref`。

实测示例（PubChem 身份档记录节选）：

```json
{"id":"aa","entity_type":"molecule","formula":"C15H25NO3","masked_structure":"FORMULA:C15H25NO3;ATOMS:44",
 "atom_count":44,"inchikey":"IUBSYMUCCVWXPE-UHFFFAOYSA-N","cif":null,
 "provenance":{"source_db":"PubChem","source_url":"https://pubchem.ncbi.nlm.nih.gov/compound/4171",
  "source_paper_doi":"10.1093/nar/gkad987","note":"分子身份取自 PubChem（CID=4171/InChIKey/ConnectivitySMILES），公共领域；匿名 ID 沿用 mock_db"}}
```

## 6. 性能实测（引自 `性能与隔离实测报告.md` §3）

| 指标 | 实测值（get_entity_A.jsonl） |
|---|---|
| 全表扫描 | 0.644 s（80,712 条，3 轮最小值） |
| 点查均值 | 243 ns/次（抽样 200 次） |
| 全载内存峰值（tracemalloc） | 324.9 MB |

## 7. 已知限制

1. **晶体 `cif` 正文当前为 null**：落盘 3DSC_MP.csv 的 cif 列是文件路径引用而非 CIF 正文（5,771/5,771 实测）；补齐需重拉 aimat-lab/3DSC 仓库 cif 目录（待补清单 #3）；
2. **晶体 `atom_count` 为 null**：无坐标正文无法计数（契约允许 null，随 cif 正文一并补齐，待补清单 #4）；
3. masked_structure 掩码规则为自研不可逆掩码（FORMULA+ATOMS 口径），规则细节属构造期实现（`src/data_build/common.py`）；
4. B 侧当前为空占位（gt 实体身份档待 E5 冻结）。
