# Task E1-06 · gt 迁移手术 — E3 交接清单（判分侧改造，先行域）

| 字段 | 值 |
|---|---|
| 任务 | Task #41（gt 迁移手术备战）交付物 2；依据监工三项裁定（2026-08-08）+ Sam 调研报告（task-06-gt迁移影响面调研.md §4/§5/§8）|
| 交接方 | E1 数据线（2026-08-08）|
| 消费方 | **E3**（判分侧/契约/gateway 域，裁定三"先行"方）|
| 指路 | 材料侧身份键交接说明.md §10 第④项有指路条目（两处互指，内容以本文为准）|
| 状态 | **E1 侧备战完毕、等 E3 开工**；E3 未完工前 E1 手术不得执行（零断裂窗）|

> **一句话**：方案 B 的执行顺序是 **E3（判分侧改造）→ E1（数据手术）→ E5（视图与题面）**。本文是 E3 的完整工作清单——五项，全部附文件与行位；其中 ①②③④ 是 E1 手术 `--execute` 前置闸的四个核验字段（见 §3），⑤ 是 E1 只提建议、归 owner/E3 落 ADR 的口径修订。

---

## 0. 背景三行（自包含）

1. **为什么迁**：open case 的 gt 物理上在 A 库（ADR-0018 §2.3 逻辑 B 设计），`b_partition.json` 33 道占位登记 A 身份键，gateway.py L1078 每次构造 Gateway 打一次性警告；Task #39 实证 ingest_open_gt 正常执行无法归零（dry-run 接受 0/拒收 17，隔离闸正确拒收），监工裁定方案 B：物理迁出。
2. **迁什么**（裁定二，全量）：104 个同组成候选实体（89 z + 15 c，含 37 个 G_B 成员）从 A 侧三库删除（get_entity_A/search_candidates_A 各 104 行、get_property_A 278 行，合计 486 行），B 侧注册存在性（只返 bool、不返性质）。
3. **为什么 E3 先行**（裁定三，零断裂窗）：判分侧 resolve() 组成索引只建在 get_entity_A 全库上（§1），迁出瞬间 33 道 open 题 hit_gb 恒 False——E3 不先改完，手术本身造成全量 open 题零分。

---

## 1. 五项清单（逐条附文件与行位）

### ① evaluate.py resolve() 组成索引扩 A∪B —— **头号阻塞**

| 项 | 内容 |
|---|---|
| 位置 | `src/benchmark/evaluate.py` **L770–780**（数据源注释原文：「A 侧全库身份档 get_entity_A.jsonl（92,193 行；get_entity_B.jsonl 为 0 行——B 是分区概念不是物理文件，映射建在 A 侧全库身份档上）」），`_COMPOSITION_INDEX_CACHE` 及其下游 resolve()；同源收缩项：`_a_entity_id_set`（grounding 条件② anchor∈A 全集）|
| 现状问题 | 组成倒排索引只建在 get_entity_A 全库身份档上；gt 迁出后，提交组成 == gt 组成时 resolve() 解析不到任何实体 ID → hit_gb 恒 False → 33 道 open 题判分结构性断裂（Sam §4.3）|
| 改造要求 | 索引数据源扩为 **A∪B 身份档**（get_entity_A.jsonl ∪ get_entity_B.jsonl）；缓存键已含 backend 根路径与 ε 口径，数据源扩并后注意缓存失效语义；`_a_entity_id_set` 是否同扩由 grounding 语义裁定（5 个跨案锚点实体已随裁定一判废 7 题处理，anchor 校验面见 Sam §3.3）|
| E1 已就绪 | get_entity_B 载荷预案已生成：`src/data_build/_build_state/gt_migration_plan/b_side_payload_get_entity_B.jsonl`（37 行 gt 身份档，字段与 A 侧行一致，供索引直接消费）|
| 验收 | 迁出后 33 道 open 题：提交 gt 组成 resolve() 命中 G_B（hit_gb=True 路径活）；提交非 G_B 同组成不误判 |

### ② 材料存在性通道（4 primary 键 + 11 降级键，承材料侧交接 §7.1.2 裁定）

| 项 | 内容 |
|---|---|
| 契约升版 | 后端库规格契约 §3.5：`check_molecule_exists_B.jsonl` 增列 `material_identity_key`（+ `material_key_type` 区分 primary/degraded）——现行契约只许 inchikey/canonical_smiles/provenance 三字段，晶体 gt 无分子键、结构上无法承载（Sam §5.1）|
| gateway 实现 | `dag_poc/gateway.py` `BPartitionBackendJsonlReader.load` **L455–551**（存在性通道仅建两列索引于 **L528–533**，无材料分支）增材料分支；`normalize_b_key` 对材料键走字符串回落（无 RDKit 路径，与 z*/c* 回落同款，Sam §5.4/§4.2 L1615）|
| 降级键复算路径 | 11 条降级键 gt（材料 05–15）**承材料侧身份键交接说明 §7.1.2 监工裁定**：用 A 侧可见字段 formula+tc 按降级键格式复算比对（接口 B 扩展，`material_identity.degraded_material_identity_key`/`tc_tier_floor` 纯函数可直接 import）——与 grounding `==` 口径同源，不另起一套键逻辑；该扩展同时是 §10 待办①（本就归 E3），与本项合批最经济 |
| 载荷量 | primary 直键 4 条（c04592/c04551/c03293/c03296）+ 降级 11 条；E1 侧预案见 `gt_migration_plan/b_side_payload_check_molecule_exists_B.jsonl`（材料行已带 `_pending_contract` 标记，契约升版落地前不得写盘）|
| 验收 | 15 道材料 gt 经 check_molecule_exists 返 True；降级键经 formula+tc 复算可判等（实证 11,481 条口径，同 §10 待办①）|

### ③ 5 项测试语义改写（全部在 `dag_poc/mcp_mock_server/tests/test_backend_source_switch_task06.py`，Sam §4.1）

| # | 测试 | 现行语义 | 改写方向 |
|---|---|---|---|
| 1 | `test_assertion2_gt_queryable_without_view` | 无视图时 gt 必须可经 get_entity 查到 | **反向**：gt 经 A 侧必 UNKNOWN_ENTITY + 经存在性接口 exists=True |
| 2 | `test_assertion5_all_case_gt_queryable_f1` | 视图闸层 gt∉白名单 → UNKNOWN_ENTITY 与"真不存在"同码 | 断言或仍绿，但归因从"视图闸拦截"变"物理不存在"，F1 口径说明须同步 |
| 3 | `test_b_a_placeholder_overlap_registered_not_raised` | 占位 b_ids 非空且放行不 raise | **反转**：放行集为空（占位清零）|
| 4 | `test_machine_b_a_resolution_direction_locked` | `assert checked > 0`（docstring 已自带消解预告）| 按预告更新（归零口径）|
| 5 | `test_machine_f5_judge_anchor_set_backend` | gt ∈ `_a_entity_id_set`（A 全集）| 改 A∪B 口径或 B 侧独立断言（与①同批）|

**另**：触 `b_partition`/`b_set`/`g_b`/`placeholder` 的相关测试文件共 **14 个**（test_ab_isolation、test_backend_jsonl_reader、test_budget、test_evaluate_open_discovery、test_existence_interface_task05、test_gateway_rdkit_grounding、test_grounding_composition_task05、test_mock_server_contract_tools、test_pr40_case_view_gateway、test_resolve_composition_task05、test_review_merge_fixes、gwtest_common、_tmp_smoke_examples 等），手术后全量复跑确认非语义断裂（Sam §4.1 尾）。

### ④ gateway.py L1078 警告语义改造（迁移后归零的验收口径）

| 项 | 内容 |
|---|---|
| 位置 | `dag_poc/gateway.py` **L1018–1083**（B∩A=∅ 两层装载闸）、**L1078**（一次性 warning，模块级 `_b_a_placeholder_warned`）、L1102–1128（G_B 断言同款占位放行）|
| 迁移后预期 | 104 实体迁出 A、B 侧以结构键注册（非 z*/c* 形态）→ 两层相交自然清零 → **警告不再触发（归零机制已内建，无需改代码即归零）**|
| 语义改造 | 警告文案现行写「ingest_open_gt 未执行……执行后应归零」——方案 B 下该消解路径失效，文案与触发语义须改为方案 B 口径；**占位机器判据（z*/c*+数字形态即判占位）随占位清零收窄或下线**（Sam §4.2/R7：B 侧若用结构键登记则判据自然空转）——去留归 E3 裁定（Sam §8.1 E3-5）|
| b_partition.json | 33 道 open 占位整批清除（E1 手术批执行，载荷预案见 `gt_migration_plan/b_partition_cleanup.json`，保留 2 fixture case）；fixture 退役或保留回落归 E3 裁定（Sam §8.1 E3-6）|
| **验收口径（E1 手术后的归零实证标准）** | ① 构造 33 道 open case Gateway 各一次，L1078 warning 触发次数 == 0；② 无 case 构造（顶层 fixture 回落路径）相交检查通过；③ 残留相交 raise 路径仍有效（非占位真相交必须 raise，隔离红线不放宽）|

### ⑤ ADR-0018 §8 第三行口径修订建议（E1 只提建议，落 ADR 归 owner/E3）

| 项 | 内容 |
|---|---|
| 位置 | `governance/adr/0018-open类构造方式per-case-AB配对.md` **§八 同分异构判法 表格第三行**（原文：「同组成分子仅在 A 侧出现 \| 按 §4.1 返"不存在"，不影响判分」）|
| 失效推论 | 裁定二全迁后，gt 同组成候选全集（104 个）全部迁往 B 侧——对 gt 组成族而言「同组成仅 A 侧」情形**不再存在**；该行口径的前提消失 |
| E1 建议文案（供参考，非定稿） | 第三行改为「同组成实体已全部迁至 B 侧（方案 B 手术，2026-08）：存在性经 B 侧通道判定，A 侧查询一律 UNKNOWN_ENTITY；判分经 §八 前两行口径（全满足纳入 G_B / 部分满足废弃）不变」，并在 §八 开头补一句手术背景脚注 |
| 归属 | **E1 不立 ADR、不改 ADR**——建议随本清单提交，由 owner/E3 在契约/ADR 修订批内落 |

---

## 2. 未决点两项（E3 契约升版时定形，Sam §9 R9）

1. **药物侧存在性注册键口径**：E1 预案按 Task #39 真键口径（judge registry 真 InChIKey/canonical SMILES）；Sam §6.2 另提匿名域 dummy atom 形态（agent 化学操作面在匿名域，dummy 形态才可被查中）；另有 ID 复用案（z*/c* 原 ID 迁位置）。三案取舍影响视图 gt.entity_ref 改写口径（见 per-case视图交接说明.md §8）与 gateway 占位判据去留，**须 E3-1 契约升版定形后 E1 重生注册载荷**；
2. **get_property_B/search_candidates_B 保持空**是 Sam §6.2 最简形态预案（B 侧只返存在性、永不返性质，ADR-0018 §4），若 E3 判分需要 B 侧真值另有 judge 段通道，不通过这两文件。

---

## 3. E1 手术执行闸与 E3 的接口（greenlight 契约）

E1 手术脚本 `src/data_build/migrate_gt_to_B.py --execute` 的前置闸读取
`src/data_build/_build_state/e3_surgery_greenlight.json`，**以下六字段全部为 true 才放行**（缺件 exit 2）：

| 字段 | 对应本文 |
|---|---|
| `e3_completed` / `supervisor_notice` | E3 完工声明 + 监工执行通知（裁定三双签）|
| `e3_resolve_a_union_b` | §1 ① |
| `e3_material_existence_channel` | §1 ② |
| `e3_tests_rewritten` | §1 ③ |
| `e3_warning_semantics` | §1 ④ |

E3 每完成一项即把对应字段置 true（文件不存在时 E1 收口批创建骨架）。E1 手术执行后负责：三库删行 486、B 侧注册、血缘 486 行 append、b_partition 占位清除、MANIFEST/CHECKSUMS 再冻结（入库操作手册 §8 解冻登记）；视图与模板处置归 E5（见 per-case视图交接说明.md §8）。

---

## 4. 边界声明

本文纯文档交接（Task #41），未动任何 E3 域代码/测试/契约/ADR；五项的实质口径以 Sam 调研报告（task-06-gt迁移影响面调研.md）对应节号原文为准。
