# tools 与 MCP server 调研文档（E3 · 步 3 · 交付物 1）

| 字段 | 值 |
|---|---|
| **任务** | `task/E3-tools敲定与MCP统一/task.md` 子目标 1、2 |
| **对齐 goal** | `goal.md` §6 步 3（tools 数量下限口径）、§1.5（两类任务）、§2b（open 类工具需求） |
| **对齐基准（桩）** | `dag_poc/tools_schema.json`（v0.1.0-stub，9 个契约工具）、`dag_poc/answer_schema.json` |
| **作者** | Taylor（E3 工具线 · 任务 #3） |
| **日期** | 2026-08-06 |
| **验收红线自查** | 本文全部数字均带出处（本地文件路径或 URL）；无出处数字一律不写 |

---

## 0. 结论先行

**我方敲定的 tools 数量是 9 个 v1 契约工具（A 源可查 ×4、existence-only ×1、纯计算/收卷 ×4，B 库直接暴露 ×0），它同时落在文献同域区间的下段并严格满足 goal.md §6 步 3 的 fixed/open 双下限口径；现有 23 个灵枢工具因领域不匹配且数字虚高（6 真/9 假/8 转发）不予沿用。**

三个分论点：

1. **文献区间**（§1）：14 条已发表 benchmark 对照显示，**领域任务型评测给 agent 的工具数为 1–18 量级**（化学域 agent 主流 10–18，τ-bench retail 15，oracle 制可低至 1）；77–16,464 的大工具集均为"考工具选择"的通用评测，与我方"考科学发现过程"的目标不同构，不作对标。
2. **我方数量 = 9**（§2）：fixed 类按 6 个 checkpoint 逐一列明支撑工具（下限 6 个功能位、由 5 个工具覆盖），open 类"存在性接口 + A 侧性质查询 + 结构编辑/规范化校验"三类齐备——**8 = 两条下限的最小并集；`get_entity` 为第 9 个预留位**（修复五后已派生真实数据：RDKit formula/atom_count，定位材料线 CIF 身份查询，绑定 E1 身份档交付）。
3. **不沿用 23 的理由**（§2.5）：23 个工具的基座是基因/灵枢领域，与本项目药物+材料双领域不匹配；且其中仅 6 个真查库、9 个硬编码假实现、8 个纯转发，"23"是虚高数字，仅作数量级参考（goal.md §6 步 3 明文）。

---

## 1. 文献 tools 数量对照表

**小结：同类领域任务评测的工具数集中在个位数到十几个；大工具集（≥77）考的是工具检索/选择能力，不是科学发现过程。**

### 1.1 对照表（14 条，每条含出处）

| # | benchmark / 论文 | venue | 给 agent 的工具数 | 工具形态 | 清单出处（本地路径或 URL） |
|---|---|---|---|---|---|
| 1 | τ-bench（Yao et al.） | ICLR 2025（arXiv:2406.12045） | **τ-retail 15 个**（本地官方任务文件枚举：115 个任务引用的工具函数并集；每任务 0–7 个，分布 {0:2, 1:27, 2:13, 3:10, 4:19, 5:26, 6:15, 7:3}） | 领域专用 API 工具 + 政策手册 | 本地 `reference/tau-bench/retail_tasks_test.py`（2026-08-06 枚举，3358 行、115 任务）；并集清单：calculate / cancel_pending_order / exchange_delivered_order_items / find_user_id_by_email / find_user_id_by_name_zip / get_order_details / get_product_details / get_user_details / list_all_product_types / modify_pending_order_address / modify_pending_order_items / modify_pending_order_payment / modify_user_address / return_delivered_order_items / transfer_to_human_agents；论文 arXiv:2406.12045、本地 `reference/tau-bench/tau-bench.md`。【口径裁定（修复六，2026-08-06）】既往同一行并存 16/15 两个互斥口径，裁定取 **15**（任务文件引用并集口径，本地可复现）；旧 16 为"tools 目录文件数"口径，差额 1 = `think`（通用思考工具，未被任何测试任务引用）——本评测考的是 agent 用域内 API 干活，`think` 不计入。既往"τ-airline 14"出自远程仓库目录枚举（GitHub Contents API），本地 `reference/tau-bench/` 无 airline 任务文件可枚举、不可复现，撤下该数字；原"GitHub API 实测"出处一并删除 |
| 2 | ToolBench / ToolLLM（OpenBMB） | ICLR 2024 Spotlight（arXiv:2307.16789） | 全库 16,464+ RapidAPI API（49 类）；每题候选 `api_list` 数个至数十个（G1 单工具 / G2 同类多工具 / G3 跨集合多工具） | 真实第三方 API | `reference/ToolBench/README_采样说明.md`（L4、L14–L16）；`reference/ToolBench/case_示例.json`（G2 真实 api_list） |
| 3 | InfoMosaic-Bench | ICLR 2026（arXiv:2510.02271，OpenReview `AhQFDBBIRZ`） | **77** 个 MCP 工具（621 题 / 6 域） | MCP 工具，题目由工具真实输出反向合成 | `reference/ICLR-InfoMosaic-Bench/阅读报告.md` §1（工具数行 L19；Abstract 出处 L81） |
| 4 | Toolathlon / Tool Decathlon（HKUST+All Hands+CMU+Duke） | ICLR 2026（arXiv:2510.25726，OpenReview `z53s5p0qhf`） | **604** 个 MCP 工具（32 个真实软件、108 题、平均约 20 轮调用） | MCP server（多数为团队自研/修订） | `reference/ICLR-Tool-Decathlon/阅读报告.md` §1（L21）；`reference/ICLR-Tool-Decathlon/数据格式与样例.md`（L19） |
| 5 | ChemCrow（Bran et al.） | Nature Machine Intelligence 6, 2024（arXiv:2304.05376） | **18** 个专家设计的化学工具 | LangChain ReAct 注册工具（RDKit/RXN4Chem/安全库等） | `reference/ChemCrow/README_采样说明.md`（§它是什么）；`reference/ChemCrow/agent_tools_registry.py`（18 工具注册，实测文件） |
| 6 | CACTUS（SciAgentArena 收录的化学 agent） | 随 SciAgentArena（arXiv:2606.12736，2026） | **10** 个（分子性质计算 MW/LogP/TPSA + 成药性过滤 Brenk/PAINS） | cheminformatics 工具 | 【转引，无一手出处】转引自 `reference/SciAgentArena/README_REPO.md`（L54，agent 目录表）；CACTUS 本体无公开工具注册清单可核 |
| 7 | SciAgentArena / Agent4Science（dd 药物发现域） | arXiv:2606.12736（2026，Yale/Harvard 等 33 人） | 不设统一工具清单：agent zoo 各框架自带（ChemCrow 18 / CACTUS 10 / ToolUniverse / STELLA 等）；dd 域 79 题答卷为可执行 Python 代码 | 框架自带工具 + 代码执行 | `reference/SciAgentArena/README_采样说明.md`（§它是什么、§判分机制）；`reference/SciAgentArena/README_REPO.md`（L42–L64） |
| 8 | MADE（Malik et al., Oxford） | ICML 2026（arXiv:2601.20996） | **1 类 oracle 动作**（候选结构→形成能评估），预算 ≤5 次查询/episode | 受限预算 oracle（ORB/MACE/解析式） | `reference/MADE/README_采样说明.md`（§一个完整 case、§架构与判分机制）；`reference/MADE/README_REPO.md`（L59） |
| 9 | Tartarus（Aspuru-Guzik 组） | NeurIPS 2023 Datasets & Benchmarks（arXiv:2209.12487） | **0 个 agent 工具**（被测对象是生成模型/优化算法，直接产出 SMILES）；评分侧为 5 个物理模拟 oracle（docking/tadf/pce/reactivity/filter） | 物理模拟 oracle，不暴露为工具 | `reference/Tartarus/README_采样说明.md`（§四大领域任务、§怎么测） |
| 10 | ScienceAgentBench（OSU） | ICLR 2025（arXiv:2410.05080） | **0 个 API 工具**：唯一"工具"是代码执行（交付物 = 可执行 Python 程序，102 题） | 代码执行环境 | `reference/ICLR-ScienceAgentBench/阅读报告.md`（§一基本信息、§二核心做法） |
| 11 | DiscoveryBench（AI2） | arXiv 2024（arXiv:2407.01725） | **0 个固定工具**：给 264 题各配真实数据集 + 列语义 + 领域知识，agent 自行分析产出假设 | 数据集即材料，工具自备 | `reference/DiscoveryBench/README_采样说明.md`（§它是什么、§一个完整 case） |
| 12 | MatTools（HKU 等） | arXiv 2025（arXiv:2505.10852） | 无固定工具清单：工具面 = pymatgen / pymatgen-analysis-defects 库函数；49 题 / 138 任务，每题带可断言 properties 清单 + 单元测试 | 代码生成调用库函数 | `reference/MatTools/README_采样说明.md`（§与需求的匹配度，L14–L15、L22） |
| 13 | MLE-bench（OpenAI） | ICLR 2025（arXiv:2410.07095） | 无固定工具清单：环境 = 75 个离线 Kaggle 竞赛的计算环境（代码执行为主），单题可自主运行最长 24 小时 | 计算环境 + 代码执行 | `reference/ICLR-MLE-bench/阅读报告.md`（§数据量 L19；§0 一句话 L13） |
| 14 | GAIA（Meta/HF/AutoGPT） | ICLR 2024（arXiv:2311.12983） | 工具由 agent 自带不固定；**难度分层显式使用"步数 + 工具数"**：Level 1 ≤5 步且至多 1 工具；Level 2 约 5–10 步需多工具；Level 3 任意 | agent 自带（浏览器/代码解释器等） | arXiv:2311.12983（GAIA 论文一手出处，难度分层定义见论文） |

### 1.2 区间结论

**小结：文献工具数呈三段分布，与我方任务同构的是最低一段（1–18）。**

| 段 | 区间 | 代表 | 设计意图 | 与我方同构性 |
|---|---|---|---|---|
| **① 领域任务/科学发现型** | **1–18**（化学域 agent 主流 10–18；oracle 制低至 1） | ChemCrow 18、CACTUS 10、τ-bench retail 15、MADE 1 | 工具是干活的手段，考的是任务完成与过程（备注：Tartarus、ScienceAgentBench 两个 **0 工具样本**属 oracle 制/代码执行制，不暴露 agent 工具，不入本区间，保持区间可证伪） | **高**——我方同属此类 |
| ② 多源信息搜寻型 | 77 | InfoMosaic-Bench | 考跨源依赖链的工具编排 | 中——仅 cp1 文献检索与之局部同构 |
| ③ 工具选择/检索压力型 | 604–16,464+ | Toolathlon、ToolBench | 大工具池本身即难度来源（工具检索） | **低**——我方不设"工具检索"考点 |

两点推论（直接服务 §2）：

1. **对标段①**：我方 9 个工具落在 1–18 区间的下段，与 τ-bench（retail 15，域内客服任务，本地任务文件枚举口径）和 CACTUS（10，分子性质任务）同数量级；比 ChemCrow 的 18 少，是因为 ChemCrow 面向开放化学任务全集，而我方工具集只需覆盖冻结的 checkpoint DAG（§2.2）与 open 三类需求（§2.3）。
2. **不追求段③**：工具数量多≠评测更难。我方难度旋钮是"宇宙规模 + 查证预算"（`dag_poc/docs/INTERFACE_CONTRACT.md` §7），不是工具池大小。

---

## 2. 我方敲定的 tools 数量与理由

**小结：敲定 9 个 v1 契约工具。算术口径（task-02 重要 1 改写，监工实测裁定）：8 = fixed 六 checkpoint 下限与 open 三类下限的最小并集；`get_entity` 为第 9 个预留位（修复五后已派生真实数据，定位材料线 CIF 身份查询，绑定 E1 身份档交付）。总数 9 落在文献同域区间（1–18）内。**

### 2.1 9 个工具总览（与桩逐一对齐）

出处：`dag_poc/tools_schema.json`（v0.2.0，2026-08-06，owner AI-E3-Lee）`tools[]` 与 `meta.source_tier_legend`。

| # | 工具 | 一句话功能 | source_tier | 预算 | 服务的 checkpoint / 机制 |
|---|---|---|---|---|---|
| 1 | `literature_search` | 自然语言检索学术文献，返回论文元信息 | A | search_budget | cp1 文献检索 |
| 2 | `search_candidates` | 按约束/关键词检索候选实体，返回匿名 ID + 结构类可见字段 | A | search_budget | cp2 候选搜集 |
| 3 | `get_entity` | 按 ID 查单个实体的结构/身份可见字段（不含性质）。【第 9 个预留位，task-02 重要 1】修复五后已派生真实数据（RDKit formula/atom_count，重原子口径；masked_structure 恒 None 待 E1 身份档），定位材料线 CIF 身份查询，绑定 E1 身份档交付 | A | free | cp2/cp3 的实体身份确认 |
| 4 | `get_property` | 按 ID 查性质快照真值（grounded 判定唯一数据源） | A | verify_budget | cp3 性质查证、cp4/cp5 的数据原料 |
| 5 | `compare` | 对已查得的实体按性质键排序（保留：cp4 主路径证据工具 + 拓扑验证依赖，理由见 §2.2 cp5 行） | none | free | cp4/cp5 |
| 6 | `submit_answer` | 提交符合 `answer_schema.json` 的结构化答卷 | none | free | cp6 提交 |
| 7 | `check_molecule_exists` | open 类存在性接口：返值严格等价 m ∈ B，只返单个 bool | existence-only | existence_budget（max_attempts ≤10） | open 类停止信号 |
| 8 | `edit_molecule` | 对 A 侧锚点施加封闭算子集内的单步结构编辑（纯计算） | none | free | open 类推断生成、grounding 条件③ |
| 9 | `normalize_structure` | 结构表示规范化为匿名域唯一形式（纯计算） | none | free | open 类同一性判定、grounding 复核 |

分层统计（与任务书头部口径一致）：**A ×4（literature_search / search_candidates / get_entity / get_property）、existence-only ×1（check_molecule_exists）、none ×4（compare / submit_answer / edit_molecule / normalize_structure）、B 直接暴露 ×0。**

### 2.2 fixed 类下限口径：每个 checkpoint 至少一个可落地 tool 支撑其产物（逐一列明）

出处：`goal.md` §6 步 3 下限口径条文；`dag_poc/tools_schema.json` `meta.tool_count_policy.fixed_lower_bound` 与 `meta.fixed_checkpoint_coverage`。

| checkpoint | 产物 | 支撑工具（至少一个） | 说明 |
|---|---|---|---|
| cp1 文献检索 | `trace.docs`（文献 ID 集合） | `literature_search` | 返回 `papers[].doc_id`（DOI/PMID）供 cp1 硬判 |
| cp2 候选搜集 | `sub.pool`（候选池）且 `sub.pool` 中每个 ID 真检索到过 | `search_candidates` | 返回匿名实体 ID；产物不得含性质数值（性质走计费的 `get_property`） |
| cp3 性质查证 | `sub.value[c][p]` 且 `grounded(c,p,v)` 成立 | `get_property` | 返回值即真值（ACTION_RULES §6），录像行是 grounded() 的唯一原料 |
| cp4 条件筛选 | `sub.pass_set` | `get_property`（数据）+ `compare`（可选便利） | 筛选所需数值只能来自 cp3 查证 |
| cp5 排序 | `sub.ranking` | `compare`（agent 亦可自排，判分口径不变；**保留理由**：① cp4 主路径证据工具——evidence_map 中 cp4 支撑工具含 compare，录像行支撑 evidence_time 首次建立时刻判据；② 路径拓扑验证（no_path_violation）的合法轨迹用例依赖 compare 录像行，删它则 cp4/cp5 拓扑链断） | `compare` 不引入新信息，仅便利 |
| cp6 提交 | `sub.best`（含 NO_SOLUTION） | `submit_answer` | 三通道收口之一（承 INTERFACE_CONTRACT §3.2） |

**结论**：6 个功能位由 5 个工具覆盖（cp3/cp4 共用 `get_property`）。少于 5 个则必有 checkpoint 的产物无工具支撑，撞 goal §4 支柱 2（编造产物要能被工具落地检查抓出）的前提。

### 2.3 open 类下限口径：三类齐备（逐一列明）

出处：`goal.md` §6 步 3 下限口径条文、§1.5 类型二三条关键设计约束、§2b⑤；`dag_poc/tools_schema.json` `meta.open_triad_coverage`。

| 类别 | 需求来源 | 支撑工具 |
|---|---|---|
| ① 存在性接口 | goal §1.5 约束"存在性查询接口"：agent 推出分子 m 后查接口判存在性，只知存/不存、不知性质 | `check_molecule_exists`（existence-only，只返单 bool） |
| ② A 侧性质查询 | goal §2b③ 线索性联系必须完全由 A 内数据归纳得出——归纳需要查 A 侧性质 | `get_property`（source_tier=A；SOURCE_DENIED 保留未启用、v1 无产生路径，B 侧性质复核仅评分期执行，见契约规范 §2.4） |
| ③ 结构编辑/规范化校验 | goal §2b④ 质量门要求 `edit_op(anchor)` 规范化后 == gt，且答卷 `edit_op` 字段须有工具侧对应物 | `edit_molecule` + `normalize_structure` |

另有两个 open 类必需但属 §2.2 已计入的工具：`literature_search`/`search_candidates`（agent 获取 A 侧锚点与线索的检索入口）、`submit_answer`（收卷）。**结论**：三类无一缺位；**8 = fixed 下限并 open 下限的最小并集；`get_entity` 为第 9 个预留位**（修复五前对 mock_db 空操作，监工实测指出"9 = 最小并集、无一冗余"算术不成立；修复五后已 RDKit 派生 formula/atom_count 真实数据，定位材料线 CIF 身份查询，绑定 E1 身份档交付）。

### 2.4 与文献区间的对标结论

| 维度 | 文献证据 | 我方 |
|---|---|---|
| 数量级 | 同域段①为 1–18（§1.2） | 9，位于区间下段 |
| 工具数与 checkpoint 的关系 | InfoMosaic-Bench 77 工具服务 621 题的工具依赖链（`reference/ICLR-InfoMosaic-Bench/阅读报告.md` §2）；MatTools 每题 properties 断言决定该题需要的库函数（`reference/MatTools/README_采样说明.md` L29"性质清单即卡点"） | 我方工具数由 checkpoint 下限反推，而非先定数再凑工具 |
| 预算制先例 | MADE ≤5 次 oracle 查询（`reference/MADE/README_采样说明.md`）；SciAgentArena C3 预算制 oracle 超限抛错（`reference/SciAgentArena/README_采样说明.md` §判分机制） | search_budget / verify_budget / existence_budget 三类预算（`dag_poc/tools_schema.json` 各 tool `budget` 字段） |

### 2.5 为何不沿用现有 23 个（显式说明）

**小结：领域不匹配 + 数字虚高，两条理由独立成立、任一即足以否决。**

**理由一 · 领域不匹配。** 现有 23 个工具来自灵枢基因 MCP（`dag_poc/data/TOOLS_INVENTORY.md` 头部："来源：`~/work/damo-gene-lingshu-mcp/server.py` 实际注册的 23 个 @mcp.tool"），基座是基因/灵枢领域；本项目定位是**材料发现 + 生物分子（药物）发现**双领域（`goal.md` 文档头）。`simulate_virtual_cell`、`check_cell_simulation`、`validate_target` 等基因域工具与双领域任务无对应关系（`dag_poc/tools_schema.json` `legacy_tool_disposition.dropped_with_reason`）。goal.md §6 步 3 明文："现有 23 个来自灵枢基因 MCP，与本项目双领域任务不匹配，仅作数量级参考"。

**理由二 · 数字虚高：23 = 6 真 + 9 假 + 8 转发。** 出处：`task/task12/权威数据源与数据缺口指南.md` §1.1（逐行核验 `mock_server.py` 行号在案）：

| 档 | 数量 | 代表证据 |
|---|---|---|
| 真查库 | 6 | `predict_admet`（40 条）/ `crystal_predict`（17 条）/ `search_molecule_database`（40 条，15/40 SMILES 错配另计）/ `validate_target`（仅 1 条靶点）/ `literature_search`（14 条）/ `get_cif_by_formula`（空壳无 CIF 内容） |
| 硬编码假实现 | 9 | `predict_binding` 恒返 −8.5；`generate_molecules` 返非法 SMILES `C0CC`；`preview_structure_file` 恒返 `P6/mmm`；`screen_literature` 五元判定恒 True/low/high |
| 纯转发/无需数据 | 8 | `fetch_bib_info` 转 `literature_search`、`get_or_generate_cif` 转 `get_cif_by_formula`、`compare_materials` 纯排序等 |

假实现工具不满足 goal §4 支柱 1（GT-first：真值可验）——`predict_binding` 恒返 −8.5 意味着任何 agent 拿到同一个数，checkpoint 区分度为零；把它计入"23 个工具"会误导对标。

### 2.6 23 → 9 的收敛去留（对照表）

出处：`dag_poc/tools_schema.json` `legacy_tool_disposition`（merged / dropped_with_reason）与 `alias_map`（23 个旧名归一，历史 trace 仍可解析）。

| 去向 | 旧工具 | 新契约 |
|---|---|---|
| 合并 | `fetch_bib_info`、`search_drug_literature`、`screen_literature` | → `literature_search` |
| 合并 | `get_cif_by_formula`、`get_or_generate_cif`、`download_poscar`、`preview_structure_file`、`poscar_to_cif` | → `get_entity`（+ `get_property` 晶体性质线） |
| 合并 | `predict_admet`、`crystal_predict` | → `get_property` |
| 合并 | `search_molecule_database` | → `search_candidates`（并写死禁 fallback 全库） |
| 合并 | `compare_materials` | → `compare` |
| 新增 | —（旧无对应物） | `submit_answer`、`check_molecule_exists`、`edit_molecule`、`normalize_structure` |
| 弃用 | `predict_binding` | 硬编码假实现；药物线如需结合亲和力由 E1 步 4 按真实子库重新立项 |
| 弃用 | `simulate_virtual_cell`、`check_cell_simulation` | 基因/灵枢领域，不匹配 |
| 弃用 | `generate_molecules`、`crystal_generate`、`check_crystal_generation` | 假实现/AI 生成入口不在 v1 提供——候选只来自检索 + 推理编辑（goal §2b 路线） |
| 弃用 | `validate_target` | 基因/灵枢靶点验证；药物线如需靶点信息由 E1 步 4 决定 |
| 弃用 | `generate_report`、`generate_drug_report` | 判分无关；交卷已由 `submit_answer` 承担 |
| 弃用 | `echo_text` | 测试工具，不发布 |

---

## 3. 与 schema 桩的对齐核对（留痕）

**小结：本文全部口径与 `dag_poc/tools_schema.json` v0.1.0-stub 一致，未发现需报监工的实质冲突。**

| 核对项 | 桩内出处 | 本文口径 | 判定 |
|---|---|---|---|
| 工具总数与分层 | `tools[]`（9 条）、`meta.source_tier_legend` | §2.1 表 | 一致 |
| fixed 下限（cp1–cp6 支撑工具） | `meta.tool_count_policy.fixed_lower_bound`、`meta.fixed_checkpoint_coverage` | §2.2 | 一致 |
| open 下限（三类齐备） | `meta.tool_count_policy.open_lower_bound`、`meta.open_triad_coverage` | §2.3 | 一致 |
| "不以 23 为基线"理由 | `meta.tool_count_policy.not_baseline_23` | §2.5 | 一致（本文补充了 task12 §1.1 的逐条证据） |
| 23 旧名归一 | `alias_map`（23 旧名 → 规范 name / DEPRECATED） | §2.6 | 一致 |

一项待监工裁定的既有事项（不重复裁定，仅引用）：ADR-0011 决策 3 的"fallback 返回全库"与新契约"无命中返 EMPTY_RESULT、禁 fallback"方向相反，前任工程师 Lee 已在 `task/E3-tools敲定与MCP统一/progress.md` §3"登记修复项"报监工；本文 §2.6 中 `search_candidates` 的行为口径按 goal.md §2b②（A 的任何查询不得带出整库内容）与桩执行。

---

## 附：出处索引（快速回溯）

- 桩：`dag_poc/tools_schema.json`、`dag_poc/answer_schema.json`
- goal：`goal.md` §1.5、§2b、§6 步 3
- 任务验收：`task/E3-tools敲定与MCP统一/task.md` §3 子目标 1、2
- 23 工具盘点：`task/task12/权威数据源与数据缺口指南.md` §1
- 现有出入参：`dag_poc/data/TOOLS_INVENTORY.md`
- 网关 4+1 与录像：`dag_poc/docs/INTERFACE_CONTRACT.md` §3
- 文献素材：`reference/tau-bench/`、`reference/ToolBench/`、`reference/ICLR-InfoMosaic-Bench/`、`reference/ICLR-Tool-Decathlon/`、`reference/ChemCrow/`、`reference/SciAgentArena/`、`reference/MADE/`、`reference/Tartarus/`、`reference/ICLR-ScienceAgentBench/`、`reference/DiscoveryBench/`、`reference/MatTools/`、`reference/ICLR-MLE-bench/`
- τ-bench 枚举落盘产物（task-02 重要 5）：`reference/tau-bench/tools_enumeration_20260806.json`（115 任务每任务引用工具数分布 + 并集 15 清单 + 裁定理由，ast 解析 retail_tasks_test.py 生成，可复跑）
- τ-bench 工具数核验（2026-08-06，本地可复现）：枚举本地 `reference/tau-bench/retail_tasks_test.py`（115 个官方任务）统计每个任务引用的工具函数，并集 = 15（裁定口径，见 §1.1 表行 1）；论文出处 https://arxiv.org/abs/2406.12045。既往"GitHub Contents API 实测枚举"出处不可复现，已删除
