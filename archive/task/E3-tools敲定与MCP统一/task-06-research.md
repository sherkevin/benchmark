# task-06 调研报告（任务 #64）——PR-45：gateway 数据源切到 E1 backend 的可执行实施规格

> 调研人：Alex（只读调研，零业务代码改动）。日期：2026-08-08。
> 依据：task-06 任务书全文（task/E3-tools敲定与MCP统一/task-06-修数据源未切到backend.md）、
> problems.md PR-45、gateway.py（1,870 行，行号为当前 HEAD c570812 实测）、
> dag_poc/data/backend 实物实测、dag_poc/templates 全量 case 清点。

## 0. 核心结论（七条）

1. **PR-45 已实跑复现**：`Gateway()`（无视图）`get_entity('z000374')` 返 `UNKNOWN_ENTITY`；
   `Gateway(case_id='case_dh_drug_01', case_kind='open')` 下 z000374 **在白名单（29 成员）内仍返
   UNKNOWN_ENTITY**——视图闸已放行，堵点确在视图之下的数据源层，与监工实测一致。
2. **同病面比监工预估更大**：不只 get_entity——`get_property`（_entity_props L1689 纯 mock_db）、
   `search_candidates`（_a_universe L1616 纯 mock_db）、`literature_search`（mock_db.literature
   仅 14 条，backend literature_search_A 309 条未接）、`edit_molecule`（分支 2 锚点受理 L1382
   直查 mock_db；分支 3 条件②基准 _a_smiles_universe L1676 遍历 mock_db admet 键）四工具同病。
   compare/check_molecule_exists/normalize_structure/submit_answer 四工具不受影响（见 §1）。
3. **索引已存在，零新增 IO 即合规**：task-03 已把 get_entity_A.jsonl（92,193 条）全量载入
   `_A_IDENTITY_CACHE`（模块级懒缓存，`_load_a_identity` L579-587），`ent_records` 就是
   id→record 的内存 dict。存在性判定切源 = 复用该 dict，**无需线性扫、无需预构建索引文件、
   不违反"不得退回内存小表"约束**（判定依据仍是 E1 落盘全库，只是装载方式缓存化）。
4. **性能实测（冷载一次性）**：get_entity_A 92k 全载 0.19s；get_property_A 240,664 行全载
   0.27s；search_candidates_A 92k 全载 0.28s。按 `_MATERIAL_KEY_CACHE` 同款模块级懒缓存
   （键=backend 根路径）模式新增两张表即可，每进程一次性成本 <0.6s。
5. **gt 可查性核实（验收第 5 条口径与实物不符，如实登记）**：实物 176 道 case 文件、
   170 道带 gt.id，其中 **169 道 gt 已在 backend**（含 E5 全部 139 道），唯一例外是旧题
   `case_cs_supercon_material`（gt='xb_exp_1'，mock_db 旧晶体）。任务书"80 道/30 道可查"
   口径与实物不符——E5 实际已造 139 道（co/cs 各 drug 30+material 30 + dh 15+4，另有 spare 3），
   详见 §6.4，列为裁定项 F1。
6. **mock_db 的 40 个 admet 实体全部在 backend**（E1 建库时已并入，get_entity_A 前两行就是
   'aa'/'ab'）——E4 九组冒烟 trace 的药侧实体切源后仍可查；**但 17 个旧晶体（xa/xb…）不在
   backend**，旧 4 道超导 open 与旧材料题的晶体实体切源后转 UNKNOWN_ENTITY（任务书 §5
   风险 1 预期内，如实报）。mock_db.json 本体必须保留（Gateway 构造 L814 仍读它；
   ADR-0014 legacy 重判 + evaluate.py 判分构造依赖）。
7. **判分侧有一处必须同步切**：evaluate.py L1003 条件② `a_ids = judge.db.get('admet')…`
   仍是 mock_db 口径——gateway 切源后 z-id 锚点会被判"锚点不在 A"，open 题 grounding 条件②
   结构性误判。此项超出任务书 §2 的 gateway 范围，列为必改关联点 + 裁定项 F5。

---

## 1. 逐工具现状表（gateway.py，HEAD c570812，共 1,870 行）

| 工具 | 定义行 | 实体/数据来源现状 | 判定 | 切源改动点 |
|---|---|---|---|---|
| get_entity | L1130-1185 | **存在性判定**卡 mock_db：分子 `eid in self.db['admet']` L1140、晶体 `self.db['structures'].get(eid)` L1175；**结构字段已接 backend**（_a_identity 身份档 L1146-1154，formula/masked_structure/atom_count 透传）；缺档回落走 mock_db.molecule_search RDKit 派生 L1162-1174 | **混合**（PR-45 本体） | 存在性改查 `_a_identity` ent_records（id→record dict）；晶体分支取值同步改 backend 记录（get_entity_A 晶体行字段齐备：formula/masked_structure/atom_count/cif，实测 c00001 样例在场）；molecule_search 回落分支保留为 backend 缺档兼容 |
| get_property | L1187-1219 | `_entity_props(eid)` L1689-1701 **纯 mock_db**（admet 条目投影 L1692-1695 / crystals 经 _cif_key L1696-1700）。视图闸已在计费后生效 L1207-1208 | **同病** | _entity_props 改查新增的 get_property_A 真值表缓存 `{(entity_id, prop): value}`（240,664 行全 case_ref=None 全局行）；props 白名单=该实体在表中的 prop 集；表不在场回落 mock_db 既有口径（过渡兼容，与其余通道同款） |
| search_candidates | L1066-1128 | `_a_universe()` L1616-1634 **纯 mock_db**（admet L1622-1625 + crystals L1626-1633）；keywords 子串扫 id L1113；约束匹配 props 投影 L1114-1118 | **同病** | _a_universe 改由 search_candidates_A.jsonl 缓存构建：entity_type/structure_hint 直取记录字段；可查性质投影=filter_props（dict 直取，**不是字符串**，实测 92,193 行全为 dict）∪ 该实体 get_property_A 行 prop 集；视图裁剪点 L1103-1109 不动 |
| literature_search | L1027-1064 | **纯 mock_db**：`self.db['literature']` 仅 14 条 L1046；backend `literature_search_A.jsonl` 309 条（doc_id/title/year/citation_count/keywords）未接 | **同病** | 文献源切 literature_search_A 缓存；keywords 匹配逻辑不变；视图 doc 白名单过滤 L1054-1060 不动。注意：15 道 dh drug 视图 docs=0（切源后这些 case 文献检索恒 EMPTY_RESULT，符合"空即无文献"同码口径）；dh material 4 道 docs=5 |
| compare | L1223-1258 | 只消费 `self.observed`（唯一写入口 get_property L1218）| **间接受影响，无独立改动** | 随 get_property 切源自动生效；PROPERTY_NOT_VERIFIED/UNKNOWN_ENTITY 口径不动 |
| check_molecule_exists | L1320-1347 | 查 `self.b_set`（b_partition B 侧，normalize_b_key 口径 L1345）| **不受影响** | 无 |
| edit_molecule | L1349-1439 | **分支 2（实体 ID 锚点）受理判据卡 mock_db**：`anchor in self.db['admet'] or anchor in self.db['structures']` L1382——z-id/c-id 锚点恒落分支 3 按 SMILES 解析失败返 ANCHOR_NOT_IN_A；分支 3 条件②基准 `_a_smiles_universe()` L1676 遍历 mock_db admet 键 | **同病** | 分支 2 受理判据改查 ent_records；视图闸 L1390-1394 不动；_a_smiles_universe 改由 smiles_map（74,941 分子）构建——**注意 RDKit 规范化成本**：74,941 次 canonical_chemical 不可每次现算，必须模块级缓存（见 §3 风险） |
| normalize_structure | L1441-1462 | 纯 RDKit 规范化，不查库 | **不受影响** | 无 |
| submit_answer | L1260-1282 | 仅 answer_schema 校验 | **不受影响** | 无 |

### 1.1 构造期两处 A 全集断言（连带改动点）

- B∩A=∅ 断言 L859-868、G_B∩A=∅ 断言 L877-880：`a_ids` 从 mock_db admet+crystals 推导。
  切源后应改为 backend ent_records 键集（更严——mock A ⊂ backend A，现状检查范围偏窄，
  B 成员若撞 backend A 实体将漏检）。

### 1.2 mock_server.py legacy 通道（不改，登记）

mock_server.py 的 deprecated 层（`_legacy_literature_search` L192、C3 fallback 开关
L357-364）直读 mock_db，属 9 契约工具名单之外的保留层——切源范围不含它（任务 #30 登记口径）。
gateway_mcp.py 走 `Gateway(case_id=env)`（L130-133），切源后 MCP 运行态自动受益，无独立改动。

---

## 2. 数据源盘点

### 2.1 mock_db.json（1,254 行，冻结快照）

| 表 | 规模 | 字段样例 | 现有消费点 |
|---|---|---|---|
| admet | **40** | bbb/caco2/half_life_days/herg/logD/logS/pass（pass 为 gold meta） | get_entity 存在性 L1140、_entity_props L1692、_a_universe L1622、edit_molecule L1382、__init__ 断言 L860/L877、evaluate.py L1003（judge 侧） |
| crystals | 17（xa_exp_1 形态） | bandgap/condition/confidence/formation_energy_per_atom/is_superconductor/structure_in_db/tc | _entity_props L1696-1700、_a_universe L1626、_cif_key L1640、_tc_table 回落 L1526-1529 |
| structures | 17（xa 形态） | cif_key/data_provenance/source | get_entity 晶体存在性 L1175、晶体 formula L1179 |
| literature | **14** | doi/title/year/citation_count/keywords/authors… | literature_search L1046 |
| molecule_search | 40 | name/smiles | get_entity 回落 L1163、_entity_smiles 回落 L1657、_a_smiles_universe 回落 L1681 |
| targets / _mapping_scoring_only | 1 / 3 | — | _mapping_scoring_only 供 judge 侧 normalize_id（evaluate.py L50） |

**E4 历史 trace 依赖面（ADR-0014 legacy）**：125 份 trace 中被查实体 top = xb(214 次)/
aa(46)/xd/zb-zg/ec/ba…——药侧（aa/ba/…）在 backend，**晶体侧（xb 等）不在 backend**。
重判安全性：grounding cited_values 对账回放 events.ret（evaluate.py L976-988 从 trace 行取
返回值，不重查库）→ 历史 trace 重判不依赖活库；fixed 命中 normalize_id 对陌生 id 原样透传
→ 无漂移。**mock_db.json 文件本体不得删**（Gateway 构造 L814 / evaluate.py load_mock_db
L22-27 / bare_baseline L419/L460 均直接开文件）。

### 2.2 backend（dag_poc/data/backend/，E1 交付）

| 文件 | 行数 | 字段 | 现状消费 |
|---|---|---|---|
| get_entity/get_entity_A.jsonl | **92,193**（molecule 74,941 + crystal 17,252） | id/entity_type/formula/masked_structure/atom_count/cif/provenance | 仅 _a_identity 身份档（formula/masked_structure/atom_count 透传 + 材料键复算）；**不参与存在性判定**（PR-45 本体） |
| search_candidates/search_candidates_A.jsonl | 92,193 | id/entity_type/structure_hint/smiles/formula/**filter_props（dict：分子 mw/logp/tpsa，全行 dict 类型）**/provenance | 仅 smiles_map（edit/get_entity 派生）；**universe 未接** |
| get_property/get_property_A.jsonl | **240,664**（全 case_ref=None） | entity_id/prop/value（值已类型化 int/float/str） | 仅 judge 侧：evaluate.py _a_property_table L941-982（质量门）、gateway _tc_table L1497-1530（降级键）。**gateway get_property 运行态未接** |
| literature_search/literature_search_A.jsonl | 309 | doc_id/title/year/citation_count/keywords/provenance | **未接** |
| *_B.jsonl ×4 | 0 行 | — | BPartitionBackendJsonlReader 扫描恒空 → 回落 b_partition.json（占位期） |
| per_case_views/ | **24 份**（dh drug 15 + dh material 4 + spare 3 + e105 示例 1 + 无其他） | per_case_view_v1 | 已接（PR-40，任务 #58） |

性质行分布：element_count 92,193 / atom_count 74,941 / tc·family·structure_in_db·condition
各 17,252 / logS 2,299 / bbb 757 / logD 604 / half_life 523 / caco2 235 / herg 104。
**命名差异**：backend 用 `half_life`，mock_db 用 `half_life_days`（旧 fixed case 全用后者）；
backend 无 `pass`（gold 字段，本就该拦）。药侧性质稀疏覆盖（herg 仅 104 实体有值）。

---

## 3. 索引方案（任务书 §5 风险 1）

**实测（冷载，单次全扫 json.loads）**：

| 文件 | 耗时 | 产物 |
|---|---|---|
| get_entity_A（92,193 行） | 0.19s | id→record dict |
| get_property_A（240,664 行） | 0.27s | (entity_id, prop)→value dict |
| search_candidates_A（92,193 行） | 0.28s | id→record dict |

**选型：复用/仿制既有模块级懒缓存 dict（推荐，唯一合规路）**
- 存在性判定的索引**已经存在**：`_A_IDENTITY_CACHE`（L576-587，task-03 落地）装载的
  ent_records 即 92,193 键 dict，O(1) 成员判定。切源只是把判定入口从
  `self.db.get('admet', {})` 换成它——判定依据仍是 E1 落盘全库（非内存小表），缓存仅是
  装载方式，完全满足"不得为性能把存在性判定退回内存小表"。
- get_property 真值表与 search universe 无现成缓存 → 新增两张模块级懒缓存，照
  `_MATERIAL_KEY_CACHE`/`_A_IDENTITY_CACHE` 同款模式（缓存键=backend 根路径，测试临时
  backend 互不串扰；缺场回落 mock_db 口径）。
- **否决项**：① 每次调用线性扫 jsonl（92k×调用次数，检索延迟不可接受）；② 预构建索引文件
  （新增 E1 交付物依赖与校验和面，违反"不改 dag_poc/data/**"范围约束）；③ 存在性退回
  mock_db 40 条小表（红线，任务书明令禁止）。
- **RDKit 成本告警**：`_a_smiles_universe` 现状对 mock_db 40 键逐个 canonical_chemical；
  切源后是 74,941 次 RDKit 规范化（估算分钟级）——必须模块级缓存且只在 edit_molecule
  分支 3 首用时构建一次；或改"锚点 canonical 后查预建反向索引"。此为切源引入的最大单项
  性能风险，实施时须实测首用时延并留痕。

---

## 4. 候选池膨胀与截断口径（任务书 §5 风险 2）

- **现状截断机制已完备**：k∈[1,50] 默认 10（L1090-1092）+ offset 分页（L1095-1097，
  任务 #8 M3 为穷举设计）+ 出参 total_found（L1128）+ 排序 `sorted(hit_ids)` 升序 ID
  （L1122）。切源后 40→92k 无需新参数。
- **与 tools_schema 契约一致性核查（通过）**：search_candidates output oneOf 声明
  candidates maxItems=50 / total_found / offset 回显，与实现逐字一致；无 `limit` 参数，
  k 即截断口径——**契约零改动**。实施时在 report.md 登记一句"fixed 无视图口径=
  k/offset 截断 + total_found 声明命中总数"即满足任务书要求。
- **fixed 类无视图口径（建议写进 report 的表述）**：命中集全量排序（ID 升序，确定性），
  按 offset/k 分页返回，agent 侧靠 total_found 判是否取尽；与 open 类差异仅在视图裁剪
  （L1103-1109），一条代码路径。
- **新增口径分裂（风险，见 §8-R3 与裁定项 F2）**：约束可查性质白名单 `_queryable_props`
  （L1636-1638）切源后将含 filter_props 的 mw/logp/tpsa（search 可过滤），但 get_property
  真值表无此三性质（不可查证）——"可过滤不可查证"的分裂需声明口径。

---

## 5. 视图闸插入点行号清单（切源不动区确认）

| 闸点 | 行号 | 语义 | 切源后 |
|---|---|---|---|
| 构造期视图装载 | L846-848（调 _resolve_case_view L751-802） | open 缺视图 raise CASE_VIEW_MISSING（fail-closed）；fixed/None 缺视图全库 | **不动** |
| literature_search doc 白名单 | L1054-1060 | 过滤后截断；空即 EMPTY_RESULT 同码 | **不动**（源换 literature_search_A） |
| search_candidates universe 裁剪 | L1103-1109 | 白名单外无命中走 EMPTY_RESULT 同码 | **不动**（universe 本体换源，裁剪点保留） |
| get_entity 视图外同码 | L1136-1139 | 视图外与"不存在"同码 UNKNOWN_ENTITY，防源探测 | **不动**（其下的存在性判定换源——正是本任务） |
| get_property 计费后视图校验 | L1207-1208 | 保先计费后校验 | **不动** |
| edit_molecule 实体锚点视图闸 | L1390-1394 | 视图外锚点同码 ANCHOR_NOT_IN_A（二轮评审 Critical-1） | **不动**（受理判据 L1382 换源） |
| compare | 无独立闸 | observed 单一写入口 get_property 已被闸住（监工复验认可） | **不动** |

结论：切源只动"视图内且库内存在"这条路径的数据源，全部视图闸位置、错误码、
防源探测同码口径逐一确认不变。

---

## 6. 五条验收断言的可执行方案（任务书 §3）

统一前置：`import sys; sys.path.insert(0,'dag_poc'); from gateway import Gateway`。

**断言 1（视图白名单成员全查得到）**
```python
gw = Gateway(case_id='case_dh_drug_01', case_kind='open')   # 视图在场（29 成员，实测）
for eid in sorted(gw._case_view_ids):
    r = gw.call('get_entity', {'id': eid})
    assert 'error_code' not in r, (eid, r)
```
预期：29/29 通过（当前实测 0/29，z000374 即返 UNKNOWN_ENTITY）。

**断言 2（gt 无视图可查）**
```python
gw = Gateway()
r = gw.call('get_entity', {'id': 'z064386'})    # case_dh_drug_01 的 gt
assert r.get('id') == 'z064386' and r['entity_type'] == 'molecule'
```
预期：通过（z064386 在 get_entity_A，实测在库）。

**断言 3（视图外实体仍 UNKNOWN_ENTITY，防源探测）**
```python
gw = Gateway(case_id='case_dh_drug_01', case_kind='open')
r = gw.call('get_entity', {'id': 'z064386'})   # gt 在库但不在该视图白名单（validate_view 保证）
assert r['error_code'] == 'UNKNOWN_ENTITY'
```
预期：通过——切源后该断言是最关键的"视图语义不漂移"证明（gt 在库仍被视图闸拦）。

**断言 4（白名单成员 get_property 取值非空）**
```python
gw = Gateway(case_id='case_dh_drug_01', case_kind='open')
r = gw.call('get_property', {'id': 'z000374', 'props': ['logS']})
assert 'error_code' not in r and r['values'][0]['value'] is not None
```
预期：通过（实测 z000374 在 get_property_A 有 atom_count/element_count/logD/logS 四性质；
29 个白名单成员 29/29 有性质行）。注意：dh drug 视图 props 以 logS/logD 为主，
herg/bbb/caco2 稀疏——用例选 logS 稳妥。

**断言 5（全部 case 的 gt 可查）——口径核实见 6.4，建议按实物口径执行**
```python
import json, glob
ent = ...  # get_entity_A 全 id 集（或直接用切源后 Gateway().call('get_entity')）
for p in glob.glob('dag_poc/templates/*/case_*.json'):
    gt = (json.load(open(p)).get('gt') or {}).get('id')
    if gt is None: continue
    assert gt in ent, p     # 预期唯一失败：case_cs_supercon_material（xb_exp_1，旧题预期内）
```

### 6.4 "80 道"口径核实（裁定项 F1）

| 口径 | 数值 |
|---|---|
| 任务书表述 | "现有 80 道 case，30 道可查、50 道查不到"；§5 提"旧 37 道" |
| 实物清点（本次实测） | **176 道 case 文件**：E5 新题 139（co_drug 30 + co_material 30 + cs_drug 30 + cs_material 30 + dh_drug 15 + dh_material 4）+ spare_dh 3 + 旧题 34（fixed_discovery 29 + co_cns/supercon + cs_cns/supercon + open 4） |
| 带 gt.id 的 case | 170 道（6 道无 gt.id：2 道旧模板 co_cns_drug/co_supercon_material 变体 + 4 道旧 open 超导） |
| gt 已在 backend | **169/170**（唯一不在库：case_cs_supercon_material 的 xb_exp_1） |
| 切源后断言 5 预期 | 169 通过 + 1 预期失败（旧题，任务书 §5 风险 1 明言不为其妥协）；切源前为 30/170 通过（30 道全为 mock_db 药侧旧题） |

结论：任务书"80 道"与实物不符（E5 task-04/05 后题量已扩至 139+37），**以实物口径执行
断言 5 并如实登记差异**，请监工确认（F1）。

### 6.5 与 E4 合跑的接缝

断言 1-4 也可直接经 MCP 运行态跑：`DAGPOC_CASE_ID=case_dh_drug_01` 起 gateway_mcp.py
（L130-133 消费 env）+ agent_harness `verify_case_channel`——env 通道为现行一 case 一
server 接缝（task-05 裁定 F2 口径），无需新参数。

---

## 7. 回归面预估

### 7.1 会漂移的测试（实施时必改/必核）

| 测试 | 漂移机制 | 处置建议 |
|---|---|---|
| **test_ab_isolation.py L48-89** | 对 `gw._a_universe()` 全集**逐 ID 关键词扫 + 分页穷举**（现 57 实体 → 切源后 92,193，单测将运行数小时） | **必须重设计**（裁定项 F4）：保隔离语义、换抽样断言或 fixture 小 backend；此为红线级隔离认证测试，改法需监工认可 |
| test_pr40_case_view_gateway.py | 用 mock 时代 id（aa/bd/c013xx）建临时视图——aa/bd 均已在 backend（实测 'aa' 在场），断言大概率不漂移；L353 c01360 走视图闸（先于存在性）不变 | 全量跑一遍确认，重点看白名单内 get_entity/get_property 取值断言（取值来源换成 backend 行，值内容可能不同——断言若写死 mock_db 值会漂） |
| test_budget.py L330-350 | "库外分子"用例若用了现在 backend 里存在的 id，UNKNOWN_ENTITY 断言翻转 | 逐条核对所用 id |
| test_gateway_rdkit_grounding.py / test_review_fixes_41.py | 锚点 'aa' 等已接 backend smiles_map，行为保持；_a_smiles_universe 首构成本上升（缓存后一次性） | 跑通确认 + 首构耗时留痕 |
| gw.__main__ 冒烟（L1854-1871） | 用 'aa'/'ab'，在 backend | 不变 |

### 7.2 judge 侧数据路径（15 实验点是否受影响）

| 路径 | 现状 | 切源影响 |
|---|---|---|
| grounding cited_values 对账（evaluate.py L976-988） | 回放 trace 行 ret 值进 observed，**不重查库** | **零漂移**（历史 trace 重判安全） |
| 条件② anchor∈A（evaluate.py **L1003**） | `judge.db.get('admet')…` mock_db 口径 | **必漂**：z-id 锚点被判非 A → open grounding 条件②误判失败。须同步切 backend ent_records 键集（裁定项 F5：是否纳入本任务范围） |
| fixed 命中 normalize_id（evaluate.py L30-57 / bare_baseline L403/L460） | mock_db 查表，陌生 id 原样透传 | z-id 透传无漂移；旧题口径保留 |
| open hit_gb / resolve（evaluate.py L519-554） | 走 b_partition + resolve 组成映射 | 零漂移（不依赖 A 存在性源） |
| 质量门 _a_property_table（evaluate.py L941-982） | 已消费 get_property_A（judge 侧全局口径） | 零漂移；gateway get_property 切源后**运行态与判分侧真值表同源**，口径反而收敛 |
| Gateway() 构造依赖 mock_db.json 文件在场（evaluate.py L976 judge=Gateway()） | 构造期开 mock_db.json L814 | **mock_db 不得删**（与任务书 §4 一致）；可降为"内容过渡兼容"但文件保留 |

### 7.3 旧 case 预期损坏面（任务书 §5 风险 1，如实报）

- 旧 29 道 fixed_discovery 药题：实体（aa/bc…）在 backend ✓，但 `half_life_days` 性质名
  backend 无（backend 是 `half_life`）→ 涉该性质的约束/查证转 UNKNOWN_PROPERTY。
- 旧超导系（4 道 open + 2 道材料模板 + crystals 17 实体）：晶体 id（xa/xb…）**不在
  backend** → 全面转 UNKNOWN_ENTITY。任务书明言这些题将被 E5 新题替换，不为其妥协。
- E4 九组冒烟 trace：重判走 trace 回放（§7.2），**不依赖活库，可重判能力保留**。

---

## 8. 风险清单（任务书 §5 两条 + 本次新发现）

| # | 风险 | 等级 | 应对 |
|---|---|---|---|
| R1（任务书）| 切源后旧 case 全挂 | 中 | 如实报（§7.3），不妥协；mock_db 文件保留 |
| R2（任务书）| 92k 线性扫性能 | **已消解** | 索引已存在（§3），实测冷载 <0.3s/表 |
| R3（新）| filter_props 的 mw/logp/tpsa 可约束过滤但 get_property 不可查证——口径分裂，agent 过滤后无法取证（grounding cited_values 写不出） | 中 | 裁定项 F2：建议在 tools_schema behavior_notes 声明"filter-only 性质"，数据补齐归 E1（不在本任务改数据范围） |
| R4（新）| _a_smiles_universe 切源后首构需规范化 74,941 条 SMILES（RDKit，分钟级风险） | 高 | 模块级缓存 + 首次使用时构建 + 实测留痕；必要时改懒构建反向索引 |
| R5（新）| evaluate.py L1003 条件②未同步 → open 题 grounding 结构性误判（切源即触发） | 高 | 裁定项 F5：建议与 gateway 切源同批实施（同一判分口径事件） |
| R6（新）| test_ab_isolation 穷举式测试切源后运行时间爆炸（57→92,193 实体逐 ID 扫） | 高 | 裁定项 F4：重设计方案需监工认可（红线级隔离认证） |
| R7（新）| 药侧性质稀疏（herg 104/caco2 235 实体有值）→ 白名单成员 get_property 选错性质会 UNKNOWN_PROPERTY | 低 | 验收用例固定选 logS（断言 4 已如此）；E5 造题已按库覆盖配 props |
| R8（新）| dh drug 15 道视图 literature_doc_ids=0 → 切源后这些 case 文献检索恒 EMPTY_RESULT | 低 | 属视图语义（空即无文献，同码防探测），非缺陷；report 登记即可 |

---

## 9. 裁定项（需监工拍板，附推荐选项）

> **裁定索引（任务 #67 回写补一行，2026-08-08）**：F1-F5 均已按推荐选项裁定并在实施中落地（ee6a5c5），另增 F6（B∩A 键空间收窄 + 占位机器识别，同 commit 落地）；对账与台账登记见 report.md §13.3/§13.6 与 problems.md PR-46~50，正文不重写。

| # | 事项 | 推荐选项 |
|---|---|---|
| **F1** | 断言 5 的"80 道"口径与实物（176 文件/170 带 gt）不符 | **按实物口径执行**：169/170 通过 + case_cs_supercon_material 预期失败如实登记；任务书数字订正归回写轮 |
| **F2** | filter_props（mw/logp/tpsa）可过滤不可查证的口径分裂 | **声明 filter-only 口径**（tools_schema behavior_notes + report 登记），不在 gateway 侧把三性质塞进 get_property 真值面（数据补齐若需要归 E1） |
| **F3** | 旧性质名 half_life_days vs backend half_life 是否做 gateway 侧别名 | **不做别名**（旧题将被替换；别名会引入隐性口径漂移）；legacy 重判若需要归 judge 侧 normalize 层另议 |
| **F4** | test_ab_isolation 切源后不可运行，重设计方式 | **fixture 小 backend 保穷举语义**（临时 backend 根注入，57 实体级规模穷举不变）+ 全库抽样不变式断言补充；备选：纯抽样。涉及红线级隔离认证，须监工选 |
| **F5** | evaluate.py L1003 条件② mock_db 口径是否纳入本任务同批切 | **纳入**（gateway 切源不同步 judge 侧 = open 题 grounding 即刻断裂，两者是同一口径事件）；若范围收窄则须明示遗留风险 |

---

## 10. 改动点汇总（行号级，供实施任务直接取用）

gateway.py（主改动面）：
1. `_tool_get_entity` 存在性判定：L1140（admet 分支）/L1175（structures 分支）改查
   `_a_identity` ent_records；晶体取值分支 L1177-1183 同步改 backend 记录字段。
2. `_entity_props` L1689-1701：改查新增 get_property_A 表缓存；缺场回落 mock_db。
3. `_a_universe` L1616-1634：改由 search_candidates_A 缓存构建（filter_props dict 直取）。
4. `_tool_literature_search` 源 L1046：切 literature_search_A 缓存。
5. `_tool_edit_molecule` 分支 2 受理判据 L1382：改查 ent_records。
6. `_a_smiles_universe` L1662-1687：键集改 ent_records 分子实体，模块级缓存（R4）。
7. `__init__` B∩A/G_B∩A 断言 a_ids 来源 L860-862/L877-879：改 backend 键集。
8. 新增模块级懒缓存两张（property 表/search universe）+ 装载函数，仿
   `_A_IDENTITY_CACHE`/`_MATERIAL_KEY_CACHE` 模式（缓存键=backend 根路径）。

evaluate.py（关联改动面，F5 裁定后执行）：
9. L1003 条件② `a_ids` 改 backend ent_records 键集（与 gateway 同源同缓存）。

不动区：全部视图闸（§5 表）、check_molecule_exists/normalize_structure/submit_answer、
compare 口径、mock_db.json 文件本体、dag_poc/data/** 内容、query_lint.py、harness。

测试基线：304（dag_poc 241 + eval_matrix 61 + mcp e2e 2），实施后全量回归对齐。
