# task6 · 调研结果(给项目组的汇报 = task 对外成品)

> 让你看完就知道:**现在能用什么、还差什么、我们独有什么、下一步做什么**。
>
> **职责分工**(避免重复,各有唯一来源):
> - **每个 benchmark 长什么样 / 数据格式 / 怎么改造** → 解读卡,分类总入口 [`benchmark-cards-index.md`](benchmark-cards-index.md)(⚠️ 用 Obsidian 打开)。
> - **checkpoint 怎么分档 + 怎么造 trace** → [`checkpoint_palette.md`](checkpoint_palette.md)。
> - **本文** = 串起上面两者的汇报 + 我们的差异化分析,不重复 per-benchmark 细节(那是卡的活)。

---

## 一图看清:我们离 goal 还差什么,调研填了多少

| Goal 子项 | 现状 | 调研填了多少 | 还差什么 |
|---|---|---|---|
| **30–50 题 MVP 题集** | 一道正式题都没有 | 4 个可改造基石 + 5 个题目真值数据源(均已源码级亲核,卡里有数据格式 + 改造步骤) | 选题 + 写参考解题路径 + 写 4 档详细度提示卡 |
| **测量机器** | 只跑过模拟 agent | 打分公式齐(进度率/部分得分/正负分)+ 三层评测架构齐 + 一致性校准协议齐 | 接真 LLM 跑实测 |
| **论文** | 没动笔 | 卖点更硬 + method 章每组件都有 published 工作可 cite + baseline 齐 | 真实 LLM 实验数据 |
| **差异化(占住 AI4S 过程量评测位置)** | 不是空白(χ-Bench 是"孪生")| 但**没人同时做 [AI4S 发现 + 中间 checkpoint + 分级人工干预]** | 首创面收敛到 3 点(详见 §「差异化分析」),每点更硬、有先例背书 |

**这张表是驾驶舱。** 详细差异化论证 → 见下「差异化分析」;可用资产全表 → `benchmark-cards-index.md`。

---

## 6 个关键发现(口语版)

1. **2 个可直接改造的现成 benchmark**:Stanford 的 **PhysicianBench**(医疗 EHR,670 cp)和 Deep Origin 的 **DO Challenge**(药物虚筛,1M 分子,CC-BY-4.0)。
2. **材料方向意外金矿**:**Matbench Discovery** 附带的 **MPtrj** 有 158 万个 DFT 弛豫中间帧(每帧带能量/力 N×3/应力 3×3)= **免费的材料"过程量真值",零人工标注**。
3. **论文卖点反而变强**:本该最该 cite 的先例 **TRACE**(λ_min 分级介入)**数据集与代码从未公开**(超参 §A.3 其实给齐——"超参不给"的旧判断已被一手亲核翻案)→ 我们做可运行离散版 = "**首个公开可复现的分级人工干预 benchmark**"。
4. **方法学技术栈齐了**:进度率(AgentBoard)+ 部分得分(TheAgentCompany,公式可一字 lift)+ 正负分细则(HealthBench)+ 三层评测(确定性 / AI 判官 jury / CEF 交叉验证),都有 published 工作可 cite。
5. **6 个源码级亲核全部完成**(原 P0 队列已清空):PhysicianBench / TheAgentCompany / AutoMat / BioProt / PharmaBench / χ-Bench —— 亲核还**纠正了多处社媒/二级错**(BioProt 实为 100 条非~1000、license null;AutoMat repo 空壳只能借 idea;MolBench 实为 MolClaw 4 赛道非"190 单轮 QA";ChemCrow 18 工具非 14)。
6. **发现设计"孪生" χ-Bench**:医疗运营长程 agent,把我们两件事独立做了——三步造题法 + 复合验证器(确定性合约 ∧ 多票 LLM judge)。**好处**:证明我们方法有同期先例撑腰。**代价**:这两点改标"采用已验证范式",**首创收敛到真正只有我们做的 3 件**(见下「差异化分析」§3)。同批还带来药物侧数据源 **TrialPanorama**。

---

## 能用什么(头部资产 + 去哪看全表)

**全部可用资产的分类表**(领域 / 验证什么 / 对我们 R·B·seed·baseline / 亲核 / →卡)在 [`benchmark-cards-index.md`](benchmark-cards-index.md);**每篇的数据格式 + 真实样例 + 具体改造步骤**在对应解读卡。这里只列 ⭐ MVP 直接候选:

| ⭐ MVP 候选 | 角色 | 一句话怎么用(详见卡) |
|---|---|---|
| [DO Challenge](../../papers/DO%20Challenge.md) | R 药物虚筛第一题 | 1M 分子+真值现成,插"预算×命中率"硬 cp,~3–4 天 |
| [PhysicianBench](../../papers/PhysicianBench.md) | R 医疗框架 | 670 cp=pytest 函数,`passed/total` 升主分;FHIR 工具换药物材料工具 |
| [Matbench Discovery](../../papers/Matbench%20Discovery.md) | R 材料题 | 用 MPtrj 帧的力/应力定义零标注收敛 cp |
| [AgentBoard](../../papers/AgentBoard.md) | B 进度公式 | 子目标序列 + `PR=max(1/K·Σf)` 直接套 |
| [TheAgentCompany](../../papers/TheAgentCompany.md) | B 部分得分 | `(R/T·.5)+(R//T·.5)` 一字 lift |
| [HealthBench](../../papers/HealthBench.md) | B 正负分 rubric | `{criterion, points∈[-10,+10]}` 模板 |
| [χ-Bench](../../papers/CHI-Bench.md) | 设计先例 | 复合验证器源码 Apache-2.0 可借;三 delta 全干净 |

数据源 seed:[PharmaBench](../../papers/PharmaBench.md)(ADMET)·[LLM4Mat-Bench](../../papers/LLM4Mat-Bench.md)(材料)·[TrialPanorama](../../papers/TrialPanorama.md)(临床试验)·[BioProt](../../papers/BioProt.md)(实验协议)·[MolBench](../../papers/MolBench.md)(MolClaw)。baseline(§2 对照,只 cite 不改造)见索引表。

---

## 差异化分析:谁先做过、我们独有什么

### 1. 先例分 3 档(按"验证什么过程量")

**已有 ≥9 个 benchmark 验证了某种过程量——但都不是"AI4S 长程 discovery + 离散 cp + L0–L3 分级介入"三件套同时落地。** 按距离我们目标由近到远:

| 档 | 验证什么 | 现有 benchmark | 与我们距离 |
|---|---|---|---|
| **A · 离散 checkpoint** | 通过/不通过(或复合验证器输出 binary) | PhysicianBench / AutoMat / TheAgentCompany / **χ-Bench**(设计孪生) | **最近** |
| **B · 轨迹连续量** | 单调进度 / 曲线 AUC / 时间-到-解 / 信息增益 | AgentBoard / TRACE / Syntheseus / SEISMO·PMO / BioAgent | 较近(同精神,非离散卡) |
| **C · 纯方法/框架**(无 packaged 数据) | 概念框架 / 评估范式 | Mozi(HITL)/ FALAT(失败归因)/ AppWorld / DiscoveryBench | 远(借法不借题) |

还有 **D · 只评最终答案**(MedAgentBench / AgentClinic / LAB-Bench / MolBench / BioXArena / MedHELM / HealthBench / LLM4Mat / Matbench Discovery 等)——论文 §2 对照组,不在过程量先例之列。

### 2. 三档逐 benchmark 速查表(详情指向卡)

#### A 档 · 离散 checkpoint(与我们最近)

| Benchmark | 验证什么过程量 | 与我们差什么 |
|---|---|---|
| [PhysicianBench](../../papers/PhysicianBench.md) | 670 cp = pytest 函数,3 grader(Code/Hybrid/LLM-judge);主分 Pass@1 binary | 无 L0–L3;主分 binary(`passed/total` 底层已现成,我们升主分);非 discovery 域 |
| [AutoMat](../../papers/AutoMat.md) | 85 claims 的 5 维 holistic LLM-judge rubric + κ=0.69 校准 | holistic 非离散 cp;定位是"论文复现"非 discovery;**repo 空壳代码未公开** |
| [TheAgentCompany](../../papers/TheAgentCompany.md) | cp 带分值 + 确定性 Python evaluator + Collaboration 类 + 部分得分公式 | 非 AI4S;Collaboration **无 enum**(仅论文 prose);非 discovery |
| [χ-Bench](../../papers/CHI-Bench.md) ⭐ **设计孪生** | 复合验证器 = `DeterministicPass ∧ JudgePass`;三步造 case + per-stage rubric;pass^3 | 无 L0–L3;医疗运营非 discovery;纯 binary 聚合(无加权部分得分) |

#### B 档 · 轨迹连续量

| Benchmark | 验证什么 | 与我们差什么 |
|---|---|---|
| [AgentBoard](../../papers/AgentBoard.md) | Progress Rate `PR=max(1/K·Σf)` 单调进度;ρ>0.95 vs 人类 | 数据通用 agent(GPL-2.0 限 fork);**借公式不借数据** |
| [TRACE](../../papers/TRACE.md) | Hierarchical Trajectory Utility + **λ_min**(给 oracle 前 λ 提示找最小 λ 使成功率≥θ)| 非 AI4S;**数据集/代码从未公开**(超参 §A.3 给齐)→ 我们离散版自动成"首个公开可复现" |
| [Syntheseus](../../papers/Syntheseus.md) | time-to-first-solution + 单调 diversity(packing number) | 算法级搜索性能,非 agent 工具调用/求助轨迹 |
| [SEISMO / PMO](../../papers/SEISMO-PMO.md) | AUC of optimization curve(同终点不同速度→不同分) | 优化 sample efficiency,非 agent 介入度量 |
| [BioAgent Bench](../../papers/BioAgent%20Bench.md) | Completion rate = steps_completed / steps_to_completion(LLM 动态估计) | 规模小(10 任务);纯 LLM-judge 无 κ 校准 |

**B 档共同特征**:连续/曲线指标,不是离散卡;都**没有人类介入轴**。

#### C 档 · 方法/框架(借法不借题)

| 框架 | 它定义了什么 | 与我们关系 |
|---|---|---|
| [FALAT](../../papers/FALAT.md) | Dependency-guided search 区分 error-introducing vs propagated step;184 条人工标注 | 失败归因方法可借,定位 root-cause cp |
| [DiscoveryBench](../../papers/DiscoveryBench.md) | gold_workflow 作 eval 参数;"从论文人工抽 workflow"协议 | 我们 MVP 第 4–30 题的造题协议 |
| Mozi(无代码,arXiv:2603.03655)| Skill Graph DAG + HITL at high-uncertainty boundaries | 借 HITL 概念,无实物 |

### 3. 总差距 + 论文首创面收敛到 3 点

把"已有最强者"vs"我们计划"逐维度对照:

| 维度 | 现有最强者(亲核后) | 我们计划 | 还独有? |
|---|---|---|---|
| 1. structured cp + GT + 多类 grader | PhysicianBench(670 cp=pytest;4 类 taxonomy 数据里无 enum) | 同 + **加 enum + 加权部分得分** | 🟡 cp 结构与 grader 现成,cp 分类 schema 我们造 |
| 2. cp 落到 AI4S **discovery** 长程 | AutoMat(论文复现非 discovery)/ PhysicianBench(EHR)/ DO Challenge(虚筛子方向) | 药物 + 材料 **discovery** 全谱 | 🟡 lead optimization / 材料 discovery / E2E **仍我们造** |
| 3. graded human intervention 形式化 | ~~TRACE λ_min~~ ⚠️ **数据集/代码从未公开,无法复刻**(超参齐全) | L0–L3 离散 + 介入卡(AI4S-specific)+ **可运行 reference 实现** | 🟢 **实质独有**——首个公开可复现 |
| 4. "介入守恒"(前置 prompt 详细度 = 运行时介入)统一框架 | **无** | 有(PRD §2.3,src 已实证) | 🟢 **完全独有** |
| 5. Collaboration / 主动求助作过程量 | TheAgentCompany(单类 cp,数字员工) | 同精神,用在 AI4S | 🟡 方法已有,域未做 |
| 6. 复合验证器(确定性合约 + 多票 LLM judge) | **χ-Bench**(医疗运营;两层 AND 输出 binary) | 同结构,**升级为加权部分得分** + AI4S domain + 分级介入 | 🟡 **结构已有先例,增量在三点** |

**论文首创面收敛到 3 点(其余均"采用已验证范式")**:

> **① L0–L3 分级人类介入 + 可复现 reference 实现**(TRACE 不可复刻让出);
> **② 落到药物 + 材料 discovery 长程**(χ-Bench=医疗运营、PhysicianBench=EHR、AutoMat=论文复现,均非 discovery);
> **③ 加权部分得分 + 介入守恒框架**(χ-Bench / PhysicianBench 都还是 binary)。
>
> 复合验证器结构、per-stage rubric、三步造 case 法 → 明确标注"**采用 χ-Bench(2026-05)已验证范式**"。

**为什么这个 framing 比"我们组合多家"强**:
- 「**首个公开可复现** graded intervention benchmark」是 TRACE 不可复刻让出的硬 claim,审稿无法反驳。
- 加权部分得分 + 介入守恒,**没人做过**(χ-Bench / PhysicianBench 都仍 binary)。
- AI4S **discovery** 域:medical agent 已经卷,但"药物/材料发现"仍是供给侧空白。
- 复合验证器虽不首创但**结构已被同期 published 工作验证**,可信度反升(不是凭空设想)。

---

## 我们还差什么

**🔴 P0(卡 MVP)** —— 原"亲核队列"已清空 ✅,现 P0 转向:
1. 真实 LLM 实测(src 接真 agent)
2. "过程量 vs 最终答案"相关性实验(论文核心实验)

**🟡 P1(做 MVP 时启动)**:L0–L3 提示卡内容(~400 条)· 药物/材料 discovery 长程任务的"参考解题路径"真值

**🟢 P2(nice-to-have)**:发邮件向 TRACE 通讯作者 Irwin King 索**数据集/代码**(超参已公开,缺的是 DeepResearch-Bench 数据与 oracle 轨迹)· VASPilot / AstaBench / SciAgentGym 等 Gemini 单源亲核

---

## MVP 路线图(7 步)

1. **第 1 题** = 改造 [DO Challenge](../../papers/DO%20Challenge.md) + HealthBench 正负分 + TheAgentCompany 部分得分 + AgentBoard 进度公式 + 派生 4 档 + 参考解题路径
2. **第 2 题** = 改造 [PhysicianBench](../../papers/PhysicianBench.md) 框架 + 自加 cp 分类 + 接 PharmaBench/LLM4Mat 真值 + 派生 4 档
3. **材料题** = 改造 [Matbench Discovery](../../papers/Matbench%20Discovery.md) + MPtrj —— "给未弛豫结构→DFT/ML 找基态",真值轨迹来自弛豫帧
4. **第 4–30 题** = 用 [DiscoveryBench](../../papers/DiscoveryBench.md) 协议从已发表论文人工抽解题路径 → 配真值 → 套上面模板
5. **一致性校准** = 借 AutoMat 的 κ 校准协议(stratified n=40 盲评 within-1)
6. **评测器** = 三层架构(确定性代码 + AI 判官 jury + CEF 交叉验证)
7. **论文卖点** = 首个公开可复现的 AI4S 分级人工干预 benchmark

---

## 亲核统计

- ✅ **一手亲核 ~33 篇**:task4 已读 7 + MolBench(task2)+ R/B/seed 全部源码级亲核(PhysicianBench / DO / Matbench·MPtrj / AgentBoard / TheAgentCompany / HealthBench / MedHELM / MEDIC / BioXArena / LLM4Mat / Syntheseus / SEISMO·PMO / BioAgent / FALAT / AutoMat / BioProt / PharmaBench / χ-Bench / DiscoveryBench / TRACE / TrialPanorama)+ 11 张 baseline 简卡亦经一手核
- ⚠️ **2 篇确认无可获取实物** = TRACE(无代码,理论先例)/ Mozi(无代码)
- ⚠️ **数篇 Gemini 单源待验** = AstaBench(库已有卡)/ SciAgentGym / VASPilot 等
- **P0 亲核队列已清空。** 全部分类与亲核状态以 [`benchmark-cards-index.md`](benchmark-cards-index.md) 为准。

---

## 演进备注(诚实记录)

| 版本 | 何时 | 主要变化 |
|---|---|---|
| v1.0 | 2026-06-03 | task4 结论:"AI4S/医疗几乎全是终答型,我们独有 [结构化 cp + L0–L3 + AI4S]" |
| v1.1 | 2026-06-09 | 亲核 7/11:PhysicianBench 4 类 taxonomy 无 enum + TRACE 无公开物 → 维度 3 由 🟡 升 🟢 |
| v1.2 | 2026-06-09 | cubox 发现 χ-Bench(设计孪生)→ 维度 6 让出,**首创收敛到 3 点** |
| v1.3 | 2026-06-10 | 6 篇源码级 audit 全部完成 + 22 张深卡灌满 + 11 张简卡纠错 → 全表证据 🟡 升 🟢,首创 3 点结论不变且更硬 |
