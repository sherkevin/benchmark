# 接口层契约（INTERFACE_CONTRACT）· 符号语义 / 答卷单 / 取数网关 / 可执行改写

> **一句话**：`pool ⊆ snapshot.universe`、`agent.value[c][p]` 这些符号不是笔误、也不是多余，它们**在指一个我们还没造出来的东西**——agent 与评测机之间的接口层。本文把这个东西的定义补出来，而不是把符号删掉。
>
> **owner 定性（2026-07-30）**："本质是因为缺少一个标准的 LLM JSON 输出——没办法确定 LLM response 里有没有 `agent.best` 这样的 JSON key，你现在写的 template 本身是不完整的。" 这句话把根因钉到了最准的位置：**答卷格式是 template 的组成部分**，template 少了它就不自包含，判分公式引用的 key 从未被规定过。因此 `template.json` 已补入 `answer_contract`（含 JSON Schema），`case_001_cns.json` 补入其物化版。
>
> 定位：`dag_poc/` 构造区规范文档，服务 PR-09（引擎实现）。**权威顺序的唯一正文在 [`governance/DECISIONS.md`](../../governance/DECISIONS.md) 开头**，本文不重述；目标与方法论正文在根目录 `goal.md`（v3.0.1）。本文与 `template.json` 同层（template 引用本文的符号定义）。
> 贯穿例子：`case_001_cns`（8 个候选分子，分子A = Metoprolol 是唯一全过者）。
>
> **阶段时效声明（2026-08-16 补注）**：本文写于 2026-07-30，其中“匿名化 × 开放空间互斥”（§根因 3、§5）的论证基于当时的匿名化前提。**ADR-0023（2026-08-09 裁定，2026-08-15 全库回滚真实态）已取消实体匿名化**：agent 可见真实 formula/InChIKey/SMILES，防污染改靠任务设计 + 裸答基线。故根因 3 的“两个命名空间对不上”已自然消解（库内就是实名世界，`z*/c*` 仅作索引句柄）；保留原文作决策演化留痕，**不得再作现行口径引用**。隔离支柱仍为 A/B 物理分层（B 不可查 + 存在性接口只返 bool），与身份是否真实无关。
>
> **模板落点（ADR-0009 起）**：`templates/fixed_discovery/`（GT 反构造类）与 `templates/open_discovery/`（开放类）；性质定义格式见 `property_spec.json`，领域清单见 `properties/`。旧 `templates/drug_candidate_discovery/` 已标 superseded，仅作历史快照。

---

## 0. 结论先行（只读这一页也够）

**1）判分表达式里的符号分三类，我们只造好了一类。**

| 层 | 是什么（大白话） | 谁产出 | 现状 |
|---|---|---|---|
| **世界层** W | 考卷背后的真相：宇宙里有哪些分子、每个分子的真值 | 出卷人（`snapshot.json` + `case.json`） | ✅ 已有，字段齐、血缘可追 |
| **答卷层** S | agent 交上来的那张卷子：它的候选池、它查到的数、它的筛选/排序/推荐 | agent | ❌ **不存在**（agent 只输出自由散文） |
| **录像层** T | 考场监控：它真的调了哪些接口、接口返回了什么、什么时刻 | 环境/网关 | ❌ **不存在**（没有网关，就没有录像） |

`pool`、`agent.value[c][p]`、`agent.pass_set`、`grounded()`、`search_calls`、`history` 全部落在后两层——**它们是悬空指针，不是错误符号**。世界层的 `snapshot.universe`、`truth[c][p]`、`G`、`tol[p]` 反而个个都算得出来。

**2）缺的那"一个"= 接口层，由三件套组成：**

| # | 件 | 通俗类比 | 解决什么 |
|---|---|---|---|
| ① | **取数网关**（instrumented gateway） | 考场 + 监考录像 | `history` / `search_calls` / `grounded()` / 真值一致性 |
| ② | **答卷契约**（全项目共用 `answer_schema.json`：3 顶层 + 3 候选字段，strict JSON Schema） | 结构化答题卡，不是作文纸 | `pool` / `agent.value` / `pass_set` / `ranking` / `best` / `fail_property` |
| ③ | **符号绑定表**（symbol table） | 答题卡与判分公式之间的对照表 | 每个符号是哪一层的哪个字段、什么类型、缺了怎么判 |

**3）不删任何符号；只有一条改"归属"。** `pool ⊆ snapshot.universe` 保留，但从"考察 agent 能力的计分项"重新定义为**反幻觉断言**（查交卷单里有没有编造的分子 ID）+ **接口不变量**（网关侧结构上就不该被违反，违反即引擎 bug）。这是它本来该干的活，之前只是被摆错了位置。

**4）顺手修掉三个隐藏 bug**（详见 §6）：`first_failed_prop` 的顺序敏感、排序并列无判定、`no_path_violation` 完全没有数据源。

**5）一条不容乐观的实话**（§7）：8 个分子 × 3 个性质 = 24 次查询就能穷举完，当前 case 的过程分几乎没有难度。接口层建好后必须同时拧"宇宙规模 + 查证预算"两个旋钮，否则区分度还是只能来自终答。

---

## 1. 这些符号各自在干什么活（用处与作用）

**先说结论：五个符号 = 五道关卡的定义域，删任何一个都等于拆掉一道关卡，直接撞红线 1（"不出只看终答的退化版本"）。**

| 符号 | 它守哪道关 | 它专门抓哪种作弊 | 删掉的后果 |
|---|---|---|---|
| `pool` | cp2 候选搜集 | 不建池、直接从记忆里蹦出一个答案 | cp2 消失；cp3/cp6 失去定义域（"逐一说明其余候选为什么不行"无从判起） |
| `pool ⊆ snapshot.universe` | cp2（反幻觉子条件） | 编造一个不存在的实体，再给它编一套达标数值 | 存在性检查消失，编造实体零成本 |
| `agent.value[c][p]` | cp3 性质查证 | 跳过查证、靠常识蒙筛选结果 | **GT-first 方法的心脏停跳**：真值先于题目存在的整个设计失去落点 |
| `grounded(...)` | cp3（工具落地子条件） | 数值编得像真的（背诵 / 幻觉数值） | GOAL §4.2 的"编造产物可被抓出"失效 |
| `pass_set` / `ranking` / `fail_property` / `best` | cp4 / cp5 / cp6 | 筛错、排错、归因错、决策错 | 只剩终答，过程量归零 |

下面逐条展开，每条都用 `case_001_cns` 的真数据走一遍。

### 1.1 `pool` —— 过程量的第一个观测点

**作用**：证明 agent **真的建过候选池**，而不是一步跳到答案。长程任务的第一步就是"把搜索空间变成一张候选表"，这一步在真实药物发现里对应"从文献/分子库里捞一批可查的分子"。

**它同时是后面所有关卡的定义域**：cp3 判"池里每个候选的性质有没有查对"，cp6 判"池里每个落选者的理由对不对"。池子没定义，这两句话都是空话。

**真例子**：理想 pool = {分子A…分子H}（8 个）。若 agent 只写 "我推荐分子A"，pool 为空 → cp2 判 0，且 cp3/cp6 因定义域为空**不能算满分**（空集上 `all(...)` 恒真，这是当前表达式的一个真空通过漏洞，见 §6.4）。

### 1.2 `pool ⊆ snapshot.universe` —— 存在性（反幻觉），不是"限制 agent 只能在我们清单里选"

**它想抓的**：agent 声称"候选 X 的 bbb=1、herg=0、caco2=-4.2，我推荐它"，而 X 根本不存在。这在真实科学 agent 里是高频失效模式（编 CAS 号、编文献、编性质）。

**它被摆错的地方**：`snapshot.universe` 只有 8 条 mock 记录，而 PR-05 明确定了"开放空间，agent 自行检索建池"。两条设计对撞：
- 若真开放 → agent 会报 "Metoprolol / 阿司匹林" 这类真实名字，永远 ⊄ {分子A…分子H} → **合法探索被判死**；
- 若给了清单 → 候选空间根本不开放，`⊆` 恒真，cp2 变成免费分。

**正确形态**：`⊆` 的右侧不该是"我们那 8 条"，而应是**网关背后的实体表**（agent 唯一能触达实体的地方）。于是：
- 对**交卷单**求值：`set(sub.pool) <= world.universe` = 反幻觉断言（有意义，因为交卷单是自由 JSON，agent 完全可以往里塞不存在的 ID）；
- 对**网关**求值：恒真 = 接口不变量（agent 只能引用检索得到的 ID；若违反，是引擎/网关的 bug，不是 agent 的失分项）；
- 再加一条更强的伴生条件 `all(c in trace.queried for c in sub.pool)`：不仅"存在"，而且"你确实检索到过它"——把 `pool` 从"声称"钉到"录像"上。

### 1.3 `agent.value[c][p]` —— GT-first 方法的心脏

**作用**：`truth[c][p]` 是我们出卷时就冻结的真值（分子A 的 caco2 = −4.35，血缘到 `reference/NobelClaw/证据_admet_gt.md §2.2`）。`agent.value[c][p]` 是 agent 声称查到的值。二者对账 = **"它是不是真把每个候选的每项约束性质都查证过"**。

**为什么不能删**：删了 cp3，过程分只剩"筛选集合对不对"。case_001 的合格集是 {分子A}，8 个候选的合格/不合格是二值 → 靠"CNS 药物常识 + 陷阱组名字都像抗精神病药"就能猜中，**过程分退化为运气**。cp3 是把"运气"排除掉的唯一手段：你不查，就报不出 −4.35 这个数。

**真例子**：`agent.value['分子C']['herg']` 应为 1（Imipramine 阻断 hERG，陷阱组）。agent 若报 0，cp3 亮红——**在它给出最终答案之前，我们就已经知道它会踩坑**。这正是"过程量评测"的卖点句。

### 1.4 `grounded(agent.value, history)` —— 工具落地检查

**作用**：GOAL §4.2 写死了"验证器查产物不认动作"，但配了一条防线：**产物数值必须被真实接口调用支撑**。`grounded` 就是这条防线：交卷单里的每个数，都要能在录像里找到对应的一条接口返回。

**没有它会怎样**：agent 用记忆写出"Metoprolol 的 Caco-2 约 −4.3"（这数在训练语料里），一个接口都不调，cp3 全过。**整套过程评测被记忆绕过**，撞红线 5。

### 1.5 `pass_set` / `ranking` / `fail_property` / `best` —— 四种不同的"想清楚"

| 符号 | 关卡 | 考的能力 |
|---|---|---|
| `pass_set` | cp4 条件筛除 | 会不会把多条约束**同时**应用（陷阱组考的就是"只看 BBB 不看 hERG"） |
| `ranking` | cp5 排序 | 会不会按目标量排序（`-{caco2}` = caco2 越大越好） |
| `fail_property` | cp6 落选归因 | **说得出"分子B 为什么不行"** —— 这是过程量评测最锋利的一刀：终答评测永远问不出这个问题 |
| `best` | cp6 决策 / 无解声明 | 敢不敢在无解时说 NO_SOLUTION（ADR-0006 的负例题考点） |

**`fail_property` 的独特价值值得单说**：agent 说"分子B 不行"，可能因为它真查了 bbb=0，也可能因为它瞎猜。要求它**指名失败性质**，等于要求"结论必须挂在证据上"——而这个判定是纯集合运算，完全 hard，不需要 LLM judge。

---

## 2. 为什么它们跑不起来：三个根因

**结论：不是符号写错了，是三样基础设施没造。**

### 根因 1 · 缺「答卷契约」→ agent 侧符号全部悬空

agent 的原生输出是自由文本。判分需要的是带类型的记录。中间没有一份"答题卡字段定义"，于是 `agent.pass_set` 这种写法只能停留在伪代码。

**注意这里有个陷阱**：如果靠"从散文里抽字段"来补这一层，就必须上 LLM 抽取器 —— 而抽取错误会伪装成 agent 的失分，**判分可信度归零，且实质上把 LLM judge 从后门放进来了**（撞红线 3）。所以答卷层只能走**结构化提交**，不能走事后解析。这也正是那轮评审"结构化字段优先"的正确内核。

### 根因 2 · 缺「取数网关」→ 录像层符号全部悬空，且真值无法一致

`history`、`search_calls`、`grounded()` 都要求一份可信的调用录像。更根本的是：**`truth` 现在只存在于我们本地的 snapshot 文件里，agent 根本拿不到**。若让 agent 去调外部真实工具（灵枢 MCP / 文献检索），会同时炸掉三件事：

1. **真值不一致**：不同预测器给出的 caco2 不同，`|agent.value − truth| <= tol` 无从设 tol；
2. **无录像**：外部工具的调用与返回不在我们掌控内，`grounded` 无法核；
3. **不可复现**：接口漂移 → 同一题不同时间不同分。

`ACTION_RULES.md` §6 早就写了权威口径：**"所有性质数据均来自数据库查询接口，接口返回值即为真值"**。规范里那个"接口"从头到尾没有被实现——**这就是缺的那一个东西最直白的证据**。

### 根因 3 · 命名空间不一致：匿名化 × 开放空间 目前互斥（已消解，ADR-0023）

> **现状注记（2026-08-16）**：本节描述的矛盾随 **ADR-0023 取消匿名化**已不存在——库内即真实化学身份（formula/InChIKey/SMILES 真值，2026-08-15 全量回滚），与外部世界同一命名空间，无需身份解析层。下文保留作当时推论的历史留痕。

mock 机制（PR-04）把分子名抹成“分子A”，防背诵；开放空间（PR-05）要 agent 自己去外部检索建池。两者当时形态下不能共存：

- agent 在外部世界检索到的是 **Metoprolol**，我们的世界里那条记录叫 **分子A**；
- 没有身份解析层，两个命名空间对不上 → `pool` 与 `universe` 无法比较，`truth[c][p]` 无法按 agent 给的名字取值。

**这不是符号的问题，是世界模型的问题**：我们造了一个匿名世界，却让 agent 去实名世界里找答案。

### 复盘：当初删「variables 字典」删错在哪

`CLAUDE.md` 记录了评审修订"删 variables 字典"。复盘后判断：**删的动作对，删的理由错，导致把问题连根一起丢了**。

- 当时那本字典被塞在 **每个 template 里**、并随 case 复制，逐条重复维护 → 位置错、成本高，删它是对的；
- 但符号需要绑定这件事本身没有消失。删掉之后，符号从"有定义但位置错"变成"没有定义"，PR-02 只规定了 `{}` 的书写规范（花括号只包 JSON key），**语义绑定这一半从未被覆盖**——PR-02 标 CLOSED 是超前的。

**正确形态**：符号是**跨 template 通用**的（任何发现类任务都有 pool / 查证 / 筛选 / 排序 / 提交），所以绑定表应该**提到全局一层、由引擎实现一次**，template 只声明"我用到哪些符号"。见 §3.3。

---

## 3. 解决方案：接口层三件套

### 3.1 ① 取数网关（考场）

**设计原则**：agent 触达世界的唯一通道 = 我们自己实现的一组工具；网关背后就是冻结快照；每次调用写一行录像。

按灵枢 23 工具的八类功能收敛（对应表见 `docs/AGENT_TOOLS.md` 文末）成 4 + 1 个最小工具（v1）：

| 工具 | 对应灵枢功能类 | 入参 → 返回 | 录像/计费 |
|---|---|---|---|
| `search_candidates(constraints, k)` | ① 文献检索 / ⑤ 分子库检索 | 约束或关键词 → 一批实体 ID + `agent_visible_fields` 子集 | 记 `search_call`，计检索预算 |
| `get_entity(id)` | ② 结构查询 | ID → 可见字段（存在性） | 记 `lookup` |
| `get_property(id, props[])` | ③ 性质预测 | ID + 性质名 → **快照真值** | 记 `observe`，**计查证预算** |
| `compare(ids, key)` | ⑦ 结果对比 | ID 列表 + 排序键 → 排序（便利工具，可不给） | 记 `compare` |
| `submit_answer(payload)` | —（新增，评测专用） | 答卷单 JSON → 受理/字段校验错误 | 记 `submit`，终态 |

**这一件东西同时解决四个问题**：
1. `history` / `search_calls` = 录像的全表与子集，**免费得到**；
2. `grounded(c,p,v)` = "v 是否等于某次 `get_property` 的返回"，**免费得到**；
3. **真值一致性由构造保证**——接口返回的就是快照值，`ACTION_RULES` §6 的"接口返回值即真值"第一次真正成立；
4. **不依赖灵枢后端真假**（ADR-0004 应然设计原则）：网关是我们的出卷设施，ADR-0003 那份"药物后端是 mock，只能测编排诚实度"的焦虑随之解除——我们的网关就是"真算的数据库"。

**关键副作用（必须承认）**：`get_property` 直接返回真值，所以 cp3 考的不是"预测能力"，而是**"有没有真查 + 有没有如实转述"**。这是有意的（GT-first 的判分口径就是对账），但它把难度责任推给了"宇宙规模 + 查证预算"，见 §7。

### 3.2 ② 答卷契约 = 一份全项目共用的 LLM JSON schema（`dag_poc/answer_schema.json`）

**定位（先说清这个，否则必写肿）**：答卷 schema 不是 checkpoint 的镜像，而是**抽指标的原料**。它只需满足两件事：

1. **真正是个 template**：与领域无关、与 case 无关，**不需要按 case 物化**；全项目一份，住在 `dag_poc/answer_schema.json`（跟符号绑定表同理：跨 template 通用的东西不该住在 template 里）。
2. **checkpoint 需要的 key 都能从里面抽到**（不要求一一对应）：checkpoint 增删不改 schema，只改抽取式。

#### 三个顶层字段 + 三个候选字段，就是全部

```json
{
  "candidates": [
    {"id": "分子A", "measurements": [{"name":"bbb","value":1}, {"name":"caco2","value":-4.35}], "reject": null},
    {"id": "分子C", "measurements": [{"name":"bbb","value":1}, {"name":"herg","value":1}],   "reject": "herg"}
  ],
  "ranking": ["分子A"],
  "answer": "分子A"
}
```

只回答三个问题：**考察了谁（candidates）· 怎么排（ranking）· 选了谁（answer）**。与上一版相比删了三样东西，每样都有理由：

| 删掉的 | 为什么 |
|---|---|
| `verdict`（pass/reject） | **冗余**——`reject is None` 已经表达了合格。冗余字段还会凭空造出"自相矛盾"这种与科学无关的失分模式（cp4 也因此去掉了一条自洽子条件） |
| `enum: [bbb,herg,caco2]` | **那是 case 的事**。性质名合法性由引擎按 `case.props` 校（`UNKNOWN_PROPERTY`），schema 只管形状 → case 不再需要物化 schema |
| `minItems/maxItems = 约束条数` | 同上。"漏查一项性质"改由 cp3 的 `p in sub.value[c]` 拓住，比卡在 schema 层更可归因（能区分"漏查"与"格式错"） |

两条保留的强约束：`value` 允许 `number | string`（布尔用 1/0，类别型如材料线的 `condition=ambient` 给字符串）；`answer` 的值域是动态集合（池内 ID ∪ `NO_SOLUTION`），**enum 表达不了，只能由 cp6 的 logic 判**。

#### 为什么必须是 JSON Schema，而不是题面里写一句"请输出 JSON"

只叮嘱无法确定 response 里到底有没有 `answer` 这个 key。三条通道共用同一份 `response_schema`，判分侧无分支：**主通道** = API 侧 `response_format={"type":"json_schema","strict":true}`（解码器保证 required/类型）；**备通道** = 网关 `submit_answer(payload)` 同 schema 校验；**兜底** = 本地校验 + 一次仅回语法错的重交。strict 同时要求每层 `additionalProperties=false`、所有 property 进 `required`、可选值用 nullable 表达（`reject` 允许 `null`）。

历史坑位（保留备忘）：**strict 不支持动态 key 的对象**，所以 `{"分子A": {"bbb": 1}}` 这种"以实体 ID 作 key"的字典写不出合法 schema，必须用定长 key 的数组表。

#### 抽取映射（要求 2 的完整交圈）

全部 6 个答卷侧符号，均从上述三个字段派生，**无一例外**（定义在 `answer_schema.json#/symbol_extraction`）：

| checkpoint 符号 | 抽取式 |
|---|---|
| `sub.pool` | `{c.id for c in candidates}` |
| `sub.value[c][p]` | `candidates[c].measurements` 中 `name == p` 的 `value` |
| `sub.pass_set` | `{c.id for c in candidates if c.reject is None}` |
| `sub.reject_reasons[c]` | `candidates[c].reject` |
| `sub.ranking` | `ranking` |
| `sub.best` | `answer` |

另有四个常用量**无需加字段**即可派生：空转判定（`len(sub.pool)`）、漏查性质（`case.props - sub.value[c].keys()`）、无解声明（`sub.best == 'NO_SOLUTION'`）、归因覆盖率。**新增 checkpoint 先试派生，派生不出来才讨论改 schema。**

#### 三层分工 + 四条纪律

| 层 | 拦什么 |
|---|---|
| schema（解码器） | key 缺失、类型错、多余 key |
| 引擎绑定层 | 数组表 → 映射；`UNKNOWN_PROPERTY` 忽略该条；`UNKNOWN_ID` **不静默剔除**（留给 cp2 反幻觉） |
| checkpoint logic | 全部科学正确性（含漏查、筛错、排错、归因错、不敢说无解） |

1. **不解析散文**：只认符合 schema 的 JSON。没交卷 = `NO_SUBMISSION`，按答错处理。
2. **缺字段 = 读它的那个 cp 判 0**，其余 cp 照常求值；**禁止用 LLM 抽取器还原字段**。
3. **格式错 ≠ 科学错**：语法层允许一次重交（只回 schema 错误）。
4. **`answer` 允许取 `"NO_SOLUTION"`**（无解题诚实性考点，ADR-0006）。

#### 泛化性校验（已做）

- **跨领域**：材料线 case（`tc` 数值 / `condition` 类别 / `family` 类别）套同一 schema **无需任何修改**——`value` 允许字符串即可承载 `condition=ambient`。
- **跨 case**：`case_001_cns.json` 的 `answer_contract` 现在只有三行（指向共用 schema + 声明本题用到的性质名），**不再拷一份 schema**。
- **覆盖与区分度实跑**：logic 里出现的 6 个答卷侧符号 100% 可抽取；金标准答卷 cp2–cp6 全过；空转答卷（一个候选、不查性质、瞎选）cp2/cp3/cp4 全不过。

### 3.3 ③ 符号绑定表（variables 字典的正确复活形态）

全局一份，由引擎实现；template 只写符号名。层：**W** 世界 / **S** 答卷 / **T** 录像 / **D** 派生（引擎从 W 重算的 gold）。

| 符号 | 层 | 类型 | 定义 / 来源 | 缺失或非法时 |
|---|---|---|---|---|
| `world.universe` | W | set[str] | `snapshot.molecules[].id` 全集 | 快照错误 → 引擎报错，不算分 |
| `world.truth[c][p]` | W | num/bool | 快照记录里 `p` 字段 | 同上 |
| `case.props` | W | list[str] | `slot_values.property_constraints[].property` | 同上 |
| `case.constraints` | W | list | `{property, op, threshold}` | 同上 |
| `case.metric` | W | expr | `slot_values.target_metric`（如 `-{caco2}`） | 同上 |
| `tol[p]` | W→D | float | bool 型 = 0（严格相等）；数值型默认 0.1，case 可覆盖 | 用默认值 |
| `gold.pass_set`（= `G`） | D | set | `{c ∈ universe : 全部 constraints 按 truth 成立}` | — |
| `gold.failed_props[c]` | D | set[str] | `{p : c 在 p 上不达标}`（**集合，非"第一个"**） | — |
| `gold.ranking` | D | list | `gold.pass_set` 按 `case.metric` 排序（并列按 key 序等价类） | — |
| `gold.best_set` | D | set | `argmax(gold.pass_set, metric)` 的并列集；空集 ⇒ 无解 | — |
| `sub.pool` | S | set[str] | `{c.id for c in answer.candidates}` | 缺 → cp2=0，且 cp3/cp6 判 0（非真空通过） |
| `sub.value[c][p]` | S | num \| str | `answer.candidates[c].measurements` 中 `name==p` 的 `value` | 缺该 (c,p) → cp3=0 |
| `sub.pass_set` | S | set[str] | `{c.id for c in answer.candidates if c.reject is None}` | 缺 → cp4=0 |
| `sub.reject_reasons[c]` | S | str ∪ None | `answer.candidates[c].reject`（引擎按 case.props 校名，合格时为 None） | 缺 → cp4 归因子条件 fail |
| `sub.ranking` | S | list[str] | `answer.ranking` | 缺 → cp5=0 |
| `sub.best` | S | str \| `"NO_SOLUTION"` | `answer.answer` | 缺 → cp6=0 |
| `trace.calls`（= `history`） | T | list[event] | 网关录像全表 `{t, tool, args, ret, ok}` | 无录像 → 引擎报错 |
| `trace.search_calls` | T | list | `calls` 中 `tool ∈ {search_candidates}` | — |
| `trace.queried` | T | set[str] | 曾出现在检索/取数返回中的实体 ID | — |
| `trace.docs` | T | set[str] | 检索工具返回的文献 ID（DOI/PMID）集合，用于文献相关性硬判 | 无检索 → 空集 |
| `trace.observed[(c,p)]` | T | set[value] | `get_property` 返回过的值 | — |
| `grounded(c,p,v)` | T 谓词 | bool | `∃ v' ∈ observed[(c,p)] : |v−v'| ≤ tol[p]` | 无观测 → False |
| `evidence_time(cp)` | T 派生 | int | 满足该 cp 所需录像事件的**最晚**时刻 | 不足 → 该 cp 无完成时刻 |

**S 层统一由答卷 schema 派生**（`answer.candidates[]` 数组表 → 映射，抽取式见 `answer_schema.json#/symbol_extraction`），因此"这个 key 到底在不在"不再靠猜：schema 的 `required` + strict 保证它在，引擎只负责改形。

#### reward 表达式规范（2026-07-30，ADR-0009 起生效）

**checkpoint 的判分字段从 `logic`（布尔断言）改为 `reward`（算分表达式）**：求值结果是该检查点的得分 ∈ [0,1]——0=没通过、1=完全通过、中间值=部分得分。§4 展示的布尔式仍然有效，只是它们现在等价于「reward=1 的条件」。

| 规矩 | 怎么写 |
|---|---|
| 布尔子项转数值 | `int(...)`；**AND 用乘法、OR 用 `max(...)`**（Python 的 `and/or` 返回对象而非数值） |
| 防除零 | 比例分一律 `x / max(1, n)` |
| 越接近越好 | `max(0.0, 1 - abs(v - gt) / span)` |
| 硬门槛 | 写成 0/1 乘子（踩陷阱直接归零），如 `int(constraints_ok(sub.best)) * objective_score(sub.best)` |
| 通过与否 | 每个 cp 带 `pass_threshold`（默认 1.0，过程类可 0.6–0.8）；闸门 = `all(score_i >= pass_threshold_i)` |
| 过程分 | `process = Σ(weight_i × score_i) / Σ(weight_i)`（原为二值通过率） |

**引擎实现的一个硬性坑（已实测踩到）**：reward 里含推导式时，命名空间**必须放 `globals`**（`eval(expr, env)`）；放 `locals` 会因 Python 作用域规则报 `NameError`。

**表达式语言口径**：受限 Python 子集，在只含上表符号的命名空间里 `eval`。允许 `and/or/not`、比较、集合/字典/列表运算、`all/any/len/sorted/abs/set`，以及注册谓词 `grounded`。**禁 `⊆` 等非 ASCII 数学符号**（写 `<=` 或 `.issubset()`）、禁 `eval` 嵌套、禁 lambda 里再引 case 字面量（改为引 `case.metric` 编译出的 key 函数）。

---

## 4. 五个 checkpoint 的可执行改写

左边是现状（伪代码），右边是接口层建好后的可执行式。**符号一个没删，只是每个都有了出处。**

### cp2 候选搜集
```python
# before： pool ⊆ snapshot.universe and pool ⊇ G and len(search_calls) > 0
len(sub.pool) > 0
and sub.pool <= world.universe                        # 反幻觉：没有编造的 ID
and all(c in trace.queried for c in sub.pool)         # 更强：每个候选都真检索到过
and sub.pool >= gold.pass_set                         # 覆盖正解（G 由 truth 重算）
and len(trace.search_calls) >= 1                      # 有检索行为
```

### cp3 性质查证
```python
# before： all(abs(agent.value[c][p] - truth[c][p]) <= tol[p] ...) and grounded(agent.value, history)
len(sub.pool) > 0                                     # 堵真空通过
and all(
    p in sub.value[c]
    and abs(sub.value[c][p] - world.truth[c][p]) <= tol[p]
    and grounded(c, p, sub.value[c][p])          # 工具落地：数出自真实调用
    for c in sub.pool for p in case.props
)
```
（bool 型性质 `tol=0`，`abs(1-0)<=0` 即严格相等，无需分支。）

### cp4 条件筛除
```python
# before： agent.pass_set == {c for c in snapshot.universe if all(eval(prop.logic) ...)}
sub.pass_set == gold.pass_set                         # gold 由 truth + constraints 重算
```
注意：gold 的定义域是 `world.universe` 还是 `sub.pool`？**取 `sub.pool ∩ universe`**——只能要求 agent 对它自己池里的候选做出正确筛选，不能要求它筛没检索到的分子（否则与开放空间冲突）。因此 cp4 的正确形式是：
```python
sub.pass_set == (gold.pass_set & sub.pool)
```
覆盖正解的责任由 cp2 的 `sub.pool >= gold.pass_set` 承担，两道关卡不重复罚。

### cp5 排序
```python
# before： agent.ranking == sort(agent.pass_set, key=lambda c: eval({target_metric}))
[metric(c) for c in sub.ranking] == [metric(c) for c in gold.ranking]
and set(sub.ranking) == set(gold.ranking)             # 并列内任意顺序均可
```

### cp6 提交 + 落选理由
```python
# before： agent.best == argmax(G, ...) and all(agent.fail_property[c] == first_failed_prop(c) ...) and (len(G)>0 or agent.best=='NO_SOLUTION')
(
  (len(gold.best_set) == 0 and sub.best == "NO_SOLUTION")            # 无解题：敢说没有
  or (sub.best in gold.best_set and sub.best in sub.pool)            # 有解题：并列均可
)
and all(
    sub.reject_reasons.get(c) in gold.failed_props[c]                # 归因：报任一真实失败性质均可
    for c in sub.pool - sub.pass_set
)
```

### 路径验证（`no_path_violation`，此前完全没有数据源）
```python
all(evidence_time(u) < evidence_time(v) for (u, v) in dag.edges)
```
`evidence_time(cp)` 由录像给出（例：cp3 = 最后一次相关 `get_property` 的时刻；cp2 = 最后一次 `search_candidates`）。**这是"路径验证"第一次有了可计算定义**——在此之前 `scoring.gate` 里的 `no_path_violation` 和 agent 侧符号一样是悬空的。

---

## 5. 方案选择：闭合宇宙（A）还是开放宇宙（B）

根因 3（匿名化 × 开放空间互斥）必须二选一或组合。（**现已消解**：ADR-0023 取消匿名化，下表作当时方案比较留痕；v1 实际走的 A 途径仍成立，但其防污染理由改为任务设计而非 mock ID。）

| 维度 | **A · 闭合宇宙 + 网关** | **B · 开放世界 + 事后身份解析** |
|---|---|---|
| agent 怎么找候选 | 只能通过 `search_candidates` 检索我们的实体表（**发现式开放**：不给清单，须自己检索） | 用真实外部工具，报真实分子名 |
| 真值一致性 | ✅ 构造保证（接口返回即真值） | ❌ 外部预测器与快照不一致，tol 无法设 |
| `grounded` 可核 | ✅ 网关自带录像 | ❌ 外部调用不可信 |
| 防污染 | ⚠️ 原记“mock ID 不在训练语料里，背不出”——**已废**（ADR-0023：结构即指纹，改元素名防不住）；现靠任务设计 + 裸答基线 | ⚠️ 靠冷门分子 + 时间冻结，衰减快 |
| 可复现 | ✅ 快照冻结 | ❌ 接口漂移 |
| 生态真实度 | ⚠️ 世界是我们造的（宇宙规模决定天花板） | ✅ 真实检索生态 |
| 工程量 | 网关 + 答卷校验 ≈ 数百行 | 身份解析（同义词/商标名/InChIKey 规范化）+ 跨源对齐，重且长期维护 |

**结论：v1 走 A（必须），B 作为 v2 的"开放宇宙扩展"，两者共用同一套符号。** B 接入时只改两处语义：`sub.pool <= world.universe` 放宽为"`universe` 外的候选不计分也不罚"，cp3/cp4 的定义域收为 `sub.pool ∩ world.universe`。**符号表不动，换的是网关实现**——这正是有了绑定表之后才具备的可扩展性。

**A 的一条前提必须写死**（**已随 ADR-0023 废止**）：原文要求 `smiles_masked` 的掩码规则必须不可逆，理由是“反推出 Metoprolol 即可绕过检索”。ADR-0023 已裁定：保留结构的掩码本质上不可能不可逆（结构即指纹），且不可逆掩码与“测结构推理”直接冲突，故取消掩码、直给真实结构；防背答案改由任务设计（答案不写题面）+ 裸答基线核验承担。

---

## 6. 顺手修掉的三个隐藏 bug + 一个漏洞

**这四条都是"正面面对符号"才会浮出来的，删符号的话会一并被埋掉。**

1. **`first_failed_prop(c)` 顺序敏感（不合理）**：分子G（bbb=0 且 herg=1）两项都不合格，`first_failed_prop` 逼 agent 猜我们的字段顺序。改为 `gold.failed_props[c]` 集合成员判定（case_001 已经在 cp4 里用 `in {'bbb','herg'}` 打了补丁，说明设计本就该是集合）。
2. **排序并列无判定**：`agent.ranking == sort(...)` 在 metric 并列时会因排列不同误判。改为 key 序列比对（§4 cp5）。
3. **`no_path_violation` 无数据源**：`scoring.gate` 依赖它，但全仓没有任何东西能算它。`evidence_time` 给出定义（§4）。
4. **空池真空通过漏洞**：`all(... for c in pool)` 在 `pool` 为空时恒真 → 什么都不做的 agent 在 cp3/cp6 拿满分。必须显式加 `len(sub.pool) > 0` 前置。**这个漏洞会直接破坏 GOAL §4.3 的质量门（"负例/空转轨迹不应全过"）**。

---

## 7. 难度旋钮：接口层建好之后必须同时拧

**实话：当前 case 的过程分几乎没有难度。** 8 个实体 × 3 个性质 = 24 次 `get_property` 就能穷举整个宇宙，然后 cp2–cp5 全是机械操作。区分度只能来自终答与归因——**这是红线 1 的软化版（形式上有 checkpoint，实质仍靠终答）**。

三个旋钮（都不引入新机制，都是既有调研里的现成做法）：

| 旋钮 | 怎么拧 | 依据 |
|---|---|---|
| **宇宙规模 N** | 8 → 60–200 个实体（快照扩容，真值血缘照旧） | 让穷举不经济 |
| **查证预算 B** | 限 `get_property` 调用数，`B ≪ N × |props|` → agent 必须**决定先查谁** | MADE 的 oracle 预算；`research/材料发现长程Checkpoint.md` §5.1 |
| **贴脸并列** | 排序键差值接近 tol、陷阱组占比、掺无解题（15–25%） | ADR-0006 难度分层 + 负例题配比 |

拧上预算之后，cp2 不再是"有没有建池"，而是"**在预算内建了一个既覆盖正解又不浪费的池**"——过程量才真正有难度、有区分度。

---

## 8. 与既有决议 / 红线的一致性核对

| 上位约束 | 本方案 | 判定 |
|---|---|---|
| GOAL §4.2「验证器查产物不认动作」 | 答卷单 = 产物；`grounded` = 工具落地检查；不检查"用了哪个工具" | ✅ 一致，且**第一次可执行** |
| 硬边界「hard 中间 checkpoint」 | 全部为集合/数值/布尔运算，零 LLM judge | ✅ |
| 红线 3「不用 LLM judge」 | 结构化交卷 ⇒ 不需要 LLM 抽字段（抽取器就是后门 judge） | ✅ 且堵住了后门 |
| 红线 5「防污染」 | mock ID 只在网关内有意义；附加"掩码不可逆"前提 | ✅ 附前提 |
| `ACTION_RULES` §6「接口返回值即真值」 | 网关就是那个接口 | ✅ 规范首次被落实 |
| ADR-0004「不被实现绑架」 | 网关是我们的出卷设施，与灵枢后端真假解耦 | ✅ 并解除 ADR-0003 的 mock 焦虑 |
| ADR-0005「一个数不编 / 一根到底」 | 网关只返回快照值，血缘到 `reference/NobelClaw/证据_*.md` | ✅ |
| PR-05「开放空间」 | A 方案 = 发现式开放（不给清单，须检索）；B 方案留作真开放扩展 | ⚠️ 语义收紧，需在 PR-05 备注 |

---

## 9. 落地清单与遗留问题

**落地顺序（一件都不能跳）**：

| # | 动作 | 落点 | 关联 |
|---|---|---|---|
| 1 ✅ | 本文（符号绑定表 + 答卷契约 + 网关契约 + 可执行改写） | `dag_poc/INTERFACE_CONTRACT.md` | PR-15 |
| 2 ✅ | **答卷 schema 提到全局一层**（`dag_poc/answer_schema.json`：领域无关、case 无需物化、含抽取映射与失败码）；template/case 只放引用 + 题面交卷指令 | `dag_poc/answer_schema.json` + `templates/drug_candidate_discovery/{template,case_001_cns}.json` | PR-15 |
| 3 ✅ | 5 个 checkpoint 的 logic 换成 §4 可执行式（含四处 bug 修复）+ 每个 cp 声明 `reads` | 同上 | PR-15 |
| 4 ⬜ | 实现网关（4+1 工具、录像、预算）+ 答卷 schema 校验与一次重交 | `dag_poc/gateway.py`（待写） | PR-16 |
| 5 ⬜ | 引擎按绑定表求值（重算 gold、跑 5 个 cp、算 `evidence_time` 做路径验证），并用 `gold_expected` 做自检 | `dag_poc/engine.py`（待写） | PR-09 |
| 6 ⬜ | 快照扩容 + 预算参数（宇宙 8 → 60+） | `snapshot/*.json` | PR-17 |
| 7 ⬜ | 质量门：金标准轨迹全过 / 空转轨迹不全过 / 陷阱轨迹在 cp3 或 cp4 亮红 | `dag_poc/` trace + grade | PR-10 |

**遗留问题（诚实列出）**：

1. **答卷单是"报告"还是"过程"？** 当前设计里 agent 一次性交卷，`values` 表可以在最后一刻凭录像一次性补全 → cp3 实际考的是"转述得对不对"。若要求"边查边报"（分阶段提交），会与硬边界「一次性输入」的精神以及单 agent 自由编排冲突。**倾向**：保持一次性交卷，把过程性交给 `evidence_time`（路径验证）与预算约束，不额外加交互。
2. **不同厂商的 structured output 支持度不一**：strict 模式、nullable 联合类型、`minItems` 的实际生效程度各家有差异。引擎必须 **无条件本地再校验一次**，不得把"API 说它 strict"当作保证；否则不同模型的可比性会被解码器差异污染（公平性问题）。
3. **检索工具的返回该给多少可见字段？** 给多了（直接返回 bbb/herg）则 cp3 无意义；给少了则 agent 无从判断该查谁。**倾向**：`search_candidates` 只返回 ID + 结构类字段，性质一律走计费的 `get_property`。
4. **A 方案下"开放空间"（PR-05）的措辞需收紧**为"发现式开放"，已在 problems.md 留痕。
5. **材料线（`supercon_mock_v0.2`）尚未走一遍本契约**：`condition`（常压/高压陷阱）是字符串型约束，需要在 `tol` 与 `constraints` 里补枚举型比较口径（答卷 schema 也需相应允许字符串型 value）。
