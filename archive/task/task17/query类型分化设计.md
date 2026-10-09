# Query 类型分化设计（药物/材料对称 × 按目标形态分类）

> **用途**：owner 要 template 具备多样性、与七家论文对齐。本文按**目标形态**把 query 分三类，每类对应**不同判分 DAG + 不同题面段结构 + 对标不同论文**，并在**药物线/材料线一一对称**铺开。依据 task16 的段结构实测（`A 动作 → D 交付` 两段式、约束外置、题量越大越该收敛到少数主模板）。
>
> 状态：2026-08-01 ｜ 上游：`task16/report.md`、计划 `query类型分化设计` ｜ 段标记：A=动作 / D=字段级交付 / M=评分标尺 / S=情境 / C=约束（外置到 `domain_knowledge`）

---

## 0. 一句话总览

**四类 query，各配一套判分口径与题面模板；药物线与材料线用同一套类型，实例一一对称。**

| 类型 | 一句话 | 判分口径 | 题面段结构 | 对标论文 | 本 task 落地 |
|---|---|---|---|---|---|
| **类型1 约束满足** | 硬约束下找唯一解 | 过程 DAG，无 cp5 排序 | `A + D` | ScienceAgentBench | ✅ |
| **类型2 约束最优** | 硬约束 + 合格集内目标量最优 | 过程 DAG，完整 6 cp | `A + M + D` | RE-Bench | ✅ |
| **类型3 证据溯源** | 约束埋进长文，先抽证据再解 | 过程 DAG + 前置证据 cp | `S + A + D` | InfoMosaic + Toolathlon | ⏳ 第二阶段 |
| **类型4 open 发现探索** | **B 完全不可查**，须从 A 内线索**推理出 A 中不存在的分子**，存在性接口验证 | 终态命中 `G_B` **×** grounding（`goal.md` §2b） | `A + D` | **ADR-0012** §1 第二类 | **默认进第一版**，当前 0 template |

> **类型 1–3 评过程（checkpoint DAG），类型 4 评「终态闸门 × grounding」。** 类型 4 是 owner 明确的高价值任务：**B 完全不可见**，agent 只能靠 A 内的线索性联系**推理出一个 A 中不存在的分子**，再用存在性接口验证——这才是生成式发现能力。判分不做路径拓扑验证，但 **grounding 不可豁免**（须申报 anchor/cited_values/edit_op）。

---

## 0b. 为什么这么分类（分类依据，可溯源）

**一句话：分类不是拍脑袋，而是三条既有结论的交叉——分类主轴来自项目内核与既有 ADR，题面形态来自 task16 实测，发现类形态来自 task9 方案。**

### 依据 1：主轴选“目标形态”——因为它直接决定判分 DAG 长什么样

项目内核是“评过程”（[`goal.md`](../../goal.md) §1：不只看终答、更看寻找过程对不对）。既然判的是过程，**类型就应按“过程形状不同”来分**——而过程形状由目标形态决定：

- **找唯一解**（类型1）无优劣梯度 → DAG 无排序环节；
- **约束最优**（类型2）有优劣梯度 → DAG 多一个 cp5 排序；
- **open 发现探索**（类型4）推理路径不唯一 → 不做路径拓扑验证，改用「终态命中 `G_B` × grounding」。

这直接承接 [ADR-0009](../../governance/adr/0009-reward表达式与两类模板家族.md) 已立的“**题目本来就有两类、因此是两类不同的模板**”（一类由 GT 反向构造、一类开放性目标）。本方案把 ADR-0009 的两类**细化为四类并补齐双领域对称**，主轴一致。

> 为何不按“对标论文范式”或“领域×任务矩阵”分：前者把分类绑在别人的实现上（他们换了我们就乱）；后者会造成 4 格里药物/材料只是换个名词、判分 DAG 完全一样（无真实多样性）。只有按目标形态分，不同类才真正考不同能力。

### 依据 2：每类的题面段结构来自 task16 的 223 条实测

段结构（A/D/M/S）不是我自定，是 [`task16/专家背书query原文汇编.md`](../task16/专家背书query原文汇编.md) §8–9 对 7 家、223 条真实 query 句级标注得出的：

- **`D` 交付段是唯一 100% 必需段**（ScienceAgentBench 102/102 均有且压尾）→ 所以四类题面都必须有 D 段、写出判分器要读的字段名；
- **类型2 加 `M` 评分标尺段**：对标 RE-Bench 7/7 把评分公式写进题面（因为它有优劣梯度，agent 需知道“好到什么程度”）；
- **类型3 加 `S` 情境段**：对标 InfoMosaic/Toolathlon 把约束埋进长情境；
- **约束外置到 `domain_knowledge`**：对标 ScienceAgentBench（题面不塞约束，放专家知识字段）。

### 依据 3：类型3/4 的形态来自既有发现类方案

- **类型3 证据溯源** 直接用 [`task9/发现类任务构造方案.md`](../task9/发现类任务构造方案.md) 的“证据闭包（MSS）+ haystack 埋线索 + 四道门”——那份方案已把“先抽证据再解题”的判分形式设计好了，本方案只是把它列为四类之一。
- **类型4 open 发现探索** 承 **ADR-0012 §1 第二类**（Accepted 2026-08-02）：B 不可见、推理出 A 中不存在的分子、存在性接口验证。与类型3 同属"发现能力"，但它考的是**生成**而非**检索**，故判分改为「终态闸门 × grounding」。

### 一张表：每类的分类依据回链

| 类型 | 为何独立成一类 | 依据出处 |
|---|---|---|
| 1 约束满足 | 无优劣梯度、找唯一解 → DAG 无排序 | ADR-0009 “由 GT 反向构造”一类 + goal.md 评过程 |
| 2 约束最优 | 有优劣梯度 → DAG 有排序 cp5 | ADR-0009 “开放性目标”一类 + task16 的 M 段 |
| 3 证据溯源 | 约束隐在长文、需先抽证据 | task9 发现类方案（MSS + haystack） |
| 4 open 发现探索 | gt 不在可查库内、须从线索推理生成 | **ADR-0012 §1 第二类**（Accepted）+ goal §2b 造题五步 |

---

## 1. 类型 1 · 约束满足型 constraint_satisfaction

**本质**：给定一组硬约束，在候选空间里找到唯一同时满足全部约束者（合格集 |G|=1）。这是"找那个对的"，不涉及优劣排序。

**判分 DAG**（5 个 cp，**去掉 cp5_ranking**）：

```
cp1_literature → cp2_candidate_pool → cp3_property_coverage → cp4_filter → cp6_answer
```

其中：cp4 的合格集应恰好 1 个（出题时用 `verify_snapshot_identity` 同类穷举确认 |G|=1）；cp6 命中即 1.0，无距离衰减分支（因唯一解）。

**题面段结构 `A + D`**（对标 ScienceAgentBench 的 `A→D`，D 段字段级、压尾）：

```
A: Identify a {domain} candidate that is {P1}, {P2}, and {P3}.
D: Submit two fields: screened_pool (all candidate IDs you examined) and
   ranked_candidates (the single qualifying ID as the sole element).
```

**对称实例矩阵**：

| 线 | domain | 硬约束（外置 domain_knowledge） | 唯一解 ID | 真名 |
|---|---|---|---|---|
| 药物 | 中枢神经系统 | bbb==1, herg==0, caco2>-5.0 | `aa` | Metoprolol |
| 材料 | 常压超导 | condition==ambient, structure_in_db==true, tc>某窗口下界且<上界 | 单一晶体 | （材料线） |

---

## 2. 类型 2 · 约束最优型 constrained_optimization

**本质**：硬约束筛出合格集后，在合格集内按目标量求最优（对应现 `open_discovery`）。这是"找最好的那个"。

**判分 DAG**（完整 6 cp，比类型 1 **多 cp5_ranking**）：

```
cp1_literature → cp2_candidate_pool → cp3_property_coverage
  → cp4_constraint_filter → cp5_objective_ranking → cp6_answer
```

其中：cp5 逐位比对目标量重排序；cp6 含"合格集内非最优按距离衰减"分支（因有优劣梯度）。

**题面段结构 `A + M + D`**（对标 RE-Bench 把评分标尺写进题面）：

```
A: Among {domain} candidates that satisfy {constraints}, find the one that best {objective}.
M: The candidate is scored by {target_metric} ({direction}); a stronger value ranks higher.
D: Submit two fields: screened_pool (all candidate IDs examined) and
   ranked_candidates (qualifying IDs ordered best-first; the first is your recommendation).
```

**对称实例矩阵**：

| 线 | domain | 硬约束 | 目标量 | 陷阱（区分点） |
|---|---|---|---|---|
| 药物 | 口服 CNS 择优 | bbb==1, herg==0 | caco2 最大（吸收最好） | 高 herg 的高吸收分子应被门槛排除 |
| 材料 | 常压超导 | condition==ambient, structure_in_db==true | tc 最大 | `xd`(LaH10) tc=250K 但高压，门槛归零 |

---

## 3. 类型 3 · 证据溯源型 evidence_grounded（第二阶段）

**本质**：把约束**打散埋进一篇长上下文**（合成的研究报告 / 筛选日志），agent 必须先做证据抽取 + 共指消解，才能还原出约束再解题。对应 T-33 的 haystack 埋线索。

**判分 DAG**：在类型 1 或 2 的主干**前置一组证据 checkpoint**（每条被埋的线索是否被找到并用对），需扩 `evaluate.py`，故列第二阶段。

**题面段结构 `S + A + D`**（对标 InfoMosaic 多源整合 + Toolathlon 情境铺陈）：

```
S: [一段合成研究报告/筛选日志，三条约束分散其中，同一实体用不同别名指代]
A: Based on the report above, identify the {domain} candidate meeting all stated criteria.
D: Submit screened_pool and ranked_candidates as above.
```

**对称实例**：药物＝三条性质线索分散埋进 3 段文档、同分子用不同别名；材料＝把约束埋进一份材料筛选记录。

---

## 3b. 类型 4 · open 发现探索型 discovery_haystack

> **权威定义**：[`goal.md`](../../goal.md) §1.5 类型二 + §2b（承 [ADR-0012](../../governance/adr/0012-两类benchmark设计范式与query专家背书要求.md) §1 第二类，Accepted 2026-08-02）。本节与之保持一致；如有冲突以 goal 为准。

**本质**：数据库分 **A（agent 可查）** 与 **B（agent 完全不可查）**。给一个设计目标，**agent 根据 A 中的信息推理出一个 A 中不存在的分子**——它不知道该分子是否存在，只能给出推断；评分时去 B 里查这个推断，看是否猜对。

**考的能力**：不是"会不会多调一个工具"，而是**能不能从已知线索推出未知的东西**（生成式发现）。

### 三条关键设计约束（缺一不可）

| 约束 | 含义 |
|---|---|
| **线索性联系** | 被推理出的分子须与 A 中分子有可推导的结构-性质关系；**线索必须完全可由 A 内数据归纳得出**（同系列 ≥k 条同向样本），**不得依赖匿名映射之外的真实世界化学知识**（gt 已匿名，外部先验失效） |
| **存在性查询接口** | `check_molecule_exists`：返值严格等价 `m ∈ B`，**A 内分子与库外分子一律返"不存在"**；只返**单个 bool**，不带性质/相似度/距离信息 |
| **B 不可见** | agent 推理过程中完全看不到 B 的内容，只能通过存在性接口验证 |

### 判分：终态闸门 × grounding（**不是"只评终态"**）

$$\text{open\_score} = \mathbb{1}[\text{hit}(G_B)] \times \mathbb{1}[\text{grounding\_pass}]$$

其中：
- $\text{hit}(G_B)$：提交的推断分子属于**B 侧合格集 $G_B$**（造题时穷举落盘，gt + 同等合格者），且经 B 侧性质复核满足题面目标窗口。**不是"命中 B 中任意分子"**——否则 agent 循环猜常见分子即可刷分；
- $\text{grounding\_pass}$：产物落地验证通过——答卷须申报 `anchor_molecules[]`（∈ A）、`cited_values`、`edit_op`，且每条引用值与 trace 中真实工具返回值在容差内一致、`edit_op(anchor)` 规范化后等于所提交分子；
- **不做路径拓扑验证**（推理路径不唯一，强制则误判），但 grounding 不可豁免；
- 存在性接口设 `max_attempts`（建议 ≤10），超限记 0 分。

### 造题五步与质量门

按 `goal.md` §2b 执行。其中第 ④ 步**可发现性质量门**是硬门，两条均为可执行断言：

1. **A 侧无替代解**：A 全域穷举须证**A 内不存在满足全部硬约束且目标量不劣于 gt（容差 δ）的分子**。<br>⚠️ 不能写成"仅穷举 A 拿不到 gt"——那是"gt 在 A 中不存在"的**重言式、零过滤力**；真正要卡的是"A 内已有不劣于 gt 的合格解"，那会使题目**退化为 fixed 检索**。
2. **线索可达**：case 须声明 `anchor ∈ A` + `edit_op ∈ 封闭算子集` + **跳数 ≤2** + `evidence_set`（A 内 ≥k 条同向证据），脚本校验 `edit_op(anchor)` 规范化后 == gt。

两条任一不成立则该 case **废弃**。

### 防蒙对（open 类无路径检查，必需额外一道门）

**无工具裸答基线**：不给 tools、固定 temperature，把题面问模型 **k=10 次**；**任一次命中 $G_B$ 即判污染并废弃**。

### 题面段结构 `A + D`

题面不提示"去换源"（那是错误定义的残留），而是给出设计目标并要求提交推断：

```
A: Propose a {domain} candidate that best achieves {design_objective}.
   The candidate you propose may not exist in the database you can query.
D: Submit your proposed molecule, together with anchor_molecules, cited_values
   and edit_op that justify your reasoning. Only the final proposal is scored.
```

### 落地前提（当前均未就位）

| 前提 | 现状 |
|---|---|
| B 侧库与 A/B 物理分层（含 source 多源标签） | ❌ `molecule_search[].source` 全为 `mock_db` 单一值 |
| `check_molecule_exists` 存在性接口 | ❌ 不存在（归 E3） |
| `evaluate.py` 支持 `open_score`（终态×grounding） | ❌ 当前只有 6 cp 过程分 |
| open 答卷 schema（anchor/cited_values/edit_op） | ❌ 待与 `answer_schema.json` 同批交付（归 E3） |

**状态**：goal v3.0.1 已裁定 **open 类默认进第一版**，其专用交付物为无条件前置（仅触发 T-29 Plan B 时降级）。当前 **0 template / 0 case**。

---

### 📌 历史记录：本节曾写错的定义（保留以备追溯）

2026-08-01 本节初稿把类型 4 写成**"B 源可达、agent 主动换源/调 `predict_admet` 即可查到，只评终态 gt，对标 GAIA"**。

**错在哪**：那是**换源检索**，把 ADR-0012 的**生成式发现**降级成了"会不会多调一个工具"——难度与考察点完全不同。owner 原话"agent **只能看 A**、要**推**的 gt 在 B"中，"只能看 A"即 B 不可见、"要推的"即推理生成，两处均被读漏。

**另有一处判断当时是对的**：把类型 4 与 SciCode/NewtonBench 的"数值计算/合成"捆在一起砍掉是错的——`predict_admet` 等接口虽为 mock 但返回真实值，不需要合成环境。这与"B 是否可见"是两个独立问题，该结论仍成立（见 §6）。

---

## 4. 与现有模板的关系

| 现有 | 去向 |
|---|---|
| `fixed_discovery`（全药物，6 cp） | → **类型 1**（去 cp5），并补对称材料实例 |
| `open_discovery`（全材料，6 cp） | → **类型 2**，并补对称药物实例 |
| 无 | → **类型 3**（全新，承 T-33） |

**非推倒重来**：判分内核（`evaluate.py` 的 cp reward、答卷契约 `screened_pool`/`ranked_candidates`、门控与 task_score 公式）全部不动；本次只做"类型分化 + 双领域对称 + 句式差异化 + 约束外置"。

## 5. 约束外置到 `domain_knowledge`（三类共用）

对标 ScienceAgentBench：题面（`query`）只留 A/D(/M/S) 段，**把"什么算合格"的性质阈值移入 case 的 `domain_knowledge` 字段**。该字段的最终药化/材料学术语表述待 T-49 由领域专家执笔，本 task 先落**英文骨架 + 数值占位**。

示例（类型 1 药物 case）：

```
domain_knowledge（占位，待专家核）:
  - BBB permeability: the candidate must cross the blood-brain barrier (bbb == 1).
  - hERG safety: the candidate must not block the hERG channel (herg == 0).
  - Oral absorption: Caco-2 permeability above threshold (caco2 > -5.0).
```

## 6. 类型取舍（4 类保留，1 类并入，1 类砍掉）

- **✅ 类型4 open 发现探索型**（原误砍，已恢复；后按 ADR-0012 重定义）：owner 指出这是高价值发现类任务。**恢复的理由仍成立**——`predict` 接口虽为 mock 但返回真实值、无需合成环境；但**定义已更正**为"B 不可见 + 推理生成"，判分为「终态命中 `G_B` × grounding」而非"只评终态 gt"（详见 §3b）。
- **⚠️ 数值计算/合成型**（SciCode/NewtonBench 式，砍掉）：判分是数值/符号比对或代码执行结果，考的是"算得对不对"而非"发现过程"，偏离我方"评过程"内核。**注意：这与类型 4 不同**——类型 4 的 predict 接口只是"查真实值"（一步拿到），不是"让 agent 做数值计算/写公式"。
- **➡️ 性质预测核验型**（并入 cp3，不单列）：我方性质真值来自数据库接口、agent 查得到，判分退化为"查没查"（覆盖性），**已被各类型的 cp3 覆盖**，无需单独成类。
