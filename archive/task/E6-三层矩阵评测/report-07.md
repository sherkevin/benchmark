# report-07 — task-07 抽验判分链路（实跑收口报告）

> 2026-08-10 · E6 矩阵线（Taylor，任务 #20）· 依据 `task-07-抽验判分链路.md`
> 本文只记已发生的事实与实测数字，无预测性措辞。

## 0. 一句话结论

**四条断言：① 未过（cp2 六单元全 0）、② 触发硬停线（H1 与 H2 均 3 道全错，已按纪律停下报监工）、③ 逐道核对完成（判分器路由问题 1 类、错误信封乱码 1 类，其余对不上均为 trace 本身没做到）、④ 过（4 道次非零总分）。全量矩阵不开。**

---

## 1. 资产核实（开跑前）

| 项 | 实测 |
|---|---|
| 150 道手写题落盘 | `dag_poc/templates/{constraint_satisfaction(60), constrained_optimization(60), discovery_haystack(30)}/`，lineage 五批×30 齐 |
| 付费档 | `.env` L28 `# LLM_PROFILE=dogfooding` 已注释 = idealab；run_meta.profile 实测 idealab |
| 判分入口 | `evaluate.score_case(case_path, trace_path, response_json)`；30 道 dh 全部 ∈ `discovery_case_ids()`（b_partition.cases） |
| 报错率取数 | `trace_replay_task07.py --mode aggregate --matrix <root>` |

## 2. 挑题规则（固定、可复现）

**规则：每候选池按 case_id 字典序升序，取第 ⌈n/2⌉ 道（中位）。spare_* 不属手写正批，排除在候选池外。**

| # | 类型 | 候选池 | n | ⌈n/2⌉ | 挑出 |
|---|---|---|---|---|---|
| 1 | 类型1 药物侧 | `case_cs_drug_*.json` | 30 | 15 | **case_cs_drug_15** |
| 2 | 类型2 材料侧 | `case_co_material_*.json` | 30 | 15 | **case_co_material_15** |
| 3 | open | `case_dh_*.json`（drug15+material15 合并字典序） | 30 | 15 | **case_dh_drug_15**（drug_01~15 占字典序前 15 位，第 15 道即 drug_15） |

复算命令：`./.venv/bin/python -m src.eval_matrix.spotcheck_task07 pick`（规则写死在
`src/eval_matrix/spotcheck_task07.py:pick_cases()`，实算输出含 pool 大小与位次）。

## 3. 实跑与留痕

- **隔离**：落盘根目录 `task/E6-三层矩阵评测/spotcheck_runs/`（经 matrix_runner
  新增的 `E6_RUNS_ROOT` env override——加性改动，env 不设时行为与默认目录完全
  不变；H1 子进程经 env 继承同一隔离目录）。正式 `matrix_runs/` 零写入（跑后
  `find` 核实官方目录无本 3 道新单元）。
- **执行**：`spotcheck_task07.py run --harness H1|H2`（qwen3.7-max，workers=3，
  base-port 9100 与正式跑批 9000 隔离）。
- **结果**：6 单元全部 status=done。

| harness | case | steps | 工具调用 | 交卷通道 | 空卷 | 耗时 |
|---|---|---|---|---|---|---|
| H1 | case_cs_drug_15 | 15 | 29 | submit_answer | 否 | 111.0s |
| H1 | case_co_material_15 | 15 | 41 | submit_answer | 否 | 232.7s |
| H1 | case_dh_drug_15 | 15 | 42 | **None（空卷）** | **是** | 187.4s |
| H2 | case_cs_drug_15 | 68 | 67 | submit_answer | 否 | 294.5s |
| H2 | case_co_material_15 | 62 | 61 | submit_answer | 否 | 292.1s |
| H2 | case_dh_drug_15 | 57 | 56 | text_fallback | 否 | 543.0s |

每单元目录含：判分投影 trace、`_full.jsonl` 全量证据流、`response.json`、
`run_meta.json`（七项齐全，见 §7）、`score.json`（六 cp + 总分 + gold_pass_set/
ranked/best/命中候选）。

## 4. 四条断言逐条结论（task-06 §2.2 原文口径）

### 断言① cp2 > 0 —— **未过**

六个单元 cp2 全为 0（H1：0.0/0.0/0.0；H2：0.0/0.0/0.0）。cp2 = pool非空 ×
(pool∩检索命中)/|pool| × gt∈pool，三因子中 **gt∉pool 恒为 0**：两个 fixed 道
agent 交的候选池里都没有 gt（cs：gt=z033990 从未出现在 16 次检索返回；co：
答卷 23 个候选不含 gt=c03453）；dh 道 H1 空卷、H2 走 text_fallback。
判分器判 0 与 trace 事实一致（§5 逐道核对），未过的原因是**检索环节没把 gt
送进答卷候选池**，不是判分器不给分。

### 断言② cp6 有对有错 —— **硬停线触发，已停下报监工**

| harness | case_cs_drug_15 | case_co_material_15 | case_dh_drug_15 | 判定 |
|---|---|---|---|---|
| H1 | cp6=0.0 | cp6=0.0 | cp6=0.0 | **3 道全错** |
| H2 | cp6=0.0 | cp6=0.0 | cp6=0.0 | **3 道全错** |

（dh 道按判分器实际路由走了 fixed 分支，cp6 即其终选判定，见 §5 登记项 R1。）
`spotcheck_task07.py haltline` 退出码 2。**按任务书 §3：已停下，报监工裁定，
未自行判断"应该没事"，未开任何后续跑批。**

### 断言③ 六个 cp 逐道人工对 trace —— **完成，明细见 §5**

归类结果：**判分器问题 2 类（路由 R1、错误信封乱码 R2，均登记报监工，未改
一行）**；其余全部对不上项均为 **trace 本身没做到那一步**。

### 断言④ 至少 1 道非零总分 —— **过**

总分口径 = Σ(cp_i × CP_WEIGHTS_i)（判分器权威权重，满分 10）：

| 单元 | 总分 |
|---|---|
| H1 cs / H1 co | 3.0 / 3.0（cp1=1.0×1 + cp3=1.0×2） |
| H1 dh | 0.0 |
| H2 cs / H2 co | 1.0 / 1.0（cp1=1.0×1） |
| H2 dh | ≈1.11（fixed 分支 cp1 0 + cp4 0.5×2 + cp5 0.111×1） |

4 个单元非零，断言④成立。

## 5. 断言③逐道核对明细（判分器错 vs trace 未做到，分开写）

### H1 case_cs_drug_15（gt=z033990）

| cp | 分 | trace 实况 | 归类 |
|---|---|---|---|
| cp1 | 1.0 | literature_search 1 次且拿到论文 | 一致 ✅ |
| cp2 | 0.0 | 16 次 search_candidates 全用 keywords（且把数组序列化成字符串传入），**从未用 constraints 参数**；检索过的 100 个不同候选中无 gt。而 case 的 s_feasible 显示库内有 36 个可行解含 gt——约束检索是可达的，agent 没用 | **trace 未做到** |
| cp3 | 1.0 | 9 次 get_property 中 3 次成功（6 次 UNKNOWN_PROPERTY：请求了不可查的 molecular_weight 等）；手工复算成功查证恰好覆盖 pool 3 候选 × 2 约束性质 = 6/6 = 1.0 | 判分器正确 ✅（调用多数报错但成功部分恰好全覆盖，分数如实） |
| cp4/cp5 | 0.0 | 答卷 ranking=[z000012,z000007]，与 gold_pass_set（按成功查证值筛）无交集 | **trace 未做到** |
| cp6 | 0.0 | best=z000012 ≠ gt z033990，亦 ∉gps | **trace 未做到** |

### H1 case_co_material_15（gt=c03453）

| cp | 分 | trace 实况 | 归类 |
|---|---|---|---|
| cp1 | 1.0 | literature_search 2 次有论文 | 一致 ✅ |
| cp2 | 0.0 | 答卷 candidates 23 个不含 gt；检索命中集与 pool 交集因子为 0 | **trace 未做到** |
| cp3 | 1.0 | 26 次 get_property 查证覆盖 ranked×约束性质满分 | 判分器正确 ✅ |
| cp4 | 0.0 | gold_pass_set=**0**（答卷候选经成功查证值过滤后无一满足全部约束）→ F1=0 | **trace 未做到** |
| cp5/cp6 | 0.0 | best=c01859 ≠ gt | **trace 未做到** |

### H1 case_dh_drug_15（gt=z003922）

| cp | 分 | trace 实况 | 归类 |
|---|---|---|---|
| 全部 | 0.0 | ①交卷被拒：submit_answer 返 SCHEMA_VIOLATION（答卷未过 answer_schema 校验）→ submission_channel=None、空卷留痕；②7 次 check_molecule_exists 全部 exists=false。**叠加判分器路由问题 R1**：本道是手写 dh（b_partition 已含该 case），但 case 文件带 gt 字段，`score_case` 的 `is_open='gt' not in case` 先行，discovery 分支永不可达，本道被当 fixed 判 | **trace 未做到 + 判分器路由问题（登记 R1，未改）** |

### H2 case_cs_drug_15（gt=z033990）

| cp | 分 | trace 实况 | 归类 |
|---|---|---|---|
| cp1 | 1.0 | literature_search 1 次有论文 | 一致 ✅ |
| cp2 | 0.0 | 28 次检索无 gt 进答卷（最终交 NO_SOLUTION） | **trace 未做到** |
| cp3 | 0.0 | 19 次 get_property **全部 INVALID_ARGUMENT「未知入参 ['properties']」**——说明书原文已写明「字段名叫 props 不叫 properties」并给了可照抄示例，模型未遵守 | **trace 未做到**（说明书无错，实测 description 内 props 出现 3 次且显式防混淆） |
| cp6 | 0.0 | answer=NO_SOLUTION | **trace 未做到** |

### H2 case_co_material_15（gt=c03453）

| cp | 分 | trace 实况 | 归类 |
|---|---|---|---|
| cp1 | 1.0 | literature_search 3 次 | 一致 ✅ |
| cp2 | 0.0 | 答卷候选不含 gt | **trace 未做到** |
| cp3 | 0.0 | **全程 0 次 get_property**（只用 get_entity 20 次看结构，未查证任何性质即交卷） | **trace 未做到** |
| cp6 | 0.0 | best=c01677 ≠ gt | **trace 未做到** |

### H2 case_dh_drug_15（gt=z003922）

| cp | 分 | trace 实况 | 归类 |
|---|---|---|---|
| cp4=0.5 / cp5≈0.111 | — | text_fallback 终态文本里的 SMILES 片段被当 best/ranked 解析；4 次 submit_answer 未产出 accepted payload | **trace 未做到**（未按契约交卷）+ **路由问题 R1**（同 H1 dh，走 fixed 而非 open_discovery 三门闭环） |
| 其余 | 0.0 | — | 同上 |

## 6. 报错率对比（已换算为每次调用报错率；绝对次数不可比）

**样本量差异明示**：改前基线 = 26 份 run / 总调用 1,208 次（aggregate 实测
现存 matrix_runs，UNKNOWN_PROPERTY 418 与 INVALID_ARGUMENT 54 与锁定基线
逐项相等）；改后 = 本次 3 道 × 2 harness / 总调用 286 次。

| 指标 | 改前基线（率） | 改后抽验（率） | 变化 |
|---|---|---|---|
| UNKNOWN_PROPERTY 率 | 418/1208 = **34.60%** | 22/286 = **7.69%** | ↓ 26.9pp |
| └ H1 | 342/964 = 35.48% | 16/112 = 14.29% | ↓ 21.2pp |
| └ H2 | 76/244 = 31.15% | 6/174 = 3.45% | ↓ 27.7pp |
| INVALID_ARGUMENT 率 | 54/1208 = **4.47%** | 53/286 = **18.53%** | ↑ 14.1pp |
| └ H2（全部出自 H2） | 54/244 = 22.13% | 53/174 = 30.46% | ↑ 8.3pp |
| 工具成功率 | 560/1208 = **46.4%**（锁定口径 48.7%） | 84/286 = **29.4%** | ↓ 17.0pp |
| └ H1 | 49.5% | 53.6% | ↑ 4.1pp |
| └ H2 | 34.0% | **13.8%** | ↓ 20.2pp |

**解读（只陈述已查实归因）**：
1. UNKNOWN_PROPERTY 率大降——新说明书的可查性质清单起作用（H1 残留 16 次是
   agent 请求了清单外的 molecular_weight/logD 等；错误信封本身带可查清单，但
   H1 链路信封中文乱码，见登记 R2）。
2. H2 INVALID_ARGUMENT 率上升的唯一查实根因：get_property 19+ 次调用用
   `properties` 作参数名被拒（说明书已显式防混淆，模型未遵守）——**属模型
   行为，不属说明书缺陷**。
3. H2 成功率大跌（13.8%）的主项是 EMPTY_RESULT 86 次（检索空结果），占改后
   全部报错的 42%——新库清零旧条目后检索面变窄，keywords 检索落空率高。

复算命令：
```
./.venv/bin/python dag_poc/mcp_mock_server/trace_replay_task07.py --mode aggregate --matrix task/E6-三层矩阵评测/spotcheck_runs
./.venv/bin/python dag_poc/mcp_mock_server/trace_replay_task07.py --mode aggregate --matrix task/E6-三层矩阵评测/matrix_runs
```

## 7. token 七项留痕（run_meta.seven_items，独立复算）

口径：只读 `_full.jsonl` 逐条 llm_call 累加（不复用 harness/runner 汇总函数）；
cached 两等价字段取 max；失败重试 = 成功调用的 attempt 前置失败数 + 终失败
事件数。命令：`./.venv/bin/python -m src.eval_matrix.spotcheck_task07 meta7`。

| 单元 | llm_calls | prompt | completion | total | reasoning | cached | 失败重试 |
|---|---|---|---|---|---|---|---|
| H1 cs | 15 | 207,077 | 4,831 | 211,908 | 0 | 118,272 | 0 |
| H1 co | 15 | 387,556 | 11,657 | 399,213 | 0 | 244,992 | 0 |
| H1 dh | 16 | 210,269 | 8,872 | 219,141 | 0 | 156,288 | 0 |
| H2 cs | **0** | 0 | 0 | 0 | 0 | 0 | 0 |
| H2 co | **0** | 0 | 0 | 0 | 0 | 0 | 0 |
| H2 dh | **0** | 0 | 0 | 0 | 0 | 0 | 0 |

（H1 seven_items 的 total 与 runner 原生 run_meta.token_usage.total_tokens
逐项相等：211,908 / 399,213 / 219,141，两路取数互为交叉校验；
reasoning=0 为 idealab 非推理档如实；完整数值在每个单元 run_meta.json
→ token_usage.seven_items。）

## 8. H2 token 遗留核查结论（task-07 §5）

**落不下来，登记为遗留问题报监工。** 证据：3 道 H2 的 `_full.jsonl` 中
llm_call 事件数均为 0（meta7 独立复扫实测）。H2 半边的 token 成本维度为空，
三层矩阵第三层（H1 vs H2 对比）在成本维度上不可比；H2 仅有
raw_stream.result.usage 的单元级数字（run_meta.token_usage，
prompt=input+cache_read+cache_creation 口径）供展示，不进任何汇总 totals。
**按指令未改 harness。**

## 9. 登记项（报监工裁定归谁改，本任务未动任何一行相关代码）

| # | 问题 | 证据 | 疑似归属 |
|---|---|---|---|
| R1 | **判分器路由**：手写 dh 题带 gt 字段，`score_case` 的 `is_open='gt' not in case` 先行判 fixed，`discovery_case_ids()` 分支永不可达——30 道手写 dh 全部受影响，open 三门闭环判分实际未接通 | case_dh_drug_15.json 含 gt 键；score_case L302-324；本次 dh 两单元均走 _score_fixed | E3（判分器）或 E5（题面契约）裁定 |
| R2 | **H1 错误信封中文乱码（落盘级）**：UNKNOWN_PROPERTY/SCHEMA_VIOLATION 的中文说明（含可查性质清单）在 H1 trace 中为 mojibake（UTF-8 被按 ISO-8859-1 解码，`err.encode('latin-1').decode('utf-8')` 可还原）。agent 运行期读不到信封里的纠错信息。根因：mock_server HTTP 响应未声明 charset=utf-8，requests 回退 ISO-8859-1。H2 侧不受影响（claude CLI 链路错误文本正常） | H1 cs trace UNKNOWN_PROPERTY 事件 error 字段实测 | dag_poc/mock_server（E3/E4 线） |
| R3 | **H2 无 llm_call 证据流**：token 成本维度 H2 半边恒空（§8） | 3 道 H2 llm_calls=0 | E4（harness）裁定是否补流 |
| R4 | **search_candidates keywords 传参形态**：H1 agent 把 keywords 数组序列化成字符串 `"[\"solubility\"]"` 传入未被拒（行为未定义）；且 agent 普遍不用 constraints 参数——说明书是否需强化约束检索示例，报监工酌定 | H1 cs/co trace 检索调用实测 | E3（说明书）参考 |

## 10. 红线遵守自查

| 红线 | 状态 |
|---|---|
| 不改判分器给分逻辑 | 未改（R1/R2 只登记） |
| 不改题（150 道冻结） | 未动 |
| 不改 harness | 未动（R3 只登记；matrix_runner 仅加 E6_RUNS_ROOT env override，加性、默认行为不变、不涉给分/调用逻辑） |
| 断言没过不放宽 | 断言①未过如实记、断言②触发即停 |
| 不开全量矩阵 | 只跑 3 道 × 2 harness = 6 单元 |

## 11. 复算命令汇总

```bash
# 挑题规则复算（监工可重挑核对）
./.venv/bin/python -m src.eval_matrix.spotcheck_task07 pick
# 判分重跑（对每单元重调 evaluate.score_case 覆写 score.json）
./.venv/bin/python -m src.eval_matrix.spotcheck_task07 score
# cp6 硬停线检查（全对/全错 → 退出码 2）
./.venv/bin/python -m src.eval_matrix.spotcheck_task07 haltline
# token 七项独立复算（只读 _full trace）
./.venv/bin/python -m src.eval_matrix.spotcheck_task07 meta7
# 汇总表
./.venv/bin/python -m src.eval_matrix.spotcheck_task07 summarize
# 报错率（改后 / 基线）
./.venv/bin/python dag_poc/mcp_mock_server/trace_replay_task07.py --mode aggregate --matrix task/E6-三层矩阵评测/spotcheck_runs
./.venv/bin/python dag_poc/mcp_mock_server/trace_replay_task07.py --mode aggregate --matrix task/E6-三层矩阵评测/matrix_runs
```

## 12. 交付物清单

| # | 交付物 | 路径 |
|---|---|---|
| 1 | 6 单元完整落盘（trace+_full+response+run_meta 七项+score.json） | `task/E6-三层矩阵评测/spotcheck_runs/qwen3.7-max/{H1,H2}/case_*/round_00/` |
| 2 | 抽验编排脚本（挑题/跑/判分/硬停线/七项/汇总，全部可复算） | `src/eval_matrix/spotcheck_task07.py` |
| 3 | 四断言逐条结论 + 逐道核对明细 | 本报告 §4/§5 |
| 4 | 报错率换算对比表 | 本报告 §6 |
| 5 | H2 token 遗留结论（落不下来 + 证据） | 本报告 §8 |
| 6 | 登记项 R1~R4 | 本报告 §9 |
