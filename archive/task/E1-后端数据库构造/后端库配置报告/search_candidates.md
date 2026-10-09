# 后端库配置报告 · search_candidates

| 字段 | 值 |
|---|---|
| 工具规范名 | `search_candidates` |
| alias | `search_molecule_database`（旧名按 tools_schema.json `alias_map` 归一） |
| source_tier | A |
| 预算 | search_budget（计检索预算） |
| 后端来源决策 | **自建**，数据源借用 Zinc-250K / 3DSC / PubChem（调研报告 §3 决策表行 2） |
| 契约依据 | `后端库规格契约.md` v1.1 §3.2、§6.3 |
| 支撑 checkpoint | cp2 候选搜集 |

## 1. 数据数量（实测）

| 侧 | 文件 | 记录数 | 体积 |
|---|---|---:|---|
| A 侧 | `dag_poc/data/backend/search_candidates/search_candidates_A.jsonl` | **80,712** | 39,977,332 bytes（du 39M） |
| B 侧 | `dag_poc/data/backend/search_candidates/search_candidates_B.jsonl` | **0**（空占位 + MANIFEST 说明；随 E5 造题冻结增长，每 case ≥1） | 0 B |

**A 池构成**（80,712 ＝ 40 ＋ 74,901 ＋ 5,771）：

- PubChem 40 分子身份档：40（ID 沿用 mock_db aa..ej）；
- ZINC 药物分子池：74,901（磁盘实存 100,044 行 → 唯一 SMILES 86,161 → 非法拒收 232 → canonical 去重 11,028）；
- 3DSC 材料池：5,771（全量过门，0 拒收）。

契约下限：药物线 ≥10,000 ✓、材料线 5,771（3DSC 全量）✓。入库拒收 232 条，全部为断言一「SMILES 非法」（ZINC 源金属配合物/脏行），清单落盘 `src/data_build/_build_state/rejections_search_candidates.jsonl`。

## 2. 功能

按约束/关键词检索候选实体（分子或材料），返回实体 ID + 结构类可见字段，支撑 cp2。**返回字段严格只有 id / entity_type / structure_hint 三键，不含任何性质数值**（性质一律走计费的 get_property，否则 cp3 无意义）；无命中返 `EMPTY_RESULT`，**禁止 fallback 全库**。

## 3. 调用方式（契约 §6.3 读取接口签名）

```
load:   search_candidates/search_candidates_A.jsonl → list[record]（保序）+ dict[id → record]
query(constraints: [{property, op, threshold}], keywords: str|None, k: int) → {returned, candidates[{id, entity_type, structure_hint}]}
```

- 约束在 `filter_props` 上比较（op ∈ lt/le/gt/ge/eq/ne，口径由 property_spec 定）；过滤为线性扫（80,712 行全扫亚秒级，实测 §6）；
- 分页：入参 `offset` + 出参 `total_found` 配合穷举（k 硬上限 50，A 全集超 50 需分页）；
- 每次调用计 search_budget。

## 4. 出入参含义（对照 tools_schema.json）

**入参**：

| 参数 | 类型 | 必填 | 含义 |
|---|---|---|---|
| `constraints` | array[{property, op, threshold}] | 语义必填（与 keywords 至少给一） | 结构化约束；property 只允许白名单内可查性质，白名单外返 `UNKNOWN_PROPERTY`；op ∈ lt/le/gt/ge/eq/ne |
| `keywords` | string\|null | 同上 | 自然语言关键词 |
| `k` | integer（1–50，默认 10） | 否 | 单页返回上限 |
| `offset` | integer（≥0，默认 0） | 否 | 分页偏移 |

**出参**：

| 键 | 类型 | 含义 |
|---|---|---|
| `returned` | integer | 本页返回数 |
| `candidates[]` | array（maxItems 50） | 每项 `{id, entity_type, structure_hint}`；id 为匿名实体 ID（唯一可被后续工具引用的键），structure_hint 为 masked 结构提示（掩码不可逆） |
| `total_found` | integer | 命中总数（配合 offset 判是否取尽） |
| `offset` | integer | 本页偏移回显 |

**错误码**：`INVALID_ARGUMENT` / `UNKNOWN_PROPERTY` / `EMPTY_RESULT` / `BUDGET_EXCEEDED` / `SOURCE_DENIED`（保留未启用）。

## 5. 子库记录字段与 provenance 写法

**A 侧字段**：`id` / `entity_type`（molecule\|crystal）/ `structure_hint` / `smiles`（仅约束过滤用，不进工具输出）/ `formula` / `filter_props`（只驱动 constraints 过滤，永不出现在返回中）/ `provenance`。

**B 侧字段**（待 E5 冻结）：`id` / `entity_type` / `smiles`（或 `formula`）/ `case_ref` / `provenance`——不含任何性质字段。

实测示例（PubChem 身份档记录节选）：

```json
{"id":"aa","entity_type":"molecule","structure_hint":"FORMULA:C15H25NO3;ATOMS:44",
 "smiles":"COCCc1ccc(OCC(O)CNC(C)C)cc1","formula":"C15H25NO3","filter_props":{"mw":267.36,"logp":null,"tpsa":null},
 "provenance":{"source_db":"PubChem","source_url":"https://pubchem.ncbi.nlm.nih.gov/compound/4171",
  "source_paper_doi":"10.1093/nar/gkad987","note":"分子身份取自 PubChem（CID=4171/InChIKey/ConnectivitySMILES），公共领域；匿名 ID 沿用 mock_db"}}
```

## 6. 性能实测（引自 `性能与隔离实测报告.md` §3）

| 指标 | 实测值（search_candidates_A.jsonl） |
|---|---|
| 全表扫描 | 0.755 s（80,712 条，3 轮最小值） |
| 点查均值 | 253 ns/次（抽样 200 次） |
| 全载内存峰值（tracemalloc） | 377.2 MB |

## 7. 已知限制

1. **ZINC 分子的 filter_props 大部分为 null**：落盘 ZINC 子集按许可只含 ID+SMILES（无 mw/logp/tpsa 描述符列）；硬红线禁止计算补齐，补齐需重拉 Zinc-250K 含属性列版本（待补清单 #2）；
2. **ZINC 源文件截断**：磁盘实存 100,044 行仅为其 MANIFEST 声明 200,394 行的一半，补齐需重拉后增量入库（待补清单 #7）；
3. B 侧当前为空占位（open case gt 实体待 E5 冻结），隔离断言已按一般形式实现，gt 入库后自动生效；
4. 约束过滤为全表线性扫，无二级索引（实测亚秒，暂无升索引必要）。
