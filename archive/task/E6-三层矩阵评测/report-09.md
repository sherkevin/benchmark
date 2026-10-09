# report-09 — task-09 修后重跑 3 道（实跑收口报告）

> 2026-08-10 · E6 矩阵线（Taylor，任务 #21）· 依据 `task-09-修后重跑3道.md`
> 本文只记已发生的事实与实测数字，无预测性措辞。

## 0. 一句话结论

**四条断言：① 未过（cp2 六单元仍全 0）、② 硬停线再次触发（H1 与 H2 均 3 道全错，已按纪律停下报监工）、③ 逐道核对完成、④ 过（4 单元非零总分）。五条上游修复全部实测生效（PR-61/62/63/64/65 逐条有落盘证据），但断言①②仍不过——失分点已从工具链缺陷转移到模型行为层（检索未把 gt 送进候选池、tc/logS 上的放宽重试仍在）。全量不开，交监工复验。**

---

## 1. 五条修复逐条实测核实（不采信自述）

| # | 修复 | 本轮实测证据 | 结论 |
|---|---|---|---|
| PR-61 | mw/logp/tpsa 出白名单返 UNKNOWN_PROPERTY | 本轮 279 次调用中 **0 次** mw/logp/tpsa 相关 EMPTY_RESULT；残留 UNKNOWN_PROPERTY 6 次全部是 `tc`（见 §4），不再出现「宣称可过滤实际 0 数据」的放宽重试链 | ✅ 生效 |
| PR-62 | 判分路由显式 template_id | dh 题 `template_id=discovery_haystack_v1`，`case_is_discovery()=True`；直调 `_score_open_discovery` 返回 5 元组（含 extra），dh 两单元 scores 六 cp 全标 `not_attempted`（open 分支专属标记，fixed 分支不产生该标记） | ✅ 生效（见 §5 分支证明） |
| PR-63 | 错误信封 charset | H1 co 落盘 trace 中 UNKNOWN_PROPERTY 信封实测为正常中文：「性质 'num_elements' 不可查证。本工具可查值性质：['atom_count','bbb','caco2',…]」（上轮同位置为 mojibake） | ✅ 生效 |
| PR-64 | H2 --include-partial-messages 逐次 usage | H2 三单元 `_full.jsonl` llm_call 事件数 = 16/28/24（上轮恒 0）；run_meta.seven_items 由逐次累加产出 | ✅ 生效（交叉校验见 §6） |
| PR-65 | keywords 字符串化数组报 INVALID_ARGUMENT | H1 search_candidates 19 次字符串化写法全部被拒，信封原文：「keywords 收到了数组的字符串化写法…只接受单个字符串」（上轮该形态被静默接受） | ✅ 生效 |

---

## 2. 落盘与执行（同题同规则同 harness）

- **题目**：沿用 task-07 §2 规则挑出的 3 道（`case_cs_drug_15` / `case_co_material_15` / `case_dh_drug_15`），未重新挑题。
- **隔离**：落盘根目录 `task/E6-三层矩阵评测/spotcheck_runs_t09/`（`E6_SPOTCHECK_ROOT`/`E6_RUNS_ROOT` env 指向新目录）；task-07 基线目录 `spotcheck_runs/` 零写入。
- **执行**：`spotcheck_task07.py run --harness H1|H2`（qwen3.7-max，6 单元全部 status=done）。

| harness | case | steps | 工具调用 | ok | 交卷通道 | 空卷 | 耗时 |
|---|---|---|---|---|---|---|---|
| H1 | case_cs_drug_15 | 15 | 16 | 2 | submit_answer | 否 | 57.3s |
| H1 | case_co_material_15 | 15 | 57 | 47 | submit_answer | 否 | 266.5s |
| H1 | case_dh_drug_15 | 15 | 39 | 21 | submit_answer | 否 | 263.6s |
| H2 | case_cs_drug_15 | 55 | 54 | 23 | submit_answer | 否 | 179.0s |
| H2 | case_co_material_15 | 71 | 70 | 13 | submit_answer | 否 | 348.6s |
| H2 | case_dh_drug_15 | 65 | 64 | 1 | **text_fallback** | 否 | 195.8s |

对照上轮：**H1 dh 上轮空卷 → 本轮正常交卷**（PR-63 信封可读 + mw 白名单修复后不再空转耗尽）；H2 dh 仍 text_fallback（原因见 §5，是模型未按契约交卷，不是通道问题）。

---

## 3. 四条断言逐条结论（task-06 §2.2 原文口径，一字未改）

### 断言① cp2 > 0 —— **仍未过**

六单元 cp2 全 0（H1：0/0/0；H2：0/0/0）。cp2 = pool非空 × 可查证比例 × gt∈pool 三因子乘积，**gt∉pool 因子仍恒为 0**：

| 单元 | 答卷候选数 | gt | gt∈pool |
|---|---|---|---|
| H1 cs | 0（交 NO_SOLUTION） | z033990 | 否 |
| H1 co | 37 | c03453 | 否 |
| H2 cs | 9 | z033990 | 否 |
| H2 co | 4 | c03453 | 否 |

（dh 两单元走 open 分支，cp2 标 not_attempted。）判分器判 0 与 trace 事实一致——**检索环节仍未把 gt 送进答卷候选池**。上轮根因之一是 mw 空转耗尽预算挤占检索机会，本轮该根因已修，但结果仍未进池，说明还有别的因素（模型检索策略，见 §8 登记 N1）。

### 断言② cp6 有对有错 —— **硬停线再次触发，已停下报监工**

| harness | cs | co | dh | 判定 |
|---|---|---|---|---|
| H1 | cp6=0.0（best=NO_SOLUTION） | cp6=0.0（best=c01867≠gt） | open_score=0.0 | **3 道全错** |
| H2 | cp6=0.0（best=z000305≠gt） | cp6=0.0（best=c01757≠gt） | open_score=0.0 | **3 道全错** |

`haltline` 退出码 2。按任务书 §3：**已停下报监工，未自行判断，未开全量**。

### 断言③ 六个 cp 逐道对 trace —— **完成**

逐道核对结论（判分器错 vs trace 未做到分开）：

| 单元 | 判分器问题 | trace 未做到 |
|---|---|---|
| H1 cs | 无 | 19 次 search_candidates 全部字符串化 keywords 被拒（PR-65 生效）→ 检索几乎全废，最终交 NO_SOLUTION；cp3=0 因无成功查证 |
| H1 co | 无 | 37 候选不含 gt；tc 不可查证仍反复查（6 次 CALL_LIMIT_EXCEEDED）；cp3=0.441 为成功查证部分覆盖如实给分 |
| H1 dh | 无 | 交卷分子不在 G_B，且 edit_op 无法复现所交答案串（§5） |
| H2 cs | 无 | 答卷 9 候选不含 gt；best=z000305≠gt |
| H2 co | 无 | 42 次 get_property 用错参数名（property/properties/prop_name/…）被拒；4 候选不含 gt |
| H2 dh | 无 | 唯一一次 submit_answer 因入参名全错被拒（anchor_molecules/cited_values/edit_op… 未知入参）→ text_fallback（§5） |

**本轮未发现新的判分器缺陷**——所有失分均与 trace 实际动作一致。

### 断言④ 至少 1 道非零总分 —— **过**

| 单元 | 总分 | 构成 |
|---|---|---|
| H1 cs | 1.0 | cp1=1.0×1 |
| H1 co | ≈1.88 | cp1=1.0×1 + cp3=0.441×2 |
| H2 cs | 2.0 | cp1=1.0×1 + cp3=0.5×2 |
| H2 co | 3.0 | cp1=1.0×1 + cp3=1.0×2 |
| H1 dh / H2 dh | 0.0 | open_score=0（§5） |

4 单元非零，断言④成立。

---

## 4. 新旧逐项对照表（核心产出）

**率的口径**：每轮换成「每次调用的报错率」再比（本轮总调用 279 次，上轮 286 次，绝对次数不直接可比）。

| 指标 | 上轮（task-07） | 本轮（task-09） | 变化 |
|---|---|---|---|
| `EMPTY_RESULT` 次数 / 率 | 86 / 30.07%（86/286） | **55 / 19.71%（55/279）** | ↓ 31 次、↓ 10.4pp |
| `CALL_LIMIT_EXCEEDED` 次数 / 率 | 17 / 5.94% | **19 / 6.81%（19/279）** | **未归零**，+2 次 |
| `UNKNOWN_PROPERTY` 率 | 7.69%（22/286） | **2.15%（6/279）** | ↓ 5.5pp |
| `INVALID_ARGUMENT` 率 | 18.53%（53/286） | **29.39%（82/279）** | ↑ 10.9pp |
| 工具成功率 | 29.4%（84/286） | **38.4%（107/279）** | ↑ 9.0pp |
| cp2（六单元） | 全 0 | **全 0** | 未变 |
| cp6（六单元） | 全 0 | **全 0**（dh 为 open_score=0） | 未变 |
| 分档成功率 H1 / H2 | 53.6% / 13.8% | **62.5% / 22.2%** | H1 ↑8.9pp、H2 ↑8.4pp |

### 监工最想看的两个数

**① `EMPTY_RESULT` 降了但没「大降」（86→55，-36%）**。PR-61 修的那部分（mw/logp/tpsa 空数据返空）确实消失了，残留 55 次全部是**另一类**：`literature_search`（约 21 次，查无论文）与 `search_candidates` keywords 检索落空（约 34 次，如 H2 查 "cuprate"/"YBCO"/"BSCCO" 无命中）。即「还有别的东西在返空」——是库内本就无对应条目的正常空结果 + keywords 面窄，不是白名单 bug 复发。

**② `CALL_LIMIT_EXCEEDED` 未归零（17→19）**。本轮 19 次逐条查实：**全部是 `get_property`/`get_entity`/`compare` 上对 `tc`（超导临界温度）与 `logS` 的放宽重试耗尽调用预算**（H1 co 6 次 tc、H2 co 8 次 tc + 1 compare、H2 cs 2 次 logS、H2 dh 2 次）。`tc` 不在 PR-61 出白名单的三项内，也不在可查清单内（信封已如实报 UNKNOWN_PROPERTY 6 次），但 agent 收到拒信后仍反复换 id 重查同一性质直至打满上限——**行为模式与上轮 mw 相同，换了载体**。已登记 N1 报监工。

### INVALID_ARGUMENT 上升归因（查实）

82 次构成：H1 search_candidates 19（字符串化 keywords，PR-65 拒绝——上轮这类**不报错**，故本轮计数上升一部分是「以前漏报的现在如实报」）；H2 get_property 42（参数名猜错：property/properties/prop/prop_name/…）+ get_entity 4 + exists 6 + submit_answer 2 + search 8 + literature 1。H2 的参数名漂移是模型行为，说明书字段名无错。

复算命令：
```
./.venv/bin/python dag_poc/mcp_mock_server/trace_replay_task07.py --mode aggregate --matrix task/E6-三层矩阵评测/spotcheck_runs_t09
./.venv/bin/python dag_poc/mcp_mock_server/trace_replay_task07.py --mode aggregate --matrix task/E6-三层矩阵评测/spotcheck_runs   # 上轮基线
```

---

## 5. dh 道专段（open 判分公式首次真实执行）

**① 分支证明（是 open 不是 fixed）**：
- case 文件 `template_id='discovery_haystack_v1'`，`case_is_discovery(case)` 实测 True；`score_case` 主路由 PR-62 分支（`evaluate.py` L317-321）优先于 `'gt' not in case` 旧判据。
- 落盘证据：dh 两单元 score.json 的 `scores._status` 六 cp 全为 `not_attempted`——该标记**只有 `_score_open_discovery` 会写**（fixed 分支产 scored/missed），即路由留痕。
- 独立复跑：直调 `_score_open_discovery(case, events, …)` 返回 5 元组（含 extra），与 score_case 的 [:4] 前四项一致。

**② hit(G_B) 判定**：G_B = {z003922}（B 分区合格集，grounding_mode=**real**，非占位）。
- H1：提交 best='[16*]18[19*]16[22*]3'（edit-op 形态字符串，非分子 ID）→ **hit_gb=false**。
- H2：text_fallback，best=NO_SOLUTION、ranked=[] → **hit_gb=false**。

**③ grounding 三条子判据**：

| 子判据 | H1 | H2 |
|---|---|---|
| anchor ∈ A | ✅ true | ❌ false（无契约交卷，无 anchor） |
| 引用值与工具返回一致 | ✅ true | ❌ false |
| edit_op(anchor) == 提交分子 | ❌ false（所交答案串无法由 edit_op 复现） | ❌ false |
| **grounding_pass** | **false** | **false** |

**④ 存在性接口与预算**：
- H1：`check_molecule_exists` 3 次，全部成功信封（attempts=3），无 ATTEMPTS_EXCEEDED，预算未耗尽。
- H2：trace 中 7 行，其中仅 1 次成功信封（attempts=1），其余 6 次因入参名猜错（id/entity_id/sid/molecule）被 INVALID_ARGUMENT 拒；预算未耗尽。

**结论**：open_score = 𝟙[hit(G_B)] × 𝟙[grounding_pass] = 0×0 = **0.0**（两单元），公式首次真实执行，判分器无缺陷；失分全在 trace（交的不是 G_B 内分子 / 未按契约交卷）。

**上轮对照如实记**：H1 上轮空卷 → 本轮正常交卷（修复见效）；**H2 仍 text_fallback**——唯一一次 submit_answer 因入参名全错（'anchor_molecules','answer','candidates','cited_values','edit_op','ranking' 全被判未知入参）被拒，终态文本兜底落盘。**是模型没按契约交卷，不是通道问题**（通道本身收到并如实拒绝了该调用）。

---

## 6. token 七项与 H2 交叉校验

七项齐全（每个单元 run_meta.token_usage.seven_items），独立复算命令见 §10。

| 单元 | llm_calls | prompt | completion | total | reasoning | cached | 失败重试 |
|---|---|---|---|---|---|---|---|
| H1 cs | 15 | 98,738 | 2,092 | 100,830 | null | 59,136 | 0 |
| H1 co | 15 | 286,675 | 13,464 | 300,139 | null | 173,184 | 0 |
| H1 dh | 15 | 162,808 | 14,475 | 177,283 | null | 109,824 | 0 |
| H2 cs | **16** | 603,228 | 8,240 | 611,468 | null | 558,748 | 0 |
| H2 co | **28** | 1,144,484 | 14,489 | 1,158,973 | null | 1,086,535 | 0 |
| H2 dh | **24** | 789,131 | 7,409 | 796,540 | null | 753,100 | 0 |

reasoning 写 **null**（claude/idealab 流未暴露推理 token 字段，不填 0）。H1 的 reasoning 亦 null（本轮起统一口径：字段从未出现 → null；上轮 report-07 记 0 系旧口径，此处如实说明差异）。

**H2 交叉校验（PR-64 后首次可做）**：stream 路 = `_full.jsonl` 逐次 llm_call（message_delta）累加；session 路 = raw_stream result 事件 usage 汇总。结果：**三单元 diff 全 0，identical=true**。

**独立性取证（防 PR-55 同源误判）**：直读 H2 cs 的 raw_stream，独立累加 16 条 `stream_event.message_delta` 的 usage = {output 8,240 / input 96 / cache_read 558,748 / cache_creation 44,384}，与 result 事件 usage **逐项相等**。定性：**claude CLI 的 result 汇总本身就是逐次 delta 之和（CLI 自身口径），不是两路抄同一来源**——两路取数代码路径、事件类型、累加逻辑均不同。但因此「接近但不完全相等」的判据在该数据层**天然无法满足**（只要 CLI 汇总口径不变，恒相等）。已登记 N2 报监工：若需真独立第二路，需引入服务端计费单等外部源，不改 harness 前提下本轮做到此为止。

---

## 7. 噪声门改动清单（goal §7.5，只列不实施）

开全量时「3 轮 + 每格必报离散度 + 均值差小于离散度不得声称差异」。现只有均值列，需加离散度列（极差或标准差）的位置：

| 表 | 需加离散度的列 |
|---|---|
| `result_tables/01_main_table.md` A 表（fixed 主表） | `process 均值`、`task_score 均值`（通过率列可加 3 轮极差或保留二值分布） |
| `result_tables/01_main_table.md` B 表（open 主表） | `open_score 均值`、`平均 attempts` |
| `result_tables/02_process_breakdown.md` | `cp1~cp6` 六列、`process 均值` |
| `result_tables/03_ablation_harness.md` | `task_score 均值`、`process 均值` 两指标行（Δ 列需以离散度为声称门槛） |

本任务未改任何一张表。

---

## 8. 登记项（报监工，本任务未改题/判分器/工具/harness 任何一行）

| # | 事项 | 证据 |
|---|---|---|
| N1 | **CALL_LIMIT_EXCEEDED 未归零的新载体**：agent 对不可查性质 `tc`/`logS` 收到 UNKNOWN_PROPERTY 后仍换 id 重查同一性质直至打满调用上限（19 次）。PR-61 只处理了 mw/logp/tpsa；是扩大白名单治理、还是在信封里加「该性质全局不可查，换性质而非换 id」的强提示，报监工裁定归属 | §4②；trace 逐条：H1 co tc×6、H2 co tc×8+compare×1、H2 cs logS×2、H2 dh×2 |
| N2 | H2 token 两路 identical=true 属 CLI 汇总口径天然相等（§6 取证），task-09 §6「不应完全相等」判据在该数据层不可满足；如需真独立第二路需外部计费源 | §6 |
| N3 | H2 dh 模型参数名漂移严重（get_property 42 次、submit_answer 1 次全因入参名被拒）——说明书字段名无误，属模型行为；是否需说明书更强的参数名锚定示例，报监工酌定 | §4 INVALID_ARGUMENT 构成 |

---

## 9. 红线遵守自查

| 红线 | 状态 |
|---|---|
| 不重新挑题 | 沿用 task-07 同 3 道 |
| 不改题/判分器/工具/harness | 未动（N1~N3 只登记）；仅接线类适配：matrix_runner H2 cmd 补 `--include-partial-messages`（PR-64 落地接线，task-08 声明只影响日志流）+ tool_events 过滤兼容 llm_call 事件（不加不改给分/调用逻辑） |
| 不放宽断言 | 四断言原文执行，①未过如实记、②触发即停 |
| 不覆盖 task-07 落盘 | 落 `spotcheck_runs_t09/`，`spotcheck_runs/` 零写入 |
| 不自行开全量 | 只跑 6 单元；硬停线触发已停，等监工复验发令 |

---

## 10. 复算命令汇总

```bash
# 挑题（与 task-07 同规则）
./.venv/bin/python -m src.eval_matrix.spotcheck_task07 pick
# 判分重跑（对每单元重调 evaluate.score_case）
E6_SPOTCHECK_ROOT=task/E6-三层矩阵评测/spotcheck_runs_t09 ./.venv/bin/python -m src.eval_matrix.spotcheck_task07 score
# cp6 硬停线（全对/全错 → 退出码 2）
E6_SPOTCHECK_ROOT=task/E6-三层矩阵评测/spotcheck_runs_t09 ./.venv/bin/python -m src.eval_matrix.spotcheck_task07 haltline
# token 七项独立复算（只读 _full，含 H2 两路交叉校验）
E6_SPOTCHECK_ROOT=task/E6-三层矩阵评测/spotcheck_runs_t09 ./.venv/bin/python -m src.eval_matrix.spotcheck_task07 meta7
# 汇总表
E6_SPOTCHECK_ROOT=task/E6-三层矩阵评测/spotcheck_runs_t09 ./.venv/bin/python -m src.eval_matrix.spotcheck_task07 summarize
# 报错率（本轮 / 上轮基线）
./.venv/bin/python dag_poc/mcp_mock_server/trace_replay_task07.py --mode aggregate --matrix task/E6-三层矩阵评测/spotcheck_runs_t09
./.venv/bin/python dag_poc/mcp_mock_server/trace_replay_task07.py --mode aggregate --matrix task/E6-三层矩阵评测/spotcheck_runs
```

## 11. 交付物清单

| # | 交付物 | 路径 |
|---|---|---|
| 1 | 6 单元完整落盘（trace+_full+response+run_meta 七项+score.json） | `task/E6-三层矩阵评测/spotcheck_runs_t09/qwen3.7-max/{H1,H2}/case_*/round_00/` |
| 2 | 四断言逐条结论（含实测数字） | 本报告 §3 |
| 3 | 新旧逐项对照表（率口径一致） | 本报告 §4 |
| 4 | dh open 判分明细（分支证明/hit(G_B)/grounding 三条/exists 预算） | 本报告 §5 |
| 5 | H2 token 交叉校验 + 独立性取证 | 本报告 §6 |
| 6 | 噪声门改动清单（只列不改） | 本报告 §7 |
| 7 | report-09.md（本文，含复算命令） | `task/E6-三层矩阵评测/report-09.md` |
