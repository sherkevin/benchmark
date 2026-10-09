# Task E4-04 — harness 侧接通 per-case 视图（PR-40 一半）+ task-03 收口

> ## 📌 跑 LLM 必读：中间结果必须完整落盘（owner 要求，2026-08-07）
>
> **凡本 task 涉及调用 LLM，每次调用都必须留完整 JSON log。** 必留字段与两个已实测的坑见 [`ACTION_RULES.md` §7](../../ACTION_RULES.md)：`request`/`response` 完整原样（含 `usage`、`finish_reason`、完整 `message`）+ `latency_ms`/`attempt`/`seq`/`timestamp`/`code_version`；落盘在自己 task 目录下；temperature 一律调 `agent_harness.temperature_for(model)`。

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E4-04 / `task/E4-agent-harness搭建/` |
| **类型** | Bugfix（红线级）+ 收口 |
| **状态** | ✅ 已收口（2026-08-07；harness 半边完成 + 断言脚本落盘实跑；合跑待 E3 §10 与视图文件到位，见 report-task-04.md；task-03 状态行已一并修复） |
| **负责人** | E4（harness 线） |
| **预计起止** | 2026-08-07 ~ （约半天） |
| **上游来源** | `problems.md` **PR-40**（最高）——本 task 做其中的 harness 一半，gateway 一半归 E3 |
| **优先级** | 🔴 最高 —— 不修则 open 题在实跑时直接退化为检索题 |

---

## 一句话目标

> 让 harness 把 `case_id` 传进 gateway 会话，使 agent 的查询真正走该 case 的可见集视图，而不是全库。

---

## 0. task-03 已完成，但状态行滞后

我实测：`traces_new/` **27 项**、`dag_poc/traces` 的 `git status` **零改动**、progress 提及本 task **10 处** ⇒ **task-03 实际已完成**，但它的状态行还标着"🟡 待开工"。

**这是本项目第六次状态行滞后**。我已改自己的核查方式（以产出文件时间戳与内容为准、不采信状态行），但请你收口时把状态行一并更新——否则我无法判断该不该派下一批。

另外你上轮报的 `evaluate.py` 只认旧工具名（PR-34）**已由 E3 修好并通过我的复验**：新契约 trace 实跑抽到证据 4/6 槽位（`mol_ids=26`），修复前是全 0。**你报了但不越位改，这个做法是对的。**

---

## 1. PR-40：视图机制做好了，但实跑链路没接（主任务）

### 1.1 事实

E1-05 已交付 per-case 视图机制（`src/data_build/per_case_view.py`，A1–A8 断言全绿）。但我实测：

| 文件 | 对视图的提及 |
|---|---|
| `src/benchmark/agent_harness.py` | **0 处** |
| `dag_poc/gateway.py` | **0 处** |

⇒ agent 经 MCP 工具查询时拿到的是**全库**，不是该 case 的白名单。

### 1.2 后果（为什么定最高优先）

| # | 后果 |
|---|---|
| 1 | **agent 可能直接查到 gt** ——open 题的设计前提是"gt 在 A 里不存在"，全库可见时同族外推的目标就在眼前 ⇒ **open 题退化为检索题** |
| 2 | E5 精心配置的梯度三参数全部失效 —— 干扰项 `K=25` 在实跑时变成全库 8 万条 |
| 3 | `goal.md` §2b② 的「A 的任何查询不得带出 B 内容」在**运行期**失守 |

**为什么此前没暴露**：E1 的 A1–A8 断言验的是"视图文件本身正确"，你的冒烟跑的是 fixed 题（fixed 无 A/B 隔离需求），**两边都没错，是中间这段接线没人负责**——我派 E1-05 时写"视图对所有 A 侧工具生效"，但 E1 的写作区只到 `src/data_build/`。**这段是我漏派的。**

### 1.3 你要做的（harness 一半）

| # | 要求 |
|---|---|
| 1 | harness 建立 gateway 会话时**把当前 case 的 `case_id` 传进去** |
| 2 | `case_id` 从 case JSON 读，不要另建映射 |
| 3 | **open 类 case 若拿不到 `case_id` 应报错**，不得用默认值或跳过——静默降级会让隔离失守而无人知 |
| 4 | fixed 类保持现状（无 A/B 隔离需求），但接口要统一，别写两条路 |

**gateway 侧的加载逻辑归 E3**（已派 E3-05 §10）。你们的接缝是「`case_id` 怎么传」，请与 E3 对齐参数名后各自实现。

---

## 2. 验收要跑的那一条实测

PR-40 的验收标准是：**实跑一道 open 题，断言 agent 的查询返回集 ⊆ 该 case 白名单，且 gt 不可达。**

这条需要你和 E3 都做完才能跑。你先把 harness 一半做好，**E3 的 gateway 一半到位后立刻合跑一次**，把结果写进 report。

---

## 3. 范围与非目标

**不做**：**不改 `dag_poc/gateway.py`**（E3 的，PR-40 另一半）、不改 `evaluate.py`（E3 的）、不建视图文件（E5 按 E1 格式写）、不改 `dag_poc/data/**`（E1 的）、不造 case（E5 的）。

---

## 4. 预期交付物

| # | 交付物 | 通过标准 |
|---|---|---|
| 1 | harness 传 `case_id` | 从 case JSON 读；open 类缺失时报错不静默 |
| 2 | 与 E3 的参数名对齐 | 在 report 写清接缝约定 |
| 3 | 合跑实测（待 E3 到位） | agent 查询返回集 ⊆ 白名单、gt 不可达，**贴实跑输出** |
| 4 | task-03 收口 + **状态行更新** | 第六次滞后，这次务必一并改 |

---

## 5. 一条来自 E3 的知会（照办即可）

E3 已把 `answer_schema` 的 operator 枚举收窄为组成算子（0.2.0 → 0.3.0）。它明确知会：**冒烟期 prompt 不要引导 agent 用旧三算子**（`replace_substituent` / `substitute_element` / `ring_closure`）**作 grounding 编辑** —— 那三个已从枚举移除，用了会判不过。
