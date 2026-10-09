# task-05 二轮调研报告（task #57）——§10 视图接线 + §9 降级键 + §7 resolve + 断言动态化

> 调研人：Alex（只读调研，零代码改动）。日期：2026-08-07。
> 依据：task-05 文件 §7/§9/§10/§11、report.md 监工验收节（L602-645）、problems.md PR-40/PR-37、
> ADR-0018 修订 R1（59a35ac）、E4-04 任务书、E1《材料侧身份键交接说明》§7.1/§7.1.1/§7.1.2。
> 优先级（监工排定）：**§10 视图接线 > §9 接口 B 降级键 > §7 resolve 映射 > 断言动态取**。

## 0. 核心结论（六条）

1. **§10 的机制件全部就绪、只差 gateway 接线**：E1-05 `per_case_view.py`（425 行，load_view/validate_view/ViewedBackendReader/enumerate_in_view）可直接复用；**E4-04 harness 一半已实现且在途**（agent_harness.py 已改：参数名定为 `case_id`、头名 `X-Dagpoc-Case-Id`、env `DAGPOC_CASE_ID`）——E3 沿用 `case_id` 即零改动对齐。
2. **视图数据现状是验收的真正堵点**：`per_case_views/` 目录实测只有 E1 示例 `e105_example_001.json` 一份；15 道 dh open 的 `view_ref` 已回填指向 `case_dh_drug_<nn>.json` 但**文件全部不在场**（E5-04 在途）；`b_partition.json cases` 也只注册了 2 个 fixture（R1-① 注册同样在途）。§10 代码可以先做并用示例视图验收机制，生产验收须等 E5 视图文件。
3. **gateway 五个 A 侧工具的过滤插入点已全部定位到行号**（见 §1.3）；`check_molecule_exists`（查 b_set）与 `edit_molecule`/`normalize_structure`/`submit_answer` 不在过滤面内的边界确认成立。
4. **§9 的复算纯函数 E1 已交付**（`degraded_material_identity_key(formula, tc)`，material_identity.py L113-125），缺口只在判分/grounding 消费面：`_material_key_universe()`（gateway L1278-1307）现算仅 primary 5,740 键，11,481 条降级键在 evaluate.py 条件②（L844）与质量门（L1086）不可见。
5. **§7 resolve 的数据源完备**：G_B 成员是实体 ID（15 道 dh 的 g_b 全在 case JSON），其组成在 `get_entity_A.jsonl`（92,193 行）的 formula 字段全可得（实测 z064386 在场）；`get_entity_B.jsonl` 为 0 行——resolve 挂 evaluate.py 判分侧，复用 task-05 的 composition_of/composition_equal 纯函数组即可。E6 消费点已锁定（bare_baseline.py L286-318 import evaluate.hit_gb）。
6. **断言动态化的失败已实跑复现**（`assert 17252 == 5771`）；硬编码共 3 处断言（L63/L74/L79）+ 3 处文案（L7-8/L58/L180）。键列已扩至 17,252 行（primary 5,771 + degraded 11,481），动态化必须按 `key_type` 分流，不是简单换数字。

---

## 1. 规格①（最高优先）：§10 PR-40 —— gateway 按 case 加载 per-case 视图

### 1.1 视图机制现状（E1-05 交付物实查）

| 项 | 实测 |
|---|---|
| 载体 | `src/data_build/per_case_view.py`（425 行；import backend_reader） |
| 视图路径 | `dag_poc/data/backend/per_case_views/<case_id>.json`（`views_dir()` L55-56，VIEWS_DIRNAME L38） |
| schema | `per_case_view_v1`（L39）；validate_view L78-152 载入期拦 n≥1/d∈[1,2]/K≥10/family∩distractors=∅/gt∉白名单 |
| 白名单字段 | `visible_entity_ids` = family_sequence ∪ distractors（load_view_file L162-163 推导回填）；文献白名单 `literature_doc_ids` |
| 参考过滤层 | `ViewedBackendReader` L204-291：get_entity/get_property 白名单外→UNKNOWN_ENTITY；search_candidates ∩白名单空则 EMPTY_RESULT；literature_search papers∩doc 白名单——**gateway 实现应与此错误码口径一致** |
| A 侧工具集 | `A_SIDE_TOOLS` L40 = (search_candidates, get_entity, get_property, literature_search)——**compare 不在 E1 清单**（E1 版未含，gateway 侧 compare 无需独立过滤，见 §1.3） |
| 目录实查 | 仅 `e105_example_001.json`（n=4/d=1/K=10，family c01350-c01353，distractors 10 个 c000xx，gt.entity_ref=c01360，5 个 doc_id） |
| open case 注册 | **0 道**：15 道 dh case JSON 的 view_ref 全指向不存在的文件；4 道旧 open（case_open_*superconductor*）无 view_ref 字段 |

### 1.2 会话构造与 E4-04 接缝（对齐结论：**已定名，E3 直接沿用**）

| 项 | 实测证据 |
|---|---|
| Gateway 现状 | `Gateway.__init__` 已有 `case_id=None` 构造参数（gateway.py L684、L705），现用途仅 B 分区装载（L706-709）。视图加载**加在构造参数同一入口**，不新增会话方法 |
| E4 harness 已实现（在途未提交，git status `M src/benchmark/agent_harness.py`） | `CASE_ID_HEADER="X-Dagpoc-Case-Id"`（L130）、`CASE_ID_ENV="DAGPOC_CASE_ID"`（L131）、`CaseIdentityError`（L134）、`resolve_case_identity`（L151-164：open 缺 case_id 即 raise）、`verify_case_channel`（L167-181：open 类 env 不一致即 raise）、`MCPBackend(case_id=...)` 每请求带头（L324-327）、`run_case` 全链接线（L661-666） |
| server 侧现状 | mock_server.py L76 与 gateway_mcp.py L130 均只读 env `DAGPOC_CASE_ID` 构造 `Gateway(case_id=...)`；**X-Dagpoc-Case-Id 头当前无人消费**（harness 注释 L126-127 明示它是"会话级预留"） |
| 对齐结论 | **参数名 `case_id` 已由 E4 定死**（其注释 L128-129：E3 若另行裁定只需改一个常量）。E3 沿用 `case_id` 即零改动对齐，**建议不再另裁**。现行运行通道 = env（一 case 一 server 实例）；头通道留作多会话并发预留（需 FastMCP 中间件读头，建议二期，见裁定项 F2） |
| 与 #54 口径关系 | evaluate.py `_load_per_case_view`（L923-938）已消费同一视图文件：不在场返 None 回落全局 A（**造题期**合法回落）。gateway 是**运行期**，口径必须更严：open 缺视图 raise，不回落——两侧读同一文件同一 schema，建议 gateway 直接复用 E1 `per_case_view.load_view`（自带 validate_view，import 接缝仿 material_identity 先例 gateway.py L626-633） |

### 1.3 五个 A 侧查询工具：数据装载与过滤插入点（逐个到行号）

| 工具 | 现状数据源（行号） | 视图过滤插入点 | 错误码口径 |
|---|---|---|---|
| literature_search | `_tool_literature_search` L884-910；扫 `self.db['literature']`（mock_db），doc_id=doi\|title（L901） | papers 组装后、L907 空判前：`papers = [p for p in papers if p['doc_id'] in visible_docs]` | 空 → EMPTY_RESULT（与 L908 同码，不区分"无文献"与"不在白名单"） |
| search_candidates | L912-967；universe=`_a_universe()`（L948 调，L1309-1327 从 mock_db admet+crystals 建） | L948 取 universe 后一行裁剪：`universe = {i: v for i, v in universe.items() if i in visible_ids}`（其下 hit_ids/total_found/offset 全链自然受限） | 无命中 → EMPTY_RESULT（L960 既有） |
| get_entity | L969-1020；入口查 db admet（L975）/structures（L1010） | L974 id 校验后、L975 前插一行：不在白名单 → 直达 L1019-1020 同款 UNKNOWN_ENTITY | UNKNOWN_ENTITY（与"不存在"同码，防源探测——与 ViewedBackendReader.get_entity L267-270 同口径） |
| get_property | L1022-1048；`_entity_props(eid)`（L1038→L1382） | 保持既有计费顺序（docstring L1025-1027：先计费后校验防免费枚举）：L1037 计费后、L1038 前插白名单判 → UNKNOWN_ENTITY | UNKNOWN_ENTITY |
| compare | L1052-1082；只消费 `self.observed`（get_property L1047 单一写入口） | **无需独立过滤**：get_property 过滤后白名单外实体进不了 observed，compare 自然拿不到值。加一行防御性注释说明此不变式即可（任务书列 compare 是为防绕路，机制上已被上游闸住） | 既有 PROPERTY_NOT_VERIFIED/UNKNOWN_ENTITY（L1069-1071）不动 |

**过滤面外边界确认**（任务书点名的四个）：
- `check_molecule_exists`（L1144-1171）：查 `self.b_set`（B 集合），语义是"∈ B 否"，与 A 视图无关——**不过滤**；
- `edit_molecule`（L1173-1252）/`normalize_structure`（L1253-1277）：锚点合法性基准（`_a_smiles_universe`/`_material_key_universe`）是 judge 侧 grounding 条件②的判据源，若收窄到视图会把"anchor ∈ A（全集）"语义偷换成"anchor ∈ 视图"，grounding 判据与运行期可见性是两层——**本次不过滤**（如需收窄另立裁定）；
- `submit_answer`：收卷工具，无查询面。

### 1.4 未注册视图的报错设计（open 不得静默回落全库）

| 项 | 建议 |
|---|---|
| 判定时机 | `Gateway.__init__` 装载期（fail fast，与 b_set/G_B 三条构造断言 L716-740 同款风格）：`case_id` 非空且视图文件不在场/校验失败时 raise ValueError |
| open/fixed 区分（裁定项 F1） | **主案**：harness 已算出 `case_kind`（resolve_case_identity L159），Gateway 增可选参数 `case_kind=None`：open+缺视图 → raise；fixed+缺视图 → 不过滤放行（代码路径统一走同一装载函数）。**备案**（不加参数）：以"b_partition.cases[case_id] 已注册"作 open 判据——但 R1-① 注册在途，时序耦合，不推荐 |
| 错误码（若走到工具层） | 新增 `CASE_VIEW_NOT_REGISTERED`（装载期 raise 为主，信封仅作双保险）；文案不含库规模/白名单内容 |
| fixed 统一路径 | fixed 类可不加载视图但走同一函数：`_load_case_view(case_id)` 返 (view, None/err)，fixed 容忍 None、open 不容忍 |

### 1.5 验收方案（可执行）

1. **机制验收（不依赖 E5，立即可跑）**：case_id=`e105_example_001` 构造 Gateway，断言：①五工具任一查询返回的实体 ID 集 ⊆ 该视图 14 个 visible_entity_ids；②`get_entity('c01360')`（gt）返 UNKNOWN_ENTITY；③`literature_search` 返回 doc_id ⊆ 5 个白名单 doc；④未注册 case（如 `case_dh_drug_01`）构造即 raise。
2. **生产验收（与 E4 合跑，待 E5 视图文件）**：任选一道 dh open，`DAGPOC_CASE_ID=<case_id>` 起 server，harness 实跑后解析 trace 录像全部 search_candidates/get_entity/get_property 成功行的 id 集 ⊆ 白名单，且 gt.id（case JSON `gt.id`）不在任一返回行——与 E4-04 任务书 §2 的合跑条款对接。
3. **回归**：fixed 组 15 实验点口径零变化（fixed 无视图时行为与今日逐字节一致）。

### 1.6 不动区

`check_molecule_exists` 全链（L1144-1171 与 b_set 装载三断言）、`_a_universe`/`_entity_props` 本体（只在调用点裁剪，不改函数内口径）、录像层（call/history/queried/docs 不改）、mock_db 数据、`_load_per_case_view`（evaluate 侧既有）。

---

## 2. 规格②：§9 接口 B 降级键 grounding 复算（E5 材料侧 11~12 道的堵点）

### 2.1 现状盘点（带行号）

| 项 | 实测 |
|---|---|
| 键列 | `src/data_build/_build_state/material_identity_keys.jsonl` 17,252 行：primary 5,771（`key_type:"primary"`，`<reduced>_SG<n>`）+ degraded 11,481（`key_type:"degraded"`，`<reduced>_ALT_<floor(tc)>_K<元素种类数>`，含 `degradation_reason`） |
| 复算纯函数（E1 已交付，零 IO） | `src/data_build/material_identity.py`：`tc_tier_floor`（L101）、`degraded_material_identity_key(formula_anon, tc)`（L113-125）、主键 `material_identity_key`（L90）。E1 自检 S6 已全量实测：A 侧可见字段（formula+tc）可独立复算同一把降级键 |
| gateway 侧缺口 | `_material_key_universe()`（gateway.py L1278-1307）只走 `material_key_from_record`（L643-663，需 SPACEGROUP 段）→ 现算仅 primary 5,740 键；降级行无空间群段，`material_key_from_record` 对它们返 None，**判分面不可见** |
| grounding 消费点 | evaluate.py 条件② L844 `a_material_keys = judge._material_key_universe()`（anchor 材料键 == 判 in_A，L855）；质量门 composition_grounding_gate L1086 同消费。两处对降级键 anchor 当前一律判 not-in-A |
| 碰撞登记 | primary 31 组/62 条（全为 3DSC 上游重复录入，gateway L1285 注释与测试 L79 已锁"同键判等是正确行为"）；全库降级后唯一键 17,170、冲突组 82/164 条（E1 交接 §7.1.1） |
| 一次性告警现状 | gateway `_MATERIAL_IMPORT_WARNED`（L612、L635-639）：import 失败一次性 logging.warning 后回落空集——§9 要补的是**降级键命中碰撞时**的显式信号，现状完全没有 |
| 缺口登记 | E1 交接说明 §7.1.2（L138-145）：监工已裁定"降级键纳入 grounding == 口径，归 E3 走 formula+tc 复算路径"；**硬前置**：E3 未实证 11,481 条可判等前，E5 不得按降级键造材料题 |

### 2.2 复算逻辑规格

**复算什么**：给定降级键形态 anchor（或实体记录），用 A 侧可见字段复算同口径键并与键列/现算集做 == 判等。
**数据源**：`get_entity_A.jsonl` 的 `formula`（匿名式）+ `get_property_A.jsonl` 的 `(entity_id, 'tc')`（evaluate.py 已有 `_a_property_table` L941-982 可复用，per-case 作用域口径现成）。

| 组件 | 签名建议 | 落位 |
|---|---|---|
| 降级键现算 | 直接 import `degraded_material_identity_key(formula_anon, tc)`（勿自写拼装，防口径漂移——主键侧已有"接口 B 直调对照"测试先例） | gateway.py `_load_material_identity_fns` 接缝（L615-640）扩返第四个函数 |
| universe 扩展 | `_material_key_universe()` 返结构升为 `{key: {'entity_ids': set, 'key_type': 'primary'/'degraded'}}`（或并列两个字典，避免破坏现有 == 消费方——**优先并列新函数 `_material_key_universe_full()`，旧函数保留原返回值**） | gateway.py |
| 条件②接通 | evaluate.py L844-855：anchor ∈ primary 键集 → 现状不变；anchor ∈ degraded 键集 → `anchor_in_A=True` 且 `details['anchor_key_type']='degraded'` + `details['key_collisions']=n`（n=该键实体数） | evaluate.py |
| 唯一性拦截（监工边界） | 新增显式信号：凡判定语义需要"该键唯一对应一个实体"而命中键为 **degraded 且碰撞 n≥2** → 返错误信封 `DEGRADED_KEY_NOT_UNIQUE`（judge 侧判据返 `{'determinable': False, 'reason': ...}`），**不得静默按相等处理**；排除语义（判"这个不是 gt"：键不等即排除）不受限 | evaluate.py + 质量门 details |
| E5 开工信号 | 实现并实证 11,481 条降级键可判等后，在 report.md 写明解除 §7.1.2 硬前置 | report.md |

### 2.3 测试建议

1. 全量等价：11,481 条降级行用 formula+tc 复算 == 键列值（S6 口径 E3 侧独立复核，动态取数不钉常量——与规格④同源）；
2. 条件②：降级键 anchor 正例（判 in_A 且 key_type 标注）/伪键负例（改 tc 档或 _ALT 段）；
3. 唯一性拦截：构造降级键碰撞（键列实有 82 组可直取）→ 唯一性判定路径必须返不可判定而非相等；
4. 排除语义不受拦截影响：键不等 → 正常排除；
5. 回归：primary 侧 4 用例（test_material_identity_grounding.py）行为零变化。

### 2.4 不动区

primary 主键全链（`material_key_from_record`、`_SG` 拼装、SG 符号缓存）、31 组碰撞的"同键判等"既有口径、`_load_material_identity_fns` 的缺依赖回落行为（返 None 不崩，仅补降级函数同模式）、分子侧 canonical 通道。

---

## 3. 规格③：§7 R1-② resolve 组成↔实体 ID 映射（E6 open 污染判定堵点）

### 3.1 依据与消费点（实查）

| 项 | 实测 |
|---|---|
| ADR 原文（git show 59a35ac） | R1-②：映射归 E3 判分侧、**不放 gateway**（gateway 不该持有 G_B，持有即泄漏风险）；判定式 `hit_gb = 𝟙[resolve(c_submit) ∩ G_B ≠ ∅]`；映射**必须一对多**（§8 同分异构全收） |
| E6 消费点 | `src/eval_matrix/bare_baseline.py` `judge_open_case`（L286-318）：`from src.benchmark.evaluate import hit_gb`（L294），逐样本 `hit_fn(content, case_id)`（L303）；现状注 L289-292 自述缺口①②未注册/无映射，判定标"待口径裁定"（L312） |
| 现状 hit_gb 为何字面不可执行 | evaluate.py L519-554：`normalize_b_key(answer) in gb_set`——answer 是组成串（如 `[16*]20[19*]22[22*]1[81*]2`）、G_B 成员是实体 ID（如 `z064386`），两侧字符串空间不相交 ⇒ 恒不命中 |
| G_B 成员表示 | 15 道 dh case JSON 的 `g_b` 全是实体 ID 列表（实测：case_dh_drug_09 有 3 个成员=同分异构全收形态，其余各 1 个）；**未注册进 b_partition.cases**（R1-① 归 E5，在途） |
| 组成数据源 | `get_entity_A.jsonl` 92,193 行全库：G_B 成员（z/c 实体）组成全可得——晶体取 `formula` 字段，分子取 dummy SMILES 同位素计数或 formula 回落（task-05 已冻结的 `composition_of` 形态③口径，evaluate.py L653-696）；`get_entity_B.jsonl` **0 行**（B 是分区概念非物理文件，resolve 建在 A 侧全库身份档上） |
| 采样产物 | E6 samples 目录现 269 份 json 全为 cs/co 类；**dh open 的 1350 次（15 道×3 模型×30）尚未落盘**——等 R1-①注册 + resolve 双就绪后出结论 |

### 3.2 签名与落位

```
resolve(composition, gw=None, backend_root=None) -> set[entity_id]
```
- 落位：evaluate.py 判分侧（组成纯函数组同区，L557 区块之后）；
- 语义：`composition_of(composition, gw)` 规范化（形态①匿名式/②材料键前段）→ 与全库实体组成做 `composition_equal(ε=0.02)` 比对 → 收集**全部**命中实体 ID（一对多，同分异构全收，与 ADR §8 一致）；
- 性能规格：全库 92,193 行不得逐样本现扫——建倒排索引 `{frozen_composition: set(eid)}` 模块级懒缓存（仿 `_MATERIAL_KEY_CACHE` 以 backend 根路径为键，gateway.py L610/L1296 先例）；分子域剥 H 口径与条件③一致（调用侧处置）；
- hit_gb 改造：`hit_gb(answer, case_id)` 内部改为 `resolve(answer) ∩ g_b_ids ≠ ∅`；G_B 成员为实体 ID 时直接交集；**fixture 占位口径（顶层 g_b 的 SMILES 键）保留既有 normalize_b_key 双轨**，placeholder 标记语义（L527-530、L541-552）不动；
- case 未注册 G_B → **报错不回落顶层**（承 R1-①；task-05 文件 §7 L158 同款要求）——bare_baseline L296-301 的"缺口①"标注逻辑随之转为真实判据。

### 3.3 测试建议

1. 一对多：构造/取用同组成多实体（dh_09 的 3 成员是真实样本）→ resolve 全收；
2. ε 边界：组成差 ≤0.02 命中、>0.02 不命中（与条件③同容差，复用既有边界用例模式）；
3. hit_gb 判定式：resolve∩G_B 非空→hit；G_B 未注册→报错（不回落顶层 fixture g_b）；
4. 双轨：占位 SMILES G_B（case_fixture_isolation_v1/v2）行为零回归；
5. 与 E6 联调口径：judge_open_case 的 notes 中"缺口②"标注在 resolve 落地后消失的判据写进测试断言。

### 3.4 不动区

`composition_of`/`apply_composition_edit`/`composition_equal`/`parse_edit_detail` 签名与行为（#54 已冻结）、grounding 三条件判定逻辑、fixed 判分全链、`BPartitionJsonReader`/`BPartitionBackendJsonlReader`。

---

## 4. 规格④：test_material_key_recompute 断言改动态取（归属：E3 自修）

### 4.1 硬编码盘点（实查 + 实跑复现）

测试文件：`dag_poc/mcp_mock_server/tests/test_material_identity_grounding.py`（192 行，E3 在 d3b7d4a 所写——监工裁定归属 E3，非待 E1）。

| 位置 | 内容 | 性质 |
|---|---|---|
| L63 | `assert len(key_column) == 5771` | **断言硬编码①**（键列规模） |
| L74 | `assert len(universe) == 5740` | **断言硬编码②**（唯一键数） |
| L79 | `assert len(dup_groups) == 31 and set(dup_groups.values()) == {2}` | **断言硬编码③**（碰撞组数 31；`=={2}` 是"全恰 2 条/组"形状断言） |
| L7-8、L58、L180 | docstring 与 log 模板里的 5,771/5,740/31 字样 | 文案（建议同步改动态注记，非断言） |

**失败实跑复现**（2026-08-07）：`assert 17252 == 5771`——键列已被 E1-05 扩至 17,252 行（primary 5,771 + degraded 11,481）。全量 collect 实测 199 tests（无 mcp 环境口径），该用例即 problems.md PR-37 登记的唯一失败。

### 4.2 动态化规格（语义不得弱化）

测试语义 = **接口 B 现算键（material_key_from_record）与键列的一致性复算**。动态化只换"库规模魔法数"，复算断言全保留：

1. `_load_key_column()` 按 `key_type` 分流：返回 `{id: (key, key_type)}` 或两份字典（primary/degraded）；
2. L63 改：`primary 子集规模 == 键列内 key_type=='primary' 的行数`（动态）；复算逐一 == 比对断言（L65-71）**原样保留**——这是核心语义，只限 primary 子集（degraded 行无 SPACEGROUP 段，现算返 None，不在本用例复算面）；
3. L74 改：`len(universe) == primary 子集经现算去重后的规模`（动态），等价于"无一条现算失败"的守恒式；
4. L79 改：碰撞组数动态取（键列 groupby），**形状断言 `values()=={2}` 保留与否列为裁定项 F4**（它是"上游重复录入恰 2 条/组"的数据事实，数据再扩会再挂——建议改"每组≥2 且组数动态"+ 注释说明 3DSC 事实）；
5. **建议顺带扩面**（与规格②天然配对，可选）：新增 degraded 子集复算用例——`degraded_material_identity_key(formula, tc)` 现算 == 键列 degraded 行，动态取数；这同时是规格②的全量等价测试（两处合并一次写）；
6. docstring/log 模板文案同步改"动态口径 + 落盘时实数"（log 里写实数是对的，它本来就是实跑留痕）。

### 4.3 归属登记订正（任务书点名）

- problems.md PR-37（L61）现文"待 E1 侧随扩量数据同步更新测试常量；E3 保持对照口径不触碰"→ **订正为"E3 自修：断言改数据源动态取（监工裁定，report.md 验收节）"**；
- report.md §11.3 与 §11.7 第 5 项同款"待 E1 侧"表述一并订正；
- 订正动作归实施任务（#61）收口时做，调研只登记。

### 4.4 不动区

该文件其余三组用例（真实样本/条件②接通/接缝容错，L83-192）零触碰；rehearsal log 写盘机制不动。

---

## 5. 实施顺序与并行性（文件交叠面分析）

| 项 | 触碰文件 | 依赖 |
|---|---|---|
| ①§10（#58） | gateway.py（__init__ + 五工具 + per_case_view import）；新测试文件 | 机制部分无依赖；生产验收等 E5 视图文件 + E4 合跑 |
| ②§9（#59） | gateway.py（_material_key_universe 区块 L1278-1307 + import 接缝 L615-640）+ evaluate.py（条件② L844 / 质量门 L1086） | 无代码前置；E1 纯函数已就绪 |
| ③§7（#60） | evaluate.py（hit_gb L519-554 + 新增 resolve + 倒排缓存） | 语义上承 R1-①（E5 注册，在途）——**代码可先做**，生效待注册 |
| ④（#61） | 仅 test_material_identity_grounding.py + problems/report 文案 | 无 |

**交叠面**：①与②同改 gateway.py 但区段不相交（①在 __init__/工具层，②在材料键 universe 层）；②与③同改 evaluate.py 但区段不相交（②在 L844/L1086，③在 L519-554+新函数区）。**结论：四项可两两并行——（①+③）一批、（②+④）一批，或四人全并行（评审时按区段核对）**。唯一串行约束：②与④若都写 degraded 复算断言应合并一次写（§4.2 第 5 条），避免重复用例。

**建议顺序**（监工优先级 + 解锁速度）：④ 最先（最小改动、立即让全量回归归零，为后续三项提供干净基线）→ ①（最高优先，机制验收不等 E5）→ ②（解 E5 堵点后即发开工信号）→ ③（等 R1-① 注册到位后联调 E6）。

## 6. 风险与裁定项

| # | 项 | 类型 |
|---|---|---|
| F1 | §10 open/fixed 区分机制：Gateway 加 `case_kind` 参数（主案）vs 以 b_partition.cases 注册推断（备案，与 R1-① 时序耦合不推荐） | 需裁定 |
| F2 | `X-Dagpoc-Case-Id` 头通道当前无人消费（server 只读 env）：本次按 env 通道实现（一 case 一 server），头通道二期——是否认可 | 需裁定/知会 E4 |
| F3 | §9 唯一性拦截的"判定依赖唯一性"由谁声明：case JSON 加字段 vs 消费方（E5 造题脚本）调用时显式传入 | 需裁定 |
| F4 | ④ 碰撞组形状断言 `=={2}` 保留还是降为注释（上游再扩会再挂） | 需裁定 |
| R1 | §10 生产验收被 E5 视图文件阻塞（15 道 view_ref 全空转）——机制验收先行，验收报告注明"生产联调待 E5" | 风险 |
| R2 | ③ 的 hit_gb 改造与 15 实验点基线：fixed 类不走 open 判分，理论零影响，但须跑 15 点回归逐字节核对（承 task-04 惯例） | 风险 |
| R3 | ②的 tc 数据源：降级键复算需 per-entity tc，须确认 get_property_A.jsonl 对 11,481 条 UCI 晶体的 tc 覆盖完整（E1 S6 声称已全量实测，实施时再跑一遍守恒式） | 风险 |
| R4 | E6 在途改动（failures.jsonl/samples 持续落盘、query_lint.py 有 M）：联调 ③ 时以 E6 最新提交为准，不触碰其在途文件 | 纪律 |

**git 纪律**（承前轮）：E1/E4/E5/E6 在途未提交改动只观察不触碰；暂存用精确路径，勿 `git add -A`（当前工作区有 agent_harness.py(E4)、query_lint.py(E2)、E6 samples 等在途项）。
