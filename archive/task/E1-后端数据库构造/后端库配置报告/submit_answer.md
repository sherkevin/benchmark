# 后端库配置报告 · submit_answer

| 字段 | 值 |
|---|---|
| 工具规范名 | `submit_answer` |
| alias | 无 |
| source_tier | **none（纯计算/收卷类，无查询后端）** |
| 预算 | free（每次尝试落录像行；仅一次成功提交） |
| 后端来源决策 | **无需后端库**（调研报告 §3 决策表行 6、契约 v1.1 §3.6） |
| 支撑 checkpoint | cp6 提交 |

## 结论：纯计算/收卷类，无查询后端

`submit_answer` 是终态**收卷工具**：只做 `dag_poc/answer_schema.json` 的 payload 结构校验与终态落盘，不查询任何数据、不触达任何子库，无文件、无字段规格、无数据数量可言（契约 §3.6 一句话结论）。终态记录复用 tau-bench 式终态落盘思路（调研报告 §3 行 6），但那是录像/终态机制而非查询后端。

## 理由

1. **契约依据**：契约 v1.1 §3.6 明写「submit_answer（收卷，只做 answer_schema.json 校验）……均不触达任何子库」；
2. **职责边界**：值域判断（含 NO_SOLUTION）不在本层，由 cp6 logic 判——本工具只保证「交上来的卷子语法合法」；
3. **三通道收口之一**：API structured output / 本工具 / 本地校验（INTERFACE_CONTRACT §3.2），三者共用同一份 answer_schema.json，均无需查询后端。

## 出入参含义（对照 tools_schema.json）

**入参**：`payload`（object，符合 answer_schema.json 的完整答卷对象：candidates/ranking/answer + open 扩展三字段）。
**出参**：`accepted`（boolean）；`errors`（array[string]，SCHEMA_VIOLATION 时给出字段级错误列表，仅语法错，允许一次重交）。
**错误码**：`SCHEMA_VIOLATION` / `DUPLICATE_SUBMISSION`（终态只允许一次成功提交）。
