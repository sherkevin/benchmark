# E3 工具线 · progress.md（认领人 AI-E3-Lee · 2026-08-06 起）

## 0. 子任务文档：Day-1 schema 桩交付

### 0.1 与 goal.md 的对齐说明

本交付服务 **goal.md §6 步 3**（tools 与 MCP server 调研文档 → 敲定 tools 数量、每个 tool 的功能与出入参；含 **A/B 源划分方案与 open 类专用工具 `check_molecule_exists` 的出入参定义**），是 task/E3-tools敲定与MCP统一 的 **Day-1 桩**：先把"每个 tool 的契约"与"全局答卷契约"以机器可读 JSON Schema 冻结，供 E1（后端库按 tool 需求建子库）、E4（harness 按契约接入）、E5（template/case 引用答卷 schema）并行消费。

直接对齐的上位条款：

| 上位条款 | 本桩的落点 |
|---|---|
| goal §2b⑤ + ADR-0012 §1：存在性接口语义写死 | `check_molecule_exists` 在 tools_schema 中按"严格等价 m∈B、只返单 bool、无性质/相似度/距离、max_attempts≤10"落契约 |
| goal §6 步 3 下限口径 | tools_schema 头部元数据写明 fixed / open 两口径，及"不以 23 为基线"的理由 |
| INTERFACE_CONTRACT §3.1 4+1 最小收敛 | tools_schema v1 骨架 = search_candidates / get_entity / get_property / compare（可省）/ submit_answer |
| INTERFACE_CONTRACT §3.2 答卷契约蓝本 | answer_schema.json：3 顶层字段 + open 扩展三字段 + symbol_extraction 六符号 |
| ADR-0011 现存契约（screened_pool / ranked_candidates） | answer_schema 内 `legacy_contract_mapping` 显式声明派生关系，evaluate.py 不产生第二套口径 |

### 0.2 子目标与验收标准（本桩范围）

| # | 子目标 | 验收标准 |
|---|---|---|
| 1 | `dag_poc/tools_schema.json` | 全部 tool 名称 + 入参/出参 JSON Schema 字段齐全（返回样例可为空）；每 tool 含 name / 一句话功能 / input / output / error 语义 / source_tier / budget；check_molecule_exists 带 max_attempts≤10；alias 映射覆盖既有 23 工具名；录像行与 `{tools.<name>.out.<key>}` 引用可支撑 |
| 2 | `dag_poc/answer_schema.json` | strict JSON Schema；candidates/ranking/answer + open 三扩展字段；内嵌 symbol_extraction 六符号；与 ADR-0011 契约的映射显式声明 |
| 3 | 自我审查（见 §4） | 四项自查逐条记录结论 |

### 0.3 不做的事（本桩边界）

- ❌ 不写 `gateway.py`（属后续任务，写作区纪律也禁止本轮写）；
- ❌ 不改 `evaluate.py`、不改任何 template/case、不改 ADR；
- ❌ 不构造后端库数据、不定宇宙规模与预算具体数值（E1 步 4 + 后续旋钮任务）；
- ❌ 不改既有 case 判分口径；不实际修复 `search_molecule_database` 的 fallback 代码（只在本桩定契约、登记修复项）；
- ❌ 文献 tools 数量调研表（task.md 子目标 1）不在 Day-1 桩范围，后续单独交付。

---

## 1. 已完成

| 产物 | 路径 |
|---|---|
| 子任务文档（本文件） | `task/E3-tools敲定与MCP统一/progress.md` |
| task.md 状态更新（🚧 进行中 / 认领人 AI-E3-Lee / 2026-08-06） | `task/E3-tools敲定与MCP统一/task.md` |
| tools 契约桩（9 tool + alias 表 + 统一错误码表） | `dag_poc/tools_schema.json` |
| 全局答卷契约桩（含 symbol_extraction 与 ADR-0011 映射） | `dag_poc/answer_schema.json` |
| 自我审查记录 | 本文件 §4 |
| **tools 与 MCP 调研文档（任务 #3 交付物 1：文献 tools 数量对照表 14 条含出处 + 我方敲定 9 个与理由）** | `task/E3-tools敲定与MCP统一/tools与MCP调研文档.md` |
| **tools 契约规范（任务 #3 交付物 2：逐 tool 契约 ×9 + 统一 MCP 调用约定 + A/B 源分层设计 + ADR-0011 映射）** | `task/E3-tools敲定与MCP统一/tools契约规范.md` |
| **E3 收口报告（任务 #7：结论摘要 / 验收逐条对照 / 交付物总表 / 关键设计决策留痕 / 遗留与交接清单 / 风险声明）** | `task/E3-tools敲定与MCP统一/report.md` |
| task.md 状态更新（✅ 完成，保留认领人 AI-E3-Lee / 2026-08-06） | `task/E3-tools敲定与MCP统一/task.md` |

## 2. 进行中

- 无。E3 工具线已收口（report.md 已交付，遗留事项见 report.md §5 交接清单；其中 ADR-0014 双解析前置义务已另立任务 #11 跟踪）。

## 3. ⛔️ 阻塞

- 无阻塞项。以下两项为**报备/登记**，不阻塞本桩交付：

### 报备 1（文档与文件系统不一致）
`dag_poc/docs/INTERFACE_CONTRACT.md §9` 落地清单第 2 项标 ✅，声称"答卷 schema 提到全局一层（`dag_poc/answer_schema.json`：PR-15）"，但**该文件在本轮交付前实际不存在**（goal.md §7 也将其列为"接口层三项未落地"之一，两处口径互证）。本桩已补交 `dag_poc/answer_schema.json`，使 §9 第 2 项的 ✅ 与文件系统状态一致；建议监工同步复核 §9 其余 ✅ 项（如第 3 项 checkpoint logic 可执行式）是否同样名实相符。

### 登记修复项（契约先行，代码修复已于任务 #8 落地，见 §6）
`mcp_mock_server/mock_server.py` 的 `search_molecule_database`：检索词字面匹配不上时 **fallback 返回全库**。
- **缺陷 1（历史）**：截断至前 5 条曾致 3 道题结构性无解（ADR-0011 决策 3，已修）；
- **缺陷 2（现行，本轮登记）**：ADR-0011 的修法把 fallback 扩为返回全库 20 条——这在 fixed 类是"保底可解"，但对 **open 类 A/B 分层是捷径**：agent 可凭一次空查询拿到全库清单，绕过约束检索。
- **桩内契约（本轮已定）**：tools_schema 中 `search_candidates` 行为语义写死"查不到返错误码 `EMPTY_RESULT`，**不得 fallback 全库**"。
- **【任务 #8 C3 修复后现状，与代码对齐】**：网关通道（`dag_poc/gateway.py`，新契约）恒 EMPTY_RESULT；legacy 通道（`mock_server.py`）由环境变量开关 `MCP_STRICT_EMPTY_RESULT` 控制：**默认保持 ADR-0011 决策 3 返全库**（37 个既有 case 的 query 全是自然语言、字面匹配恒不命中，候选获取链路全靠此 fallback，直接改断会打穿既有 case），=1 时启用 EMPTY_RESULT。
- **注**：ADR-0011 决策 3 明文"fallback 返回全库"，与本桩"禁 fallback"契约方向相反；按权威顺序 goal.md §2b②（A 的任何查询不得带出整库内容）优先，**待监工对 ADR-0011 该条追加修订裁定后再定 legacy 通道最终默认值**（当前默认=返全库，见 §6 登记不修项）。

---

## 4. 自我审查记录（第 3 步硬约束）

### 4.1 check_molecule_exists 语义是否严格等价 m∈B、只返 bool、无性质泄漏？
**结论：是。** 桩中写死三条：① 接口语义条目逐字声明"返回 `exists` 严格等价于 m ∈ B：A 内分子与库外分子一律返 false"；② output schema 仅 `exists`(boolean) + `attempt`(int) + `attempts_remaining`(int) 三个字段，无任何性质/相似度/距离字段，且 `additionalProperties=false` 结构性封死泄漏通道；③ 停止信号（返 true 可结束）与评分依据（∈ G_B + B 侧性质复核）分离的声明写入 behavior_notes；max_attempts 上限 10、超限 `ATTEMPTS_EXCEEDED` 记 0 分、attempts 计入指标均已落契约。

### 4.2 仅凭 A 侧工具，结构上是否无法获得 B 侧任何信息？
**结论：是。** 逐工具核查：`literature_search` / `search_candidates` / `get_entity` / `get_property` 均标 `source_tier=A`，output 无任何 B 源字段；`compare` 是纯排序便利工具（输入只能来自已查得的 A 侧数据）；`submit_answer` 只收卷不吐数据；`check_molecule_exists` 是 agent 唯一可触达 B  membership 的通道，且只吐单 bool（source_tier=existence-only）；`edit_molecule` / `normalize_structure` 是纯结构计算工具，不查任何库。录像层由 gateway（后续任务）实现时须按同一 tier 表做物理隔离。

### 4.3 答卷 schema 是否覆盖 open grounding 三条件所需全部字段？
**结论：是。** goal §2b grounding 三条件 ↔ 字段对照：① cited_values 与 trace 真实返回容差一致 → `cited_values[{mol,prop,value}]`；② anchor ∈ A → `anchor_molecules[]`（引擎按 A 侧实体表校成员）；③ edit_op(anchor) 规范化后 == 提交分子 → `edit_op{anchor,operator,detail}` + `answer`（提交分子 ID）。三条件抽取式已写入 answer_schema 的 `open_grounding_checks`。fixed 类三顶层字段同时完整（含 NO_SOLUTION 由 cp6 logic 判、schema 不定值域）。

### 4.4 与 goal.md 冲突处理
未发现 goal.md 与本轮交付的实质冲突。唯一张力点（ADR-0011"fallback 返回全库" vs goal §2b②"A 的任何查询不得带出 B/整库内容"）已在 §3 登记并报监工裁定，本轮**不自行偏离 goal**，桩契约按 goal 口径写死。

### 4.5 两套口径检查（ADR-0011 vs INTERFACE_CONTRACT §3.2）
answer_schema 的 `legacy_contract_mapping` 给出可机械执行的派生：`screened_pool = [c.id for c in candidates]`、`ranked_candidates = [c.id for c in candidates if c.reject is null]（按 ranking 序）`、`measurements` 供 cp3 对账（evaluate.py 现从 trace 的 predict_admet 返回取查证值，迁移后改为按本映射取答卷值再对账）。单向派生、无第二套独立契约，evaluate.py 不需要同时维护两种解析分支。

---

## 5. 任务 #3 文档交付（Taylor · 2026-08-06）

### 5.1 交付物登记（均落在本目录写作区内）

| 产物 | 路径 | 覆盖验收项（task.md §3） |
|---|---|---|
| tools 与 MCP 调研文档 | `task/E3-tools敲定与MCP统一/tools与MCP调研文档.md` | 子目标 1（文献 tools 数量对照表，14 条，每条注明论文+venue+工具数+清单出处）；子目标 2（我方敲定 9 个 + 对标文献区间 + fixed/open 双下限口径 + 显式说明不沿用 23） |
| tools 契约规范 | `task/E3-tools敲定与MCP统一/tools契约规范.md` | 子目标 3（逐 tool 契约 ×9：名称/功能/入参/出参/错误语义）；子目标 4（统一 MCP 调用约定：命名/参数/返回/11 错误码/录像行/reward 引用）；子目标 5（A/B 源分层设计 + 存在性接口三概念分离 + max_attempts≤10 + 隔离测试方案，脚本实现归 Jimmy 任务 #4）；子目标 6（工具调用记录与 checkpoint 的衔接：§2.5 录像派生判分符号） |

### 5.2 关键结论摘要

1. **文献区间**：14 条已发表 benchmark 对照——领域任务/科学发现型评测给 agent 的工具数为 0–18（化学域 agent 主流 10–18、τ-bench 14–16、oracle 制 0–1）；77（InfoMosaic-Bench）与 604–16,464（Toolathlon/ToolBench）属"考工具选择"的不同构评测，不作对标。
2. **我方敲定 9 个 v1 契约工具**（A×4 + existence-only×1 + none×4，B 直接暴露×0），与 `dag_poc/tools_schema.json` v0.1.0-stub 逐一对齐；9 = fixed 六 checkpoint 下限并 open 三类下限的最小并集。
3. **与桩核对无实质不一致**：契约规范 §5 记录 3 处口径收敛留痕（TOOLS_INVENTORY 旧字段收敛、search_candidates strict 形态收紧、compare 可省均兼容），均以 goal.md 与桩为准；ADR-0011 决策 3 fallback 事项不重复裁定，沿用 §3 已登记项。

### 5.3 边界自查

- 未写 TODO.md / problems.md / _并行路总览.md / goal.md / governance/adr/ / dag_poc/ / src/ / 其他 task/E*/ 目录任何文件；
- Jimmy 任务 #4（gateway.py / no_path_violation）只在契约规范 §3.5 与附录交接清单中作为需求输入引用，未代写其进展。

---

## 6. 任务 #8 三维评审修复记录（Jimmy · 2026-08-06）

三维评审（完整性 Mark / 正确性 Ryan / 影响面 Daniel）对任务 #4 实现的发现，本轮统一修复。写作区：gateway.py、mcp_mock_server/**、evaluate.py（只加不改既有 reward）、tools_schema/answer_schema（最小同步）、progress.md（本轮归 Jimmy 维护）。

### 6.1 已完成修复项对照表

| # | 来源 | 修法（一句话） | 文件位置 |
|---|---|---|---|
| C1 | Ryan | 存在性预算改服务端按真实调用计数（`existence_used += 1`），超限返 False+exhausted+ATTEMPTS_EXCEEDED；入参 attempt 降级为可选元数据 | `dag_poc/gateway.py::_tool_check_molecule_exists`；`tools_schema.json` check_molecule_exists |
| C2 | Ryan | search_candidates 约束 property 与 `_entity_props` 共用同一份可查性质投影白名单（新增 fail_reason 进 meta 黑名单），白名单外一律 UNKNOWN_PROPERTY | `dag_poc/gateway.py`（ADMET_META_KEYS/CRYSTAL_META_KEYS/_queryable_props/_a_universe） |
| C3 | Daniel | mock_server 加环境变量开关 `MCP_STRICT_EMPTY_RESULT=1` 才启用 EMPTY_RESULT，默认保持 ADR-0011 返全库；网关通道恒 EMPTY_RESULT；同步修正 schema 过期表述 | `dag_poc/mcp_mock_server/mock_server.py::_search_molecule_database_impl`；`tools_schema.json` search_candidates behavior_notes |
| C4 | Ryan | 补两条负例断言：①固定 attempt=1 连续调用超 max_attempts 必触发 ATTEMPTS_EXCEEDED；②约束 `pass` 必返 UNKNOWN_PROPERTY | `dag_poc/mcp_mock_server/tests/test_gateway_isolation.py`（test_exists_server_side_counting / test_search_constraint_whitelist_blocks_meta） |
| M1 | Ryan | cp4 fallback_prio 0→1（严格大于 cp3 prio），无 compare 合法轨迹 strict 下通过；补测试 | `gateway.py::DEFAULT_EVIDENCE_MAP`；测试 test_path_legal_without_compare_passes |
| M2 | Ryan | Gateway.__init__ 加载 b_partition 后断言 B∩A=∅，相交即 raise；补测试（临时分区文件） | `gateway.py::__init__`；测试 test_b_partition_disjoint_assertion |
| M3 | Mark | search_candidates 增可选 offset 入参 + total_found/offset 出参；隔离测试穷举口径改对 A 全集断言（含 bbb=null 的 cb） | `gateway.py::_tool_search_candidates`；`tools_schema.json`；测试 test_a_source_exhaustive_isolation |
| M4 | Daniel | submit_answer 双向桥接：旧 candidate_result.{screened_pool,ranked_candidates} 机械派生为新 schema 受理；新答卷经 `answer_view_legacy()` 反向派生旧字段（选 evaluate.py 零改动方向）；补测试 | `gateway.py`（_bridge_legacy_payload/answer_view_legacy）；测试 test_submit_answer_legacy_bridge |
| M5 | Daniel | 补 compat 翻转两用例：legacy "compare 后复核性质"→判逆序 gate=False；legacy 缺 compare→降级 gate_ok=True | 测试 test_evaluate_compat_flip_scenarios |
| M6 | Mark | gateway 补 `docs()`：从 literature_search 成功录像行收集 papers[].doc_id；补测试 | `gateway.py::docs`；测试 test_docs_derivation |
| Minor 1 | Ryan | `_is_gateway_recording` 改全表检查 + args/ret 结构特征；补测试 | `src/benchmark/evaluate.py`；测试 test_is_gateway_recording_full_table |
| Minor 2 | Ryan | max_attempts = max(1, min(int(x),10))，非法类型构造期报 ValueError；补测试 | `gateway.py::__init__`；测试 test_exists_over_budget |
| Minor 3 | Ryan | get_property 计费顺序统一为"先计费后校验"（UNKNOWN_ENTITY/UNKNOWN_PROPERTY 也耗预算，防免费枚举实体存在性），写进 behavior_notes；补测试 | `gateway.py::_tool_get_property`；`tools_schema.json`；测试 test_property_budget_billed_before_validation |
| Minor 4 | Ryan | 删 gateway.py 两支相同的死代码表达式（`v['value'] if isinstance(...) else v['value']`） | `gateway.py::_tool_get_property` |
| Minor 5 | Daniel | evaluate.py alias_map 加载包 try/except，tools_schema.json 缺失时降级 unavailable 而非崩溃 | `src/benchmark/evaluate.py::evaluate_no_path_violation` |
| Minor 6 | Daniel | load_mock_db 直接用 __file__ 锚定快照路径，去掉 CWD 相对分支 | `src/benchmark/evaluate.py::load_mock_db` |
| Minor 7 | Mark | 测试补 A2（穷举过程全部成功 ret 逐字段扫 B 成员标识）与 A5（10 次随机猜测枚举 G_B 零命中） | 测试 test_a_source_exhaustive_isolation 断言 3 / test_a5_random_guess_gb_zero_hit |
| Minor 8 | Ryan | tools_schema search_candidates required 改 []（"至少给一"语义与网关校验口径对齐）；SOURCE_DENIED 在 unified_error_codes 标注"保留未启用" | `tools_schema.json` |
| Minor 9 | Ryan | grounded 布尔启发式收紧：观测值为 0/1 整型即按布尔严格相等（tol=0），堵住 1.09 过 tol=0.1 的缝；补测试 | `gateway.py::grounded`；测试 test_verify_budget_and_grounded |

### 6.2 schema 变更点（冻结纪律下最小同步，全部登记）

**tools_schema.json**（answer_schema.json 本轮未改，M4 桥接直接消费既有 legacy_contract_mapping）：
1. `check_molecule_exists.input.required`：`[molecule_id, attempt]` → `[molecule_id]`；attempt 描述改"可选元数据，仅回显不参与预算"；behavior_notes 增第 ⑤ 条（服务端计数写死）。
2. `search_candidates.input`：新增可选 `offset`（integer≥0，default 0）；`required` 由 `[constraints, keywords]` 改为 `[]` 并加 required_note（与网关"至少给一"校验口径对齐）。
3. `search_candidates.output`：新增 `total_found`/`offset` 并列入 required；errors 增 UNKNOWN_PROPERTY。
4. `search_candidates.behavior_notes`：删除"代码修复属后续任务"过期表述，改写为修复后现状（网关通道已实现 EMPTY_RESULT / legacy 通道开关 / 白名单 / 分页）。
5. `get_property.behavior_notes`：追加计费顺序写死条款（Minor 3）。
6. `meta.unified_error_codes.SOURCE_DENIED`：标注【保留未启用】（v1 网关无产生路径，留待 E1 物理分层后启用）。

### 6.3 测试与回归结论

- `python3 -m pytest dag_poc/mcp_mock_server/tests/test_gateway_isolation.py -q`：**23 项全绿**（含全部新增对抗用例：C4①②、M1 无 compare 合法轨迹、M2 分区断言、M3 分页穷举全集、M4 双向桥接、M5 compat 翻转×2、M6 docs、A2/A5、mock_server 开关双行为、Minor 1/2/3/9）。
- 回归：修改前后各跑一遍 evaluate.py 全量 15 实验点，`_all_results.json` 逐字段比对（case/model/gt/best/hit/submitted/gold_pass_set/tools/cp1–cp6/process/gate/task_score + no_path_violation/path_validation_mode/path_violations）：**零差异**；stdout 完全一致；stderr 仅 2 条降级警告的诊断文本变化——M1 修平局后 cp3/cp4 从"证据不可区分"变"可区分且合法"，降级边列表少列 [cp3,cp4]，降级行为与得分均不变（可解释）。
- mock_server 开关验证：默认（未置开关）= ADR-0011 返全库 40 条；`MCP_STRICT_EMPTY_RESULT=1` = EMPTY_RESULT；有命中时两种模式行为一致（测试 test_mock_server_empty_result_switch）。

### 6.4 登记不修项（报监工，不越界）

| # | 来源 | 事项 | 处置 |
|---|---|---|---|
| 1 | Daniel W1 | 4 个 template.json 的 scoring.gate 文本未同步 no_path_violation | 属 E5 写作区（dag_poc/templates/**），登记交接 E5 |
| 2 | Daniel W3 | _all_results.json 的 27 处历史补账数字与 gate 改动同 commit | 登记：**基线账本自 f9a07a6 起按现行公式重算、历史数字作废**，留待 E3 report.md 收口引用 |
| 3 | Daniel S4 | b_partition.json 为占位数据 | open 端到端判分等 E1 后端库物理分层交接（既有声明，重申） |
| 4 | Daniel S2 | DEPRECATED 归一语义备查 | 建议在 tools_schema alias_map 注释补一句；本轮未改（alias_map._comment 已有"标 DEPRECATED 的旧名无新契约对应物、历史 trace 仍可解析但不参与新 case 判分"表述，如需再精化留给后续） |
| 附 | C3 遗留 | legacy 通道 EMPTY_RESULT 最终默认值 | 待监工对 ADR-0011 决策 3 追加修订裁定后再定（当前默认=返全库，开关 MCP_STRICT_EMPTY_RESULT） |

## 7. 任务 #11 落实 ADR-0014 双解析（Jimmy · 2026-08-06）

**背景**：E3 answer_schema.json 与 E2 规范 M7 同日冻结两套答卷字段名；监工裁定以 answer_schema.json 为唯一来源（ADR-0014），但 37 case + 63 trace 全用旧字段，判分侧必须先双解析、不得直接收敛。

### 7.1 双解析实现（evaluate.py，只加不改既有 reward/权重/阈值）

- 新增 `parse_answer_symbols(response_json)`：优先读新契约 candidates/ranking/answer（pool=candidates IDs；ranked=ranking 中 reject==null 序列，即 legacy_contract_mapping 口径；best=answer），读不到回落旧契约 candidate_result.{screened_pool, ranked_candidates}（现状行为逐字段保持，best_hint=None 维持 ranked[0] 口径）。
- `score_case` 改经该函数取符号，best_hint 与 pool/ranked 同走 normalize_id；`_score_fixed`/`_score_open` 增 `best_hint=None` 可选参（旧路径恒 None，行为不变）。
- 与网关侧 `gateway.py::_bridge_legacy_payload` / `answer_view_legacy`（任务 #8 M4 双向桥接）同一份 symbol 口径，注释互相指认。

### 7.2 schema 措辞与口径（裁定边界条款）

- answer_schema.json `legacy_contract_mapping._comment`：原"收敛后引擎只读本 schema…不再维护双解析分支"改为"双解析分支保留至 E5 完成旧 case 迁移（见 ADR-0014），迁移完毕后方可移除"。
- `meta.scope` 写明：candidates/ranking/answer 三主字段 fixed 与 open 两类均必填非 null（open 类不得漏交，E2 题面 D 段以此为准）；open 三扩展字段必填、fixed 填 null。
- **时点依赖登记**：fallback 移除依赖 **E5 完成全部旧 case 迁移**；迁移完毕前不得执行收敛、不得移除双解析分支（ADR-0014 §决策 3）。

### 7.3 顺带确认三件

1. open 必填 candidates/ranking：required 本已含全部六字段，口径已在 meta.scope 写明（如上）。
2. check_molecule_exists 的 max_attempts≤10：tools_schema.json 文本齐备（budget="max_attempts 上限 10，超限记 0 分"、attempt maximum=10、出参 attempts_remaining、behavior_notes ④⑤），无需改。
3. no_path_violation：已实现（gateway.py 单一实现 + evaluate.py 接入）并通过两轮复核（任务 #5 Mark 三维评审 + 任务 #9 Chris 复核），strict/compat 双模式、23 项测试覆盖——E6 开跑前置条件已就位。

### 7.4 测试与回归

- 新增 `tests/test_answer_dual_parse.py` 8 项：新/旧路径符号派生、新旧并存优先新契约、NO_SOLUTION 与边界、桥接往返闭环、真实 case/trace 上新旧形状判分逐字段一致、旧路径锚点、answer 字段生效——全绿。
- 全量：31 passed（新增 8 + 既有 23，零失败）。
- 回归：15 实验点 `_all_results.json` 与基线（/tmp/e3_task8_baseline/）逐字段比对**零差异**（stdout 仅 stderr 降级警告重定向差异，判分输出一致）。

---

## 8. task-02 返工章节（判据修复与 MCP 接入 · Taylor 收口 · 2026-08-06）

**背景**：监工验收意见判旧版 report 不通过（致命 5 + 重要 7），task-02 任务书下达返工。本章为返工全过程流水与治理登记；report.md 全量回写见其 §7（任务 #20）。返工末测试基线：**97 passed + 1 skipped**（skip 为 mcp e2e，mcp 包未装）。

### 8.1 返工任务流水

| 任务 | 内容 | commit | 结果 |
|---|---|---|---|
| #13 修复三 | per-case B 分区 + BPartitionReader 接缝 + 可证伪隔离测试重写（同命名空间、真穷举求解器、投毒负对照、a5 邻域采样） | 5f5e72f | 隔离测试改可证伪 |
| #14 修复一 | evidence_time 判据 max→min（agg 可配）+ 回访必过/交错正反例 + max/min 对照重放 | e87bacd | 68 trace 重放 27 翻转（24 救回 + 3 新增真逆序），明细落 max_min_diff_table.md |
| #15 修复二 | gateway_mcp.py 9 工具 MCP 运行态（端口 9001、数据驱动注册、启动断言防漂移、双 server 分治） | 5bdab8a | 9001 运行态达成（e2e 因 mcp 包未装 skip，已登记） |
| #16 修复五 | grounding 条件③真实化（RDKit canonical SMILES）+ get_entity 派生真实数据 | 0a483c4 | 条件③去入参拼接；边界算子占位期恒 False 已标注 |
| #17 修复四 | evaluate.py open 判分三门闭环（hit_gb + evaluate_grounding 三判据 + _score_open_discovery） | 0a483c4..21b5e11 | open case 自 __main__ 不再 KeyError；d289340 卷入主体，以区间 diff 为准 |
| #18 回归验证（第一轮） | 全量测试 + 15 实验点零差异 + fixed 判分零改动审读 | — | **8/8 PASS** |
| #19 修复六 | tools_schema oneOf/examples（v0.2.0）+ 契约与调研文档回写 | a8a2247 | 12 错误码信封 + 23 条实测 examples 落盘 |
| #21 评审合并修复 | 三维评审发现合并修复（见 8.1.1 摘要）+ cp5 fallback_prio 1→2 | a8a2247..54c2bb8 | test_review_merge_fixes.py 18 测全绿；15 点零差异实证无回归 |
| #22 ADR-0015 读取器 | BPartitionBackendJsonlReader（目录扫描/A 侧白名单/两列索引/per-case has_case_data）+ 句柄级隔离自检 + 交错正例 + family 注释 | 54c2bb8..303cf63 | 主体随 5c01dfd 入库、尾段 303cf63，区间核验无缺失 |
| #23 增量文档 | PROPERTY_NOT_VERIFIED + answer_schema 升版 0.2.0 + 调研文档四处订正 + τ-bench 枚举落盘 | 5c01dfd（owner 混合提交卷入） | 五产物内容核验完好入库 |
| #24 二次回归验证 | 停止准则 1–3 实证 + 交付物快照 | — | **8/8 PASS**（97 passed + 1 skipped） |
| #25 轻 3 落码 | compare 错误码两分支（PROPERTY_NOT_VERIFIED vs UNKNOWN_ENTITY）+ example 实跑二次校准 | f0275f0 | 15 点零差异复验 |
| #20 修复七 | report.md 全量回写 + progress.md 本章节 | 本次提交 | 即本章与 report.md §7 |

#### 8.1.1 任务 #21 三维评审合并修复摘要（C1/M1–M5/m1–m3）

| 编号 | 内容 |
|---|---|
| C1 | canonical_chemical 长度上限：超长 SMILES 不再进 RDKit 原生层（原触发 SIGSEGV exit 139），四个触发面经唯一汇聚点一处覆盖 |
| M1 | B 成员判定共用口径 normalize_b_key：Kekulé 写法 vs canonical 写法在查询侧/装载侧/hit_gb 判分侧三处一致 |
| M2 | 异常不穿透：Gateway.call 内部异常转错误信封且先落录像行（"逐调用录像"不变式）；compare 对 null 性质值返信封不崩；gateway_mcp 薄包装同样兜底 |
| M3 | prio 严格递增阶梯：同刻 get_property+compare 的 cp3<cp4 平局被破除 |
| M4 | edit_molecule 分支 2 的 _require_rdkit 守卫在场（缺包路径进信封） |
| M5 | max_min_diff_table.md §4 诊断字段计数订正：8 点/11 点 → **9 点 False→None / 13 点边清单变化（9 清空、4 换边）**，与 §2.1 明细一致 |
| m1 | B∩A 断言覆盖 g_b：g_b ⊆ b_set 与 g_b ∩ A全集 == ∅ 装载期强制 |
| m2 | gateway.py evidence_time docstring 纠偏：旧文案"最晚时刻"改"首次建立时刻（min 聚合）" |
| m3 | edit_molecule 分支 1（ed: 占位串锚点）注释纠偏：标明返工前该路径行为与占位语义 |

### 8.2 治理登记（报监工，不静默改）

**修订请求（拟文照录，两文件在 E3 写作区外，本轮不碰）**：

> evidence_time 判据方向经监工验收意见裁定由 max 改 min（修复一，commit e87bacd）。以下三处原文与现行实现冲突，请求同步修订为"首次建立时刻（min 聚合）"口径：① INTERFACE_CONTRACT §3.3 L266 "满足该 cp 所需录像事件的最晚时刻"→"首次建立时刻（agg 可配，缺省 min）"；② INTERFACE_CONTRACT §4 L351 "cp3 = 最后一次相关 get_property 的时刻；cp2 = 最后一次 search_candidates"→"第一次相关……"；③ ADR-0008 §6 L56 "由 evidence_time(cp) 定义（录像最晚事件时刻，逐边比较）"→"（录像首次建立时刻，逐边比较）"。依据：拓扑序问"依赖是否在被使用前先建立"，68 trace 重放实证 max 口径误杀 24 条回访轨迹（max_min_diff_table.md）。

**其余登记项**：

| # | 事项 | 状态与请求 |
|---|---|---|
| 1 | ADR-0011 决策 3（fallback 全库）开关最终默认值 | 待监工裁定；当前 legacy 通道默认返全库（开关 `MCP_STRICT_EMPTY_RESULT`），网关通道恒 EMPTY_RESULT |
| 2 | mcp 包环境批准 | 未装致 e2e skip（1 skipped）；建议 python3.12 + pip install mcp，批准后补跑 e2e |
| 3 | query_spec.json 引"0.1.0-stub"与 answer_schema v0.2.0 不一致 | 属 E2 冻结区，E3 不碰；请监工协调 E2 同步 |
| 4 | cp5 fallback_prio 由 1 调为 2 | 评审合并修复落地：min 口径下原机制声明失实，prio 阶梯改 cp3=0 / cp4=1 / cp5=2 / cp6=3（严格递增）；15 实验点零差异实证无回归（test_prio_ladder_strictly_increasing 在场） |

### 8.3 与 E1/E4/E5 交接更新（取代 report §5 旧口径）

| 对象 | 交接内容 |
|---|---|
| **E1** | `BPartitionBackendJsonlReader` 已按 ADR-0015 就绪（目录扫描 / A 侧白名单 / InChIKey+SMILES 两列索引 / per-case has_case_data），B 侧落库即自动生效，占位 fixture 自动退场；旧"E1 只需替换本文件、接口不变"已撤回，三处差异（文件布局/键空间/G_B case_ref）均已对齐（report.md §7.2②） |
| **E4** | harness MCP 通道指向 `gateway_mcp.py` **9001**（`GATEWAY_MCP_PORT` 可配、`DAGPOC_CASE_ID` 按 case 跑）；环境前置：Python≥3.10 / rdkit≥2025.09 / mcp；legacy mock_server 9000 冻结不接新 case；错误统一信封 `{error_code, error}` |
| **E5** | 真实 G_B 冻结入库（backend B 侧 jsonl）后占位 fixture 自动退场（backend B 侧 4 个 `*_B.jsonl` 现全 0 行，机制已通、数据待 E5）；open template/case 尚为 0，待造 |

### 8.4 边界自查（本章节与任务 #20）

- 本轮（任务 #20）只写 report.md 与 progress.md 两文件，未改代码与 schema，未碰 goal.md / adr / templates / INTERFACE_CONTRACT / query_spec.json；
- 两文件数字口径已与 max_min_diff_table.md、实测基线（97 passed + 1 skipped）交叉核对：27 翻转（24+3）、9/13 诊断字段、19 字段零差异、68 trace 均一致。

---

## 9. 二轮返工记录（监工二次验收后 · Taylor · 2026-08-06）

### 9.1 版本记录补记（监工点名："以后升版本请记一行"）

- **补记（此前漏记）**：`dag_poc/answer_schema.json` 升至 **0.2.0**（任务 #23）——0.1.0-stub → 0.2.0 仅升版本标识与状态（meta.version/supersedes/status"现行正式版"/upgrade_note），**字段内容零改动**（向后兼容硬约束）；随 owner 混合提交 5c01dfd 入库（卷入登记同 §8.1 #23 行）。同日 `tools_schema.json` 升 v0.2.0（oneOf + examples，commit a8a2247）。
- **约定确立**：此后任一 schema 升版，须在本文件记一行（版本 / 内容 / 所在 commit），不得再靠下游实测发现。
- 遗留登记重申：query_spec.json 仍引"0.1.0-stub"，属 E2 冻结区，监工已授权 E2 同步（见 §8.2 登记 3）。

### 9.2 二轮返工 commit 链（git 时序，每行均经 git log/实查核实）

| 序 | commit | 任务 | 一行简述 |
|---|---|---|---|
| 1 | 914b48d | #27 | task-02 任务书状态与实际进展对齐（依监工 §8.1，头部状态行 + 致命 1–5/重要/轻量逐项标记） |
| 2 | 4cd337b | #31 | dummy atom 真实规范化：canonical_structure 走 RDKit（'[2*][1*]'→'[1*][2*]'）、静默 fallback 移除、取用 E1-03 映射表（**混合提交**，message 为 E2-03 验收、卷入本项，以文件实查为准） |
| 3 | 312dda6 | #30 | mock_server.py 单面合并：@mcp.tool 31 = 9 契约真注册 + 22 deprecated 层；**双 server 分治口径作废**，E4 交接改指 mock_server.py 单面（替代 §8.3 的 9001 口径） |
| 4 | 071f7da | #32 | 隔离 rehearsal 证据链（BEFORE/AFTER/selftest 日志 + rejections 负例拒收，照 E1 口径；隔离证据源改指 rehearsal_logs）+ 移除 edit_molecule ed: 占位直通（改返 ANCHOR_NOT_IN_A） |
| 5 | 1b10fe6 | #33 | 重要 7 落地：test_gateway_isolation.py 物理拆为 test_ab_isolation(6)/test_path_validation(7)/test_budget(18) 三文件独立报数，旧文件删 |
| — | HEAD=1b10fe6 | #34 | 终验全绿：**128 passed + 1 skipped**（dag_poc 126+1，+2 为 src/tests 双解析用例；src/tests/test_core.py 另有 4 个既有 fixture 错误非 E3 范围不计入）；15 实验点 19 判分/门控字段零差异；fixed 判分与 _score_open 逐行零改动 |

对账与实施明细见 report.md §8（重要 7 + 轻量 4 逐条对账表 + 二轮优先四项实施记录）。

### 9.3 外部阻塞登记（诚实条款延续，均非 E3 可自解）

| # | 阻塞 | 状态与解除方 |
|---|---|---|
| 1 | 真实 B 数据 | backend B 侧 4 个 `*_B.jsonl` 实测全 0 行；等 E5 冻结入库，届时 ADR-0015 读取器自动接管 |
| 2 | 真 open case | 文件数为 0，等 E5 出题；open 判分三门已就绪（hit_gb/evaluate_grounding/_score_open_discovery）仅待真实数据 |
| 3 | mcp 包未装 | e2e skip（1 skipped），环境批准事项（建议 python3.12 + pip install mcp），批准后补跑（同 §8.2 登记 2） |

### 9.4 边界自查（任务 #29）

- 本轮只写 report.md 与 progress.md 两文件，未改代码与 schema，未碰 goal.md / governance / adr / templates / INTERFACE_CONTRACT / query_spec.json；
- report.md 末尾「监工验收意见」「监工二次验收」两节原文未动；
- 全部 commit 号与数字（31=9+22、6/7/18、126+1、128+1）均经 git log 与 pytest 实查核实后写入。

---

## 10. task-03 实施与评审修复记录（Taylor · 2026-08-07）

实施与修复明细见 report.md §9（§9.1 匿名层 / §9.2 材料侧身份键 / §9.3 mcp e2e / §9.4 评审与修复 / §9.5 atom_count 口径 / §9.6 遗留）。

### 10.1 task-03 commit 链（git 时序，每行均经 git log 实查）

| 序 | commit | 时刻 | 任务 | 一行简述 |
|---|---|---|---|---|
| 1 | 4acdd60 | 01:21 | #36（§1） | 匿名层端到端：数据源切换三处（get_entity 透传/_entity_smiles/_a_smiles_universe）+ 条件③实跑证据（日志 01:18:53 在先）+ 三处标注改写在后 |
| 2 | d3b7d4a | 01:38 | #37（§2） | 材料侧身份键接口 B 现算接入 grounding == 口径（日志 01:36:03 在先）+ 判别力评估（5,771/5,771、唯一键 5,740、31 碰撞全为上游重复） |
| — | （§3） | — | #38 | mcp e2e 环境项零代码改动（见 10.2 双解释器口径） |
| 3 | df93eca | 02:03 | #41 | 三维评审代码侧 7 项修复（新增 test_review_fixes_41.py 11 用例） |
| 4 | b33e3a0 | 02:07 | #42 | 评审文档侧：契约与示例匿名域同步 + atom_count 口径登记 + smoke_examples.json 重生成 |
| — | HEAD=b33e3a0 | — | #43 | 复核全 PASS：**143 passed + 1 skipped 零回退**；15 实验点零差异；fixed 判分与 _score_open 逐行零改动 |

### 10.2 双解释器口径登记（本轮新增，长期有效）

| 解释器 | 环境 | 用途 | 命令示例 |
|---|---|---|---|
| python3.9（系统） | rdkit 2025.09.2，无 mcp | 全量常规回归（143+1 基线口径） | `python3 -m pytest dag_poc/mcp_mock_server/tests/ -q` |
| python3.10 venv（/tmp/dagpoc_e2e_venv） | **mcp==1.29.0**（钉死：2.0.0 无 fastmcp 子模块）+ rdkit 2026.3.5，阿里云镜像装包（PyPI 官方源不可达） | e2e 真实 MCP 通道（2 passed） | `/tmp/dagpoc_e2e_venv/bin/python -m pytest dag_poc/mcp_mock_server/tests/test_gateway_mcp_e2e.py -q` |

注：venv 在 /tmp 重启会丢，重建同口径即可；迁固定路径待监工裁定（report.md §9.6 #3）。

### 10.3 测试基线演进（实查核实）

**128 + 1 → 132 + 1 → 143 + 1**（dag_poc/mcp_mock_server/tests 全量口径，含 test_answer_dual_parse 8 项；1 skipped 为 e2e）：
- 128+1（#34 终验，HEAD=1b10fe6，同 §9.2 口径：dag_poc 126 + src/tests 双解析 2 例并入）；
- 132+1（HEAD=d3b7d4a）：**§1 净增 2 用例**（test_gateway_rdkit_grounding.py 17→19，新增两实跑用例含一处原用例改写合并）+ **§2 新增 4 用例**（test_material_identity_grounding.py）；
- 143+1（HEAD=b33e3a0）：+11 评审修复用例（test_review_fixes_41.py）；本轮实跑复核：排除 E4 在途 traces 影响的 dual_parse 5 failed 后 135+8=143 全绿（§9.6 #2 登记）。

### 10.4 schema 版本登记（依 §9.1 升版记一行约定）

- 本轮 tools_schema.json（b33e3a0）为**描述与示例同步**（get_entity/check_molecule_exists/edit_molecule/normalize_structure 字段描述、behavior_notes、examples、错误文案；meta.examples_source 取数声明更新），**无 output 结构/字段/错误码变更，不构成升版**（meta.version 维持 0.2.0，核读确认）；meta.placeholder_boundary 未动（任务 #36 已改，#42 未重复动）。

### 10.5 边界自查（任务 #40）

- 本轮只改三处：task-03 头部状态行与 §1/§2/§3 标题标记、report.md 新增 §9、progress.md 新增 §10；监工各节（report.md 末尾三节）原文零改动；
- 全部 commit 号（4acdd60/d3b7d4a/df93eca/b33e3a0）与时序、用例数（11/4/2）、材料数字（5,771/5,740/31）、基线（143+1）均经 git log/pytest/日志实查；
- 一处口径偏差如实注明：任务线索给的条件③证据时序（01:10/01:14）与库内证据不符，实查为日志生成 01:18:53、随 4acdd60 于 01:21 入库，"证据在先、改写在后"时序结论不变，本文按实查值写入。

---

## 11. task-04 实施与评审修复记录（Taylor · 2026-08-07）

实施、裁定与修复明细见 report.md §10（§10.1 根因 / §10.2 实现 / §10.3 停止准则 / §10.4 翻转公布 / §10.5 账本失效 / §10.6 裁定 A / §10.7 后续项 / §10.8 知会 E4）。

### 11.1 task-04 commit 链（git 时序，每行均经 git log 实查）

| 序 | commit | 时刻 | 任务 | 一行简述 |
|---|---|---|---|---|
| 1 | e967a1d | 11:36 | #46（task-04 §2） | dual_parse 测试改自带最小样本 fixture（PR-35 测试脆弱性一半） |
| 2 | b85f4a7 | 11:40 | #45（task-04 §1） | extract_tool_data 新契约迁移 + legacy 双轨分派（_NEW_CONTRACT_TOOL_NAMES 显式名单）；legacy 冻结零改动；新契约 6 点 cp1-cp5 非 0（avg task_score 0.067→0.127）；新增 test_tool_extract_dispatch.py 8 用例 |
| 3 | 4751917 | 13:00 | 监工裁定 A | 两段式断言：legacy 9 点逐字段零差异 + 新契约 6 点翻转方向断言（判分字段子集）；全量实测 152 passed + 1 skipped |
| 4 | 178ef6c | 13:19 | #50 | 三维评审合并修复 8 项（5 Major + 3 Minor）：get_property 拆分调用合并 / compare 形状 {ordered_ids} / fixture 固化最小切片 / 翻转断言收窄 / open 3 点翻转守卫；全量实测 **155 passed + 1 skipped** |
| — | QA #48 | — | Chris | 停止准则三条全过（155+1 零失败 / 新契约 6 点非 0 / legacy 9 点重判零差异） |

注：任务指派链写作 b85f4a7→e967a1d→4751917→178ef6c（任务序，§1 在 §2 前），git 时序为 e967a1d（11:36）先于 b85f4a7（11:40）提交，本表按 git 时序如实呈现。

### 11.2 测试基线演进（实查核实）

**143 + 1 → 152 + 1 → 155 + 1**（dag_poc/mcp_mock_server/tests 全量口径；1 skipped 为 mcp e2e 环境项）：
- 143+1（HEAD=b33e3a0，task-03 收口，见 §10.3）；
- 152+1（4751917 commit message 实测值）：b85f4a7 新增 test_tool_extract_dispatch.py 8 用例 + e967a1d fixture 自带化 + 裁定 A 断言改造后；
- 155+1（178ef6c commit message 实测值）：#50 评审修复新增拆分合并与 open 翻转守卫等用例；本轮 #49 在 git archive HEAD 干净副本重跑复核 **155 passed + 1 skipped 一致**。

### 11.3 账本失效登记（仿 ADR-0011 先例）

`dag_poc/traces/_all_results.json` 现值为修复前断裂态快照（新契约 6 点 cp1-cp5 恒 0）。**该账本已失效，仅作修复前断裂态快照保留**；evaluate.py `__main__` 红线禁改，账本解冻/迁移等 E4-03 迁 trace 后处置（report.md §10.5/§10.7）。

### 11.4 知会 E4（commit 指针订正）

E4 交接 README 中 commit 指针误写为 df93eca（该 commit 为 task-03 评审代码侧修复），**正确的双轨抽取起点为 b85f4a7**。此处登记订正；E4 文件为在途工作，本轮不改。

### 11.5 历史翻转幅度公布（task-04 §6）

fixed avg task_score **0.067→0.127**；open 类 3 点翻转明细：open_balanced_superconductor × 3 模型 **cp1 0→1.0、task_score 0.1567→0.2067**（Nora 评审点名项，方向守卫测试 test_open_case_flip_guard 已锁定）。完整对照与取证方式见 report.md §10.4。

### 11.6 边界自查（任务 #49）

- 本轮只改三处：task-04 头部状态行与 §1/§2 标题标记、report.md 新增 §10、progress.md 新增 §11；监工各节原文零改动；src/ 与 dag_poc/ 代码与测试零触碰；
- 数字全部取证：全量测试与 evaluate 重跑（15 点对照、open 3 点 0.1567→0.2067）均在 git archive HEAD 干净副本（/tmp/evid49）实跑，未动工作区 dag_poc/traces 在途文件（rehearsal_logs/ 无 task-04 证据，故走副本重跑路线）；legacy 9 点零差异为逐字段 dict 相等比对 9/9 IDENTICAL；open 3 点 BEFORE 以强制 `_extract_legacy` 模拟修复前抽取实跑取得；
- 时序偏差如实注明：任务指派链（b85f4a7→e967a1d）与 git 提交时序（e967a1d 11:36 先于 b85f4a7 11:40）不一致，§11.1 按 git 时序呈现并在表后注明；任务链语义顺序不变。

---

## 12. task-05 实施与评审修复记录（Taylor · 2026-08-07）

实施、裁定与修复明细见 report.md §11（§11.1 八条+四裁定 / §11.2 commit 链与归属偏离 / §11.3 基线演进 / §11.4 rehearsal 索引 / §11.5 评审合账 / §11.6 自证循环 / §11.7 后续项）。

### 12.1 task-05 commit 链（git 时序，每行均经 git log 实查）

| 序 | commit | 时刻 | 任务 | 一行简述 |
|---|---|---|---|---|
| 0 | ab7a2db | 14:11 | 检查点快照 | 六条线核查快照（混入 E1/E5 文件，判分影响经 worktree 对照实证为零；不列入交付链，见 report.md §11.2） |
| 1 | 1be7c3b | 14:22 | #52（§1） | 存在性接口三分支锁死 + 字节级无泄漏 + max_attempts 超限记 0 分实证；165 passed + 1 skipped |
| 2 | c0b41a3 | 15:13 | #53（§2） | 条件③改组成多重集比对；answer_schema 0.2.0→0.3.0；176 passed + 1 skipped |
| 3 | d8b393a | 15:38 | #54（§3） | composition_grounding_gate + 出入参文档；195 passed + 1 skipped |
| 4 | a411e03/8888944 | 16:26/16:33 | 监工验收 | 卷入评审修复代码侧（HEAD 与修复内容逐字节一致，QA 核实） |
| 5 | ef0e794 | 16:44 | #56 | 评审修复剩余面（测试/rehearsal/文档）；自报 198 passed，QA 实测 200 passed |

注：任务指派链写作 1be7c3b→ab7a2db→c0b41a3→d8b393a→a411e03/8888944→ef0e794，git 时序为 ab7a2db（14:11）先于 1be7c3b（14:22），本表按 git 时序如实呈现（ab7a2db 为检查点快照非交付本体）。

### 12.2 schema 版本登记（依 §9.1 升版记一行约定）

- **answer_schema.json 0.2.0→0.3.0**（c0b41a3，用户裁定四条落地）：open 类 edit_op 的 operator enum 由结构算子五件套收窄为组成算子 **{add_atom, remove_atom}**（其余三算子废弃，owner 裁定算子总数 ≤2）；detail 文法定为 `<token>|<token>*<n>`（支持 n>1）；组成比对容差 ε=0.02；跳数口径 ≤2 沿用 goal §2b④。本次只改 agent 可见的答卷字段枚举（open 类 edit_op），不牵连 fixed 类既有答卷字段、不动 dummy atom 结构规范化。
- **trace_schema.json answer_contract 同步升 v0.3.0**（ef0e794 Minor-5）并注明 edit_op 收窄。
- tools_schema.json 本轮仅 behavior_notes 描述性追加（⑦ 字节级同形锁死条 + ④ 判分侧落点说明），无 output 结构变更不构成升版。

### 12.3 测试基线演进与报数口径（实查核实）

**155 + 1 → 165 + 1 → 176 + 1 → 195 + 1 → 实测 200 + 1**（各档出处与报数差异（自报 198 vs 实测 200）见 report.md §11.3；收口以 QA 实测 200 为准）。唯一失败 test_material_key_recompute 归因 E1 扩量数据（worktree 因果切割确证，与本线零因果）；4 errors 在 43950b4 基线同样存在。

### 12.4 台账登记（problems.md，任务 #55 新增）

三项入 problems.md：①test_material_key_recompute 失败归因与常量同步；②**材料域编辑面运行态断裂**（独立条目：edit_molecule 分支 2 受理面为 mock_db admet/structures 旧 POC 键集，E1 晶体实体与材料身份键锚点实测均返 ANCHOR_NOT_IN_A；质量门整域 fail-closed，待收口后解封）；③ab7a2db 混提事件与提交纪律（检查点快照单独成 commit 不列入交付链）。

### 12.5 悬空引用处置（任务 #55）

“收口归 #55”类引用：#55 为本次任务编号，任务结束即失效。文档侧两处（tools契约规范.md §6.5 标题与正文）已改指 problems.md 台账条目；代码侧四处（test_composition_gate_task05.py 三处 + task05_condition3_composition_rehearsal.py 两处注释/输出，其中测试断言绑定 '#55' 字样）属代码改动，本任务纪律不改代码，留待后续代码任务同步。

### 12.6 边界自查（任务 #55）

- 本轮只改四处：task-05 头部状态行与 §5 交付物打勾、report.md 新增 §11、progress.md 新增 §12、problems.md 台账三条 + 议题群总览；tools契约规范.md 仅改 §6.5 两处悬空引用指向；监工各节原文零改动；src/ 与 dag_poc/ 代码与测试零触碰；
- 数字全部引自已验证口径：commit 链经 git log 实查（含时刻）；基线各档出处注明（commit message/评审 diff/QA 实测）；rehearsal 两份日志经 ls 实查存在；无杜撰数字；
- 时序偏差如实注明：任务指派链（1be7c3b→ab7a2db）与 git 时序（ab7a2db 14:11 先于 1be7c3b 14:22）不一致，§12.1 按 git 时序呈现并注明 ab7a2db 为检查点快照非交付本体。

---

## 13. task-05 追加四项实施与二轮评审记录（Taylor · 2026-08-07）

实施与修复明细见 report.md §12（§12.1 commit 链 / §12.2 关键机制 / §12.3 验证证据 / §12.4 E5 开工信号 / §12.5 二轮评审修复与知会 E4 / §12.6 归属偏离与台账登记）。

### 13.1 追加四项 commit 链（git 时序，每行均经 git log 实查）

| 序 | commit | 时刻 | 任务 | 一行简述 |
|---|---|---|---|---|
| 0 | 6c14fdd | 21:25 | 检查点快照 | 监工验收检查点，**卷入本线 gateway.py §10 主体与 PR-37 订正文本载体**（本线第二次归属偏离，Ryan S1 + Mark W2；HEAD 语义完整已核实，登记见 problems.md PR-42） |
| 1 | cea3c7b | 21:25 | #61 | 断言动态取数：test_material_key_recompute 归属纠正，按 key_type 分流动态取，PR-37 CLOSED |
| 2 | 963857d | 21:34 | #58（§10 / PR-40） | gateway 按 case 加载 per-case 视图并全工具过滤（五工具视图过滤 + CASE_VIEW_MISSING fail-closed，视图 23 份） |
| 3 | fd29ee8 | 21:48 | #59（§9） | 接口 B 降级键 grounding 复算（degraded 从严拦截，DEGRADED_KEY_NOT_UNIQUE 错误信封；E5 材料侧堵点解除） |
| 4 | 2e19d98 | 22:04 | #60（§7 R1-②） | resolve 组成↔实体 ID 一对多映射 + hit_gb 保三元组签名（未注册 case raise ValueError，遵 R1-①） |
| 5 | 05235d4 | 22:41 | 二轮评审修复（关联 #62） | 二轮三维评审代码侧合并修复 6 项：Critical-1 edit_molecule 视图闸 ANCHOR_NOT_IN_A / Warning-2 open 缺 case_id raise CASE_VIEW_MISSING / S3 PRIMARY_KEY_NOT_UNIQUE fail-closed / S4 backend_root 透传 / S5 统计键 / S6 断言独立取数 |

注：任务指派写作“§10 commit 链 6c14fdd→963857d→05235d4”——6c14fdd 实为检查点快照（21:25）卷入主体，§10 正式交付 commit 为 963857d，05235d4 为二轮评审修复；按 PR-39 纪律，检查点快照不列入交付链，验收记账以“四提交文件清单”（cea3c7b/963857d/fd29ee8/2e19d98）为准而非区间并集。

### 13.2 测试基线演进（实查核实）

**200 + 1 → 281 + 1 → 290 + 1 → 301 + 1 → 304 passed + 1 skipped + 5 errors**（dag_poc/mcp_mock_server/tests 全量口径；skip 为 mcp e2e 环境项，5 errors 为既有 fixture 环境项，均不计本线缺陷）：281+1（§10 视图接入前基线）→ 290+1（963857d 视图过滤用例）→ 301+1（fd29ee8/2e19d98 降级键与 resolve 用例）→ 304+1+5（05235d4 二轮修复后终态，Chris 复验实测）。

### 13.3 Chris 复验结论（6/6 PASS）

① 全量实测 **304 passed + 1 skipped + 5 errors**；② **15 实验点逐字节零差异**；③ 视图 23 份实查；④ primary 碰撞组 31 / 唯一键 5,709（Chris 实测口径）；⑤ 降级键碰撞组 51 / 涉及 102 条（键列 17,252 行 = primary 5,771 + degraded 11,481）；⑥ rehearsal 三份日志在库（PR-40_case_view_realdata.log / task-05-section9_degraded_key_realdata.log / task-05-section7_resolve_realdata.log）。

### 13.4 台账与知会登记（problems.md，任务 #62）

五项入 problems.md：①PR-40 状态更新为**已实施待 E4 合跑验收**（三通道 env DAGPOC_CASE_ID / 头 X-Dagpoc-Case-Id / 参数 case_id 已备，新增 env DAGPOC_CASE_KIND，harness 侧传 case_id 归 E4-04 接缝）；②新增契约债务（Mark W1：E6 bare_baseline.py judge_open_case 对未注册 case 无条件调 hit_fn，hit_gb raise 即崩；本轮不动区纪律不改 E6）；③归属偏离登记（Ryan S1 + Mark W2：6c14fdd 检查点卷入，PR-39 纪律延续）；④从严口径出入登记（Ryan S3：§9 实施口径“degraded 唯一命中也拦截”为任务书口径，压过调研 n≥2 口径，n≥2 从未落码）；⑤在途改动知会（Mark S3：b_partition.json 他线在途，验收以干净 worktree 为准）。跨线知会：DAGPOC_CASE_KIND 知会 E4（Ryan S2，见 report.md §12.5）；E5 开工信号（report.md §12.4）。

### 13.5 边界自查（任务 #62）

- 本轮只改四处：task-05 头部状态行与追加项状态订正、report.md 新增 §12 与 §11.3/§11.7 两处归属订正（Ryan W1）、progress.md 新增 §13、problems.md PR-40 更新 + 四条新台账 + 议题群总览；监工各节原文零改动；src/ 与 dag_poc/ 代码与测试零触碰；
- 数字全部引自已验证口径：commit 链经 git log 实查（含时刻）；基线 304+1+5、15 点零差异、视图 23 份、碰撞组 31/5,709 与 51/102 均为 Chris 复验实测口径；无杜撰数字；
- 归属偏离如实注明：6c14fdd 检查点卷入本线主体与 PR-37 订正文本载体（本线两次归属偏离），记账以四提交文件清单为准（PR-39 纪律延续），详见 problems.md PR-42。

---

## 14. task-06 实施与验收记录（Taylor · 2026-08-08）

实施与验收明细见 report.md §13（§13.1 问题与根因 / §13.2 commit 链与切源落点 / §13.3 六裁定对账 / §13.4 验收证据 / §13.5 分层口径解释 / §13.6 知会 E1 与台账）。

### 14.1 task-06 全链（调研→裁定→实施→验收→收口，git 时序实查）

| 阶段 | 产出 | 任务 |
|---|---|---|
| 调研 | task-06-research.md：逐工具现状表 + 五断言可执行方案 + R1-R8 风险清单 + 裁定项 F1-F5（F6 为 B∩A 口径收尾裁定） | #64（Alex，只读零改动） |
| 裁定 | 六项 F1-F6：F1 实物口径 169+预期失败 / F2 filter-only / F3 无别名 / F4 fixture 穷举 / F5 判分侧同源 / F6 B∩A 键空间收窄+占位机器识别 | Leader |
| 实施 | 5d987dd（08-08 11:44，主体切源随保护交付物全量提交入库）→ ee6a5c5（12:04，裁定收尾）→ cbcaa15（12:14，登记补齐） | #65（Jimmy） |
| 验收 | §3 五断言实跑 **5/5 PASS** + 回归 317+1+5 + 三红线全过 + 15 点零差异 | #66（Chris 独立实测） |
| 收口 | 本回写：task-06 状态行 + report.md §13 + progress.md §14 + problems.md PR-45 更新/PR-46~50 新增 | #67（Taylor） |

### 14.2 测试基线演进（实查核实）

**304 + 1 + 5 → 317 passed + 1 skipped + 5 errors**（304+1+5 为 task-05 终态基线，见 §13.3；317+1+5 为 ee6a5c5 commit message 自报与 Chris #66 复验一致；skip/errors 均为环境项）：新增用例为断言5 分层（176 逐 case）、B∩A 三条硬条件、消解方向机器断言、非占位相交两层 raise、冷启清缓存等。

### 14.3 验收数字与红线（Chris #66 实测口径）

五断言：29/29（视图白名单）/ z064386 无视图翻绿 / 视图外同码逐字节 / logS -6.1439 逐值一致 / 176 逐 case 169+19+1。三红线：mock_db 本体未删（E4 九组冒烟 legacy 重判保留，40/40 admet 实体仍可查）/ 视图闸不动区零漂移 / 存在性判定未退回内存小表。15 实验点逐字节零差异。

### 14.4 台账登记（problems.md，任务 #67）

PR-45 状态 OPEN→**已实施待监工终验收**（附五断言实测数字）；新增五条：PR-46 B∩A 红区断言键空间口径收窄与占位放行（F6 裁定依据、机器断言双保险、E1 ingest_open_gt 消解条件、22/24 case 占位现状）/ PR-47 filter_props mw/logp/tpsa filter-only（数据补齐归 E1 另立项）/ PR-48 half_life_days 旧题预期损坏（不做别名，F3）/ PR-49 任务书“80 道”与实物 176 道口径出入（F1，如实）/ PR-50 时延登记（冷启 12.3s→0.4s、test_ab_isolation 230s→110s、rehearsal 129s 时序竞争归因只登记不优化）。知会 E1：ingest_open_gt 后占位放行分支应归零（report.md §13.6）。

### 14.5 边界自查（任务 #67）

- 本轮只改五处：task-06 头部状态行、report.md 新增 §13、progress.md 新增 §14、problems.md PR-45 更新 + 五条新台账 + 议题群总览、task-06-research.md 裁定索引补一行；监工各节原文零改动；src/ 与 dag_poc/ 代码与测试零触碰；
- 数字全部引 Chris #66 实测口径与 git log 实查，无杜撰；
- 归属偏离如实注明：主体切源随 5d987dd 全量提交入库（卷入他线交付物，467 文件），归属按文件清单记账而非区间并集（PR-39/PR-42 同款纪律表述），见 report.md §13.2 与 problems.md PR-45。

---

## 15. task-07 阶段一五项 + §9 加严实施与验证记录（Taylor · 2026-08-09）

实施与验收明细见 report.md §14（§14.1 commit 链 / §14.2 五项逐条结果 / §14.3 #75/#78 修复纪要 / §14.4 改前改后口径 / §14.5 回归数字 / §14.6 台账登记）。

### 15.1 task-07 全链（调研→实施→验证→修复→收口，git 时序实查）

| 时刻 | commit | 任务 | 内容 |
|---|---|---|---|
| 16:06 | 3e1a72b | #69 调研基线 | 派阶段一三条 task |
| 16:49 | 1c8c2e8 | #71 G3 | 判分器 CP_WEIGHTS 权威化 + 四模板 v2 改写 + cp 集合一致性断言测试 |
| 17:09 | **c99a23f** | #70 G1 实际载体 | 删 40 旧 mock 条目 + k 默认 10→50 + 全局调用上限 50（CALL_LIMIT_EXCEEDED，submit_answer 豁免）；阶段一进展复验检查点，commit message 明写“E3-07 完成三项” |
| 18:08 | 9037cdf | — | 两条返工节落进 task 任务书（仅 4 文件；任务指派所指 G1 归属与此 commit 有出入，已如实登记） |
| 18:51 | faa436c | #73 G4 | 过程指标 _status 四态 + 捷径闸；混入 E5 已 staged 的 37 个模板归档 rename（归属偏离第三次，Lee 自产 4 文件纯净） |
| 19:30 | 85c67eb | #72 G2 | 9 工具说明书 516→6,014 字、三样齐备、错误信封动态清单 |
| 21:11 | 13bd258 | #75 | 33 条 canonical 去方向重规范化 + changelog 留痕 + sc_class 对外清单回退 + literature_search 示例订正 |
| 21:42 | 3f5c45f | #78 | tools_schema 内 sc_class 5 处字样清零 |

### 15.2 验证链（Chris 独立实测口径）

- **#74 总体 PASS 7/9**：项 8 揪出 33 条 NON-CANON 真实失败（推翻 Jimmy #72“假警报”自报，根因 RDKit 版本差异）；
- **#76 复验 PASS 7/7**：canonical 双环境（RDKit 2025.09.2/2026.03.5）扫描均归零、changelog 33/33 全量 JSON 解析级吻合、6 条抽查去方向等价独立复算、判分链路在场、sc_class 回退且 filter 能力保留（Cuprate=874）、literature_search 示例逐字段一致、全量 **361 passed + 1 skipped + 2 xfailed 零 failed**、回放 418/54 复现、CHECKSUMS 绿。

### 15.3 改前改后报错数对比口径（监工 §9 加严，如实）

改前基线已由 trace_replay_task07.py aggregate 复现锁定：UNKNOWN_PROPERTY=418、INVALID_ARGUMENT=54、工具成功率 48.7%。**改后数字当前无法产出（不虚报）**——报错数取决于模型在新说明书下的实际调用行为，须待下轮 E4/E6 harness 用新 tools_schema 实跑同口径回放才能得到；回放脚本 aggregate 模式即为届时对账工具。

### 15.4 台账登记（problems.md，任务 #77）

新增五条 PR-56~60（接现有最大编号顺延，他线 PR-51~55 未动）：PR-56 RDKit canonical 版本依赖债务（建库期无版本记录、33 条实证漂移、须 pin 建库版本、4,368 条版本稳定方向符号是否预防性去方向待监工裁定）/ PR-57 sc_class 捷径面（filter 圈 874 条答案域、对外隐藏但 filter 能力未动、待监工/E5 裁定；smoke_examples.json 3 处字样待裁定）/ PR-58 faa436c 混提归属登记（第三次）/ PR-59 “假警报”自报不实订正与流程教训 / PR-60 方案疑点 Q1/Q2 口径出入报备。议题群总览同步。

### 15.5 边界自查（任务 #77）

- 本轮只改四处：task-07 任务书状态行与六处结果行、report.md 新增 §14、progress.md 新增 §15、problems.md 五条新台账 + 议题群总览；监工各节原文零改动；src/ 与 dag_poc/ 代码、数据、测试零触碰；工作区他线在途文件未动（精确路径暂存）；
- 数字全部引 Chris #74/#76 实测口径与 git log 实查，无杜撰；改后报错数不虚报；
- G1 归属出入如实登记（9037cdf 实为任务书落盘，c99a23f 才是 G1 载体）；faa436c 混提归属偏离第三次按 PR-39/PR-42 同款纪律表述（PR-58）。

---

## 16. task-08 修四条阻塞项（PR-61/62/63/65）实施与验证记录（Taylor · 2026-08-10）

实施与验收明细见 report.md §15（§15.1 四条逐条结果 / §15.2 混装归属登记 / §15.3 验证数字与无 LLM 声明 / §15.4 replay 漂移口径差 / §15.5 调研疑点 / §15.6 台账）。

### 16.1 四条交付与载体 commit（混装入库，git 实查）

| 项 | 交付 | 载体 commit（混装） |
|---|---|---|
| PR-61 | _queryable_props 动态剔除投影全 None 键 + UNKNOWN_PROPERTY 语义 + tools_schema v0.3.2 九处/smoke 两处/docstring 三处同步 + viability 断言 5 测试（注入负例自证） | **a01a6eb**（08-10 17:40，owner“对比文档细化”裹入七件） |
| PR-62 | template_id 显式路由两处（L302 主路由 + L1901 aggregate，后者为调研补发现）+ DISCOVERY_TEMPLATE_ID 常量 + case_is_discovery 纯函数；30 道 dh 全进 open 分支，fixed 120/旧 open 4 零变化；防回归 4 测试 | **22e8d99**（他线“对比逻辑纠错”裹入三件，23 文件混装） |
| PR-63 | CharsetUtf8AsgiMiddleware 共用中间件（mock_server/gateway_mcp 两处 wrap），中文直读实证 | **22e8d99**（裹入三件） |
| PR-65 | keywords 字符串化数组/非 str 拒绝 INVALID_ARGUMENT 带正确形态示例，不做宽容解析 | **a01a6eb**（裹入） |

另：**181413b** 裹挟 Chris 取证脚本（.r83_inject.py/.r83_routing.py）入库，Chris 已删工作区文件；Leader 裁定不改写历史，建议全仓提交方改白名单式 add。此为**归属偏离第四次**（登记 PR-67）。

### 16.2 验证链（Chris #83 独立实测口径，HEAD=e968a33）

- 全量 **383 passed / 1 failed / 1 skipped / 5 errors**（errors 全预存在 fixture 缺失）；
- 唯一失败 test_resolve_one_to_many_isomers_all_collected 归因 **b9a3576**（task-07 批次⑤ dh_09 g_b 5 成员→单成员重注册，摧毁测试前提，与 task-08 零交集），二选一待监工裁定（PR-66）；
- aggregate 回放 **418/54 复现不变**；replay 漂移实查 ok→UNKNOWN_PROPERTY 11 次（与规格 ±14 口径差如实登记）；
- 红线全守（case 零改动/判分公式零改动/harness 未动/无同义词模糊匹配/无宽容解析）；修完未开全量矩阵，交回监工复验（E6 重跑 3 道）；
- **本轮无 LLM 调用**（ACTION_RULES §7.5 如实声明）。

### 16.3 台账登记（problems.md，任务 #84）

PR-61/62/63/65 四条状态 OPEN→**已修复待监工复验**（载体 commit 注明混装入库）；新增两条：PR-66 dh_09 重注册致测试失败二选一待裁定（归 E5 线或测试动态化）/ PR-67 外部全仓提交归属偏离第四次（含取证脚本裹挟，建议白名单式 add）。议题群总览同步（他线 PR 未动）。

### 16.4 边界自查（任务 #84）

- 本轮只改四处：task-08 任务书状态表与四条结果行、report.md 新增 §15、progress.md 新增 §16、problems.md 四条状态更新 + 两条新台账 + 议题群总览；监工各节原文零改动；src/ 与 dag_poc/ 代码、数据、测试零触碰；工作区他线在途文件未动（精确路径暂存）；
- 数字全部引 Chris #83 实测口径与 git log 实查，无杜撰；无 LLM 调用不编数字；
- 归属偏离第四次如实登记（本线文件无独立 commit，随 a01a6eb/22e8d99 混装、181413b 裹挟取证脚本；三代理逐件/逐 hunk 核验完整无篡改，不重写历史），见 report.md §15.2 与 problems.md PR-67。

### 16.5 两条小活收口登记（PR-66/PR-68，任务 #88）

Jimmy 干净提交 **9793fff**（无混装，任务 #86）落地两条：

- **PR-66**（监工裁定选 b：改测试不改题）：test_resolve_one_to_many_isomers_all_collected 改 `_multi_member_dh_case()` 动态挑 len(g_b)>=2 的 dh 注册题、不钉题号，无多成员样本时 pytest.skip 带原因（“需补样本而非删测试”）；禁魔法数承 PR-37 口径；
- **PR-68**：dag_poc 侧 test_ab_isolation.py 改唯一 basename **test_ab_isolation_gateway.py**（git R098，仅文件头自引用与改名登记，零语义变化），src/data_build 侧同名文件不动；
- **Chris #87 独立实测四项全 PASS**：pytest dag_poc src 单命令两遍一致 **384 passed / 1 skipped（预存在条件 skip）/ 5 errors（预存在 fixture 缺失）/ 0 failed** 无 collection error；动态测试实跑选中 case_dh_drug_07（2 成员）、skip 分支临时构造实证后还原；改名全仓功能性引用核查零残留；--collect-only 390 全收无同名冲突；
- **Leader 裁定登记**：Chris 提示的两处 docstring/注释旧名提及（test_backend_source_switch_task06.py:27、test_budget.py:31）属历史说明非功能引用，**不订正，只登记**；
- problems.md 同步：PR-66/PR-68 状态均改**已修复待监工复验**（监工裁定原文保留，载体 9793fff 与 Chris #87 实测摘要入解决路径），议题群总览 G2 补 PR-68；他线 PR 与监工手改原文零改动。
