# Task E4-03 — trace 落盘防护 + 冒烟证据固化

> ## 📌 跑 LLM 必读：中间结果必须完整落盘（owner 要求，2026-08-07）
>
> **凡本 task 涉及调用 LLM（跑 agent、跑基线、批量生成），每次调用都必须留完整 JSON log。**
> 后续算指标、查 case 全依赖它——**没留痕就只能重跑，代价极高**。
>
> 必留字段与两个已实测的坑见 [`ACTION_RULES.md` §7](../../ACTION_RULES.md)。要点：
> - `request` 完整原样（model/temperature/max_tokens/messages/tools），**不能只存解析后的结果**；
> - `response` 完整原样，尤其 **`usage`（token 用量）**、**`finish_reason`（区分答完还是被截断）**、**完整 `message` 对象**；
> - `latency_ms`、`attempt`（含失败与重试）、`seq`/`timestamp`、`code_version`（git hash）；
> - 落盘在**自己 task 目录下**，不要写进 `dag_poc/traces/`（PR-35：会被外部进程 git 还原、交付物会丢）；
> - temperature 一律调 `agent_harness.temperature_for(model)`——**kimi-k3 传 0 会返 HTTP 400**。


| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E4-03 / `task/E4-agent-harness搭建/` |
| **类型** | Bugfix（交付物保护）+ 收尾 |
| **状态** | ✅ **已收口**（2026-08-07；状态行滞后于 task-04 收口时修复，见 report-task-03.md 与 report-task-04.md；QA 独立核验六项全过） |
| **负责人** | E4（harness 线） |
| **预计起止** | 2026-08-07 ~ （约半天） |
| **上游来源** | `problems.md` **PR-35**（本 task 做落盘防护这一半；测试 fixture 那一半归 E3-04） |
| **下游消费方** | **E3**（要用你的新 trace 验判分修复）· **E6**（跑测编排要知道 trace 从哪读） |

---

## 一句话目标

> 把 trace 落盘挪出会被 git 还原的目录，让你的交付物不再反复丢失；并把已跑出的 9 组冒烟证据固化成 E3 能直接用的验证材料。

---

## 0. task-02 的核查结果

产出都在：`dag_poc/trace_schema.json` 定稿、9 组端到端冒烟跑通（3 case × 3 模型）、`smoke_evidence` 与 `_smoke_eval_results.json` 已落盘。

**你报的两件事，一件已升为最高优先问题，一件本 task 处理：**

**第一件（已归 E3）**：你发现 `evaluate.py` 的 `extract_tool_data()` 只认旧工具名，新契约 trace 判分 cp1–cp5 系统性为 0。**我核实了，你报得对**——判分器认的是 `predict_admet`/`crystal_predict` 等旧名，与新契约 9 工具的交集只有 `literature_search` 一个。已登记 `problems.md` **PR-34（最高）**，归 E3 改（`evaluate.py` 属它的写作区）。

**你报了但没越位改 `evaluate.py`，这个做法是对的。** 这条断裂如果没被你的端到端冒烟撞出来，会一直藏着——因为历史 63 份 trace 都是旧工具跑的，判分器读得懂。

**第二件（本 task）**：`dag_poc/traces/` 被外部进程 git 还原，你两次落盘的新 trace 丢失。

---

## 1. trace 落盘防护（主任务）

### 1.1 事实

你报：2026-08-07 01:35:53，落盘的新 trace/答卷（cns、sotalol 六组，均为 git 已跟踪文件）被**整体还原为旧作废内容**，还原后与 git HEAD 逐字节一致；未被 git 跟踪的 open 组未受影响。疑似有并行流程执行了 `git checkout -- dag_poc/traces/`。

**连带后果**：E3 的 `test_answer_dual_parse.py` 拿 `traces/case_cns_metoprolol__qwen3.7-max_response.json` 当 fixture，还原后该文件 `response` 变 `None`，**导致 5 个测试失败**。

**我未执行过 `git checkout`**，还原源尚未查明。但不管源是什么，**你的交付物不该放在会被别人覆盖的位置**。

### 1.2 要做

| # | 要求 |
|---|---|
| 1 | **新 trace 落盘改到 `task/E4-agent-harness搭建/` 下的独立目录**（如 `traces_new/`），不与 git 跟踪的 `dag_poc/traces/` 混用 |
| 2 | `dag_poc/traces/` 保持只读——那里是**历史资产**（63 份旧 trace 是判据回归的 fixture，按 ADR-0014 修订 R1 不得改写） |
| 3 | harness 的输出路径**做成可配置**，不要写死；默认指向你自己的目录 |
| 4 | 在 report 里写清新旧两个目录的分工，供 E3/E6 取用 |

**为什么不是"让别人别动"**：还原源没查明前，靠约定不可靠；换个位置是确定能生效的防护。

---

## 2. 冒烟证据固化（给 E3 用）

E3 要用你的新 trace 验证判分修复（PR-34 的通过标准是"用新 trace 实跑证明 cp1–cp5 非 0"）。

**要做**：把 9 组冒烟里**至少 2 组 fixed 类**的 trace + 答卷固化到你的新目录，并在 report 里给出路径与说明（哪个 case、哪个模型、用的是哪几个新工具）。**这份材料要能被 E3 直接拿去跑，不需要再问你。**

---

## 3. 范围与非目标

**不做**：**不改 `src/benchmark/evaluate.py`**（PR-34 归 E3，你继续不越位）、不改 `dag_poc/traces/` 里的历史文件、不改 `templates/**`（E5 的）、不改共享账本。

---

## 4. 预期交付物

| # | 交付物 | 通过标准 |
|---|---|---|
| 1 | trace 落盘位置迁移 | 新 trace 在 `task/E4-*/` 下；`dag_poc/traces/` 不再被写入 |
| 2 | 输出路径可配置 | 不写死；默认指向自己目录 |
| 3 | 冒烟证据固化 | ≥2 组 fixed 类 trace + 答卷，**E3 可直接取用**，路径与说明在 report 里 |
| 4 | 新旧目录分工说明 | 供 E3/E6 取用 |
| 5 | report 收口 | 含"还原事件"的完整时间线（便于后续排查还原源） |

---

## 5. 约束

**继续保持不越位**：发现别人写作区的问题就报监工，像你这次报 `evaluate.py` 一样。需要立 ADR 的报监工。进度写自己的 `progress.md`。
