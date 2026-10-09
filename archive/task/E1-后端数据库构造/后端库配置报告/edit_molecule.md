# 后端库配置报告 · edit_molecule

| 字段 | 值 |
|---|---|
| 工具规范名 | `edit_molecule` |
| alias | 无 |
| source_tier | **none（纯计算/收卷类，无查询后端）** |
| 预算 | free（录像仍记行，供 grounding 回放） |
| 后端来源决策 | **无需后端库**（调研报告 §3 决策表行 8、契约 v1.1 §3.6） |
| 支撑 checkpoint | open 类三件套之一（结构编辑）；答卷 edit_op 字段的工具侧对应物 |

## 结论：纯计算/收卷类，无查询后端

`edit_molecule` 是对 A 侧锚点分子施加封闭算子集内单步结构编辑的**纯结构计算工具**（RDKit 封闭算子集）：不访问 A/B 任何库、不返回任何性质——产物性质 agent 依旧不可知。anchor ∈ A 的前置校验（`ANCHOR_NOT_IN_A`）基于会话内已触达实体集合（录像可回放），不是对后端的查询。产物合法性校验可参考 PoseBusters 规则 config（调研报告借用组件，属评分侧参照而非查询后端）。无文件、无字段规格、无数据数量可言（契约 §3.6 一句话结论）。

## 理由

1. **契约依据**：契约 v1.1 §3.6 明写「edit_molecule（RDKit 封闭算子集纯计算）……均不触达任何子库」；
2. **红线依据**：task.md §8「不真算性质」——本工具只做结构变换不算性质，与红线一致；若挂后端反而引入「编辑产物带性质」的泄漏面；
3. **判分衔接**：grounding 条件③（edit_op(anchor) 规范化后 == 提交分子）用本工具录像 + normalize_structure 复核，全程不查库。

## 出入参含义（对照 tools_schema.json）

**入参**：`anchor_id`（string，锚点分子 ID，必须 ∈ A，否则 `ANCHOR_NOT_IN_A`）；`operator`（enum：add_atom / remove_atom / replace_substituent / substitute_element / ring_closure，封闭算子集，超出返 `OP_OUT_OF_SET`，配合跳数 ≤2 约束）；`detail`（string\|null，算子参数）。
**出参**：`result_id`（编辑产物匿名域规范化标识，可直接作为 check_molecule_exists 入参）；`normalized_structure`（与 normalize_structure 同口径）；`applied`（boolean，结构上不可施加时 false，不算错误码）。
**错误码**：`INVALID_ARGUMENT` / `ANCHOR_NOT_IN_A` / `OP_OUT_OF_SET`。
