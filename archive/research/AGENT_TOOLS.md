# Agent 工具标杆清单

> **这是 agent 工具的唯一权威清单，后续所有相关 tool 都写在这里。**
> **一个 tool 一节**，节内第一行先报状态，再说它干什么、输入输出长什么样。
> 例子取自实跑校准样本，可直接照抄。标了「未实现」或「未接入」的**故意不给例子**——还没有实跑数据，写了就是虚构。

**第一行状态怎么读**，三段用 `｜` 隔开：

| 第一段 属哪一档 | 第二段 实现/可用情况 | 第三段 现在用不用 |
|---|---|---|
| **基础 tool**：所有档位都给，主测的困难档只有这几个 | **已实现**：我方代码里有 | **已在用**：agent 现在就调得到 |
| **便捷 tool**：只在低难度档给，每个替代掉 agent 的一步推理（ADR-0026）| **未实现**：代码里没有 | **未启用**：代码里有，但困难档不给 |
| **扩展 tool**：灵枢 MCP 侧的真实 tool（本清单收录 30 个）| **本地实测可用 / 内网边界搁置 / 只返 mock 数据 / 外部 403 待补丁**（2026-08-14 实测，见 `toolcheck/`）| **未接入**：agent 调不到 |
| **已作废**：不再提供 | — | **不再提供** |

**给不给一个 tool，只用一条判据**（ADR-0025）：

$$\text{给 tool} \iff \text{这一步是「查资料」或「做实验」}$$

其中：

- **查资料** —— 取外部世界已有的知识（别人发表过什么、测过什么）。科学家必须去查，因为知识不在他脑子里；
- **做实验** —— 取外部世界的测量结果（这个分子的溶解度实际是多少）。科学家必须动手测，因为答案不在纸上；
- **其余一切** —— 提候选、决定测什么、权衡取舍、归纳外推、排序、格式转换，都是他在纸上和脑子里的活，**不给 tool**。

把 agent 当成一位科学家：**他离开书桌才能拿到的东西才给 tool**。所以一句分工是——文献靠 `literature_search` 查，性质靠 `get_property` 做实验，最后靠 `submit_answer` 收尾，**候选分子靠他自己想，没有 tool 帮他找**。

**分子标识按领域定**（统一到一种语法做不到：晶体没有 SMILES，分子用组成式则同分异构体撞车）：

| 领域 | 标识形态 | 例 |
|---|---|---|
| 小分子药物 | 规范化 SMILES | `CC(=O)Oc1ccccc1C(=O)O` |
| 超导材料 | 规范化组成式（元素按原子序数升序、配比保留两位小数）| `Y1Ba2Cu2.95Ca0.05O6.79` |

入参字段名两侧统一，**agent 不需要记两套接口**；每个 tool 收到标识串后自己在内部归一，agent 不感知这一步。**已回滚（2026-08-15，toolcheck 任务③）**：三库 A 侧 92,153 实体 formula 全量真实式（晶体带小数配比、registry 直读真值）、分子 inchikey 透传、`edit_molecule`/`normalize_structure` 直处理真实 SMILES；材料身份键同步切真实式（`AgGe100Pd99Sr50_SG139` 形态，元素字典序归约）。

**调用预算**：一道题最多调 **50 次** tool，超了报 `CALL_LIMIT_EXCEEDED`。其中 `get_property` 每查一次性质额外扣一次查证预算——**查性质等于做一次实验，做了就要付代价**，别拿它反复试探。

**改 tool 面之前必须先看的两条代码实况**（实测 2026-08-13）：

- 我方 `dag_poc/mcp_mock_server/mock_server.py` 实际注册 **9 个**，名单以 `CONTRACT_REGISTERED_NAMES` 为准，且 `_assert_tool_surface()` 强制校验暴露面**恰为 9 个**（多一个都算破窗）。**加 tool 或删 tool 必须同步改这两处**，否则模块加载即失败；`tools_schema.json` 也仍是 9 个，同步改。
- 灵枢 MCP（仓库 `gene-damo/damo-gene-lingshu-mcp`，分支 `jingwu/feature/tools2mcp_pr`）`server.py` 实注册 **34 个**真实 tool，全部有真后端、统一返 JSON 字符串、都套了异常兜底（`_tool_boundary`：异常不裸抛，转成固定 `error_code`，所以"调失败"也能当判据）。**它的 `MCP_USAGE.md` 仍写 23 个，已落后，工具数只认 `server.py` 的装饰器数量**。其中 **4 个文献检索类 tool 不纳入本清单**（文献一律走本地 DOI 库，决策与实测记录在 `toolcheck/results/REPORT.md`），故本清单收录灵枢侧 **30 个**。

**11 个公共数据库 tool 的共同风险**（下面每个都指回这里）：它们连的是真库，**真库里存着真答案**。若题面的靶点或分子能被直接查到结论，agent 就不需要推理、只需检索。**接入前必须逐题过一道污染体检**：把题面字面丢进这些 tool，看能不能直接撞出答案。

**实测记录（2026-08-14 全量实调；当日删除 4 个文献类后现行口径 39 个）**

**时延总览（2026-08-15 单线程实测，`toolcheck/latency_probe.py`；明细逐工具写在各节，原始逐次数据在 `toolcheck/results/latency_ours.jsonl` / `latency_lingshu.jsonl`）**：离线 9 工具全部 P50 ≤36ms、P95 ≤0.6s（首个请求的库加载尖峰，稳态 2-5ms）；线上 23 工具三档——纯本地计算 ~10ms（echo_text/poscar_to_cif 等 5 个）、外网公共库 API P50 0.5-6.3s（10 个，pubChem 最慢）、真跑模型/长任务 P50 20-28s（generate_molecules 27.5s、crystal_generate 20.2s、simulate_virtual_cell 20.4s）；异步任务查询类 60ms（check_*）。**离线与线上差 3-6 个数量级，这就是"检索/查值类 mock 到本地库"的量化依据**。注：测法为本地起灵枢服务（配置注入）+ 工具真实外呼公共 API，时延大头即外网往返；download_poscar 的时延是假 SID 被拒路径（真外呼 MPDS）。

隔离测试台在 `toolcheck/`，完整报告见 `toolcheck/results/REPORT.md`。做法：开一个只能看到这批 tool 的 claude code 实例（内置 tool 全关、其他 MCP 全挡，已用启动事件存证），再用不经过模型的直连探针做判定基准。结论（真实 Diamond 配置注入后的口径）：

| | 能用 | 不能用 |
|---|---|---|
| 基础 + 便捷 tool（9 个已注册）| **9**（全部）| 0 |
| 扩展 tool（本清单收录 30 个）| **23** | 7（4 内网边界搁置 + 2 只返 mock 不必修 + 1 外部 API 403 补丁待应用）|

> ⚠️ 本表曾写「16 能用 / 14 不能用」，**已推翻**：那批「缺后端/缺凭证」大部分只是灵枢的 Diamond 配置没喂进去（本地启动方式问题，不是工具缺陷）。用 `toolcheck/lingshu_with_config.py` 注入真实配置后，`predict_admet` / `predict_binding` / `generate_molecules` / `crystal_generate` / `simulate_virtual_cell` 等 7 个重型工具全部真跑模型恢复可用；`download_poscar` 亦确认已真连 MPDS。修正过程见 REPORT.md「为何之前估错了」。其中那个外部 API 403（`search_clinical_trials`）补丁在 `toolcheck/patches/`（已验证 403→200），应用后即为 **24 能用 / 6 搁置**。

**先说清 mock 这个词**：本项目说的 mock 是“**有真库，从里面查真值，只是不走完整链路**”——值是真的，省的是链路，这是正当做法。我方 9 个 tool 就是这种（读 `dag_poc/data/mock_db.json`，实测 `tc=36.7`、`logS=-1.41` 均为库内真值）。灵枢的 `SHOWCASE_MOCK` 分两层：GLP-1R 那条是**手工录入的真值**（UniProt P43220、PDB 7KI0、CHEMBL3137309 均可核），也算正当，只是覆盖面只有一条产品线；**真正有问题的是“都不匹配时返回的与输入无关的兜底常数”**（仅 `validate_target` 一个，详见报告）。

**前提**：灵枢预发环境当时是挂的（`HTTP 500` / `x-error-code: 1108` / `SYSTEM:fiber_data_is_not_health`，DNS 与 TLS 均正常，是 FC 函数实例不健康），只能退回本地起服务。最初用 `LINGSHU_DIAMOND_DISABLED=true` 启动、配置全空，造成上一段更正的那批误判；现已改为注入真实配置重测，上表 23/7 即重测后口径。真正剩下的 7 个里，4 个是 KeyCenter 内网边界（须内网环境才跑得动，见各节搁置说明），不是工具坏了。

**实测抬出的三个坑**（详说在报告里）：

1. **1 个 tool 会闷不作声地编数**——`validate_target` 库里**只有 GLP-1R 一条记录**，命中返真值，没命中则返与输入无关的固定常数（`score: 0.65`）且**不带任何模拟数据提示**。实测用 ABL1（伊马替尼靶点）得到“结构数据有限、需进一步验证”——**这是事实错误**。对比 `predict_binding` 兜底自带 `warning: Simulated data`，是诚实的。**接入前必须在网关层把 `data_provenance` 含 MOCK 且无 warning 的返回转成显式错误。**
2. **端口撞车**——我方 mock 与灵枢本地版默认都监听 **9000**，同时起必有一个连错。`toolcheck/` 把我方改到 9100。
3. **让 agent 自报“我调成功了没”不可靠**——claude 实例与直连探针 43 个里 6 个分歧，全是它把“后端没配置但有响应”和“返 mock”当成了能用，且同一个 `GENERATION_NOT_CONFIGURED` 它前后给了两种结论。**这直接支持了本 benchmark “真值必须从独立账本反查、不能信 agent 自述”的基本设定。**

---

## `literature_search` 查文献

**基础 tool ｜ 已实现 ｜ 已在用（所有档位）**。按一句自然语言检索文献，只返文献元信息，不返任何分子或性质。

**它是本 benchmark 唯一的文献入口**（owner 定，2026-08-14）。文献一律走**本地 DOI 库**，**不接任何真实搜索 API**。三条好处：无需凭证、不依赖外网、**结果可重现**（真搜索的返回会随时间变，同一道题两次跑拿到不同文献，判分就不可重现）。

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 3ms / max 3ms，通过 20/20
**输入**：`query` 查询语句（必填）、`max_results` 要几篇（可选）。

**输出**：`papers` 数组，每篇给文献编号、标题、年份、引用数；另给命中总数与本次返回数。

```
输入  {"query": "CNS drug candidates blood-brain barrier", "max_results": 2}

输出  {"query": "CNS drug candidates blood-brain barrier",
       "total_found": 2, "returned": 2,
       "papers": [
         {"doc_id": "10.1002/9781118788523.ch2",
          "title": "Pharmacokinetics of CNS Penetration",
          "year": 2015, "citation_count": 9},
         {"doc_id": "10.1002/9781118788523.ch21",
          "title": "Case Studies of Non-CNS Drugs to Minimize Brain Penetration",
          "year": 2015, "citation_count": 0}]}
```

**必须知道的边界（扩量实测 2026-08-15）**：库在 `dag_poc/data/backend/literature_search/literature_search_A.jsonl`，共 **2655 条、2655 个唯一 DOI**（`doc_id` 就是真 DOI，全部 Crossref 真实题录，绝不编造），年份跨 1931–2026，域配比 **药物 1752 / 超导 903（66:34，按老库 36:64 实测比例等比放大）**。keywords 双路标注：老 309 条手工标注原样合入（DOI 精确匹配），新增 2346 条从 title 自动抽取（停用词过滤+种子词优先+复数归一，50 条抽检记录见 `toolcheck/results/literature_spotcheck_50.md`）。字段只有 `doc_id`、`title`、`year`、`citation_count`、`keywords`——**没有摘要，也没有正文**。另：`mock_db.json` 里还有 14 条旧口径，仅当 backend 不在场时兜底。因此：

- 能从文献推出的：**该看哪个性质**（`keywords` 里就带着 `herg`、`caco2`、`critical temperature` 这类词）；
- **推不出的**：任何**阈值数值**，库里不存在数值内容。故题面必须给出阈值，不得要求 agent 从文献推阈值。

**匹配机制（读代码 + 实测 2026-08-14）**：query 整串小写化后逐条检查 `keywords`，**任一关键词是 query 的子串即命中**（OR 语义）；**不匹配 title、不分词、无相关性排序**（按库内顺序截 `max_results`）。方向是「关键词 ⊆ query」而非「query 的词 ⊆ 文档」，两个直接后果（均实测）：
- query 带的领域词越多命中面越宽（OR）：`"high temperature superconductor"` 命中 310 条（`high temperature superconductor`/`cuprate`/`supercon` 等多个 kw 都是它的子串），远多于单词 `"superconductor"`（236 条，命中 `superconductor`/`supercon`）；
- **同义词 / 缩写 / 更短说法搜不到**：`"tc"` 0 条（关键词是 `critical temperature`）、库外领域 `"topological insulator"` 0 条，返 `EMPTY_RESULT`。

**覆盖缺口（扩量后 2026-08-15 重验）**：老库时代的 `synthesis`（0 条）已随扩量补上（44 条）；`cryogenic` 仅 1 条、`"topological insulator"` 0 条仍是窄口。出题时须用库内真存在的词（`superconductor` 236、`cuprate` 53、`critical temperature` 83、`solubility` 87、`qt prolongation` 30、`herg` 21、`caco-2`/`caco2` 各 24），否则 agent 拿题面原词去查会空手而归。**既然文献不接真实搜索，覆盖面就是硬约束**：要扩覆盖只能**往本地库里加条目**（构建脚本 `src/data_build/build_literature_search.py`，Crossref 增量缓存，重跑不联网），不能靠接外部 API 绕过。

---

## `get_property` 查性质（做实验）

**基础 tool ｜ 已实现 ｜ 已在用（所有档位）**。给一个标识串和一个性质名，返回这个分子这项性质的真值。**这是唯一能拿到性质数值的 tool**，也是判分用的真值来源。

**一次可以查多个性质**（`props` 是数组，已实测：`props:["tc","family"]` 一次返两项值）。

> ⚠️ **扣费语义在本 server 上无法验证**：`mock_server.py`（9 工具契约层）的返回里**没有任何预算字段**（实测只有 `id` 与 `values`）。credit 机制实际存于 workspace agent 用的那个 server（`src/probe_headless/ws_mcp_server_dopant.py`）。因此“查一次扣一次”还是“按性质个数逐项扣”，**得到那个 server 上实测才能定，本文不下断言**。

**可查的 12 项**（大小写敏感、精确拼写、不做同义词纠错）：

| 性质名 | 含义 | 适用侧 |
|---|---|---|
| `logS` | 水溶解度（对数） | 药物 |
| `logD` | 分配系数 | 药物 |
| `bbb` | 血脑屏障通过性 | 药物 |
| `caco2` | 肠道通透性 | 药物 |
| `herg` | 心毒风险 | 药物 |
| `half_life` | 半衰期 | 药物 |
| `tc` | 超导临界温度 | 材料 |
| `family` | 超导体家族 | 材料 |
| `condition` | 制备条件 | 材料 |
| `structure_in_db` | 结构是否入库 | 材料 |
| `atom_count` | 原子总数 | **只有分子侧**（晶体实体查不到，见下）|
| `element_count` | 元素种数 | 两侧 |

> ⚠️ **`atom_count` 分实体类型（实测 2026-08-14）**：分子实体 `z000007` 查它**正常返 16**；晶体实体 `c00064` 查它返 `UNKNOWN_PROPERTY`。原因是晶体投影里没有这个字段。**连带后果**：ADR-0025 §8 登记的“现约束含 `atom_count`”若用在**材料题**上无解，用在药物题上没事。另外错误信息自相矛盾（说不可查，列的清单里又有它）——因为判断用当前实体字段集、列清单用全库并集，**待修（一处改动，改成列当前实体可查集）**。

**各性质的库内覆盖量级（实测 2026-08-14）**：值源 `get_property_A.jsonl` 240,467 行、覆盖全部 92,153 个实体。晶体五项全满值（各 17,252）、分子 `atom_count`/`element_count` 全满值（各 74,901）；ADME 六项稀疏——`logS` 2,272 / `bbb` 730 / `logD` 582 / `half_life` 503 / `caco2` 220 / `herg` 仅 98。完整表见 `search_candidates` 一节。

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 187ms / max 187ms，通过 20/20
**输入**（已对照 `tools/list` 的 inputSchema 实测校正 2026-08-14）：`id` 实体标识串（必填）、`props` 性质名**数组**（必填）。

> ⚠️ 旧文档曾写作 `smiles` + `prop`（单个），**与代码不符**，已改。下面例子里的 `smiles`/`prop` 字段名也属旧形态，只看语义，不要照搬字段名。

**输出**：`id` 与 `values`（按 `props` 逐项给值）。布尔用 1 和 0，类别型用字符串。

```
输入  {"smiles": "CN1C=NC2=C1C(=O)N(C)C(=O)N2C", "prop": "logS"}

输出  {"smiles": "Cn1cnc2c1c(=O)n(C)c(=O)n2C", "prop": "logS", "value": -1.02}
```

**查不到时的兜底**：agent 自己想出来的新结构，规范化之后库里没有，不报错，而是返回一句说明——

```
输入  {"smiles": "<agent 推出的新分子>", "prop": "logS"}

输出  {"smiles": "<规范化后的写法>", "prop": "logS", "value": null,
       "status": "NOT_FOUND",
       "note": "该分子是新分子或不存在，暂未探测到其性质"}
```

两点要记住：

- **兜底也扣 credit**。查一个库里没有的分子同样算做了一次实验，防止 agent 拿它免费试探哪些分子存在。
- **答案侧的分子和库外的新分子返回完全一样的兜底**。所以从"查得到值 / 说没有"里分辨不出哪个是答案——这是有意的，agent 只能靠推理不能靠探测。

**两种不扣 credit 的用法错误**（它们不是做实验，只是写错了）：

- **性质名写错**（不在上面 12 项里）返 `UNKNOWN_PROPERTY`。
- **SMILES 语法非法**（化合价不合、括号不配对、环闭合编号不对）返 `INVALID_STRUCTURE`，并指明错在哪里。它只告诉你"你写的这个东西化学上不存在"，**不会替你改**。

**这里有一个被押注的设定**：它是**查表**，库外分子一律返 `NOT_FOUND`。这正是我们不进优化搜索赛道的地基——爬山搜索在这里无利可图（ADR-0025 §4）。换成真模型现场算分，这条地基就没了，详见 `crystal_predict` 与 `predict_admet` 两节。

---

## `submit_answer` 收尾

**基础 tool ｜ 已实现 ｜ 已在用（所有档位）**。标志一道题做完了，并交上这道题要求的答卷。

**怎么用**：submit 之前，agent 先读本次题目给的 response 契约（这道题要交哪些字段），按它填好，再调 submit 交上去。

**它会校验，别以为不校**（实测 2026-08-14 校正）：submit 当场按一份全局 `answer_schema` 校验，缺字段直接报 `SCHEMA_VIOLATION` 并列出必填项（实测为 `anchor_molecules`、`answer`、`candidates`、`cited_values`、`edit_op`、`ranking`，多余字段也会被拒），且**重交机会有限**——实测第二次即报“重交机会已用完”。

> ⚠️ 旧文档写“不做统一格式校验”“submit 本身不校验”，**与代码不符**，已改。

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 13ms / max 13ms，通过 20/20
**输入**：`payload` 本题要求的答卷对象（字段由该题的 response 契约定）。

**输出**：`accepted` 是否收下。

```
输入  {"payload": { ...按本题 response 契约填的字段... }}

输出  {"accepted": true}
```

一道题只能成功交一次，重复交报 `DUPLICATE_SUBMISSION`。

---

## `search_candidates` 批量取候选

**便捷 tool ｜ 已实现 ｜ 未启用（代码已注册可调，属 B4 待清）**。按过滤条件一次拿一批候选实体。

**替代的推理**：**想候选**——本该由 agent 凭领域知识提出，改成一次查询直接拿一批。

**时延（2026-08-15 单线程实测，n=20）**：P50 36ms / P95 587ms / max 587ms，通过 20/20
**输入**（已实测校正 2026-08-14）：`constraints` 过滤条件数组（每条给 `property`/`op`/**`threshold`**，`op` 只收 `lt`/`le`/`gt`/`ge`/`eq`/`ne`，不收 `>=` 这类比较符）、`keywords` 关键词（**字符串，不是数组**）、`k` 要几条（上限 50）、`offset` 偏移。`constraints` 与 `keywords` **至少给一个**，都不给报 `INVALID_ARGUMENT`。

> ⚠️ 旧文档写的是 `value` 与“`op` 取 ge/le/eq”，**两处都与代码不符**，且漏了 `keywords` 与 `offset`，已改。

**输出**：`candidates`（每条给 `id`/`entity_type`/`structure_hint`）、`total_found` 命中总数、`returned`、`offset`。命中按 **ID 字符串升序**（确定性，分页稳定）；无命中报 `EMPTY_RESULT`（**禁止 fallback**，不退回全库）。

```
输入  {"constraints": [{"property": "tc", "op": "ge", "threshold": 30}], "k": 3}

输出  {"returned": 3,
       "candidates": [
         {"id": "c00062", "entity_type": "crystal",
          "structure_hint": "FORMULA:[44*]0.01[45*]2[68*]0.99;SPACEGROUP:P 6/m m m;SYSTEM:hexagonal;LATTICE:3.07,3.07,3.53"},
         {"id": "c00063", …}, {"id": "c00064", …}],
       "total_found": 6287, "offset": 0}
```

（tc≥30K 的晶体共 6,287 条，首次返回 ID 升序前 3 条；翻页用 `offset`。）

**后端库量级（实测 2026-08-14）**：候选池在 `dag_poc/data/backend/search_candidates/search_candidates_A.jsonl`，共 **92,153 条**（分子 74,901 + 晶体 17,252），字段为 `id`/`entity_type`/`structure_hint`/`smiles`/`formula`/`filter_props`。约束过滤用的性质值联查 `get_property/get_property_A.jsonl`（240,467 行，覆盖全部 92,153 个实体）。**性质覆盖两极分化，直接决定各约束的候选池上限**：

| 侧 | 性质 | 有值条数 |
|---|---|---|
| 晶体（17,252）| `tc`/`family`/`condition`/`structure_in_db`/`element_count` | **全满值**（各 17,252）|
| 分子（74,901）| `atom_count`/`element_count` | 全满值（各 74,901）|
| 分子 | `logS` | 2,272 |
| 分子 | `bbb` | 730 |
| 分子 | `logD` | 582 |
| 分子 | `half_life` | 503 |
| 分子 | `caco2` | 220 |
| 分子 | `herg` | 98 |

**过滤机制（读代码 + 实测 2026-08-14）**：多条 `constraints` 之间是 **AND**（须同时满足）；`keywords` 是**按 ID 子串**匹配（`kw in id.lower()`——不是领域词检索：搜 `"c0006"` 能出 c00062/63/64，搜 `"cuprate"` 必然 0 命中，找领域文献用 `literature_search`）；两者**同给时取并集**（OR，不是 AND）。约束性质必须在可查性质白名单内（与 `get_property` 同源），白名单外报 `UNKNOWN_PROPERTY` 并附可用清单。

**注意**：它一次就把「建候选池」这件事做完了，等于同时替掉了提候选与初筛两步，是替代推理最彻底的一个。

---

## `get_entity` 看单个实体

**便捷 tool ｜ 已实现 ｜ 未启用（代码已注册可调，属 B4 待清）**。给一个标识串，返回它的组成信息，不返任何性质。

**替代的推理**：**从结构算组成**——分子式、原子数都能从标识串直接数出来，本该 agent 自己算。

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 12ms / max 12ms，通过 20/20
**输入**：`id` 实体标识（必填）。

**输出**：`formula` 化学式、`atom_count` 原子总数、`canonical_structure` 规范化写法。

```
输入  {"id": "z002730"}

输出  {"id": "z002730", "entity_type": "molecule",
       "structure": {"formula": "C8H10N4O2",
                     "canonical_structure": "Cn1cnc2c1c(=O)n(C)c(=O)n2C",
                     "atom_count": 24}}
```

**注意**：这几项都是从结构确定性算出的（不查库、不扣实验预算），正因如此它属于 ADR-0025 §8 所说的「机械计算类量」，困难档不给。

---

## `compare` 排序

**便捷 tool ｜ 已实现 ｜ 未启用（代码已注册可调，属 B4 待清）**。给一批标识串和一个性质名，返回排好序的列表。

**替代的推理**：**排序取舍**——一批候选谁优谁劣，本该 agent 自己排。

**时延（2026-08-15 单线程实测，n=20）**：P50 1ms / P95 2ms / max 2ms，通过 20/20
**输入**：`ids` 标识串列表、`key` 排序性质名（都必填）、`ascending` 是否升序（默认降序）。

**输出**：`ordered_ids` 排好序的列表。

```
输入  {"ids": ["z002730", "z002879"], "key": "caco2"}

输出  {"ordered_ids": ["z002879", "z002730"]}
```

**注意**：排序键必须是**已用 `get_property` 查证过的**性质，没查过报 `PROPERTY_NOT_VERIFIED`。它不带来任何新信息，纯粹省去 agent 自己排的工夫。

---

## `check_molecule_exists` 问存在性

**便捷 tool ｜ 已实现 ｜ 未启用（代码已注册可调，属 B4 待清）**。问一个结构在不在答案侧，有次数上限。

**替代的推理**：**判断自己推出的分子在不在答案侧**。

**时延（2026-08-15 单线程实测，n=10）**：P50 2ms / P95 2ms / max 2ms，通过 10/10（n=10：服务端写死 existence 预算 10 次/题）
**输入**：`molecule_id` 结构串（必填）、`attempt` 第几次尝试。

**输出**：`exists` 在不在、`attempt` 第几次、`attempts_remaining` 还剩几次。

```
输入  {"molecule_id": "CC(=O)Oc1ccccc1C(=O)OF"}

输出  {"exists": true, "attempt": 1, "attempts_remaining": 9}
```

**注意**：次数用尽报 `ATTEMPTS_EXCEEDED`。困难档不给它的另一个理由是——`get_property` 的 NOT_FOUND 兜底已经覆盖了存在性判断（ADR-0024 §3），它在基础档里是多余的。

---

## `edit_molecule` 改结构

**便捷 tool ｜ 已实现 ｜ 未启用（代码已注册可调，属 B4 待清）**。在一个锚点分子上按封闭算子集做一次改动，返回新结构。

**替代的推理**：**构造新结构**——想好怎么改之后把结构写出来，本该 agent 自己写。

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 13ms / max 13ms，通过 20/20
**输入**：`anchor_id` 锚点分子、`operator` 改动类型（限封闭算子集）、`detail` 改动细节，三者必填。

**输出**：`result_id` 新分子标识、`normalized_structure` 规范化写法、`applied` 是否成功。

```
输入  {"anchor_id": "CC(=O)Oc1ccccc1C(=O)O",
       "operator": "add_atom", "detail": "F"}

输出  {"result_id": "O=C(O)c1ccccc1OC(=O)CF",
       "normalized_structure": "O=C(O)c1ccccc1OC(=O)CF", "applied": true}
```

**注意**：锚点不在可见侧报 `ANCHOR_NOT_IN_A`，算子超集报 `OP_OUT_OF_SET`。**它是最需要警惕的一个**——ADR-0025 §4 论证过：一旦提供它，最优策略退化成爬山搜索（生成变体→逐个查性质→留最好的→再来一轮），完全不需要化学理解。**给它等于把评测从推理降级为搜索**，故只能出现在最低难度档，且档间差值须如实报告。

---

## `get_neighbors` 找结构邻居

**便捷 tool ｜ 未实现 ｜ 未启用**。给一个种子分子，返回一批结构上相近的分子。

**替代的推理**：**找相关分子**——「下一个更可能的分子是谁」是 agent 的判断。

**输入**：`smiles` 种子分子（必填）、`k` 要几个（可选）。

**输出**：`neighbors` 一批标识串。

**数据状态**：⚠️ **未实现**。邻居关系的数据来源与「相关」的定义尚未确定，**故不给示例**；实物就位后再补可照抄的例子。灵枢侧的 `search_molecule_database` 相似物检索分支可以顶替它，见那一节。

---
## `download_poscar` 下载晶体结构

**时延（2026-08-15 单线程实测，n=10）**：P50 897ms / P95 2.7s / max 2.7s，通过 0/10（假 SID 被拒路径，真外呼 MPDS；有效数据路径需真实 SID）

**扩展 tool ｜ 本地实测可用（已真连 MPDS）｜ 未接入**。按 SID 从 MPDS 下 POSCAR 并解析。

**判据**：查资料 → 将来可进基础档。纯取数，判据干净，无争议。

> 配置注入后已真连 MPDS：探针用编造的假 SID 得到「No valid POSCAR data returned for S1000000」——是 MPDS 说没这条记录，链路是通的（2026-08-14）。

---

## `get_cif_by_formula` 查实验晶体结构

**扩展 tool ｜ 内网边界，暂时搁置 ｜ 未接入**。按化学式从 OSS 库取**实验**测定的 CIF 结构文件。

**搁置说明（2026-08-14）**：36.5 万对象的 CIF 索引存在 OSS，凭证经 KeyCenter 解密，pre/prod KeyCenter 均内网不可达——须内网环境（预发机/办公网/VPN）才能跑，不是代码或配置问题；条目保留待内网验证。

**判据**：查资料 → 将来可进基础档。

**为什么对判分有用**：返回里带**实验 / AI 生成**的区分位（`data_provenance`），这是现成的 hard checkpoint 判据——可以机器断言 agent 用的是实验结构还是生成结构。

---

## `validate_target` 验证靶点

**扩展 tool ｜ ⚠️ 库内只有 GLP-1R 一条，其余返固定常数且无提示 ｜ 未接入**。给一个靶点，返回成药性、3D 结构、结合口袋。

**判据**：查资料 → 将来可进基础档。

---

## `search_pubchem_compound` 查 PubChem 化合物

**时延（2026-08-15 单线程实测，n=10）**：P50 6.3s / P95 8.1s / max 8.1s，通过 10/10

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。按化学名或 SMILES 查 PubChem，返回 CID、分子式等。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `get_uniprot_target_profile` 查 UniProt 靶点档案

**时延（2026-08-15 单线程实测，n=20）**：P50 911ms / P95 3.1s / max 3.1s，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。按基因名或登录号查 UniProtKB 靶点档案（默认人类）。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `search_pubmed_evidence` 查 PubMed 文献证据

**时延（2026-08-15 单线程实测，n=10）**：P50 4.1s / P95 7.4s / max 7.4s，通过 10/10

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。按生物医学问题查 PubMed，返回 PMID、标题等。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `search_chembl_activities` 查 ChEMBL 活性数据

**时延（2026-08-15 单线程实测，n=10）**：P50 4.4s / P95 7.1s / max 7.1s，通过 10/10

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。查一个靶点的活性测量值。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。**这个尤其危险**：活性值就是性质真值，等于绕过 `get_property` 直接拿数。

---

## `get_opentargets_association` 查靶点疾病关联

**时延（2026-08-15 单线程实测，n=20）**：P50 1.6s / P95 4.2s / max 4.2s，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。查 OpenTargets 里一个靶点与疾病的关联强度。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `search_clinical_trials` 查临床试验

**扩展 tool ｜ 实测外部 API 403（补丁已备待应用）｜ 未接入**。按疾病、靶点、药名或自由文本查 ClinicalTrials.gov。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `search_openfda_drug_label` 查药品说明书

**时延（2026-08-15 单线程实测，n=10）**：P50 3.3s / P95 11.1s / max 11.1s，通过 10/10

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。按通用名或商品名查 openFDA 说明书，返回适应症等。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `get_pdb_structures` 查 PDB 蛋白结构

**时延（2026-08-15 单线程实测，n=20）**：P50 1.8s / P95 4.6s / max 4.6s，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。按靶点文本搜或按 PDB ID 取 RCSB PDB 实验结构。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `get_alphafold_structure_summary` 查 AlphaFold 预测结构

**时延（2026-08-15 单线程实测，n=20）**：P50 2.6s / P95 5.0s / max 5.0s，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。按 UniProt 登录号或基因名取 AlphaFold DB 预测结构摘要。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `get_reactome_pathways` 查 Reactome 通路

**时延（2026-08-15 单线程实测，n=20）**：P50 1.3s / P95 4.5s / max 4.5s，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。按基因、蛋白、疾病过程或关键词查 Reactome 通路。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `get_string_interactions` 查蛋白互作网络

**时延（2026-08-15 单线程实测，n=20）**：P50 692ms / P95 6.5s / max 6.5s，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。按基因符号取 STRING 蛋白-蛋白互作网络的边。

**判据**：查资料 → 将来可进基础档，但须先过开头写的**污染体检**。

---

## `crystal_predict` 预测晶体性质

**扩展 tool ｜ 内网边界，暂时搁置 ｜ 未接入**。从 CIF 算晶体性质：Tc、形成能、是不是超导。

**搁置说明（2026-08-14）**：输入 `cif_key` 要靠 `get_cif_by_formula` 从 OSS 取，而后者卡在 KeyCenter 内网边界——上游通了它才能测；条目保留待内网验证。

**判据**：做实验 → 判据上可进基础档，**但接不接是赛道选择**。

**接入前要注意（这是本清单里最重的一个风险）**：它是我方 `get_property` 材料侧的真实对应物——**我方查表，它现场算**。目前库外分子一律返 `NOT_FOUND`，这正是 ADR-0025 §4 押注"不进优化搜索赛道"的地基：可评分空间小，爬山无利可图。一旦换成现场算，**oracle 就能给任意分子打分，爬山搜索立刻变成有效策略**，直接把我们推回 GuacaMol / Tartarus / TDC-PMO 那条赛道——而那条赛道上我们的可评分空间小若干数量级，必输。**故接入与否需单独立 ADR，不能当作普通工具接入处理。**

---

## `predict_admet` 预测 ADMET

**时延（2026-08-15 单线程实测，n=10）**：P50 517ms / P95 899ms / max 899ms，通过 10/10

**扩展 tool ｜ 本地实测可用（真调药物发现 API，Uni-Mol + RDKit）｜ 未接入**。给 SMILES 预测 ADMET 与理化性质。

**判据**：做实验 → 判据上可进基础档，**但接不接是赛道选择**。

**接入前要注意**：同 `crystal_predict`，它是我方 `get_property` 药物侧（`logS`/`logD`/`bbb`/`caco2`/`herg`/`half_life`）的真实对应物，同一条赛道风险，同样需单独立 ADR。

---

## `predict_binding` 预测结合亲和力

**时延（2026-08-15 单线程实测，n=10）**：P50 3.1s / P95 3.2s / max 3.2s，通过 10/10

**扩展 tool ｜ 本地实测可用（真调 DiffDock-L + Uni-Dock）｜ 未接入**。给蛋白与配体，返回结合亲和力与接触残基（3D 对接）。

**判据**：做实验 → 将来可进基础档。

**接入前要注意**：它给的是我方目前完全没有的性质维度（亲和力），不与 `get_property` 的 12 项重叠，**所以赛道风险比上两个小**——它不构成"任意分子都能打分"的通用 oracle，只在有靶点的题里生效。

---

## `simulate_virtual_cell` 虚拟细胞扰动模拟

**时延（2026-08-15 单线程实测，n=10）**：P50 20.4s / P95 21.5s / max 21.5s，通过 10/10

**扩展 tool ｜ 本地实测可用（真调 Cell API）｜ 未接入**。用 Lingshu-Cell 模拟基因扰动后的细胞响应。

**判据**：做实验（计算实验）→ 将来可进基础档。

**接入前要注意**：**它是异步的**，只返 `task_id`，必须再调 `check_cell_simulation` 取结果。两点连带问题：一是预算怎么算（轮询算不算一次调用？不定就有人拿轮询刷预算）；二是它天然构成一条多步依赖链，**这对长程 checkpoint 是加分项**——提交任务、轮询、拿结果三步都可以单独设卡。

---

## `check_cell_simulation` 取虚拟细胞任务结果

**时延（2026-08-15 单线程实测，n=10）**：P50 64ms / P95 79ms / max 79ms，通过 10/10

**扩展 tool ｜ 本地实测可用（配对轮询实测通过）｜ 未接入**。按 `task_id` 查上一项的状态与结果。

**判据**：做实验（与 `simulate_virtual_cell` 配对）→ **不单独给**，只能跟着上一项一起接。

---

## `crystal_generate` 生成晶体结构

**时延（2026-08-15 单线程实测，n=10）**：P50 20.2s / P95 21.0s / max 21.0s，通过 10/10

**扩展 tool ｜ 本地实测可用（真调 GPU 生成服务）｜ 未接入**。按化学式用 Elements-G 扩散模型生成晶体结构。

**判据**：替代推理（**提候选**）→ **只能进便捷档**。

**它对应我方哪个便捷 tool**：`search_candidates` 的材料版。**它是现成的、有真后端的便捷 tool 候选，比我方自己写的 mock 便捷 tool 更可信**——便捷档如果要做，优先用它。

---

## `check_crystal_generation` 取晶体生成任务结果

**时延（2026-08-15 单线程实测，n=10）**：P50 62ms / P95 106ms / max 106ms，通过 10/10

**扩展 tool ｜ 本地实测可用（配对轮询实测通过）｜ 未接入**。按 `task_id` 查上一项的状态与结果。

**判据**：替代推理（与 `crystal_generate` 配对）→ **不单独给**。

---

## `generate_molecules` 从头生成分子

**时延（2026-08-15 单线程实测，n=10）**：P50 27.5s / P95 27.6s / max 27.6s，通过 10/10

**扩展 tool ｜ 本地实测可用（真调 DiffSBDD GPU 生成）｜ 未接入**。针对一个靶点从头生成一批候选分子。

**判据**：替代推理（**提候选**）→ 只能进**最低**难度档。

**为什么它是整个清单里最需警惕的一个**：ADR-0025 §4 论证过，给了结构编辑工具，最优策略退化为爬山搜索。我方 `edit_molecule` 至少还限在封闭算子集、一步一改；**这个是直接吐一批新分子，连"改哪里"都不用想了**。它替掉的不只是构造结构，而是整个"凭化学理解提候选"的环节。只能出现在最低难度档，且档间差值须如实报告。

---

## `compare_materials` 材料结果排序

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 2ms / max 2ms，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 未接入**。把一批材料预测结果按 tc、形成能、带隙等排序对比。

**判据**：替代推理（**排序取舍**）→ 只能进便捷档。

**它对应我方哪个便捷 tool**：`compare` 的材料版。

---

## `get_or_generate_cif` 查不到就生成结构

**扩展 tool ｜ 内网边界，暂时搁置 ｜ 未接入**。给一个化学式，先查实验 CIF 库，查不到就转 AI 生成。

**搁置说明（2026-08-14）**：与 `get_cif_by_formula` 同因——CIF 索引卡在 KeyCenter 内网边界；条目保留待内网验证。

**判据**：替代推理（**"查不到该怎么办"的决策**）→ 只能进便捷档。

**它替掉了一个我方便捷 tool 里没有的东西**：不是替提候选也不是替排序，而是**替掉了 agent 遇到数据缺失时的应变判断**（该退而求其次用生成结构，还是换一个化学式）。我方无对应物，是新增的一类，若用于分档须单独记账。

---

## `search_molecule_database` 查相似分子 / 虚拟筛选

**扩展 tool ｜ 本地未命中则返空（如实告知）｜ 未接入**。查 ChEMBL/PubChem 相似化合物，**或**跑 GPU 虚拟筛选。

**判据**：**混合，不能整个接**。一个 tool 里包了两种完全不同的语义：

- **查相似物** → 这是"找相关分子"，等于我方的 `get_neighbors`，**属替代推理**，只能进便捷档；
- **跑虚拟筛选** → 这是真算一批打分，**属做实验**，相当于批量 `get_property`。

**结论**：若要用必须先拆成两个 tool 分别定档。否则一个名字同时给了候选与打分，便捷度无法归因（违反 ADR-0026 §2 的归因前提）。

---

## `echo_text` 回显

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 7ms / max 7ms，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 不接入**。原样回显输入，验 MCP 链路通不通。

**判据**：不属业务工具面，**永不给 agent**。

---

## `poscar_to_cif` POSCAR 转 CIF

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 13ms / max 13ms，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 不接入**。POSCAR 文本转 CIF 文本，纯格式转换，不调模型。

**判据**：系统内务，**不给**。理由同 `normalize_structure`——规范化与格式转换是系统内务，**内化进各 tool**，agent 不该感知（ADR-0025）。

---

## `preview_structure_file` 结构文件摘要

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 6ms / max 6ms，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 不接入**。解析结构文件，返回化学式、原子数、空间群。

**判据**：系统内务，**不给**。这些全是从结构确定性算出的**机械计算类量**（ADR-0025 §8），等于我方便捷 tool 里的 `get_entity`。若非要给，只能进便捷档。

---

## `generate_report` 出材料报告

**扩展 tool ｜ 内网边界，暂时搁置 ｜ 不接入**。出 PDF 报告，带性质表、图表、晶体结构可视化。

**搁置说明（2026-08-14）**：报告产物上传 OSS artifacts 档位，配置注入后同样卡在 KeyCenter 内网边界（本机实测 `ARTIFACT_UPLOAD_ERROR`）；本 tool 判据本就定为不接入，搁置不影响清单结论。

**判据**：交付动作，**不给**。我方用 `submit_answer` + `answer_schema` 契约代替，形式更可机器核验。

---

## `generate_drug_report` 出候选药排序报告

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 6ms / max 6ms，通过 20/20

**扩展 tool ｜ 本地实测可用 ｜ 不接入**。出候选药**多目标加权排序**报告。

**判据**：**双重替代，坚决不给**。它既替了排序、又替了权衡，而"多个目标冲突时如何取舍"**正是约束优化题要测的那一项**（ADR-0025 §5）。给了等于直接送分。

---

## `normalize_structure` 结构规范化

**时延（2026-08-15 单线程实测，n=20）**：P50 2ms / P95 13ms / max 13ms，通过 20/20

**已作废 ｜ 我方代码里仍注册 ｜ 应删未删**。把不同写法的结构串归一到标准形式。

**作废理由**：**功能内化进各 tool**。ADR-0025 定：规范化是系统内务，agent 不该感知；每个 tool 收到标识串后自己在内部归一。

**⚠️ 代码与文档不一致**：它已判作废，但**仍在 `CONTRACT_REGISTERED_NAMES` 的 9 个里、agent 现在调得到**。删它必须同步改 `CONTRACT_REGISTERED_NAMES`、`_assert_tool_surface()` 的数量断言和 `tools_schema.json`，否则模块加载即失败。此为待清项。

---

## `resolve_structure` 拿结构换 ID

**已作废 ｜ 未实现 ｜ 不再提供**。给一个结构串，返回它在库里的 ID。

**作废理由**：**ADR-0022 已撤销**。它原本是「拿结构换 ID」的桥，而 ADR-0024 把 tool 的 key 直接改成标识串，桥不再需要。

---

## `eval_edited_structure` 评估改后结构

**已作废 ｜ 未实现 ｜ 不再提供**。给一个 agent 改出来的新结构，返回它的性质。

**作废理由**：**等价于 `get_property`**。key 改为标识串后，agent 改出的新结构直接用 `get_property` 查即可，不需要第二个入口。
