# 后端库配置报告 · compare

| 字段 | 值 |
|---|---|
| 工具规范名 | `compare` |
| alias | `compare_materials`（旧名按 tools_schema.json `alias_map` 归一） |
| source_tier | **none（纯计算/收卷类，无查询后端）** |
| 预算 | free（不计预算；录像仍记 compare 行） |
| 后端来源决策 | **无需后端库**（调研报告 §3 决策表行 5、契约 v1.1 §3.6） |
| 支撑 checkpoint | cp4 条件筛选 / cp5 排序 |

## 结论：纯计算/收卷类，无查询后端

`compare` 是对给定实体 ID 列表按指定性质键排序的**纯便利工具**：输入 IDs 必须是 agent 已通过 search_candidates/get_property 触达过的实体，**不引入任何新信息、不触达任何子库**。排序所用的性质值来自 agent 会话内已有的 get_property 查得结果（trace.observed），排序本身是内存中的确定性比较运算，无文件、无字段规格、无数据数量可言（契约 §3.6 一句话结论）。

## 理由

1. **契约依据**：契约 v1.1 §3.6 明写「compare（纯便利排序，输入必须是 agent 已触达实体）……四者均不触达任何子库，无文件、无字段规格」；
2. **机制依据**：若 compare 自带后端直查性质值，将绕过 get_property 的 verify_budget 计费，使 cp3 查证预算形同虚设——无后端正是计费完整性要求；
3. **可省性**：INTERFACE_CONTRACT §3.1 标注本工具可省——省略时 cp5 由 agent 自行排序，判分口径不变。

## 出入参含义（对照 tools_schema.json）

**入参**：`ids`（array[string]，minItems 2，待排序实体 ID）；`key`（string，排序键性质名，须为该批实体已查得的性质）；`ascending`（boolean，默认 false）。
**出参**：`ordered_ids`（array[string]，排序结果）。
**错误码**：`INVALID_ARGUMENT` / `UNKNOWN_ENTITY`。
