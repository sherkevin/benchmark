# Task E1-07 — 把结合亲和力/靶点数据 mock 化入库（M-02 串行第一步）

> 2026-08-11 派发 · 监工 · **M-02 四条串行线的第一步：数据。工具/题/判分都等你**

---

## 0. 与 goal 的对齐

M-02 把生物医药从"药物+材料"扩到含**药物-靶点相互作用**（owner 定），要新增 3 个查询类工具（`predict_binding` / `validate_target` + 桩类）。goal §6 步 4 的红线是"离线 mock 库、每个工具背后有真值表"。

**本任务是全链第一步**：没有你的 backend，E3 的工具就是空壳、E5 的题没处可查、判分没真值可对。你做完并验证过，我才派 E3。

---

## 1. 背景：为什么现在做这个

owner 决定把灵枢 23 个工具去重后全量纳入。其中 `predict_binding`（结合亲和力）、`validate_target`（靶点验证）**过去是硬编码假实现**（`predict_binding` 恒返 -8.5）——这正是 benchmark 的大忌（工具宣称能算、实际是假值）。

好消息：**你（E1）之前已经拉过真实数据源**，我实测确认在场：

| 源 | 实测体量 | 路径 |
|---|---|---|
| BindingDB | **239,940 行**结合记录 | `dag_poc/data/sources/BindingDB/ligands_*.tsv`（按 UniProt 靶点分文件） |
| PDB | 蛋白结构 JSON | `dag_poc/data/sources/PDB/*.json` |
| UniProt | 靶点条目 JSON | `dag_poc/data/sources/UniProt/*.json` |

所以这不是从零拉数据，是**把已有源 mock 化成 backend 真值表**。

---

## 2. 要做的三件

### 2.1 `predict_binding` backend（结合亲和力，真数据）

- 从 BindingDB 的 `ligands_<靶点>.tsv` 构造 `(靶点 ID, 配体 ID) → 结合亲和力` 真值表
- 落盘格式与现有 backend 一致（参照 `get_property/get_property_A.jsonl` 的一行一记录结构）
- **配体分子必须走现有匿名 ID 体系**（`z`+6 位），与现有 92,193 实体对齐——不能引入新的 ID 命名，否则判分反查会断
- 落盘位置：`dag_poc/data/backend/predict_binding/`，配 MANIFEST（同现有源）

### 2.2 `validate_target` backend（靶点验证，真数据）

- 从 PDB + UniProt 构造靶点真值表：靶点身份、结构可见字段、druggability（若源里有）
- 靶点用什么 ID：**看 BindingDB 用的是 UniProt accession（如 P07550）**，靶点 ID 体系要和 `predict_binding` 对齐，否则两个工具的靶点对不上
- 落盘位置：`dag_poc/data/backend/validate_target/`

### 2.3 桩类工具的占位数据（必须标记）

`simulate_virtual_cell` / `generate_molecules` / `crystal_generate` —— **无真数据，做 mock 桩**：

- 返回结构合法的占位值（字段齐全、类型对）
- **每条返回值必须带 `"data_provenance": "MOCK_STUB"`** —— 这是判分不采信桩值的依据
- 桩逻辑要确定性（同输入同输出），不要随机

---

## 3. 明确不做的事（红线）

| 不做 | 原因 |
|---|---|
| **不给桩工具编真值** | 桩必须标 `MOCK_STUB`。重演 `predict_binding` 恒返 -8.5 的假实现坑 |
| **不引入新 ID 命名体系** | 配体/分子走现有 `z`+6 位匿名 ID，靶点走 UniProt accession；乱起 ID 会让判分反查断链 |
| 不动现有 backend（get_property 等 9 个） | 只新增，不改现役 |
| 不接工具代码 | 那是 E3 的活。你只造 backend 数据 + 定字段契约 |
| 不碰题、不碰判分 | 串行下游，不越界 |

---

## 4. 交付物

| # | 交付物 | 验收口径 |
|---|---|---|
| 1 | `predict_binding` backend + MANIFEST | 对 BindingDB 内真实 (靶点,配体) 对能返回亲和力，数值可溯源到原始 tsv |
| 2 | `validate_target` backend + MANIFEST | 对库内靶点返身份/结构真值 |
| 3 | 3 个桩工具的占位数据规格 | 返回带 `MOCK_STUB` 标记、字段合法、确定性 |
| 4 | **字段契约说明**（给 E3 接工具用） | 每个新工具的入参名、出参 key、ID 体系，写清楚 |
| 5 | report | 含各 backend 条数、ID 体系对齐说明、桩标记说明 |

---

## 5. 验收方式

我会独立实测：
- 抽 BindingDB 里一条真实结合对，看 `predict_binding` backend 能否查到、数值对不对
- 检查配体 ID 是否与现有 92,193 实体体系一致（不一致则判分会断）
- 桩工具返回值必须能被程序识别出 `MOCK_STUB`

**做完交我复验，我验过再派 E3 接工具**——串行，不要自己往下做工具/题。

---

## 6. token 与日志留痕

本任务是数据处理，一般不涉 LLM。若涉及则按 ACTION_RULES §7.5 落七项；纯数据处理则 report 里写明"无 LLM 调用"，不编数字。
