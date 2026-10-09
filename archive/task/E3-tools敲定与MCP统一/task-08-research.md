# task-08 调研：四条阻塞项（PR-61/62/63/65）现状与实施规格

- 调研代理：Alex（只读，未改任何代码/数据；本文档为唯一落盘产出）
- 调研时点 HEAD：`d54cca2`（master；注：task-07 收口时为 72f68d6，其后仅对比文档改版提交，本线四文件行号均已按现行工作区重核）
- 行号基准：`dag_poc/gateway.py` 2,345 行、`src/benchmark/evaluate.py` 1,941 行、`dag_poc/tools_schema.json`、`dag_poc/mcp_mock_server/mock_server.py` 454 行、`gateway_mcp.py` 215 行
- 上轮回归基线（任务书给定）：361 passed / 1 skipped / 2 xfailed；本次实测收集数 368（dag_poc/mcp_mock_server/tests 283 + src/tests 6 + src/eval_matrix 79），差 4 项口径差见 §5.5
- 本轮为纯本地调研，无 LLM 调用（任务书 §7 口径）

## 0. 结论速览（四条项各一句话改法）

| 项 | 一句话改法 |
|---|---|
| PR-61 | `_queryable_props()`（gateway L2047）改为剔除"投影值全为 None"的性质键，mw/logp/tpsa 即出白名单，过滤请求自然落 UNKNOWN_PROPERTY（L1414 拦），同步改写 tools_schema.json 9 处 + smoke_examples.json 2 处 + 3 段 docstring，并新增"每项宣称可过滤性质存在非空阈值"的机器断言（动态取数、带注入负例自证） |
| PR-62 | `score_case` 路由（evaluate L302/L312）与 `__main__` aggregate 口径（L1901）改用显式字段 `template_id == 'discovery_haystack_v1'` 判 discovery 类（case_kind 字段 150 道全不存在，不可用），30 道 dh 带 gt 也进 `_score_open_discovery`；其余两类 120 道与旧 open 4 道分流零改动 |
| PR-63 | mock_server.py L452 与 gateway_mcp.py L213 的 `streamable_http_app()` 产物外包一层 ASGI 中间件，给 JSON 响应 Content-Type 追加 `; charset=utf-8`，H1 requests 客户端不再回退 ISO-8859-1 |
| PR-65 | gateway L1393 后新增 keywords 类型校验：非 str（含数组）与字符串化数组形态（`'['` 开头）一律 INVALID_ARGUMENT 带正确形态示例，不做任何宽容解析；tools_schema L119/L140 同步补负例声明 |

---

## 1. PR-61（最高）：mw/logp/tpsa 宣称可过滤但永空

### 1.1 现状与根因链（全部实跑核实）

1. **filter_only 动态计算落点**（任务书指 L1422/L1584，核实吻合）：
   - search_candidates 侧：gateway **L1420-1428**（`vprops = self._value_queryable_props()` L1420、`hidden = set(_EXTERNALLY_HIDDEN_PROPS)` L1421、`filter_only = sorted(qprops - vprops - hidden)` **L1422**、UNKNOWN_PROPERTY 信封带清单 L1423-1428）
   - get_property 侧：gateway **L1583-1591**（同款结构，filter_only 在 **L1584-1585**）
2. **白名单定义**：
   - `_queryable_props()` **L2047-2055**：`return {p for _, props, _ in self._a_universe().values() for p in props}`——A 全集投影性质**键**并集，实测 **16 项**（真值表 12 项 atom_count/bbb/caco2/condition/element_count/family/half_life/herg/logD/logS/structure_in_db/tc + filter-only 三项 logp/mw/tpsa + 隐藏项 sc_class；对外列示 15 项）
   - `_value_queryable_props()` **L2057-2068**：get_property_A 真值表性质并集，实测 **12 项**
   - `_EXTERNALLY_HIDDEN_PROPS = ('sc_class',)` **L121**（隐藏集机制：可过滤、不对外列示）
3. **根因（本调研坐实的最后一环）**：`_a_universe()` **L2002-2045** 中投影 props = `filter_props`（dict 直取，L2024-2025）∪ 真值表行（L2026）。实跑统计 search_candidates_A 全部 92,153 条的 filter_props：
   - `mw`/`logp`/`tpsa`：各 **74,901 个键，值全为 None**（None 计数 = 键计数，100%）
   - `sc_class`：17,252 键，值均非 None（故 sc_class 过滤真实可用，874 条材料圈出先例成立）
   - 样例 z000001 投影 = `{'mw': None, 'logp': None, 'tpsa': None}` ∪ 真值表项
4. **"为何落 EMPTY_RESULT 而非 UNKNOWN_PROPERTY"精确代码路径**：
   - 三项在投影中**有键** → 进 `_queryable_props()` 白名单 → 白名单校验 **L1412-1414** 放行（UNKNOWN_PROPERTY 只对白名单外性质触发，三项在名单内，拦不到）
   - 过滤循环 **L1456-1460**：`sat = {i for ... if all(_cmp(props.get(c['property']), ...)}`
   - `_cmp()` **L323-335**：`if val is None: return False`（**L325-326**，docstring"缺值视为不满足"）→ 值全 None ⇒ 任何 op/阈值恒 False ⇒ sat 恒空
   - `hit_ids` 空 → **L1461-1463** 返 EMPTY_RESULT（"禁止全库 fallback"），agent 被误导反复放宽阈值重试（H1 case_cs_drug_15 实测白烧 14 次、打满 50 次上限）
5. **零数据复核（抽查验证监工结论成立）**：get_property_A.jsonl 全量计数 mw=0、logp=0、tpsa=0 行（真值表 12 性质行数：logS 2,272 / bbb 730 / caco2 220 / logD 582 / half_life 503 / herg 98 / tc·family·structure_in_db·condition 各 17,252 / atom_count 74,901 / element_count 92,153）
6. **复现实跑**：`constraints=[{property:'mw', op:'ge', threshold:0}]`（恒真阈值）→ EMPTY_RESULT；对照 `logS ge -6` → returned 50 / total_found 2,173（与监工对照数字吻合）

### 1.2 方案 A 实施规格（Leader 裁定：彻底移出，三处同步）

**动作 A1 —— 白名单收窄（gateway.py，1 处函数体改动）**
- 改 `_queryable_props()` L2047-2055：在并集基础上**剔除投影值全为 None 的性质键**。推荐实现口径：对 `_a_universe()` 每项 props 只收"至少一个实体该键值非 None"的键（即 `{p for _, props, _ in u.values() for p, v in props.items() if v is not None}`）。效果：mw/logp/tpsa 出白名单（16→13 项，含隐藏 sc_class）；**E1 未来补数据后三项自动回白名单、无需改代码**（此即"重新宣称可用"路径一句话备注，写进 docstring）。
- 不推荐硬编码集合减法（`qprops - {'mw','logp','tpsa'}`）：E1 补数据后仍需二次改码，违反自洽交付意图。
- 备选兜底：若实施代理认为动态 None 剔除影响面大，可退为显式常量减法，但必须在 docstring 登记"E1 补数据后须手工删常量"义务——二选一由实施代理定，验收口径相同。
- 连带 docstring 改写 3 处：`_a_universe` F2 注记 **L2012-2014**、`_queryable_props` 同步义务 **L2050-2054**（16 项→13 项、删 filter-only 叙述）、`_entity_props` F2 注记 **L2148-2150**。

**动作 A2 —— 错误信封 filter_only 叙述消失（2 处）**
- search_candidates **L1415-1428**：改后 `filter_only` 恒为空集，"其中 filter-only [...] 只能入 constraints 过滤"叙述必须删段（不得留"filter-only []"空列表文案）；可用清单 = `sorted(qprops - hidden)` 自然变 12 项。
- get_property **L1578-1591**：同款删段；`{filter_only} 为 filter-only` 句删，可查值清单叙述不变（仍 12 项）。
- 错误码语义改变**零额外代码**：mw/logp/tpsa 出白名单后，constraints 用它们 → L1414 命中 → UNKNOWN_PROPERTY（带可用清单信封），即任务书动作 2 要求的语义归并。

**动作 A3 —— tools_schema.json 说明书/示例同步（9 处，grep 全量核实无遗漏）**
| 行 | 位置 | 改法 |
|---|---|---|
| L6 | meta.status | 追加 v0.3.2 变更记录（mw/logp/tpsa 移出对外宣称） |
| L11 | meta.property_manifest_sync | 16 项叙述改 13 项（对外 12），删 filter-only 三项列举，同步义务文本保留 |
| L119 | search_candidates description | 删"另有 filter-only 三项 logp、mw、tpsa——只能入本工具 constraints 过滤"整句；对外清单改 12 项 |
| L186 | search_candidates behavior_notes ⑧ | F2 口径段改写为"三项曾宣称 filter-only，实为全 None 死面，task-08 移出（PR-61）" |
| L189 | search_candidates examples 错误信封示例 | 信封文案按改后实跑重写（可用清单 12 项、无 filter-only 句） |
| L247 | get_property description | 删"另有 filter-only 三项 logp、mw、tpsa……一律被拒"句 |
| L294 | get_property behavior_notes | F2 登记段同款改写 |
| L297 | get_property examples 错误信封示例 | 信封文案按改后实跑重写 |
| L302 | rank_by_property description | 删"filter-only 三项 logp/mw/tpsa 无查证值、不可作排序键"句（三项已不存在于性质面） |
- 参数白名单侧：constraints 校验白名单即 `_queryable_props()`（L1412-1414），A1 生效后自动收窄，**无独立参数白名单落点**（任务书"constraints 参数白名单"与白名单本体是同一物，登记备查）。
- tools_schema **L140** keywords/input schema 不受本项影响。

**动作 A4 —— smoke_examples.json 与消费面（2 处）**
- `dag_poc/mcp_mock_server/tests/smoke_examples.json` **L73**（search_candidates UNKNOWN_PROPERTY 示例信封，含"filter-only ['logp','mw','tpsa']"文案）与 **L137**（get_property 同款）：按改后实跑文案重写。
- 消费方仅 `tests/_tmp_smoke_examples.py`（下划线前缀，pytest 不收集，不在 361 基线内）——改写后手工跑一次留痕即可。

**动作 A5 —— 机器断言（任务书动作 3，"断言本身要被验证过才算交付"）**
- 新测试文件落点：`dag_poc/mcp_mock_server/tests/test_queryable_props_viability_task08.py`（命名风格仿 test_call_limit_task07.py）
- 正例断言（每项宣称可过滤/可查的性质须"至少存在一个阈值/取值使其返非空"）：
  - 清单动态取 `gw._queryable_props() - set(_EXTERNALLY_HIDDEN_PROPS)`（**不许硬编码性质名**，承项目"库规模类断言禁魔法数"规范）
  - 阈值动态构造：遍历 `_a_universe()` 投影收集每项的**非 None 实测值集合**；数值型取实测最小值作 `ge` 阈值（必然命中至少该实体），字符串/枚举型取任一实测值作 `eq` 阈值；对每项断言 `search_candidates` 返非 error 且 `total_found >= 1`
  - 可查值侧同款：对 `_value_queryable_props()` 每项动态取一实体一值，断言 `get_property` 返该值非空
- 负例自证（可证伪性）：构造/注入一个"宣称可过滤但值全 None"的性质（monkeypatch `_a_universe` 投影注入假键或复用改前形态的辅助函数），断言 viability 检查函数对其**必须报错**（白名单必须不含它）——把断言逻辑抽成可测纯函数 `assert_viable_filter_props(universe)` 以便负例直接打它
- 隐藏集口径：sc_class 不在对外宣称面，viability 断言对其豁免（可过滤能力不动，仍实测一次 `eq` 命中留痕）

**E1 补数据重新宣称路径（一句话备注，写进 _queryable_props docstring）**：E1 向 filter_props/get_property_A 写入 mw/logp/tpsa 真实值后，动态 None 剔除自动恢复三项入白名单；届时按 meta.property_manifest_sync 义务改写 tools_schema 清单并重跑 trace_replay_task07.py manifest_check 即完成重新宣称。

---

## 2. PR-62（最高）：30 道 dh 带 gt 被 is_open 判据劫进 fixed 分支

### 2.1 现状核实（行号与字段实况）

1. **evaluate.py L302**（任务书指认，核实吻合）：`is_open = 'gt' not in case`，位于 `score_case`（**L294**）；分派在 **L312-324**：is_open=True 且 `case_id in discovery_case_ids()`（L317）→ `_score_open_discovery`（L320），否则 `_score_open`（L322）；is_open=False → `_score_fixed`（L324）。30 道 dh 全带 gt ⇒ is_open=False ⇒ 全部劫进 `_score_fixed`——监工结论坐实。
2. **本调研补发现（任务书未提，必须同改）**：`__main__` 聚合处 **L1901** `aggregate(scores, events=events, is_open='gt' not in case_obj, ...)` 是第二处 gt 推断判据——只改 L302 不改 L1901 会造成判分路由与聚合口径（no_path_violation open 判据 L1737/L1752、task_score 聚合 L1823/L1847）不一致。
3. **30 道 dh case 文件实况**（`dag_poc/templates/discovery_haystack/case_dh_*.json`，逐文件扫描）：
   - `gt` 键：**30/30 全在**
   - `template_id`：**30/30 = 'discovery_haystack_v1'**
   - `case_kind`：**30/30 缺失**；全库 150 道 case（三目录）case_kind 存在数 = **0/150**
   - 其他键（均 30/30）：case_id/lineage/target_domain/gradient/delta/evidence_k/s_feasible_size/direction/target_window/visible_set/view_ref/g_b/isomer_handling/anchor/edit_op/evidence_set/quality_gates/domain_knowledge/query/provenance（grid_key 15/30）
4. **全集 template_id 分布**（150 道现役）：constrained_optimization_v1 = 60、constraint_satisfaction_v1 = 60、discovery_haystack_v1 = 30；归档区 `templates/_deprecated/open_discovery/` 另有 4 道 open_discovery_v1（无 gt，供 test_legacy_open_path_unchanged 使用）。
5. **discovery_case_ids()**（evaluate **L526-544**）：读 `dag_poc/mcp_mock_server/b_partition.json`（注意实物路径在 mcp_mock_server/ 下，不在 dag_poc/data/）的 cases 键集 = **38 键**：30 道 case_dh_*（与文件 1:1，无缺无多）+ 2 道 case_fixture_isolation_v1/v2 + 6 道 spare_dh_*（备用键，无对应 case 文件，不经 score_case 文件路由，不影响 PR-62）。注册正确、上层劫走——监工结论坐实。
6. **_score_open_discovery 全貌**（evaluate **L1580-1610**，31 行）：open_score = 1[hit(G_B)] × 1[grounding_pass] × 1[非存在性预算耗尽]（L1591-1599），cp1-cp6 恒 0.0 + `_status` 恒 not_attempted（L1600-1604），extra 带 hit_gb/grounding/open_gate/open_score/grounding_mode/existence_*（L1605-1609）。调用面仅两处：score_case L320、open_case_metric **L1613-1632**（L1629）。

### 2.2 实施规格

**动作 B1 —— 显式类型字段路由（用 template_id，不用 case_kind）**
- 字段实况支撑：case_kind 全库 0/150 不存在；template_id 全库 150/150 在场且三值互斥（+归档 open_discovery_v1）。任务书二选一由字段实况裁定为 template_id。
- 改 `score_case`（L302/L312-324）：
  ```
  is_discovery = case.get('template_id') == 'discovery_haystack_v1'
  ```
  分派顺序：`is_discovery` → `_score_open_discovery`（**不看 gt 有无**，30 道带 gt 的 dh 由此进 open 分支）；否则维持 `is_open = 'gt' not in case` → `_score_open` / `_score_fixed` 原样。
- 常量落点建议：判分器顶部定义 `DISCOVERY_TEMPLATE_ID = 'discovery_haystack_v1'`（CP_WEIGHTS L1639 附近），不裸字符串散落。
- **改 L1901**：`is_open=(case_obj.get('template_id') != DISCOVERY_TEMPLATE_ID and 'gt' not in case_obj)`（或直接复用同一判据函数抽成 `case_is_discovery(case)` 纯函数，L302/L1901 共用）。

**动作 B2 —— 机器断言（30 道 dh 全路由 open 分支，防回归形态）**
- 落点：扩展 `dag_poc/mcp_mock_server/tests/test_evaluate_open_discovery.py` 或新文件 `test_dh_routing_task08.py`（同目录）。
- 样式参照 `test_resolve_composition_task05.py`（动态取数、不钉常量、防回归）：
  - 动态 glob `dag_poc/templates/discovery_haystack/case_dh_*.json`，断言文件数 >= 1（防目录搬空静默绿）且逐一断言 `case_is_discovery(json.load(f)) is True`
  - 路由行为断言：对每道 dh（或抽样+数量断言组合），monkeypatch/spy `_score_open_discovery` 与 `_score_fixed`，调 `score_case`（trace/答卷可用合成最小 fixture，仿 test_evaluate_open_discovery `_golden_events`/合成 case 模式），断言命中 open 分支且 fixed 零调用
  - extra 形状断言：scores 含 `_status`、cp1-cp6 == 0.0（open 类定义），open_gate/open_score 键在场
- **fixed/open 全集分流核对方案（防误伤）**：
  - 动态枚举三目录 150 道，按 template_id 建期望分流表：dh 30 → open_discovery 分支；co 60 + cs 60（全带 gt）→ `_score_fixed`；断言每道 `case_is_discovery` 判定与期望一致（纯字段断言，不需要跑 trace，快且全覆盖）
  - 旧 open 4 道（_deprecated/open_discovery_v1、无 gt）：断言仍走 `_score_open`——由既有 `test_legacy_open_path_unchanged`（L206-221，逐字段基线锚点）天然守住，零新增

**动作 B3 —— 既有测试同步项（1 处 fixture 必改）**
- `test_evaluate_open_discovery.py` **L260-262**：合成 discovery fixture `{'template_id': 'open_discovery', ...}` 在 B1 后将不再路由进 `_score_open_discovery`（它靠 b_partition 键集+无 gt 走旧 is_open 路径进 `_score_open`，L290-291 断言 open_gate 字段会当场失败）→ fixture template_id 必须改为 `'discovery_haystack_v1'`。
- `test_process_metrics_status_gate.py` L204 直调 `_score_open_discovery`（不经路由）→ 零影响。
- `test_legacy_open_path_unchanged` / `test_gt_missing_no_keyerror` → 零影响（open_discovery_v1 不含 gt，维持旧路径）。

---

## 3. PR-63：MCP HTTP 响应缺 charset=utf-8 致 H1 中文错误乱码

### 3.1 现状与链路核实

1. **两条服务链路**：
   - `mock_server.py` **L452-453**：`app = mcp.streamable_http_app(); uvicorn.run(app, ...)`（端口 9000，契约层 9 工具 = Gateway 薄包装 L130/L100-101，启动断言 L435-443）
   - `gateway_mcp.py` **L213-214**：`app = MCP_APP.streamable_http_app(); uvicorn.run(app, ...)`（端口 9001，独立新契约 server）
   - 两处均未声明 charset（全文件无 Content-Type/charset 落点，grep 核实）
2. **H1/H2 走向（实物口径，与任务书设问不同，见 §6 疑点 3）**：E6 实跑 mcp_config（`task/E6-三层矩阵评测/*/_server/mcp_config_p9*.json`）指向 `http://localhost:<port>/mcp`，H1 与 H2 均连 **mock_server.py 一条链路**（:9000/:9100 等）；gateway_mcp.py（:9001）为独立调试/备用运行态。乱码差异在客户端：H1 agent_harness 用 requests，响应头无 charset → `r.text` 回退 ISO-8859-1 → 乱码；H2 商用 CLI 的 MCP 客户端按 JSON 规范 UTF-8 解码 → 不受影响。
3. **乱码实证（H1 spotcheck trace 实跑扫描）**：3 条 trace 错误行 26/7/19 条**全部乱码**（如 `'æ\x97\xa0å\x91½ä¸\xad...'`），`err.encode('latin-1').decode('utf-8')` 全部还原为可读中文（"无命中文献，不得编造论文"等）——监工现象 100% 复现。

### 3.2 实施规格

- **改法**：mock_server.py **L452** 与 gateway_mcp.py **L213** 的 `streamable_http_app()` 返回值外包 ASGI 中间件（Starlette 风格即可，无新依赖）：响应头 `content-type` 以 `application/json` 开头且不含 `charset` 时改写为 `application/json; charset=utf-8`（SSE `text/event-stream` 响应同款追加）。建议把中间件写成两文件共用的小函数（放 gateway_mcp.py 导出、mock_server 复用，避免双实现漂移——两文件已有此复用先例 L435-438）。
- **不动**：工具实现/错误信封/判分面零改动；PR-64（harness 客户端侧编码加固）归 E4，本线不做（红线）。
- **验证手段（三层）**：
  1. 单测（新文件 `dag_poc/mcp_mock_server/tests/test_charset_task08.py`）：用 httpx/TestClient 直打 ASGI app（不起真端口），POST /mcp initialize + tools/call 触发一次错误信封，断言 `'; charset=utf-8' in response.headers['content-type']` 且 `response.text` 含目标中文（如"检索无命中"）不含 `'æ'`/`'Ã'` 乱码特征字符
  2. 端到端：起 mock_server 后 requests 实打，`r.text` 直读中文（复现 §3.1 第 3 条的扫描脚本反向断言：乱码特征字符数 = 0）
  3. 留痕对照：对一段既有乱码 trace 文本执行 latin-1→utf-8 还原，与修复后直读文本逐字一致

---

## 4. PR-65：keywords 字符串化数组被静默放行

### 4.1 现状（实跑核实）

1. **收 keywords 的工具**：9 工具中仅 `search_candidates`（参数白名单 `{'constraints','keywords','k','offset'}`，gateway **L1390**）。
2. **现行校验 = 无**：L1392-1397 只查 constraints 类型与"两者至少给一"；keywords **无任何类型校验**，L1453-1455 `kw = str(keywords).lower()` 强制字符串化后对实体 ID 做子串匹配——任何形态都被吞掉。
3. **实跑三形态**：
   - `keywords='["solubility"]'`（字符串化数组）→ 放行 → 无 ID 匹配 → **EMPTY_RESULT**（静默误导：agent 以为查询合法但库无货）
   - `keywords=['solubility']`（真数组）→ 同款 str() 吞掉 → EMPTY_RESULT
   - `keywords=123`（int）→ str() 吞掉 → 竟命中 50 条（ID 含 '123' 子串）
4. **schema 侧**：tools_schema **L140** 已声明 `"keywords": {"type": ["string","null"]}`，**L119** description 写"keywords 为可选字符串"——声明在场、运行态不执行，断裂点就在 gateway 校验缺失。
5. **matrix trace 实况**：341 次 search_candidates 调用中 keywords 参数 124 次、全为 str（尚无字符串化数组实害记录，本项是监工抽查发现的风险面堵口）。

### 4.2 实施规格

- **拒绝点**：gateway **L1393 后**（keywords 取出后、constraints 校验旁）新增两段：
  1. `keywords is not None and not isinstance(keywords, str)` → `_err('INVALID_ARGUMENT', 'keywords 须为字符串或 null（不接受数组/数值等其他类型）')`
  2. str 形态但 `keywords.lstrip().startswith('[')` → `_err('INVALID_ARGUMENT', 'keywords 收到了数组的字符串化写法（如 \'["solubility"]\'）：keywords 只接受单个字符串（如 "z002730"），不要传 JSON 数组或其字符串化形态')`
- **边界（任务书红线）**：不做宽容解析——不 json.loads 替 agent 还原数组、不取首元素、不同义词展开；只把话说清楚。
- **信封示例文案落点**：tools_schema search_candidates examples（**L188-189 区域**）追加一条 INVALID_ARGUMENT 负例（照抄实跑信封）；description **L119**"keywords 为可选字符串"补半句"（数组/字符串化数组/数值一律 INVALID_ARGUMENT）"。
- **回归确认**：既有测试 keywords 用法全为 str/None（test_budget L162/191/309、gwtest_common L88、test_evaluate_open_discovery fixture L78 等，grep 全量核实）→ 零破坏；`keywords=None` 必须继续放行（"至少给一"校验 L1396-1397 依赖 None falsy）。

---

## 5. 回归面盘点

### 5.1 基线口径
- 任务书基线 361 passed / 1 skipped / 2 xfailed（=364）；本调研 collect-only 实测 **368**（mcp_mock_server/tests 283 + src/tests 6 + eval_matrix 79）。差 4 项无法回溯到具体用例（疑似收口后新增/参数化口径微差），**建议实施代理以改动前当次全量实跑为新基线**，不与 364 硬对。

### 5.2 PR-61 影响面
| 对象 | 影响 | 处置 |
|---|---|---|
| `test_backend_source_switch_task06.py::test_machine_f2_filter_only_mw`（**L251-266**） | **必红**：断言三项"可过滤"（L259 `!= UNKNOWN_PROPERTY`） | 反向重写：断言三项过滤返 UNKNOWN_PROPERTY 且信封带可用清单；get_property 侧断言（L262-266）不变 |
| smoke_examples.json L73/L137 信封文案 | 文案漂移 | 实跑重写（§1.2 A4） |
| tools_schema.json 9 处 | 文案漂移 | §1.2 A3 |
| trace_replay_task07.py **manifest_check**（L184-230） | live_fo 变空集、exposed_all 变 12 项——逻辑天然兼容（filter-only 检查空过），但 description 必须列全 12 项否则报"缺性质" | 无需改脚本逻辑；实施后跑一遍留痕 |
| trace_replay_task07.py **aggregate**（418/54） | **不变**——只读落盘 trace 记录的 error_code | 无需动 |
| trace_replay_task07.py **replay** | **会变**：matrix trace 中 mw 约束调用 14 次（logp/tpsa 0 次，实扫全矩阵核实）将从 EMPTY_RESULT 翻 UNKNOWN_PROPERTY——重放 UNKNOWN_PROPERTY = 基线 +14 量级漂移、drift 表现 EMPTY_RESULT→UNKNOWN_PROPERTY 14 | 属**错误码语义改变的预期口径变化**，非回归；实施后在脚本 docstring"改后数字口径说明"段（L19-26 区域）补一句 PR-61 口径登记，重跑报数如实对账 |
| 隐藏集机制 `_EXTERNALLY_HIDDEN_PROPS` | **不扩展**——mw/logp/tpsa 是"移出白名单（不可过滤）"，与 sc_class 的"可过滤但不列示"语义不同，不得混用该机制（sc_class 先例仅限列示面） | 无改动 |

### 5.3 PR-62 影响面
| 对象 | 影响 | 处置 |
|---|---|---|
| test_evaluate_open_discovery.py L260-262 合成 fixture | **必红**（template_id='open_discovery' 不再路由 discovery） | fixture 改 'discovery_haystack_v1'（§2.2 B3） |
| test_legacy_open_path_unchanged / test_gt_missing_no_keyerror / test_discovery_case_ids_from_reader | 零影响（见 §2.2 B3 论证） | 无 |
| test_process_metrics_status_gate.py（L204 直调 _score_open_discovery） | 零影响（不经路由） | 无 |
| 120 道 co/cs fixed 判分 | 零影响（template_id ≠ dh 且带 gt → 仍 _score_fixed；判分公式不动） | B2 全集分流断言守门 |
| evaluate_no_path_violation/aggregate 的 is_open 参数链（L1737/1752/1823/1847） | L1901 同步后口径一致；dh 类 no_path_violation 将按 open 口径算——**这是判据纠正的预期行为变化**（此前 dh 被按 fixed 口径 cp 全错，正是 cp6 断言失败的根因之一） | 实施后对一道 dh 跑 score_case 留痕对照 |

### 5.4 PR-63 / PR-65 影响面
- PR-63：中间件只包启动 app，不动工具面；`test_mock_server_contract_tools.py`（FakeFastMCP 桩，不经 HTTP 层）零影响。
- PR-65：既有 keywords 用法全 str/None（§4.2），零破坏。

### 5.5 验收回归建议顺序
1. 改动前全量实跑一次记新基线（pytest dag_poc/mcp_mock_server/tests src/tests src/eval_matrix）
2. 四条实施 → 同范围全量复跑 + trace_replay 三模式 + 任务书 §9 四条独立实测项（mw/logp/tpsa 实调、dh 一道 score_case 走 open 分支、注入无数据性质断言报错、H1 错误文本中文直读）

---

## 6. 方案疑点清单（任务书与实物不符/未覆盖处，均不改方案方向）

1. **PR-62 路由字段**：任务书给 template_id/case_kind 二选一。实物：case_kind 全库 0/150 不存在，唯 template_id 可用（三值互斥、150/150 在场）——按任务书授权"给字段实况支撑"选定 template_id，非方向变更。
2. **evaluate.py 第二处 gt 判据**：任务书只指 L302；实物另有 **L1901**（__main__ aggregate is_open）同款推断，必须同改，否则判分/聚合口径分裂。
3. **PR-63 链路口径**：任务书设问"H1 走哪条、H2 走哪条"（mock_server vs gateway_mcp）。实物：H1/H2 都连 mock_server.py 一条链路（E6 mcp_config 实证），gateway_mcp.py 未被 E6 使用；乱码差异在客户端解码而非服务端链路。改法仍两处同步（mock_server 为主、gateway_mcp 防后续启用再踩）。
4. **PR-61 白名单数量**：任务书问"当前 16 项？"——核实 16 项（含隐藏 sc_class；对外列示 15 项），方案 A 后 13 项（对外 12 项）。
5. **b_partition 38 键构成**：任务书"38 个里含 30"——实物 38 = 30 case_dh_* + 2 fixture + **6 spare_dh_* 备用键（无 case 文件）**；spare 不经文件路由，PR-62 无碍，登记备查。
6. **基线收集数**：364（任务书）vs 368（本次 collect-only），差 4 项，口径差如实登记（§5.1），以实施前实跑为准。
7. **filter_props 全 None 的归属**：mw/logp/tpsa 值全 None 属 E1 数据面现状（F2 裁定登记在册），本线只收缩宣称面不塞数据——与方案 A 裁定一致，无疑义。

## 7. 交付物清单（供实施代理照做）

- gateway.py：L2047-2055（A1）+ L1415-1428/L1578-1591 文案删段（A2）+ L1393 后 keywords 校验（PR-65）+ docstring 3 处（A1 附带）
- tools_schema.json：9 处（A3 表）+ keywords 负例 2 处（PR-65）
- smoke_examples.json：L73/L137（A4）
- evaluate.py：L302/L312-324 路由 + L1901 aggregate（B1）+ DISCOVERY_TEMPLATE_ID 常量
- mock_server.py L452 / gateway_mcp.py L213：charset 中间件（PR-63）
- 新测试 3 个文件：test_queryable_props_viability_task08.py（PR-61 断言）、test_dh_routing_task08.py（PR-62 断言，或并入 test_evaluate_open_discovery.py）、test_charset_task08.py（PR-63）
- 既有测试改写 2 处：test_machine_f2_filter_only_mw 反向、test_evaluate_open_discovery L261 fixture
- trace_replay_task07.py：docstring 口径补记（replay +14 预期漂移），脚本逻辑零改动
