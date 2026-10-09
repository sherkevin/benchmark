# M-01 收口报告 — 工具接口统一为 MCP 标准形态

> 2026-08-10 · 执行人：监工本人 · 提交链 `68f7852`（立项）→ `ac8bf92`（收口）

---

## 一句话结论

**七项改动我亲自做完并逐项实测：核心 6 项全绿，第 7 项一致性断言经可证伪验证。** 唯一遗留：一个基于旧契约的测试文件需对齐，已交接 E3。

---

## 做了什么（对照 task §3 七项）

| # | 动作 | 实测结果 |
|---|---|---|
| 1 | `input` → `inputSchema`（MCP 标准字段名） | 9/9 工具 ✅ |
| 2 | 参数级 description 补示例值（照 tau-bench） | 19 个参数全非空、全带示例；新填 4 个空白含 `get_property.id`（原为空，正是 42 次错误直接原因）✅ |
| 3 | MCP 侧参数展开，去 `arguments:str` 包壳 | `get_property` 现暴露 `id`/`props` 独立字段 + required ✅ |
| 4 | 删 10 个 dropped + 12 个别名注册 | **`list_tools` 31 → 9** ✅ |
| 5 | H1 跟随字段名改动 | H1 读到 9 工具、`properties=[id,props]`、required 正确 ✅ |
| 6 | 删两处 `[:4]` 截断 | dh 题返 **5 元**、`extra` 含 `hit_gb`+grounding 四子判据；fixed 题仍 4 元不变 ✅ |
| 7 | 加暴露面一致性断言 | 可证伪验证通过（见下）✅ |

## 关键前后对比

| | 改前 | 改后 |
|---|---|---|
| MCP `list_tools` 工具数 | 31 | **9** |
| `get_property` MCP 侧 description | 一句话（521 字/9 工具） | **772 字**（从真源读） |
| `get_property.id` 参数 description | **空** | 带示例 `"z002730"` |
| 参数形态 | `arguments: string` 包壳 | 独立字段 `id`/`props` + required |
| `echo_text` 等 dropped 工具 | 下发给 agent | 不在 `list_tools` |
| open 判分 `extra`（`open_score`/`hit_gb`） | 被 `[:4]` 切掉 | 完整返回 |

## 唯一真源保证

工具级 + 参数级 description **全部从 `tools_schema.json` 读取**（`_tdesc` / `_pdesc`）。H1 走 `_load_contract_input_schemas` 读同一文件，H2 走 MCP `list_tools`（description 也从该文件注入）。**同一份真源同时供两条 harness，从机制上杜绝再次漂移**——这正是 E3 上轮补 6,241 字说明书却对 H2 无效的根因。

## 第 7 项断言的可证伪验证（断言本身要被验证过才算交付）

| 注入的破窗 | 断言反应 |
|---|---|
| 多注册一个 `_intruder` 工具 | ✅ 报「MCP 暴露面与契约不符：多出 `['_intruder']`」 |
| 把 `get_property` 做回 `arguments` 包壳 | ✅ 报「仍是 arguments 包壳形态——参数须展开为独立字段」 |

---

## 红线遵守

| 红线 | 状态 |
|---|---|
| 不做参数纠错/同义词匹配/意图猜测 | ✅ 未加任何纠错；旧包壳调用直接 TypeError 报废 |
| 不改 `server.py`（灵枢原版） | ✅ 未动 |
| 不改题 | ✅ 未动 |
| 不改判分给分逻辑 | ✅ 只删 `[:4]` 截断，未动任何权重/阈值 |
| 不自行开全量矩阵 | ✅ 未跑 |

---

## 遗留（交接 E3）

**`dag_poc/mcp_mock_server/tests/test_mock_server_contract_tools.py` 需对齐新契约。**

该文件 377 行、20+ 测试，通篇建立在旧契约上：

- `test_ast_decorator_count_is_31` — 断言"注册面应为 31"，现应为 9
- `test_smoke_*`（9 个）— 用 `fns['get_property']('{json字符串}')` 包壳形态调用，现应用展开参数 `get_property(id=..., props=...)`
- `test_deprecated_layer_smoke_all_22` 等 — 测 deprecated 层的 22 个工具，现已不注册

**为什么不我改**：这个文件有大量历史锚点（AST 行号切片、deprecated 层快照），是 E3 一线的资产，E3 更熟它的历史。我改核心代码，测试对齐归 E3 —— 避免我不熟历史而误伤。

**交接要点**：核心功能我已验证完好（9 工具新签名全部返真值），测试失败纯属"断言的是旧的错误状态"，不是功能坏。E3 只需把测试的期望值从旧契约更新到新契约。

---

## 对论文的价值

修完后 H1 vs H2 才第一次成为有效消融：

- 改前差异 = 我们给两边的接口不同（9 vs 31 工具、有无 schema），无效
- 改后差异 = 两个 agent 框架的真实差异，有效

可写进论文的观察：**同一模型、同一批题，仅因工具 schema 声明方式（参数展开 vs 字符串包壳、说明书位置），工具调用成功率就差出一截**——benchmark 结果对接口声明方式高度敏感，此前少有工作报告。
