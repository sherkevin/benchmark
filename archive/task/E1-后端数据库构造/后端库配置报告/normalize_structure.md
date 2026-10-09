# 后端库配置报告 · normalize_structure

| 字段 | 值 |
|---|---|
| 工具规范名 | `normalize_structure` |
| alias | 无 |
| source_tier | **none（纯计算/收卷类，无查询后端）** |
| 预算 | free |
| 后端来源决策 | **无需后端库**（调研报告 §3 决策表行 9、契约 v1.1 §3.6） |
| 支撑 checkpoint | open 类三件套之一（规范化校验）；等价匿名变换下的同一性判定 |

## 结论：纯计算/收卷类，无查询后端

`normalize_structure` 是把任意结构表示规范化为匿名域唯一形式的**纯规范化计算工具**（RDKit canonicalization 同口径）：性质不变、每个真实分子对应唯一匿名标识；不查询任何数据、不触达任何子库，无文件、无字段规格、无数据数量可言（契约 §3.6 一句话结论）。评分侧复用**同一规范化函数**核对 edit_op(anchor) == 提交分子——该同口径要求正是「纯函数、无状态、无后端」的直接理由（若依赖库状态，评分侧与工具侧口径就可能漂移）。

## 理由

1. **契约依据**：契约 v1.1 §3.6 明写「normalize_structure（RDKit canonicalization 纯计算）……均不触达任何子库」；
2. **机制依据**：本工具是等价匿名变换（ADR-0012 §3）下同一性判定的载体，必须是确定性纯函数——查库会引入状态依赖，破坏「同一输入永远同一输出」；
3. **下游衔接**：`check_molecule_exists_B.jsonl` 的 `canonical_smiles` 列也按本工具同口径规范化后入库，两侧口径由同一函数保证（数据侧规范见契约 §3.5，非运行时查库）。

## 出入参含义（对照 tools_schema.json）

**入参**：`structure`（string，待规范化的结构表示，匿名域 SMILES/式子）。
**出参**：`canonical_id`（规范化后的唯一标识）；`valid`（boolean，结构是否合法；非法结构返 false，INVALID_ARGUMENT 语义由调用方判）。
**错误码**：`INVALID_ARGUMENT`。
