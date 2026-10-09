# Task M-01（监工亲自执行）— 工具接口统一为 MCP 标准形态

> 2026-08-10 · 执行人：监工本人（owner 指令"你亲自去修"）· 一次性技术债清理

---

## 0. 与 goal 的对齐

`goal.md` §6 步 3 要求"把已调研的数据集与 MCP server **合并为同一套调用方式**，敲定 tools 数量"。

**这一步的文档产出早已存在**（`tools_schema.json` 的 `legacy_tool_disposition` 明确写了 23→9 收敛方案），**但实物从未跟上**：mock_server 至今注册 19 个工具（下发给 agent 时展开成 31 个），其中 10 个是方案里明确 `dropped_with_reason` 的。

本任务把"同一套调用方式"真正落地。

---

## 1. 问题（全部经实测，非推断）

### 1.1 两条 harness 拿到的接口不是同一个

| | H1 | H2 |
|---|---|---|
| 工具个数 | 9（`CONTRACT_TOOL_NAMES` 白名单） | **31**（claude CLI 遥测 `mcpToolCount: 31`） |
| 参数 schema | `id`/`props` 独立字段 + required | 只有 `arguments: string` |
| 说明书字数 | 6,241 | **521**（MCP 侧手写 docstring） |
| 实测参数名错误 | **0 次** | **63 次** |

### 1.2 违反项目自己定的方案

`legacy_tool_disposition.dropped_with_reason` 原文写着：

- `echo_text`：**"测试工具，不发布"** —— 实际下发了
- `predict_binding`："硬编码假实现（恒返 -8.5）"
- `generate_molecules`："硬编码假实现（返回非法 SMILES C0CC）"
- `simulate_virtual_cell` / `validate_target`："基因/灵枢领域工具，与药物+材料双领域不匹配"
- `generate_report` / `generate_drug_report`："纯报告类转发工具，判分无关"

### 1.3 说明书写在了错的位置

对照 tau-bench 官方源码（Sierra，`get_user_details`）：

| | tau-bench | 我们 |
|---|---|---|
| 工具级 description | 40 字（一句话说功能） | `get_property` **772 字** |
| 参数级 description | `"The user id, such as 'sara_doe_496'."`（**带示例值**） | `get_property.id` = **空** |

模型生成某个参数值时，锚定的是那个参数自己的 description。我们把 6,241 字堆在工具级、参数级留空——**字数不是问题，位置才是问题**。

### 1.4 open 判分结果被截断（并入本任务，因同属接口契约层）

`evaluate.py` L322 / L333：`return _score_open_discovery(...)[:4]`

该函数 `return scores, gb_set, ranked, best, extra` 共 5 个，`extra` 里装 `open_score` / `hit_gb` / `open_gate`。被 `[:4]` 切掉后，上层脚本按 `len(res)==4` 判成 fixed ⇒ **goal §2b 的 open 判分公式算了但传不出去**。

---

## 2. 格式标准（依据两个独立一手来源）

### 2.1 MCP 官方规范（2025-06-18）

工具定义字段：`name` / `title`（可选）/ `description` / **`inputSchema`** / `outputSchema`（可选）/ `annotations`（可选）。

`inputSchema` 是 JSON Schema：`type: object` + `properties` 逐参数声明 + `required` 列必填。

### 2.2 tau-bench 官方源码

OpenAI function calling 形态：`{"type":"function","function":{name, description, parameters}}`，其中 `parameters` 结构与 MCP 的 `inputSchema` **完全一致**。

### 2.3 结论

两者不是选择题，处在不同层：

- **MCP** 管服务端怎么暴露工具 → 用标准 `inputSchema`
- **function calling** 管怎么告诉模型 → 客户端从 MCP 拿到再转 `parameters`

标准链路：**MCP `inputSchema` → 模型 API `parameters`**，一对一机械转换。这就是"只有一种调用逻辑"的落地形态。

### 2.4 工具数量参照

同期科研类 agent benchmark：AstaBench **8 个**（MCP 暴露检索工具，与我们最像）、InfoMosaic-Bench 77 个、Tool-Decathlon 604 个（办公/运维场景）。**我们 9 个属合理量级。**

---

## 3. 要做的六项

| # | 动作 | 文件 |
|---|---|---|
| 1 | `input` → `inputSchema`（对齐 MCP 标准字段名） | `dag_poc/tools_schema.json` |
| 2 | **每个参数补 description，带示例值**（照 tau-bench）；工具级 description 压到一两句 | 同上 |
| 3 | MCP 侧**参数展开成独立字段**，去掉 `arguments: str` 包壳 | `dag_poc/mcp_mock_server/mock_server.py` |
| 4 | **删 10 个 dropped 工具 + 12 个别名注册**，只留 9 个契约工具 | 同上 |
| 5 | H1 侧跟随字段名改动（`input` → `inputSchema`） | `src/benchmark/agent_harness.py` |
| 6 | **删两处 `[:4]`**，让 open 分支返回完整 5 元组 | `src/benchmark/evaluate.py` |
| 7 | **加一致性断言**：dump H1 与 H2 实际下发的工具定义，逐字段比对必须一致 | 新建测试 |

---

## 4. 明确不做的事（红线）

| 不做 | 原因 |
|---|---|
| **不做任何参数纠错/同义词匹配/意图猜测** | 会污染 trace（trace 是判分唯一依据），且违反"猜字段名不属 benchmark 该考的能力"的既定裁定 |
| 不改 `server.py` | 那是灵枢原版 MCP server，非实跑链路，其工具引用属正常 |
| 不改题（150 道已冻结） | 不在本任务范围 |
| 不改判分给分逻辑 | 只删 `[:4]` 这个截断，不动任何权重与阈值 |
| 不自行开全量矩阵 | 修完交 E6 重跑 3 道验证 |

---

## 5. 验收标准

| # | 断言 | 期望 |
|---|---|---|
| 1 | MCP `list_tools` 返回的工具数 | **9**（不再是 19/31） |
| 2 | `get_property` 的 `inputSchema.properties` | 含 `id`、`props`，各有非空 description 且带示例值 |
| 3 | H1 与 H2 拿到的工具定义 | 工具名集合、参数名、required **逐项相同** |
| 4 | `score_case` 对 dh 题的返回 | **5 个元素**，`extra` 含 `open_score`/`hit_gb` |
| 5 | 回归 | `pytest dag_poc src` 单条命令跑到底，failed 不增加 |
| 6 | `echo_text` 等 10 个 dropped 工具 | 在 `list_tools` 中**不存在** |

---

## 6. 产物落盘位置

- 代码改动：原地
- 验证脚本与实测输出：`task/_监工亲自执行/M-01-工具接口统一/`
- 收口报告：本目录 `report.md`
