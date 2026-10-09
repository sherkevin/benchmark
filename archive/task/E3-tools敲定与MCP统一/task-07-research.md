# task-07 阶段一五项调研（实施规格）

| 字段 | 值 |
|---|---|
| 任务 | #69 / E3-07 阶段一五项调研 |
| 上游依据 | `governance/方案定稿-20260809.md`（方案已定，本文只做行号级映射与依赖核查） |
| 基准 | HEAD `3e1a72b`（gateway.py 2,225 行，task-06 切源后行号已全部重核） |
| 性质 | 纯调研，零业务代码改动 |

监工 §0 两处更正已遵照执行：① `_a_universe()` 已切 backend（实测 A 全集 92,193），不按"连旧库"调研；② H2 三层套壳归 E4，本文不涉及。

---

## 0. 基线快照（§8 前置，改动前实测）

**预期基线 317+1+5 在当前工作区环境不可复现**，如实登记：

| 套 | 实测 | 与基线差异归因 |
|---|---|---|
| `dag_poc/mcp_mock_server/tests` | **235 passed + 20 failed + 1 skipped**（256 collected） | 20 failed 全为环境差异（见下），非代码回归 |
| `src/eval_matrix` | **71 passed**（0 failed） | 全绿 |

20 failed 的两组环境根因（均与业务代码无关，CHECKSUMS --verify 实测通过、数据未动）：

1. **`.venv` 缺 `pymatgen`**（7 项失败）：`dag_poc/gateway.py` `_load_material_identity_fns()`（L794 附近接缝）import `src.data_build.material_identity` 失败 → `material_key_from_record` 恒返 None → 材料侧测试红。涉及：`test_material_identity_grounding.py`（2）、`test_degraded_key_grounding_task05.py`（3）、`test_review_fixes_41.py::test_material_key_cache_keyed_by_backend_root`（1）、`test_composition_gate_task05.py::test_gate_primary_key_collision_not_unique`（1）。
2. **`.venv` 装了真 `mcp 1.29.0`**（13 项失败）：`test_mock_server_contract_tools.py` 的 `ms` fixture（L150-173）只在 `import mcp` 失败时才注 FakeFastMCP 桩；真 FastMCP 无 `.registered` 属性（实测 `dir()` 仅 add_tool/call_tool/list_tools/remove_tool/tool）→ 全部契约 smoke 红。基线时代该包不在场故走桩通过。

**给实施者的基线纪律**：动手前先 `pip install pymatgen`（或明确接受当前环境基线 235+20f+1s / 71），改后对比必须同环境同口径；20 failed 名单已在本报告留档，不得混入本次改动的回归判断。另：基线时代"5 errors"本次未复现（eval_matrix 全绿），按当前环境为准。

附带环境事件：调研期间发现端口 9000 有残留 `mock_server.py` 进程（PID 93340，他线实验遗留），已清理；实施期跑回归前建议先 `lsof -i :9000` 确认无占位。

---

## 1. §1 删 40 旧 mock 条目 + 默认 k 10→50

### 1.1 排序落点与实证（行号为 HEAD 现行）

| 落点 | 位置 | 现状 |
|---|---|---|
| 字符串排序 | `dag_poc/gateway.py` **L1392** `ordered = sorted(hit_ids)` | ID 字符串升序，两字母条目恒排最前 |
| 默认 k | `dag_poc/gateway.py` **L1360** `k = args.get('k', 10)` | 改 50 唯一代码点（上限校验 L1361 已是 1..50，不动） |
| 分页切片 | L1394 `ordered[offset:offset + k]` | 与 k 联动，不动 |

**实跑实证**（本次调研现场跑）：`search_candidates(constraints=[{element_count ge 1}], k=10)` 返回 **aa~aj 十个旧条目**，total_found=92,193；k=50/offset=40 起才是 `c00025` 起的新库 ID。方案 §1.1 描述与实物完全一致。

### 1.2 40 条旧条目的定义位置（删除对象）

ID 全集（40 个，get_entity_A.jsonl 第 1-40 行）：`aa-ah, ba-bh, ca-cd, da-dj, ea-ej`。

| 库文件 | 现行规模 | 待删 | 删后 |
|---|---|---|---|
| `dag_poc/data/backend/get_entity/get_entity_A.jsonl` | 92,193 | 40 行（第 1-40 行） | 92,153 |
| `dag_poc/data/backend/search_candidates/search_candidates_A.jsonl` | 92,193 | 40 行 | 92,153 |
| `dag_poc/data/backend/get_property/get_property_A.jsonl` | 240,664 | **197 行**（40 个 ID 的性质行，逐 prop 分布：element_count 40/atom_count 40/logS 27/bbb 27/logD 22/half_life 20/caco2 15/herg 6） | 240,467 |
| `literature_search_A.jsonl` | 309 | 0 | 不变 |

**CHECKSUMS 刷新链**：删后须重跑 `src/data_build/finalize_backend.py`（各工具 MANIFEST.json record_count/md5 + 根 MANIFEST）与 `src/data_build/generate_checksums.py`（CHECKSUMS.json 48 件主清单 + 2 附录），`--verify` 须 exit=0。注意 E5 task-06 收口登记："此后 CHECKSUMS 刷新须串行"（CHECKSUMS.json 为 E1/E5 共享文件），实施时先与并行线对时。

### 1.3 全仓依赖引用面（grep 全量，按处置分类）

**A 类·查库引用（删后行为改变，必须同步改）**：

| 文件:行 | 引用形态 | 删后影响与处置 |
|---|---|---|
| `dag_poc/gateway.py` **L2210-2222**（`__main__` 冒烟） | `keywords:'aa'`、`get_property('aa')`、`compare(['aa','ab'])` | 冒烟将 EMPTY_RESULT；改指新库 ID（如 z000374/c00001） |
| `dag_poc/tools_schema.json` | 8 处 `'aa'`（examples，如 L186 search_candidates 成功例、L338 compare 例） | examples 改新库 ID（§2 同批改） |
| `dag_poc/mcp_mock_server/pr40_case_view_rehearsal.py` L134-177 | `family_sequence:['aa']`、`get_property('aa')`、`compare(['aa','bd'])` | rehearsal 脚本，改新库 ID 或登记为历史件不再跑 |
| `src/data_build/migrate_gt_to_B.py` **L277-278** | 硬断言 `if got != 92193: fail` | 基线数字改 92,153（该脚本为 ADR-0020 暂缓手术件，改数字不改逻辑） |

**B 类·纯字面量/自建沙箱（不查真库，零漂移，勿误改）**：

| 文件:行 | 说明 |
|---|---|
| `src/data_build/test_ab_isolation.py` L124-141 | 沙箱负向自检自写临时 jsonl 用 'aa'/'ab'，不读真库；真实库键集断言①（A∩B=∅）删 40 条后键集缩小但断言不漂 |
| `src/data_build/ingest_gate.py` L209 | 读历史修复日志 fix_log 的 'aa' 键，字符串比对不查库 |
| `dag_poc/mcp_mock_server/tests/test_answer_dual_parse.py`（多处） | 答卷解析 fixture 字面量，不查库 |
| `dag_poc/mcp_mock_server/tests/test_composition_gate_task05.py` L49 等 | 合成证据表键，不查 backend |
| `src/benchmark/mock_tools.py` L7/L165 | 旧 mock 服务文档与示例；**mock_db.json 本体不删**（task-06 红线：E4 九组冒烟 legacy 重判依赖 40/40 admet 实体可查） |

**C 类·任务书点名但实测不改代码**：任务书 §1.2 点名 `mock_tools.py`、`test_ab_isolation.py`、`ingest_gate.py` "代码引用要同步改"——实测三者的 'aa' 引用全是 B 类字面量（不查 backend），删 backend 40 条不影响其运行；真正要改的是 A 类四处。此差异如实登记（不违背方案，属方案粗述与实物细化之差）。

**穷举测试爆炸风险（task-06 遗留项）已消解**：`tests/test_ab_isolation.py` L29 已改 fixture 小 backend（裁定 F4 口径，L309 明确"真实 A 全集 92,193 不做全库穷举"），删 40 条无爆炸面。

### 1.4 删后测试断言漂移预估

| 断言/测试 | 漂移判定 |
|---|---|
| `test_resolve_composition_task05.py` L207 `entities == 92193 or > 0` | 有 or 兜底，不漂 |
| `test_backend_source_switch_task06.py`（176 逐 case 断言5） | 169 gt 全是 z/c 新库 ID（唯一例外 xb_exp_1 属 B 侧），不漂 |
| per-case 视图负例（`per_case_view_assert_result.json` 的 `get_entity(aa∈全库∉白名单)`） | 期望码 UNKNOWN_ENTITY 删前后同码，行为不漂；**文案 "aa∈全库" 变假**，断言结果文件重生成时文案自然更新 |
| `migrate_gt_to_B.py` L277 | 必漂（A 类已列） |
| **30 道旧 gt 题**（gt 指向旧 40 ID：fixed_discovery 29 道 + constraint_satisfaction 1 道，实测清点；方案称"37 道旧题"，口径差 7 道未深究——或含 open 旧题/spare） | **删后这些题 gt 不可查、判分失效**。方案第 11 项（E5 归属）会把旧题移 `_deprecated/`，§1 与之是**时序耦合**：建议 Leader 裁定 §1 与第 11 项的先后，或接受旧题在删除后立即失效（它们本就不进新矩阵） |

### 1.5 k 默认值 10→50 的全部同步点

| # | 位置 | 现状 → 目标 |
|---|---|---|
| 1 | `dag_poc/gateway.py` L1360 | `args.get('k', 10)` → `args.get('k', 50)`（上限校验 L1361 `1<=k<=50` 已就位，零改） |
| 2 | `dag_poc/tools_schema.json` L139 | `"default": 10` → `"default": 50` |
| 3 | `tools契约规范.md` L72 | "`k` 1–50，默认 10" → 默认 50 |
| 4 | `tools契约规范.md` L275 | "如 `k` 默认 10" → 默认 50 |
| 5 | `tools_schema.json` L184 behavior_notes ⑤⑦ | "超 k≤50 硬上限"叙述保留；"57 实体"旧口径已于 task-06 订正，k 联动叙述补一句 |

**验收脚本建议**（对应方案 §1.3）：抽 `case_cs_drug_04` 实跑 `search_candidates(constraints=[logS 0.5..1.5 区间])`，断言返回 ID 全为 z/c 格式且合格候选在返回集内（该题约束下命中集合 < 50 时一页即含全部合格者——实施时用该题 constraints 实测命中规模留痕）。

---

## 2. §2 九工具说明书补全（含第 3 项错误带清单）

### 2.1 说明书消费链（单一事实源已确认）

`dag_poc/tools_schema.json` `tools[].description` 是唯一事实源，两条消费链：
- H1：`src/benchmark/agent_harness.py` L592-602（组 LLM tools schema 时直取 `t.get("description")`）
- H2：`dag_poc/mcp_mock_server/gateway_mcp.py` L191 `load_tool_descriptions()` → L199 `_fn.__doc__ = description` → L200 `MCP_APP.tool(description=...)`

⇒ 只改 tools_schema.json 的 description（与 input 结构），两个 harness 同时生效。deprecated 层（legacy 22 名）不在九工具范围，不动。

### 2.2 现状与缺口（逐工具）

九工具 description 现长 39~66 字符，**三样全缺**（无可查性质清单、无参数确切结构说明、无照抄示例——examples 数组存在但用旧 ID 且不在 description 内）。schema 每工具已有 `input/output/errors/behavior_notes/examples` 字段可承载，无需动结构。

**可查性质清单来源（backend 实测 top，供写清单）**——get_property_A.jsonl 按 `prop` 聚合：

| 性质 | 行数 | 性质 | 行数 |
|---|---|---|---|
| element_count | 92,193 | logS | 2,299 |
| atom_count | 74,941 | bbb | 757 |
| tc | 17,252 | logD | 604 |
| family | 17,252 | half_life | 523 |
| structure_in_db | 17,252 | caco2 | 235 |
| condition | 17,252 | herg | 104 |

运行期白名单 `_queryable_props()`（gateway.py L1936-1938）实测 16 项 = 上表 12 项 + filter-only 三项 `mw/logp/tpsa` + `sc_class`。**说明书必须标明：mw/logp/tpsa 可入 search_candidates constraints、但 get_property 查值被拒（task-06 裁定 F2）**；sc_class 为视图/判分内部投影，是否列入对外清单由实施者按 `_queryable_props()` 实物决定并留痕。

**逐工具缺口一句话**（九工具共性：缺①性质清单适用范围②参数确切结构③照抄示例；个性）：
- `search_candidates`：constraints 要 `[{property, op, threshold}]`、op 集是 `lt/le/gt/ge/eq/ne`（gateway.py L113 `_CONSTRAINT_OPS`，**不是 `>=`/`<=`**——实测传 `>=` 返 INVALID_ARGUMENT，这是 54 次 INVALID_ARGUMENT 的一大来源）；字段名 `props` 不存在的坑在 get_property 侧。
- `get_property`：入参是 `props`（字符串数组）不是 `properties`；性质名大小写敏感（`logS`/`logD` 精确拼写）。
- `check_molecule_exists`：max_attempts ≤ 10 服务端计数（L1149/L1620-1629）。
- 其余工具（get_entity/compare/submit_answer/edit_molecule/normalize_structure/literature_search）：按契约规范.md §1.1-1.9 既有口径抄入 description 即可。

### 2.3 错误信封带可用清单的落点

统一信封 `_err()`（gateway.py L166）。需改两处 UNKNOWN_PROPERTY 文案（其余错误码按需）：

| 落点 | 现文案 | 改法 |
|---|---|---|
| gateway.py **L1358-1359**（search_candidates 约束性质） | "约束性质 X 不在可查性质白名单（引擎按 property_spec/case.props 校）" | 追加"可用的有：{sorted(_queryable_props())}"+ 标明 filter-only 三项 |
| gateway.py **L1497**（get_property 性质） | "性质 X 不合法（引擎按 case.props 校）" | 同上，但可用清单须扣除 filter-only 三项（它们不可查值，列进去会二次误导） |
| gateway.py L1362（k 非法）/L1351（非法约束项） | 现有文案 | 可选：INVALID_ARGUMENT 文案补"constraints 项须为 {property,op,threshold}，op∈lt/le/gt/ge/eq/ne"（对应 54 次 INVALID_ARGUMENT） |

清单动态取 `_queryable_props()` 现算，不得硬编码（防库变更再脱钩）。

### 2.4 不做同义词模糊匹配的边界核查

实测 gateway 内**无任何性质名同义/模糊容错逻辑**（grep 同义/synonym/模糊/fuzzy 仅命中工具名 alias_map，L330-334/L1262——那是旧**工具名**归一，双轨分派的一部分，方案未要求移除，保留）。⇒ 本项是"不做"的边界声明，无需删代码；实施时在 schema meta 或契约文档登记"性质名精确匹配、无同义词纠错"即可。

### 2.5 验收口径说明

方案 §2.5 要求"报改前改后对比数字"（UNKNOWN_PROPERTY 418 / INVALID_ARGUMENT 54 / 成功率 48.7% 为基线）。对比重跑归实施者按 E4/H 侧既有 trace 重放链路执行；E3 侧交"改前基线数字登记 + 改后同链路重跑"。

---

## 3. §3 判分器权威化（模板按判分器改写）

### 3.1 差异实证（逐条给证据）

| 方案所述脱钩 | 实测证据 |
|---|---|
| 判分器硬编码抄旧模板权重 | `src/benchmark/evaluate.py` **L1603-1605**：注释"取自 templates/fixed_discovery/template.json"+ `CP_WEIGHTS={cp1:1,cp2:2,cp3:2,cp4:2,cp5:1,cp6:2}` + `CP_THRESHOLDS={cp1:0.6,cp2:1.0,cp3:0.8,cp4:0.5,cp5:1.0,cp6:1.0}`；汇总消费 L1754-1756 |
| 模板还写旧工具名 | `fixed_discovery/template.json` `tools` 键实测含 `predict_admet`、`search_molecule_database`、`search_drug_literature`（旧名）；判分器侧双轨分派在 evaluate.py L120/L123（extract_tool_data 按工具名集合显式分派，L78 注释）+ L1632 alias 归一 |
| 答卷 2 字段 vs 3 字段 | 模板 `response.candidate_result` 实测 `{screened_pool, ranked_candidates}` 2 字段；判分器/answer_schema.json 实测顶层 required = `candidates/ranking/answer`（+open 三件套 anchor_molecules/cited_values/edit_op）3+3 字段 |
| "判分器有 cp5、模板没有" | **部分与实物不符，见 §8 方案疑点 Q1** |

### 3.2 模板消费面（改写时必须保形的三处运行时读取）

| 读取点 | 依赖 template.json 的什么 |
|---|---|
| `dag_poc/mcp_mock_server/tests/gwtest_common.py` **L32** | `load_dag_edges(fixed_discovery/template.json)` → EDGES（测试基线） |
| `dag_poc/gateway.py` **L2220**（`__main__`）+ L2194 `load_dag_edges` 实现 | `checkpoint_dag[].id/next` 边表 → `no_path_violation` |
| `src/benchmark/evaluate.py` L1784 | `cases_dir='templates/fixed_discovery'`（main 冒烟目录） |

**改写约束**：`checkpoint_dag` 必须保持 list-of-{id, pre/next} 结构；cp 全名（如 `cp5_ranking`）与判分器短名（`cp5`）的映射已有现成机械口径 `cp_short()`（gateway.py **L2203-2205**：取前 3 字符），断言与模板改写都复用它，不得另立新映射。

### 3.3 "模板按判分器改写"改动文件清单

| 文件 | 改动 |
|---|---|
| `dag_poc/templates/fixed_discovery/template.json` | tools 换九工具新名（可照抄 tools_schema.json tools[].name）；response 改 `candidates/ranking/answer` 3 字段；cp5 保留（已在）；cp 命名保持 `cpN_xxx` 前缀形式 |
| `dag_poc/templates/constraint_satisfaction/template.json` | **补 cp5**（实测唯一缺 cp5 的 fixed 系模板：cp 列表现为 cp1/cp2/cp3/cp4_filter/cp6）；tools/response 同上 |
| `dag_poc/templates/constrained_optimization/template.json` | 6 cp 已齐；tools/response 对齐（实测 tools 字段也含旧名，实施时逐一核对） |
| `dag_poc/templates/open_discovery/template.json` | 同上（open 系，判分走 _score_open/_score_open_discovery，改写只动文档面不改判分） |
| `src/benchmark/evaluate.py` L1603 注释 | "取自 templates/fixed_discovery/template.json"改指改写后模板（或注明"模板以判分器为准，本常量为权威"） |
| `discovery_haystack/` | 无 template.json（纯 case 目录），不动 |

### 3.4 cp 集合相等断言的插入点与实现方案

- **插入点**：`dag_poc/mcp_mock_server/tests/` 新建 `test_template_judge_cp_alignment.py`（与既有"机器断言"先例同款——task 书引 `assert 'def resolve(' not in gateway源`，现例见 `test_mock_server_contract_tools.py::test_ast_contract_names_all_registered` L81 AST 断言模式）。
- **实现**：对四个有 template.json 的模板，`{cp_short(c['id']) for c in checkpoint_dag} == set(evaluate.CP_WEIGHTS)`，不等即 raise 并打印差集；import 走 gwtest_common 既有 sys.path 注入，零新基建。
- **拦得住的证明**：实施时先临时把某模板删一个 cp 跑一次看断言红（留痕后还原）。

---

## 4. §4 过程指标前两条

### 4.1 "没执行到 vs 执行了但错"的可区分信号（trace 结构核查）

trace = 录像行列表（gateway.py `history_rows` L1161，每行 `{t, tool, args, ret, ok}`）；判分侧 `score_case`（evaluate.py L294）经 `extract_tool_data(events)` 已按工具名抽取（L297，task-04 双轨分派后的新契约）。**每个 cp 的 0 分都可拆因**：

| cp | "没执行到"信号 | "执行了但错"信号 | 现行代码（都记 0） |
|---|---|---|---|
| cp1 | events 无 literature_search 行（`lit_searches==0`） | 有检索但 `lit_unique_papers==0` | evaluate.py L335-337 |
| cp2 | 答卷 pool 空且 trace 无 search_candidates 成功行 | pool 非空但 gt∉pool / pool∩录像集=0 | L340 |
| cp3 | `admet_results` 空（无 get_property 成功行） | 有查证但覆盖不全 | L341-349 |
| cp4/cp5 | ranked 空 | ranked 非空但与 gps 不符 | L350-363 |
| cp6 | best 为 None（未交卷/无答案） | best 非 gt 且非 gps | L364-365 |

**实现方案**：`_score_fixed`/`_score_open*` 各 cp 计算处同步产出 `status[cp] ∈ {not_attempted, attempted_wrong, partial, passed}`（信号全在上表现有变量里，无需新查库），随返回值带出。
**兼容性约束**：`score_case` 返回 4 元组被 **8 处**消费（evaluate.py L1807 main + tests 7 处：test_answer_dual_parse L134/135/145/166/190、test_tool_extract_dispatch L240/266、test_evaluate_open_discovery L204）——status 以"scores dict 内加 `_status` 子键"或"第 5 返回元"二选一，前者零破坏、推荐前者；open 侧 `_score_open_discovery(...)[:4]`（L321）切片语义同保。
**验收**：用任务书实测参照（cp1=1.0, cp3=0.667, 四个 0）造/找一份真 trace 跑判分，断言四个 0 各自的 status 标注与人工读 trace 一致。

### 4.2 捷径过滤闸

现状：evaluate.py/gateway.py 内**无任何捷径/跳步逻辑**（grep 捷径/跳步/shortcut 零命中）——纯新增。
可复用基建：`no_path_violation`（gateway.py L1246-1251）+ `evidence_time`（L1241，DEFAULT_EVIDENCE_MAP L148 已有 cp→工具映射）已能判"cp 完成时刻时序"；**捷径闸 = 下游 cp 得分而上游 cp status=not_attempted 时封顶/清零**（如未做 cp2 检索却 cp6 蒙对 gt）。
**插入点**：`_score_fixed` 末尾、return 前（L366 附近），输入即 4.1 的 status 表——故 4.1 是 4.2 的前置，两条同人串行做。
**口径留痕**：闸的封顶规则（清零 vs 限 0.3 与 scoring.gate_cap_on_wrong_answer 对齐）由实施者定并写进判分 docstring + progress 登记。

### 4.3 本轮不做项（只登记）

`no_regression` 检查点与 `H_s`/Cost/pass@k 方差——按方案留论文修改期，在 report.md/progress.md 登记"未启动、依据方案定稿第四组 PR-22 表"即可，零代码。

---

## 5. §5 单题调用上限 50

### 5.1 现有计数机制盘点（max_attempts 是单工具、非全局）

| 机制 | 位置 | 口径 |
|---|---|---|
| `search_budget` | gateway.py L1146/L1298-1300（literature_search）、L1368-1370（search_candidates） | 两检索工具共享；**默认 None=不限** |
| `verify_budget` | L1145/L1482-1484（get_property） | 默认 None=不限 |
| `max_attempts` | L968/L1148-1151/L1620-1629（check_molecule_exists） | **单工具**上限 `max(1,min(x,10))` 写死 10，服务端计数 `existence_used`，超限返 exists=False+ATTEMPTS_EXCEEDED |
| 全局调用计数 | **不存在**（无 call_count/total_calls） | —— |

compare/edit_molecule/normalize_structure/get_entity/submit_answer 无任何计数。

### 5.2 实施规格

- **计数口径建议：按 case 全局**（Gateway 实例=一题一会话，天然单题边界）——方案说"单题工具调用上限"，全局口径才能限制"穷举翻页"（翻页走 search_candidates，若按工具分别限 50，九工具合计可达数百次，与方案意图相悖）。此口径选择登记为实施交接项，若 Leader/监工另有口径以裁定为准。
- **插入点**：`call()` 统一入口 gateway.py **L1255-1281**——alias 归一后、`_execute` 前加全局计数检查；`self.calls_used` 与 `self.call_limit=50` 在 `__init__`（L1145-1166 预算区）初始化。
- **超限错误信封**：沿用 `_err()`，建议新码 `CALL_LIMIT_EXCEEDED`，文案明确"已达单题工具调用上限 50 次，本题不再接受工具调用"（方案 §5.2 要求不静默）；错误码表同步进 tools契约规范.md §2.4（12→13 个）。**submit_answer 是否豁免须裁定**：建议豁免（触顶后仍允许交卷，否则 agent 连已有成果都交不出，cp6 恒 0 不可解释）——作为交接裁定项报 Leader。
- **录像留痕**：超限调用仍落 history_rows（ok=False，与 M2 修复同款不变式），触顶率统计（归 E6）直接数 trace 中 `CALL_LIMIT_EXCEEDED` 行即可，E3 侧零额外接口。
- **与既有三预算关系**：不动（更细粒度的旋钮保留）；全局上限是叠加的最外层闸。

### 5.3 与 k=50 联动的穷举不可行性实证

实测 A 全集 92,193；92,193/50 = 1,843.86 ⇒ **≥1,844 次翻页 ≫ 50 次上限**（数字与方案一致）。即使只扫分子侧 74,941 也需 1,499 次。⇒ k=50 与上限 50 落码必须同一批改动合入（两数同量级自洽），不得分批上线。

---

## 6. §8 风险规避落实

| 风险 | 落实 |
|---|---|
| 删旧条目牵连面广 | 基线快照见 §0（先跑后改纪律已立）；依赖面 A/B/C 三类已全量 grep 分类（§1.3）；30 道旧 gt 题的时序耦合已报（§1.4） |
| 说明书写太细泄题 | 粒度边界：只列性质**名单**（12 可查 + 3 filter-only 标注）与参数**结构**；**不列**任何性质的取值范围/分布/均值（本次调研表中出现的行数统计属库规模口径，供实施者写断言用，不得进 description） |
| 上限 50 卡死 open 题 | 触顶率监控口径：E6 跑矩阵时按 trace 统计 `CALL_LIMIT_EXCEEDED` 出现率（per-case），open 题触顶率若大面积（口径由 E6 定阈值）报监工，**不得自行放宽**（方案红线）；E3 侧保证错误码可机读即完成义务 |

---

## 7. 实施分组建议（供 Leader 派单）

| 组 | 内容 | 理由 |
|---|---|---|
| **G1（同人串行）** | §1 删 40 条+k=50 + §5 上限 50 | 同落 gateway.py 检索/计数面（L1360/L1392/L1255），且 k=50 与上限 50 必须同批合入（§5.3）；§1 的 CHECKSUMS 刷新也在这条链 |
| **G2（独立）** | §2 说明书+错误带清单 | 落点全在 tools_schema.json/契约文档 + gateway 两处错误文案，与 G1 仅 tools_schema.json examples 的旧 ID 有微交叉（G1 删条目时顺手改 examples 或留 G2 收尾，二选一报 Leader） |
| **G3（独立）** | §3 模板改写+cp 断言 | 落点全在 templates/ + evaluate.py 注释 + 新测试文件，与 G1/G2 零文件交叉 |
| **G4（依赖 G3 无、依赖 G1 无，但建议在 G1 后）** | §4 过程指标两条 | 只动 evaluate.py 判分面；4.1 先行 4.2 串行；回归面与 G1 的库删除无耦合，但基线对比放 G1 后更干净 |
| 验证 | 全量回归 + 基线对比 | 归 Chris 线；注意 §0 环境差异须先修复（pymatgen）或冻结当前环境口径 |

并行度：G2/G3 可与 G1 并行开工（文件面不撞）；G4 建议排 G1 之后。

---

## 8. 方案疑点

**Q1（非致命，口径细化）**：方案定稿第 10 项与 task-07 §3.1 均述"判分器有 cp5、模板没有"。实测：**fixed_discovery 模板 checkpoint_dag 含 `cp5_ranking`（6 cp 齐）**；真正缺 cp5 的是 `constraint_satisfaction/template.json`（cp 列表现仅 cp1/cp2/cp3/cp4_filter/cp6）。另模板 cp 用全名（cp5_ranking）、判分器用短名（cp5），"相等"须经 `cp_short()` 前缀映射（gateway.py L2203）。不构成方案错误（"补 cp5"动作仍需要、落点是 constraint_satisfaction），但实施与断言必须按实物口径执行，登记备查。

**Q2（数字口径，仅留痕）**：方案第 11 项称"37 道旧题"；本次按 `templates/*/case_*.json` glob 实测 gt 为旧 40 ID 的题 30 道（fixed 29 + cs 1），差 7 道未深究（或含 open 旧题、spare 件或非 case 前缀文件）。不影响 §1 实施（删条目不看题数），留痕。

其余方案数字全部实测吻合：92,193 / 40 条两字母（0.04%）/ 74,901+17,252 组成* / 418 次 UNKNOWN_PROPERTY 与 48.7% 成功率引自监工实测（本线未重跑 trace 统计）/ 1,844 翻页推算 / 默认 k=10 / sorted(hit_ids) 字符串排序。**注**：任务书 §1.1 写"z+6位 74,901"，与 get_property 侧分子行数 74,941 及 task-06 报告口径 74,941 存在 40 之差（74,901+40=74,941，疑任务书口径未计旧 40 中的分子 40 条）——不影响任何实施动作，留痕。

## 9. 附：改动点汇总（实施者交接清单）

1. backend 三库删 40/40/197 行 + finalize_backend + generate_checksums（§1.2）
2. gateway.py L1360 k 默认 50；L1255 call() 全局计数+CALL_LIMIT_EXCEEDED（§1.1/§5.2）
3. gateway.py L1358/L1497 错误文案带可用清单（动态取 _queryable_props）（§2.3）
4. gateway.py L2210-2222 __main__ 冒烟改新库 ID（§1.3-A）
5. tools_schema.json：description×9 三样补全 + default k=50 + examples 去旧 ID（§2.2/§1.5）
6. tools契约规范.md：k 默认两处 + 错误码表 +1 + 性质名精确匹配登记（§1.5/§5.2/§2.4）
7. templates/ 四模板改写 + evaluate.py L1603 注释（§3.3）
8. 新测试 test_template_judge_cp_alignment.py（§3.4）
9. evaluate.py _score_fixed/_score_open*：status 标注 + 捷径闸（§4）
10. migrate_gt_to_B.py L277 基线数字（§1.3-A）

**裁定请求（报 Leader）**：① submit_answer 是否豁免全局上限（§5.2，建议豁免）；② 全局 vs 按工具计数口径确认（§5.2，建议全局）；③ §1 与方案第 11 项（E5 移 _deprecated）时序（§1.4）；④ sc_class 是否列入对外性质清单（§2.2）。
