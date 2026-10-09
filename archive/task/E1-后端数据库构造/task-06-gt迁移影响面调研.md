# E1-06 Task #40 · gt 迁移手术（方案 B）影响面调研报告

| 字段 | 值 |
|---|---|
| 任务 | Task #40（调研 gt 迁移手术影响面，方案 B 前置）|
| 调研方 | E1 数据线（Sam，2026-08-08）|
| 性质 | 纯调研：未写任何数据产物、未改任何冻结文件；本文是唯一落盘件 |
| 依据 | 监工裁定方案 B（open case 的 gt 及同组成候选从 A 侧物理迁出、B 侧重新匿名注册）；task-06-report.md §1 停报实证链（dry-run 0 过闸：接受 0/拒收 17，15 条隔离闸 + 2 条 DOI 缺失）|
| 方法 | 全部数字为实跑/全量扫描（templates 33 份、views 34 份、四库全量行扫、tests 全量 grep、gateway/evaluate 逐点读），无估算值 |

---

## 0. 可行性结论与推荐实施顺序（金字塔顶）

### 0.1 一句话结论

**方案 B 技术可行，但不是 E1 单域数据手术**：实测爆炸半径为 A 侧三库 486 行删改 + B 侧四空文件填充 + 34 份视图（23 冻结 + 11 未冻结）+ 33 份 templates + b_partition.json + 6 份 MANIFEST/CHECKSUMS + provenance 账本 486 行血缘 + judge 段映射表口径裁定 + E3 判分侧两处数据源 + 5 个 E3 测试语义改写 + gateway 警告分支归零。其中存在 **1 个核心裁定点**：

> **5 个 G_B 成员同时是他案造题素材**（实算：z002195=drug_04 的锚点；c04551/c04592 互入 material_01/02 的族序列与证据集；z065584/z063321 为 4 道药物题的干扰项）。物理迁出与这 7 道已冻结题面**不可兼得**（视图载入校验对 family_sequence 长度==n、distractors 长度==K 违规即 raise），除非同步重建这 7 道题。

若裁定"5 个跨案实体豁免迁出"，则方案 B 退化为部分隔离，b_partition 警告无法真正归零（占位放行分支仍须保留），违背裁定初衷。因此**推荐"迁出 + 重建 7 道题"**，并把手术定性为 E3→E1→E5 三域联合变更。

### 0.2 推荐实施顺序（六步，阻塞关系见 §8）

| 步 | 域 | 事项 | 阻塞 |
|---|---|---|---|
| 0 | 监工 | 两项裁定：①5 个跨案 G_B 成员去留（迁出+重建 7 题 vs 豁免+部分隔离）；②67 个非 G_B 同组成候选迁出范围（104 全迁 vs 仅迁 37 G_B）| 阻塞步 2 之后全部 |
| 1 | E3 | 契约 §3.5 升版：check_molecule_exists_B 增加材料身份键通道（primary 键列 + 降级键 formula+tc 复算路径，对齐材料侧交接 §7.1.2 裁定）| 阻塞 E1 B 侧注册 |
| 2 | E3 | 判分侧数据源切换：evaluate.py resolve()/_a_entity_id_set 从 A 全库身份档扩为 A∪B 身份档（ADR-0018 R1-② 归 E3）；同步改写 5 个测试语义（§4.1）| 阻塞手术生效（否则迁出即判分断裂）|
| 3 | E1 | 数据手术：三库删行（104 或 37 实体，486 或相应行数）→ provenance 补血缘 → B 侧四文件注册 → b_partition 占位清除 → MANIFEST/CHECKSUMS 再冻结（入库操作手册 §8 解冻登记）| 依赖步 0/1 |
| 4 | E5 | 33 份视图 gt.entity_ref 改 B 侧 ID + 7 份跨案视图重建 + templates isomer_handling/g_b 同步 + 复跑 assert_per_case_view.py 与造题质量门 | 依赖步 3 的 B 侧新 ID 清单 |
| 5 | 收口 | 统一冻结 + 全量冒烟（重点：B 文件非空触发 default_b_partition_reader 自动切 JSONL 读取器的行为切换点）+ E2 query_lint O4 双向查 + 警告归零实证 | 依赖步 3/4 |

---

## 1. 清单一：gt 实体全量清单与口径对齐

### 1.1 19 vs 22 差集（James 实跑口径对齐结果）

task-06-report.md 记载的"19 道带 g_b vs 22 道占位"差集 = **3 道 spare_dh_drug 备胎题**：
- **19** = d8b5ac3 时点带 g_b 注册的 open case（15 药 + 4 材料首批），其中 15 药（17 条 gt 记录）进 dry-run：接受 0/拒收 17（15 隔离闸 + 2 DOI 缺失）；4 材料为晶体、双键列无法承载未进投闸集（task-06-report.md §2.2 表 3–4）；
- **22** = 19 + 3 道 spare（spare_dh_drug_01/02/03 当时已在 b_partition.json 有占位登记，但 g_b 注册口径未计入实跑批）；
- **当前态（已实测）**：b_partition.json 已扩至 2 fixture + **33 道 open 占位**（15 药 + 15 材料 + 3 spare），与 templates/discovery_haystack/ 33 份带 g_b 的 case **一一对齐**；材料 05–15 共 11 道为 E5 扩数据后新注册（占位从 22→33）（该文件当前 git 状态为 `M` 未提交）。

### 1.2 gt 全量清单（33 道 / G_B 评分集 37 成员 / 同组成候选 104）

三层口径（全部实算自 templates `g_b` + `isomer_handling` 声明）：

| 口径 | 数量 | 定义 |
|---|---|---|
| open case 数 | **33**（15 药 + 15 材料 + 3 spare）| templates 带 g_b 声明的 case |
| G_B 评分集成员（distinct）| **37**（22 药 + 15 材料）| satisfy_window：有目标性质且满足窗口，命中任一即得分（ADR-0018 §8）|
| 同组成候选全集（distinct）| **104**（89 z + 15 c）| same_composition_entities 并集；其中 67 个缺目标性质（missing_logS/tc）不入 G_B（§4.3 存在≠得分）|

每道 case 的 G_B 成员（匿名实体 ID）：

| case | G_B 成员 | 同组成候选数（含缺性质）|
|---|---|---|
| drug_01 | z064386 | 5 |
| drug_02 | z062680 | 2 |
| drug_03 | z001336 | 3 |
| drug_04 | z011837 | 2 |
| drug_05 | z065578 | 1 |
| drug_06 | z045475 | 1 |
| drug_07 | z012362 | 1 |
| drug_08 | z002195 | 2 |
| drug_09 | **z000008, z006792, z063325**（3 成员）| 5（另 z060640/z060641 缺 logS）|
| drug_10 | z070014 | 1 |
| drug_11 | z065584 | 3 |
| drug_12 | z007224 | 1 |
| drug_13 | z006758 | 1 |
| drug_14 | z043948 | 1 |
| drug_15 | z045546 | 2 |
| material_01..15 | c04592, c04551, c03293, c03296, c10199, c10545, c08114, c12895, c16897, c06524, c15707, c12344, c08275, c07576, c12566（各 1）| 各 1（材料线无同组成冗余，gt=唯一候选）|
| spare_01 | z043848 | 11 |
| spare_02 | **z004015, z018577, z063321**（3 成员）| 9（另 6 条缺 logS）|
| spare_03 | z001332 | 3 |

非 G_B 的 67 条缺性质候选**全部在药物侧**（材料线 gt 即唯一候选），且全量扫描确认**无任何跨 case 引用**（§3.3），迁出自由度最高。

---

## 2. 清单二：A 侧四库行分布与迁出行数变化（全量实算）

### 2.1 104 个候选实体的现行分布

| 库 | 命中 | 明细 |
|---|---|---|
| get_entity_A.jsonl（92,193 行）| **104/104** | 全部在 A 身份档（根因即此：ADR-0018 §2.3 逻辑 B，gt 物理就在 A 全集）|
| search_candidates_A.jsonl（92,193 行）| **104/104** | 与身份档同行对齐 |
| get_property_A.jsonl（240,664 行）| **278 行** | 分布：2 行×66 实体 / 3 行×21 / 4 行×2 / 5 行×15（材料每道 5 行：tc/sc_class/family/element_count/atom_count 系）|
| literature_search_A.jsonl（309 篇）| **0 行** | 309 篇文献均无实体 ID 引用（纯 keywords 检索 + 视图 literature_doc_ids 白名单过滤），**迁出不触文献库** |

### 2.2 迁出后行数变化（按裁定范围两档）

| 方案 | get_entity_A | get_property_A | search_candidates_A | 文献库 |
|---|---|---|---|---|
| 全迁 104 | 92,193 → 92,089（-104）| 240,664 → 240,386（-278）| 92,193 → 92,089（-104）| 不变 |
| 仅迁 37 G_B | 92,193 → 92,156（-37）| 240,664 → 240,521（-143）| 92,193 → 92,156（-37）| 不变 |

两档性质行数均已实算拆分：G_B 37 实体 = **143 行**（3 行×20 / 4 行×2 / 5 行×15，材料全在此组）；非 G_B 67 实体 = **135 行**（2 行×66 / 3 行×1，全部缺目标性质行）。67 条虽无 logS/tc，但各有 2 行普通性质（logP/MW 系），迁出同样触 get_property_A。

### 2.3 同组成候选是否需迁出（ADR-0018 §8 判法的迁移推论）

- **37 个 G_B 成员必须迁出**——方案 B 的前提（否则 hit_gb 的 resolve 命中集与 A 侧可达性矛盾依旧）。
- **67 个缺性质候选**：§8 判法下它们"因无目标性质不入 G_B"（§4.3 存在≠得分），逻辑 B 设计中留 A 无害；但物理隔离口径下它们与 gt **组成全等**，留在 A 侧意味着 agent 可经 search_candidates 的 composition 约束查得"同组成实体存在于 A"——不构成得分泄漏（无性质可查）但构成**组成 oracle 推断面**（agent 可确认"目标组成的实体在此库生态中出现过"）。**两档口径提交监工裁定（§0.2 步 0-②）**；本报告实证：67 条无跨案引用（§3.3），全迁无造题面代价，仅增 67 实体行删改与 B 侧注册工作量。

---

## 3. 清单三：per-case 视图依赖（34 份全量扫描）

### 3.1 视图总盘与冻结状态

| 状态 | 数量 | 明细 |
|---|---|---|
| 生产视图 | **33** | 15 药 + 15 材料 + 3 spare，一 case 一文件 |
| 示例视图 | 1 | e105_example_001.json（机制演示，gt 指向 A 库实体是刻意设计）|
| **已冻结**（CHECKSUMS 主清单）| **23** | 15 药 + 3 spare + 4 材料（01–04）+ 1 示例 |
| **未冻结**（git untracked）| **11** | 材料 05–15 视图（E5 扩数据批，尚未走入库冻结）|

### 3.2 自家 gt 引用（迁出后全部需改）

33 份生产视图全部在 `/gt/entity_ref` 声明自家 gt（`side:"B"`，另带 `composition` 匿名组成多重集）。**迁出后 entity_ref 须改 B 侧注册 ID**——这正是视图机制预留的原生口径（per-case视图交接说明.md §2：「生产 case 经 ingest_open_gt 入库后填 B 侧 ID」；交接说明 §2 另注：示例视图的 gt 指向 A 库实体仅是机制演示）。`composition` 字段与侧无关，不需改。

### 3.3 跨 case gt 引用（迁出即断的硬冲突，实算到字段路径）

全量扫描 33 templates + 34 views 的全部字符串字段，对 104 候选做跨 case 归属比对，**命中且仅命中 5 个 G_B 成员、7 处消费**（67 条非 G_B 候选零跨案引用）：

| 被引用实体 | G_B 属于 | 消费 case 与精确位置 | 迁出后果 |
|---|---|---|---|
| z002195 | drug_08 | drug_04：`/anchor`、`/edit_op/anchor`、`/evidence_set[4]/entity_id`、`/family_sequence[4]`、visible_set | **最重**：drug_04 锚点迁出后 edit_molecule 锚点判 ∉A → ANCHOR_NOT_IN_A，整题立断；family 长度==n 校验 raise |
| c04551 | material_02 | material_01：`/evidence_set[0]/entity_id`、`/family_sequence[0]`、visible_set | 族序列首员缺失，单调链断、长度校验 raise |
| c04592 | material_01 | material_02：`/evidence_set[2]/entity_id`、`/family_sequence[2]`、visible_set | 同上（material_01/02 互为素材，成对处理）|
| z065584 | drug_11 | drug_05：`/distractors[23]`；drug_06：`/distractors[24]` | K 减员，distractors 长度==K 校验 raise |
| z063321 | spare_02 | drug_03：`/distractors[21]`；drug_11：`/distractors[22]` | 同上 |

受影响消费 case 共 **7 道**：drug_03/04/05/06/11、material_01/02。

### 3.4 归属判断：视图改写归 E5（依据 + 分工建议）

**判断依据**（per-case视图交接说明.md 明文）：
- §1 表：交付方 E1 提供机制（per_case_view.py/断言集/示例），**消费方 E5「按 §2 格式写 30 份视图、按 §4 调穷举接口过质量门」**——视图内容是 E5 造题资产；
- §2 视图格式约束（n/d/K、family 有序、K≥10）是 **ADR-0018 §9 交付标准 2 的造题参数**，改 family_sequence/distractors 属于**重新造题**（重选素材须过 A6 穷举质量门与单调性复核），非数据搬运；
- §5 断言集 A1–A8 是视图正确性验收门，重跑义务在视图作者侧。

**结论**：entity_ref 批量换 ID（机械操作）与 7 道视图重建（造题操作）**均归 E5**；E1 可在手术批内顺手代跑机械换 ID，但 7 道重建与质量门复验必须 E5 署名。若监工要求缩短关键路径，可裁定 E1 代执行 + E5 复核，责任界面以复核署名为准。

---

## 4. 清单四：E3 依赖（测试全量 grep + gateway 消费点 + 判分侧隐藏依赖）

### 4.1 断言"gt 可经 A 侧工具查到"的测试（逐条）

全量 grep `dag_poc/mcp_mock_server/tests/` 与 `src/tests/`（后者无相关断言）：

| # | 文件 | 测试/断言 | 现行语义 | 迁出后 |
|---|---|---|---|---|
| 1 | tests/test_backend_source_switch_task06.py | `test_assertion2_gt_queryable_without_view`（断言 2）| **无视图时 gt 必须可经 get_entity 查到** | **直接冲突**——方案 B 下 gt 物理不在 A，须改为"gt 经 A 侧必 UNKNOWN_ENTITY + 经存在性接口 exists=True"的反向语义（任务书已知冲突点，实锤在此）|
| 2 | 同上 | `test_assertion5_all_case_gt_queryable_f1`（断言 5）| 分层：视图闸层 gt∉白名单 → UNKNOWN_ENTITY 与"真不存在"同码 | 断言或仍绿，但**归因漂移**：从"视图闸拦截"变"物理不存在"，F1 口径说明须同步 |
| 3 | 同上 | `test_b_a_placeholder_overlap_registered_not_raised` | 断言占位 b_ids **非空**且放行不 raise | **失败**——占位清零后 b_ids 为空，断言须反转为"放行集为空" |
| 4 | 同上 | `test_machine_b_a_resolution_direction_locked` | `assert checked > 0`；docstring 明写「ingest_open_gt 执行后放行集归零，本断言需同步更新」| **失败**（已自带消解预告，改造方向既定）|
| 5 | 同上 | `test_machine_f5_judge_anchor_set_backend`（F5）| gt ∈ `_a_entity_id_set`（judge 锚点集建在 A 全集上）| **失败**——gt 迁出 A 全集，F5 须改为 A∪B 口径或 B 侧独立断言 |

其余触 `b_partition`/`b_set`/`g_b` 的测试文件共 **14 个**（多为行为兼容性测试，迁出后需复跑确认非语义断裂）：test_ab_isolation、test_backend_jsonl_reader、test_budget、test_evaluate_open_discovery、test_existence_interface_task05、test_gateway_rdkit_grounding、test_grounding_composition_task05、test_mock_server_contract_tools、test_pr40_case_view_gateway、test_resolve_composition_task05、test_review_merge_fixes、gwtest_common（夹具）、_tmp_smoke_examples 等。

### 4.2 gateway.py 消费点（逐行实测）

| 位置 | 消费 | 迁出影响 |
|---|---|---|
| L93 | `DEFAULT_B_PARTITION_PATH`（b_partition.json 过渡 fixture 路径）| B 文件非空后 reader 自动切走，fixture 退居回落 |
| L455–551 | `BPartitionBackendJsonlReader`：存在性通道仅建 **inchikey/canonical_smiles 两列索引**（L528–533）；其余 *_B.jsonl 按 case_ref 过滤、缺 case_ref 即 raise | **无材料分支**——晶体 gt 无法入索引（清单五）|
| L554–564 | `default_b_partition_reader`：backend 布局有该 case B 数据 → 切 JSONL 读取器，否则回落 fixture | **行为切换点**：B 侧首行落盘即全体 open case 运行时换读取器，须冒烟 |
| L1005–1012 | reader.load → `b_set`/`g_b`（normalize_b_key 归一）| 键空间从 z*/c* 占位变为 B 侧注册键 |
| L1018–1083 | B∩A=∅ 两层装载闸：层 1 vs A 实体 ID 全集（92,193）、层 2 vs A canonical SMILES 全集；占位机器判据放行（z*/c*+数字 / provenance「占位」/ placeholder_warning）+ **L1078 一次性 warning**（模块级 `_b_a_placeholder_warned`）| 迁出后两层相交自然清零 → 放行集空 → **警告不再触发（归零机制已内建）**；占位放行分支是否移除归 E3 裁定（保留无害、移除更干净）|
| L1102–1128 | G_B⊆B 与 G_B∩A=∅ 断言（同款占位放行）| 同上归零 |
| L1615 | `check_molecule_exists`：`exists = normalize_b_key(mid) in self.b_set` | 存在性语义不变；但材料键归一需新通道（清单五）|

### 4.3 判分侧隐藏依赖（本次调研最重大的非显性发现）

`src/benchmark/evaluate.py`（L777–780 注释原文）：**resolve() 组成倒排索引建在 get_entity_A.jsonl 全库身份档（92,193 行）上**，注释明写「get_entity_B.jsonl 为 0 行——B 是分区概念不是物理文件，映射建在 A 侧全库身份档上」。推论链：

1. `hit_gb = 𝟙[resolve(提交组成) ∩ G_B ≠ ∅]`（ADR-0018 R1-②）；
2. gt 迁出 get_entity_A 后，**提交组成 == gt 组成时 resolve() 解析不到任何实体 ID → hit_gb 恒 False → 33 道 open 题判分结构性断裂**；
3. `_a_entity_id_set`（grounding 条件② anchor∈A 全集）同源收缩——若 5 个跨案锚点类实体迁出，相关 anchor 校验连带受影响。

**归属**：ADR-0018 R1-② 明定组成↔实体 ID 映射归 E3 判分侧。**结论：手术批必须同批把判分侧数据源扩为 A∪B 身份档（get_entity_B.jsonl 填 gt 身份档行入索引），否则手术本身造成全量 open 题零分。此为方案 B 的头号阻塞，优先级高于数据搬运本身。**

---

## 5. 清单五：材料 gt 的 B 侧存在性通道（晶体无分子键）

### 5.1 现状：结构上无法承载（实锤）

- 15 道材料 gt 全部是晶体：无 InChIKey、无 canonical SMILES（ADR-0017 §4 口径：5,771 条晶体无 SMILES/InChIKey，身份键 = 匿名域 reduced_formula + 空间群）；
- `check_molecule_exists_B.jsonl` 契约形态仅两列身份键（inchikey / canonical_smiles）+ provenance（后端库规格契约 §3.5）；gateway `BPartitionBackendJsonlReader.load` 只对该两列建哈希集（L528–533）——**无材料分支**；
- task-06-report.md §2.2 已实证：4 道材料 gt 因此"未进投闸集"。

### 5.2 15 道材料 gt 的键型拆分（实查 material_identity_keys.jsonl）

| 键型 | 数量 | gt | 键形态 |
|---|---|---|---|
| primary（主键）| **4** | c04592, c04551, c03293, c03296 | `<匿名域 reduced_formula>_SG<空间群号>`，如 c04592 = `[17*]8[24*]6[60*]_SG148`（judge 段真身 La1Mo6Se8 / mp-37784 / R-3）|
| degraded（降级键）| **11** | 材料 05–15 | `<formula>_ALT_<tc档>_K<元素种类数>`，如 `[22*]692[33*]100…_ALT_91_K5`（UCI 源无 CIF/空间群）|

### 5.3 既有口径与裁定（材料侧身份键交接说明.md）

- §4：材料身份键取用接口 A（读 `_build_state/material_identity_keys.jsonl` 键列，17,252 行，md5=9f6ad6c7…）/ 接口 B（纯函数现算）；
- §7.1.1：降级键判别力弱（同公式+同档不同空间群合并，冲突组 82/164 条）——**E5 造题对降级键只能用于排除、不得用于唯一性判定**；
- §7.1.2（Task #34 登记，**E3/E5 硬前置条件**）：缺口事实 = gateway `_material_key_universe()` 现算仅覆盖 5,740 primary，11,481 条降级键判分面不可见；**监工裁定：降级键纳入 grounding `==` 口径，归 E3 扩展接口 B（用 A 侧可见字段 formula+tc 按降级键格式复算比对）；E3 未完成并实证前，E5 不得按降级键可当锚点造材料题**。

### 5.4 结论：需 E3 新增材料通道（契约升版）

存在性接口返 bool（非唯一性判定），与降级键"仅排除不判唯一"约束**语义相容**；但承载需要：
1. `check_molecule_exists_B.jsonl` 增列 `material_identity_key`（primary 4 条直键命中）；
2. 降级 11 条走 §7.1.2 裁定的 formula+tc 复算路径（E3 接口 B 扩展，与判分 grounding 同源同口径，避免两套键逻辑）；
3. 契约 §3.5 升版登记 + gateway `BPartitionBackendJsonlReader` 增材料分支 + `normalize_b_key` 对材料键的归一口径（材料键无 RDKit 路径，原样字符串口径即可，与现有 z*/c* 分支同款回落）。

---

## 6. 清单六：B 侧注册形态（四空文件要长成什么样）

### 6.1 现状（实测）

四个 *_B.jsonl 均为空文件（md5=`d41d8cd98f00b204e9800998ecf8427e`）：check_molecule_exists_B / get_entity_B / get_property_B / search_candidates_B。b_partition.json 33 道占位（members/g_b 登记 A 身份键，provenance 明写「B 侧未入库，空占位」）。

### 6.2 迁移后目标形态（按契约 §3.2/§3.5 + ADR-0018 §4）

| 文件 | 目标内容 | 字段口径 |
|---|---|---|
| check_molecule_exists_B.jsonl | **37 G_B 成员**（或按裁定 104）存在性登记 | 药物：inchikey + canonical_smiles（匿名域 dummy atom 形态）；材料：material_identity_key 新列（§5.4）；provenance.note 承载 case_ref（契约 v1.2 裁定二：存在性通道无 case_ref 列）|
| get_entity_B.jsonl | **gt 身份档**（judge/判分侧持有；A 侧读取白名单只许 *_A.jsonl，B 文件不进任何 A 查询路径）| 供 evaluate.py resolve() 建 A∪B 组成索引（§4.3 头号阻塞的解法落点）|
| get_property_B.jsonl | **保持空** | ADR-0018 §4：B 侧 gt 对 agent 只返存在性 bool、永不返性质（§4.3 存在≠得分）；判分如需真值走 judge 段 |
| search_candidates_B.jsonl | 视成员登记范围：若登记非 gt 的 B 成员则按 case_ref 必填写入；最简形态可为空（members 语义由存在性通道承载）| case_ref 缺即装载 raise（契约硬约束）|

### 6.3 匿名口径（ADR-0018 §4 逐条）

- 存在性三分支：m∈B → true；m∈A 或库外 → false；
- 返回体只含 exists/attempt/attempts_remaining，**不含任何性质/相似度/距离**；
- 不存在返回体逐字节相同（防源探测）；max_attempts≤10 服务端计数。
- **ID 复用建议（本报告提出，供裁定）**：B 侧注册沿用 z*/c* 匿名 ID（物理位置迁移、ID 不变），则 anonymization_mapping.json judge 段、E2 题库、视图 entity_ref 可零改动或最小改动；若新发 B 侧 ID，则映射表/视图/templates 须全量换键，爆炸半径显著放大。注意：若 B 侧登记键采用 z*/c* ID 形态（而非结构键），gateway 占位机器判据（z*/c*+数字形态即判占位）会把真 B 成员误判为占位——**该判据须同批收窄或下线**（归 E3）；若 B 侧一律用结构键（InChIKey/SMILES/material_identity_key）登记，则 z*/c* 形态不进入 b_set，判据自然空转，可随占位清零一并下线。

---

## 7. 清单七：冻结面影响（CHECKSUMS 逐文件 + judge 段 + provenance）

### 7.1 CHECKSUMS 主清单全量（37 文件，md5=`06232bd217a592dbe51621f5eee1c386`，2026-08-08T01:59:14Z）+ 手术触碰标注

| 文件 | 现 md5（前 12 位）| 手术是否触碰 |
|---|---|---|
| MANIFEST.json（根）| 920eb146487b | **触**（行数/汇总刷新）|
| get_entity/MANIFEST.json | a992ef08095e | **触** |
| get_entity/get_entity_A.jsonl（30.9MB / 92,193 行）| 04305683b9a0 | **触**（删 104 或 37 行）|
| get_entity/get_entity_B.jsonl（0 行）| d41d8cd98f00 | **触**（填 gt 身份档）|
| get_property/MANIFEST.json | f25ad33a5fa2 | **触** |
| get_property/get_property_A.jsonl（17.3MB / 240,664 行）| afa3e79eca25 | **触**（删 278 或 143 行）|
| get_property/get_property_B.jsonl（0 行）| d41d8cd98f00 | 不触（保持空，§6.2）|
| search_candidates/MANIFEST.json | 585db2d24031 | **触** |
| search_candidates/search_candidates_A.jsonl（42.2MB / 92,193 行）| a9af11a08199 | **触**（同 get_entity_A）|
| search_candidates/search_candidates_B.jsonl（0 行）| d41d8cd98f00 | 视裁定（最简保持空）|
| check_molecule_exists/MANIFEST.json | c1ee974af787 | **触** |
| check_molecule_exists/check_molecule_exists_B.jsonl（0 行）| d41d8cd98f00 | **触**（填 37 或 104 存在性行）|
| literature_search/MANIFEST.json | ab1a090acbd3 | 不触 |
| literature_search/literature_search_A.jsonl（309 行）| 993989f1e2aa | 不触（0 实体引用）|
| per_case_views/ 23 份（drug_01–15、spare_01–03、material_01–04、e105_example_001；md5 如 drug_01=0e8ac16a1dd0 … spare_03=92a908630b3c）| 见清单 | **触 22 份**（全部生产视图 entity_ref 改写；e105_example_001 是机制演示件不触）；其中 7 份跨案视图内容重建（§3.3）|
| **CHECKSUMS 本体** | 06232bd2… | **触**（全量重算）|
| 附录交接产物：enumerate_A.json（md5=5635cc18…）| — | **风险触**：若 E5 复跑 test_ab_isolation 穷举会重写该附录（静默窗约束）|
| 附录交接产物：material_identity_keys.jsonl（17,252 行，md5=9f6ad6c7…）| — | 不触（键列不变；B 侧注册只消费它）|

**清单外但同批触碰**：
- `per_case_views/case_dh_material_05..15.json`（11 份，git untracked 未冻结）——同批改写后**首次入冻结清单**；
- `dag_poc/mcp_mock_server/b_partition.json`（git `M` 未提交）——不在 CHECKSUMS 域内（E3 运行时资产），占位清除或整文件退役（B 文件非空后 reader 不再消费它，保留回落价值由 E3 裁定）；
- `dag_poc/templates/discovery_haystack/*.json` 33 份（isomer_handling/g_b 记录 A 侧 ID）——不在 backend 冻结域，归 E5 题面资产同步。

**触碰合计**：冻结域内 ≥ 31 个文件变更（3 A 库 + 2 B 文件 + 5 MANIFEST + CHECKSUMS 本体 + 22 视图）+ 11 份新入册 + 域外 34 份（b_partition + templates 33）——远超任务书"23+"预估口径。

### 7.2 anonymization_mapping.json judge 段（真实身份反查）

实测 judge 段（visibility=judge_only）含**全部 104 个候选实体的真实身份**：分子侧 74,941 条（z→InChIKey/canonical SMILES/真名公式），晶体侧 5,771 条（c→formula_sc/mp-id/空间群，如 c04592=La1Mo6Se8/mp-37784/R-3）。
- 迁出后 judge 段**必须保留这些条目**：判分反查（hit_gb 真值核对、DOI/来源追溯）依赖它；物理迁移不改真实身份；
- 若采 §6.3"ID 复用"建议：judge 段**零改动**；若新发 B 侧 ID：须增 104 条 B 侧 ID→真实身份条目（映射表为 E1-03 冻结资产，触之须解冻登记）；
- agent 可见性不受影响（judge_only 域，四道匿名红线扫描口径不变）。

### 7.3 provenance 账本血缘

`src/data_build/_build_state/provenance_ledger_*.jsonl` 中 gt 相关行实测（键格式 `entity_id` 或 `entity_id|prop`）：get_entity **104** 行 + get_property **278** 行 + search_candidates **104** 行 = **486 行**。手术须按入库操作手册口径**补迁移血缘**（A 侧删除事件 + B 侧注册事件，来源指向本手术批与裁定号）；账本为 append-only，不改历史行。

### 7.4 解冻登记（入库操作手册 §8）

§8 现存 4 条历史解冻记录（E1-05 Task #30/#31/#35、Task #38，均监工授权 + 静默窗约定）。本手术为**迄今最大解冻面**（31+ 冻结文件），须新登记一条：授权依据（方案 B 裁定）、静默窗起止、复跑项清单（assert_per_case_view.py / generate_checksums.py --verify / E3 测试全量 / E2 query_lint O4）。

---

## 8. 清单八：改动面划分（E1 / E3 / E5）与阻塞关系

### 8.1 三域清单

**E3（代码与契约域，先行）**
| # | 事项 | 依据 |
|---|---|---|
| E3-1 | 后端库规格契约 §3.5 升版：check_molecule_exists_B 增材料身份键列/复算路径 | §5.4 |
| E3-2 | gateway `BPartitionBackendJsonlReader` 材料分支 + normalize_b_key 材料键口径 | §5.4 |
| E3-3 | evaluate.py resolve()/_a_entity_id_set 数据源扩 A∪B 身份档（头号阻塞）| §4.3 |
| E3-4 | 测试语义改写 5 项（§4.1 表）+ 14 个相关测试复跑 | §4.1 |
| E3-5 | gateway 占位放行分支与 L1078 警告处置裁定（归零后收窄/下线/保留）| §4.2 |
| E3-6 | b_partition.json fixture 退役或保留裁定 | §7.1 |

**E1（数据手术域，中军）**
| # | 事项 | 依据 |
|---|---|---|
| E1-1 | A 侧三库按裁定范围删行（104/37 实体；486/143+行）| §2 |
| E1-2 | B 侧注册：check_molecule_exists_B + get_entity_B（判分身份档）| §6.2 |
| E1-3 | provenance 补 486+ 行迁移血缘 | §7.3 |
| E1-4 | MANIFEST×6 + CHECKSUMS 再冻结、§8 解冻登记 | §7.1/§7.4 |
| E1-5 | （可代跑）视图 entity_ref 机械换 ID——责任界面见 §3.4 | §3.2 |

**E5（题面重建域，殿后）**
| # | 事项 | 依据 |
|---|---|---|
| E5-1 | 33 份视图 gt.entity_ref 改 B 侧 ID（验收：assert_per_case_view.py A1–A8）| §3.2 |
| E5-2 | **7 道跨案视图重建**（drug_03/04/05/06/11、material_01/02）：重选 family/distractor/anchor 素材 + 单调链与 A6 穷举门复验 | §3.3 |
| E5-3 | templates isomer_handling/g_b/provenance 同步（含 spare 批）| §1.2 |
| E5-4 | 材料 05–15 视图 11 份入库冻结（本就欠的冻结动作，随批完成）| §3.1 |
| E5-5 | E2 query_lint O4 g_b 注册双向查复跑 | 收口 |

### 8.2 阻塞关系图（线性关键路径）

```
监工裁定（5 跨案实体去留 + 67 候选范围）
   └→ E3-1/E3-2（材料通道契约+实现）──┐
   └→ E3-3（判分侧 A∪B 数据源）───────┤
                                       ├→ E1-1..E1-4（数据手术+再冻结）
         E3-4/E3-5/E3-6 可并行 ────────┘        │
                                                 └→ E5-1..E5-5（视图与题面）
                                                       └→ 统一冻结+冒烟收口
```
关键路径长度由 **E3-3（判分侧）与 E5-2（7 道题重建）** 决定；E3-2 材料通道若走 §7.1.2 接口 B 扩展，与 E3-3 同源可合并一批。

---

## 9. 风险与不确定项（实算优先）

| # | 风险 | 实证/实算状态 |
|---|---|---|
| R1 | **5 个 G_B 成员跨案消费 → 7 道题面硬断**：drug_04 锚点迁出即 ANCHOR_NOT_IN_A 整题立断；material_01/02 族序列单调链断（family 长度==n 载入即 raise）；4 道 distractors 减员（长度==K raise）| **已实算到字段路径**（§3.3 表）。"同组成候选迁出后 A 内单调序列是否被破坏"的答案：**是，且破坏点精确为 material_01 family[0]、material_02 family[2]、drug_04 family[4]（兼锚点）**。重建 7 题的素材重选须重新过 A6 穷举门，工作量归 E5 评估 |
| R2 | **判分断裂**：resolve() 索引只建在 A 全库身份档上，迁出即 33 道 open 题 hit_gb 恒 False | 已实证（§4.3，evaluate.py 注释原文）。解法确定（A∪B 数据源），未实施 |
| R3 | **材料降级键 11/15**：§7.1.2 硬前置条件（E3 接口 B 扩展未完成并实证 11,481 条可判等前受限）；本手术的材料存在性通道与其同根，E3 该扩展是手术最长杆 | 已实证裁定原文（§5.3）|
| R4 | **67 条非 G_B 候选留 A 的组成 oracle 面**：agent 可经组成约束确认"目标组成在 A 出现过"（不得分但泄漏存在性线索）；全迁则无此面但工作量+67 实体 | 已实证无跨案引用（§2.3/§3.3）；两档待裁定 |
| R5 | **reader 自动切换的行为突变**：B 文件首行落盘即 default_b_partition_reader 全体 open case 切 JSONL 读取器（含 has_case_data 判据、case_ref 必填 raise 路径）——切换瞬间 33 道运行时行为变化，须全量冒烟 | 已实证切换逻辑（gateway L554–564/L496–512）|
| R6 | **enumerate_A.json 附录重写风险**：E5 重建 7 题若复跑 test_ab_isolation 穷举会重写该冻结附录，须纳入静默窗 | 已实证（per-case视图交接说明 §6 已知边界同款记载）|
| R7 | **占位机器判据误伤**：若 B 侧登记键用 z*/c* ID 形态，判据把真成员当占位放行（隔离闸被架空）；用结构键则无此患 | 已实证判据逻辑（gateway L1054–1057）；建议采结构键口径 |
| R8 | **spare 批定位**：3 道 spare 不在冻结 30 道正式题内但已入 b_partition 占位与视图冻结，手术范围是否含 spare 须明示（本报告按含处理：37 G_B 成员已含 spare_02 的 z004015/z018577/z063321）| 口径已对齐（§1.1）|
| R9 | **未知项：B 侧注册键的最终契约**（结构键 vs ID 复用 vs 双列并存）未在现有 ADR/契约中定死，E3-1 契约升版时才定型——本报告 §6.3 给出两案对比供裁定 | 调研边界已标注 |

---

## 10. 附录：本报告实证方法留痕

- templates 全量：`dag_poc/templates/discovery_haystack/*.json` 33 份逐份解析 g_b/isomer_handling；
- A 侧四库：全量行扫 92,193+240,664+92,193+309 行，104 候选逐一判在；
- 视图：34 份全量深扫（递归全字符串字段 × 104 候选归属比对）；
- tests：`b_partition|b_set|g_b|placeholder` 全量 grep 两测试域；
- gateway/evaluate：逐行读 L440–565 / L1000–1130 / L1590–1680（gateway）与 L500–1000（evaluate）；
- 冻结：CHECKSUMS.json 37 条逐一解析 + git status 实测未冻结面；
- 中间数据留档：/tmp/gt_cases.json（33 case 明细）、/tmp/gt_aside.json（逐 case 候选性质行）。
