# E5 task-03 收口报告 — 药物侧 open 发现题 15 道

| 字段 | 值 |
|---|---|
| 任务 | task-03：造 30 道 open 发现题（ADR-0018 per-case A/B 配对），见 `task-03-造30道open发现题.md`；本收口覆盖**药物侧 15 道**（材料侧等 E1-05，见 §5） |
| 完成日期 | 2026-08-07（含 Task #11 根因修复与 09/14 重配、收口轮 view_ref 回填） |
| 配套文档 | 造题经验：`造case经验/task-03-open药物批.md`；进度与阻塞：`progress.md` |

**核心结论：药物侧 15 道 open case 全部过两条质量门并通过独立验证复检（5 项全过），可冻结；22 投造 / 15 过门 / 7 判废（31.8%，全部来自门 1，未放宽任何口径）；09/14 两道因可见集族泄漏返工重配（根因修复后保持原 gt、新增判废 0）；材料侧 15 道按任务书暂缓，等 E1-05 扩数据。**

---

## 1. 交付清单

| # | 交付物 | 路径 / 数量 |
|---|---|---|
| 1 | 药物侧 open case 15 道 | `dag_poc/templates/discovery_haystack/case_dh_drug_01~15.json`（新模板目录 discovery_haystack） |
| 2 | 造题脚本 | `src/case_build/build_open_drug_cases.py`（五步法 + 双质量门 + 预筛 + `--repair` 重配模式 + 写盘前断言） |
| 3 | 判废账本 | `src/case_build/open_drug_discard_ledger.json`（gate_discards 7 / prescreened_excluded 1018 / task11_repair 留痕） |
| 4 | 梯度三参数 | 每道 `gradient` 字段显式落盘：n=3~5、d=1~2、K=25（分布：n 3/4/5 = 6/4/5 道，d 1/2 = 8/7 道，方向 maximize 9 / minimize 6） |
| 5 | 可见集白名单 | 每道 `visible_set`（n+K=30 个实体 ID，gt 不在其中）；`view_ref` 已按 E1-05 规范回填（见 §4） |
| 6 | G_B 与同分异构记录 | 每道 `g_b` + `isomer_handling` 字段（|G_B|=1 十四道、=3 一道） |
| 7 | 质量门自检 | 每道 `quality_gates` 字段（门 1 穷举口径 + 门 2 对账记录）；判废数与原因见 §3 |
| 8 | 造题经验文档 | `造case经验/task-03-open药物批.md` |

## 2. §7 七项交付标准逐条对照

| # | 标准 | 结果 | 证据 |
|---|---|---|---|
| 1 | 30 道 open case（药 15 + 材 15） | ⚠️ 药物 15 道达标；材料 15 道按任务书 §5 暂缓（等 E1-05 扩数据） | 15 个文件齐备；case_id 语义中性（`case_dh_drug_NN`，无答案真名）；lint case 级违规 0 |
| 2 | 每道梯度参数 n/d/K 显式落盘 | ✅ 达标 | 每道 `gradient` 字段；取值均在 ADR-0018 §3 范围内（n 3~5 / d 1~2 / K=25 ≥ 硬下限 10） |
| 3 | 每道可见集白名单，走 E1-05 视图机制，A 中不含 gt | ✅ 达标（机制对接已就绪） | `visible_set` 落盘 + `view_ref` 回填 `per_case_views/case_dh_drug_NN.json`（per_case_view_v1 格式可直接表达）；gt ∉ visible_set 为写盘前断言项；视图文件本体待冻结验收后落盘（本收口边界不含写 `dag_poc/data/`） |
| 4 | G_B 落盘，含同分异构处置记录 | ✅ 达标 | 每道 `g_b` + `isomer_handling`（政策 ADR-0018 §8，全库组成普查口径） |
| 5 | 质量门两条自检记录，含判废数与原因 | ✅ 达标 | 每道 `quality_gates` 两门结论；判废 7 道全部门 1（K=25 三次换种子击穿，逐条记录击穿实体），另预筛确定性剔除 1018 配置另册留痕 |
| 6 | 造 case 经验文档 | ✅ 达标 | `造case经验/task-03-open药物批.md`：过门/判废分布、族泄漏坑、同分异构实战、type4 lint 差异、材料侧注意事项 |
| 7 | report 收口 + 状态行更新 | ✅ 达标（即本报告 + progress.md 更新） | — |

**lint 口径（批次 e5_task03，strict）**：case 级违规 0；corpus 级恰 1 条 M13（type4 drug=15/material=0，材料侧计划缺口，预期内）；pending 仅 A1 留痕（E5-01 已批口径）；M9a 开头两词 8 种/15 道 = 0.533（≥0.3）；M9b gt 全库唯一（与 task-02 的 30 个 gt 无重复，脚本断言）；M11 预算句逐字通过。view_ref 回填后重跑 lint 结论不变。

## 3. 判废与返工记录（如实报告）

### 3.1 判废：7 道（投造口径分母含判废）

- 投造 22 次 → 过门 15 → **判废 7（31.8%）**，与 query_spec 30% 先验一致。
- 判废原因**全部为质量门 1**：可见集内存在满足目标窗口（不劣于 gt，容差 δ=0.25）的实体；每道均 K=25 三次换种子复抽仍击穿才判废。
- **未为通过调小 K、未降 n、未改 δ。**
- 另有选格期确定性预筛剔除 1018 配置（段成员落窗 846 / 门 1 期望过门概率过低 102 / 同分异构确定性废弃 70），不消耗投造配额，账本另册留痕。

### 3.2 返工：2 道（case_dh_drug_09 / 14）

- 原因：首轮独立验证（13/15 全绿）查出两道**可见集族泄漏**——干扰池排除条件只含族代表元，同 count 同 backbone 实体混入可见集（09 多出 z013189、14 多出 z045676），n/K 落盘失真。
- 处置：根因修复（`same_backbone()` 全库组成判据剔除 + 写盘前断言 + 预筛分母同步）后 `--repair` 重配，**保持原 gt**（09：z000008，|G_B|=3；14：z043948，|G_B|=1），段成员不变仅干扰项重抽，一次过两门 + self_lint。
- **新增判废 0**；复检 5 项全过（同 backbone 数==n、可见集长==n+K、gt 不在可见集、同族序列严格单调、evidence 与当前库对账一致），15 道整体可冻结。

## 4. view_ref 回填（收口轮条件动作，已执行）

- E1-05 视图规范已正式定稿（`per_case_view_v1`，见 `task/E1-后端数据库构造/per-case视图交接说明.md` §2：字段表 + 载入期校验 + 示例 `e105_example_001.json`）。
- 本批 15 道的 `visible_set` **可直接按该格式表达**：`family_sequence` = evidence_set 的 n 个实体（沿外推轴有序）、`distractors` = visible_set 余下 K 个成员、gt 属 B 侧不入白名单。
- 已回填：15 道 `view_ref` = `dag_poc/data/backend/per_case_views/case_dh_drug_NN.json`，`view_ref_note` 同步更新为格式映射说明。
- 未做：视图文件本体（`per_case_views/*.json`）的落盘——属 `dag_poc/data/` 写操作，在本收口边界之外，随冻结验收 / G_B 入库步骤执行。
- 回填后重跑 lint（批次 e5_task03，strict）：结果与回填前一致（case 级违规 0 / corpus 恰 1 条 M13 / pending 仅 A1），lint 不消费 view_ref 字段。

## 5. 遗留与移交

| # | 事项 | 归属 / 状态 |
|---|---|---|
| 1 | **材料侧 15 道 open** | 等 E1-05 扩数据后开工；query_spec 配额 material=21 未动。**若 E1 扩完仍不足 15 道，报监工，不自行降 n / 降 K**（task-03 §5 明令） |
| 2 | **view_ref 视图文件本体落盘** | 随冻结验收执行（本次收口边界不含写 `dag_poc/data/`）；落盘后按 E1 交接说明 §5 复跑 `assert_per_case_view.py` |
| 3 | **edit_op 枚举不一致（E5-03）** | `answer_schema.json` operator 枚举是结构算子五件套，与 ADR-0018 组成算子不一致——归 E3，待监工裁决；本批 case 的 edit_op.operator 取最贴近枚举值（add_atom/remove_atom），组成语义写入 detail；E6 评分解析请以 detail 为准。**【Task #13 更新】** E3 在途 answer_schema 0.3.0 把 detail 文法冻结为 `<token>`/`<token>*<n>`，本批 15 道自由文本 detail 15/15 不匹配，两选项（E5 冻结轮回填 / E3 豁免构造期文件）报监工，E5 未自行选边 |
| 4 | **case_ref 口径（E5-04）** | `get_property_A.jsonl` 全行 case_ref=null；open 题 evidence 对账按 entity_id+prop+value 三元组，不依赖 case_ref；若 E1-05 视图交付后 case_ref 有正式语义再按新口径回填 |
| 5 | **G_B 冻结入库（E5-05）** | `ingest_open_gt.py` 把 15 道 gt 入 B 侧库——**待本批验收后执行**（入库后 view 的 gt.entity_ref 填 B 侧 ID）；适配清单见 progress.md E5-05（z-ID 反查→三字段→隔离闸→b_partition 注册→--dry-run），归属待监工裁定 |
| 6 | **裸答基线（E6）** | 待跑：不给 tools 直接问模型，任一次命中 G_B 的 case 废弃——**预计会再废一批**，届时按账本口径如实补记 |
| 7 | **人工复核清单** | M2/M3/M10/N6 ×15 待送审（同 task-02 惯例）；M10 背书深审（≥30% 抽样）归 owner |
| 8 | **E5-01 strict 退出码口径** | 维持已批达标线（违规 0 + pending 仅 A1），待监工对 lint 修法的最终裁决 |

## 6. Task #13 三维评审修复记录（2026-08-07）

完整性/正确性/影响面三维独立评审意见合并修复，8 项清单全数落地，如实留痕。

### 6.1 必修：9 道 gate2 note 单调方向定向修正

- 根因：`build_open_drug_cases.py` 旧 L386 `slope_txt = "decreases" if direction == "min" else "increases"` 把**优化方向**当成**趋势方向**。本批 9 道 maximize 题全是低端外推位形（remove_atom），段内 logS 随计数递减，note 却写 "increases"，与同一记录列出的数值自相矛盾。
- 修复①（脚本）：slope_txt 改由实际证据值推导——按计数升序的 evidence 首尾比较（`evidence[0] > evidence[-1]` → decreases）。
- 修复②（存量）：02/03/06/09/10/12/13/14/15 九道文件仅定向替换 `quality_gates.gate2_clue_reachable` 的“单调（increases）”→“单调（decreases）”，逐道先数值核验（首尾值比较）再替换，其余字段一律未动、未重生成整道。
- 自验：9/9 note 方向与数值一致（首尾值均递减）。

### 6.2 应修：脚本四项加固（build_open_drug_cases.py）

| # | 问题 | 修复 |
|---|---|---|
| 1 | view_ref 非幂等（生成器硬编码 None + 过期 note；repair 不保留会打回 null） | 生成器按 E1-05 定稿直接产出 `view_ref = dag_poc/data/backend/per_case_views/<case_id>.json` 与新 note；repair_cases 写盘前从旧文件恢复 view_ref/view_ref_note，并加写盘断言防回退 |
| 2 | repair 多道时 existing_gts 快照过期（前一道换的新 gt 未并入 used，后一道可能重复选中，破 M9b） | 每道 repair 写盘后 `existing_gts.add(gt_id)` |
| 3 | 非 repair 全量分支整体覆盖账本（丢 task11_repair/task11_note 留痕） | 改读旧账本按键合并（gate_discards/prescreened_excluded 更新，其余键保留） |
| 4 | `--repair` 无 ID 参数时 IndexError 崩溃 | 入口加参数校验，报用法退出；`--spare` 同样校验数字参数 |

### 6.3 spare 冗余机制与补造

- 机制：spare 前缀命名使 lint 的 `case_*.json` glob 不扫入，不影响 M13 存量统计与冻结清单；用途：E6 裸答基线废弃冻结道时顶替。走同一套五步法与质量门（口径不变）；gt 排除全部存量 gt（含 task-02 的 30 个）。
- 补造结果（`--spare 3`，同目录存放）：

| 文件 | gt | 梯度 | 方向 | \|G_B\| |
|---|---|---|---|---|
| spare_dh_drug_01 | z043848 | n=3 d=2 K=25 | maximize | 1 |
| spare_dh_drug_02 | z004015 | n=5 d=1 K=25 | maximize | 3 |
| spare_dh_drug_03 | z001332 | n=3 d=1 K=25 | maximize | 1 |

- 补造判废 5 条入账本 `spare_discards` 段；账本另列 `spares`/`spares_note` 段留痕（账本按键合并机制首次生效，task11 留痕未丢）。

### 6.4 修复后自验（全过）

- 9 道逐道确认 note 方向与数值一致（脚本化核验：evidence 首尾值比较 == note 声明方向）。
- 15 道 lint 整批重跑（批次 `e5_task03`，strict）：**case 级违规 0**；corpus 恰 1 条 M13（drug=15/material=0）；pending 仅 A1。
- spare 不被扫入：目录共 18 文件，标准 glob `case_*.json` 恰 15 道；宽 glob `*.json` 探针实测 lint 会扫入 18 道（drug=18）——证明隔离仅靠命名前缀，冻结/入库环节勿用宽 glob。

### 6.5 登记联动

- progress.md 新增 E5-05（G_B 冻结入库适配清单：z-ID 反查→三字段→B∩A=∅ 隔离闸→b_partition 注册→--dry-run，归属待监工）；E5-03 更新（answer_schema 0.3.0 detail 文法 `<token>`/`<token>*<n>`，15/15 不匹配，两选项报监工不自选边）；task-03 任务书状态行同步更新。
