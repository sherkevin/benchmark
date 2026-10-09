# task-05 调研报告：存在性接口 / 条件③组成比对 / 质量门规格（调研=task #51，供 #52/#53/#54 实施）

| 字段 | 值 |
|---|---|
| 调研人 | Alex（只读调研，全部结论含行号/实跑证据） |
| 基线 | pytest `dag_poc/mcp_mock_server/tests/` = **155 passed, 1 skipped**（本次实测） |
| 上游 | ADR-0018 §4/§5 · task-05-存在性接口与组成层面grounding.md |
| 下游 | #52 存在性接口 · #53 条件③组成比对 · #54 E5 质量门 |

## 0. 核心结论（先总）

1. **存在性接口三分支已实现且语义正确**（本次实跑验证）：B 内→true、A 内/库外→false 同一成功出口（gateway.py L1169-1170），**正常路径返回体结构已逐字节同形**；缺的是断言锁死与判分侧落点。
2. **无自证循环**：evaluate.py 对 exists 返回价值零消费（全文实测），hit_gb 独立经 B 分区读取器判定。
3. **"超限记 0 分"判分侧落点不存在**——目前只有网关错误信封文案（"该 case 记 0 分"），evaluate.py 无任何 attempt/existence 消费。这是 #52 的核心缺口。
4. **条件③判定点**在 evaluate.py evaluate_grounding L642-677（结构口径：录像 edit_molecule 成功行 result_id 与 answer 同 canonical）。anchor 元素计数**数据源已就绪**：A 侧身份档 get_entity_A.jsonl 每行带 `formula` 字段（如 `[16*]15[19*]25[22*]3[81*]1`，直接解析即可，不需 RDKit）。
5. **4 个裁定项**需上报：①edit_op 封闭算子集/detail 格式与现行 5 元 enum 冲突；②材料侧计数为 occupancy 小数，多重集相等需容差口径；③条件③改组成后是否保留"录像须有 edit_molecule 成功行"行为证据；④跳数≤2 在组成算子下的定义。
6. **质量门第二条判据目前无代码实体**（仅 E5 任务书文字定义），#54 需提供纯函数接口。

## 1. ADR-0018 §4/§5 硬约束提炼（governance/adr/0018-open类构造方式per-case-AB配对.md）

§4 存在性接口（L85-105）：
- **H1 三分支写死**：∈B→true；∈A→false；库外→false（L89-93），不得配置化；owner 原话：查不到性质，只能查存不存在。
- **H2 只返单个 bool**：不带性质/相似度/距离/任何提示（L99）。
- **H3 "不存在"返回体逐字节相同**：A 内与库外不得由文案差异反推成员资格，**要有断言**（L100）。
- **H4 max_attempts ≤ 10**：写死上限（gateway.py:744），超限记 0 分，attempts 计入指标（L101）。
- **H5 返"存在"≠得分**：评分依据 = ∈G_B 且经 B 侧性质复核；接口返值只是停止信号，混用=自证循环+可被暴力枚举攻破（L103-105）。

§5 组成层面（L109-133）：
- **H6 不需要拓扑结构**：edit_op 定义为**组成算子**，封闭集 = {+n 个元素 e, −n 个元素 e}，跳数 ≤2（L125）。
- **H7 条件③新判法**：anchor 的元素计数按 edit_op 变换后，与提交分子式做**多重集相等**（L133）；条件①②不变。
- **H8 同分异构**（§8 L174-189）：组成比对会把同分异构判为相等，是**预期行为**；同组成多分子部分满足目标窗口→该 case 废弃（归 E5 造题处置，判据里不得试图区分）。
- **H9 固定映射保留**（§6）：元素计数在匿名域固定映射下（如 16=C、19=H、22=O、81=N，见 E5 task-03 §2.3 实测族），映射表 agent 全程不可见（anonymization_mapping.json visibility 已声明）。

## 2. 现状盘点（逐函数/逐路径）

### 2.1 存在性接口实现（dag_poc/gateway.py）

| 位置 | 内容 |
|---|---|
| L1143-1170 `_tool_check_molecule_exists` | 唯一成功出口 L1169-1170：`{'exists', 'attempt', 'attempts_remaining'}`；`exists = normalize_b_key(mid) in self.b_set`（L1168）——三分支已是"∈B 真、其余假"且 A 内/库外**共用同一 return** |
| L1152-1160 | 入参校验：未知入参/molecule_id 缺失/attempt 越界 → INVALID_ARGUMENT 信封（attempt 为可选元数据，1..10） |
| L1161-1164 | 超预算：`existence_used >= max_attempts` → ATTEMPTS_EXCEEDED 信封，附 `exists=false, attempt, attempts_remaining=0, exhausted=true` |
| L1165 | `existence_used += 1`——服务端真实计数（任务 #8 C1，agent 自报不参与预算） |
| L744 | `max_attempts = max(1, min(int(max_attempts), 10))` 写死上下限；非法类型 L746 raise |
| L760 | `existence_used = 0` 会话级初始化 |
| L228-249 `normalize_b_key` | 查询键归一（canonical_chemical 优先，try/except 回落原串）——**不抛异常** |
| L850-876 `call()` | 异常兜底：内部异常转 INTERNAL_ERROR 信封（含异常类型文案），先落录像再返回 |
| L380-467 `BPartitionBackendJsonlReader` | b_set/g_b 装载：存在性通道 check_molecule_exists_B.jsonl 两列索引（inchikey/canonical_smiles）全量为 g_b；带 case_ref 文件按 case 过滤 |
| L708-739 `Gateway.__init__` | 装载三断言：B∩A=∅、G_B⊆members、G_B∩A=∅，违反即 raise |

**实跑验证（本次）**：A 内 'aa' → `{'exists': False, 'attempt': 1, 'attempts_remaining': 9}`；库外 'zz_not_in_db_9527' → 同结构 exists=False；B 内成员 → exists=True；两 false 分支键集合+类型完全一致。**第 11 次调用** → ATTEMPTS_EXCEEDED 信封，existence_used 停在 10。

### 2.2 字节级泄漏路径清单（H3）

| 路径 | 返回面貌 | 泄漏风险 |
|---|---|---|
| 正常查询（A 内/库外/B 内） | 同一出口三字段 | **无**（A 内与库外逐字节同形，仅计数字段随次数变）——但无断言锁死，未来改动可回归破坏 |
| ATTEMPTS_EXCEEDED（超预算） | 多 error_code/error/exhausted 字段 | 对任何查询**同等触发**（不区分成员资格），agent 已知自己调用次数，无增量信息；**口径须写入断言声明** |
| INVALID_ARGUMENT（入参非法） | 错误信封 | 同等触发，无泄漏 |
| INTERNAL_ERROR（call 兜底） | 含异常类型文案 | 存在性路径现无异常源（normalize_b_key 有 try/except、b_set 为集合成员判定）——理论不可达，断言应守住"凡带 exists 语义的成功体必为三字段形状" |
| MCP 面（mock_server.py L152-154 / gateway_mcp.py） | `_contract_call`→Gateway.call 薄包装 | **无独立返回路径**，改动自动同步，无额外泄漏面 |
| JSON 序列化顺序 | Python dict 插入序 | 两侧同一出口→同序；断言建议用 sort_keys 归一后逐字节比 |

**断言设计建议（#52 交付 1）**：同一 Gateway 会话内，取 A 内分子（admet 首键 'aa'、structures 首键）、库外分子若干（含乱串/超长串/空串边界），断言：① 去掉 attempt/attempts_remaining 两个计数字段后，返回体 json.dumps(sort_keys=True) 与 A 内参考体**逐字节相等**；② 键集合严格 == {'exists','attempt','attempts_remaining'}（additionalProperties=false 的执行面）；③ 任一非成功信封必含 error_code 且不得出现"裸 exists 无信封"混形。

### 2.3 自证循环审计（H5，逐路径）

**结论：无自证循环。**
- evaluate.py：`hit_gb`（L519+）经 BPartitionJsonReader 独立重判 ∈G_B，不读 trace 中 exists 值；`extract_tool_data`/`_extract_new` 对 check_molecule_exists 显式"无判分语义，跳过"（L165、L211-212）；`_score_open_discovery`（L683-701）：open_gate = hit_gb ∧ grounding_pass——无 exists 消费；全文 grep `exists/attempts/existence` 判分消费为零。
- gateway.py：exists 只返给 agent；Gateway 自身不产分。
- **残留风险（文字层）**：ATTEMPTS_EXCEEDED 的 error 文案写着"该 case 记 0 分"，但判分侧并未执行——见 2.4。

### 2.4 max_attempts 现状与"记 0 分"缺口（H4，调研项 4）

- 上限写死：L744（已验，构造期 clamp 1..10）。
- 拒绝行为：第 11 次真实调用返 ATTEMPTS_EXCEEDED（实跑已证），录像行 ok=False 落盘（call L869-875）。
- **open 路径生效性**：max_attempts/existence_used 是 Gateway 实例级状态，open case 与 fixed case 走同一 Gateway 类与同一工具分发（CANONICAL_TOOLS L102 含 check_molecule_exists），结构上必然生效；E4 harness 跑 open case 经同一 call()。缺的是**端到端实跑留痕**（#52 交付 2：open 场景实跑 11 次）。
- **"记 0 分"判分侧落点：不存在**。evaluate.py 无 attempt/existence/budget 任何消费（grep 0 命中）；open 判分 `_score_open_discovery` 只看 hit_gb ∧ grounding。→ **#52 需在判分侧补落点**，规格见 §3.1。

### 2.5 grounding 条件③现状（src/benchmark/evaluate.py）

- 位置：`evaluate_grounding` L557-680；条件③ = L642-677（键名 `edit_op_reproduces_answer`）。
- **现判法（结构口径）**：扫描录像中 edit_molecule 行，取 `error_code` 缺席且 `applied is True` 者（L651-656），三元组 (anchor_id, operator, detail) 与答卷 edit_op 相符（L666）且 `result_id` 与 `answer` 规范化后相等（canonical_chemical 优先、canonical_structure 回落，L658-674）。
- 条件①（L579-604）：cited_values 经 Gateway.grounded 对账，不变。条件②（L606-640）：anchor ∈ A（实体 ID 全集 ∪ A 分子 canonical SMILES 全集 ∪ 材料身份键全集），不变。
- 三门闭环：`_score_open_discovery` L696 open_gate = hit ∧ grounding_pass；grounding_mode placeholder/real 落账（L699-701）。
- 实跑证据基线：`rehearsal_logs/grounding_condition3_realdata.log`（任务 #36 生成：aa + add_atom F → 三判据全 True；注意 detail='F' 是**真实元素符号**口径）。

**组成比对数据流盘点（#53 设计输入）**：
| 量 | 来源 | 形态 |
|---|---|---|
| anchor 元素计数（分子侧实体 ID） | get_entity_A.jsonl `formula` 字段（gateway `_load_a_identity` 已装载，judge=Gateway() 即持有） | `[16*]15[19*]25[22*]3[81*]1`——解析正则即可，无需 RDKit |
| anchor 元素计数（分子侧 SMILES 锚点） | dummy atom canonical SMILES | RDKit 按 GetIsotope()==label 计数（复用既有 RDKit 通道） |
| anchor 元素计数（材料侧） | 材料身份键 `<reduced_formula>_SG<n>` | 取 `_SG` 前段解析，如 `[20*]1.98[32*]1[42*]0.02[48*]2`——**计数可为小数（occupancy）** |
| edit_op | answer_schema.json `properties.edit_op`：{anchor, operator, detail}；operator enum 现为 **5 元**（add_atom/remove_atom/replace_substituent/substitute_element/ring_closure） | 与 ADR-0018 §2.3 组成封闭集 {+n e, −n e} **不一致**（裁定项 1） |
| 提交分子式 | 答卷 `answer` 字段（可能是 dummy SMILES / formula 串 / 材料键） | 需统一解析器 composition_of() |

### 2.6 质量门现状（E5，调研项 8）

- **第一条判据**（A 内无替代解穷举）：归 E1-05 per-case 视图穷举接口——**在途未提交**（观察到 `dag_poc/data/backend/per_case_views/`、`src/data_build/per_case_view.py` 等 untracked，只观察不触碰）。目前无 E3 可依赖的落码。
- **第二条判据**（线索可达）：仅存在于 E5 任务书文字（task-03-造30道open发现题.md §3.2 L99-102）：a) edit_op(anchor) 组成变换结果与 gt 分子式多重集相等；b) evidence_set 每条可由 A 侧接口查得。**无代码实体**——#54 交付。
- E5 case 现状：templates/open_discovery/ 仅 4 个旧超导 case（旧契约形态），30 道新 open 题未开造（等 E3-05/E1-05）。

### 2.7 tools_schema 契约现状（check_molecule_exists）

- 三分支语义已写明（description + behavior_notes ①）；只返单 bool 已写明（②+output additionalProperties=false）；max_attempts 上限 10/超限记 0 分/attempts 计入指标已写明（budget 字段 + ④）；服务端计数口径已写明（⑤）。
- **缺口**：①"不存在返回体逐字节相同"未写入契约文字（behavior_notes 可加一条）；②错误信封 enum 缺 INTERNAL_ERROR（call 兜底可产生，虽当前路径不可达）；③edit_op 契约（answer_schema edit_op enum 5 元）与组成算子集的冲突见裁定项 1。

### 2.8 测试基线与分组惯例

- 基线 155 passed + 1 skipped；惯例（任务 #33 物理拆分后）：**每个测试文件独立报数，docstring 首行声明所属组别**（如 isolation_ 组）。新测试按同样惯例新建独立文件、声明组别。
- 现有相关文件：test_budget.py（存在性语义抽查 L76-81）、test_review_merge_fixes.py（M1 双写法一致）、test_backend_jsonl_reader.py（装载/隔离）、test_gateway_rdkit_grounding.py（条件③真实化 L329-357）、test_evaluate_open_discovery.py（三门闭环+条件③负例 L170-180）。

## 3. 三个实施任务的规格

### 3.1 #52 存在性接口（三分支 + 字节级断言 + 11 次实跑 + 记 0 分落点）

**判定：三分支实现本体无需重写**（已符合 H1），本任务重心 = 断言锁死 + 判分侧落点 + 实跑留痕 + 契约回写。

| # | 规格 | 落点 | 验收口径 |
|---|---|---|---|
| 1 | 字节级无泄漏断言 | 新建 `tests/test_existence_byte_equality.py`（docstring 声明组别，如 existence_ 组） | 按 §2.2 断言设计三条；A 内参考分子 + ≥3 个库外样本（含乱串/超长/空串）；去计数字段后 json.dumps(sort_keys=True) 逐字节相等；另加 fuzz 循环（如 20 个随机串） |
| 2 | 三分支实跑用例 | 同文件 | B 成员 True、A 内 False、库外 False 各 ≥1 实跑断言（B 成员用顶层 b_partition 既有 fixture，勿新造 B 数据） |
| 3 | **11 次实跑留痕**（open 路径生效性） | 同文件或独立 rehearsal 脚本；日志落 `rehearsal_logs/`（仿 grounding_condition3_realdata.log 惯例） | 同一 Gateway 会话连调 11 次：1-10 次成功信封、第 11 次 ATTEMPTS_EXCEEDED 且 exists=false/exhausted=true；声明该会话即 open case 共用路径（Gateway 实例级预算，CANONICAL_TOOLS 分发同一入口） |
| 4 | **"记 0 分"判分侧落点**（核心新增） | src/benchmark/evaluate.py open 判分链 | 判据（推荐）：trace 中出现 check_molecule_exists 的 ATTEMPTS_EXCEEDED 信封行 ⇒ 该 case open_score 强制 0（open_gate=False），并在 extra 落 `existence_exhausted=True` 留痕；同时把成功存在性调用次数计入 extra（attempts 指标，供 goal §6 步 6 平均 attempts 汇报）。**不得把 exists 值本身接入任何得分**（H5 红线，新增消费点即自证循环） |
| 5 | 契约回写 | dag_poc/tools_schema.json behavior_notes | 加"不存在返回体逐字节相同（断言锁死，测试名）"一条；错误信封 enum 是否补 INTERNAL_ERROR 见裁定项 |
| 6 | 回归 | 全量 pytest | 155+1 → 新增后仍零失败；test_budget.py 既有断言（L76-81）不得动 |

**验证方案（回答调研项 4）**：max_attempts 机制 = Gateway.__init__ L744 clamp + L1161-1165 服务端计数，open 与 fixed 共享同一 Gateway 类实例状态，结构上对 open 路径必然生效；验证以"open case 会话内实跑 11 次 + 判分侧对含 ATTEMPTS_EXCEEDED 行的 trace 判 0 分"两条实跑证据为准。

### 3.2 #53 条件③改组成多重集比对

**判定：只改 evaluate_grounding 条件③（L642-677），其余判分公式零改动。**

改法（数据流四步）：
1. **composition_of(x) 统一解析器**（新增辅助函数，建议放 evaluate.py 或独立模块）：
   - formula 串（`[16*]15[19*]25...`）→ 正则 `\[(\d+)\*\](\d+(?:\.\d+)?)` 解析为 {label: count}（材料侧计数含小数）；
   - dummy atom SMILES → RDKit 计数 GetIsotope() 非零原子（复用既有 RDKit 守卫 _require_rdkit 口径）；
   - 材料身份键 → 拆 `_SG` 取 reduced_formula 段解析；
   - 实体 ID → 查 A 侧身份档 formula 字段（judge._a_identity 已装载；材料侧走 _material_key_universe 同一接缝）。
2. **edit_op 施加**：anchor 计数向量按 operator 变换（add_atom: +n 个 detail 元素；remove_atom: −n 个；n 与 detail 格式见裁定项 1/4）。
3. **多重集相等**：变换后向量与 composition_of(answer) 逐项相等（材料侧浮点容差见裁定项 2）。
4. **录像行为证据**（裁定项 3 的推荐口径）：**保留**"录像中存在同三元组 edit_molecule 成功行（applied=True）"要求，只把身份判定从 result_id==answer 结构相等改为组成相等——否则 test_evaluate_open_discovery.py L170-180 的负例语义（applied=False 行不得充当证据）崩塌，且 agent 可跳过 edit_molecule 直接编 edit_op+answer 绕过 trace grounding。

**测试要求**：
- **同分异构用例（task-05 交付 4 点名）**：构造"组成相同但结构不同"的 answer，判**相等**（True）——这是组成口径与旧结构口径的行为差异点，必须显式锁。
- 正例回归：rehearsal_logs/grounding_condition3_realdata.log 场景（aa + add_atom F）在新判据下仍 True（anchor formula + F == 产物组成，成立）。
- 负例：组成差一个原子 → False；edit_op 三元组与录像不符 → False。
- 落位：新建 `tests/test_grounding_composition.py`（composition_ 组）或并入 test_gateway_rdkit_grounding.py（该文件已承载条件③组）——建议新文件，独立报数。

**不动区清单（§2.3 硬约束，禁止触碰）**：
| 禁触对象 | 位置 | 理由 |
|---|---|---|
| `canonical_structure`（RDKit 两级 + dummy atom 规范化 [2*][1*]→[1*][2*]） | gateway.py L1252-1273 调用面 + 实现 | B 侧成员存完整匿名拓扑，身份键比对仍用它（task-05 §2.3 明文不删） |
| `_rdkit_edit` / `_tool_edit_molecule` 两分支 | gateway.py L266-300+、L1172-1250 | 工具接口的结构编辑语义不变（applied 仍是真实结构可施加性）；变的只是判分侧条件③的**身份判据** |
| `_tool_normalize_structure` | gateway.py L1252-1273 | 同上 |
| `normalize_b_key` + b_set/g_b 装载三断言 | gateway.py L228-249、L708-739 | 存在性/判分共用口径 |
| evaluate_grounding 条件①②、hit_gb、_score_open_discovery 公式 | evaluate.py L579-640、L519+、L683-701 | 本次只改条件③身份判定 |
| dummy atom 相关测试（test_gateway_rdkit_grounding 条件③正例、test_backend_jsonl_reader 隔离组） | tests/ | 结构口径用例应继续通过（组成口径更粗，结构相等蕴含组成相等）；唯 L170-180 负例语义依赖"录像行要求"，按上面第 4 步保留则零改动 |

### 3.3 #54 E5 质量门第二条判据接口

**接口签名建议**（E3 提供给 E5 直接调用；落点建议 src/benchmark/ 下新模块或 evaluate.py 内纯函数，**不引入活体 Gateway 会话依赖**，造题期可离线跑）：

```
composition_grounding_gate(case_id, anchor, edit_op, gt, evidence_set, tol=None) -> dict

入参：
  case_id      : str   —— per-case 视图标识（evidence_set 查得性验证走该 case 的 A 侧视图）
  anchor       : str   —— 锚点（实体 ID / dummy atom SMILES / 材料身份键，三种形态都受理）
  edit_op      : dict  —— {'anchor','operator','detail'}，与 answer_schema.edit_op 同形状
  gt           : str   —— gt 分子式或 gt 身份键（B 侧组成由 judge 侧数据派生，入参不需要 B 明文结构）
  evidence_set : list[dict] —— 每条 {'mol','prop','value'}（同 cited_values 形状）
  tol          : dict  —— 容差（浮点组成/性质值），缺省走项目默认口径

出参（全部布尔 + 诊断）：
  {
    'pass': bool,                    # 总判据 = 以下四条全真
    'composition_equal': bool,       # edit_op(anchor) 组成变换 == gt 分子式（多重集相等）
    'hops_within_limit': bool,       # 跳数 ≤2（定义见裁定项 4）
    'anchor_in_A': bool,             # anchor ∈ 该 case 可见集（复用条件②口径，E1-05 视图就位后切视图）
    'evidence_all_queryable': bool,  # evidence_set 每条可由 A 侧接口查得（get_property 在 A 视图内有值）
    'details': {...}                 # 逐条失败原因（供 E5 判废记录，E5 task-03 交付 5 要求如实报判废原因）
  }
```

要点：
- **第一条质量门不归 E3**（E1-05 穷举接口，在途）——本函数不得包含"A 内无替代解"逻辑。
- 复用而非新写：组成解析用 #53 的 composition_of；anchor∈A 用条件②同一函数；查得性用 Gateway(case_id) A 侧只读路径（*_A.jsonl 白名单，B 文件句柄零接触——与 test_backend_jsonl_reader 句柄级隔离断言同口径）。
- 出入参文档随代码落 docstring + tools契约规范/调研文档回写（E3 惯例，#54 收尾交付）。
- E5 侧消费方式（对齐 task-03-造30道open发现题.md §3.2）：造题脚本逐 case 调用，pass=False 即换 gt 重配，判废计数入自检记录。

## 4. 风险与裁定项（需上报）

| # | 项 | 现状冲突 | 建议裁定方向 |
|---|---|---|---|
| **1** | **edit_op 封闭算子集** | answer_schema/tools_schema 现行 operator enum = 5 元（add_atom/remove_atom/replace_substituent/substitute_element/ring_closure）；ADR-0018 §2.3 组成封闭集 = {+n e, −n e}。replace_substituent/substitute_element 可解释为 −e1+e2 组合，ring_closure 无组成语义；**detail 现格式为单元素符号（rehearsal 实证 detail='F'），无法表达 n>1** | 请监工/owner 裁：a) enum 收敛为 add_atom/remove_atom 两个（其余废弃）还是保留 5 元并定组成解释；b) detail 格式升级（如 `{"label": 22, "count": 2}` 或串 `22*2`），涉及 answer_schema 升版 |
| **2** | **材料侧浮点计数** | get_entity_A.jsonl 晶体 formula 含 occupancy 小数（`[20*]1.98[42*]0.02`），多重集相等需容差；容差取多大直接影响判据松紧 | 请裁定容差口径（建议相对/绝对双容差，如 abs ≤ 1e-6 或与 E1 数据精度对齐） |
| **3** | **条件③是否保留录像行证据要求** | 纯组成重算可脱离录像（agent 可不跑 edit_molecule 直接编 edit_op+answer，弱化 trace grounding）；现有负例测试（test_evaluate_open_discovery L170-180）依赖录像行要求 | 建议保留（行为证据+身份判据双层），若监工裁定纯组成重算则需同步改该测试语义并报 |
| **4** | **跳数 ≤2 在组成算子下的定义** | 结构口径一跳=一次 edit_molecule 成功行；组成口径 {+n e} 一跳是 n=1 还是 n 任意？goal §2b④ 原文为结构跳数 | 建议：单算子 count 之和 ≤2（即 |+n| 的 n ≤2），与 ADR-0018 "跳数≤2 沿用" 一致；请确认 |
| 5 | **判别力风险（观察项）** | 组成口径比结构口径粗：同分异构判相等是预期（ADR-0018 §8），但意味着条件③对"agent 提交错误拓扑但组成对"不敏感——判别力主要靠 hit_gb（B 成员身份是完整拓扑键）兜底 | 无需裁定，登记知悉：B 侧身份键比对（normalize_b_key 完整拓扑）不受本次改动影响，判别力底盘仍在 |
| 6 | **ATTEMPTS_EXCEEDED 信封附加字段** | 比成功信封多 error/exhausted 字段，虽同等触发不泄漏成员资格，但严格读 H3"逐字节相同"仅约束"不存在"正常返回体 | 建议断言口径显式声明"字节级约束作用于成功信封；错误信封对所有查询同等面貌"，避免过度解读 |

## 5. git 纪律提醒（写入各实施任务规格）

- **精确路径暂存，勿 `git add -A`**：当前工作区有 E1/E5 在途未提交改动（`dag_poc/data/backend/per_case_views/`、`dag_poc/templates/discovery_haystack/`、`src/case_build/build_open_drug_cases.py`、`src/data_build/per_case_view.py` 等 untracked；两个 rehearsal log 与 E1 手册 M 状态在途），误暂存会污染 E3 提交。
- #52 暂存面（预期）：`dag_poc/gateway.py`（如有动）、`src/benchmark/evaluate.py`、`dag_poc/tools_schema.json`、新测试文件；#53：`src/benchmark/evaluate.py`、新测试文件、rehearsal_logs/；#54：判据模块 + 文档。逐文件 `git add <精确路径>`。
- 红线（task-05 §4）：不改 `dag_poc/traces/**`、`templates/**`、`query_spec.json`、`dag_poc/data/**`；**不删 task-03 的 dummy atom 结构规范化**（§3.2 不动区清单）。

## 6. 实施顺序建议

1. **#52 与 #53 可并行**（#52 动存在性/预算链，#53 动 grounding 条件③，文件交集仅 evaluate.py 不同区段——建议 #52 先合或约定区段）；**#54 依赖 #53 的 composition_of**，串行在后。
2. #52 内部：字节级断言先行（锁住现状）→ 判分侧记 0 分落点 → 11 次实跑留痕 → 契约回写。
3. #53 内部：裁定项 1/2/4 先决（detail 格式/容差/跳数定义定了才能写组成变换）→ composition_of → 条件③替换 → 同分异构用例 + 回归。
4. 停止准则（对齐 task-05 §5）：pytest 零失败（155+1+新增）；三种输入各有实跑用例且字节级断言在场；11 次实跑第 11 次被拒且判分侧记 0 分留痕；条件③组成比对含"同分异构判相等"用例；质量门接口出入参文档齐。
