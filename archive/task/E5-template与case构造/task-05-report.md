# E5 task-05 收口报告 — fixed 90 道缺口（30 → 120 达标）

| 字段 | 值 |
|---|---|
| 任务 | task-05：造 fixed 的 90 道缺口，见 `task-05-造fixed的90道缺口.md` |
| 执行 | 药物批（Task #25）· 材料批（Task #26）· 十项复检（Task #28）· 修复轮（Task #31）· 定向复检（Task #32）· 收口（Task #30） |
| 完成日期 | 2026-08-08 |
| 配套文档 | 造题经验：`造case经验/task-05-fixed90道.md`；进度与阻塞：`progress.md` |

**核心结论：fixed 推理类 30 → 120 达标（goal.md §6 题量表 fixed 项闭合）——投 90 过 90 废 0；质检链全过（Task #28 十项复检全 PASS → 三维评审 → Task #31 修复轮 → Task #32 定向复检七项全 PASS）。提交时序裁决（致命项 F1）：**E1 backend 扩量仍未提交**（get_property_A.jsonl HEAD 165,723 行 vs 工作树 240,664 行，+74,941 行在途），新 90 道 constraints 的 atom_count/element_count 可查性依赖该扩量，故**本批暂不提交**，白名单清单已就绪（见 §8），待 E1 backend 提交后随即可提交。**

---

## 1. 配比账（硬数字）

| 维度 | 明细 |
|---|---|
| 类型 | 类型1 约束满足 **+44**（case_cs_drug_09~30 ×22 + case_cs_material_09~30 ×22）；类型2 约束最优 **+46**（case_co_drug_08~30 ×23 + case_co_material_08~30 ×23） |
| 领域 | 药物 **45**（cs 22 + co 23）/ 材料 **45**（cs 22 + co 23） |
| 投造口径 | 投 90 过 90 **废 0**（判废如实口径：选格期预检剔除单列、不冒充判废） |
| 格位 | used_cells = **120**，零撞格（(template_type, target_domain, grid_key) 全集唯一，跨批次生效） |
| gt | used_gt = **124**（全语料口径：fixed 120 + open 药物 15/材料 4 与 spare 3 并入查重池；首跑两起事故的教训口径，见 §5） |

## 2. 三项关键机制

1. **best_is_unique 口径切换**：弃 task-02 时代的 `len(m)==1` 选格过滤，改为"格内最优解唯一"判定。容量依据：材料侧单成员格全库仅 **5 个**，沿用旧口径根本凑不出 45 道；切换后容量充足且判据更贴题义（约束最优解唯一 ≠ 格只有一个成员）。
2. **grid_key 契约 v1.0**：新 90 道逐题落盘 grid_key（E2-06 定契约，类型统一避免 [3,'3'] 陷阱）；旧 30 道**回填**（M 状态、纯增量 +135 行，只加元数据、不动题面与 gt，不影响 E6 采样）。
3. **结构判据回迁 constraints**（任务书 §5-⑤ 交付物）：新 90 道 atom_count ×45（药物）/ element_count ×45（材料）直接入 constraints——依赖 E1 已注册的正式可查性质（方向 A，见 E5-02）；**旧 30 道保持 domain_knowledge 文字判据写法不动**。两批差异说明：旧批落盘时性质未注册、只能走 dk（task-05 返工裁定）；新批注册就位后回迁，题面/判分链路更简洁；judge 侧对两批分别按 constraints ∧ dk 完整口径求值，取数差异已在经验文档登记。element_count 口径锚定 E1 注册行 = 互异匿名 label 数（实测 143 个 label 重复晶体与 token 口径分叉，旧 15 道材料不受影响）。

## 3. 质检链（全过）

| 环节 | 结论 |
|---|---|
| Task #28 十项复检 | **全 PASS**（含 120 道全量 lint、逐道独立复算） |
| 三维评审（完整性/正确性/影响面） | 发现项全数处置：38 道择优句补口径按 **a 案**落 domain_knowledge；数字更正 3 处；重跑保护改 **fail-fast**；影响面 S1 事实核查走**注释修正分支**（不动代码逻辑）；致命项 F1 = 提交时序依赖（见 §8 裁决） |
| Task #31 修复轮 | 评审发现项合并修复落地 |
| Task #32 定向复检 | **七项全 PASS** |

## 4. 遗留与交接（归监工转派）

| # | 事项 | 归口 |
|---|---|---|
| 1 | E6 bare_baseline 的 FIXED_CASE_IDS 与 config glob 扩至 **120 道**（污染检验覆盖面） | E6 |
| 2 | E2/E6 gate 3b 背书覆盖清单扩至 120 道 case_id | E2/E6 |
| 3 | M9a type2 余量薄（**19/60 = 0.317**，阈值 0.3），后续扩题优先补 type2 句式 | E5 下批 |
| 4 | agent_harness.run_case 旧排练通道只发 query 不带 dk——后续工具层拼 prompt 须统一走 `bare_baseline.build_prompt`（择优句可见性依赖此路径） | E4/E6 |
| 5 | E5-06 / E5-07 等既往事项现状不变（见 progress.md 阻塞条目） | 维持 |

## 5. 首跑两起事故留痕（均已修复，教训入库）

1. **药物批 gt 复用中止**：首跑出现跨类型复用 gt 的排格错误，当场中止；修复为跨批查重双保险（账本 run_history 留痕）。教训：**排格查重必须跨类型**（cs 与 co 共享同一 gt 池）。
2. **材料批 M9 撞 open 批 gt**：首跑撞 open 材料批 gt；修复为 used_gt 采用**全语料口径**（并入 open 药物 15 + 材料 4 + spare 3，选格池加 gt_free 预筛，账本 run_history 留痕）。教训：**gt 查重的分母是全语料，不是当前批次**。

## 6. 产物清单（工作树就绪，待提交）

- 90 道新题：`dag_poc/templates/constraint_satisfaction/case_cs_{drug,material}_{09..30}.json`（44）+ `constrained_optimization/case_co_{drug,material}_{08..30}.json`（46）
- 30 道旧题 grid_key 回填（M 状态，纯增量 +135 行）
- `src/case_build/` 九项：common.py、shared_text.py、build_drug_cases.py（M）+ backfill_grid_key.py、build_drug_cases_task05.py、build_material_cases_task05.py、task05_drug_ledger.json、task05_material_ledger.json、_evidence_m9_corpus_20260808.json
- 账本 run_history 段含两起事故留痕

## 7. 提交纪律段（第一步时序裁决结果，如实记录）

**裁决：本批暂不提交。**

- 依据（致命项 F1，Daniel 影响面评审）：新 90 道 constraints 的 atom_count/element_count 可查性依赖 E1 backend 扩量（get_property_A.jsonl +74,941 行、MANIFEST、CHECKSUMS、drug_admet_v1.json）。
- 实测（2026-08-08，Task #30 收口时点）：`git status -- dag_poc/data/backend/ dag_poc/properties/` 显示 5 个文件仍未提交（CHECKSUMS.json / MANIFEST.json / get_property/MANIFEST.json / get_property/get_property_A.jsonl / drug_admet_v1.json）；HEAD 版 get_property_A.jsonl **165,723** 行，工作树 **240,664** 行——扩量在 HEAD 为 0。
- 处置：按裁决规则**不提交、不裹挟 E1 文件、不按目录粗提交**；90 道与配套产物全部留工作树，待 E1 backend 提交后按下述白名单四组原子提交：
  1. 90 道新题（cs 44 + co 46）；
  2. 30 道旧题 grid_key 回填；
  3. src/case_build/ 九项；
  4. task/E5-template与case构造/ 文档三项（progress.md、任务书、造case经验/task-05-fixed90道.md，含本报告）。
- **严禁裹挟清单**：dag_poc/data/**、dag_poc/properties/**、src/data_build/**、src/eval_matrix/**、task/E1/E3/E6 文档、task7 submodule、rehearsal_logs、.review_tmp/。
- 风险提示：工作树产物在提交前存在被并行线 git 操作影响的可能，E1 backend 一旦提交应**立即**执行本白名单提交。

## 8. 结论复述

fixed 120 达标、质检链全过、两起事故留痕修复、交接清单 5 项归监工转派；**唯一未闭合项 = 提交**，阻塞于 E1 backend 在途扩量，白名单与提交顺序已就绪（§7）。

---

## 监工验收（E5-05，2026-08-08 实跑）

**结论：通过。fixed 120/120 达标，drift 修正的主动作闭环。**

| 项 | 实测 |
|---|---|
| fixed 题量 | ✅ **120/120**（类型1 60 + 类型2 60，配比精确） |
| 全量 lint | ✅ 120 道跑 `check_grid_uniqueness`（E2 新加）**退出码 0** |
| 合计进度 | 139/150 = **93%**（open 还缺 11 道，等 E3 接口 B） |

### 「废 0」我核了，是诚实的

前几批判废 31.8%，本批 0，差异大，我本来要查是不是放松了判据。查完结论：**你的口径是干净的**。

report §20 原文：「投 90 过 90 **废 0**（判废如实口径：**选格期预检剔除单列、不冒充判废**）」——你把预检剔除和判废分开记，没有把预检数字算进判废来让流程"显得严格"。前批 31.8% 是门 1 抽样的自然判废，本批因预检更充分而无道级判废，两者口径一致、不是标准变了。

### 一处你自己撞上并修好的坑，值得记

report §52：「**材料批 M9 撞 open 批 gt**：首跑撞 open 材料批 gt；修复为 `used_gt` 采用**全语料口径**（并入 open 药物 15 + 材料 4 + spare 3，选格池加 `gt_free` 预筛）。**教训：gt 查重的分母是全语料，不是当前批次。**」

这正是我在 §7 风险预判里写的"跨批次要查重"，你撞上了、修好了、还把教训提炼成一句可复用的规则。

### 一处裁定：撤销你的「暂不提交」时序判断

你按致命项 F1 判断"E1 backend 扩量未提交，故本批暂不提交，待 E1 提交后随即可提交"。

**这个判断我撤销，已改为立刻全量提交**（commit `5d987dd`，467 文件 / 515,062 行）。三条理由：

1. **PR-35 已经发生过一次**——`dag_poc/traces/` 被外部进程 git 还原，E4 两次落盘的交付物全丢。你这 90 道题加 E1 的 7.5 万行数据，悬在工作区的风险远高于"等 E1"；
2. **git 可以一次原子提交**，不需要分先后——"等上游先提交"在同一个仓库里没有实际收益；
3. 提交前我做了安全检查：`.env` 0 处、`config.json` 0 处、待提交文件无明文 key。

**你识别出"工作树与 HEAD 不一致"这件事是对的**（get_property_A 从 165,723 行涨到 240,664 行），只是处置方向应该是"报监工请求一次性提交"，而不是"自己按住不提交"。**资产悬在工作区的每一分钟都是风险敞口。**

