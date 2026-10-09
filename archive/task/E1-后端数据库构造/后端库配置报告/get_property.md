# 后端库配置报告 · get_property

| 字段 | 值 |
|---|---|
| 工具规范名 | `get_property` |
| alias | `predict_admet`、`crystal_predict`（旧名按 `alias_map` 归一） |
| source_tier | A |
| 预算 | verify_budget（计查证预算；超限 BUDGET_EXCEEDED） |
| 后端来源决策 | **改造**（调研报告 §3 决策表行 4，9 工具中唯一「改造」决策：药物线 ← TDC ADMET 成品数据；材料线自建 ← 3DSC） |
| 契约依据 | `后端库规格契约.md` v1.1 §3.4、§6.4 |
| 支撑 checkpoint | cp3 性质查证、cp4 条件筛选；grounded() 判定的唯一数据源 |

## 1. 数据数量（实测）

| 侧 | 文件 | 记录数（一行一 (实体, 性质) 对） | 体积 |
|---|---|---:|---|
| A 侧 | `dag_poc/data/backend/get_property/get_property_A.jsonl` | **27,618**（实体粒度去重 9,198） | du 9.3M |
| B 侧 | `dag_poc/data/backend/get_property/get_property_B.jsonl` | **0**（空占位 + MANIFEST 说明） | 0 B |

**A 侧分行统计**：

| prop | 行数 | 来源 |
|---|---:|---|
| bbb | 757 | TDC_ADMET:BBB_Martins |
| herg | 104 | TDC_ADMET:hERG |
| caco2 | 235 | TDC_ADMET:Caco2_Wang |
| half_life | 523 | TDC_ADMET:Half_Life_Obach（hr） |
| logD | 604 | TDC_ADMET:Lipophilicity_AstraZeneca（实验 logD7.4，构造期锁定来源） |
| logS | 2,311 | AqSolDB（多源取 Occurrences 高、SD 小者） |
| tc / family / structure_in_db / condition | 5,771 ×4 | 3DSC / 3DSC / cif 列非空推导 / **人工标注** |

药物线命中 3,427 个 A 池分子（按最大片段 canonical SMILES 匹配 TDC/AqSolDB）；未命中实体不出行（契约「缺失即无此行」，禁止填 0/null 冒充真值）。入库过门 27,618、拒收 0。

## 2. 功能

按 ID 查询实体的指定性质，返回快照真值；**返回值即真值**（ACTION_RULES §6），是 cp3 性质查证与 grounded() 判定的唯一数据源。open 类中本工具只查 A 侧——B 侧性质复核仅在评分期由评测机执行，agent 永不可调用（SOURCE_DENIED）。计费顺序写死：入参形状校验（免费）→ 预算检查并计费 → UNKNOWN_ENTITY → UNKNOWN_PROPERTY（实体/性质错误也消耗查证预算，防免费枚举实体存在性）。

## 3. 调用方式（契约 §6.4 读取接口签名）

```
load:   get_property/get_property_A.jsonl → dict[(entity_id, prop) → value]
get_property(id, props[]) → {id, values[{name, value}]}
```

- 索引键为 `(entity_id, prop)` 复合键（全载后建 dict）；
- `id` 不在 A 侧返 `UNKNOWN_ENTITY`；`prop` 不在 case.props 清单返 `UNKNOWN_PROPERTY`；
- B 侧文件不在 agent 会话进程加载；每次调用计 verify_budget。

## 4. 出入参含义（对照 tools_schema.json）

**入参**：

| 参数 | 类型 | 必填 | 含义 |
|---|---|---|---|
| `id` | string | 是 | 实体匿名 ID |
| `props` | array[string]（minItems 1） | 是 | 性质名列表；合法性由引擎按 case.props 校，不合法返 `UNKNOWN_PROPERTY` |

**出参**：

| 键 | 类型 | 含义 |
|---|---|---|
| `id` | string | 回显 |
| `values[]` | array | 每项 `{name, value}`；value 为 number 或 string（布尔用 1/0，类别型如 condition=ambient 用字符串）；**缺失性质无此项**（缺失即无此行） |

**错误码**：`INVALID_ARGUMENT` / `UNKNOWN_ENTITY` / `UNKNOWN_PROPERTY` / `BUDGET_EXCEEDED` / `SOURCE_DENIED`。

## 5. 子库记录字段与 provenance 写法

**记录字段**（A/B 同构）：`entity_id` / `prop`（必须在 `dag_poc/properties/<domain>_v1.json` 清单内）/ `value` / `case_ref`（仅 B 侧必填）/ `provenance`。

实测示例（TDC 药物线，首行记录原文节选）：

```json
{"entity_id":"aa","prop":"bbb","value":1,"case_ref":null,
 "provenance":{"source_db":"TDC_ADMET:BBB_Martins","source_url":"https://dataverse.harvard.edu/api/access/datafile/4259566",
  "source_paper_doi":"arXiv:2102.09548","note":"TDC（NeurIPS D&B 2021）成品实验数据直取；二分类 0/1；按最大片段 canonical SMILES 匹配；B 类 pickle 预测器严禁当真值（未使用）"}}
```

材料线 condition 人工标注写法（契约 §4.3 写死口径）：`source_db="manual_annotation"`，`source_url`/`source_paper_doi` 填标注依据文献 DOI（逐条 CrossRef 核验），note 注明「压力条件为人工标注（非数据库直取）+ 标注人/日期」。

## 6. 性能实测（引自 `性能与隔离实测报告.md` §3）

| 指标 | 实测值（get_property_A.jsonl） |
|---|---|
| 全表扫描 | 0.159 s（27,618 条，3 轮最小值） |
| 点查均值 | 90 ns/次（抽样 200 次） |
| 全载内存峰值（tracemalloc） | 29.6 MB |

## 7. 已知限制

1. **`binding_affinity` 性质行待补**：数据源未锁定（契约 §3.4 注明待 UniProt/PDB/BindingDB 另立项），当前不出行（待补清单 #1）；
2. **5,755 条晶体 `condition` = "未知"**：未完成逐条文献标注（红线：不得从 3DSC 推导、不得默认 ambient）；已命中 16 条（PdH_x 14 条 ambient、H4Si1 high_pressure 等，DOI 逐条 CrossRef 核验）；后续按材料族 + 文献补标（待补清单 #6）；
3. **logD 来源**：构造期锁定为 TDC Lipophilicity_AstraZeneca（实验 logD7.4），与契约 §3.4「待 TDC 拉取后锁定」条款对应，已闭环；
4. B 侧当前为空占位（各 open case gt 实体 × 题面涉及性质全集，待 E5 冻结后由 E1 过门入库）。
