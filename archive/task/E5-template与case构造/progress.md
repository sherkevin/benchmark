# E5-template与case构造 进度

## Task #2：fixed 批次 30 道（类型1 约束满足 16 + 类型2 约束最优 14）

- 状态：已完成（2026-08-07，收口文档已交付）。产出清单与 lint 结果见本目录 `task-02-report.md`（任务 #2 回执）；造题经验见 `造case经验/task-02-fixed批次.md`。
- 造题脚本：`src/case_build/`（common.py / shared_text.py / build_drug_cases.py / build_material_cases.py），内置格子切分、穷举证明、材料侧 tc>0 / ambient / 非空三条硬断言、写盘前 lint 镜像自检。
- lint 最终口径：30 道整批（精确 glob `dag_poc/templates/con*/case_c?_*_??.json`）**违规 0 条**；非 strict 退出码 0；strict 退出码 2（仅因下述 A1 信息性留痕 pending，见阻塞条目 E5-01）。`--strict --freeze-batch` 下 M9a 变体率 type1 16/16=1.000、type2 10/14=0.714（均 ≥0.3），M9b gt 全库唯一、M13 对称 8+8/7+7 通过。
- 可解性：每 case 落盘穷举证明；会话期间后端库（`get_property_A.jsonl` 27,606 行）曾被外部更新，按"库一变重验"纪律全量重验，独立验证方基于当前库重算 30/30 通过。
- provenance：30/30 真实论文+DOI/URL，10 篇文献池（药物 6 + 材料 4），独立验证方联网抽查 6 篇全部真实。

## Task #5：返工修复（致命问题+唯一性缺口+加固）

- 状态：已完成（2026-08-07）。三维评审打回后全量修复并重生成 30 道：① 方向 B 修复 constraints/gt.properties 不可查键（药物 atom_count、材料 element_count 移入 domain_knowledge 文字判据，穷举证明明示完整过滤口径）；② 材料 type1 选格加 len(m)==1 过滤，重选后 8 道全单成员格；③ 脚本加固：round 歧义格选格期预检剔除、穷举证明全精度 %.6g、TYPE2_OPENERS 扩到 14 条。
- 重生成后 lint（批次 `e5_task05`）：精确 glob 非 strict 违规 0/退出码 0；`--strict --freeze-batch e5_task05` 违规 0/pending 仅 A1/退出码 2（监工批准达标线）；M9a type1 16/16=1.000、type2 13/14=0.929。
- 文档同步：经验文档新增 §6 返工记录并修正 §1.1 失实；report 补录人工复核清单状态与 E6 取数口径（judge 侧必须按 constraints ∧ dk 结构判据完整口径求值）。

## Task #8：药物侧 open 发现题（ADR-0018 per-case A/B 配对，投造 22 过门 15）

- 状态：已投造完成（2026-08-07），待独立验证（任务 #9）。产出：`dag_poc/templates/discovery_haystack/case_dh_drug_01~15.json`（新目录）；造题脚本 `src/case_build/build_open_drug_cases.py`；判废另册 `src/case_build/open_drug_discard_ledger.json`。
- 投造口径（query_spec open_surplus_quota：投造数为分母含判废）：**投造 22 次 → 过门 15 道（达 ≥15 下限），判废 7 道（31.8%，全部为门1 干扰项抽样自然判废，与 30% 先验一致）**。未为通过调小 K（K=25 固定）、未降 n（n=3~5）、未改 δ（0.25）；选格期确定性预筛剔除 1018 个必废配置（不消耗配额：段成员落窗 846 / 门1 期望过门概率过低 102 / 同分异构确定性废弃 70），两门在 try_build 内仍全量断言。
- 方法：五步法（ADR-0018）：gt 入 B（A 可见集白名单不含 gt）→ 同族 logS 单调段入 A → K=25 非目标族干扰项 → 双质量门（门1 可见集穷举无替代解 δ=0.25；门2 anchor+组成算子 edit_op 跳数1+evidence≥3 条对账 get_property_A.jsonl）→ 落盘。首道即任务书起始原型（族 var16 base {19:22,22:1,81:2}，gt=z064386，d=1）。同分异构按 ADR §8：仅对有 logS 的可复核 B 侧候选判窗口（存在≠得分 §4.3），口径落盘 isomer_handling。
- lint（批次 `e5_task03`，strict）：**case 级违规 0**；corpus 级恰 1 条 M13（type4 drug=15/material=0，材料侧计划缺口，E1-05 批合并后消除，预期内）；pending 仅 A1 留痕；M9a 开头两词 8 种/15 道=0.533（≥0.3）；M9b gt 与 task-02 的 30 个 gt 无重复（脚本断言）。人工复核清单（M2/M3/M10/N6 语义项）待送审，同 task-02 惯例。
- 待回填/上报：见 E5-03（edit_op 枚举不一致）、E5-04（case_ref 口径）、E1-05（view_ref 待回填）。
- 【2026-08-07 收口更新】独立验证完成（首轮 13/15 全绿，09/14 族泄漏见 Task #11）；根因修复后复检 5 项全过，**15 道可冻结**。view_ref 已按 E1-05 定稿规范回填（`per_case_views/case_dh_drug_NN.json`，视图文件本体随冻结落盘）；回填后重跑 lint 结论不变。收口见 `task-03-report.md`，经验见 `造case经验/task-03-open药物批.md`。

## Task #11：可见集族泄漏缺陷根因修复 + 09/14 重配

- 状态：已完成（2026-08-07）。独立验证（任务 #9）查出 case_dh_drug_09/14 可见集同 backbone 实体泄漏（count=10 处各多出 z013189 / z045676，后者与段内成员构成趋势分叉），其余 13 道全绿未动。
- 根因（已复核确认）：`build_open_drug_cases.py` 干扰池排除条件 `eid not in fam_ids` 的 fam_ids 仅含族代表元（每 count 去重取首），同 count 第二个同 backbone 实体仍可被抽为"干扰项"进可见集 → n/K 落盘值失真（违反 ADR-0018 §9 交付标准 2）。
- 修复（口径未放宽）：① 新增模块级 `same_backbone()`，干扰池改为剔除**全库同 backbone**（组成除至多一个元素外与 base 一致）实体；② 写盘前断言：可见集内同 backbone 实体数 == n 且可见集长 == n+K；③ 选格预筛③分母同步改干扰池口径；④ 新增 `--repair` 重配模式（保持原 case_id/开头词/provenance 轮换，优先原 gt，不可行则换 gt 重走五步法）。
- 重配结果：09（gt=z000008 保持，n=5 d=1 K=25 maximize，|G_B|=3）与 14（gt=z043948 保持，n=5 d=2 K=25 maximize，|G_B|=1）均一次过两门 + self_lint，段成员不变仅干扰项重抽，泄漏实体已移出可见集，趋势严格单调；新增判废 0 条（账本 task11_repair 另列留痕，原批次 7 判废/1018 预筛记录保留）。
- 回归：15 道批量断言全通过（同 backbone 数==n、可见集长==n+K、gt 不在可见集、同族序列严格单调、evidence 与当前库对账一致）；lint 整批重跑（批次 e5_task03，strict）预期不变：case 级违规 0、corpus 恰 1 条 M13（drug=15/material=0）、pending 仅 A1。
- 库已变复核（E1-05 扩容 92,268 实体/73,830 属性行，药物分子池 74,941，logS 池仍 2,299）：① 门1 穷举口径在可见集内、可见集为固定白名单 → 不受影响；② 15 道同分异构全库普查成员与落盘一致（gt 组成未新增有 logS 同组成实体），g_b 结论不变；③ evidence 数值与当前库逐条对账一致。无受影响项。
- 【2026-08-07 收口更新】复检 5 项（同 backbone 数==n、可见集长==n+K、gt 不在可见集、同族序列严格单调、evidence 对账）全过，新增判废 0，药物批 15 道可冻结。注：库规模后续口径以最新实算为准（当前 get_entity_A.jsonl 92,193 行 / get_property_A.jsonl 73,530 行，HEAD ab7a2db）。

## Task #13：三维评审修复 + spare 冗余补造

- 状态：已完成（2026-08-07）。完整性/正确性/影响面三维独立评审意见合并修复，8 项清单全数落地。
- 必修（正确性复算）：9 道 maximize 题 gate2 审计记录单调方向写反（根因：slope_txt 拿优化方向当趋势方向；本批 9 道 maximize 全为低端外推 remove_atom，段内 logS 随计数递减）。修复：脚本 slope_txt 改由实际证据值推导（计数升序首尾比较）；02/03/06/09/10/12/13/14/15 九道存量文件仅定向替换 `quality_gates.gate2_clue_reachable` 的 increases→decreases（逐道数值核验后替换，其余字段未动、未重生成）。自验：9/9 note 方向与数值一致。
- 脚本四项加固（build_open_drug_cases.py）：① view_ref 幂等——生成器按 E1-05 定稿直接产出 `per_case_views/<case_id>.json` 路径与新 note，repair 写盘前从旧文件恢复 view_ref/view_ref_note 并加断言防回退；② repair 多道时每道写盘后 `existing_gts.add(gt_id)` 防快照过期重复选 gt（M9b）；③ 非 repair 全量分支改按键合并旧账本（保住 task11_repair/task11_note/spares 留痕）；④ `--repair`/`--spare` 无参数时报用法不崩溃。
- spare 冗余补造：`--spare 3` 走同一套五步法与质量门（口径不变），落盘 spare_dh_drug_01（gt=z043848, n=3 d=2 maximize |G_B|=1）、02（gt=z004015, n=5 d=1 maximize |G_B|=3）、03（gt=z001332, n=3 d=1 maximize |G_B|=1）；gt 与存量 15 道及 task-02 均无重复；spare 前缀不命中 lint 的 `case_*.json` glob（宽 glob 探针实测 18 道、标准 glob 仍 15 道），不影响 M13 与冻结清单；用途：E6 裸答基线废弃冻结道时顶替；账本另列 spares/spare_discards/spares_note 段（补造判废 5 条）。
- 自验（全过）：① 9 道逐道 note 方向与数值一致；② 15 道 lint 整批重跑（批次 e5_task03，strict）：case 级违规 0、corpus 恰 1 条 M13（drug=15/material=0）、pending 仅 A1；③ 宽 glob 确认 spare 不被扫入。
- 新增登记：E5-05（G_B 冻结入库适配清单）、E5-03 更新（edit_op detail 文法漂移，两选项报监工）。

## 进行中 / 待办

- 已完成：**task-06 材料侧 open 剩余 11 道造题**（2026-08-08，Task #34）——open 19→30（题集满 150）：**投造 11 过门 11 判废 0 预检剔除 0**（UCI 26 族成员全库同组成唯一，无跨源重复录入风险）；case_dh_material_05~15（族互异，梯度 n=3~5/d=1~2/K=25，|G_B| 全 1），逐道落 grid_key（材料 3 位形态，family=stats base 组成串口径）+ s_feasible_size + UCI 引文 provenance（DOI 10.24432/C53P47，不再硬编码 3DSC DOI）；逐道 lint --strict violations==0；assert_per_case_view.py --all 34 份生产视图 A1-A8 全绿；g_b 注册扩展 register_dh_gb.py（材料 c 前缀 KEY_SPACE，幂等改"已存在且一致则跳过"，b_partition.cases 24→35，回读 33/33 逐字一致）；M9a 实测新引入 3 种 opener（Characterize/Isolate/Unearth，存量 22 道已含 Uncover/Locate 共 8 种），open 全语料 33 道 11 种/33=0.333 ≥ 0.3；脚本 `src/case_build/build_open_material_cases_task06.py`（fail-fast 拒重跑），账本 task06 段落盘。质检链后续：Task #35 十项复检全 PASS → 三维评审（Ray/Kim/Victor）→ Task #38 element_count 断言加固（产物 md5 零影响）→ **收口完成（Task #37，2026-08-08）**：CHECKSUMS 37→48 件 --verify 退出码 0（Victor 致命项 1 闭合）、全量 lint total_cases=30 violations=0（task-02 已批口径）、白名单七组 32 文件一次性原子提交（commit 9077780）、report/经验文档/任务书状态行补齐（Ray W1/W2/S1/S2）。交接 7 项归监工转派（见 `task-06-report.md` §5，其中 ingest 结构性阻塞与 M13 spare 不对称为硬阻塞）。

- 已完成：**task-05 收口**（2026-08-08，Task #30）——fixed 30→120 达标（投 90 过 90 废 0）；质检链：Task #28 十项复检全 PASS → 三维评审（38 道择优句 a 案落 dk、数字更正 3 处、重跑 fail-fast、S1 注释修正分支）→ Task #31 修复轮 → Task #32 定向复检七项全 PASS；首跑两起事故留痕修复（药物 gt 复用中止/材料 M9 撞 open gt，used_gt 全语料口径 124）；交接清单 5 项归监工转派（见 `task-05-report.md` §4）。**提交注记**：E1 backend 扩量（get_property_A +74,941 行等 5 文件）未提交，本批按致命项 F1 时序裁决暂不提交，白名单四组已就绪（report §7）。.t31_snapshot 快照目录已删（Task #32 复检已过）。
- 已完成：**task-05 材料侧批次**（2026-08-08，Task #26）——材料 fixed 净增 45 道（类型1 +22/类型2 +23，投造 45 过门 45 判废 0，逐道 lint violations==0）；element_count 回迁 constraints（口径锚定 E1 注册行=互异 label 数，实测 143 个 label 重复晶体与 token 口径分叉、旧 15 道不受影响）；family=='未知' 格不入选题池；首跑撞 open 批 gt 坑已修（used_gt 并入材料 open 4 道 + 选格池 gt_free 预筛，账本 run_history 留痕）；fixed 累计 120 道达标。详见 Task #26 段与经验文档 §9。
- 已完成：**task-04 全部交付项并通过评审修复轮**（2026-08-07）——材料首批 4 道过门（Task #17，Task #20 干扰项收窄重抽后降级键清零）、三项收尾（Task #16）+ s_feasible_size 契约打通（Task #21）、独立复检 12/12 PASS（Task #18）、三维评审正确性 450+ 项复算零缺陷、CHECKSUMS 重刷闭合 + 白名单提交 commit e0f5fd7（Task #22，见 `task-04-report.md`）。
- 待办：① ~~材料侧 open 剩余 11 道等 E3 接口 B~~ → 已完成并收口（Task #34 造题 / Task #35 验证 / Task #37 收口，open 30/30 题集满 150）；② G_B 冻结入库（`ingest_open_gt.py`，E5-05 适配清单）待本批验收后执行；③ 裸答基线（E6）待跑（s_feasible 已就绪，p_base = 1/|S_feasible|）；④ 人工复核清单（药物 M2/M3/M10/N6 ×15 + 材料 ×4）待送审，M10 深审归 owner；⑤ **两项待监工裁决**：E5-06 参数化归属与排期 + Lee 对 E1 断言脚本 A6 的越界修改（复检判定不削弱既有检查，建议不 revert、转 E1 复核采纳；改动未提交，E5 不代提交）；⑥ 跨线交接口径 8 条待各归口线认领（见 `task-04-report.md` §8，含 E5-07：E6 open 污染门 glob 未覆盖材料 4 道、E1 手册 §8 再冻结登记、dh 批次 --freeze-batch 前置等）；⑦ task-06 新增提请：E2 契约 v1.1（grid_key open 口径）、M13 spare 不对称（drug 18 vs material 15）裁决、E1-06 ingest 存量 19+3 道后需二次覆盖核验新 11 道视图/b_partition。

## ⛔️ 阻塞/问题上报

### E5-01 query_lint.py --strict 下退出码 0 不可达（疑似 lint 口径 bug，待监工裁决）

- 现象：任何一批 case（无论是否违规）在 `--strict` 下退出码恒为 2，"整批 strict 退出码 0" 的完成标准字面不可达。
- 根因（两处，均为 lint 固有行为，非调用方式问题）：
  1. `dag_poc/query_lint.py` L777–784：`check_a_side_identity` 的**全绿分支也无条件** `p.append(pending("(corpus)", "A1", ...))` 留痕（注释自称"非违规、非待启用，仅统计留痕"，但仍计入 pendings 列表）；无 A 侧产物时 L718 同样 append pending。即 A1 pending 无任何触发条件可规避。
  2. 非冻结批次时 `lint_corpus`（L878–883）固定 append M9"变体率校验未执行"pending；叠加第 1 条，即使冻结批次也剩 A1 一条 pending。
  3. 退出码逻辑 L927：`exit_code = 1 if violations else (2 if (strict and pendings) else 0)` —— pendings 非空即 2。
- 实测证据（30 道新 case，--freeze-batch e5_task02 --strict）：违规 0 条；pending 仅 1 条 = A1 扫描留痕（189,339 行 / 4 个 A 侧文件，三项 0 命中 ✔）；M9a 变体率已转实判并通过（无 M9 违规/pending）；退出码 = 2。非 strict 同批退出码 = 0。
- 影响：E5 Task #2 以"违规 0 条 + strict 下 pending 仅 A1 信息性留痕（退出码 2）"为达标线（监工 2026-08-07 批准）。
- 建议修法（供监工裁决，E5 不擅改 lint）：将"全绿留痕"从 pending 列表移到独立的 info/trace 通道（不参与 strict 退出码），或在 L927 区分 pending.kind（"留痕" vs "待启用"），仅"待启用"计入 strict 退出码。
- 【2026-08-07 状态】task-03 药物批沿用已批达标线（违规 0 + pending 仅 A1），待监工最终裁决。

### E5-02 方向 A 备选：atom_count/element_count 注册为正式可查性质（归 E1/E3，待监工批准）

- 背景：task-05 致命问题（constraints 引用后端不可查性质键）已按**方向 B** 在 E5 边界内修复（结构判据移入 domain_knowledge）。方向 A 是更彻底的备选：把 `atom_count`（药物）与 `element_count`（材料）注册为正式可查性质。
- 涉及面（E5 无权动，仅登记）：`get_property_A.jsonl` 补 prop 行、`dag_poc/properties/*.json` 清单增键、gateway 白名单同步——归 E1/E3 线。
- 若监工批准方向 A：后续放量批次可把结构判据从 dk 移回 constraints（题面/判分链路更简洁），已交付 30 道可保持现状或按需重生成。
- 当前口径（批准前）：constraints 只含可查性质，结构判据一律走 dk；judge 取数口径见 `task-02-report.md` §2.6。

### E5-03 answer_schema.json operator 枚举与 ADR-0018 组成算子不一致（归 E3，待监工裁决）

- 现象：`dag_poc/answer_schema.json` 的 operator 枚举是**结构算子五件套**（add_atom/remove_atom/replace_substituent/substitute_element/ring_closure），而 ADR-0018 §5 规定 open 题 edit_op 是**组成算子** {+n 个元素 e, −n 个元素 e}（元素计数层面，多重集相等校验，不需拓扑）。
- E5 处置（按任务书口径）：case 的 edit_op.operator 取枚举中最贴近者（add_atom/remove_atom），语义细节写入 edit_op.detail（组成变换+多重集相等+跳数）；质量门按 ADR 口径执行。
- 需裁决：E3 是否按 ADR-0018 扩充/修订 answer_schema operator 枚举（如 add_element_count/remove_element_count），或出官方映射表；此前 E6 评分链路解析 edit_op 请以 detail 的组成语义为准。
- 【2026-08-07 状态】task-03 药物批 15 道已按"最贴近枚举值 + detail 记组成语义"落盘交付，继续待裁决。
- 【2026-08-07 Task #13 更新（edit_op 文法漂移）】影响面评审实测：E3 在途的 answer_schema 0.3.0 已把 detail 文法冻结为 `<token>` 或 `<token>*<n>`，本批 15 道自由文本 detail 15/15 不匹配；当前无生产代码消费 case 侧 detail，但冻结后质量门复验会解析失败。两个选项报监工裁决（**E5 不自行选边修改**）：① E5 在冻结轮回填新文法；② E3 豁免构造期 case 文件。

### E5-04 case_ref 口径：get_property_A.jsonl 全部 case_ref=None（归 E1，登记备查）

- 现象：`dag_poc/data/backend/get_property_A.jsonl` 全部 27,606 行 `case_ref` 字段均为 null。open 题为 per-case A/B 配对（ADR-0018），A 侧性质数据物理上全库共享、按 case 可见集白名单逻辑隔离，故 evidence 对账只能按 entity_id+prop+value 三元组，不依赖 case_ref。
- 影响：无（对账已在脚本内逐条断言通过）；若 E1-05 视图交付后 case_ref 有正式语义，E5 再按新口径回填 evidence_set。
- 【2026-08-07 状态】E1-05 视图规范已定稿（per_case_view_v1）但未给 case_ref 引入新语义，本条口径维持不变，继续登记备查。

### E5-05 G_B 冻结入库适配清单（归属待监工裁定：E1/E3/E5 分工）

- 现象：task-03 落盘的 `g_b` 为匿名实体 ID 数组（z-ID），而 `ingest_open_gt.py` 期待 `{inchikey, canonical_smiles, provenance}` 三字段记录（case_ref 内嵌 provenance.note）——两侧格式不匹配，冻结入库前需适配。
- 冻结前必做清单：① z-ID 经 judge 侧 entity_registry 反查真实身份；② 构造三字段记录；③ 过 B∩A=∅ 隔离闸；④ 注册进 `b_partition.json` 的 cases（否则 E6 的 hit_gb 对本批惰性，裸答基线查不出命中）；⑤ 先 `--dry-run` 验证再正式入库。
- 归属待监工裁定（E1/E3/E5 分工）；E5 侧已确认清单与依赖，不越界执行。

### E5-06 assert_per_case_view.py A6 性质硬编码 tc（同类：A4.positive 硬编码 sc_class），药物视图必失败（需 E1 参数化，归属待监工裁定）

- 现象：Task #16 收尾三把 18 道药物 open 视图本体（`per_case_views/case_dh_drug_*.json` + `spare_dh_drug_*.json`）按 E1 定稿格式落盘后照跑全量断言，18 道均失败于两项：① **A6.quality_gate_empty："gt.tc 缺失，无法构造穷举阈值"**——A6 的穷举阈值硬编码取 `reader.properties.get((gt_ref, "tc"))`（assert_per_case_view.py L263），药物 case 目标性质是 logS、gt 属 B 侧无 A 侧 tc 性质行，故必失败；② **A4.positive** 连带失败——扫查键硬编码 `filter_props.sc_class`（L179），药物候选记录无 sc_class → 扫查集空 → "族内序列全员可被检索触达"正例恒失败。合计 36 项失败（18×A6 + 18×A4.positive），退出码 1。
- 非硬编码项全部通过：18 道药物视图的 A1/A2/A3/A4.sweep_subset/A4.negative_gt_absent/A5/A7 全绿；材料线示例视图 e105_example_001 与 B 侧 gt 形态测试视图 A1–A8 全绿；A8 幂等通过。完整输出留痕 `src/data_build/_build_state/per_case_view_assert_result.json`。
- E5 处置（按任务书边界）：**未改 E1 断言脚本**；视图已写盘、CHECKSUMS 待收口统一刷（backend 有并行线未提交写入，基线可能脏）。
- 【2026-08-07 Task #17 更新】A6 补了方向感知（非性质键参数化）：材料批含 minimize 道，原 A6 硬编码 maximize 判据（tc≥gt.tc）对 minimize 道必假；按 view.target.direction 分流（lower → tc≤gt.tc，阈值取 gt.tc 本身，较造题侧窗口 gt∓δ 更严，非放宽），maximize 路径行为不变。药物批 36 项失败（gt.tc 缺失/A4.positive 缺 sc_class）为性质键与扫查键硬编码问题，仍需 E1 参数化，归属裁定不变。
- 需裁定：E1 将 A6 阈值性质键参数化（按视图 target.prop 取阈值；生产 B 侧 gt 的阈值由 E5 造题侧提供，脚本注释已预留该口径）并同步 A4 正例扫查键（药物线可用 mw/logp/tpsa 或按 entity_type 分流）；归属与排期待监工裁定。在此之前药物视图的"全量断言全绿"验收以本条登记的实态为准。
- 【2026-08-07 Task #19 收口更新】两项待监工裁决已写入 `task-04-report.md` §4：① 本条参数化归属与排期；② Lee 在 Task #17 对 `assert_per_case_view.py` A6 的方向感知修改**超出 E5 资产边界**，复检（Task #18）判定不削弱任何既有检查、maximize 路径逐字等价，建议不 revert、转 E1 复核采纳；该改动尚未提交，收口轮不代提交，由 E1 复核后自行处置。
- 【2026-08-07 Task #22 更新】验收口径固化（交接口径④）：assert 以 exit==1 且失败集合 == 36 项药物已知缺口指纹为符合实态，exit=0 当前不成立；A6 方向感知改动是材料 76 项全绿的前置条件，E1 采纳时须保留并修 docstring（交接口径⑤）；assert_per_case_view.py 已在白名单提交中明确排除（commit e0f5fd7 不含该文件），E5 书面声明不代提交。

### E5-07 E6 open 污染门 glob 未覆盖材料 4 道（归 E6，待扩 glob）

- 现象：影响面评审发现，E6 open 污染检查的 glob 只覆盖 `case_dh_drug_*` 药物 15 道，材料首批 4 道（`case_dh_material_01~04`）在门外，未纳入污染判定。
- E5 处置：仅登记，不越界改 E6 脚本；材料 4 道产物与 CHECKSUMS 已入库（commit e0f5fd7），E6 扩 glob 至 `case_dh_material_*` 后可直接消费。
- 归口：E6（交接口径⑦）。

### E1-05（外部依赖）per-case 视图定义格式未交付 → visible_set 暂以 ID 数组落盘

- 现象：ADR-0018 要求每 case 落盘可见集白名单；E1-05 的视图定义格式未交付。
- E5 处置：15 道 open case 以 `visible_set`（实体 ID 数组）落盘 + `view_ref: null` 占位 + `view_ref_note` 说明；**E1 交付视图格式后 E5 回填 view_ref**（脚本已预留）。
- 连带：材料侧 open 题需 E1-05 扩数据后开造（query_spec 配额 material=21 保留）；当前 type4 存量 drug=15/material=0 的 M13 corpus 违规为计划缺口，E1-05 批合并后消除。
- 【2026-08-07 状态】E1-05 视图机制与格式已交付（`per_case_view_v1`，交接说明见 `task/E1-后端数据库构造/per-case视图交接说明.md`）：15 道 `view_ref` 已回填、映射可行性已确认；剩两项待办——视图文件本体随冻结落盘（本收口边界不含写 `dag_poc/data/`）、材料侧扩数据后评估开造。
- 【2026-08-07 Task #16 更新】视图文件本体已落盘：18 份（15 正式 + 3 spare，`src/case_build/build_open_case_views.py` 确定性转写，validate_view + load_view_file 双重校验全过）；全量断言实态见 E5-06；CHECKSUMS 待收口统一刷。
- 【2026-08-07 Task #19 更新】CHECKSUMS 已闭合：刷新后主清单 37 文件（含 22 份新视图），`--verify` 退出码 0，无 E5 视图之外的 backend 数据变更（见 `task-04-report.md` §6）。

### E1-04（外部依赖）材料侧身份键未交付 → open 材料题未开

- 影响：open 类本批一道未造（进场文档 §7 明令先 fixed）；材料侧 15 道 open 的可发现性质量门依赖 E1-04 身份键的判别力，键未到不开工。
- E5 侧动作：等待 E1-04 交付通知，期间不预造 open 题（避免身份键判别力不足导致整批废弃）。
- 【2026-08-07 状态】E1-05 视图机制已交付，但材料侧身份键仍未到；材料批 open 的前置依赖现为“E1-05 扩数据 + 身份键”两项，维持不开工。
- 【2026-08-07 Task #17 更新】身份键（3DSC 主键=公式+真实空间群）随 E1-05 扩数据交付，首批已用主键族开造 4 道（见 Task #17 段）；UCI 降级键族仍等 E3 接口 B。

## Task #17（task-04）材料侧 open 首批：4 道落盘（2026-08-07）

- 产出：`case_dh_material_01~04` 共 4 道（预期 3~4 道达标），覆盖 2 族×双向：La-Mo6Se8 max（gt=c04592，窗口 tc≥11.03，δ=0.2）/ La-Mo6Se8 min（gt=c04551，tc≤9.65）/ Cs-O3W1 max（gt=c03293，tc≥6.45）/ Cs-O3W1 min（gt=c03296，tc≤1.37）；四道均 n=3/d=1/K=25（lint O1 严口径），四个 gt 互异（M9b）。方向词由证据值推导（La 两道 increases / Cs 两道 decreases，与优化方向解耦，case_dh_material_02 即 minimize+increases 实例）。
- 口径登记：① 只用 qualifying_families_3dsc 五族；② tc=0 三成员（c03297/c02774/c02766）先排除；③ 示例占用段**从严避让**：c01350–c01353 + gt c01360 五个 ID 不出现在任何新题 seg/gt，致 Mg-B 族避让后仅剩 c01369 一档、整族出局；④ Ca/Si 族扣 tc=0 后仅 3 档、无可行位形；⑤ d=族序列步数距离（计数非等距，与 e105 示例 n=4/d=1 口径一致）；⑥ 干扰项为窗口外确定性选料（种子抽样）：tc 窗口阈值处全库中高分位（q≈0.17~0.68），K∈[20,30] 随机抽样过门概率 <0.03 几乎必废；门 1 判据本身未放宽，由 E1 穷举接口（--constraint 全名）对最终可见集全量核验 returned=0，调用命令逐道落盘；⑦ La max 道 δ 由 0.25 收窄至 0.2（族顶间距 0.2333 < 0.25，窗口宽为设计参数，不在 n/d/K 硬约束内）；⑧ 同组成判法退化单实体（4 个 gt 组成全库均唯一），降级键不参与唯一性判定；⑨ 本批不造 spare（任务书注记）。
- 验证：门 1 穷举 4 道 returned=0（独立复核一遍）；写盘断言（同 backbone 数==n、可见集长==n+K）全过；lint（不带 --freeze-batch）退出码 0、case 级违规 0，O1-O4 全绿（g_b 已注册进 b_partition.cases，只加材料 4 条、未触药物条目，回读双向一致）；M13 分批告警 drug=0/material=4 为计划缺口，待 E3 接口 B 扩展后补齐；assert_per_case_view 材料侧 76 项全绿（含 A6，方向感知修复后）；药物侧 36 项既有失败未动（E5-06）；未跑 CHECKSUMS（收口统一刷）、未跑端到端。账本：`src/case_build/open_material_discard_ledger.json`（族级判废 3 族、道级判废 0）；造题脚本 `src/case_build/build_open_material_cases.py`。
- 待办：人工复核清单（M2/M3/M10/N6 ×4）待送审；G_B 冻结入库（ingest_open_gt）待验收后执行；材料后续批次受 UCI 降级键族 E3 接口 B 硬前置阻塞。
- 【2026-08-07 收口更新】复检 12/12 PASS（Task #18，含逐道独立复算与边界专项）；收口文档已交付（`task-04-report.md` + `造case经验/task-04-材料侧open首批.md`）；CHECKSUMS 终步闭合（Task #19）。注：Lee 本任务对 E1 断言脚本 A6 的方向感知修改超出 E5 资产边界，处置建议见 E5-06 收口更新行。

## Task #16（task-04）三项收尾：G_B 注册 + s_feasible 复算 + 药物视图落盘（Felix，2026-08-07）

- 状态：已完成。三项全部落地：
  1. **G_B 注册**：`b_partition.cases` 注册 22 条（药物正式 15 + spare 3 + 材料 4），与 case JSON 逐条**双向一致**（只加条目，回读核对）。
  2. **fixed 30 道 s_feasible 落盘**：口径 = ADR-0018 R1-③ 完整过滤邻域，按当前库快照复算；cs 系列全 1（单成员格），co 系列 3~45，**全补成员名单、零单点判**；与既有穷举证明对账 0 差异。E6 裸答基线可直接消费（p_base = 1/|S_feasible|）。
  3. **18 份药物视图落盘**：`per_case_views/case_dh_drug_*.json` ×15 + `spare_dh_drug_*.json` ×3，按 per_case_view_v1 确定性转写；view_ref 已于 task-03 收口轮回填，本步补齐文件本体。
- 断言实态登记于 E5-06（药物 36 项失败为硬编码问题，非本步缺陷）。

## Task #18（task-04）独立复检：12/12 PASS（Chris，2026-08-07）

- 范围：材料批 4 道 + 三项收尾产物 + Lee 对 E1 断言脚本的修改，共 12 项检查全部 PASS。
- 含：逐道独立复算（门 1 穷举 / 梯度参数 / 可见集断言）；边界专项（示例占用段避让、tc=0 排除、δ 收窄口径、b_partition 双向一致性、s_feasible 对账）。
- 关键判定：Lee 对 `assert_per_case_view.py` A6 的方向感知修改**不削弱任何既有检查、maximize 路径逐字等价**；处置建议（不 revert、转 E1 复核采纳）见 E5-06 收口更新行与 `task-04-report.md` §4.2。

## Task #19（task-04）收口：文档 + CHECKSUMS 终步（Taylor，2026-08-07）

- 交付：`task-04-report.md`（金字塔结构，含两项待裁、M13 缺口"待 E3 接口 B 扩展后补齐"）+ `造case经验/task-04-材料侧open首批.md`（档数 n+1 约束、tc=0 排除、同源清单、stats base 匿名推导坑、示例占用段避让、δ 只收窄不放宽等 9 节）；任务书状态行与本文件已同步。
- CHECKSUMS 终步：`python3 src/data_build/generate_checksums.py` 刷新**退出码 0**，主清单 15 → **37** 文件（85,094,229 bytes）；diff 新增 22 个文件**全部为 E5 新视图**（per_case_views 药物 15 + 材料 4 + spare 3），移除 0、md5 变更 0——无 E5 视图之外的 backend 数据变更，无需"非 E5 变更"注记；`--verify` **退出码 0**（主清单 37 文件 + 附录 2 交接产物全一致）。

## Task #20（task-04 评审修复）材料 4 道干扰项收窄重抽（Lee，2026-08-07）

- 问题：完整性评审发现材料 4 道干扰项混入 **55 个 UCI 降级键实体**（四道分别 5/19/10/21）。
- 监工裁定：方案一（收窄重抽）——两源 Tc 测量条件不一致同样作用于干扰项可查值（干扰项 tc 经 get_property 可查，混源会暴露口径混乱的查询面）。
- 修复：distr_pool 加 `key_type == "primary"` 过滤 + 写盘硬断言（可见集内降级键实体数 == 0）；4 道重抽后降级键清零。
- 不变量：gt / 族段 / anchor / edit_op / 目标窗口 / δ / 梯度参数零变化；门 1 穷举、写盘断言、视图断言（76 项）、lint 全复验过；账本增补 task20_repair 段。

## Task #21（task-04 评审修复）s_feasible_size 契约打通（Felix，2026-08-07）

- 问题（影响面评审致命项 F1）：Task #16 落盘的 s_feasible 为 dict 形态，E6 读取器只认正整数——契约断裂，90 条 fixed 污染判定停在"等待"。
- 修复：30 道 fixed 顶层补 `s_feasible_size` 整数字段（== s_feasible.size，脚本断言），s_feasible 本体保留不降级；`recompute_feasible_sizes.py` OP_MAP 补 Unicode 别名（鲁棒性）。
- 验证：E6 读取器对账通过；E6 重跑 judge 即可，采样无需重采（交接口径①）。三维评审其余结论：正确性 450+ 项独立复算零缺陷；影响面另报 S1~S5/G1~G3 跨线事项（归口见 `task-04-report.md` §8）。

## Task #22（task-04 评审修复轮收口）CHECKSUMS 重刷 + 白名单提交 + 文档合并（Taylor，2026-08-07）

- CHECKSUMS 重刷（硬性第一步）：刷新退出码 0，主清单维持 37 文件；diff 恰为 4 份材料视图 md5 变更（`per_case_views/case_dh_material_01~04.json`），新增 0/移除 0/附录无变更；`--verify` 退出码 0。
- 白名单提交（F2 纪律）：**commit e0f5fd7**，43 files（CHECKSUMS ×1 + 材料视图 ×4 + 材料 case ×4 + fixed case ×30 s_feasible_size 补写 + build_open_material_cases.py + 账本 + recompute_feasible_sizes.py + per_case_view_assert_result.json），git show --stat 逐条核验与白名单一致；assert_per_case_view.py（A6 越界改动，待 E1 复核）、b_partition.json、rehearsal_logs、E4/E6/task7 等跨线文件**均未提交**；另 `backfill_feasible_size_key.py`（untracked）不在白名单、未提交、归属待确认。
- 文档合并：`task-04-report.md` 更正失实表述（E6 契约断裂→修复→对账通过）、补 Task #20/#21 记录、新增跨线交接口径 8 条（§8）与白名单提交记录（§9）；任务书状态行已同步；本文件新增 E5-07 与 Task #20/#21/#22 段。

## Task #25（task-05）共享基建 + 药物侧 fixed 45 道（Lee，2026-08-08）

- 状态：已完成。产出：`case_cs_drug_09~30`（22 道）+ `case_co_drug_08~30`（23 道）；投造 45 / 过门 45 / 判废 0；脚本 `src/case_build/build_drug_cases_task05.py`，账本 `src/case_build/task05_drug_ledger.json`（含首跑中断实录）。
- 共享基建三处（材料批 Task #26 直接复用）：① common.py QUERYABLE_PROPS 加 atom_count/element_count（E1-05 §6 已注册：74,941/17,252 行）并更正过时注释；② common.py best_is_unique 扩为 (members, prop, direction)，旧单参调用兼容；③ shared_text.py TYPE1_OPENERS 16→21、TYPE2_OPENERS 14→20（开头两词两两互异、领域中性），type2 “drug 前 7/material 后 7”分用策略改全池轮换。
- 类型1 口径切换（红线）：弃用 len(m)==1，改 best_is_unique；22 道中 18 道多成员格（|G| 最大 53），唯一性由 exhaustive_note 穷举证明锁定（只进 judge 侧字段不进题面）；build_drug_cases.py 同步更正并标弃用（不得重跑）。
- 结构判据回迁：新批 atom_count 窗口写进 constraints（四件套：logS/atom_count 上下界），dk 不再承担结构判据；存量 30 道保持方向 B 不动（两批差异见经验文档 §4）。
- 跨批查重：used_cells 初始=已有 30 道重算格（含材料侧三位格，与 Chris Task #27 回填 grid_key 交叉核验 30/30 一致）；首跑实证坑：旧 co 占格未被类型1 池排除致 gt 复用（z002880），修复为按 (domain, grid) 跨类型排除后 45 道一次全过；used_cells 终态 75、used_gt 75。
- 验收：逐道 `query_lint.py --strict --json` violations==0（45/45，不看退出码，task-02 已批口径）；全语料复核（131 case）新 45 道相关违规 0、G1 无跨批撞格、M9b gt 全库唯一；M9a 开头两词药物侧实测 type1 21 种/type2 19 种（≥18 门槛）；未跑 --freeze-batch（监工统一安排）。
- 红线核验：已交付 30 道零改动（git diff：旧 30 道唯一改动为 Chris grid_key 回填，纯增量）；未碰 open 类、gateway/evaluate、query_lint/query_spec、dag_poc/data/**。
- 遗留：材料侧 45 道（Task #26）待造；独立验证 120 道全量产物（Task #28）待排；E2 grid_key 契约 v1.0 已核对（本批逐位按契约落盘，全语料 G1 零违规）。

## Task #27（task-05 §8.1-§8.2 动作 2）30 道存量 fixed 回填 grid_key（Chris，2026-08-08）

- 范围：仅 cs/co 目录 30 道存量 fixed，顶层新增元数据字段 `grid_key`；题面/gt/s_feasible 一律不动（§8.2：不进题面不改 gt，不影响 E6 采样）。契约本体 = `dag_poc/query_spec.json` grid_key_contract（E2-06 提交版 v1.0，与任务书草案一致，无差异需注记）。
- 脚本：`src/case_build/backfill_grid_key.py`（仿 backfill_feasible_size_key.py 风格；幂等：一致跳过/不一致报错；从 gt + BackendReader 库数据一步重算；tc_band/element_count 复用 common.py 现成函数，只读不改）。脚本保持 untracked、未提交。
- 交叉核验：30/30 道 `s_feasible.members[0]` 算出的格与 gt 算出的格一致。
- 撞格自检：30 个 (template_type, target_domain, grid_key) 三元组互异，无撞格。
- 写盘面：30 个 case JSON 纯增量（git diff 删除行 0，共 +135 行），grid_key 插于 target_domain 之后；二次运行 30 道全跳过（幂等验证）。
- lint：`python3 dag_poc/query_lint.py --strict --json --cases 'dag_poc/templates/constrain*/case_c*_0*.json'` → total_cases=30，violations=0（回填前 G1 的 30 条字段缺失全部清零），pending 仅 M9/A1 两条 corpus 项，退出码 2（task-02 已批达标线）。
- 全量清单（case_id → grid_key）：co_drug_01~07 = [0,-6]/[2,-2]/[4,-4]/[5,0]/[7,-3]/[9,-5]/[11,-7]；cs_drug_01~08 = [0,-10]/[2,-7]/[5,-10]/[7,1]/[12,0]/[15,-8]/[16,-1]/[25,-8]；co_material_01~07 = [Cuprate,1,3]/[Ferrite,1,7]/[Heavy_fermion,1,4]/[Oxide,0,6]/[Chevrel,1,4]/[Carbon,0,3]/[Heavy_fermion|Chevrel,0,3]；cs_material_01~08 = [Cuprate,3,3]/[Ferrite,0,6]/[Oxide,0,1]/[Carbon,0,1]/[Oxide|Heavy_fermion,0,4]/[Other,1,1]/[Cuprate,4,3]/[Oxide,1,5]。

## Task #26（task-05）材料侧 fixed 45 道（Felix，2026-08-08）

- 状态：已完成。产出：`case_cs_material_09~30`（22 道）+ `case_co_material_08~30`（23 道）；投造 45 / 过门 45 / 判废 0（diverse_pick 确定性选格无门内判废，与药物批口径一致）；脚本 `src/case_build/build_material_cases_task05.py`，账本 `src/case_build/task05_material_ledger.json`（含首跑撞车实录 run_history）。fixed 累计 30+45+45=120 道，达 goal.md §6 目标。
- 续接点：type1 opener 索引 38~59、type2 37~59、provenance seq 30~74（药物批已占至 37/36/29）；case_id 续号 cs_material_09~30、co_material_08~30。
- 跨类型排格（Lee §6 教训落实）：used_cells 初始=已有 75 道（旧 30 + 药物 task05 45）重算格，与落盘 grid_key 逐位交叉核验 75/75 一致；occupied_grids 按 (domain, grid) 跨类型排除，材料 cs/co 共用一张占格表；每落一道增量加入。终态 used_cells=120、used_gt=124（fixed 120 + 材料 open 4）。
- element_count 回迁与口径锚定（本批独有决策）：constraints 四件套=family== ∧ tc 档窗口 ∧ element_count==N；切格/约束/穷举一律取 **E1 注册行值**（互异 [N*] label 数，build_property_counts.element_count_of），不用 common.element_count（token 计数）——实测全库 143 个晶体同 label 多次出现（如 [65*]0.5[65*]4）致两口径分叉，agent 经 get_property 查得的是注册值；旧 15 道材料 case 的 s_feasible.members/gt 实测均不含分叉实体，存量不受影响。另加实体级保险：新题 s_feasible.members 与旧 15 道成员集求交必空（逐道断言通过）。
- family=='未知' 剔除：E1-05 UCI 扩容 11,481 条未分类标签，语义空洞、task-04 判例亦不使其入题面约束；选题池剔 38 个未知格（账本 family_unknown_cells=38），终态类型1 池 53/类型2 池 47（终态口径，含材料 open 4 道 gt 排除），均富余于需求 22/23（账本 pool_exclusions 留痕，非判废）。
- 首跑撞车坑（账本 run_history 留痕，证据 `src/case_build/_evidence_m9_corpus_20260808.json`）：首跑 45 道落盘后 corpus lint 查出 1 条 M9：co_material_12 gt=c04551 与材料 open 批 case_dh_material_02 撞车；根因=used_gt 初始集只含 75 道 fixed、未并入材料 open 4 道 gt（M9 corpus 查重跨 fixed/open）；修复=used_gt 并入 dh_material_01~04 gt + 选格池增 gt_free 预筛（格内 tc 最优者已在 used_gt 则整格剔除），重跑覆盖后 45 道一次全过。
- 验收：逐道 `query_lint.py --strict --json` violations==0（45/45，不看退出码）；全语料复核（176 case）新 45 道相关违规 0、G1 无跨批撞格（37 条 G1 全为 legacy 示例）、corpus M9 仅剩 legacy gt='aa' 既有 1 条；E6 读取器 s_feasible_of 实测 45/45 命中 s_feasible_size 且三重一致；M9a 开头两词材料侧 60 道实测 type1 21 种/type2 19 种（≥18 门槛）；类型1 多成员格 20/22（|G| 最大 95，case_cs_material_26；705 为类型2 最大格），唯一性由穷举证明锁定；未跑 --freeze-batch（监工统一安排）。
- 红线核验：已有 75 道 fixed 与材料 open 4 道零改动；未碰 common.py/shared_text.py/药物侧脚本、open 类、gateway/evaluate、query_lint/query_spec、dag_poc/data/**；未提交 git（归收口轮白名单提交）。
- 遗留：独立验证 120 道全量产物（Task #28）待排；全量 lint（120 道 fixed 一起 --strict）归收口轮；legacy 示例 353 条既有违规与 gt='aa' M9 条仍为 E5 边界外已知项。

## Task #34（task-06）材料侧 open 第二批：UCI 族 11 道落盘（Lee 造题 / Terry 验证 / Taylor 收口，2026-08-08）

- 产出：`case_dh_material_05~15` 共 11 道，open 19→30（题集满 150）；投造 11 过门 11 判废 0 预检剔除 0（UCI 26 族成员全库同组成唯一，同组成普查预检无命中）；族互异、|G_B| 全 1、s_feasible_size 全 1；梯度 n=3~5/d=1~2/K=25，δ 默认 0.25、15 道收窄 0.05（族顶间距 0.19 < 0.25，只收窄不放宽）；方向 7 maximize / 4 minimize。脚本 `src/case_build/build_open_material_cases_task06.py`（fail-fast 拒重跑），账本 task06 段（attempted/built/discards/precheck_exclusions = 11/11/0/0）。
- 口径：UCI 达标族 qualifying_families_uci（26 族，E3 §9 解锁，fail-closed）；判定链不依赖降级键唯一性（只可排除）；干扰项 primary-only（池 5,771）；grid_key 三位形态 [family:str, tc_band:int, element_count:int]（family=stats base 组成串）；provenance 换 UCI 引文（DOI 10.24432/C53P47）；gt 查重全语料 144 个已用 gt 全避开；示例段与材料 01~04 gt 继续避让。
- g_b 注册：register_dh_gb.py 扩材料 c 前缀 KEY_SPACE，幂等语义改“已存在且一致则跳过”（改存量条目须显式删键再重跑）；b_partition.cases 24→35（+198 行纯增量零删除），回读 33/33 逐字一致。
- 质检链：Task #35 十项复检全 PASS；三维评审（Ray 完整性文档缺件→收口轮补齐；Kim-T1 element_count 断言缺失→方案②脚本加固，Task #38 落地、产物 md5 零影响；Victor 两致命项：CHECKSUMS 未刷新→收口轮闭合；ingest 结构性阻塞→交接③措辞照实）；全量 lint（Ray-S2）：total_cases=30、violations=0、退出码 2（pending 仅 M9 非冻结批次/A1 留痕，task-02 已批口径）；assert_per_case_view --all fails=[]（36 cases 含本批 11 道）。
- M9a：新引入 3 种 opener（Characterize/Isolate/Unearth），open 全语料 33 道 11 种/33=0.333 ≥ 0.3，余量仅 0.033（后续 dh 扩题必须同步加 opener）。
- 收口（Task #37）：CHECKSUMS 37→48 件（90,592,294 bytes）--verify 退出码 0；白名单七组 32 文件一次性原子提交（commit 9077780）；report（`task-06-report.md`）+ 经验（`造case经验/task-06-open材料第二批.md`）+ 任务书状态行已同步。交接 7 项归监工转派（E2 契约 v1.1 / M13 spare 不对称 / ingest 结构性阻塞 / E6 污染门扩面 / E4-05 合跑时序 / M9a 余量 / register_dh_gb 幂等语义）。

## Task #41（task-07 手写批次①）cs_drug 30 道原地覆盖（Lee，2026-08-09）

- 性质：手写流水线第一批（质量标杆）。四步节奏全程执行：设计意图文档先行（`task-07/batch1_设计意图.md`，30 道逐道领域名+约束组合+§3.2 相容性自答+题面草案+文献）→ 逐道落盘 → 机器核（`task07_solve.py --tiebreak logS:max` 回填 gt/s_feasible/grid_key/唯一性）→ 质检冻结。owner 红线：约束/题面全部工程师手写，脚本只算答案与验可解性；顶层均带 `lineage: handwritten-batch1`。
- 格位：128 可用格中选 30（避药物 14 禁入格 = cs_drug 10 + co_drug 4；排 co 占格 30 + 活跃 gt 90，terrain_probe 实测底账）；主流区优先（abin 2~10），无角落格豁免；26 道 logS 窗×原子带 + 4 道 bbb；**logD/half_life 本批弃用**（preflight 实测标注实体集中于 co 占的 ~10 格，强行出题只能靠偏离兜底；留痕待批次③④再评估）。
- preflight 精确口径（T43 Ryan P2 裁定，早期 46/47 两数作废）：104 候选/30 入选/66 判废（issues）/8 无解；补全漏列判废编号 A22/A23/A33/E19/E20/E22/E23/E24 与无解编号 A28/C13/D12/D14/E06/E17/E18/E21（机读清单见账本 preflight_accounting）。
- 产出：30/30 一次落盘过机器核，零判废追加、零偏离（池 3~130 全在 [3,200]，atom≥10，无 element_count）；30 互异格、30 互异 opener；21 篇文献 DOI 全部 Crossref API 核实、不写作者名。
- 质检：逐道 lint strict violations==0；`--freeze-batch cs_drug_handwritten1`：M9a 开头两词 30 种/30 道=1.0≥0.3、gt 唯一均通过；仅 corpus M13 全库存量缺口（drug=30/material=0）按现行分阶段口径留痕（材料侧受 E3 接口 B 硬前置阻塞，非单域批可闭合）。回调历史：lint 轮 1 处（case 22 域推断误判 material、case 24 arXiv provenance 无 URL），修后约束/gt/机器栏零变化。
- 账本：`src/case_build/task07_batch1_ledger.json`（30 道逐条格位/池/gt/约束摘要/偏离记录 + 批级判废历史）；脚本链：`task07_batch1_terrain_probe.py`（地形勘探）→ `task07_batch1_preflight.py`（7 轮格位规划）→ `task07_batch1_write.py`（落盘器）→ `task07_batch1_ledger.py`（账本）。
- corpus 断言：`test_template_corpus_count.py` 1 passed + 2 xfailed；b) 禁入格命中 18→8（批①覆盖 cs_drug 10 格后），断言实现不支持分批参数，维持 xfail，剩余 8 道归批②（cs_material 04/05/14/15）与批③（co_drug 01/08/09/10）。

### T43 修复轮（三维评审收口，Lee，2026-08-09）

- P1-1（Ryan）case 19/21 领域名 BCS-II 超宣称→降维为"poorly soluble, dissolution-rate-limited"同义专业表述（两题措辞互异）；设计意图自答删"高渗"断言；约束与机器栏不动。
- P1-2（Mark）atom_count 窗口按"性质名+比较符+阈值"三件套补进 constraints（30 道，对齐规程 §1.2 与 task-05 结构判据回迁先例）；同步修 dk（Ryan P2-4）：删"not via the property interface"改中性"可读实体记录或经性质接口核验"（atom_count 已在 QUERYABLE_PROPS 注册，接口可查）。全量重跑 task07_solve：**合格集/gt/池规模/grid_key 30/30 与现值逐字一致（零差异）**；逐道 lint strict violations==0；freeze-batch 仅余 corpus M13 存量缺口（现行分阶段口径）。
- P2 批：① snapshot 改落盘时 BackendReader 实算注入（92,153 实体/240,467 性质行）；② 判废口径精确化（见上段）；③ 禁入格数字统一"药物 14 禁入格（cs_drug 10 + co_drug 4）"；④ 账本补注 grid_key=gt 落格语义 + case 18/19/20 gt 恰卡窗口上界/banker's rounding 脆性留痕 + dedup 口径（排除仅作用于新题 gt 位、成员允许与活跃 gt 重叠、spare 不在排除集）；⑤ case 17 文献换 P23（动态极性表面积-口服吸收边界，10.1021/jm980313t，Crossref 核实）；⑥ 任务书状态行转进行中 + owner 三裁定（英文骨架/E6 作废重采排全量窗口/gt 查重活跃口径）补记任务书 §0。
- **E6 交接登记（Daniel P0 的 E5 侧动作）**：cs_drug_01~30 重写致 E6 已采约 808 份裸答样本与 17 个矩阵试跑单元失效；按 owner 裁定作废重采、重采排全量窗口；samples 隔离动作归 E6 线执行，E5 不碰 samples/ 目录。

## Task #44（task-07 手写批次②）cs_material 30 道原地覆盖（Lee，2026-08-09）

- 性质：手写流水线第二批。四步节奏全程执行：设计意图文档先行（`task-07/batch2_设计意图.md`，30 道逐道领域名+约束+相容性自答+题面开头+文献+口径偏离记录）→ 落盘 → 机器核（`task07_solve.py --tiebreak tc:max|tc:min` 逐道独立核 30/30 一致）→ 质检冻结。owner 红线：约束/题面全手写，脚本只算答案；顶层 `lineage: handwritten-batch2`。
- **口径偏离 1 条（族口径裁定，已 SendMessage 报备 leader）**：任务书"五族"与"5,771 条/67 格"不自洽——terrain_probe 实测 5,771=fam≠未知（未叠 tc）、叠 tc>0 后 4,007、五族白名单仅 3,743；preflight 实测五族白名单可行空格仅 23<30 凑不满；旁证：容量探针 67 格同口径、4 材料禁入格全在五族外、co_material 已用 Heavy_fermion/Carbon 格。裁定按 fam≠未知执行（禁未知族硬线不变），本批外扩 Carbon 1 + Heavy_fermion 2。另 condition 实测 38 条≠任务书 16 条，本批不入题（避"常压"硬线）。tc<=0 的 1,764 条 terrain_probe 断言排除留痕。
- 格位：30 格全部 preflight 实测空格，避材料 4 禁入格（preflight/write 双重即时断言 18 禁入格全集，Daniel R2）、co_material 30 占格、活跃 gt 63 个（co_material 30 + dh_drug 15 + dh_material 15 + spare 3）。
- preflight 精确口径：86 候选/30 入选/56 判废（issues 全名单见账本 discarded）/0 无解；五轮迭代发明小成员格拼池技术（tc 窄窗/ec 范围拼池/tc:min 方向切换三法，cs_04/10/13/19/24/29 六处，唯一性全机器核）。
- 产出：30 道落盘（池 4~126 全在 [3,200]，零逐道偏离）；约束三件套 family/tc/element_count 全落 constraints；tc:max 26 / tc:min 4（cs_03/04/11/13）；30 互异格、30 互异 opener（TYPE1_OPENERS 池尾预扩 25 种至 62）；13 篇文献（P1~P13）DOI 全部 Crossref 核实、不写作者名；snapshot BackendReader 实算 92,153 实体/240,467 性质行。分布：档0×11/档1×9/档2×5/档3×2/档4×2（档3/4 仅 Cuprate 有可行空格，数据现实留痕）；族别 Carbon1/Chevrel4/Cuprate9/Ferrite6/Heavy_fermion2/Other4/Oxide4。
- 回调历史：write 轮 2 处（CASES 初版误排判废组合→重写对齐 chosen 集；cs_13 structural ec==5 不唯一→改 ec∈[3,5] 拼池；cs_27 题面重词；snapshot 模板一处多余空格→修正后全量重刷、solve/lint 全复验）。
- 质检：逐道 lint strict violations==0（30/30）；`--freeze-batch cs_material_handwritten2`：M13 单域 freeze 硬违规（已知例外，Paul P2-2 口径：单域批 drug=0/material=30 不对称；cs 全族 60 道合跑 `--freeze-batch cs_family_60` exit 0 零违规、drug=30/material=30 对称通过，T45 修复轮 2026-08-09 实测）；corpus 守卫：禁入格活跃命中 8→4（剩余 4 = co_drug 01/08/09/10 归批③，xfail 维持）。
- 账本：`src/case_build/task07_batch2_ledger.json`（30 道逐条 + 批级判废/查重/偏离记录）；脚本链：`task07_batch2_terrain_probe.py` → `task07_batch2_preflight.py` → `task07_batch2_write.py` → `task07_batch2_ledger.py`。
- **E6 交接登记（批①同款格式，Paul P1/T45 补录）**：cs_material_01~30 重写致 460 份裸答样本（cs_material_01~19×3 模型）与 4 个矩阵试跑单元失效，作废重采排全量窗口；samples 隔离归 E6 线执行。

## Task #46（task-07 手写批次③）co_drug 30 道原地覆盖（Lee，2026-08-09）

- 性质：手写流水线第三批（类型2 约束最优）。四步节奏全程执行：设计意图文档先行（`task-07/batch3_设计意图.md`，30 道逐道领域/约束/相容性自答/题面 + §3.2 核"分子大小×溶解度"与"脂溶性×血脑屏障"两对 + §5 文献）→ 逐道落盘 → 机器核（`task07_solve.py` objectives 优先，逐道独立复算 30/30 一致）→ 质检冻结。owner 红线：约束/题面全手写，脚本只算不造；顶层 `lineage: handwritten-batch3`。
- 类型2 形态：全批带 objectives（logS:max 28 / logD:max 1（case 09，叠 logS≥−2.5）/ half_life:max 1（case 13，叠 logS≥−2.5，批①契约教训））；dk 四件含 Objective + 平手/唯一性表述；query 由 build_type2_query 组装（Scoring 段含目标量与 higher-is-better 表述）。
- 格位：30 互异格全部避开 18 禁入格——**含本批专项避让旧 co_drug 最后 4 格 (0,-6)/(0,-5)/(0,1)/(1,0)，写完 18 格全部清零**；避 cs_drug 30 占格；**旧格换人三策略**（窄原子带/窄 logS 窗/稀疏性质 fail-closed，cell_probe 实证旧 co gt 多为裸 logS 实体），30 道 gt 全部换人零沿用旧 co gt。
- preflight 精确口径：84 候选/30 入选/44 判废（issues 全名单见账本）/10 无解（高溶档配大原子带库内真空，如实记录不硬凑）；回调：第五轮编辑误删 I16/I17 候选行→补回 I16（(11,-1) 偏离入选）后全量重跑确认。
- 口径偏离 4 条（池<5，操作规程 §8 工程师裁定，deviation_note 三要素随题落盘）：case 10（池2，避脑×中溶×中大尺寸交集天然稀疏）/21（池4，bRo5 45~49 原子保 −1 档）/26（池3，55~59 原子可溶性极限）/30（池4，巨型分子×−6 档双极端）；科学问题真实、放宽任一维度即离开问题本身，不硬凑。其余 26 道池 5~73 全在 [5,200]；atom_count 三件套全落、全部 ≥10；无 element_count（owner 拍定）。
- 产出：30/30 一次落盘过机器核零判废追加；30 互异格、30 互异 opener（TYPE2_OPENERS 开工第一步预扩 38→68，本批全用新增段 38~67，task-05 builder 冻结只追加不改既有语义）；snapshot BackendReader 实算 92,153 实体/240,467 性质行；22 篇文献（复用批① P 系列 + 新增 P24 bRo5/P25 logD/P26 代谢稳定性）DOI 全部 Crossref 核实、不写作者名；gt 为 masked 实体无真实化合物身份，文献挂科学问题本身（机制上规避批②挂错化合物类别坑）。
- 质检：逐道 lint violations==0（30/30）；`--freeze-batch co_drug_handwritten3`：M9a 开头两词 30 种/30 道=1.0≥0.3 通过；M13 单域 freeze exit=1 为已知例外（批③专属第 3 条口径）——"cs+co 全族 120 道"合并 freeze（cs_co_merged_check）exit_code=0 零违规佐证 M9a/M13 状态。
- corpus 断言转正（批③专属第 2 条）：`test_template_corpus_count.py` b)（18 禁入格）删 xfail 转正 PASS，保持常规门禁。**口径偏离留痕（三要素，leader 裁定 B 补丁）**：①偏离哪条——任务书"b) 与 c) 两处 xfail 同删转正"字面口径；②实际取值——58e8a89 曾按字面双删，实测 c)（lineage）断言对全 150 道生效而非仅已手写批次，60 道缺 lineage（co_material 30 + dh 30，批④⑤未写脚本批旧题）致 pytest 红，补丁提交恢复 c) xfail（strict=True，批⑤补齐后自动 xpass-fail 提醒转正）；③为什么必须这样——批④⑤ 60 道交付前 pytest 留红数日会阻塞共享工作区其他五条并行线的全量测试，CI 红的代价大于字面合规的收益（leader 裁定）；c) 延至批次⑤收口时删（届时 150 道均带 lineage）。
- 账本：`src/case_build/task07_batch3_ledger.json`（30 道逐条格位/池/gt/目标量/偏离 + 批级判废四数/查重底账/E6 登记）；脚本链：`task07_batch3_terrain_probe.py` → `task07_batch3_cell_probe.py`（格内剖面）→ `task07_batch3_preflight.py`（7 轮格位规划）→ `task07_batch3_write.py`（落盘器）→ `task07_batch3_ledger.py`（账本）。
- **E6 交接登记（批①②同款格式）**：co_drug_01~30 重写致 201 份裸答样本（bare_baseline/samples/case_co_drug_01~07×3 模型）与 4 个矩阵试跑单元（matrix_runs/{qwen3.7-max,bailian_glm-5.2}/H1/case_co_drug_{03,15}）失效，作废重采排全量窗口；samples/ 目录不碰，隔离归 E6 线执行。

## Task #48（task-07 手写批次④）co_material 30 道原地覆盖（Lee，2026-08-10）

- 性质：手写流水线第四批（类型2 约束最优，材料侧）。四步节奏全程执行：设计意图文档先行（`task-07/batch4_设计意图.md`，30 道逐道领域/约束/§3.2 相容性自答/题面 + §1 文献 + §6 批②三教训防护对照）→ 逐道落盘 → 机器核（`task07_solve.py` objectives 优先 tc:max，逐道独立复算 30/30 一致）→ 质检冻结。owner 红线：约束/题面全手写，脚本只算不造；顶层 `lineage: handwritten-batch4`。目标量一律 tc:max（query 骨架 "(higher is better)" 固定，批中禁改共享模块）。
- **容量终核结论（28+2，leader 2026-08-10 追认）**：三重探针全量穷举（terrain_probe/swap_windows/full_terrain + 跨 ec 拼池探针）证明池≥5 可行格上限 **28**：旧格换人 23 + 旧格窄窗 2（Other(1,2)/(1,3)）+ 新格 3。缺口根因：材料侧池≥5 的 49 格中 30 个是 cs_material 占格（禁入），其余几乎全是旧 co 格（换人后 23 可行），新格仅 3；跨 ec 拼池、坏并列下潜窗等批②全部技巧用尽（Cuprate ec∈[3,4] 拼池 gt 落 cs 占格被禁）。**末轮计数更正留痕：首报 "29 道池≥5" 系 Other(1,3) 双窗重复计数，更正 29→28**。**末轮禁入格补位更正留痕：落盘自检（18 禁入格全集）拦下 2 新格（Carbon(0,1) 与 Heavy_fermion|Chevrel(0,4) 恰属 FORBIDDEN_CELLS_MATERIAL，前三重探针漏查此表）；补位探针（`task07_batch4_replacement_probe.py` 切片法穷举+禁入/cs/批内/活跃四重过滤）得 10 可行补位格，定型 S01 换格 Carbon(0,3)、S02 换族 Oxide(0,1)（Carbon 档 0 三格全灭）、S07 换格 Heavy_fermion(0,1)；替换后 28+2 总数不变**。+2 道池 3 偏离道（S39 Cuprate 42–45K ec==3 中温铜氧筛选／S40 Ferrite 七元铁基 10–24.1K 档），deviation_note 三要素齐（放宽任一维度即离开该科学问题）；不硬凑、不降池下限、不碰未知族。
- 类型2 形态：全批 objectives tc:max；dk 四件（family+tc 硬约束条（Ferrite 全道带消歧注）/element_count structural 条/Objective 条/Tie-break 条）；query 由 build_type2_query 组装（opener 0~29，批③用 38~67 零撞）。批②三教训设计期防护：叙事温区逐道核窗内（case 27 用 65K 过冷氮落于 [60,80)、case 28 用 77K 落于 [76,99.3)）、文献按族标签+ec+温区三重核对（弱对应 11 道降语境参考措辞）、Ferrite 全道消歧。
- 格位：30 互异格全部避开 18 禁入格与 cs_material 30 占格；旧格一律换人窄窗（窗顶≤旧 gt tc），30 道 gt 全部换人零沿用；查重底账活跃 gt 排除集 123（cs 60+co_drug 30+dh 30+spare 3 从严，preflight active_ledger 实扫断言）。
- preflight 精确口径（终轮）：39 候选/28 入选/11 判废（同格备选 7+不唯一 1+池小备选 1+偏离道留痕 2，全名单见账本）/0 无解；另第一轮（整档口径）42/4/38/0 留痕（根因旧 co gt 全为各格 tc 最大值 → 换人窄窗策略）。
- 产出：30/30 落盘过机器核；30 互异格、30 互异 opener；snapshot BackendReader 实算 92,153 实体/240,467 性质行；文献 15 条全部沿用批②已 Crossref 核实表（零新增，不写作者名），case 07 补位后由 P9 改 P7（gt 即重费米子族，更切题）。
- 质检：逐道 lint violations==0（30/30）；`--freeze-batch co_material_handwritten4`：M9a 开头两词 29 种/30 道=0.967≥0.3 通过（根因：TYPE2_OPENERS 池 idx 10/11 两 opener 开头两词同为 "out of"，池本身含一对两词重复，门禁不受影响）；M13 单域 freeze exit=1 为已知例外——全库 case_*.json 150 道合并 freeze（cs_co_merged_batch4）violations==0 exit_code=0 佐证。
- 账本：`src/case_build/task07_batch4_ledger.json`（30 道逐条格位/池/gt/目标量/偏离 + 批级判废四数/查重底账/E6 登记）；脚本链：`task07_batch4_swap_windows.py`（换人窗扫描）→ `task07_batch4_full_terrain.py`（全量可行组合）→ `task07_batch4_preflight.py`（候选表+active_ledger）→ `task07_batch4_replacement_probe.py`（禁入格补位探针）→ `task07_batch4_write.py`（落盘器）→ `task07_batch4_ledger.py`（账本）。
- **E6 交接登记（批①②③同款格式）**：co_material_01~30 重写致 167 份裸答样本（bare_baseline/samples/case_co_material_01~07×3 模型）与 2 个矩阵试跑单元（matrix_runs/{qwen3.7-max,bailian_glm-5.2}/H1/case_co_material_08）失效，作废重采排全量窗口；samples/ 目录不碰，隔离归 E6 线执行。
- **提交链卫生记录（复检登记，2026-08-10）**：前序提交 8f78419（主题"学术汇报幻灯片"）混入批④脚本资产 6 件（task07_batch4_{terrain_probe,swap_windows,full_terrain,preflight,write}.py + batch4_设计意图.md）及他线在途文件 10 件（eval_matrix 2 件 matrix_summarize.py/test_matrix_summarize.py、E6 5 件 matrix_runs/README.md+summary.json+progress.md+report-06.md+task-06-修汇口径与验判分.md、rehearsal 2 件 grounding_condition3/material_identity_key realdata.log、.review_tmp/t43/verify_batch1.py 1 件）；批④主提交 0433162 干净（白名单 37 件零裹挟）。leader 裁定不重写共享仓历史（rewriting 风险大于收益），登记留痕。同类混提问题为第三次出现（前两次含 faa436c 等），本段同时登记防御措施：此后每次提交前 git status 逐文件核对归属、显式 add 路径清单、禁止 git add -A。



## E5-07 · task-07 批次⑤ E6 交接登记（Task #50，2026-08-10，批①②③④同款格式）

- **E6 交接登记**：dh 30 道（case_dh_drug_01~15 + case_dh_material_01~15）原地覆盖重写（lineage=handwritten-batch5）致 450 份裸答样本（bare_baseline/samples/case_dh_drug_01~15×3 模型）+ 8 份 smoke 样本（bare_baseline_smoke/samples/case_dh_{drug_01,drug_02,material_01,material_02}）与 6 个矩阵试跑单元（matrix_runs/{qwen3.7-max,bailian_glm-5.2}/H1/case_dh_{drug_01,material_01,material_10}）失效，作废重采排全量窗口；samples/ 目录不碰，隔离归 E6 线执行。
- spare 增量：spare_dh_material_01~03 补造落盘（leader 裁定方案 A，3+3 对称），g_b 已注册（register_dh_gb.py 断言 33→36，b_partition.cases 36 道 dh 键回读逐字一致）；spare 命名不入 corpus 单层 glob，150 计数不受影响（pytest 3 passed 实测）。
- **提交链卫生记录（复检登记，Task #51 FAIL 项修复，2026-08-10）**：ba0f645（主题“文献扩充第一阶段”，95 件）裹挟 E5 批次⑤在途产物 **42 件**（git show 实测）：材料 case 15（case_dh_material_01~15）+ spare 3（spare_dh_material_01~03）+ per_case_views 18（同 18 道视图）+ 脚本/中间产物 5（task07_batch5_{capacity_probe,config_pool,reselect_drug,write}.py + config_pool.json）+ batch5_设计意图.md 1；内容已逐字核验无损（lineage=handwritten-batch5 正确）。同提交他线文件 53 件（reference/ 文献目录 52：46 SOURCE.txt + 6 阅读报告；TODO_待解决问题清单.md 1）。按既往裁定不重写共享仓历史，主提交 b9a3576 只补药物侧增量 38 件。
- **五次混提全列**（同类问题第五次，逐次补明；第五次为终收口 Task #52 复检观察项处置补登）：① **faa436c**（主题“任务 #73 过程指标”，41 件）——本线仅 4 件（test_process_metrics_status_gate.py 等 3 test + src/benchmark/evaluate.py），卷入 E5 归档手术 _deprecated/ 迁移文件 37 件；② 8f78419（主题“学术汇报幻灯片”）——卷入批④脚本资产 6 件 + 他线在途 10 件（详见批④段登记）；③ **ab7a2db**（主题“六条线核查”，task-05 期多线混合提交，此前登记以“等”带过，本段补齐 hash）——含 E5-03 open 15 道产物等多线文件混入，已登记 problems.md PR-39（ab7a2db 混提与提交纪律），当时经 worktree 对照实证判分零影响；④ **ba0f645**（95 件裹挟 42 件，见上条）；⑤ **fa130f4**（主题“文献扩充第三阶段”，本次补登）——裹挟批⑤药物探针脚本 `src/case_build/_probe_batch5_drug_final.py` 1 件（一次性提取探针，随终收口白名单提交以 D 态删除处置，历史内容可从 fa130f4 git show 找回，处置理由见 task-07-report.md §10）。注：方向反转教训——前几次为 E5 宽 add 卷入他线或多线混提，④为他线宽 add 卷入 E5 未提交产物，E5 侧无过错但裸奔窗口过长是可控因素。
- **防御措施第四条（补登）**：①提交前 git status 逐文件核对归属；②显式 add 路径清单；③禁止 git add -A；④**落盘后立即提交，缩短未提交裸奔窗口**（本次教训：产物在磁盘停留数轮会话，被 ba0f645 他线宽 add 卷入；E5 侧可控项即压缩窗口）。
- **批⑤四步节奏执行叙述（Mark S1 补登）**：① 探查（Task #24，Sam）：snapshot 实算 92,153 实体/240,467 性质行，容量实测——材料可用族 28/31、可行配置 280，药物含可行配置 331 组/7,576 配置（需求 15+3，均充裕），契约口径（grid_key/s_feasible_size/opener 余量）锤定；② 造题（Task #50，Lee）：材料 15+spare 3 先造、药物 15 重选后造，33/33 过门判废 0；资产链五步（case→视图→register_dh_gb.py 33→36→CHECKSUMS→pre_gates 门 1）；xfail 转正 pytest 3 passed；主提交 b9a3576（38 件白名单）；③ 复检（Task #51，Terry）：首轮 1 FAIL+1 观察项，小补丁 bb2e451 闭合（卫生记录+账本 e6 字段）；④ 三维评审（Ryan/Mark/Daniel）→ 修复轮 9 条一个原子提交收口（本条即修复轮补登）。质检实测（修复轮复测）：全量 lint total_cases=150 violations==0，--freeze-batch handwritten-final 无 M9 族违规（pending 仅 A1 留痕，exit=2，task-02 已批口径）；账本 quality_evidence 节点已补（与批①~④结构对齐）。
- **药物 15 道确定性重选摘要（Mark S1 补登，援引 b9a3576）**：初选 13/15 道因两处确定性缺陷作废——① config_pool 枚举器未按变元计数去重（同计数同分异构体当不同梯度点，段内计数重复退化），镜像 task-03 try_build 修复后重刷池；② d 口径初判误用位置索引距离，真实口径=计数轴绝对差（task-03 口径），初判可保留的 d12/d13 实算 d=3 超界。全 15 道经 task07_batch5_reselect_drug.py 在修复后 ok 池内确定性重选（硬约束：backbone+var 组互异/批内 gt 互异/排除集实扫/旧 dh gt 零沿用），终选回填 write.py DRUG_CASES 后全量落盘（提交 b9a3576）；根因详记见 batch5_设计意图.md §2 更正节。
- **冻结禁重跑清单登记（Daniel N1，与批③ N1 合并处置）**：批⑤两脚本入口已上硬闸——`task07_batch5_write.py`（main() 入口）与 `task07_batch5_reselect_drug.py`（脚本头）：账本/产物已存在即 raise RuntimeError 拒跑（仿批①~④ builder 做法，只拦执行不改生成逻辑；修复轮账本补节点经先解冻→改生成器→重跑验产物零 diff→重新上闸流程完成）。冻结禁重跑名单（累计）：build_drug_cases_task05.py、build_material_cases_task05.py、build_open_material_cases_task06.py（fail-fast）、批①~④各 builder、**task07_batch5_write.py、task07_batch5_reselect_drug.py**（本批新增）。

## Task #52（task-07 终收口）手写 150 道 case 五批全闭环（Taylor，2026-08-10）

- **三绿终验（复跑留证，2026-08-10）**：① 全量 lint `query_lint.py --strict --json --cases 'dag_poc/templates/*/case_*.json'`：total_cases=150、violations=0、exit=2（pending 2：M9 冻结时必跑留痕 + A1 扫描范围留痕，task-02 已批达标线）；② `--freeze-batch handwritten-final`：violations=0、exit=2（pending 仅 A1），M9 变体率转实判通过、无 M9 族违规；③ pytest `test_template_corpus_count.py` 3 passed（150=60/60/30 配额、18 禁入格零命中、全量携带 lineage）；④ `generate_checksums.py --verify` exit=0（主清单 52 文件 + 附录 2 交接产物 md5 全一致）。
- **题面互异核（交付物 2）**：机器层 150 道 query 两两比对（difflib，11,175 对）完全相同对 **0**；跨类型天花板 cs↔co 0.336、cs/co↔dh 0.06；批内最高 cs 0.870 / co 0.951 / dh 0.997（dh 高相似为 open 题型固有骨架，差异维度=opener/领域/目标量，如实登记不虚构多样性）。人工层并排读 38 道（top-6 高相似对全文 + 分层抽样 32 道）：**没有两道读起来一样**。
- **产出**：`task-07/task-07-report.md`（配比账/偏离汇总三要素/判废账五批四数/容量终核/提交链全账含五次混提登记/E6 交接总账/冻结清单）；`造case经验/task-07-手写150道.md`（四步节奏/三教训防护演进/枚举器缺陷与硬闸/混提防御四条/xfail 转正曲折/容量探针先行）；任务书状态行改“🟢 完成”终态。
- **两项复检观察项处置**：① 第五次混提登记 fa130f4（裹挟批⑤药物探针脚本 1 件，见上方五次混提全列）；② `_probe_batch5_drug_final.py` 磁盘删除（D 态）随本收口提交一并提交——裁决理由：一次性提取探针、使命已兑现（batch5_设计意图.md §2 回填完成）、全库零引用、下划线前缀非正式资产；历史内容保留于 fa130f4 可随时 git show 找回（不选恢复入库，避免正式资产目录混入一次性脚本）。
- **白名单原子提交**：仅 4 文档（report/经验文档/任务书/本 progress）+ probe D 态，显式 add 逐文件核对，零裹挟（PhysicianBench submodule 指针与 E6/E3 在途件一概不碰）；commit hash：**ac5d110**（5 文件 +185/−41；hash 回填单独微提交，判例同 45ab792/1e88360）。
