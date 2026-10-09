# ADR-0024 · 工具集精简为 SMILES 原生的 5 工具

- 状态：已裁定（Owner 2026-08-09）；**§2 工具集已由 [[0025]] 进一步收窄为 3 个（`get_neighbors` 删）**
- 决策者：Owner
- 关联：**Supersedes [[0022]]**（resolve_structure 不再需要）· 承接 [[0023]]（取消匿名、agent 见真实 SMILES）· **判据依据见 [[0025]]**（科学家代入法）
- 起因：逐个审视工具后发现半数要么替 agent 代劳、要么无用、要么 agent 不该感知

---

## 1. 一句话决定

**工具集从 9 个精简到 5 个，全部以 SMILES 为分子标识（不再用内部 ID）。** 删掉替 agent 代劳的、无用的、和 agent 不该感知的四个工具，把它们的功能要么内化进保留工具、要么直接砍掉。

## 2. 最终工具集

| 工具 | 处置 | 关键改动 |
|---|---|---|
| `literature_search` | 留 | 不变 |
| `get_entity` | 留，改 key | 入参从 `id` 改 `smiles`；返规范化写法 + 分子式 + 原子数，从 SMILES 直接算，不查库不扣费 |
| `get_property` | 留，重构 | 入参从 `id` 改 `smiles`；一次只查一个性质；每次扣查证预算；查不到走兜底 |
| `submit_answer` | 留，改语义 | 只当结束标志 + 填本题 response 契约，不做统一 schema 校验 |
| `get_neighbors` | **已删（[[0025]]）** | 原为候选发现入口；按科学家代入判据，“下一个更可能的分子是谁”属理论推导，不给工具 |
| ~~`compare`~~ | 删 | 排序 agent 自己做，工具不带新信息 |
| ~~`edit_molecule`~~ | 删 | agent 自己改 SMILES 再查，用不到 |
| ~~`normalize_structure`~~ | 删 | 功能内化进各工具（收到 SMILES 自己规范化）；agent 不感知 |
| ~~`resolve_structure`~~ | 删 | key 直接是 SMILES，不需要"结构换 ID"的桥（撤 ADR-0022） |
| ~~`check_molecule_exists`~~ | 删 | 存在性判断内化进 `get_property` 兜底 |
| ~~`search_candidates`~~ | 删 | 已按前序裁定废弃（替 agent 一步做完 cp2/cp3/cp4） |

## 3. 五项逐条理由

**一、`get_entity` 的 key 从 id 改 SMILES。** 原来用内部 ID（如 `z002730`）当 key，可 agent 根本不知道哪个 ID 对应哪个分子，等于拿到一个查不动的钥匙。改成 SMILES 后，agent 用它天然理解的化学写法直接指代分子。这也是"其他做分子 agent 的通行做法"。

**二、`get_property` 建模成"做一次实验"。** 一次只查一个性质、每次扣一次查证预算。理由：查性质在现实里就是做实验，做一次付一次代价，不该让 agent 一次调用把一个分子的所有性质白拿。

**三、`get_property` 加兜底替代存在性接口。** agent 自己推出的新 SMILES，规范化后库里没有时，不报错，返回

$$\text{lookup}(\mathrm{canonical}(s)) = \varnothing \;\Rightarrow\; \{\text{value}: \text{null},\ \text{status}: \text{NOT\_FOUND},\ \text{note}: \text{“新分子或不存在，暂未探测到其性质”}\}$$

其中：

- $s$ —— agent 传入的 SMILES；
- $\mathrm{canonical}(\cdot)$ —— 工具内部的规范化（agent 不感知）；
- $\mathrm{lookup}$ —— 在可见侧（A 层离线库）查该规范化结构；
- 命中则返性质真值，未命中返上式兜底。

**这个兜底顺手消掉了 `check_molecule_exists`**：查性质时若返兜底，agent 就知道这分子不在库里，无需独立的存在性工具。

**四、`normalize_structure` 内化、不让 agent 感知。** 它难实现且不该暴露给 agent。改为：system prompt 声明"所有分子一律 SMILES"，每个工具收到 SMILES 后在内部各自规范化。规范化是工具的内务，不是 agent 要调的一步。

**五、`submit_answer` 只当结束标志。** 原来 submit 对着一份全局 `answer_schema.json` 统一校验，但不同题的 response 契约不同，统一校验没意义。改为：submit 前 agent 先读本题的 response 契约、按它填，submit 只标志结束并交上；核验由 judge 在收卷后按该题自己的契约做。

## 4. 隔离不变量（必须落成断言）

`get_property` 兜底把它变成了存在性探针，故必须守住：

**答案侧（B 层）的分子与库外的新分子，返回完全相同的 NOT_FOUND 兜底。** 否则 agent 能靠"这个查得到值、那个说没有"反推答案侧成员。credit 是探测的唯一上限，不再单设次数帽（原 check_molecule_exists 的 10 次帽随之取消）。此不变量与 ADR-0023 的答案隔离口径一致。

## 5. 连带改动

| 位置 | 改什么 | 状态 |
|---|---|---|
| `dag_poc/docs/AGENT_TOOLS.md` | 重写为 5 工具、SMILES 原生 | ✅ 本次 |
| `governance/adr/0022` | 标 Superseded | ✅ 本次 |
| `dag_poc/tools_schema.json` | 删 5 个工具定义；get_entity/get_property 入参改 smiles；get_property props 改单值 + 兜底 | ⏳ 待 E3 |
| `dag_poc/gateway.py` | 各工具入口内部规范化；get_property 兜底与计费；删已砍工具实现 | ⏳ 待 E3 |
| `dag_poc/mcp_mock_server/mock_server.py` | 注册面从 9 改为 5；`_assert_tool_surface` 基准同步 | ⏳ 待 E3 |
| system prompt | 声明"分子一律 SMILES" | ⏳ 待 E4 |
| `answer_schema.json` | 从"全局统一 schema"改为"每题各自 response 契约" | ⏳ 待 E5 |

## 6. 本 ADR 不裁定的事

- **`get_neighbors` 的邻居定义与数据源**——候选发现的具体机制另行立项，本 ADR 只确认它是发现入口。
- **判分口径**——工具集变了，但 cp1–cp6 的判据本身不在此改。
- **既有 37/61 个 trace**——用旧工具名（含已删的 compare/edit_molecule 等），仍靠 `tools_schema.json` 的 alias_map 解析；新工具集跑出的结果与旧结果不可直接对比。

## 7. 待验证

1. 5 工具端到端跑通一道现有题。
2. 隔离断言：答案侧分子经 `get_property` 返回的兜底，与库外新分子逐字节相同。

---

## 8. 判分层连带裁定（2026-08-09 追加，Owner 三项）

工具集改了，判分层必须跟上。本节裁定三件事，一次性改完只重跑一轮矩阵。

### 8.1 checkpoint 采用两层结构：模板定骨架，每个 query 填实例

**决定**：checkpoint 不由代码写死一套给所有题共用，而是**每个题型（template）有自己的卡点骨架，每道 query 把骨架的槽位填成自己的实例。**

两层各管什么：

| 层 | 内容 | 谁定 |
|---|---|---|
| 骨架（模板层） | 这个题型有哪几关、每关的判分表达式长什么样 | template.json 的 `checkpoint_dag` |
| 实例（query 层） | 表达式里的槽位填什么值（金标准、约束、要查的性质） | 每个 case 文件的 `gt`/`constraints` 等字段 |

槽位机制现状（实测）：reward 表达式引用五个命名空间 `gt`、`response`、`tools`、`tool_call_count`、`tool_call_ids`，每个 case 填自己的值，因此**同一骨架在不同 query 上自然产出不同的 check 实例**。

**这不是新建机制，是把已有的分化做实。** 实测两个在用模板的骨架已经不同：约束满足题是 `cp4_filter`/`cp5_ranking`，带约束优化题是 `cp4_constraint_filter`/`cp5_objective_ranking`，reward 表达式指纹也不同（`cd451f44` vs `a86e27d5`）。

**要改的是代码侧**：`evaluate.py` 里 `CP_WEIGHTS`/`CP_THRESHOLDS` 目前是硬编码常量、注释自称"判分器为唯一权威"，与本裁定冲突。改为**从 template 读骨架与权重**，代码不再持有固定的六关表。

**已知缺口**：`discovery_haystack`（30 道题）没有 template.json，骨架缺失，须补。

**代价如实记录**：骨架按题型分化后，**跨题型的分数不可直接平均**（任务形态不同、关数不同）。同题型内仍可聚合（约束满足 60 题、带约束优化 60 题、大海捞针 30 题各自可比）。对外报结果时须按题型分开报，不报一个全局平均分。

### 8.2 删除 cp2（候选建池）

**决定**：cp2 整关删除。

**理由（Owner）**：cp2 的核心因子是"agent 交的候选必须出现在它的工具检索记录里"，用来抓凭空编造。但**agent 凭自身化学知识提出的候选，是真知识而非编造，不该判错**。罚它等于逼 agent 放弃推理、退化成纯检索机器，与本 benchmark 要测结构推理的目标相反。

补充证据（三个因子逐个被覆盖）：

$$\text{cp2} = \underbrace{\mathbb{1}[|R|>0]}_{\text{①}} \times \underbrace{\frac{|R \cap C|}{\max(1,|R|)}}_{\text{②}} \times \underbrace{\mathbb{1}[g \in R]}_{\text{③}}$$

其中：

- $R$ —— agent 交的候选池（`response.ranking`）；
- $C$ —— agent 经工具检索到过的分子集合；
- $g$ —— 金标准答案。

因子①（交没交）被 cp4/cp5/cp6 覆盖，池空则三者皆零；因子③（答案在池内）被 cp6 完全覆盖且 cp6 更严（权重 2、阈值 1.0）；因子②按上述理由本身不该罚。**三个因子无一独立成立。**

**连带**：cp2 是唯一依赖候选来源工具的判据，删除后 `get_neighbors` 不再背判分责任，只作 agent 探索用途；前序"cp2 依赖已废 `search_candidates`"的冲突随之消失。

### 8.3 分子标识全面改为规范化 SMILES

**决定**：`gt`、`response`、判分比对一律用规范化 SMILES，不再用内部 ID。

**理由**：工具集已全部 SMILES 原生（ADR-0024 §2），**agent 手里没有内部 ID，填不出 `ranking`/`answer`**。这不是优化选项，是不改则题跑不了。

判分口径（以终答为例）：

$$\text{cp\_answer} = \mathbb{1}\big[\mathrm{canon}(a) = \mathrm{canon}(g)\big]$$

其中：

- $a$ —— agent 提交的答案 SMILES（`response.answer`）；
- $g$ —— 金标准分子的 SMILES（`gt.smiles`，从原 `gt.id` 迁移）；
- $\mathrm{canon}(\cdot)$ —— 判分侧规范化，与工具内部所用为同一函数；
- 两侧都规范化后比对，故 agent 用哪种等价写法都认。

**附带收益**：内部 ID 是自编号、换库即失效；规范化 SMILES 是化学身份，跨库稳定，既有 trace 重判不会因换库而失配。

### 8.4 落地清单与影响

| 位置 | 改什么 |
|---|---|
| `evaluate.py` | 骨架与权重改为从 template 读；删 cp2 相关分支；比对改规范化 SMILES |
| 三个在用 template | 删 cp2 关；权重重算；`gt.id` 槽位改 `gt.smiles` |
| `discovery_haystack` | 补 template.json（骨架缺失） |
| 150 个 case 文件 | `gt.id` → `gt.smiles`（批量迁移） |
| response 契约 | `candidates[].id`/`ranking`/`answer` 改 SMILES |

**既有矩阵结果全部失效**：关数变了、分母变了、标识体系变了。须重跑，且不与旧结果对比。

### 8.5 本节不裁定

- **每个题型该留哪几关**——须先实测（尤其 `discovery_haystack` 在六关下哪几关是摆设），有据再定，不凭感觉裁。
- **`get_neighbors` 的邻居定义与数据源**——另行立项。
