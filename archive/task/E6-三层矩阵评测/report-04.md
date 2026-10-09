# E6-04 裸答基线收口 — report-04（出污染清单与矩阵开跑准备）

> 日期：2026-08-08 · 出处：`task/E6-三层矩阵评测/task-04-出污染清单与矩阵开跑准备.md` · ADR-0018 修订 R1 · ADR-0019（dogfooding）
> 判据写死不得调松（goal §2b）；污染 case 只出判定结论、绝不修改 case（重造归 E5）；命中判定全部复用判分器同一套逻辑，无自写字符串比对；API key 不入代码。

## 0. 一句话结论

**open 判定链路端到端打通且 45 条满 k 结论已出（0 污染）；fixed 离线出 70/90 条结论（0 废弃，pass@10 全 0），20 条采样不完整不硬判；污染清单当前为空（两版已落盘）。** 3 付费模型的剩余 97 份采样按监工指令挂起，切换付费档后断点续跑复判；dogfooding 免费档冒烟验证了采样→判定全链路与命中留痕。

---

## 1. 模型通道处置（本轮最高优先，监工指令逐条落实）

| # | 指令 | 处置（实测留痕） |
|---|---|---|
| 1 | 联调/冒烟一律走 dogfooding 免费档 | ✅ 本轮冒烟、命中路径验证全部走 `.env LLM_PROFILE=dogfooding`（Peach-07-17-DogFooding，timeout 600s），代码未改（`agent_harness._load_llm_config` 整组换 key/model/endpoint） |
| 2 | 不静默消耗付费配额；免费档结果不得冒充 3 模型判定 | ✅ 付费采样进程已全部停止（ps 确认 0 进程）；冒烟落盘独立目录 `bare_baseline_smoke/`，与正式采样目录物理隔离，冒烟结果不进任何污染判定 |
| 3 | 已采付费样本留痕保留、切换后断点续用 | ✅ 已采 **1253/1350**（failures.jsonl 1895 条留痕），断点续跑机制不变（存在且有效即跳过） |

**挂起标注**：3 个付费矩阵模型（qwen3.7-max / bailian/glm-5.2 / moonshot/kimi-k3）的正式基线采样**等监工切换付费档指令**——切换方法：注释 `.env` 的 `LLM_PROFILE=dogfooding` 行（回落 idealab 档），然后 `python3 -m src.eval_matrix.bare_baseline sample` 断点续跑、`judge` 复判。

实测通道事实（留痕）：dogfooding 档下付费模型名被服务端 IRC-001 拒绝（不会静默重定向到 Peach）；上轮 30 路并发曾触发服务端 504 Gateway Time-out 风暴（qwen cs/co 长思考样本缺口主因，已回调并发并留痕于 `bare_baseline_config.json → max_workers_note`）。

## 2. open 判定链路（R1-② 已接真实实现）+ dogfooding 冒烟结果

### 2.1 代码改动（全部限 `src/eval_matrix/`）

- `resolve_composition_to_ids`：原签名级占位 → 直接委托 `evaluate.resolve`（E3 task-05 §7 交付，一对多同分异构全收，不可解析→空集）；
- `judge_open_case` 重写：自由文本抽候选提交串（组成串形态 `\[\d+\*\]…` + 实体 ID 形态 `[zc]\d{4,6}`，**仅作候选提取**），逐 token 调 `evaluate.hit_gb`——G_B 装载走 BPartitionJsonReader、归一走 gateway.normalize_b_key、组成串形态走 R1-② 判定式 `hit = 1[resolve(提交组成) ∩ G_B ≠ ∅]`（hit_gb 内部已含该补判），任一 token 命中即该次作答命中、任一次命中即污染；命中留痕含 sample_index/matched_token/resolved_ids/intersect_gb；
- PR-41 防护（见 §3）。

### 2.2 冒烟结果（dogfooding 免费档，2 道 dh × 2 份 + 命中正向验证）

| 冒烟项 | 结果 |
|---|---|
| 采样链路（Peach，dh_drug_01/02 × 2） | ✅ 4/4 OK，单 call 实测 87–166s（推理模型量级），落盘 `bare_baseline_smoke/`，完整 request/response/usage/latency 留痕 |
| 判定链路（judge_open_case） | ✅ 生效（hit_judgment_active=true），2 道均「干净」（Peach 作答未含 G_B 成员；k=2 口径下满份） |
| 命中路径正向验证（真实 G_B 成员作提交） | ✅ `open_hit_detail('…z064386…', dh_01)` → hit=true、matched_token=z064386、resolve 出 **17 个实体**（与监工实测一致）、∩G_B={z064386} |
| 单测 | ✅ 64/64 通过（新增：一对多、空交集、判分器异常 token 不冒充命中、组成串 token 送判分器、未注册防护、G_B 装载异常防护；原 placeholder 测试改为真实实现断言） |

**口径声明**：冒烟仅验证链路，其采样结果不构成、也不冒充 3 付费模型的污染判定。

## 3. PR-41 防护（交付物 #3）

`judge_open_case` 对未注册 case（b_partition.cases 无键，hit_gb 会报错不回落顶层 g_b）与 G_B 装载异常：try/except 后落盘标注「未注册（判定未生效）」+ note（含异常摘要），不崩溃、不冒充干净、不冒充命中；`open_hit_detail` 对判分器逐 token 异常同样记不可判。单测 2 例覆盖（未注册 hit_fn 抛错不崩、装载异常标注）。

## 4. fixed 离线部分判定（用已采付费样本，R1-③ 单口径）

口径：`p_base = 1/s_feasible_size`（E5 已逐题落盘 30/30，读取契约首选键命中），废弃条件 `pass@10 > 2·p_base`（严格大于）；不足 10 份标「采样不完整」不硬判。

| 判定 | 条数（case×模型） |
|---|---|
| 干净（pass@10=0，全为 0 命中） | **70/90** |
| 污染（废弃） | **0** |
| 采样不完整（不硬判） | 20/90（qwen 16 + kimi 4；缺口即 §1 挂起的 97 份所涉） |

机器可读件：`bare_baseline/judgments/report.json`。注：fixed 命中判定复用 `evaluate.normalize_id` + `_score_fixed` cp6 文义（best==gt_id），判定结果显式标 `hit_judgment_active=false`（PR-34 依赖留痕，生效以判分器支持为前提）。

## 5. 污染清单（交付物 #1/#2，两版落盘）

- 机器可读：`task/E6-三层矩阵评测/contamination_list.json`
- 人读：`task/E6-三层矩阵评测/contamination_list.md`

**当前为空（open 0 条命中、fixed 0 条废弃）**——这不是"没跑"，是 1253 份已采样本按写死判据实判的结果；清单结构已按 task-04 §1.1 要求就位（open 每条含 case_id/模型/第几次采样/答的是什么/matched_token/resolve 实体 ID 与 G_B 交集；fixed 每条含 pass@10/p_base/|S_feasible|/命中采样序号），付费档补齐 97 份后重跑 `judge` 即自动刷新。药物侧 logS 来自 AqSolDB 公开集、污染率可能走高的风险已预置如实报口径，判据不调松。

## 6. 矩阵开跑准备（交付物 #4）

| 项 | 状态 |
|---|---|
| 耗时重估（dogfooding 档 600s 量级） | ✅ `matrix_design.md` §6.3 口径注：联调期实测基于 dogfooding（冒烟单 call 87–166s，上限口径 600s），§6.4 表按此量级仍成立（满额 3600 次并发 8 路 ≈ 75h 为最坏量级估计）；**正式付费档开跑前必须用付费模型重测**，不得沿用 dogfooding 期数字 |
| H2 状态 | ⏳ E4 端到端（task-05）未跑通——**不按 H=1 收口消融表、不拿 H1 自比充当消融**（自设规矩），`result_tables/03_ablation_harness.md` 维持"单 harness，消融不适用"占位，如实等待 |
| 题集 | 139/150 → E5 task-06 已补齐（open 30/30，题集满 150；E5 progress 2026-08-08 收口留痕）；材料 open 11 道未入本批裸答基线（基线批 45 道冻结名单不变，如实标注） |
| 三道门 | 就绪（`pre_gates.py`，task-02 交付）；开跑前置清单逐项见 progress.md 阻塞节 |

## 7. 挂起项与切换方法（唯一挂起）

| 挂起项 | 现状 | 切换方法 |
|---|---|---|
| 3 付费模型剩余采样 97/1350（qwen 91 为主，含服务端 504 结构性缺口；kimi 6；以样本文件 timestamp 实测为准，分布 glm 450/kimi 444/qwen 359，已采 1253） | 进程已停、留痕保留 | 监工指令后：注释 `.env` 的 `LLM_PROFILE=dogfooding` 行 → `python3 -m src.eval_matrix.bare_baseline sample`（断点续跑）→ `judge`（自动刷新两版污染清单）→ fixed 20 条复判 |

## 8. 禁区自查

未改 `evaluate.py` / `query_spec.json` / `dag_poc/data/**`；未跑正式三层矩阵；未修改任何 case 文件；代码与配置无 API key 字面量；本轮 LLM 调用仅 dogfooding 免费档冒烟 4 次 + 命中验证 0 次网络调用（命中验证纯离线走判分器）；付费配额消耗全部发生在本轮监工指令之前的上轮续跑（1253 份留痕如实登记）。

---

## 9. 满额 150 复判补充（2026-08-08 第二轮，任务 #13）

> 题集满额 150/150（E5 task-06 补齐）后按同一判据复判；本节为实测留痕，无预测性措辞。

### 9.1 满额口径核查（交付要点 #1）

| 项 | 实测 |
|---|---|
| 题集构成 | fixed 120（cs drug/material 各 30 + co drug/material 各 30）+ open 30（dh drug/material 各 15）= 150；与 `query_spec.json`（total_questions=150，fixed:open=4:1）一致 |
| 相对上批 45 道的新增 | fixed 90（cs 22+22，co 23+23）+ open 15（dh_material 01-15），共 105 道 |
| G_B 注册比对 | b_partition.cases 35 条（dh 30 + fixture 2 + spare 3）；dh 30 道注册 g_b 与 case JSON `g_b` 字段逐一比对 **30/30 一致**（含 dh_drug_09 的 3 成员 G_B）；gt 均∈ case g_b |
| s_feasible_size | fixed **120/120** 落盘（读取契约首选键命中），无「等待 E5 落盘」项 |

### 9.2 满额重判结论（纯离线，重跑 judge，命中走判分器同一套）

| 侧 | 判定分布（case×模型） | 结论 |
|---|---|---|
| open（90 对） | 干净满 k 45（旧批 dh_drug 全量）+ 未采样 45（新增 dh_material，无样本不冒充） | **0 污染**；新增侧待采样补齐后复判 |
| fixed（360 对） | 干净 70（pass@10 全 0）+ 采样不完整 16 + 零样本 4（qwen × co_material_02/03/04/05，上轮计入不完整，本轮如实细分）+ 未采样 274（新增 90 道×3=270 + 旧批零样本 4） | **0 废弃**；R1-③ 口径不变（p_base=1/s_feasible_size，pass@10>2·p_base 严格大于） |

机器可读件：`bare_baseline/judgments/report.json`（新增 `batch_scope`/`open_unsampled`/`fixed_unsampled` 字段留痕）；污染清单两版已同步刷新（当前仍为空：open 0 命中、fixed 0 废弃）。

### 9.3 代码变更（全部限 `src/eval_matrix/`）

1. **名单扩容**：`FIXED_CASE_IDS` 30→120、`OPEN_CASE_IDS` 15→30（显式枚举；config 同步）；新增 `n_valid==0` →「未采样」如实标注态（open/fixed 两侧），不冒充干净、不冒充不完整；summary 新增 batch_scope/unsampled 计数。
2. **候选提取正则缺陷修复（本轮实测发现）**：旧式 `\[\d+\*\][^…\]]*` 会把多段组成串在后续 `]` 处截断（`[17*]8[24*]6[60*]1` → 两段残 token）致 resolve 漏判命中——对新增 dh_material 侧正向验证时实测暴露；改为完整匹配「[原子号*]计量」连续序列（支持小数计量如 `[22*]0.19`）。命中判定本身仍全由判分器 hit_gb 作出，本修正只关候选提取完整性、不涉判据松紧；修复后对旧批 1253 份样本全量复判，结论不变（0 污染/0 废弃）。
3. 单测：66/66（新增 2 条回归：组成串不截断、完整 token 送判分器；名单断言 120/30）。

### 9.4 新增 case 采样处置（交付要点 #3，未动付费）

**授权判定**：task-04 文档未明确授权消耗付费配额（文档头部口径：「只有正式跑实验矩阵时才换回付费模型」），且上轮监工挂起指令未解除 → **本轮未擅动付费配额**，新增侧改走 dogfooding 冒烟 + 缺口标注待授权。

| 项 | 结果 |
|---|---|
| 冒烟（dogfooding 免费档） | dh_material_01/02 × 2 份共 **4/4 OK**，单 call 113–227s，落盘独立目录 `bare_baseline_smoke/`；判定链生效（hit_judgment_active=true），2 道均「干净」（k=2 口径，仅验链路）；留痕 `bare_baseline_smoke_r2.log` |
| 命中正负向离线验证 | 正向：提交 c04592（dh_material_01 真实 G_B 成员）→ hit=true、∩G_B={c04592}；resolve∩G_B 补判：resolve('[17*]8[24*]6[60*]1')={c04592}；负向：非成员 c99999 → hit=false。全程走判分器，0 次网络调用 |
| 口径声明 | 冒烟结果不构成、也不冒充 3 付费模型对新增 105 道的污染判定 |

### 9.5 缺口与等待项（唯一挂起，更新版）

满额需 4500 份（150×3×10），已采 1253（全在旧批），**缺口 3247 = 旧批 97（qwen 91 + kimi 6，以样本文件 timestamp 实测为准）+ 新增 105 道 ×3×10 = 3150**。解除条件：监工授权付费档（注释 .env 的 `LLM_PROFILE=dogfooding` 行）→ `python3 -m src.eval_matrix.bare_baseline sample` 断点续跑（只补缺的，采样进程保持唯一）→ `judge` 复判自动刷新两版清单。本轮付费配额消耗：0；dogfooding 免费档消耗：4 次调用（单价 0）。PR-41 防护在位（本轮 35 条注册下未触发，单测覆盖不变）。H2 状态不变：等 E4 端到端跑通，不收口消融表。

