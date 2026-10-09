# E6 · 三层矩阵设计定稿（模型 × harness × template）

| 字段 | 值 |
|---|---|
| **文档** | task-02 交付物 1+2（矩阵设计定稿 + 总跑测次数与总耗时预估） |
| **日期** | 2026-08-07 |
| **状态** | 设计定稿；**正式跑测未开**（等 E5 题集、专家回执、E4 二期 harness）；耗时预估已按 dogfooding 档 600s 量级复核（2026-08-08，§6.3 口径注）；H2 等 E4 端到端（task-05）跑通，不收口消融表 |
| **统计口径** | 沿用 E2 功效结论（query格式与风格规范.md §4.7.4；机器可读版 `dag_poc/query_spec.json → min_publishable`），不另立口径 |
| **容量/题量原则** | **本文档与代码一律不写死题量/容量数字**；引用处注明 `query_spec.json` 字段路径，估算代码动态读取（见 §5） |

---

## 0. 核心结论（先总后分）

**矩阵定为 M=4 模型 × H=2 harness × 4 类 fixed template（open 类 0 template，等 E5）× k=3 重复，设计定稿（M4 已定档 Peach-07-17-DogFooding，监工 2026-08-09 拍板，见 §1.1）**。
总跑测次数公式 `总次数 = M × H × 题量 × k`，满额档（题量取自 `query_spec.json → total_questions`，当前落盘 150）为 **3600 次**；单次耗时按既有 trace 时间戳采样定规划值 **600 秒/次**，串行总耗时约 **600 小时**，并发 8 路约 **75 小时**（依据见 §6）。
fixed 侧 120 题功效达标可出显著性；open 侧 30 题只出描述性——该区分已写进 `src/eval_matrix/stats.py` 的 API 强制逻辑。

四个待定项（不阻塞设计定稿，阻塞开跑）：

1. 模型第 4 格（弱档位候选）已由监工拍板定档 Peach-07-17-DogFooding（2026-08-09，见 §1.1）；
2. harness 第 2 格（真实 harness）端到端就绪度以 E4 交付为准；
3. open 侧 template/case 待 E5 构造（当前 0）；
4. 题量/容量若被 E2-04 订正，以 `query_spec.json` 落盘值为准，代码动态读取不受影响。

---

## 1. 因子一：模型（M = 4）

### 1.1 入选清单与对标依据

| # | 模型 | 能力档位定位 | 对标依据（非拍脑袋） | 状态 |
|---|---|---|---|---|
| M1 | `qwen3.7-max` | 旗舰大参数档 | idealab 默认旗舰；`dag_poc/traces/` 已有 6 条该模型 trace（PR-26 作废的是**内容**，能力档位参考仍有效）；`src/llm_api_test/config.json` 已验证可用 | ✅ 可跑 |
| M2 | `bailian/glm-5.2` | 中强档 | 同上：既有 trace 实测中位耗时 174s（§6.3 快照值），与 M1 行为差异可见；config.json 已验证可用 | ✅ 可跑 |
| M3 | `moonshot/kimi-k3` | 长上下文档 | 同上：6 条既有 trace；API 约束 temperature 只能取 1（config.json `model_constraints` 实测留痕），harness 已适配 | ✅ 可跑 |
| M4 | **Peach-07-17-DogFooding**（监工 2026-08-09 拍板）：免费档（dogfooding profile，不进裸答基线采样）、推理模型（reasoning_tokens 非空）、真题单调用实测超 120s（E6 样本有 179s 记录，timeout 抬到 600s 档）、会拉长 H2 排期 | 弱档（拉开矩阵分数梯度） | 矩阵若只有三个强模型，分数分布下界无锚点、区分度不足；弱档格位必须有 | ✅ **已定档**（免费额度，H1/H2 冒烟实证后入排期；H2 不可路由则格位 failed 留痕不硬凑） |

**M 值取 4 的依据**：三档强模型（旗舰/中强/长上下文）已在 idealab 同一 API 面验证可跑（config.json `models` 字段三个模型全部实测过 trace），加一个弱档凑齐"强-中-弱"能力谱系；每加一个模型成本线性 +25%（见 §5 公式），4 是能力覆盖与成本的可接受平衡点。M4 已定档 Peach-07-17-DogFooding（免费档，不产生 token 费用；若某 harness 可用性确认失败，该格位 failed 留痕如实报，不静默改口径）。

### 1.2 温度与采样约束（写进运行配置）

| 模型 | temperature | 依据 |
|---|---|---|
| M1 / M2 / M4 | 0 | 可复现优先；`agent_harness.py:llm_chat` 默认 temperature=0（Peach 实测接受 0 与 1，无需 override） |
| M3（kimi-k3） | 1 | API 硬约束：temperature=0 返 HTTP 400 MPE-001（config.json 实测留痕）；其随机性由 k 次重复吸收 |

---

## 2. 因子二：harness（H = 2）

以 E4 交付为准（`task/E4-agent-harness搭建/` + `src/benchmark/`）。

| # | harness | 定位 | 就绪状态 | 未就绪原因 |
|---|---|---|---|---|
| H1 | **薄壳 AgentHarness**（`src/benchmark/agent_harness.py`） | 一期薄层：LLM function calling + MCP client + JSONL tracer，mini-SWE-agent 哲学 | ✅ **就绪**：三模型 trace 已实测产出；模型可插拔（`run_case(model=...)`）；trace 与 `evaluate.py` 输入契约对齐 | — |
| H2 | **真实 harness（商用 CLI）**，经 `cli_trace_adapter.py` / `qodercli_trace_adapter.py` 转换 | 二期：goal §6.1 演进路径的"后续上真实 harness"，供 harness 维度对比 | ⏳ **部分就绪**：两个 adapter 已落盘（格式转换 + final response 提取），但**真实 CLI 端到端跑 case → 判分器吃通**链路未见 E4 验收记录 | harness 侧未接入 MCP tools 实跑；trace 契约的 harness 分支未经 E5 题集回归 |

**H 值取 2**：H1 是必选基线（E4 一期已交付），H2 是 goal §6.1 指定的二期目标；若开跑时 H2 仍未端到端就绪，矩阵降为 H=1，harness 消融表（`result_tables/03_ablation_harness.md`）相应标注"单 harness，消融不适用"，不得拿 H1 自比充当消融。

**H2 状态核查（2026-08-08，task-04 §1.2）**：E4 正在做端到端接入（task-05），跑通后会明确知会。**在那之前不按 H=1 收口消融表、不拿 H1 自比充当消融**（本设计 §2 自设规矩）；消融表维持"单 harness，消融不适用"占位状态，如实等待，不预测时点。

---

## 3. 因子三：template

从 `dag_poc/templates/` 四个目录实测枚举。**注意命名陷阱**（goal §1.5 / `query_spec.json → legacy_template_migration.warning`）：`open_discovery` 目录名带 "open" 但属 **fixed** 家族，family 判定以 case/template 的 `family` 字段为准，不看目录名。

### 3.1 fixed 家族（评过程：checkpoint DAG + 路径拓扑验证）

| template 目录 | 迁移目标类型 | 落盘 template | 落盘 case 数 | 现状 |
|---|---|---|---|---|
| `constraint_satisfaction/` | type1（约束满足，5 cp，无 cp5） | `template.json` | 2（cs_cns_drug、cs_supercon_material） | ✅ v1 目标类型 |
| `constrained_optimization/` | type2（约束最优，6 cp 含 cp5） | `template.json` | 2（co_cns_drug、co_supercon_material） | ✅ v1 目标类型 |
| `fixed_discovery/` | 迁移至 type1（drop cp5、重写 query） | `template.json` | 29（药物侧存量） | ⏳ 旧目录待迁移（`query_spec.json → legacy_template_migration`） |
| `open_discovery/` | 迁移至 type2（保留评分核，改 A+M+T+D） | `template.json` | 4（材料超导侧存量） | ⏳ 旧目录待迁移；**名为 open 实属 fixed** |

### 3.2 open 家族（评终态：`open_score = 1[hit(G_B)] × 1[grounding_pass]`）

| template 目录 | 类型 | 落盘状态 |
|---|---|---|
| `discovery_haystack/`（type4） | open 发现探索类 | ❌ **0 template / 0 case**（E5 未建；goal §1.5 陷阱二明示完成度 0） |

**对矩阵的含义**：本批矩阵的 template 维度只有 fixed 4 类；open 侧 30 题（`query_spec.json → per_type[type4].count`）的矩阵格位预留，case 由 E5 交付后纳入，估算公式已把 open 题量参数化（§5），不需要改设计。

---

## 4. 每格重复次数 k 与随机种子策略

### 4.1 k = 3

**取值**：每个（模型 × harness × 题）组合跑 **k = 3** 次独立重复。
**依据**：① M3（kimi-k3）受 API 约束只能 temperature=1，单次结果含采样噪声，k≥3 才能报均值/方差而非单点；② 成本随 k 线性增长（§5 公式），k=3 是能估计组内方差的最小常用档；③ k 是估算函数的显式参数（`stats.estimate_total_runs(k=...)`），若排期吃紧可降 k=1 并如实声明"单次结果、无组内方差"。

### 4.2 随机种子策略

| 项 | 规则 |
|---|---|
| **种子命名规则** | `seed_id = "e6-{case_id}-{model_tag}-{harness_id}-r{i}"`，其中 `i ∈ {1..k}` 为重复序号；`model_tag` 为模型名把 `/` 换成 `_`（与既有 trace 文件名规则一致），`harness_id ∈ {thinagent, cli-<name>}` |
| **数值化** | `seed = int(sha256(seed_id).hexdigest()[:8], 16)`（确定性派生，不落随机源） |
| **落盘要求** | 每次 run 的 trace 元数据必须记录 `seed_id` + `seed` + 代码版本（git short SHA），供复现对账 |
| **可复现要求** | 同 `seed_id` + 同代码版本 + 同模型版本 + temperature=0 的模型，应产出一致 trace；temperature=1 的模型（M3）复现口径为"同分布"，以 k 次重复的统计量对账 |
| **禁止** | 禁止运行期临时随机改种子、禁止种子与题面内容耦合（防止种子泄漏题面信息） |

---

## 5. 容量/题量：一律动态读取，不写死

**原则（硬约束）**：本文档出现的任何题量/容量数字仅作**引用说明**，权威来源是 `dag_poc/query_spec.json` 落盘值；统计/估算代码（`src/eval_matrix/stats.py`）一律从该 JSON 动态读取。

| 数字 | 引用字段路径 | 说明 |
|---|---|---|
| 总题量 | `total_questions`（附 `total_is_conditional` 条件声明） | 当前落盘 150；矩阵估算按此读 |
| fixed 题量 | `fixed_total` | 当前落盘 120 |
| open 题量 | `open_total` | 当前落盘 30 |
| 降级四档 | `degradation_ladder.intervals` | open 存量不足时的题量适配（§5.3） |
| 功效阈值 | `min_publishable.power_analysis` | 每组约 97 题（动态解析，代码不写死 97） |
| 容量基数 | `capacity_basis` | 见下注 |

> **容量订正注（以 query_spec.json 落盘值为准）**：`capacity_basis` 药物侧数字存在订正时点差——原值 156/154/91，E2-04 正订正为 **155/153/92**（E1 logS 方案修复后重测；`capacity_basis.correction_E2-04` 字段已留痕）。本文档不引用具体容量数字作任何结论；一切以 `query_spec.json` 落盘值为准，订正落盘后无需改本文档与代码。

---

## 6. 总跑测次数与总耗时预估

### 6.1 总次数公式

$$N_{total} = M \times H \times Q_{tier} \times k$$

其中：
- \(N_{total}\)：总跑测次数（一次 run = 一个模型在一个 harness 上跑一道题的一次独立重复）；
- \(M\)：入选模型数（§1，规划值 4）；
- \(H\)：就绪 harness 数（§2，规划值 2）；
- \(Q_{tier}\)：当前降级档下的题量 = `fixed_total` + 该档 open 有效题量，从 `query_spec.json` 动态读（`stats.question_counts()`）；
- \(k\)：每格重复次数（§4，规划值 3）。

### 6.2 各降级档总次数（规划参数 M=4、H=2、k=3；题量读自 query_spec.json）

| 降级档（`degradation_ladder` 字段） | open 有效题量 | 题量合计 | 总次数 |
|---|---|---|---|
| `maintain_30`（满额） | 30（= `open_total`） | `fixed_total` + 30 | **3600** |
| `tier_20` | 代表值 25（区间 [20,30)） | `fixed_total` + 25 | 3480 |
| `tier_10` | 代表值 15（区间 [10,20)） | `fixed_total` + 15 | 3240 |
| `tier_0` | 0（open 移入 v2） | `fixed_total` | 2880 |

> 区间档取代表值仅用于排期预估；实际开跑时按当档**实际冻结数**代入公式（代码 `stats.estimate_total_runs()` 已参数化）。
> 上表数字由 `stats.estimate_total_runs(n_models=4, n_harnesses=2, k=3)` 计算（单测 `test_estimate_total_runs_formula` 断言公式一致性），非手写。

### 6.3 单次耗时依据（来源注明）

**来源**：`dag_poc/traces/case_*__{model}.jsonl` 既有 trace 的首末事件 `timestamp` 差值采样（`stats.sample_trace_durations()` 可复算）。注意：该批 trace **内容已作废（PR-26），不得作任何分数基线**；此处只取时间戳量级作排期估算。

| 模型 | trace 数 | 中位耗时 | 均值 | 最大 |
|---|---|---|---|---|
| qwen3.7-max | 10 | 83 s | 313 s | 1751 s |
| bailian_glm-5.2 | 10 | 174 s | 425 s | 1885 s |
| moonshot_kimi-k3 | 10 | 245 s | 441 s | 1845 s |

> **防漂移快照注**：本表为 **2026-08-07 01:38** 用 `stats.sample_trace_durations()` 对当时落盘 traces 的复算快照（trace 数 = 每模型 7 条 `case_*__{model}.jsonl` + 3 条 E4 冒烟 `_full` 变体，函数 glob 全部计入；早期稿的 92/158/211、364/468/469 等旧值作废）。**traces 可能被 E4 并行冒烟重生（已实际发生过两轮：01:29–01:33、01:35:53），正式开跑前必须重跑 `stats.sample_trace_durations()` 刷新本表**，不得直接引用快照值作排期承诺。

**规划值取 600 秒/次**：高于三模型均值上限（快照 441 s），为 API 限流重试（`llm_chat` 最多 5 次退避重试）与 open 类 `max_attempts≤10` 的多轮推断留出余量；刷新采样后该结论仍成立（600 > 441，亦高于历次快照的 469/462）；实测分布若再更新，以 `sample_trace_durations()` 重算。

**dogfooding 档口径注（2026-08-08，task-04 / ADR-0019）**：联调期全局档 `LLM_PROFILE=dogfooding` 走免费模型 Peach-07-17-DogFooding，系推理模型，真题实测超 120s（裸答基线样本中有 179s 记录；task-04 冒烟实测 dh 题面单 call 87–166s），超时已改配置驱动 `LLM_REQUEST_TIMEOUT`（dogfooding 档默认 **600s**）——单次耗时量级与本节规划值 600 秒/次同量级（规划值本就是上限量级），§6.4 总耗时表按此量级仍成立。**口径声明：联调期实测基于 dogfooding 档；正式跑付费模型（三层矩阵 M1-M3）时单次耗时分布不同，必须用付费模型重测 `sample_trace_durations()` 与真题实测后再定排期，不得直接沿用 dogfooding 期数字作承诺。**另：裸答基线的 3 付费模型正式采样已按监工指令挂起（已采 1253/1350 留痕保留，切换付费档后断点续用，见 report-04）；联调冒烟一律走 dogfooding 免费档。

### 6.4 总耗时

$$T_{wall} = \frac{N_{total} \times t_{run}}{3600 \times C}$$

其中：
- \(T_{wall}\)：墙钟总耗时（小时）；
- \(t_{run}\)：单次耗时规划值（600 s，见 §6.3）；
- \(C\)：并发度（受 API 限流与 mock server 吞吐约束，需开跑前实测标定）；
- 3600：秒→小时换算。

| 并发度 C | 满额 3600 次 | tier_0 2880 次 |
|---|---|---|
| 1（串行） | 600 h | 480 h |
| 8 | 75 h（≈3.1 天） | 60 h（≈2.5 天） |
| 16 | 37.5 h | 30 h |

**排期结论**：并发 8 路下满额矩阵约 3 天墙钟，落在周级排期内可行；并发度上限须开跑前用 10 次并发冒烟实测标定，不得直接按 16 承诺排期（代码 `stats.estimate_total_hours()` 支持任意 C 重算）。按 dogfooding 档 600s 量级重估（§6.3 口径注，2026-08-08）：t_run 取 600s 上限量级时上表数字不变（规划值本就取 600s）；即联调期口径下满额 3600 次并发 8 路 ≈ **75 小时（约 3.1 天）**为当前最坏量级估计，正式跑付费模型后按重测值刷新。

**M4（Peach）排期修正（2026-08-09 冒烟实测，监工裁定口径）**：
- Peach H1 单题冒烟实测 **336.6s**（case_cs_drug_09，steps=13，tokens 232,751，交卷非空）——按实测值入排期，900 格位串行 ≈84h、并入并发 8 路批次不单独占墙钟（与付费批同池调度）。
- Peach H2 **不可路由**：claude CLI --model Peach-07-17-DogFooding 最小复现（无 tools 无附加 prompt）稳定返 `API Error: 400 平台业务限制`，与 kimi-k3 的 400 同类（idealab anthropic 协议通道不支持该模型名）——Peach H2 900 格位按 failed 留痕如实报，不硬凑；H2 排期仍按 692s 口径（仅付费三模型）。

---

## 7. 与统计层、门禁、结果表的衔接

| 环节 | 落盘 | 要点 |
|---|---|---|
| 统计层 | `src/eval_matrix/stats.py`（+ `test_stats.py`） | 两比例检验、Wilson CI、功效阈值动态读 `min_publishable`；`report_significance(side, ...)` API 级强制：n < 阈值（open 30 < 97）只出描述性 + 功效不足声明，仅 fixed 120 侧允许显著性主张 |
| 前置门禁 | `src/eval_matrix/pre_gates.py`（+ `test_pre_gates.py`） | 三道门子进程编排（题面规范 / A/B 隔离 / 背书覆盖按 ADR-0016 修订 R1 新口径），任一 fail/blocked 即拒绝开跑 |
| 结果表骨架 | `task/E6-三层矩阵评测/result_tables/` | 主表（fixed/open 分表）、过程分拆解、harness 消融；字段对齐 `evaluate.py aggregate()`；占位数据显著标注 |

**本设计不含**：正式实验、真实结果数字——本批只交付设计与统计层（task-02 范围）。
