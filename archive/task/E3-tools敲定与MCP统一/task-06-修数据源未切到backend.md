# Task E3-06 — 修 PR-45：gateway 数据源切到 E1 的 backend 库

> ## 📌 跑 LLM 必读：中间结果必须完整落盘（owner 要求，2026-08-07）
>
> **凡本 task 涉及调用 LLM，每次调用都必须留完整 JSON log。** 必留字段与两个已实测的坑见 [`ACTION_RULES.md` §7](../../ACTION_RULES.md)：`request`/`response` 完整原样（含 `usage`、`finish_reason`、完整 `message`）+ `latency_ms`/`attempt`/`seq`/`timestamp`/`code_version`；落盘在自己 task 目录下；temperature 一律调 `agent_harness.temperature_for(model)`。

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E3-06 / `task/E3-tools敲定与MCP统一/` |
| **类型** | Bugfix（红线级） |
| **状态** | ✅ **已交付**（commit 链：5d987dd 主体切源（随保护交付物全量提交入库，归属按文件清单记账，PR-39/PR-42 纪律延续）→ ee6a5c5 六项裁定 F1-F6 收尾 → cbcaa15 登记缺口补齐；§3 五条断言实跑 **5/5**（Chris 独立验收，任务 #66）：断言1 视图白名单 29/29 可查 / 断言2 gt z064386 无视图翻绿 / 断言3 视图外同码逐字节 / 断言4 logS -6.1439 逐值一致 / 断言5 176 逐 case 169+19+1；全量回归 **317 passed + 1 skipped + 5 errors**、三红线全过、15 实验点零差异。收口章节见 report.md §13，任务 #67 状态对齐） |
| **负责人** | E3（工具线） |
| **预计起止** | 2026-08-07 ~ （约半天） |
| **上游来源** | `problems.md` **PR-45**（最高） |
| **优先级** | 🔴 **最高** —— 不修则 49 道题一道都跑不了 |

---

## 一句话目标

> agent 现在只能查到 40 个实体（旧 mock_db），而 E1 的库有 92,193 个——把实体存在性判定切到 E1 的 backend。

---

## 0. 你上一轮三项，我逐条实测复验过了，全部通过

你说"如需监工同款独立实测复验随时可派"——我自己做了，结论如下：

| 项 | 我的实测 |
|---|---|
| §10 视图加载 | ✅ gateway 视图提及 **0 → 29 处**；`CASE_VIEW_MISSING` 6 处；**两种 fail-closed 都实跑 raise**（缺 case_id、未注册 case_id），错误信息明写"不得静默回落全库" |
| §10 的 `compare` | ✅ **你的设计比加过滤更彻底**——它只消费 `self.observed`，而 `observed` 唯一写入口是 `get_property`（已被视图闸住），视图外值结构性进不来。而且你区分 `PROPERTY_NOT_VERIFIED` 与 `UNKNOWN_ENTITY` 来防源探测 |
| §9 两个拦截码 | ✅ `DEGRADED_KEY_NOT_UNIQUE`（gateway）+ `PRIMARY_KEY_NOT_UNIQUE`（evaluate）都在 |
| §7 resolve | ✅ 实跑 `evaluate.resolve(z064386 的 formula)` → **17 个实体、含自身**，**与你报的数字完全一致** |
| 测试基线 304 | ✅ **你的数字对**：241（dag_poc）+ 61（eval_matrix）+ 2（mcp e2e，你装了包）；我先前报的 199 是范围窄 |

另外你在 `test_resolve_composition_task05.py` 里写了 `assert 'def resolve(' not in gateway源` —— **把我"§7 归判分侧、不放 gateway"的裁定做成了机器断言**。这个做法值得推广。

**我三处"未找到"都是我的范围问题**（compare 的过滤方式、测试目录范围、resolve 的所在文件），不是你的缺口。

---

## 1. 但我发现了一个更底层的问题（PR-45）

### 1.1 事实

`_tool_get_entity` 的入口条件：

```
if eid in self.db.get('admet', {}):      # self.db = dag_poc/data/mock_db.json
```

而 `mock_db.json` 的 `admet` 只有 **40 条**。E1 已交付的 `data/backend/get_entity/get_entity_A.jsonl` 有 **92,193 条**，但只被用来取 `formula`/`masked_structure`（`self._a_identity`），**不参与"实体是否存在"的判定**。

### 1.2 实测证据

| 测试 | 结果 |
|---|---|
| `get_entity("z000374")`，**无视图** | `UNKNOWN_ENTITY：实体不存在于本源` |
| `z000374` 是否在 `get_entity_A.jsonl` | ✅ **在** |
| `z000374` 是否在 `case_dh_drug_01` 的视图白名单 | ✅ **在** |
| agent 可触达实体占比 | 40 / 92,193 = **0.043%** |
| 现有题目的 gt 能否查到 | **30 道能查到、50 道查不到** |

**"无视图时也查不到"排除了视图因素** —— 这不是 PR-40 的问题，你的视图逻辑是对的（白名单已放行 z000374），问题在视图之下的数据源层。

### 1.3 后果

E5 造的 49 道题（30 fixed + 19 open）引用的实体绝大多数不在那 40 条内 ⇒ **实跑时 agent 查不到题目对象，一道都跑不了**。

E4 的 9 组冒烟之所以能跑通，是因为它用的是 mock_db 里的旧 case 实体。

### 1.4 这又是我漏派的

我派过 E1 建库（goal 步 4）、派过你接 MCP / 接视图 / 接判据，但**从没派过"把 gateway 的数据源从 mock_db 切到 E1 的 backend"**。

E1 的写作区到 `data/backend/` 为止，`gateway.py` 是你的；而我历次给你的任务书只写"接工具、接视图、接判据"，**没写"换数据源"**。这跟 PR-40（视图接线没人负责）是同一类漏派。

---

## 2. 要做的三件

| # | 要求 |
|---|---|
| 1 | **`_tool_get_entity` 的实体存在性判定改以 `backend/get_entity/get_entity_A.jsonl` 为准**；`mock_db` 降为过渡期兼容或彻底退役 |
| 2 | **一并核查 `get_property` / `search_candidates` / `compare` 的取值链路**是否同样卡在 mock_db —— 我只实测了 `get_entity`，其余三个很可能同病 |
| 3 | **视图口径不动**：视图外仍返 `UNKNOWN_ENTITY`（防源探测），只改"视图内且库内存在"这条路径 |

---

## 3. 验收标准（实跑，不接受"代码写了"）

| # | 断言 |
|---|---|
| 1 | 取一道现有 open 题（如 `case_dh_drug_01`）的**视图白名单成员**，`get_entity` **须全部查得到** |
| 2 | 取该题的 **gt**，在**无视图**的 gateway 下 `get_entity` **须查得到** |
| 3 | 视图外实体（如 `z000001` 对 `case_dh_drug_01`）**仍返 `UNKNOWN_ENTITY`** |
| 4 | `get_property` 对白名单成员**能取到值**（不是空） |
| 5 | 现有 80 道 case 的 gt **全部可查**（我实测现在只有 30 道可查） |

---

## 4. 范围与非目标

**不做**：不改 `dag_poc/data/**` 的库内容（E1 的）、不造 case（E5 的）、不改 `query_lint.py`（E2 的）、不改 harness（E4 的）。

**不要把 mock_db 直接删掉** —— E4 的 9 组历史冒烟 trace 依赖它，按 ADR-0014 修订 R1 的 legacy 精神保留可重判能力。

---

## 5. 风险预判

| 风险 | 说明 |
|---|---|
| **切源后旧 case 全挂** | 旧 37 道 case 的实体是 mock_db 口径。若切源后它们查不到，**如实报**——那 37 道本来就要被 E5 的新题替换（318 条 lint 违规、无一合格），不必为它们妥协 |
| **性能** | 92,193 条 × 每次线性扫会很慢，需建索引；但**不要为了性能把存在性判定退回内存小表** |
| **`search_candidates` 的返回量** | 切源后候选池从 40 涨到 9 万，若无视图会返回巨量结果。**视图是主要闸门**，fixed 类若无视图需按 `limit` 截断并在 report 说明口径 |
