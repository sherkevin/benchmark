# Task 6 — AI4S benchmark 全景调研增量(补 task4 三大缺口 + 双路交叉验证)

> **本文职责**:task6 任务**章程**(由来 / 目标 / 方法 / 状态),历史档案性质。**结论与可用资产**在 [`result.md`](result.md) + [`benchmark-cards-index.md`](benchmark-cards-index.md),不在本文。

## 1. 由来

| 上游 | 状态 |
|---|---|
| `../task4/result.md` + `../task4/benchmark_landscape_report.md`(2026-06-03)| 已完成 7 张深读卡 + 留 4 个开放问题 |
| `../../goal.md` v1.0(2026-06-08)| 项目级核心轴还原为"过程量 / 中间 checkpoint",药物 + 材料作为主域;DRBEκP 工作流中的 **D 阶段**(Discover)要求"调研先行,不允许跳过 D 直接造题" |

**task4 已发现三大缺口**(且 task4 自己在 §6 也承认):

1. **材料方向调研不足**——task4 主要集中在医疗 + 通用 agent + AI4S 数据分析,材料 agent benchmark 形态如何、有没有过程量,未深入
2. **药物方向的过程量调研不够深**——MolBench 已实查(task2),但同方向其他可能带过程量的工作(逆合成 / 多步优化 / 合成可行性)未系统盘点
3. **2025–2026 新发 benchmark 未覆盖**——task4 截止 2026-06-03,之后(尤其 2026 上半年)的新工作未在视野内

**外加 task4 §7 留 4 个开放问题**:BioProt license、MedAgentBench refsol.py 链接、2025–2026 漏网医疗 benchmark、TheAgentCompany 部分得分公式的后续移植。

## 2. 目标(GOAL)

> 在 task4 基础上,做一轮**针对性深入**的 AI4S benchmark 补充调研,产出:
>
> (a) **填补三大缺口**的 ≥10 张新深读卡(材料方向 ≥3 / 药物过程量 ≥3 / 2025-2026 新发 ≥3 / 过程量方法论 ≥2)
> (b) **回答 task4 §7 四个开放问题**,每问给出结论 + 操作建议
> (c) 把"R(可 Retrofit)/ B(可借法)/ Unusable"**三分表更新**到 2026-06-08 当前(老卡 + 新卡共 ≥20 张)
> (d) 给出 **MVP 核心集构建的优先级排序**(P0–P3)和**对项目级判定的影响**(哪些 task4 结论需要收紧 / 撤回 / 加强)
>
> **本任务的真正下游用途**:作为 `../../goal.md` DRBEκP 工作流 **D 阶段的凭据** + **R(Retrofit)阶段优先级输入**——决定 MVP 核心集"先 Retrofit 哪几个现成 benchmark"。

## 3. 子问题(对照本任务结论判定是否回答)

| # | 子问题 | 状态 |
|---|---|---|
| Q1 | 材料方向有没有带过程量的 agent benchmark?有的话各自在哪个 retrofit/borrow 档位? | ✅ 见 supplement §A(找到 1 个:AutoMat ⭐⭐⭐) |
| Q2 | 药物方向除 MolBench 外,有哪些做了真正的过程量度量? | ✅ 见 supplement §B(找到 3 个:Syntheseus / SDDBench / SEISMO/PMO) |
| Q3 | 医疗方向 2025–2026 有没有新进展(尤其 EHR / 长程任务 / structured checkpoint)? | ✅ 见 supplement §C(找到 4 个,最重要 PhysicianBench ⭐⭐⭐ / BioAgent Bench)|
| Q4 | 过程量评测方法论(progress rate / trajectory utility / scaffolded capability)有哪些可借方案? | ✅ 见 supplement §D(AgentBoard / TRACE 各 ⭐⭐⭐)|
| Q5 | task4 §7 四个开放问题各自的结论? | ✅ 见 supplement 第 2 部分 |
| Q6 | 三分表(R/B/Unusable)更新后长什么样? | ✅ 见 supplement 第 3 部分(21 张卡分类)|

## 4. 方法(双路独立调研 + 交叉验证)

**第一路 · Claude deep-research workflow** → `archive/survey_landscape.md`
- 5 角度并发 WebSearch → 24 个一手源 fetch → 120 条声明 → top 25 做 3 投票对抗式核验(2/3 反对则 kill)
- 24 通过 + 1 被驳(MASTER 的具体模型/运行数细节)
- 8 个专项 deep-read agent 各跑一篇关键论文,共 ~7.5M tokens
- **形式:14 张卡片式深读卡 + 第 2 部分回答 task4 开放问题 + 第 3 部分三分表 + 第 4 部分材料方向空白成因分析 + 第 5 部分借鉴优先级**

**第二路 · Gemini DeepSearch** → `survey_gemini_raw.docx`
- 独立信源 + 独立综合
- **形式:essay/whitepaper 风格,8 章 + 引用 ~49 个一手源**
- **独有贡献(主报告未覆盖)**:DO Challenge(Deep Origin)/ Mozi 框架(Skill Graph 治理自治)/ PharmaBench(ADMET 多目标)/ VASPilot(VASP 自动化)/ ALDbench(Argonne ALD 合成)/ AstaBench / SciAgentGym / FALAT(失败归因 dependency-guided search)

**两路交叉验证**:核心结论(材料供给侧空白 / PhysicianBench 是 EHR agent 突破 / 过程量评测需要 trajectory tracing + LLM-as-judge + κ 校准)两路一致 → **可信度提升**;独有贡献合并到本任务下游消费(见 §5)。

## 5. 交付物

| 类别 | 文件 | 状态 |
|---|---|---|
| 主深读报告 | `archive/survey_landscape.md`(~720 行,14 卡 + 3-tier 表 + §7 答 + 优先级)| ✅ 已产出 |
| 副 essay 综述 | `survey_gemini_raw.docx`(交叉验证 + 6 个独有信源)| ✅ 已收 |
| 原始论文 | `../../prd/task6_papers/`(15 篇关键论文 abstract / metadata)| ✅ 已 fetch |
| 原始调研笔记 | `../../search/task6_deep_research_raw.json`(120 声明 + 25 核验)| ✅ 已存 |
| **task result** | [`task6/result.md`](result.md) —— 本任务对外成品,自包含(合并自原 inventory + checkpoint_precedents) | ✅ 已产出(2026-06-10) |

## 6. 约束与约定

- 所有"拉取的原始文件"放 `../../prd/task6_papers/`(已遵循);所有"调研结论"放 `../../search/task6_deep_research_raw.json`(已遵循)
- 结论必须基于**至少两个独立信源**(arXiv + GitHub 或 arXiv + paper website);单源声明在主报告中标⚠️
- 不评判没读过的论文(主报告附录 A 明确列了"未深入"的项)
- 本任务**只交付调研**,不动测量机器、不写真实 LLM 实验(那是 task7+ 的事)

## 7. 与项目的关系

- **上游**:`../../goal.md` v1.0 §2.2 DRBEκP 工作流的 D 阶段
- **下游**:`task6/result.md`(待写)→ MVP 核心集构建任务(R/B 决策的优先级输入)→ 论文 related work + methods 章节(借哪几篇 + 怎么定位)
- **副产出**:对 `../task4/result.md` 部分结论的**更新与收紧**(PhysicianBench + AutoMat 出现后,"AI4S 方向供给侧完全空白"需要收紧为"差异化仍成立但收窄到'+L0–L3 graded intervention'这一条轴")

## 8. 本任务执行的实际节奏(诚实记录)

| 时间 | 动作 |
|---|---|
| 2026-06-08 13:xx | 主报告 deep-research workflow 启动(背景 ~28min)|
| 2026-06-08 18:40 | 主报告(`archive/survey_landscape.md`)落库 |
| 2026-06-08 18:47 | Gemini DeepSearch 副报告(`survey_gemini_raw.docx`)落库 |
| 2026-06-08 19:xx | 本 goal.md 补写(retroactive,调研在前 goal 在后,**非项目正常流程,因为产物已是高质量,goal 用于事后锁定问题与方法**)|

> ⚠️ **诚实说明**:按项目正常流程应当先写 goal.md 再启动调研。本次因主报告由 deep-research workflow 一次性自动产出 + 副报告并行从 Gemini 取,goal 是事后补的——这种顺序未来应避免,因为它把"我们想问什么"的纪律推到了"我们已经知道了什么"之后,容易漏问题。补 goal 的意义不在于"声称按流程办了",而是**锁定子问题 / 方法 / 与下游的衔接**,以便 task6/result.md 写作时有清晰的对照清单。
