# Task E6-04 — 出裸答污染清单 + 矩阵开跑准备

> ## 📌 跑 LLM 必读：中间结果必须完整落盘（owner 要求，2026-08-07）
>
> **凡本 task 涉及调用 LLM，每次调用都必须留完整 JSON log。** 必留字段见 [`ACTION_RULES.md` §7](../../ACTION_RULES.md)：`request`/`response` 完整原样（含 `usage`、`finish_reason`、完整 `message`）+ `latency_ms`/`attempt`/`seq`/`timestamp`/`code_version`；落盘在自己 task 目录下；temperature 一律调 `agent_harness.temperature_for(model)`。


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
| **Task ID / 路径** | E6-04 / `task/E6-三层矩阵评测/` |
| **类型** | Implementation（判定收口 + 开跑准备） |
| **状态** | 🟡 待开工 |
| **负责人** | E6（矩阵评测线） |
| **预计起止** | 2026-08-08 ~ （约 1 天） |
| **上游来源** | E3-05 §7 resolve 映射已交付 · E5 的 `s_feasible_size` 已落盘 |

---

## 一句话目标

> 三个缺口都解除了，把 1350 次采样的判定跑出来、出污染清单。

---

## 0. 你等的三件都到了

| 你报的缺口 | 现状 |
|---|---|
| ① `G_B` 未 per-case 注册 | ✅ E5 已注册，`b_partition.cases` 20 条，15/15 与 case JSON 一致 |
| ② 组成↔ID 映射缺失 | ✅ E3 交付 `evaluate.resolve`，我实测 `resolve(z064386)` → **17 个实体、一对多成立** |
| ③ fixed 候选池分母待裁定 | ✅ 已裁定取「完整过滤邻域」（ADR-0018 R1-③），E5 已逐题落 `s_feasible_size` |

另外 **PR-45 已闭合**（数据源切到 backend），gt 可查率 30/80 → **169/170**。

---

## 1. 要做的两件

### 1.1 出污染清单（主任务）

| # | 要求 |
|---|---|
| 1 | open 类：任一次命中 `G_B` 即判污染废弃；命中判定**走判分器同一套逻辑**（`resolve(提交组成) ∩ G_B ≠ ∅`），不自写字符串比对 |
| 2 | fixed 类：`p_base = 1/\|S_feasible\|`，废弃条件 `pass@10 > 2·p_base` |
| 3 | 污染清单交 E5 重造，含**命中的是哪一次、答的是什么** |
| 4 | 污染率高**不许调松判据** —— 药物侧 logS 来自 AqSolDB 公开集，模型很可能见过，如实报 |

### 1.2 矩阵开跑准备

题集现在 139/150（E5 正在补最后 11 道）。你的 `matrix_design.md` 定的是 **M=4 × H=2 × 150 题 × k=3 = 3,600 次**、并发 8 路约 75 小时。

**H2 的实际状态要盯一下**：E4 正在做端到端接入（task-05）。它跑通后会明确知会你——**在那之前不要按 H=1 收口消融表**，也不要拿 H1 自比充当消融（你自己写的这条规矩）。

---

## 2. 一件我要请你顺手核的

你的 `bare_baseline.py` 对未注册 case 无 try/except（PR-41，E3 按不动区纪律没改）。现在 `G_B` 都注册了，这条大概不会触发——但**顺手加上防护**，否则将来漏一道注册就会崩在半路。

---

## 3. 预期交付物

| # | 交付物 | 通过标准 |
|---|---|---|
| 1 | open 污染清单 | 逐 case 判定 + 命中留痕；判定走判分器同一逻辑 |
| 2 | fixed 污染清单 | 用 `s_feasible_size` 算 `p_base`；口径写进 report |
| 3 | PR-41 防护 | `bare_baseline.py` 对未注册 case 不再裸崩 |
| 4 | 矩阵开跑前置清单 | 三道门 + H2 状态 + 题集状态，逐项标就绪/未就绪 |
| 5 | report 收口 + 状态行更新 | — |

---

## 7. token 与日志的可追溯要求（owner 2026-08-08 追加，本 task 硬门禁）

> owner 原话：「调用 llm 做测试的时候标明使用的 token 数量，中间的 log json 等，以留可追溯的中间结果和日志，方便后续核查和根据其来改进方案」

留字段（`ACTION_RULES.md` §7 的 11 项）是底线，**本 task 额外要求"可汇总"**——3,600 次跑测的日志若只是散在磁盘上、无法按维度求和，等于没留。

### 7.1 逐次必留（沿用 §7，一项不缺）

`request` / `response` 完整原样（含 `usage`、`finish_reason`、完整 `message`）+ `latency_ms` / `attempt` / `seq` / `timestamp` / `code_version`。

**`usage` 尤其不能丢**：`prompt_tokens` / `completion_tokens` / `total_tokens`，若模型返回 `completion_tokens_details.reasoning_tokens`（推理模型有）或 `prompt_tokens_details.cached_tokens`，**一并留原样**，不要只抄总数。

### 7.2 必须能按维度汇总（新增）

跑完要能回答这些问题，且**答案由脚本从落盘日志算出、不靠手记**：

| # | 要能算出 |
|---|---|
| 1 | 总 token 消耗（prompt / completion / total 分列） |
| 2 | **按模型**分组的 token 与调用次数 |
| 3 | **按 harness**（H1 / H2）分组的 token 与耗时 |
| 4 | **按 template 类型**（type1 / type2 / open）分组 |
| 5 | **单题 token 分布**（中位数、最大值、超长题是哪几道） |
| 6 | 缓存命中情况（`cached_tokens` 占 prompt 的比例） |
| 7 | 重试次数与失败原因分布（按 `attempt` 与错误码） |

**交付一份汇总表 + 生成它的脚本**，脚本要能重跑复现（这是 owner 说的"根据其来改进方案"的前提——数字不可复算就无法据以改进）。

### 7.3 成本口径

按 token 数与各模型单价折算成本，**分模型列出**。DogFooding 档单价为 0，付费档按实际计费口径。

若某模型贵到影响后续排期，**在 report 里直接指出来**——这是排期与降档决策的依据。

### 7.4 落盘结构

日志目录要**按 (模型, harness, case, 重复轮次) 可定位**，不要全塞一个目录。断点续跑靠它判重，事后查 case 靠它定位。

### 7.5 一条反例（我实测踩过）

我用 dogfooding 试跑时，`usage` 与 `finish_reason` 实际在 `call_log[0].response` 里而不是顶层。我连取错两层路径才找对——**落盘时把层级写进 README 或 schema 注释**，别让后来人（包括判分脚本）猜。
