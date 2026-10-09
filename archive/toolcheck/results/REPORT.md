# 43 个 tool 实测报告

- 实测时间：2026-08-14
- 被测对象：我方 mock server 9 个（`127.0.0.1:9100`）+ 灵枢 MCP 34 个（`127.0.0.1:9000`，本地部署）
- 两条独立证据：`probe_all.py` 直连 MCP 逐个调（确定性基准）+ claude code 实例实跑（43/43 全部调到，44 轮 84 秒）
- 隔离已验证：claude 实例里工具面**恰为 43 个，内置 0 个，其他 MCP 0 个**，见 `init_isolation_proof.jsonl`

## 一句话结论

**当前工具面 39 个（我方 9 + 灵枢 30），32 能用、7 不能用。**

> ⚠️ **本节推翻了本报告之前两版的核心结论。** 前两版写“不能用的 13–14 个全是缺后端/缺凭证，本地修不了”——**错了。它们大部分只是配置没喂进去，是我自己用错了**。根因与修正过程见下一节。

| | 能用 | 不能用 |
|---|---|---|
| 我方 mock server（9）| **9**（全部）| 0 |
| 灵枢（30，已排除 4 个文献类）| **23** | 7 |

**修正前后对比**（同一台机器、同一批工具、同一个探针）：

| | 能用 | 不能用 |
|---|---|---|
| 旧：`LINGSHU_DIAMOND_DISABLED=true` 启动 | 25 | 14 |
| 新：把真实 Diamond 配置喂进去 | **32** | **7** |

**多出来的 7 个都是重型工具**：`predict_admet`（0.6s）、`predict_binding`（3.1s）、`generate_molecules`（27.4s）、`crystal_generate`（20.9s）、`check_crystal_generation`、`simulate_virtual_cell`（20.3s）、`check_cell_simulation`。耗时都在十秒以上，说明**真的在跑模型，不是返 mock**。

另有 1 个是我参数给错：`download_poscar` 的 `S1000000` 是我编的假 SID。报错从“`MPDS_SID not configured`”变成“`No valid POSCAR data returned for S1000000`”，说明**它已经真的打到 MPDS 了**，是 MPDS 说没这条记录。归为能用。

### 真正剩下的 7 个

| 根因 | 个数 | tool | 到底能不能修 |
|---|---|---|---|
| **KeyCenter 内网边界** | 4 | `get_cif_by_formula`、`get_or_generate_cif`、`generate_report`、`crystal_predict`（被前两个阻塞）| ❌ 必须在内网环境跑，不是代码/配置问题 |
| 外部 API 403 | 1 | `search_clinical_trials` | ✅ 补丁已备，待应用 |
| 设计上就返 mock | 2 | `validate_target`、`search_molecule_database` | ⚠️ 配置修不了，也不必修（见下）|

**KeyCenter 那 4 个为何真修不了**（已穷尽三条路）：

1. OSS 凭证在配置里是 `credentialCipher` 密文，`_storage.py` 的 `_resolve_credentials` **只要有密文就强制走 KeyCenter**，环境变量兜底仅对 artifacts 档位生效；
2. 配置指向的 `pre-keycenter-service-internal.alibaba-inc.com`（11.29.120.18）与 prod 端点（11.226.7.96）**都是内网 IP，本机 TCP 超时**；
3. 唯一公网可达的 daily 端点已实测：初始化能成功（24.4s）但 `synced=0 keys`，报 `KeyCenter key not authorized for this application/environment`——**pre 环境的密钥只有 pre KeyCenter 能解**。

所以这 4 个的结论是“**换个环境就能跑**”（预发机 / 办公网 / VPN），而不是“坏了”。

**两个 mock 的不必修**：`validate_target` 需要的靶点证据，现在有四个实测可用的真库 tool 能完全覆盖（`get_uniprot_target_profile` / `get_pdb_structures` / `search_chembl_activities` / `get_opentargets_association`）；`search_molecule_database` 的相似检索分支本来就属“替代推理”、困难档不给。

## 为何之前估错了：配置没喂进去

最初我用 `LINGSHU_DIAMOND_DISABLED=true LINGSHU_STORAGE_PROVIDER=local` 启动，因为预发挂了、而我**假设** Diamond 在本机也拿不到。这个假设没验，错在这里。

实际情况：

| 取配置的路 | 结果 |
|---|---|
| python `diamond_client` 传 `unit=pre` | ❌ 地址服务器 **HTTP 400**——`pre` 是 `mw` 的环境标签，不是 Diamond 地址服务器认的 unit |
| python `diamond_client` 不传 unit | ⚠️ 服务器列表 OK（44 台），但 default unit 里**没有这份配置** |
| `mw diamond get --unit pre` | ✅ **能拿到**（走 env-labels 元数据，另一套机制）|

而且关键一点：`LINGSHU_DIAMOND_DISABLED=true` 时 `start()` **直接 `return`，从不调 `replace(content)`**——整个配置快照一直是空的。而 `drug`/`elements`/`cell` 三域的 config **只认 Diamond、不读环境变量**（`literature`/`cif`/`storage` 读），所以那三域的工具全报 "configuration is not available"。

**解法（不改灵枢仓一行）**：`toolcheck/lingshu_with_config.py`。`DomainConfigStore.replace(content)` 是公开方法，而 `server.py` 在导入时已经 `add_listener(_apply_config)`，于是：用 `mw` 取配置 → 置 `DIAMOND_DISABLED` 让 `start()` 早退不崩 → 导入 `server` 后调 `replace()` 将快照灌进去 → 跑 uvicorn。配置**全程只在内存，不落盘**（内含密钥密文）。

**给灵枢仓的建议（可选）**：目前代码**没有本地注入配置的入口**（已搜过，无 `local_file`/`config_file` 之类路径），导致“Diamond 不可达 = 本地无法开发”。建议加一个 `LINGSHU_CONFIG_FILE` 环境变量：Diamond 禁用时从本地 JSON 读配置。这是本次排查挖出的**真正的 MCP 侧缺口**（区别于前面那些“我用错了”）。

## 旧口径的历史记录（以下保留）

| 分类 | 个数 | 含义 |
|---|---|---|
| ✅ 能用 | **26** | 返回真业务数据，或正确地校验并拒绝了坏参数（说明逻辑活着） |
| ❌ 后端没配置 | 12 | 明确报 `xxx not configured` / `INDEX_UNAVAILABLE` |
| ⚠️ 假成功（返 mock） | 3 | **最危险的一档**：不报错、`success` 正常，但 `data_provenance` 自报是 `SHOWCASE_MOCK` |
| ❌ 真报错 | 1 | 外部 API 拒绝（ClinicalTrials.gov 返 HTTP 403） |
| ❌ 被依赖阻塞 | 1 | 自己没问题，但上游拿不到输入，无法独立判定 |

按 server 分：

| server | 能用 | 不能用 |
|---|---|---|
| 我方 mock（9） | **9**（全部） | 0 |
| 灵枢（34） | **17** | 17 |

## 最重要的前提：这次测的是空壳

**灵枢预发环境当时是挂的。** 实测 `POST https://damo-gene-lingshu-mcp.pre-fn.alibaba-inc.com/mcp` 返回：

```
HTTP/2 500
x-error-code: 1108
x-fc-request-id: 2166038017866745574646306e11b9:827c9418265aBVUE
SYSTEM:fiber_data_is_not_health
```

DNS 解析正常（→ 203.119.238.140）、TLS 握手正常、请求确实打到了 FC API Server，是**函数实例本身不健康**，不是我方网络问题。

所以只能退回本地起服务，而本地必须用 `LINGSHU_DIAMOND_DISABLED=true` 启动——**Diamond 一禁，所有凭证（Google Scholar key、MPDS SID、药物发现 API、CIF 索引、Cell API、GPU 生成服务）全都读不到**。

**因此：那 12 个"后端没配置"和 3 个"假成功"，判的是"本地环境下不可用"，不是"这个 tool 坏了"。要下最终结论，必须等预发恢复后用本文件夹重跑一遍。** 反过来，标 ✅ 的 26 个是**下限**——它们连凭证都不需要就能跑通，生产环境只会更好。

## 六个发现

### 一、先统一“mock”这个词，否则下面全读不对

**mock 不等于假数据。** 本项目说的 mock 是：**有一个真库，从里面查真值出来，只是不走完整链路**。值是真的，省的是链路。按这个定义重新分，实测到的东西有**三种**，只有第三种才是问题：

| 种类 | 是什么 | 实例 | 判定 |
|---|---|---|---|
| **查库取真值** | 真数据库，不走真 API | 我方 9 个 tool：读 `dag_poc/data/mock_db.json`（由 `data/sources/` 真数据集构建），实测 `c00064` 的 `tc=36.7`、`z000007` 的 `logS=-1.41` 都是库里真值 | ✅ **正当**，就是该这么做 |
| **手工录入的真值小表** | 硬编码 dict，但值经得起核 | 灵枢 GLP-1R showcase：`P43220` 是 GLP1R 真 UniProt、`7KI0` 是真 PDB、`CHEMBL3137309` 是司美格鲁肽真 ID、苯丁肽类药批准年份全对 | ✅ **也算正当**，但覆盖面只有 GLP-1R 一条产品线 |
| **与输入无关的兜底常数** | 那些都不匹配时返的固定值 | `validate_target` 恒返 `score: 0.65`；`predict_binding` 兜底恒返 `kd_nM: 5.0, delta_G_kcal: -11.3` | ❌ **这才是“编的”** |

### 二、真正会“闷不作声地编数”的只有 `validate_target` 一个

它的库里**只有 GLP-1R 一条记录**。命中就返真值；没命中就返一组与输入无关的固定常数，**且不带任何“这是模拟数据”的提示**。

我探针用 ABL1（伊马替尼靶点，有 2.1Å 晶体结构、多个批准药），它返：

```
"druggability": {"score": 0.65, "assessment": "Moderately druggable",
                  "rationale": "Limited structural data available"}
"conclusion": "Target ABL1 requires further validation. Consider AlphaFold structure prediction."
```

这不是“数据缺失”而是**事实错误**——把一个教科书级成熟靶点说成结构数据有限。对比 `predict_binding` 的兜底，它自带 `"warning": "Simulated data, not from a real docking run"`，**是诚实的**。

> ⚠️ **此处更正本报告初版**：初版把 `search_molecule_database` 与 `simulate_virtual_cell` 也列为“假成功”，**判重了**。读代码后确认：前者兜底只返 `total_hits: 0` 空列表并在 note 里标了 `(mock mode)`，后者兜底只返 `model: "Generic mock"` 并提示要配 `CELL_API_BASE_URL`——**两者都没编数值**，属“未命中则返空并如实告知”，不构成事故风险。

### 三、AGENT_TOOLS.md 里我方 3 个 tool 的参数契约写错了

实测签名（以 `tools/list` 的 inputSchema 为准，完整存于 `schemas.json`）：

| tool | 文档写的 | 实际是 |
|---|---|---|
| `get_property` | `smiles` + `prop`（单个性质名） | **`id` + `props`（数组）** |
| `search_candidates` | 约束项 `{property, op, value}` | **`{property, op, threshold}`**；另有文档未提的 `keywords`（**字符串**，不是数组）与 `offset` |
| `submit_answer` | "不做统一格式校验，字段由该题 response 契约定" | **实际强制校验一个全局 answer_schema**，缺字段报 `SCHEMA_VIOLATION` 并列出必填项（`anchor_molecules`/`answer`/`candidates`/`cited_values`/`edit_op`/`ranking`），且**重交次数有限**（实测第二次即报"重交机会已用完"） |

按"工具调用前必须先验证接口契约"的既有规范，这三处属文档与代码漂移，已在 AGENT_TOOLS.md 回填。

### 四、两处环境陷阱

- **端口撞车**：我方 mock 与灵枢本地版默认都监听 **9000**，且 `~/.claude.json` 里的 `lingshu-mock` 就指向 `localhost:9000`。同时起两个必然有一个失败或连错。本文件夹把我方改到 9100。
- **`normalize_structure` 应删未删**：它在 AGENT_TOOLS.md 里判作废，但仍在 `CONTRACT_REGISTERED_NAMES` 的 9 个里、实测可调通（返 `{"canonical_id":..., "valid":true}`）。

### 五、`atom_count` 不是坏的，坏的是错误信息

单查 `atom_count` 返：

```
{"error_code": "UNKNOWN_PROPERTY",
 "error": "性质 'atom_count' 不可查证。本工具可查值性质：
           ['atom_count', 'bbb', 'caco2', 'condition', 'element_count', ...]"}
```

一句话里自相矛盾。但**根因不是 `atom_count` 坏了**，而是它分实体类型——实测：

| 实体 | 类型 | 查 `atom_count` |
|---|---|---|
| `z000007` | molecule | ✅ **正常返 16** |
| `c00064` | crystal | ❌ UNKNOWN_PROPERTY |

根因在 `dag_poc/gateway.py` 的 `_tool_get_property`：

```python
if p not in entity_props:                      # 判断用【当前实体】的字段集
    vprops = self._value_queryable_props()     # 列清单却用【全库】并集
    return _err('UNKNOWN_PROPERTY', f'性质 {p!r} 不可查证。本工具可查值性质：{sorted(vprops)}...')
```

两个集合不是一个：拒给你是因为【这个晶体】没有 `atom_count`，列清单用的是【全库】并集（分子侧有，所以含它）。

**改法**：把错误信息里的清单换成 `sorted(entity_props)`，并带上实体类型，如“晶体实体 c00064 可查：[condition, element_count, family, structure_in_db, tc]”。**一处改动。**

> ⚠️ **此处更正本报告初版**：初版写“`atom_count` 是个真 bug，会直接弄死含它的题”，**说重了**。它在分子侧完好。真问题只在晶体侧：ADR-0025 §8 登记的“现约束含 `atom_count`”若用在**材料题**上会无解，用在药物题上没事。

### 六、预算字段在契约层 server 上根本不存在

`get_property` 返回只有 `{"id":..., "values":[...]}`，**没有任何预算/余额字段**（全量 43 个 tool 的返回里只有 `check_molecule_exists` 带 `attempts_remaining`）。credit 机制实际存于 workspace agent 用的另一个 server（`src/probe_headless/ws_mcp_server_dopant.py`）。

**含义**：AGENT_TOOLS.md 里“每查一次扣一次查证预算”这句，在契约层 server 上**既不能证实也不能证伪**。两个 server 的语义差异本身就是个隐形风险：同一个 `get_property` 名字、不同的扣费行为。已在文档里标成待定，不代替实测。

## 修复方案：17 个不能用的怎么变能用

按**修它需要什么**分四类。类内难度一样，类间差很多。

### A 类：改几行代码就能修（3 项，**已全部完成**）

| 项 | 根因 | 改法 | 状态 |
|---|---|---|---|
| `get_property` 错误信息自相矛盾 | 判断用 `entity_props`（当前实体）、列清单用 `_value_queryable_props()`（全库并集） | 先列本实体可查集再列全库并集，两个口径各自标明 | ✅ **已修并实测**（`dag_poc/gateway.py`）|
| `search_clinical_trials` 返 403 | **不是限流、不是 URL 错**。ClinicalTrials.gov 的 WAF 在查 TLS 指纹与 UA 是否自洽：httpx 的指纹配上一个“不像 httpx”的 UA 就被当作伪装 | 对这个 host 不覆盖 User-Agent，交由 httpx 用默认值 | ✅ **补丁已验证**，待 PR |
| `validate_target` 兜底闷不作声编数 | 库里只有 GLP-1R，未命中则返固定常数且无提示 | `data_provenance` 改可区分的 `SHOWCASE_MOCK_FALLBACK` + 加 `warning` + 不再编 druggability 分数 | ✅ **补丁已验证**，待 PR |

**为何后两项只给补丁不直接改**：它们在 `gene-damo/damo-gene-lingshu-mcp`，是另一个团队的生产仓，不应由本项目静默提交。补丁在 `toolcheck/patches/`，两份都过了 `git apply --check`，且逻辑已在工作区副本上真跑验证：

| 补丁 | 验证方式 | 结果 |
|---|---|---|
| `A1-clinicaltrials-403-ua.diff` | 用补丁后的 `_json_headers()` 打真实请求 | ClinicalTrials **403 → 200**；PubChem / Reactome / STRING 三个对照仍带自定义 UA、照常 200（无连带影响）|
| `A3-validate-target-honest-fallback.diff` | 套到副本后直接调函数 | ABL1 返 `SHOWCASE_MOCK_FALLBACK` + `warning` + `score: null`；GLP-1R 的策展真值（P43220 / score 0.92）**不受影响** |

**回归校验**：`get_property` 那处改动前后跑同一批测试（charset / backend_source_switch / queryable_props / contract_tools），**基线与改后均为 15 failed / 32 passed，集合一致**——那 15 个本来就红，本次改动零回归。

### B 类：只缺配置项——**已从 4 项降到 1 项**

原本四项，其中三个文献类的已**改判为不接入**（owner 定 2026-08-14）：

| tool | 原本缺什么 | 现在怎么算 |
|---|---|---|
| `fetch_bib_info`、`search_drug_literature`、`screen_literature` | Google Scholar API key | **不接入**。文献一律走我方本地 DOI 库，不要真实搜索 |
| `download_poscar` | `MPDS_SID` | **已打通**（配置注入后真连 MPDS，见顶部纠错；假 SID 报「无此记录」属正常），归为能用 |

**为何文献不用真实搜索**：本地库已经就绪——`dag_poc/data/backend/literature_search/literature_search_A.jsonl`，**2655 条 / 2655 个唯一 DOI**（2026-08-15 扩量：原 309 条 → Crossref 真实题录扩至 2655，药物 1752 / 超导 903，老 309 手工关键词合入 + 新增 title 自动抽取，50 条抽检见 `literature_spotcheck_50.md`），`literature_search` 已在用它。三条好处：**不需凭证、不依赖外网、结果可重现**（真搜索的返回会随时间变，同一道题两次跑拿到不同文献，判分就不可重现）。

**代价要记清**：覆盖缺口从此是**硬约束**——扩量后 `synthesis` 已补（44 条）、`cryogenic` 仅 1 条、`topological insulator` 0 条。要扩覆盖只能**往本地库加条目**（构建脚本 `src/data_build/build_literature_search.py`，Crossref 增量缓存，重跑不联网），不能靠接外部 API 绕过。

### C 类：缺真后端服务（8 项，要么接要么绕）

| tool | 依赖的服务 |
|---|---|
| `predict_admet`、`predict_binding`、`generate_molecules`、`search_molecule_database`（筛选分支）| 灵枢药物发现 API |
| `simulate_virtual_cell`、`check_cell_simulation` | `CELL_API_BASE_URL`（lingshu-cell-api）|
| `crystal_generate`、`check_crystal_generation` | Elements-G GPU 生成服务 |

### D 类：缺数据（3 项）

`get_cif_by_formula`、`get_or_generate_cif`、`crystal_predict`（被前两个阻塞）——靠 36.5 万对象的 CIF 索引，需 OSS 凭证。

### 结论：剩下的 12 项不该在本地修

`download_poscar`（B）+ C 类 8 项 + D 类 3 项 = 12 项，共同特征是“**预发有、本地没**”。在本地逐个补凭证是下策（要拿一堆 key、还要过 KeyCenter）。**主线是等预发恢复后用 `toolcheck/` 重跑一遍，一次性量清**。

### 修完之后的账

| | 能用 | 不能用 | 不接入（主动放弃）|
|---|---|---|---|
| 我方 9 个 | 9 | 0 | — |
| 灵枢 34 个 | 18（+1：`search_clinical_trials` 修回来了）| 12 | 4（四个文献类）|

文献类那 4 个从“缺凭证/待评”改进“**主动不接**”——这不是损失而是减依赖：少四个外部依赖、少三个凭证、且判分从“可能不可重现”变成“一定可重现”。

> **它们已从标杆清单里整体删除**（owner 定 2026-08-14，理由：避免污染）。`research/AGENT_TOOLS.md` 不再收录这四个 tool 的条目，清单内灵枢侧从 34 个降为 **30 个**。删除前的基线在 commit `3929334`，具体是哪四个、为何不接，只在本报告里留记录——**本报告是一次性实测与决策台账，不是工具清单**，放在这里不会被当成可接入项，但能防住日后有人不知情又把它们接回来。

## 能用的怎么变完备

能用不等于够用。现有 26 个能用的里，完备性缺口按优先级排：

1. **两个 server 的 `get_property` 语义不一致（最该先修）**。契约层（`mock_server.py`）不返预算字段，workspace 层（`ws_mcp_server_dopant.py`）有 credit。**同一个工具名、两种扣费行为**，这会直接弄脏档间对比（ADR-0026 要求除工具供给外一切不变）。
2. **晶体侧 `atom_count` 缺值**。要么从 CIF 算出来补进投影，要么材料题的约束不用它。现状是 ADR-0025 §8 登记了用它、而材料侧查不到。
3. **`normalize_structure` 应删未删**。删它要同步改 `CONTRACT_REGISTERED_NAMES`、`_assert_tool_surface()` 的数量断言、`tools_schema.json`。
4. **“做实验”这一类外部能力目前是空的**。灵枢 17 个能用的全是查资料类 + 纯计算类，`crystal_predict`/`predict_admet`/`predict_binding` 全挂。**但这不影响困难档**——困难档的做实验走我方库，不依赖灵枢。反而应该庆幸：接了它们反而有赛道风险（见 `crystal_predict` 一节）。

## 两条证据的分歧分析

claude 实例与直连探针在 43 个里 **37 个一致，6 个分歧**。6 个分歧全是同一个模式：**claude 把"后端没配置但 tool 有响应"和"返回 mock 数据"都算成能用**。

| tool | 直连判定 | claude 判定 |
|---|---|---|
| `check_cell_simulation` | ❌ 后端没配置 | 能用 |
| `check_crystal_generation` | ❌ 后端没配置 | 能用 |
| `generate_molecules` | ❌ 后端没配置 | 能用（它换了参数，走到 mock 分支拿到 50 个候选） |
| `validate_target` | ⚠️ 假成功 | 能用 |
| `search_molecule_database` | ⚠️ 假成功 | 能用 |
| `simulate_virtual_cell` | ⚠️ 假成功 | 能用 |

而且 claude 自己前后不一致：`crystal_generate` 和 `check_crystal_generation` 返回的是**同一个** `GENERATION_NOT_CONFIGURED`，它把前者判"不能用"、后者判"能用"。

**结论：以直连探针为判定基准，claude 实例的自报结论只作为交叉验证，不作为判据。** 这一条对 benchmark 本身也是一个直接教训——让被测 agent 自己判断"我调成功了没有"是不可靠的，必须有独立账本。

## 逐个 tool 明细

### ours（9 个）

| tool | 结论 | 实测依据 | 耗时 |
|---|---|---|---|
| `search_candidates` | ✅ 能用 | candidates 返回 3 条真数据 | 0.07s |
| `literature_search` | ✅ 能用 | papers 返回 2 条真数据 | 0.0s |
| `get_entity` | ✅ 能用 | id, entity_type, structure | 0.0s |
| `get_property` | ✅ 能用 | id, values | 0.0s |
| `normalize_structure` | ✅ 能用 | canonical_id, valid | 0.0s |
| `compare` | ✅ 能用 | 实体 c00063 未查证过性质 tc | 0.0s |
| `check_molecule_exists` | ✅ 能用 | exists, attempt, attempts_remaining | 0.0s |
| `edit_molecule` | ✅ 能用 | result_id, normalized_structure, applied | 0.0s |
| `submit_answer` | ✅ 能用 | 答卷未过 answer_schema 校验（重交机会已用完） | 0.0s |

### lingshu（34 个）

| tool | 结论 | 实测依据 | 耗时 |
|---|---|---|---|
| `echo_text` | ✅ 能用 | echo, skill | 0.0s |
| `poscar_to_cif` | ✅ 能用 | formula, cif_content | 0.0s |
| `preview_structure_file` | ✅ 能用 | type, formula, num_atoms, space_group | 0.0s |
| `literature_search` | ✅ 能用 | papers 返回 2 条真数据 | 1.01s |
| `compare_materials` | ✅ 能用 | sorted 返回 2 条真数据 | 0.0s |
| `generate_report` | ✅ 能用 | report_key, title, num_materials, has_structures | 0.2s |
| `generate_drug_report` | ✅ 能用 | data_provenance, generated_at, method, base_weights | 0.0s |
| `fetch_bib_info` | ❌ 后端没配置 | Google Scholar API key not configured | 0.0s |
| `search_drug_literature` | ❌ 后端没配置 | Google Scholar API key not configured | 0.0s |
| `screen_literature` | ❌ 后端没配置 | Google Scholar API key not configured | 0.0s |
| `download_poscar` | ❌ 后端没配置 | MPDS_SID not configured | 0.0s |
| `validate_target` | ⚠️ 假成功（返 mock） | data_provenance=SHOWCASE_MOCK | 0.0s |
| `predict_admet` | ❌ 后端没配置 | Drug discovery API configuration is not available | 0.0s |
| `predict_binding` | ❌ 后端没配置 | Drug discovery API configuration is not available | 0.0s |
| `search_molecule_database` | ⚠️ 假成功（返 mock） | top_matches 返回 0 条真数据 | 0.0s |
| `generate_molecules` | ❌ 后端没配置 | Drug discovery API configuration is not available | 0.0s |
| `search_pubchem_compound` | ✅ 能用 | results 返回 1 条真数据 | 2.77s |
| `get_uniprot_target_profile` | ✅ 能用 | results 返回 1 条真数据 | 0.91s |
| `search_pubmed_evidence` | ✅ 能用 | results 返回 2 条真数据 | 2.05s |
| `search_chembl_activities` | ✅ 能用 | Data from the ChEMBL Database; review ChEMBL licensing terms before do | 7.88s |
| `get_opentargets_association` | ✅ 能用 | results 返回 2 条真数据 | 0.86s |
| `search_clinical_trials` | ❌ 真报错 | ClinicalTrials.gov request failed: HTTP 403 from https://clinicaltrial | 1.1s |
| `search_openfda_drug_label` | ✅ 能用 | openFDA records are not validated medical guidance; review FDA labels  | 6.49s |
| `get_pdb_structures` | ✅ 能用 | results 返回 1 条真数据 | 1.0s |
| `get_alphafold_structure_summary` | ✅ 能用 | results 返回 2 条真数据 | 1.45s |
| `get_reactome_pathways` | ✅ 能用 | results 返回 2 条真数据 | 2.46s |
| `get_string_interactions` | ✅ 能用 | results 返回 2 条真数据 | 0.62s |
| `get_cif_by_formula` | ❌ 后端没配置 | CIF 索引暂不可用，请稍后重试或联系管理员 | 0.0s |
| `get_or_generate_cif` | ❌ 后端没配置 | CIF 索引暂不可用，请稍后重试或联系管理员 | 0.0s |
| `crystal_predict` | ❌ 被依赖阻塞 | 无法读取指定的 CIF 文件，请确认 cif_key 是否有效 | 0.0s |
| `crystal_generate` | ❌ 后端没配置 | 结构生成服务配置不可用 | 0.0s |
| `check_crystal_generation` | ❌ 后端没配置 | 结构生成服务配置不可用 | 0.0s |
| `simulate_virtual_cell` | ⚠️ 假成功（返 mock） | data_provenance=SHOWCASE_MOCK | 0.0s |
| `check_cell_simulation` | ❌ 后端没配置 | Cell API not configured | 0.0s |

---

> 「能用」含两种：返回真数据，以及正确地拒绝了坏参数（如 `PROPERTY_NOT_VERIFIED`、`SCHEMA_VIOLATION`）——后者证明 tool 的校验逻辑在跑，只是我探针故意给了最小参数。完整原始返回见 `probe_raw.json`。
