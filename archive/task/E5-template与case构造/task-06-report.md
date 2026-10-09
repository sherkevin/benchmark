# E5 task-06 收口报告 — 补齐 open 剩余 11 道（open 30/30，题集满 150）

| 字段 | 值 |
|---|---|
| 任务 | task-06：补齐 open 剩余 11 道，见 `task-06-补齐open剩余11道.md` |
| 执行 | 造题 Lee（Task #34）· 独立验证 Terry（Task #35）· 三维评审 Ray/Kim/Victor（Task #36）· 加固修复 Lee（Task #38）· 收口 Taylor（Task #37） |
| 完成日期 | 2026-08-08 |
| 配套文档 | 造题经验：`造case经验/task-06-open材料第二批.md`；进度与阻塞：`progress.md` |

**核心结论：材料侧 open 剩余 11 道全过（投 11 过 11 判废 0 预检剔除 0），open 19→30、题集满 150（fixed 120 + open 30）——goal.md §6 题量线全线闭合。本批为 E3 接口 B 交付、UCI 26 族解锁后的首批产题。质检链全绿：Task #35 十项复检全 PASS → 三维评审（Ray 完整性 / Kim 正确性 / Victor 影响面）→ Task #38 加固修复（产物 md5 零影响）。CHECKSUMS 刷新 37→48 件、--verify 退出码 0（Victor 致命项 1 闭合）；白名单七组一次性原子提交（见 §6）。遗留 7 项归监工转派（见 §5），其中 ingest 通道结构性阻塞（Victor 致命项 2）措辞严格照实、不存在"待 ingest 后自然回填"的路径。**

---

## 1. 总账与 11 道明细

### 1.1 总账

| 维度 | 明细 |
|---|---|
| 投造口径 | 投 11 过 11 **判废 0 预检剔除 0**（UCI 26 族成员全库同组成唯一，同组成普查预检无命中；预检剔除与判废分列口径保持） |
| 进度 | open 19 → **30**（药物 15 + 材料 15）；题集 fixed 120 + open 30 = **150 满** |
| 族源 | UCI 达标族 qualifying_families_uci（26 族，E3 §9 解锁，降级键全集复算通道从严 fail-closed）；11 道**族互异** |
| 梯度 | n=3~5 / d=1~2 / K=25（lint O1 严口径，未降 n 未降 K）；δ 默认 0.25，15 道收窄 0.05（只收窄不放宽，见 §1.2 注） |
| 唯一性 | 11 道 \|G_B\| 全 1、s_feasible_size 全 1；判定链不依赖降级键唯一性（降级键只可排除，E3 拦截不触发） |
| gt 查重 | 全语料 144 个已用 gt 全避开（fixed 120 + open 19 + spare 3 + legacy 示例宽扫） |

### 1.2 11 道明细

| case_id | 族（stats base 组成串） | gt | n | d | δ | 目标窗口 | grid_key |
|---|---|---|---|---|---|---|---|
| 05 | O-[33\*]1[34\*]0.05[39\*]2[65\*]2.95 | c10199 | 3 | 1 | 0.25 | tc ≥ 91.55 | [该串, 3, 5] |
| 06 | H-[22\*]1[33\*]1[39\*]2[65\*]3 | c10545 | 4 | 1 | 0.25 | tc ≤ 37.85 | [该串, 2, 5] |
| 07 | O-[39\*]1.45[60\*]1.25[65\*]3[73\*]0.3 | c08114 | 5 | 1 | 0.25 | tc ≥ 76.65 | [该串, 3, 5] |
| 08 | C-[1\*]0.05[24\*]0.95 | c12895 | 4 | 2 | 0.25 | tc ≥ 13.95 | [该串, 1, 3] |
| 09 | Tl-[54\*]1 | c16897 | 3 | 2 | 0.25 | tc ≤ 4.35 | [该串, 0, 2] |
| 10 | H-[25\*]1[34\*]1[60\*]1 | c06524 | 3 | 1 | 0.25 | tc ≥ 34.75 | [该串, 2, 4] |
| 11 | H-[20\*]0.2[82\*]0.8 | c15707 | 4 | 1 | 0.25 | tc ≥ 3.82 | [该串, 0, 3] |
| 12 | I-[22\*]8.21[32\*]2[65\*]2[73\*]1[85\*]2 | c12344 | 3 | 1 | 0.25 | tc ≤ 56.55 | [该串, 2, 6] |
| 13 | O-[32\*]0.4[39\*]1.6[65\*]3[67\*]0.75[73\*]0.25 | c08275 | 4 | 1 | 0.25 | tc ≥ 73.75 | [该串, 3, 6] |
| 14 | O-[32\*]1.5[33\*]1[39\*]0.5[65\*]2.8[70\*]0.2 | c07576 | 3 | 2 | 0.25 | tc ≤ 32.25 | [该串, 2, 6] |
| 15 | Sr-[17\*]3[85\*]2 | c12566 | 3 | 1 | **0.05** | tc ≥ 3.01 | [该串, 0, 3] |

注：① grid_key 三位形态 [family:str, tc_band:int, element_count:int]，表中"该串"= 族列组成串原值；② 15 道 δ 由 0.25 收窄至 0.05——该族顶端间距 0.19 < 0.25，窗口宽为设计参数、不在 n/d/K 硬约束内，**只收窄不放宽**（同 task-04 La 族 δ 收窄判例）；③ 方向 7 maximize / 4 minimize，方向词由证据值推导、与优化方向解耦；④ provenance 为 UCI 引文（UCI ML Repository #464 unique_m.csv，Hamidieh 2018，DOI 10.24432/C53P47），不再硬编码 3DSC DOI。

## 2. 监工裁定六条对账

| # | 裁定 | 落实情况 |
|---|---|---|
| 1 | 门 2 内联 | 保持 task-04 内联组成校验：anchor 一律实体 ID + edit_op∈封闭算子集 + 跳数 1 + evidence≥3 条对账 get_property_A.jsonl；11 道 gate2_clue_reachable 全落盘内联证明 |
| 2 | primary-only | 干扰项池保持 primary-only（池 5,771 条），可见集内降级键实体数 == 0 写盘硬断言；Task #20 收窄裁定延续 |
| 3 | grid_key 先落盘报 E2 | 11 道逐道落盘 grid_key（三位形态，family=stats base 组成串口径），供 E2 check_grid_uniqueness 跨批查撞格；**grid_key 契约 v1.0 暂不覆盖 open（G1 对 type4 跳过），open 口径入契约 v1.1 已列交接（§5-①）** |
| 4 | opener 扩容 | 新引入 3 种 opener（Characterize/Isolate/Unearth），存量 22 道已含 Uncover/Locate 等 8 种；open 全语料 33 道实测 **11 种/33 = 0.333 ≥ 0.3** |
| 5 | 不跑 freeze-batch | 本批未跑 `--freeze-batch`（监工统一安排，非冻结批次），lint 按 task-02 已批达标线执行（§4） |
| 6 | 判废纪律 | 预检剔除单列、不冒充判废；本批同组成普查预检剔除 0、道级判废 0，账本 `task06` 段如实留痕 |

## 3. 质检链与评审处置

| 环节 | 结论 |
|---|---|
| Task #35 独立验证 | **十项复检全 PASS**（逐道穷举复算 / 可见集断言 / g_b 注册回读 33/33 逐字一致 / 视图断言等） |
| 三维评审（Task #36） | Ray 完整性：文档缺件 W1/W2/S1/S2 —— 本轮全部补齐（经验文档、progress 段、任务书状态行、全量 lint 证据 §4）；Kim 正确性：T1 element_count 口径断言缺失 —— 采纳**方案②**（脚本侧断言加固），Task #38 落地；Victor 影响面：两致命项 —— ①CHECKSUMS 未刷新（本轮闭合，§6）②ingest 通道结构性阻塞（转交接 §5-③，措辞照实） |
| Task #38 加固修复 | `build_open_material_cases_task06.py` 补 element_count 口径断言；**11 道 case、11 份视图、b_partition 等全部产物 md5 零影响**（加固仅作用于脚本未来重跑防护，且脚本 fail-fast 拒重跑） |
| 视图断言 | assert_per_case_view.py --all：fails=[]，36 cases 覆盖本批材料 05~15（结果留痕 `src/data_build/_build_state/per_case_view_assert_result.json`，随本批提交） |

## 4. 全量 lint 证据（Ray-S2 补件，收口时点实跑）

- 命令：`python3 dag_poc/query_lint.py --strict --json --cases 'dag_poc/templates/discovery_haystack/case_dh_*.json'`
- 结果：**total_cases=30，violations=0，退出码 2**；pending=2：① M9 变体率校验未执行（当前非冻结批次，冻结时必跑）② A1 扫描范围依据留痕（A 侧返回值落盘即 backend/\*\_A.jsonl，ADR-0017）——**符合 task-02 已批达标线（violations 0 + pending 仅 M9 未执行/A1 留痕 → 退出码 2）**。
- 30 道构成：药物 15 + 材料 15（含本批 11 道），O1~O4 全绿、g_b 注册在案（b_partition.cases 35 条）。

## 5. 交接清单（7 项，归监工转派）

| # | 事项 | 归口 |
|---|---|---|
| 1 | **E2 契约 v1.1：grid_key open 口径入契约**。现状：契约 v1.0 不覆盖 open，G1 对 type4 跳过；实际防撞靠 M9b + 台账（used_gt 全语料口径）。open 30 道已逐道落盘 grid_key，入契约只差口径定稿 | E2 |
| 2 | **M13 spare 不对称待裁决**：drug spare 18（15+3）vs material spare 15（无 spare），冻结阻塞于此；材料侧 spare 是否补造待监工裁定 | 监工 |
| 3 | **ingest 通道结构性阻塞（Victor 致命项 2，措辞严格照实）**：新 11 道材料 gt **不在 task39 覆盖范围**；task39 实跑 **accepted=0**（药物 gt 被 B∩A 隔离闸合规拒收，已停报待裁定）；材料 gt 在 `check_molecule_exists_B` 的**双键列结构性不可承载**——二次 ingest 被 **E1-06 停报裁定（A/B/C）前置阻塞**。此为结构性阻塞，**不存在"待 ingest 后自然回填"的路径**，须等 E1-06 裁决 | E1/监工 |
| 4 | **E6 裸答污染门扩面**：OPEN_CASE_IDS / open_dh_glob 硬编码药物 15 道，材料 15 道（首批 4 + 本批 11）在门外——E5-07 事项扩大，须扩至全部材料道 | E6 |
| 5 | **E4-05 合跑时序**：agent_harness 的 kind 扩展未提交；合跑前 E4 须先提交，否则只有 gateway 单边防护 | E4 |
| 6 | **M9a 余量仅 0.033**（11/33=0.333 vs 阈值 0.3）：后续 dh 扩题必须同步加 opener，否则下一次扩题即穿线 | E5 下批 |
| 7 | **register_dh_gb.py 幂等语义已改为"一致则跳过"**：改存量条目须显式删键再重跑，直接改值会被幂等闸吞掉 | E5 备忘/E1 |

## 6. 提交纪律段

- **CHECKSUMS 刷新（硬性第一步）**：刷新前 `git diff` 确认 CHECKSUMS.json 无工作树改动（无 E1 在途未保存改动混入）；`python3 src/data_build/generate_checksums.py` 退出码 0，主清单 **37 → 48** 文件（90,592,294 bytes，新增 11 件全部为本批材料视图 per_case_views/case_dh_material_05~15.json，移除 0、md5 变更 0）；`--verify` **退出码 0**（主清单 48 文件 + 附录 2 交接产物 md5 全部一致）。
- **白名单七组一次性原子提交（32 文件）**：
  1. `dag_poc/templates/discovery_haystack/case_dh_material_05~15.json`（11 新）；
  2. `dag_poc/data/backend/per_case_views/case_dh_material_05~15.json`（11 新）；
  3. `dag_poc/mcp_mock_server/b_partition.json`（纯增量 +198 行，git diff --cached 复核**零删除**，24→35 条）；
  4. `src/case_build/`：build_open_material_cases_task06.py、register_dh_gb.py、open_material_discard_ledger.json（3 项）；
  5. `src/data_build/_build_state/per_case_view_assert_result.json`（核验确认为本批断言留痕：fails=[]、36 cases 含材料 05~15）；
  6. `task/E5-template与case构造/`：progress.md、task-06-补齐open剩余11道.md、task-06-report.md、造case经验/task-06-open材料第二批.md（4 项）；
  7. `dag_poc/data/backend/CHECKSUMS.json`（第一步刷新版）。
- **未裹挟核对（git diff --cached --stat 逐文件）**：E1 的 build_open_gt_task39.py / open_gt_task39.json / rejections_check_molecule_exists.jsonl / task/E1-\*、E4 的 agent_harness.py 与 task/E4-\* 与 traces、E6 的 src/eval_matrix/\* 与 task/E6-\* 与 samples、task7 submodule、.review_tmp/ 均留在工作树未提交。
- **commit：9077780**（master，32 files changed，+5002/−26），git show --stat 已逐条核验与白名单一致；hash 回填于本行所在微提交（文档回填判例，同 45ab792）
- commit message 已注明：**此后 CHECKSUMS 刷新须串行，E1-06 裁决落盘动作须在本刷新之后重跑**（CHECKSUMS.json 为 E1/E5 共享文件）。

## 7. 结论复述

open 30/30、题集满 150，质检链全绿，CHECKSUMS 闭合，白名单原子提交完成；遗留 7 项交接归监工转派，其中 ingest 结构性阻塞（§5-③）与 M13 spare 不对称（§5-②）为硬阻塞项，其余为扩面/时序/备忘类。
