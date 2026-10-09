# tools 契约规范（E3 · 步 3 · 交付物 2）

| 字段 | 值 |
|---|---|
| **任务** | `task/E3-tools敲定与MCP统一/task.md` 子目标 3、4、5、6 |
| **对齐基准（桩）** | `dag_poc/tools_schema.json`（v0.1.0-stub）与 `dag_poc/answer_schema.json`——本文为桩的人类可读契约转述与统一调用约定；**与桩逐字段核对记录见 §5，发现不一致以 goal.md 与桩为准并登记** |
| **上位文件** | `goal.md` §2b（A/B 分层与存在性接口）、`governance/adr/0012-两类benchmark设计范式与query专家背书要求.md` §1、`dag_poc/docs/INTERFACE_CONTRACT.md` §3 |
| **作者** | Taylor（E3 工具线 · 任务 #3） |
| **日期** | 2026-08-06 |

---

## 0. 结论先行

**9 个 v1 契约工具的全部出入参与错误语义以 `dag_poc/tools_schema.json` 为唯一机器可读基准；本文给出逐 tool 契约、统一 MCP 调用约定（命名/参数/返回/13 个统一错误码/录像行/reward 引用），以及 open 类赖以成立的 A/B 源分层设计——其中 B 库物理隔离、存在性接口三概念分离与"仅用 A 源穷举无法得到 gt"的隔离测试方案是本章重点。**

四节导览：

1. **§1 逐 tool 契约**：9 个工具各一节，名称/功能一句话/入参 schema/出参 schema/错误语义，与桩逐字段一致。
2. **§2 统一 MCP 调用约定**：收敛现有 23 工具各自一套的出入参风格为单一规范；录像行 `{t, tool, args, ret, ok}` 与 reward 引用 `{tools.<name>.in|out.<key>}` 在此定义。
3. **§3 A/B 源分层设计**：source_tier 四级体系、B 库物理隔离、`check_molecule_exists` 三概念分离、max_attempts≤10、验收硬标准的隔离测试方案（脚本实现归 Jimmy 任务 #4，本文只写设计）。
4. **§4 与 ADR-0011 现存答卷契约的映射**：`screened_pool`/`ranked_candidates` 是 `answer_schema.json` 的单向机械派生，不是第二套口径。

---

## 1. 逐 tool 契约（9 节）

**小结：每个工具的字段表直接转写自桩的 JSON Schema（字段名/类型/约束逐一对应），实现者（gateway.py，归 Jimmy 任务 #4）可据此直接编码，无需再读桩外材料。**

通用约定（适用全部 9 个工具）：

- 入参为单个 JSON object，`additionalProperties=false`；未列入 schema 的 key 一律返 `INVALID_ARGUMENT`。
- 出参为单个 JSON object；**错误时不返正常出参**，返统一错误结构（§2.4），错误码取自该工具 `errors` 声明集。
- 每次调用无论成败落一行录像（§2.5）；预算计数在网关侧执行，超限返 `BUDGET_EXCEEDED`/`ATTEMPTS_EXCEEDED`。

### 1.1 `literature_search`（A · search_budget）

- **功能一句话**：按自然语言查询检索学术文献，返回论文元信息（ID/标题/年份/引用数），支撑 cp1 文献检索 checkpoint。
- **aliases**：`fetch_bib_info`、`search_drug_literature`、`screen_literature`（录像落盘时归一为本名）。

**入参 schema**：

| 字段 | 类型 | 约束 | 说明 |
|---|---|---|---|
| `query` | string | **必填** | 自然语言检索词 |
| `max_results` | integer | 1–20，默认 5 | 返回上限 |

**出参 schema**：

| 字段 | 类型 | 说明 |
|---|---|---|
| `query` | string | 回显检索词 |
| `total_found` | integer | 命中总数 |
| `returned` | integer | 本次返回条数 |
| `papers[]` | array of object | 每条含 `doc_id`(string，DOI/PMID，供 trace.docs 与 cp1 硬判)、`title`(string)、`year`(integer)、`citation_count`(integer)；四字段均必填，无额外字段 |

**错误语义**：`INVALID_ARGUMENT`（入参缺失/越界）、`EMPTY_RESULT`（无命中，**不得编造论文**）、`BUDGET_EXCEEDED`（检索预算耗尽）。

**行为红线**：只返回文献元信息，不返回任何分子/性质数值（性质一律走 `get_property`，承 INTERFACE_CONTRACT §9 遗留问题 3 的倾向）。

### 1.2 `search_candidates`（A · search_budget）

- **功能一句话**：按约束/关键词检索候选实体（分子或材料），返回实体 ID + 结构类可见字段，支撑 cp2 候选搜集 checkpoint。
- **aliases**：`search_molecule_database`。

**入参 schema**：

| 字段 | 类型 | 约束 | 说明 |
|---|---|---|---|
| `constraints` | array of object | **必填**（可为空数组）；每条含 `property`(string)、`op`(enum: lt/le/gt/ge/eq/ne)、`threshold`(number 或 string)，三字段必填 | 结构化约束；比较口径由 property_spec 定 |
| `keywords` | string 或 null | **必填**（可为 null） | 自然语言关键词；与 constraints 至少给一 |
| `k` | integer | 1–50，默认 50 | 返回上限（task-07 §1 默认 10→50，与单题全局调用上限 50 同批联动：92,153/50≥1,844 次翻页≫50，穷举不可行，必须约束过滤） |

> strict schema 注：`constraints` 与 `keywords` 双键必填是 strict JSON 的形态要求（nullable 表达可选），语义上"二者至少给一"，全空调 `INVALID_ARGUMENT`。详见 §5 核对记录 2。

**出参 schema**：

| 字段 | 类型 | 说明 |
|---|---|---|
| `returned` | integer | 返回条数 |
| `candidates[]` | array（≤50 条） | 每条含 `id`(string，匿名实体 ID，唯一可被后续工具引用的键)、`entity_type`(enum: molecule/crystal)、`structure_hint`(string 或 null，仅结构类字段，掩码必须不可逆，承 INTERFACE_CONTRACT §5 前提) |

**错误语义**：`INVALID_ARGUMENT`、`EMPTY_RESULT`、`BUDGET_EXCEEDED`、`SOURCE_DENIED`（【保留未启用】：v1 网关无产生路径，见 §2.4 与 meta.unified_error_codes）。

**行为红线（登记在案的修复要求）**：无命中一律返 `EMPTY_RESULT`，**严禁 fallback 返回全库或任何默认集合**——既有 `search_molecule_database` 的 fallback 全库对 open 类 A/B 分层构成捷径（ADR-0011 决策 3 与本条方向相反，待监工裁定，见 progress.md §3）。返回字段不得含任何性质数值，否则 cp3 无意义。

### 1.3 `get_entity`（A · free）

- **功能一句话**：按 ID 查询单个实体的结构/身份可见字段（存在性确认），不返回任何性质数值。
- **aliases**：`get_cif_by_formula`、`get_or_generate_cif`、`download_poscar`、`preview_structure_file`、`poscar_to_cif`。

**入参 schema**：

| 字段 | 类型 | 约束 | 说明 |
|---|---|---|---|
| `id` | string | **必填** | 实体 ID（`search_candidates` 返回的匿名 ID） |

**出参 schema**：

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | string | 回显 |
| `entity_type` | enum: molecule/crystal | 实体类型 |
| `structure` | object | 三字段必填：`formula`(string 或 null，匿名域化学式)、`masked_structure`(string 或 null，masked SMILES/CIF 键，掩码规则不可逆)、`atom_count`(integer 或 null) |

**【atom_count 口径变更登记，任务 #42】**：分子侧 `atom_count` = E1 backend 落盘口径（全原子计数，含氢），任务 #36 数据源切换起替代旧重原子口径（RDKit GetNumHeavyAtoms）——同一分子两口径不等值（如实测分子 aa：旧口径=19，backend 口径=44）；消费方（case_build 格子切分等）按现行口径取值，不得写死旧口径区间。同批变更：分子侧 formula/masked_structure 均透传 E1 backend 匿名身份档（get_entity_A.jsonl），formula 不得现算（dummy atom 无隐式 H 会丢氢，E1-03 交接口径）；backend 不在场回落 RDKit 派生旧口径。

**错误语义**：`INVALID_ARGUMENT`、`UNKNOWN_ENTITY`（不区分"不存在"与"不在本源"，防源探测，见 §3.2）。

**行为红线**：旧结构文件类工具（CIF/POSCAR 取件与预览）收敛为本工具 + `get_property`（crystal 性质线）；AI 生成类入口（`get_or_generate_cif` 的 generate 分支、`crystal_generate`、`generate_molecules`）在 v1 不提供——agent 的候选只能来自检索与推理编辑，不来自服务端生成。

### 1.4 `get_property`(A · verify_budget）

- **功能一句话**：按 ID 查询实体的指定性质，返回快照真值；计查证预算，是 cp3 性质查证与 grounded() 判定的唯一数据源。
- **aliases**：`predict_admet`、`crystal_predict`。

**入参 schema**：

| 字段 | 类型 | 约束 | 说明 |
|---|---|---|---|
| `id` | string | **必填** | 实体 ID |
| `props` | array of string | **必填**，≥1 项 | 性质名列表；合法性由引擎按 case.props 校，非法返 `UNKNOWN_PROPERTY` |

**出参 schema**：

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | string | 回显 |
| `values[]` | array of object | 每条含 `name`(string) 与 `value`(number 或 string：布尔用 1/0，类别型如 condition=ambient 用字符串，承 INTERFACE_CONTRACT §3.2) |

**错误语义**：`INVALID_ARGUMENT`、`UNKNOWN_ENTITY`、`UNKNOWN_PROPERTY`、`BUDGET_EXCEEDED`（查证预算耗尽）、`SOURCE_DENIED`（【保留未启用】：v1 网关无产生路径——B 侧实体 ID 对 agent 不可达，未知实体一律归一 `UNKNOWN_ENTITY`，不得引为隔离支柱）。

**行为红线**：返回值即真值（ACTION_RULES §6）；每次调用落录像行，`trace.observed[(c,p)]` 与 `grounded(c,p,v)` 由此派生；open 类中本工具只查 A 侧——B 侧性质复核仅在评分期由评测机执行，agent 永不可调用。

### 1.5 `compare`（none · free）

- **功能一句话**：对给定实体 ID 列表按指定性质键排序（便利工具，INTERFACE_CONTRACT §3.1 标注可省），支撑 cp4/cp5。
- **aliases**：`compare_materials`。

**入参 schema**：

| 字段 | 类型 | 约束 | 说明 |
|---|---|---|---|
| `ids` | array of string | **必填**，≥2 项 | 待排序实体 ID |
| `key` | string | **必填** | 排序键性质名（须为该批实体已查得的性质） |
| `ascending` | boolean | 默认 false | 是否升序 |

**出参 schema**：

| 字段 | 类型 | 说明 |
|---|---|---|
| `ordered_ids` | array of string | 排序后的 ID 序列 |

**错误语义**：`INVALID_ARGUMENT`、`UNKNOWN_ENTITY`、`PROPERTY_NOT_VERIFIED`（task-02 轻 3：目标实体存在但未经 get_property 查证 key 性质；与 UNKNOWN_ENTITY 语义区分——**存在性不可知 vs 性质未查证**，agent 可据此补查证；gateway.py 代码侧并行落地中，文案以实际返回为准二次校准）。

**行为红线**：输入 IDs 必须是 agent 已通过 `search_candidates`/`get_property` 触达过的实体，不引入新信息；省略本工具时 cp5 由 agent 自行排序，判分口径不变。

### 1.6 `submit_answer`（none · free）

- **功能一句话**：提交结构化答卷（payload 须通过 `dag_poc/answer_schema.json` 校验），终态收卷工具，支撑 cp6。
- **aliases**：无。

**入参 schema**：

| 字段 | 类型 | 约束 | 说明 |
|---|---|---|---|
| `payload` | object | **必填** | 符合 answer_schema.json 的完整答卷对象（candidates/ranking/answer + open 扩展三字段） |

**出参 schema**：

| 字段 | 类型 | 说明 |
|---|---|---|
| `accepted` | boolean | 是否受理 |
| `errors` | array of string | `SCHEMA_VIOLATION` 时给出字段级错误列表（仅语法错，允许一次重交） |

**错误语义**：`SCHEMA_VIOLATION`（payload 未过 answer_schema 校验；允许一次仅回语法错的重交，承 INTERFACE_CONTRACT §3.2 纪律 3）、`DUPLICATE_SUBMISSION`（终态只允许一次成功提交）。

**行为红线**：三通道收口之一（API structured output / 本工具 / 本地校验）；值域（含 NO_SOLUTION）不在本层判，由 cp6 logic 判。

### 1.7 `check_molecule_exists`（existence-only · existence_budget）

- **功能一句话**：open 类存在性接口：判定 agent 推断的分子是否存在；返值严格等价 m ∈ B，只返单个 bool。
- **aliases**：无（新增工具）。

**入参 schema**：

| 字段 | 类型 | 约束 | 说明 |
|---|---|---|---|
| `molecule_id` | string | **必填** | agent 推断出的分子标识（匿名域规范化后的结构标识；可直接用 `edit_molecule.result_id`）。【任务 #36 更新】钥匙空间为 ADR-0017 dummy atom canonical SMILES（E1-03 映射表匿名化构造）；旧 ed: 匿名编辑 token 已迁出（exists 恒 false），旧真实 SMILES 不再是 B 成员形态（任务 #36 后 b_partition 顶层已全换匿名 canonical，真实 SMILES 查询实跑 exists=false）；E5 冻结 G_B 入库后收敛为 InChIKey/canonical_smiles 双身份键 |
| `attempt` | integer | **可选元数据**，1–10 | 【任务 #8 C1 降级】agent 自报的第几次尝试，仅回显不参与预算；**预算以服务端真实调用计数为权威**（`existence_used += 1`），超限返 `ATTEMPTS_EXCEEDED`；自报值越界（非 1..10 整数）返 `INVALID_ARGUMENT` |

**出参 schema**：

| 字段 | 类型 | 说明 |
|---|---|---|
| `exists` | boolean | 严格等价 m ∈ B；**A 内分子与库外分子一律 false** |
| `attempt` | integer | 服务端真实调用计数（非 agent 自报值，C1） |
| `attempts_remaining` | integer（≥0） | 剩余尝试次数 |

**错误语义**：`INVALID_ARGUMENT`、`ATTEMPTS_EXCEEDED`（超限该 case 记 0 分，attempts 计入指标，goal §2b⑤/§6 步 6；错误信封附 exists/attempt/attempts_remaining/exhausted 诊断字段）。

**行为红线**：output schema 结构上不含任何性质/相似度/距离字段，且 `additionalProperties=false` 封死泄漏通道；三概念分离见 §3.3。

### 1.8 `edit_molecule`（none · free）

- **功能一句话**：open 类结构编辑工具：对 A 侧锚点分子施加封闭算子集内的单步结构编辑，产出候选推断分子（纯计算，不查库）。
- **aliases**：无（新增工具）。

**入参 schema**：

| 字段 | 类型 | 约束 | 说明 |
|---|---|---|---|
| `anchor_id` | string | **必填** | 锚点分子 ID，必须 ∈ A（否则 `ANCHOR_NOT_IN_A`，对应 grounding 条件②） |
| `operator` | enum | **必填** | 封闭算子集：`add_atom` / `remove_atom` / `replace_substituent` / `substitute_element` / `ring_closure`（goal §2b④；超出返 `OP_OUT_OF_SET`，配合跳数≤2 约束） |
| `detail` | string 或 null | **必填** | 算子参数（如所加原子/所换基团的匿名域记号） |

**出参 schema**：

| 字段 | 类型 | 说明 |
|---|---|---|
| `result_id` | string | 编辑产物规范化标识（可直接作为 `check_molecule_exists` 的入参）。【任务 #36 更新，单口径】实体 ID 与匿名域 SMILES 锚点均产 RDKit canonical SMILES（ADR-0017 dummy atom 载体；施加失败时回落锚点 canonical，不编造产物）；ed: 占位锚点非合法 SMILES → ANCHOR_NOT_IN_A，不再产 ed: token（任务 #32 ③） |
| `normalized_structure` | string | 规范化结构表示（与 `normalize_structure` 同口径，同 result_id） |
| `applied` | boolean | 【修复五后现状：已真实化】真实分支走 RDKit RWMol 编辑 + SanitizeMol 校验，不再恒 True；结构上不可施加时 false，不算错误码 |

**错误语义**：`INVALID_ARGUMENT`、`ANCHOR_NOT_IN_A`、`OP_OUT_OF_SET`（超限 case 废弃，goal §2b④）。

**行为红线**：纯结构计算，不访问 A/B 任何库、不返回任何性质；答卷 `edit_op` 字段的工具侧对应物——grounding 条件③（`edit_op(anchor)` 规范化后 == 提交分子）用本工具录像 + `normalize_structure` 复核。

**【修复五移交登记；任务 #36 更新：grounding 条件③已实现并经真实数据验证】**：grounding 条件③（edit_op(anchor) 规范化后 == 提交分子）在 ADR-0017 匿名层端到端验证通过（证据：dag_poc/mcp_mock_server/rehearsal_logs/grounding_condition3_realdata.log + 测试 test_grounding_condition3_realdata_entity_id_anchor / test_grounding_condition3_realdata_anon_smiles_anchor，2026-08-07 实跑三判据全 True）。本工具各分支现行口径：① ed: 前缀锚点非合法 SMILES → ANCHOR_NOT_IN_A（任务 #32 ③占位直通移除）；② 实体 ID 锚点 applied 走 RDKit 真实判断，result_id 为 RDKit 产物 canonical SMILES（任务 #32 ②，任务 #36 后为匿名域 dummy atom 形态）；③ SMILES 锚点须为 A 分子 canonical（匿名域 dummy atom SMILES）或本会话编辑产物（条件②前置校验），add_atom/remove_atom 真实施加，**replace_substituent/substitute_element/ring_closure 三算子在真实分支恒 applied=false**（RDKit 不能简洁实现，诚实返 False，此口径边界保留）。

### 1.9 `normalize_structure`（none · free）

- **功能一句话**：open 类规范化校验工具：把任意结构表示规范化为匿名域唯一形式（等价匿名变换下的同一性判定），纯计算不查库。
- **aliases**：无（新增工具）。

**入参 schema**：

| 字段 | 类型 | 约束 | 说明 |
|---|---|---|---|
| `structure` | string | **必填** | 待规范化的结构表示（匿名域 SMILES/式子） |

**出参 schema**：

| 字段 | 类型 | 说明 |
|---|---|---|
| `canonical_id` | string | 规范化后的唯一标识。【任务 #36 更新，单分支】合法 SMILES（含 ADR-0017 dummy atom 载体）→ RDKit canonical（匿名域一步不动点）；非法输入（垃圾串/ed: 占位 token）→ valid=false，canonical_id 回落字符串口径（strip+空白折叠）仅供诊断，合法性以 valid 为唯一判据 |
| `valid` | boolean | 【任务 #36 更新】= RDKit 解析成败（垃圾串/非法结构/ed: 占位 token 一律 false，落实 §1.9 契约；任务 #31 收紧后无独立占位分支，静默 fallback 移除） |

**错误语义**：`INVALID_ARGUMENT`。

**行为红线**：等价匿名变换（ADR-0012 §3）下的同一性判定——性质不变、每个真实分子对应唯一匿名标识；评分侧用同一规范化函数核对 `edit_op(anchor)` == 提交分子。**实现一致性要求**：网关与评分引擎必须调用同一个规范化函数实例（gateway.py `canonical_chemical`，修复四 evaluate_grounding 已复用），否则 grounding 条件③的两侧口径漂移。

**【修复五移交登记；任务 #36 更新：grounding 条件③已实现并经真实数据验证】**：grounding 条件③的规范化复核在 ADR-0017 匿名层端到端验证通过（证据：dag_poc/mcp_mock_server/rehearsal_logs/grounding_condition3_realdata.log + 测试 test_grounding_condition3_realdata_entity_id_anchor / test_grounding_condition3_realdata_anon_smiles_anchor，2026-08-07 实跑三判据全 True）。现行口径：ed: 前缀 token 非合法 SMILES → valid=false（任务 #31 收紧，静默 fallback 移除）；真实分支（RDKit canonical，含 ADR-0017 dummy atom 载体）同分子不同写法归一到同一标识（契约同构等价，匿名域一步不动点）。

---

## 2. 统一 MCP 调用约定

**小结：命名、参数、返回、错误码、录像、reward 引用六件事全部统一；现有 23 个灵枢工具各写一套的历史（见 TOOLS_INVENTORY 八类各异出入参）在本节约束下收敛为单一规范。**

### 2.1 命名规范

| 规则 | 内容 |
|---|---|
| 规范名 | 小写 snake_case，动宾结构（`search_candidates`、`get_property`）；以桩 `tools[].name` 为唯一合法名 |
| alias 归一 | 既有 23 旧名经桩 `alias_map` 解析到规范名；**录像落盘与判分读取前必须先归一**，历史 trace 仍可解析；标 `DEPRECATED` 的旧名无新契约对应物，历史可解析但不参与新 case 判分 |
| 禁新增 | 未在桩 `tools[]` 声明的工具名一律拒绝注册（引擎加载期断言） |

### 2.2 参数风格

| 规则 | 内容 |
|---|---|
| 单一入参对象 | 所有工具入参为单个 JSON object；不设位置参数、不设查询串 |
| strict 形态 | 每层 `additionalProperties=false`；全部 property 进 `required`；可选值用 nullable 表达（承 answer_schema.json `strict_note` 同款纪律） |
| 默认值 | 仅 schema 内声明的 `default` 生效（如 `max_results` 默认 5、`k` 默认 50）；网关不得发明 schema 外默认 |
| 实体引用 | 一律用匿名 ID（`search_candidates`/`edit_molecule` 产出）；禁用真实分子名/公式作引用键（匿名域纪律） |

### 2.3 返回结构

| 规则 | 内容 |
|---|---|
| 成功 | 直接返回该工具 output schema 定义的 object，**不加 envelope**（不套 `{success, data}`——避免与既有旧工具 `success` 字段风格混淆；新旧口径切换由 alias 归一层承担） |
| 失败 | 返回统一错误信封：`{"error_code": <枚举>, "error": <一句话人类可读说明>}`（修复六回写：字段名为 `error` 非 `message`，以 gateway.py `_err` 实测为准）；`error_code` 必须属于该工具 `errors` 声明集；部分错误码附诊断字段（如 `ATTEMPTS_EXCEEDED` 附 exists/attempt/attempts_remaining/exhausted，`SCHEMA_VIOLATION` 附字段级 errors 列表） |
| schema 表达 | 每工具 output 为 `oneOf`：成功形状（逐字段冻结自 v0.1.0-stub）∪ 统一错误信封（修复六，tools_schema 已落）；录像行 `ok` 字段 = ret 无 error_code |
| 不混合 | 一次调用要么成功出参、要么错误码，不允许"部分成功"（`props` 中混入非法性质名时整调用返 `UNKNOWN_PROPERTY`，不静默忽略） |

### 2.4 统一错误码表（13 个，收敛分散实现）

出处：桩 `meta.unified_error_codes`；"适用工具"列由桩各 tool `errors` 字段汇总。

| 错误码 | 语义 | 适用工具 |
|---|---|---|
| `INVALID_ARGUMENT` | 入参缺失/类型错/越界（如 max_attempts 超上限） | 全部 9 个 |
| `UNKNOWN_ENTITY` | 实体 ID 不存在于该工具所属源（**不区分"不存在"与"不在本源"，防源探测**） | search_candidates、get_entity、get_property、compare |
| `PROPERTY_NOT_VERIFIED` | compare 目标实体存在且已触达、但未经 get_property 查证所请求性质。**与 UNKNOWN_ENTITY 的语义区分：存在性不可知 vs 性质未查证**——前者 agent 无从补救（防源探测不告知去向），后者 agent 可据此补查证（task-02 轻 3；gateway.py 代码侧并行落地中） | compare |
| `UNKNOWN_PROPERTY` | 性质名不合法（引擎按 case.props 校，承 INTERFACE_CONTRACT §3.2） | get_property |
| `BUDGET_EXCEEDED` | 查证/检索预算耗尽（get_property 计查证预算、search_candidates/literature_search 计检索预算） | literature_search、search_candidates、get_property |
| `ATTEMPTS_EXCEEDED` | check_molecule_exists 超过 max_attempts——该 case 记 0 分，attempts 计入指标（goal §2b⑤） | check_molecule_exists |
| `EMPTY_RESULT` | 检索无命中。**严禁 fallback 返回全库/任意默认集合** | literature_search、search_candidates |
| `SOURCE_DENIED` | 试图以 B 侧实体/性质为参数调用 A 侧或存在性工具。【保留未启用】：v1 网关无产生路径（B 侧实体 ID 对 agent 不可达，未知实体一律归一 UNKNOWN_ENTITY 防源探测），不得引为隔离支柱；保留供 E1 物理分层后的网关层拦截使用 | search_candidates、get_property（仅声明，未启用） |
| `DUPLICATE_SUBMISSION` | submit_answer 重复交卷（终态只允许一次成功提交） | submit_answer |
| `SCHEMA_VIOLATION` | submit_answer 的 payload 未过 answer_schema.json 校验（允许一次仅回语法错的重交） | submit_answer |
| `ANCHOR_NOT_IN_A` | edit_molecule 的 anchor 不属于 A（grounding 条件②的工具侧前置校验） | edit_molecule |
| `OP_OUT_OF_SET` | edit_op 算子超出封闭算子集或跳数 >2（goal §2b④，超限 case 废弃） | edit_molecule |
| `CALL_LIMIT_EXCEEDED` | 单题工具调用全局上限 50 次耗尽（task-07 §5，方案定稿 PR-17：按 case 全局计数，`submit_answer` 交卷豁免；超限调用仍落录像行 ok=false，触顶率统计数 trace 中本码行即可，不得静默失败） | 全部 9 个（全局闸；submit_answer 豁免） |

### 2.5 录像行（trace 的最小单元）

出处：桩 `meta.recording.row_format`；符号定义承 `dag_poc/docs/INTERFACE_CONTRACT.md` §3.3 `trace.calls`。

每次工具调用（无论成败）记一行：

```
{t, tool, args, ret, ok}
```

| 字段 | 含义 |
|---|---|
| `t` | 自任务开始的整数序号（单调递增，evidence_time 的取值域） |
| `tool` | 本 schema 的规范 name（alias 名落盘前归一） |
| `args` | 入参对象（原样） |
| `ret` | 出参对象；失败时为统一错误结构 |
| `ok` | bool：无错误码即 true |

由录像直接派生的判分符号（承 INTERFACE_CONTRACT §3.3 绑定表，此处不重复定义）：`trace.search_calls`、`trace.queried`、`trace.docs`、`trace.observed[(c,p)]`、`grounded(c,p,v)`、`evidence_time(cp)`。

**evidence_time 判据口径（修复一后权威语义，本文与代码同步）**：`evidence_time(cp)` = 支撑该 cp 的录像事件中**首次建立时刻**（成功调用，聚合取 **min**）——拓扑序问的是“依赖是否在被使用前先建立”，先建立即合法，后续回访/复核不推翻已建立的依赖（原 max“最晚时刻”口径会误杀“结论后回访再查文献”的迭代式探索轨迹，经监工验收意见裁定改 min）。evidence_map 每项可配 `agg: 'min'|'max'`（缺省 min，policy 一行可回退）；返回 `[t, prio]` 元组，同时刻按管线次序 prio 破平局（cp4 fallback_prio=1 机制不变）。实现单一来源：dag_poc/gateway.py `compute_evidence_time`，evaluate.py 复用不双实现。

### 2.6 reward 引用规范

出处：桩 `meta.recording.reward_reference`；格式承 `dag_poc/data/TOOLS_INVENTORY.md` 尾部引用规范（key 的可查表由旧表切换为本 schema）。

```
{tools.<name>.in.<param>}       — 该工具某次调用的入参值
{tools.<name>.out.<key>}        — 该工具某次调用的出参顶层 key
{tools.<name>.out.<key>.<sub>}  — 出参的嵌套 key（如 {tools.literature_search.out.papers.doc_id}）
{tool_call_count.<name>}        — 该工具被调用的总次数
{tool_call_ids.<name>}          — 该工具涉及的实体 ID 集合（从入参提取）
```

三条硬纪律：

1. `name` 与 `key` 必须在桩 `tools[].input/output` 中可查——引擎加载 template 时凡引用不在表内即报错，不允许运行（承 TOOLS_INVENTORY "违规即拒绝执行"同款门）。
2. alias 名只能出现在历史 trace 解析路径，不得出现在新 template 的 reward 表达式中。
3. 旧 TOOLS_INVENTORY 的 key 表（如 `predict_admet` 的 `results[].half_life_days`）仅服务迁移期历史 trace；新 case 一律引用规范工具的字段（如 `get_property.out.values`），字段漂移对照见 §5 核对记录 1。

### 2.7 MCP 运行态（修复二交付，gateway_mcp.py）

**小结：新契约 9 工具已有独立 MCP 运行面（端口 9001），legacy 23 工具面（端口 9000）冻结不动；E4 harness 的 MCP 通道指向 9001。**

出处：`dag_poc/mcp_mock_server/gateway_mcp.py` 与 `mock_server.py` 两文件头部职责声明（修复二/任务 #15 交付）。

| 项 | 内容 |
|---|---|
| 新契约端点 | `gateway_mcp.py`，FastMCP server，默认端口 **9001**（`GATEWAY_MCP_PORT` 可改）；每工具是对进程内 `Gateway` 实例的薄包装（只转调、零语义改写，入参/出参 JSON 字符串透传） |
| 名单防线 | 数据驱动注册：注册名集合必须 == tools_schema.json `tools[].name` 声明集合（`verify_registered_names` 启动断言，未声明者不得注册、缺声明者启动失败） |
| legacy 端点 | `mock_server.py` 端口 **9000 冻结不动**：旧 23 工具名与签名不新增、不改行为、不接新 case（37 既有 case 的 trace 与 ADR-0011 决策 3 fallback 口径依赖它）；退场三条件见 gateway_mcp.py 头部声明（37 case 按新契约重跑归档 + 监工对 ADR-0011 裁定落盘 + evaluate.py 不再依赖 legacy 解析路径） |
| E4 指向 | **E4 harness 的 MCP 通道应指向 9001**（gateway_mcp.py 头部明文）；新 case 一律走新契约端点 |
| 会话态 | v1 进程内单会话（单 agent 单 case，goal §3 硬边界足够）；可选入参 session_key 仅透传留痕不参与逻辑；按 case 装载 B 分区用 `DAGPOC_CASE_ID` 环境变量（修复三参数） |
| 行为差异 | 9001 的 search_candidates 无命中恒 `EMPTY_RESULT`（禁全库 fallback）；9000 受 `MCP_STRICT_EMPTY_RESULT` 开关控制（默认保 ADR-0011 返全库，见 §1.2 行为红线登记） |

---

## 3. A/B 源分层设计（重点章节）

**小结：open 发现探索类的全部可信度押在这一节——A 可查、B 完全不可查、存在性接口只吐单 bool；验收硬标准是"仅用 A 源 tools 穷举求解无法得到 gt"，隔离测试方案见 §3.5。**

### 3.1 source_tier 体系

出处：桩 `meta.source_tier_legend` 与各 tool `source_tier` 字段；权威定义 ADR-0012 §1 第二类。

| tier | 定义 | 工具 | 数量 |
|---|---|---|---|
| `A` | agent 可查库（固定快照/匿名 A 库），返回值即真值（ACTION_RULES §6） | literature_search、search_candidates、get_entity、get_property | 4 |
| `existence-only` | 唯一可触达 B 成员关系的通道，只返存在性 bool，不返任何性质/相似度/距离 | check_molecule_exists | 1 |
| `none` | 纯计算/收卷工具，不触达任何库 | compare、submit_answer、edit_molecule、normalize_structure | 4 |
| `B` | agent 不可查库——**本 schema 中不存在任何直接暴露 B 内容的工具** | （无） | **0** |

设计不变量：**任何从 agent 视角可达的调用路径，其返回值的每一个字段，都不得承载 B 侧内容**（存在性 bool 本身除外，且它只承载"m ∈ B 与否"这一个 bit）。

### 3.2 B 库物理隔离原则

出处：goal §2b②（"需后端库物理分层，A 的任何查询不得带出 B 内容"）。

| 原则 | 落法 |
|---|---|
| 存储分片 | A、B 两个独立数据分片（文件/表级分离）；网关的 A 侧工具只装载 A 分片，B 分片句柄只存在于评测机进程内（评分期 `hit(G_B)` 判定与 B 侧性质复核使用），agent 可达进程不可装载 |
| 参数侧拦截 | 【修复六回写口径】`SOURCE_DENIED` 属【保留未启用】：v1 网关无产生路径（B 侧实体 ID 对 agent 不可达，未知实体一律归一 `UNKNOWN_ENTITY`），不得引为隔离支柱；隔离的实际支柱是：存储分片（本行）+ B∩A=∅ 加载断言 + 可查性质白名单 + `UNKNOWN_ENTITY` 不区分源归属（本表其余三行）；`SOURCE_DENIED` 保留供 E1 物理分层后的网关层拦截使用 |
| 不泄漏成员关系 | `UNKNOWN_ENTITY` 不区分"实体不存在"与"实体不在本源"——若对 A 侧查询回"该 ID 在别的库里"，即构成 B 成员关系泄漏（源探测攻击） |
| 不泄漏全库 | `search_candidates` 无命中返 `EMPTY_RESULT`，禁 fallback 全库（§1.2 行为红线）——全库倾倒即使只倒 A 也违反 goal §2b②"A 的任何查询不得带出整库内容"，且对 open 类构成绕过约束检索的捷径 |
| 存在性接口最小面 | `check_molecule_exists` 是 B 的唯一开口，且开口尺寸 = 1 bit/次、≤10 次（§3.3、§3.4） |

### 3.3 `check_molecule_exists` 的三概念分离

出处：goal §2b⑤ 表（"⑤ 的三个独立概念（混用即设计失效）"）；桩 behavior_notes 逐字对应。**三个概念必须分离，混用即设计失效**：

| 概念 | 定义 | 工具侧落点 |
|---|---|---|
| **停止信号** | `exists=true` → agent 可结束本轮推断 | 出参 `exists` 字段本身 |
| **接口语义（定义域，写死）** | 返值严格等价 `m ∈ B`：**A 内分子与库外分子一律返"不存在"**；返回单个 bool，不带任何性质/相似度/距离。若对 A 内分子返"存在"，agent 直接交 A 中分子即过，**整个设计崩塌** | output schema 仅 `exists`/`attempt`/`attempts_remaining` 三字段 + `additionalProperties=false` 结构性封死 |
| **评分依据** | 推断分子 ∈ `G_B`（造题期落盘的 B 侧合格集）**且**经 B 侧性质复核满足题面目标窗口；评分由评测机在 B 侧执行，**不经本工具** | 评分期逻辑，不在 agent 可达面 |

总闸门公式（唯一口径，goal §2b）：

$$\text{open\_score} = \mathbb{1}[\text{hit}(G_B)] \times \mathbb{1}[\text{grounding\_pass}]$$

其中：

- \(\mathbb{1}[\cdot]\)：指示函数，条件成立取 1、不成立取 0；
- \(\text{hit}(G_B)\)：agent 提交的推断分子属于造题期落盘的 B 侧合格集 \(G_B\)（gt + 同等合格者，goal §2b①），且经 B 侧性质复核满足题面目标窗口——**不是"命中 B 中任意分子"**；
- \(\text{grounding\_pass}\)：产物落地验证通过，即三条均成立——① 答卷 `cited_values` 每条与 trace 中真实 `get_property` 返回在容差 `tol[prop]` 内一致；② `anchor_molecules` 每个 ∈ A 且至少 1 个；③ `normalize(edit_op.anchor, edit_op.operator, edit_op.detail) == answer`（规范化口径 = `normalize_structure.canonical_id`）；三条的字段→判据映射见 `answer_schema.json` `open_grounding_checks`；
- 乘法关系：两者缺一则总分为 0；存在性预算耗尽未命中也记 0。

**工具与判分的分工**：`check_molecule_exists` 只贡献"停止信号"这一个概念；`hit(G_B)` 与 `grounding_pass` 都在评分期计算，agent 拿不到其中任何中间量。若把"接口返存在"直接当得分依据，即构成自证循环并可被暴力枚举攻破（goal §2b⑤ 明文警告）。

### 3.4 max_attempts ≤ 10

出处：goal §2b⑤ 尝试预算条文（"建议 ≤10，超限记 0 分，attempts 计入指标"）；桩已把建议写死为硬上限。

| 要素 | 契约 |
|---|---|
| 上限 | `attempt` 字段 maximum=10；第 11 次调用返 `ATTEMPTS_EXCEEDED`，该 case 记 0 分 |
| 计量 | 每次成功调用（无论 exists 真假）消耗 1 次；`attempts_remaining` 回显剩余量 |
| 报表 | 平均 attempts 随矩阵报告附报（goal §6 步 6） |
| 与暴力枚举的关系 | 10 次尝试相对匿名域分子结构空间可忽略；枚举刷分在数学上不可行——这是"停止信号≠评分依据"之外的第二道防枚举闸 |

### 3.5 验收硬标准的隔离测试方案（已真实现：真穷举求解器 + 投毒负对照）

验收硬标准（task.md §3 子目标 5）：**仅用 A 源 tools 穷举求解，无法得到 gt**。本节给出隔离测试的设计与断言清单；**实现已落地**：`dag_poc/mcp_mock_server/tests/test_gateway_isolation.py`（Jimmy 任务 #4 实现，修复三/任务 #13 改造为可证伪版：真穷举求解器筛选排序→产出提交集 S→断言 S∩G_B=∅，另加投毒负对照证明断言有判别力；指路：测试函数前缀分组 `isolation_`（A/B 隔离）/ `path_`（no_path_violation）/ `contract_`（契约行为），A1 对应 `isolation_exhaustive_reachable_disjoint` / `isolation_solver_submission_disjoint_gb`，投毒负对照对应 `isolation_poisoned_solver_must_fail`）。

**测试构造**：

1. **输入**：任一 open 类 case（含落盘的 `G_B`、A/B 分片、锚点与线索配置）；
2. **被测对象**：一个"穷举求解器"——只允许调用 `source_tier ∈ {A, none}` 的工具（即 8 个：除 `check_molecule_exists` 外的全部；存在性接口虽可触 B 的 1 bit，为测试严格性默认禁用，另做单独一组开启对照）；预算放宽到足以遍历 A 全域（对齐 goal §2b④第一条"A 侧支持全域穷举"）；
3. **求解器行为**：枚举 `search_candidates` 可达的全部 A 实体 → `get_property` 查证全部性质 → 按题面目标筛选排序 → 产出其认为最优的提交分子集合 S。

**通过断言（全部成立才算隔离合格）**：

| # | 断言 | 抓什么风险 |
|---|---|---|
| A1 | S ∩ `G_B` = ∅（穷举 A 得到的任何提交都不属于 B 侧合格集）。**已真实现**：真穷举求解器 + 投毒负对照（把 B 成员伪造进 A 侧返回，断言必须失败，证明隔离断言非恒真），见 tests `isolation_` 前缀分组 | A 侧无替代解（goal §2b④第一条的同构检验） |
| A2 | A 侧任一工具的任一返回字段，经逐字段扫描不含任何 B 成员的身份/结构/性质 | B 内容无泄漏通道（§3.1 不变量的执行级验证） |
| A3 | 以 B 侧实体 ID 为参数调用 A 侧工具，一律返 `UNKNOWN_ENTITY`（v1 实测口径：`SOURCE_DENIED` 保留未启用无产生路径，见 §2.4），且不区分源归属 | 源探测攻击被挡 |
| A4 | 对随机 A 内分子与随机库外分子调用 `check_molecule_exists`，exists 一律 false；对 `G_B` 成员一律 true | 存在性接口的定义域严格 = B（§3.3 接口语义） |
| A5 | 对存在性接口做 10 次随机猜测枚举，命中 `G_B` 的概率为 0（相对匿名域结构空间） | 暴力枚举不可行（§3.4） |

**失败处置**：A1 不成立 → 该 case 按 goal §2b④ 废弃；A2–A5 不成立 → 网关实现有缺陷，阻断发布。测试结果须随 case 批次落盘存档（可复跑），作为验收⑤"可执行验证，非'有专节'即可"的证据。

---

## 4. 与 ADR-0011 现存答卷契约的映射

**小结：`answer_schema.json` 与 ADR-0011 旧契约（evaluate.py 消费的 `candidate_result.screened_pool` / `ranked_candidates`）不是两套口径，而是新 schema 的单向机械派生；迁移期不动 evaluate.py，收敛后引擎只读新 schema。**

出处：`dag_poc/answer_schema.json` `legacy_contract_mapping`（本节为其转述并补理由）。

| 旧契约字段 | 派生式（自 answer_schema 字段） | 服务的 cp |
|---|---|---|
| `candidate_result.screened_pool` | `[c.id for c in candidates]`——考察过的全部候选（池，越全越好） | cp2/cp3 按此评 |
| `candidate_result.ranked_candidates` | `ranking` 中满足对应候选 `reject == null` 的 ID 序列，从优到劣，第一个为最终推荐 | cp4/cp5/cp6 按此评 |
| cp3 查证对账 | `candidates[].measurements` 即 `sub.value[c][p]`；迁移后 cp3 对账改从答卷取声称值再与 `world.truth`/`trace.observed` 比对（现 evaluate.py 直接从 trace 的 predict_admet→get_property 返回取值，二者口径在 grounded() 定义下收敛） | cp3 |
| 兼容规则 | 旧答卷无 `screened_pool` 时回落为 `ranked_candidates`（evaluate.py 现行行为保留）；新答卷一律按本 schema 提交 | — |

**为何这样映射（补理由）**：

1. **单向派生消灭第二套口径**：若新旧各维护一套解析逻辑，字段漂移时判分会静默分叉；派生式是可机械执行的（承 progress.md §4.5 两套口径检查结论），evaluate.py 收敛后无需双解析分支。
2. **旧字段是新字段的投影，信息只减不增**：`screened_pool` 丢了 measurements 与 reject 归因——这正是新 schema 多出来的过程量原料（cp3 对账、cp6 归因），旧契约表达不了，故映射方向只能旧←新，不能反向。
3. **迁移期不阻塞评测**：现有 37 个 case 的判分继续读旧契约；新 case 按新 schema 提交，引擎侧按上表派生旧字段即可回放旧评测管线。

---

## 5. 与 schema 桩的一致性核对记录

**小结：逐字段核对 9 个工具的 name/description/source_tier/budget/aliases/input/output/errors 与桩一致，未发现实质不一致；记录 3 处口径收敛留痕（均以 goal.md 与桩为准），1 处待监工裁定的既有事项仅引用。**

| # | 核对点 | 现象 | 处理（以 goal.md 与桩为准） |
|---|---|---|---|
| 1 | `TOOLS_INVENTORY.md` 旧出参字段与桩出参不完全重合 | 旧 `literature_search` 出参含 `authors`/`doi`/`open_access_url`/`is_open_access`，桩收敛为 `doc_id`/`title`/`year`/`citation_count`；旧 `predict_admet` mock 模式出参（`total_evaluated`/`results[]` 等）在桩 `get_property` 中收敛为 `values[{name,value}]` | 非冲突：旧表服务历史 trace 解析，新契约以桩为准；reward 引用的可查表由旧表切换到桩（§2.6 纪律 3）。已在此留痕，无需改桩 |
| 2 | INTERFACE_CONTRACT §3.1 记 `search_candidates(constraints, k)`，语义"约束或关键词至少给一" | 桩的 strict schema 把 `constraints` 与 `keywords` 均列为 required（keywords 可 null），是 strict JSON 的形态收紧，语义未变 | 按桩执行（§1.2 加注）；判定为形态差异非语义不一致 |
| 3 | INTERFACE_CONTRACT §3.1 标 `compare` 为"便利工具，可不给" | 桩保留 `compare`（source_tier=none，free） | 一致：保留不引入新信息，省略时判分口径不变（§1.5 行为红线），两种口径兼容 |
| 4 | ADR-0011 决策 3 "fallback 返回全库" 与桩"禁 fallback"方向相反 | 既有待裁定事项 | **不重复裁定**：前任工程师 Lee 已在 `task/E3-tools敲定与MCP统一/progress.md` §3 登记报监工；本文按 goal §2b② 与桩口径执行并引用该登记 |

---

## 附：实现交接清单（给 Jimmy 任务 #4 的需求输入）

本文不写代码，仅把契约中必须由实现兑现的硬点列明，供 `gateway.py`/`no_path_violation` 实现对照：

1. 9 个工具按 §1 字段表实现，出参不套 envelope（§2.3）；
2. 13 个统一错误码全覆盖，`UNKNOWN_ENTITY` 不得携带源归属信息（§2.4、§3.2）；
3. A/B 分片装载隔离（B∩A=∅ 加载断言 + 可查性质白名单 + `UNKNOWN_ENTITY` 不区分源归属；`SOURCE_DENIED` 保留未启用，不引为隔离支柱，§3.2）；
4. 每次调用落 `{t, tool, args, ret, ok}` 录像行，alias 落盘前归一（§2.5）；
5. 三类预算（search_budget / verify_budget / existence_budget）计数与超限错误码（§1、§3.4）；
6. `submit_answer` 校验 answer_schema + 一次仅回语法错的重交 + 终态单次成功（§1.6）；
7. `normalize_structure` 与评分引擎共用同一规范化函数实例（§1.9）；
8. §3.5 隔离测试断言 A1–A5 的可执行脚本（属任务 #4 验收物）。

---

## 6. E5 质量门第二条判据接口契约（task-05 §3 / 任务 #54）

**定位**：E3 提供给 E5 造题脚本直接调用的质量门第二条判据（线索可达，ADR-0018 §2.3）。造题期离线跑，不依赖活体 Gateway 会话态（内部自建 judge 侧 Gateway 实例，只读 A 侧数据；B 文件句柄零接触，与 test_backend_jsonl_reader 句柄级隔离断言同口径）。E5 消费方式（对齐 task-03-造30道open发现题.md §3.2）：逐 case 调用，`pass=False` 即换 gt 重配，`details.failures` 即判废原因（E5 task-03 交付 5 要求如实报判废原因）。

**落位与调用**：`src/benchmark/evaluate.py` 模块级函数（与 #53 纯函数组同文件）；E5 将 `src/benchmark` 加入 `sys.path` 后 `from evaluate import composition_grounding_gate`。

### 6.1 签名

```python
composition_grounding_gate(case_id, anchor, edit_op, gt, evidence_set,
                           tol=None, gw=None) -> dict
```

### 6.2 入参逐字段

| 入参 | 类型 | 取值约束 | 来源 |
|---|---|---|---|
| `case_id` | str | 非空。per-case 视图标识；视图文件 `<backend>/per_case_views/<case_id>.json` 在场 → anchor 可见性走该 case `visible_entity_ids`；不在场 → 回落全局 A 口径（条件②同源），属过渡期合法状态并在 `details.anchor_source` 留账 | E5 造题分配的 case 标识（与 per_case_views 落盘同名） |
| `anchor` | str | 非空。三种形态都受理：① A 分子实体 ID；② dummy atom SMILES（匿名域载体）；③ 材料身份键 `<reduced_formula>_SG<号>` | A 侧身份档 `get_entity_A.jsonl`（实体 ID）/ `search_candidates_A.jsonl` 匿名域 SMILES / 材料键由接口 B 现算（`_material_key_universe` 同接缝） |
| `edit_op` | dict | 恰含 `{'anchor','operator','detail'}`（answer_schema 0.3.0 edit_op 同形状）；`edit_op.anchor` 必须与 `anchor` 入参一致；`operator ∈ {'add_atom'(＋n e), 'remove_atom'(−n e)}`（组成封闭集，ADR-0018 §2.3，旧 5 元集中其余三算子无组成语义不受理）；`detail` 文法 `<token>\|<token>*<n>`（token = 真实元素符号或匿名 label，n 正整数缺省 1） | E5 造题时配置的编辑线索；与 agent 答卷 edit_op 同形状，保证造题期与判分期（条件③）同口径 |
| `gt` | str | 非空。四形态受理（composition_of 同判别序）：匿名式串 / 材料身份键 / 实体 ID / dummy SMILES | per-case 视图 `gt.composition`（匿名域元素计数多重集）序列化为匿名式串，或 B 侧身份键（dummy canonical SMILES）；B 侧组成由 judge 侧数据派生，入参不需要 B 明文结构 |
| `evidence_set` | list[dict] | 每条恰含 `{'mol','prop','value'}`（cited_values 同形状）；`mol`/`prop` 须为字符串；可为空列表（查得性空真） | E5 造题时从 A 侧真值挑选的线索集；`value` 须与 A 视图真值在容差内相符 |
| `tol` | dict \| None | `{prop: 容差}`，**仅作用于性质值比对**（语义同 Gateway.grounded：字符串/布尔严格、数值按 tol[prop] 缺省 0.1、0/1 整型观测值严格）；**组成 ε 为裁定 2 写死的固定常量 0.02，不受 tol 影响**（与 #53 条件③口径完全一致） | E5 可选覆盖，缺省走项目默认容差 |
| `gw` | 模块 \| None | gateway 模块注入接缝（测试用），缺省 `_import_gateway()` | — |

### 6.3 出参逐字段

| 字段 | 类型 | 含义 |
|---|---|---|
| `pass` | bool | 总判据 = 以下五条子判据全真；`False` 即判废重配 |
| `anchor_in_A` | bool | anchor ∈ 该 case 可见集（视图在场）或全局 A 口径（实体 ID 全集 ∪ 材料身份键 == ∪ A 分子 canonical SMILES 全集，回落时） |
| `edit_applied` | bool | edit_molecule applied=True 前置（#53 条件③裁定 3 的造题期形态）：分子域对锚点实判 `_rdkit_edit` 真实结构可施加性（detail 计数 n 次重复施加，经 `parse_edit_detail` 与组成侧同文法，避免与工具面旧文法双口径）；材料域 fail-closed 拒判（task-05 评审 Major-2，见 6.5） |
| `hops_within_limit` | bool | 算子总数（detail 计数 n）≤ 2（裁定 4 同定义） |
| `composition_equal` | bool | `composition_equal(apply_composition_edit(composition_of(anchor), op, detail), composition_of(gt))`，ε=0.02、分子域两侧剥 H、同分异构判相等是预期（H8）——与条件③身份判据同一纯函数组，不另起炉灶 |
| `evidence_all_queryable` | bool | evidence_set 每条 A 侧 get_property 查得且值相符；真值源 backend `get_property_A.jsonl`（A 视图，按 case_ref 行可见性）优先；**表在场键缺席直判 NOT_IN_CASE_VIEW，不逐条回落全局 mock_db**（task-05 评审 Major-1，防击穿 per-case 作用域），回落仅限表不在场（mock_db `_entity_props`）；逐条失败原因落 `details.evidence_unqueryable[].reason`（NOT_IN_CASE_VIEW / UNKNOWN_ENTITY / UNKNOWN_PROPERTY / VALUE_MISMATCH） |
| `details` | dict | 诊断明细：`epsilon`/`hop_limit`/`hops`/`anchor_source`/`evidence_source`/`composition{anchor,transformed,gt,match}`/`notes`（材料域断裂声明、表不在场回落原因等）/`evidence_unqueryable`/`failures`（判废原因清单，E5 自检记录直接消费） |

### 6.4 与第一条质量门的关系（分工与调用顺序）

- **第一条判据（A 内无替代解穷举）不归 E3**：归 E1-05 per-case 视图穷举接口（在途，只观察未触碰）。本函数不包含任何“A 内无替代解”逻辑，不越界实现。
- **调用顺序**：E5 造题脚本对每个候选 (anchor, edit_op, gt, evidence_set) 先调本函数（第二条，线索可达，已就位）；`pass=True` 后再走第一条穷举判据（E1-05 接口就位后接入）。两条全真才过质量门；任一为 False 即判废，判废原因分别取自本函数 `details.failures`（第二条）与 E1-05 接口返值（第一条）。

### 6.5 材料域 fail-closed（task-05 评审 Major-2，台账见 problems.md PR-38）

材料域锚点运行态不可达，本接口按 fail-closed 拒判：`domain=='material'` 一律 `edit_applied=False`、`pass=False`，并在 `details['failures']`/`details['notes']` 留痕。**实跑实证**：材料身份键锚点与晶体实体 ID 锚点（如 c00001）实调 edit_molecule 均返 ANCHOR_NOT_IN_A——分支 2 受理面为 mock_db admet/structures（旧 POC 键集），E1 身份档晶体实体与材料身份键均不在其中（实跑信封见 rehearsal_logs/task-05_condition3_composition_realdata.log B 段）。含义：造题期若放行会造出条件③ applied=True 前置结构性不可满足的死题（agent 运行期永远产生不了 applied=True 录像行），故拒判；待材料域编辑面收口（problems.md PR-38 台账登记）后放开。

### 6.6 错误行为（fail fast）

非法入参（形状错误）一律 `raise ValueError`，不静默通过：case_id/anchor/gt 非非空字符串；edit_op 非恰含三键对象、anchor 不一致、operator 出组成封闭集、detail 非串非 null；evidence_set 非列表或条目形状非法；tol 非 dict。参数语义错误（如组成不等、超跳数）不 raise，走子判据 False + failures 留账。

### 6.7 测试与基线

独立测试文件 `dag_poc/mcp_mock_server/tests/test_composition_gate_task05.py`（qualitygate_ 组，11 测试函数/21 用例）：质量门通过正例、同分异构 gt 判相等、组成不等拒绝、跳数 2 过/3 拒、缺 applied 前置拒绝、材料域 fail-closed 拒判（task-05 评审 Major-2 收紧翻转）、视图内外可见性、证据拒绝原因（mock_db 回落口径三类 + 表在场键缺席 NOT_IN_CASE_VIEW 与表不在场回落边界，task-05 评审 Major-1）与 tol 覆盖、非法输入 11 项 ValueError。全量基线 195+1 → 本轮评审修复新增 3 用例后零失败（test_material_key_recompute 失败系 E1 材料侧扩量所致，相关键列/实体行已提交，非在途改动，保持对照口径不触碰，处置措辞以收口任务登记为准）。
