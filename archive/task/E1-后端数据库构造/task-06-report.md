# Task Result · E1-06 —— ingest_open_gt 执行销账 + 药物侧 element_count 核验

> **Status: `备战完毕、待执行`** —— §2（element_count 核验）与 §3（CHECKSUMS）已全部交付；§1（警告归零）经 Task #39 停报后监工裁定**方案 B（gt 迁移手术）**，Task #41 已完成手术备战（脚本一键就绪 + dry-run 全验证 + 两份交接清单，见 §7）；**执行前置 = E3 判分侧完工 + 监工通知，当前不执行**。

> ⚠️ 本文件自包含：外部读者只读本文件即可完整理解任务做了什么、结论是什么、接下来怎么办。

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E1-06 / `task/E1-后端数据库构造/`（任务板 Task #39） |
| **类型** | Implementation（收尾） |
| **负责人** | E1（数据线） |
| **起止** | 2026-08-08 ~ 2026-08-08（半天） |
| **上游来源** | task-06 任务书 §1/§2/§3 + E3 task-06（任务 #65）PR-46 消解条件登记 |
| **下游消费方** | E3（gateway 警告语义）、E5（真 B 数据入库口径）、监工（PR-46 台账销账） |

---

## 一句话结论

> **§2 已由今早 Task #38 完整交付、本任务三项核验全绿（74,941 行逐数吻合 / 红线扫描真实元素符号 0 条 / drug_admet_v1 增键在案），CHECKSUMS `--verify` exit=0 且值零漂移（`06232bd2…` 不变，本轮纯核验+登记销账、backend 零写入）；§1 按任务书要求实跑 ingest_open_gt 到尽头——open case 真 gt 全部被身份键空间隔离闸合规拒收（15/17 条 B∩A 命中，根因是 ADR-0018 per-case 逻辑 B 设计下 dh 类 gt 物理上就在 A 库），警告无法经本次执行归零，残留相交属实，已按停报条款上报待裁定。**

---

## 1. 任务执行情况（对照 task-06 §3 四条交付标准逐条勾验）★

- [/] **交付 1：`ingest_open_gt` 已执行，构造 Gateway 不再打该警告** —— **部分完成（触发停报条款）**。
  - **已完成**：① 警告产生机制读懂（§2.1 全链路）；② 构造真 gt 文件（`build_open_gt_task39.py`，从 176 道 templates + judge 侧 entity_registry 解键，产出 15 case/17 条药物 gt 记录）；③ `--dry-run` 实跑过闸：**接受 0 条、拒收 17 条**（15 条身份键空间隔离闸 B∩A=∅ 命中、2 条断言二 source_paper_doi 缺失——可补齐但无意义，见 §4）；④ 材料侧 4 道 gt 为晶体，`check_molecule_exists_B` 双键列（InChIKey/canonical SMILES）结构上无法承载，未进投闸集。
  - **未完成**：警告归零。**原因**：accepted=0 时正式写盘只会把目录 MANIFEST 翻新成新 md5（CHECKSUMS 漂移）而 B 侧仍空、警告照旧，无任何收益，故未执行非 dry-run 写盘；残留相交属实（§2.2），按任务书"若执行后仍有相交，停下报我，不要自行处置"停报。
- [x] **交付 2：药物侧 `element_count`（全覆盖 + 红线 0 条）** —— **该交付项已由今早 Task #38 完整交付，本任务只核验不返工**（时间线说明见 §2.3）。三项核验全绿：
  - ① 实跑统计：get_property_A 中 prop=element_count 且实体为药物侧（非 c 前缀晶体）行数 **== 74,941**（总 92,193 = 药物 74,941 + 材料 17,252）；值域 1~8；档内 3~6 种 = 73,896（13,498/31,458/23,019/5,921），档外 1,045（1 种 61/2 种 416/7 种 539/8 种 29）——与 Task #38 销账表逐数一致。
  - ② 红线扫描：`check_agent_visible_anonymity.py` 实跑 exit=0，四道断言零命中——get_property_A.jsonl **真实元素符号 0 条**（该批新行的 value 均为纯整数，只给计数不给元素名，同口径覆盖）。
  - ③ drug_admet_v1.json 增键在案：`element_count`（number/tol=0/constraint_form `{value} >= {threshold}`，note 写明"匿名式 [N*] label 种类数、匿名前后不变、只给计数"）。
- [x] **交付 3：CHECKSUMS 刷新，`--verify` 退出码 0** —— 实跑 `generate_checksums.py --verify` exit=0："冻结校验通过 ✓：主清单 37 个文件 + 附录 2 个交接产物 md5 全部一致"；CHECKSUMS 本体 md5 **`06232bd217a592dbe51621f5eee1c386`**（与 Task #38 冻结值相同，**零漂移**）。**本轮属纯核验+登记销账、不碰冻结文件**：backend/ 零写入（dry-run 不写盘），故无需刷新 CHECKSUMS、无需入库手册 §8 解冻登记。
- [x] **交付 4：report 收口 + 状态行更新** —— 本报告；task-06 状态行已更新（🟡→ 反映 §1 停报待裁定的如实状态，见 §3.1）；progress.md 追加第 26 条。

**勾选符号**：`[x]` 完成、`[/]` 部分完成——无虚勾，交付 1 的"警告归零"未达成，如实标部分。

---

## 2. 关键产出 / 发现（自包含）

### 2.1 警告产生机制（先读懂再执行）★

| 环节 | 事实 |
|---|---|
| **警告打印点** | `dag_poc/gateway.py` L1078（`Gateway.__init__` 内 B∩A=∅ 装载闸）：backend 身份档在场时两层判交——层 1 B 键 vs A 实体 ID 全集（92,193）、层 2 B 键 vs A canonical SMILES 全集；相交成员若命中机器占位判据（z*/c*+数字形态，或 provenance「占位」标记 / placeholder_warning 声明作用域）则放行并打**一次性** warning（模块级 `_b_a_placeholder_warned` 闸），否则 raise。 |
| **占位的实质** | `dag_poc/mcp_mock_server/b_partition.json` 的 22 道 open case（15 药物 dh + 3 spare + 4 材料）members/g_b 直接登记 **A 身份键**（z*/c* 实体 ID），provenance 明写"B 侧未入库，空占位 / ingest_open_gt 未执行"——交接期占位，非快照错误。 |
| **预期消解路径**（E3 PR-46 登记） | E5 真 gt 经 `ingest_open_gt` 入 `backend/check_molecule_exists_B.jsonl` → `default_b_partition_reader` 自动切 `BPartitionBackendJsonlReader` → b_set 变为真实 InChIKey/SMILES 键 → 与 A 匿名域键空间不相交 → 警告归零。 |
| **实测触发路径与输出**（本轮实跑） | `Gateway(case_id='case_dh_drug_01', case_kind='open')` → `WARNING b_partition 与 A 全集相交 5 键（…ingest_open_gt 未执行…）：示例 ['z011720','z047711','z057639']`；材料 case 相交 1 键；无 case 构造相交 1 键（顶层 fixture 成员 2 撞 aa 匿名 SMILES）——任务书引文的"相交 1 键"即此。 |

### 2.2 §1 执行实证链（dry-run 走到尽头）★

| 步 | 动作 | 结果 |
|---|---|---|
| 1 | 清点真 gt 来源 | 全库唯一 gt 文件是 E1-02 演练 fixture（`rehearsal=true`，纪律禁入，**未使用**）；open_discovery 4 道旧题 gt=null；19 道带 g_b 的 open case（15 药 + 4 材料）g_b 均为匿名实体 ID。 |
| 2 | 构造真 gt 文件（`src/data_build/build_open_gt_task39.py`，新写）：g_b 匿名 ID 经 judge 侧 `entity_registry.jsonl` 解真实 InChIKey/canonical SMILES，provenance 用 `open_case_gt` 源（断言三 B 侧通道放行源）+ case_ref 承载于 note（契约 v1.2 裁定二） | 产出 `_build_state/open_gt_task39.json`：15 case/17 条药物 gt；材料 4 条（晶体，无 InChIKey/SMILES）无法承载于分子存在性通道，不入投闸集。 |
| 3 | `ingest_open_gt.py --dry-run` 实跑 | **接受 0 条、拒收 17 条**：15 条身份键空间隔离闸（InChIKey 命中=True 且 canonical SMILES 命中=True——gt 真实身份就在 A 池 registry）；2 条断言二（case_dh_drug_05/11 模板 provenance 行无 DOI，可补齐但补齐后同样撞隔离闸，无意义）。A 池键空间实测：InChIKey 74,283 / canonical SMILES 74,941。 |
| 4 | 残留相交核查 | **属实**：22 道 open case 占位成员全部 ∈ A 全集（case_dh_drug_01 实测 5/5：z011720/z047711/z057639/z064386/z068460 均在 get_entity_A 92,193 键内；材料 case c04592 同）。全部经机器占位判据放行，**非隔离破口**——与监工"已知未闭项"定性一致，但"执行 ingest_open_gt 后归零"的消解预期在当前设计下不成立（见根因）。 |
| 5 | 根因 | **ADR-0018 §2.3 per-case 逻辑 B**：dh 类 gt 物理上就是 A 库实体（z064386 ∈ get_entity_A，仅被 per-case 视图白名单排除），`test_backend_source_switch_task06.py` 断言 2/5 反向锁定"gt 必须可经 get_entity 查到"。隔离闸按 goal §2b⑤ 拒收 A 内分子入 B 是**正确行为**；E3 报告"B 成员与 A 实交 0"的复算针对的是未来库外真 B 数据，而现行设计下 dh gt 永远 ∈ A——占位消解预期与设计演进矛盾，非操作故障。 |
| 6 | 不写盘决定 | accepted=0 时正式执行只会翻新目录 MANIFEST（"当前冻结 case_ref：无"新 md5）造成 CHECKSUMS 漂移，B 侧仍空、警告照旧——无收益有漂移，故只做 dry-run 留痕。演练 fixture 零混入（E1-02 纪律守住）。 |

### 2.3 §2 element_count 核验（Task #38 已交付，本任务只核验）★

**时间线说明**：监工 task-06 落笔早于 Task #38 交付（2026-08-08 上午），故 §2 与 Task #38 重叠；**§2 已由 Task #38 完整交付，本任务仅核验、未返工一行数据**。

| 核验项 | 口径 | 实测 | 判定 |
|---|---|---|---|
| 行数 | get_property_A 中 prop=element_count 且药物侧实体（非 c 前缀）| **74,941** == 任务书基线 | ✓ |
| 值域与分布 | 逐行统计 | 值域 1~8；3 种 13,498 / 4 种 31,458 / 5 种 23,019 / 6 种 5,921（档内 73,896，与监工四档逐数一致）+ 档外 1,045（1 种 61/2 种 416/7 种 539/8 种 29，leader 裁定口径 a 另册登记在案） | ✓ |
| 红线扫描 | `check_agent_visible_anonymity.py` 实跑（四道断言，真实元素符号/InChIKey/真名/真实公式） | exit=0，get_property_A.jsonl 四道全零命中——**该批新行真实元素符号 0 条** | ✓ |
| 性质清单增键 | drug_admet_v1.json | element_count 键在案（number/tol=0/`{value} >= {threshold}`，note 写明匿名域自洽、只给计数） | ✓ |
| judge 侧账本 | Task #38 留痕 | 240,664 条血缘对齐（finalize 断言过），本轮未重跑、以 Task #38 自检六连为据 | ✓ |

### 2.4 CHECKSUMS 状态

`generate_checksums.py --verify` 实跑 exit=0（主清单 37 + 附录 2 全部一致）；CHECKSUMS md5 `06232bd217a592dbe51621f5eee1c386` **与 Task #38 冻结值相同、零漂移**——本轮 backend/ 零写入，无新值产生，无需再冻结、无需入库手册 §8 解冻登记（纯核验+登记销账，不碰冻结文件）。

---

## 3. 结论与建议

### 3.1 判定

**🟡 有条件完成（§2/§3 全绿；§1 依任务书停报条款停于"残留相交属实"，待监工裁定处置方向）** —— §1 的执行链已实跑到设计允许的全部尽头（dry-run 0 过闸是隔离闸的正确行为，不是执行失败）；警告归零在 ADR-0018 现行设计下不可经 ingest_open_gt 达成，属设计层矛盾，按任务书"不要自行处置"上报。

### 3.2 下一步行动（具体、可打勾）

| # | 动作 | 谁来做 | 何时 | 优先级 |
|---|---|---|---|---|
| 1 | **§1 处置方向裁定（三案）**：**A. 口径改判**——承认 ADR-0018 下 dh 类 B 侧是逻辑分区，占位放行机制长期有效，task-06 §1"归零"预期改为"登记销账+警告语义修订"（警告文案与 PR-46 消解条件同步改，归 E3）；**B. gt 迁移手术**——22 道 open case gt/同组成候选从 A 侧四库迁出、B 侧重注册（触 23+ 冻结文件，且与"gt 可查"测试断言直接冲突，需先改测试语义，不建议）；**C. 等 E5 交付库外真 B 分子**（若后续 open 题改回物理 B 设计）再执行 ingest | 监工/leader 裁定 | 尽快 | P0 |
| 2 | 裁定为 A 时：problems.md PR-46 状态由"已实施待消解"改为"口径改判已销账"，E3 同步修订 gateway 警告文案（去掉"执行 ingest_open_gt 后应归零"的失效预期）与 `test_machine_b_a_resolution_direction_locked` 的 `checked > 0` 断言语义 | E3（改 gateway/测试）+ E1（改 problems.md 登记，待裁定后） | 裁定后 | P1 |
| 3 | 裁定为 C 时：本 task 保持 blocked，E5 交付库外真 gt 后由 E1 按入库操作手册 §3 执行（含 §8 解冻登记 + CHECKSUMS 统一刷新） | E5 → E1 | E5 造题口径变更后 | P1 |
| 4 | 轻微项登记：case_dh_drug_05/11 模板顶层 provenance 行无 DOI（断言二拒收面之一，补齐与否归 E5 造题口径） | E5 | 顺手时 | P3 |

---

## 4. 受阻 / 失败说明

**§1 警告归零受阻（触发任务书停报条款，未自行处置）**：

- **受阻点**：`ingest_open_gt.py` dry-run 接受 0 条——17 条药物真 gt 记录中 15 条被身份键空间隔离闸（B∩A=∅）拒收（gt 真实 InChIKey 与 canonical SMILES 双双命中 A 池 registry），2 条断言二拒收（DOI 缺失，补齐后同样撞隔离闸）；材料 4 道 gt 为晶体，分子存在性通道结构上无法承载。
- **为何不是操作问题**：ADR-0018 §2.3 确立 per-case 逻辑 B——dh 类 gt 物理上是 A 库实体（实测 z064386 等全部 ∈ get_entity_A 92,193 键），仅被视图白名单排除；隔离闸拒收 A 内分子入 B 正是 goal §2b⑤ 的正确执行。"执行 ingest_open_gt 后归零"的消解预期（E3 PR-46）建立在"未来存在库外真 B 分子"之上，而现行 176 道语料中不存在这样的 gt。
- **已按停报条款执行的动作**：残留相交如实核查（属实，22 道 open case 占位成员全部机器识别放行中，非隔离破口）→ 停止一切写盘 → 上报 leader（2026-08-08，附 A/B/C 三案请示）。
- **无受阻项**：§2 核验与 §3 CHECKSUMS 全绿，无任何失败。

---

## 5. 风险与未决问题

| 类别 | 风险 / 问题 | 影响 | 缓解 / 待决 |
|---|---|---|---|
| 已知风险（既有，本轮未消解） | b_partition 占位警告持续刷屏（每次 Gateway 构造一次性 warning） | 狼来了效应——真正的隔离告警可能被忽略 | 待 §3.2 #1 裁定；裁定前占位放行机制与其机器断言双保险（非占位相交仍 raise）照常有效，隔离红线未放宽 |
| 新登记 | ADR-0018 逻辑 B 与物理 B∩A=∅ 消解预期的设计矛盾 | task-06 §1 与 PR-46 消解条件在当前设计下永不到达 | 已上报，三案待裁（§3.2 #1）；本报告 §2.2 实证链为裁定依据 |
| 新登记（轻微） | case_dh_drug_05/11 模板 provenance 无 DOI | 若日后走物理 B 入库，该 2 道需先补 provenance | 归 E5 造题口径（§3.2 #4） |
| 已销账 | 药物侧 element_count（task-06 §2） | — | Task #38 交付 + 本轮三项核验全绿，闭环 |

---

## 6. 交付物与收尾自检

### 6.1 收尾自检（本轮全部实跑）★

| # | 项 | 输出 | 退出码 |
|---|---|---|---|
| 1 | element_count 行数统计（脚本见 §2.3 口径） | 药物侧 74,941 / 材料侧 17,252 / 总 92,193；值域 1~8；档内 73,896 档外 1,045 | 0 |
| 2 | `check_agent_visible_anonymity.py` | 四库四道断言零命中（get_property_A 真实元素符号 0 条） | 0 |
| 3 | `generate_checksums.py --verify` | 主清单 37 + 附录 2 全部一致；CHECKSUMS md5=`06232bd217a592dbe51621f5eee1c386`（零漂移） | 0 |
| 4 | `ingest_open_gt.py --dry-run`（§1 实证） | 接受 0 / 拒收 17（拒收清单留痕 `_build_state/rejections_check_molecule_exists.jsonl`） | 1（dry-run 全拒收语义，符合脚本 Task #21 修复后口径） |
| 5 | 警告触发路径实跑 | `Gateway(case_id='case_dh_drug_01', case_kind='open')` 打"相交 5 键"warning（§2.1 原文留痕） | — |

### 6.2 交付物清单

| 类别 | 路径 | 说明 |
|---|---|---|
| 报告 | `task/E1-后端数据库构造/task-06-report.md` | 本报告 |
| 状态行 | `task/E1-后端数据库构造/task-06-ingest_open_gt与药物element_count.md` | 表头状态更新（如实反映 §1 停报待裁定） |
| 进度账 | `task/E1-后端数据库构造/progress.md` | 追加第 26 条 |
| gt 构造脚本 | `src/data_build/build_open_gt_task39.py` | 新写；只读 templates/ 与 _build_state/entity_registry.jsonl，只写 _build_state/ |
| gt 文件 | `src/data_build/_build_state/open_gt_task39.json` | 15 case/17 条药物真 gt（dry-run 投闸件，非演练数据） |
| 拒收留痕 | `src/data_build/_build_state/rejections_check_molecule_exists.jsonl` | dry-run 拒收清单（覆盖了 Task #17 演练期的拒收留痕——该文件位于 _build_state/ 冻结面外，演练件本应清出，无损） |

## 7. gt 迁移手术备战（Task #41，2026-08-08）★

> §1 停报后监工裁定**方案 B**并给三项裁定（不再动摇）；本节为备战收口记录，**未执行迁移、未碰任何冻结数据**。

### 7.1 监工三项裁定

1. **7 题判废不重建**：受跨案 gt 迁出影响的 7 道已冻结 open 题（drug_03/04/05/06/11、material_01/02）判废，配比重裁归监工；
2. **全量迁出**：104 条同组成候选（89 z + 15 c，含 37 个 G_B 成员）与 gt 一并迁出，B 侧只返存在性、不返性质；
3. **E3 先行、零断裂窗**：E1 手术必须等 E3 完成判分侧改造（resolve 扩 A∪B、材料存在性通道、测试语义）后执行；本任务只备战。

### 7.2 手术脚本与 dry-run 证据（全部实跑）

| 项 | 结果 |
|---|---|
| 脚本 | `src/data_build/migrate_gt_to_B.py`（--dry-run 默认 / --execute 两档；后者前置闸读 `e3_surgery_greenlight.json` 六字段，缺件 exit 2 实跳验证）|
| 全验证（对 Sam 基线逐条断言）| 33 case / 37 G_B（22 药+15 材）/ 104 候选（89 z+15 c）全中；get_entity_A 与 search_candidates_A 各 104/104 命中、get_property_A 278 行；血缘账本 104+278+104=486；四个 *_B.jsonl 全空；104/104 B 侧注册键可解；视图盘点 34=26 保留+7 判废+1 示例 |
| 手术计划落盘（12 件，`_build_state/gt_migration_plan/`）| 三库删行清单逐行（entity_id/row_no/line_md5，104+278+104=486）、B 侧四文件注册载荷（存在性 104 行/get_entity_B 37 行身份档/get_property_B 与 search_candidates_B 保持空）+ b_partition 清占位载荷（33 道移除、保留 2 fixture）、血缘预案 486 行、26 道视图 entity_ref 销账清单、判废 7 题处置方案 |
| 自检一：确定性 | dry-run 连跑两次，12 件产物 md5 逐一相同（逐字节一致）|
| 自检二：冻结面零写入 | dry-run 前后 `generate_checksums.py --verify` 均 exit=0（主清单 48 + 附录 2 全绿），CHECKSUMS 本体 md5 `c013c62406b17b52d54c1e99f79aa87e` 零漂移 |
| 判废 7 题处置方案 | templates 建议标记废弃不删除（保审计链，归属 E5 写作区）；b_partition 占位随 33 道整批清除——只出方案未动手（见 `deprecated_cases_plan.json`）|

### 7.3 两份交接清单落点

| 清单 | 落点 |
|---|---|
| E3 交接（五项：resolve 扩 A∪B / 材料通道 4 primary+11 降级键 / 5 测试改写 / L1078 警告语义 / ADR-0018 §8 第三行修订建议）| [`task-06-E3手术交接说明.md`](task-06-E3手术交接说明.md)；材料侧身份键交接说明 §10 增补第④项指路 |
| E5 知会（判废 7 题清单 / 26 道视图改 entity_ref 口径 / 执行时点）| [`per-case视图交接说明.md`](per-case视图交接说明.md) §8 |

### 7.4 执行前置条件（双硬闸）

1. **E3 完工**：task-06-E3手术交接说明.md 五项落地，`_build_state/e3_surgery_greenlight.json` 六字段置真（①②③④对应四项改造 + e3_completed + supervisor_notice）；
2. **监工执行通知**（配比重裁完成后）——两者缺一，`--execute` 拒绝（exit 2 已实证）。

未决点登记（归 E3 契约升版）：B 侧注册键终形（真键 vs 匿名域 dummy 形态 vs ID 复用，Sam §9 R9）——影响存在性载荷与视图 entity_ref 口径，定形后 E1 重生载荷。

---

## 边界自查（Task #39）

- 只写：`task/E1-后端数据库构造/` 三份文档（本报告、task-06 状态行、progress.md）、`src/data_build/build_open_gt_task39.py`（新脚本）、`src/data_build/_build_state/` 下两件产物（冻结扫描面外）。
- 未动：`dag_poc/data/backend/` 任何文件（CHECKSUMS 零漂移为证）、gateway.py、query_spec.json、共享账本、b_partition.json（本轮销账核查不碰冻结/交接文件）、四个 *_A.jsonl、anonymization_mapping.json。
- E1-02 纪律：演练 fixture（`rehearsal_open_gt.json`）零混入——未作为投闸件、未执行 `--clean` 之外的任何演练动作；本任务全程无 LLM 调用（无需 DogFooding 日志）。
- 未自行立 ADR、未越界改 E3 警告逻辑。

## 边界自查（Task #41）

- 只写：`src/data_build/migrate_gt_to_B.py`（新手术脚本）、`_build_state/gt_migration_plan/`（12 件 dry-run 产物，冻结扫描面外）、本 domain 文档四份（本报告补节、task-06 状态行、progress.md、task-06-E3手术交接说明.md 新开）+ 两份既有交接文档增节（材料侧身份键交接说明 §10、per-case视图交接说明 §8）。
- 未动：`dag_poc/data/backend/` 任何文件（CHECKSUMS 零漂移为证）、gateway.py、b_partition.json、templates/、E5 视图、anonymization_mapping.json、共享账本；未执行迁移（--execute 闸实测拒绝）；全程无 LLM 调用；未自行立 ADR（§8 第三行仅提建议）。
