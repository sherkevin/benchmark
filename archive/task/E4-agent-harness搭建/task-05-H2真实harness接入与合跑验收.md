# Task E4-05 — H2 真实 harness 端到端接入（补三层矩阵缺的那一层）

> ## 📌 跑 LLM 必读：中间结果必须完整落盘（owner 要求，2026-08-07）
>
> **凡本 task 涉及调用 LLM，每次调用都必须留完整 JSON log。** 必留字段与两个已实测的坑见 [`ACTION_RULES.md` §7](../../ACTION_RULES.md)：`request`/`response` 完整原样（含 `usage`、`finish_reason`、完整 `message`）+ `latency_ms`/`attempt`/`seq`/`timestamp`/`code_version`；落盘在自己 task 目录下；temperature 一律调 `agent_harness.temperature_for(model)`。


> ## 🟢 debug 阶段用免费模型（owner 指定 2026-08-08，见 [ADR-0019](../../governance/adr/0019-debug阶段用dogfooding免费模型.md)）
>
> **做逻辑 debug 与联调时一律用 DogFooding 免费模型；只有正式跑实验矩阵时才换回付费模型。**
>
> - 开关：`.env` 里已置 `LLM_PROFILE=dogfooding`，`agent_harness` 会自动整组替换 key/model/endpoint，**你的调用代码不用改**
> - 模型名：**`Peach-07-17-DogFooding`**（必须精确，带 `[1M]` 或小写都报 `CE-001 模型不存在`）
> - 已下架：`Qwen3.7-Max-DogFooding`（`CE-012`），不要再用
> - 老 key 对本模型无权（`PL-001 平台业务限制`），必须用 `DOGFOODING_API_KEY`
> - 这是**推理模型**，真题跑测超 120s，超时已改为配置驱动（`LLM_REQUEST_TIMEOUT`，dogfooding 档默认 600s）
> - 跑 MCP server 与端到端要用 `./.venv/bin/python`（系统 python3.9 装不了 mcp）；可执行入口是 **`mock_server.py`**，不是 `server.py`（后者缺 `src.context`，跑不起来）

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E4-05 / `task/E4-agent-harness搭建/` |
| **类型** | Implementation（矩阵维度补齐）+ 合跑验收 |
| **状态** | ✅ 已收口（2026-08-08，见 report-task-05.md） |
| **负责人** | E4（harness 线） |
| **预计起止** | 2026-08-08 ~ （约 1～2 天） |
| **上游来源** | `governance/task-drift修正-20260808.md` · `goal.md` §6.1 · E6 `matrix_design.md` §2 |
| **优先级** | 🔴 **最高** —— 缺它，「三层矩阵」只剩两层 |

---

## 一句话目标

> 两个 CLI adapter 已经落盘了，把 H2 端到端跑通，让 harness 这一维的对比真的能做。

---

## 0. 为什么现在派：这是我第二次同类漂移

`goal.md` §6 步 6 要的是「模型 × **harness** × template **三层**矩阵」，§6.1 写明「先搭 LangGraph 薄层，后续再上 Claude Code / Hermes 等真实 harness」。

E6 据此定 **H=2**，并实测标注：

| 因子 | 状态 |
|---|---|
| H1 薄壳 `AgentHarness` | ✅ 就绪，三模型 trace 已产出 |
| **H2 真实 harness（商用 CLI）** | ⏳ **两个 adapter 已落盘，从未端到端跑通** |

**而你的 task-02/03/04 全部已收口，没有任何 task 在做 H2。**

E6 的设计里已写降级路径：H2 不就绪则降 H=1、消融表标"不适用"、**且不得拿 H1 自比充当消融** ⇒ **三层矩阵会只剩两层**。

**这是我的失误**：H2 一直挂着"二期目标"的标签，我把"二期"当成了"以后再说"，没算它是不是在关键路径上。跟上次 fixed 缺 90 道是同一个模式（详见 drift 记录）。**"二期"不等于"可以不做"——它是步 6 三个因子之一。**

---

## 1. H2 要做到什么程度

**不要求做多个真实 harness，一个跑通即可** —— H=2 的意思是"有两种 harness 可比"，不是"第二种要做多完善"。

| # | 要求 |
|---|---|
| 1 | 选**一个**商用 CLI（`cli_trace_adapter.py` 或 `qodercli_trace_adapter.py` 对应的那个，哪个手上能跑就用哪个） |
| 2 | 端到端跑通：**读 case → 该 CLI 执行 → 原始输出经 adapter 转换 → 落 trace → 判分器能吃** |
| 3 | 至少跑 **1 道 fixed + 1 道 open**，两类都验（open 要带 `case_id` 走视图） |
| 4 | 产出的 trace 与 H1 的**共用同一套契约**（你的 `harness接口契约.md` §一/§二已定，硬条款是"必须经 adapter 转换后再投喂判分"） |

### 1.1 一条硬边界

你的契约 §60 自己写了：「二期接入真实 harness 时，其原始输出**必须**经 `cli_trace_adapter.py` 转换后再投喂判分与评估」。

**照这条做，不要为了跑通而让 CLI 的原始输出直连判分** —— 那样 H1/H2 的 trace 格式就不可比，harness 消融的前提（同契约、同判分）会失守。

---

## 2. 顺带做完 PR-40 的合跑终验收

E3 的 gateway 半边（§10 视图加载）和数据源切换（PR-45）都已交付并通过我的复验：

| 已就位 | 实测 |
|---|---|
| 视图加载 + fail-closed | `CASE_VIEW_MISSING` 两种情况都 raise |
| 数据源切到 backend | `get_entity` 白名单 **29/29 可查**、视图外仍 `UNKNOWN_ENTITY`、gt 可查率 **169/170** |
| 三通道接缝 | env `DAGPOC_CASE_ID` / 头 / 参数 + `DAGPOC_CASE_KIND`（E3 已登记为合跑前置） |

**你的 harness 半边也已完成**，所以现在可以合跑了。PR-40 的终验收断言：

> 实跑一道 open 题，断言 agent 的查询返回集 ⊆ 该 case 白名单，且 gt 不可达。

跑完把实跑输出贴进 report。

---

## 3. 范围与非目标

**不做**：不改 `gateway.py`/`evaluate.py`（E3 的）、不造 case（E5 的）、不跑正式三层矩阵（E6 的）、不改 `query_lint.py`（E2 的）。

**不做多个真实 harness** —— 一个跑通就够，H=2 已满足。

---

## 4. 预期交付物

| # | 交付物 | 通过标准 |
|---|---|---|
| 1 | H2 端到端跑通 | ≥1 道 fixed + ≥1 道 open；trace 经 adapter 转换、判分器能吃 |
| 2 | H1/H2 契约一致性 | 两者 trace 走同一套契约，**有对比说明** |
| 3 | PR-40 合跑终验收 | 查询返回集 ⊆ 白名单、gt 不可达，**贴实跑输出** |
| 4 | 知会 E6 | H2 可用后明确告知，它的矩阵才能按 H=2 开跑 |
| 5 | report 收口 + 状态行更新 | — |

---

## 5. 风险预判

| 风险 | 说明 |
|---|---|
| **CLI 的工具调用格式与 MCP 不同** | 商用 CLI 可能有自己的工具协议。若无法接我们的 9 个 MCP 工具，**如实报**——那说明 H2 需要额外的工具桥，是新工作量，不要硬凑 |
| **adapter 转换有信息损失** | 转换后若丢了 `usage`/`finish_reason`/时序，判分与指标会缺项。转换前后**逐字段对账一次** |
| **H2 单次耗时可能远高于 H1** | E6 的规划值是 600 秒/次；若 H2 明显更慢，**报实测值**给 E6 重算矩阵耗时，不要让它按 H1 的数字排期 |
