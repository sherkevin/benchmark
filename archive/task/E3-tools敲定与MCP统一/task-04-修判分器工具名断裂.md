# Task E3-04 — 修 PR-34：判分器接新契约工具名 + 测试 fixture 去外部依赖

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
| **Task ID / 路径** | E3-04 / `task/E3-tools敲定与MCP统一/` |
| **类型** | Bugfix（红线级 + 测试稳定性） |
| **状态** | ✅ **已交付**（§1 双轨抽取 b85f4a7 + §2 fixture 自带化 e967a1d + 监工裁定 A 两段式断言 4751917 + 评审合并修复 178ef6c；QA #48 停止准则三条全过：155 passed + 1 skipped 零失败 / 新契约 6 点 cp1-cp5 非 0 / legacy 9 点重判逐字段零差异。翻转幅度已公布：fixed avg task_score 0.067→0.127。收口章节见 report.md §10，任务 #49 状态对齐） |
| **负责人** | E3（工具线） |
| **预计起止** | 2026-08-07 ~ （约半天） |
| **上游来源** | `problems.md` **PR-34**（最高）· **PR-35**（高，本 task 只做其中的测试脆弱性一半） |
| **下游消费方** | **E4**（跑测拿到的过程分才可信）· **E5**（30 道新题才验得了判分）· **E6**（矩阵结论才成立） |
| **优先级** | 🔴 **最高** —— 一条堵着三条线的产出可信度 |

---

## 一句话目标

> 让判分器认得新契约的 9 个工具，使 agent 用新工具跑出的 trace 能拿到非零的过程分；同时把测试 fixture 从"依赖别人的 trace 文件"改成自带，避免别人一重跑就挂。

---

## 0. task-03 验收通过，但随后发现一个更严重的问题

task-03 三项全达标：匿名映射表接入有实跑留痕（`rehearsal_logs/grounding_condition3_realdata.log` 三判据全 True）、材料侧身份键 4 用例 passed（5,771/5,771 等价）、e2e 由 skip 转 passed（独立 venv 装 mcp==1.29.0）。

**但 E4 在跑端到端冒烟时发现了一个你我都漏掉的断裂**，见下。

---

## 1. PR-34（主任务）：判分器只认旧工具名，新契约 trace 判分全 0【✅ 已完成：commit b85f4a7（任务 #45）新契约迁移 + legacy 双轨分派；裁定 A 两段式断言随 4751917；评审修复随 178ef6c；停止准则三条实证见 report.md §10.3】

### 1.1 事实

`src/benchmark/evaluate.py` 的 `extract_tool_data()`（L59-95）识别的工具名是：

```
predict_admet · crystal_predict · compare_materials · fetch_bib_info · get_cif_by_formula · literature_search
```

而你已冻结的新契约 9 工具是：

```
literature_search · search_candidates · get_entity · get_property · compare
submit_answer · check_molecule_exists · edit_molecule · normalize_structure
```

**两套名单的交集只有 `literature_search` 一个。**

### 1.2 后果

agent 用新工具跑出的 trace 送进判分时，**cp1–cp5 的证据集系统性为空 ⇒ 过程分全 0**。E4 冒烟实测已复现：fixed 两组 9 个点 cp1–cp5 全 0。

**这等于"评过程"这个项目核心卖点在新契约下完全失效**——而这是我们跟其他 benchmark 的唯一实质差异。

### 1.3 为什么一直没暴露

既有 63 份历史 trace 都是旧工具跑的，判分器读得懂；MCP 换成新契约后才断裂，而**在 E4 之前没有人用新工具跑过端到端**。

**这里有我的失职**：我裁定你换工具契约时（ADR-0014/0015），只核了答卷字段（`candidates`/`ranking`/`answer`）的一致性，**漏了工具名与出参形状这条链路**——`extract_tool_data` 是从 trace 里抽证据，跟答卷字段是两条独立的路，我当成一条了。

**E4 报了但不越位改 `evaluate.py`，做法是对的。**

### 1.4 要做

| # | 要求 |
|---|---|
| 1 | `extract_tool_data()` 按**新契约 9 工具名与新出参形状**重写证据抽取。注意出参形状也变了——比如新契约的 `literature_search` 返回 `papers[].doc_id`，而判分器现在读的是 `doi` |
| 2 | **旧工具名走 legacy 分支保留**，承 ADR-0014 修订 R1 的 `legacy_adapter` 思路（63 份历史 trace 仍要能重判，不能因为改了新契约就把旧资产废掉） |
| 3 | 按 `schema_version` 或工具名集合分派，**不要靠"字段存在性"猜** |
| 4 | **改完必须用 E4 的新 trace 实跑一次，证明 cp1–cp5 非 0** —— 这是本项的通过标准，不是"代码写了"就算完 |

**E4 的新 trace 在哪**：它的落盘位置见 `task/E4-agent-harness搭建/` 下的 `smoke_evidence` 与 `_smoke_eval_results.json`。若那批 trace 又被还原（见 §2），找 E4 重新出一份。

---

## 2. PR-35 的一半：测试 fixture 去掉外部依赖【✅ 已完成：commit e967a1d（任务 #46）dual_parse 改自带最小样本 fixture；task-04 新增测试同口径固化 tests/fixtures/ 最小切片（7 文件 md5 溯源，任务 #50 Major3），不再引用 dag_poc/traces/】

`tests/test_answer_dual_parse.py` 用 `dag_poc/traces/case_cns_metoprolol__qwen3.7-max_response.json` 当 fixture。该文件被 E4 重跑（以及疑似被某个进程 `git checkout` 还原）后，`response` 变成 `None`，**导致 5 个测试失败**。

我实跑现在是 **5 failed / 138 passed**。你交付时自述的"143 passed 零回退"**是当时的真实状态**，之后被破坏——不是你报了假数据。

**但这暴露一个设计脆弱性：拿可变的真实数据当 fixture，别人一重跑测试就挂。**

**要做**：把 fixture 改成**自带最小样本**——写死在测试文件里，或放 `tests/fixtures/` 下由本测试独占。旧契约与新契约各留一份最小答卷即可，不要再引用 `dag_poc/traces/` 的任何文件。

**PR-35 的另一半**（trace 落盘位置防护）归 E4，已另派 E4-03，你不用管。

---

## 3. 范围与非目标

**不做**：不改 `dag_poc/traces/**`（那是 E4 的产出）、不改 `templates/**`（E5 的）、不改 `query_spec.json`/`query_lint.py`（E2 的）、不改 `dag_poc/data/**`（E1 的）、不改共享账本。

**不要为了让测试变绿而降低断言**——若某条断言在新契约下确实不适用，报我裁定，不要自行删。

---

## 4. 预期交付物

| # | 交付物 | 通过标准 |
|---|---|---|
| 1 | `extract_tool_data()` 支持新契约 9 工具 | 逐个工具名与出参形状对齐 `tools_schema.json` |
| 2 | legacy 分支 | 63 份历史 trace 仍可重判，有实跑证据 |
| 3 | **新 trace 判分非 0** | 用 E4 的新 trace 实跑，cp1–cp5 有非零值，**贴出实跑输出** |
| 4 | 测试 fixture 自带 | 不再引用 `dag_poc/traces/`；全量测试回到零失败 |
| 5 | report 收口 + 状态行更新 | 前两轮都出现"状态行滞后于实际"，这次一并更新 |

---

## 5. 停止准则

三条**全部**满足才提交验收：

1. `python3 -m pytest dag_poc/mcp_mock_server/tests/ -q` **零失败**；
2. 新契约 trace 判分的 cp1–cp5 **有非零值**（贴实跑输出）；
3. 历史 trace 经 legacy 分支仍可重判（贴实跑输出）。

---

## 6. 风险预判

| 风险 | 说明 |
|---|---|
| **出参形状的差异比工具名更隐蔽** | 工具名对不上会直接抽不到证据；但**出参字段名对不上会静默返回空**，更难发现。建议逐个工具核 `tools_schema.json` 的 output 定义，而不是只改名字 |
| **改完可能翻转历史结论** | 63 份 trace 的过程分会变。这是**预期的**——它们本来是在"判分器读得懂旧工具"的前提下算的。请公布变化幅度而非隐藏 |
| **cp1–cp5 非 0 不等于判对了** | 证据能抽到只是第一步。若有余力，抽一个 case 人工核一下"抽到的证据是否真的对应那个 checkpoint 该有的动作" |
