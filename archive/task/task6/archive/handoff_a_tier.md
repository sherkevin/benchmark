# Handoff · A 类(structured checkpoint)三 benchmark 深读总任务书

> **本文件是给另一个 agent 的任务书,自包含。** 你不需要打开本项目其他文件即可开工;但若需要本项目上下文,所有指针在文末附录。
>
> **委派人:** jingwu(本工作区 `/Users/jingwu/work/benchmark/`)
> **委派时机:** 2026-06-09
> **预计工作量:** 2–3 天(三论文亲读 + 三份 audit + 一份跨论文综合)
> **会话路由:** 完成后请把交付文档放到 `task6/` 目录(命名见 §5),并在本任务书 §7 追加回执即可(无需访问本会话的 session_id)

---

## 1. 一句话目标

> **打开 PhysicianBench / AutoMat / TheAgentCompany 三个 A 类 benchmark 的 GitHub 仓库 + arXiv 论文,把它们的"原始数据到底长什么样"摸清,产出三份可被外部 reader 在不打开仓库的情况下读懂的实证 audit + 一份跨论文综合判定。**

---

## 2. 为什么要做这件事(背景,~150 字)

我们项目要做的是「AI4S(药物 + 材料)长程 agent benchmark,带中间 checkpoint + L0–L3 graded human intervention」。调研(`task6/checkpoint_precedents.md`)把已有 benchmark 按"验证什么过程量"分了 3 档,其中 **A 档 = 同时做了 "structured checkpoint 节点 + GT + grader" 三件套**,与我们最近。A 档全部三个候选是:

- **PhysicianBench**(Stanford, 2026-05, arXiv:2605.02240, Apache-2.0)—— 医疗 EHR,670 checkpoint
- **AutoMat**(JHU CLSP, 2026-05, arXiv:2605.00803, license 未明)—— 材料论文复现,85 claims × 5 维 LLM-judge
- **TheAgentCompany**(CMU, 2024-12, arXiv:2412.14161)—— 数字员工,175 任务 × checkpoint + partial credit + Collaboration 类别

但我们当前的证据**全部来自二级 summary**(deep-research workflow 的输出),**没人亲自打开过任一仓库的数据文件**。在我们把它们作为 MVP 第一块 retrofit 基石之前,需要**亲核 + 跨论文综合**:每个 benchmark 实际数据形态是不是我们以为的那样?**三者综合,我们应该从谁那里借什么、最佳"组合 retrofit"策略是什么?**

---

## 3. 三论文的核查清单(按论文分小节,逐条 ✅/❌ 在 audit 文档里勾验)

> **共通规则(三论文均适用)**:
> - 每个回答必须能指到**具体文件路径 + 行号**(GitHub 文件树或 clone 后的本地路径)
> - **禁止凭印象或二级 summary 回答** —— 这次专门是要**亲核**
> - 若发现 supplement / task4 二级描述与实际数据**不符**,**显式列出 delta**(reader 最关心这种 delta)
> - 遇到不确定的标 ⚠️ 不要猜

---

### 3.A · PhysicianBench (`task6/audit_physicianbench.md`)

**仓库 / 论文 / 项目页:**
- https://github.com/HealthRex/PhysicianBench
- https://arxiv.org/abs/2605.02240
- https://healthrex.github.io/PhysicianBench/

**3.A.1 数据形态(基础事实)**
- [ ] **每个任务用什么格式存?** JSON / YAML / Python class / FHIR resource bundle?**贴一个完整任务样例**(脱敏 / 截断到 ~60 行)
- [ ] **每个 checkpoint 用什么格式存?** 是否独立文件 / 嵌在任务里 / 用 ID 引用?**贴 3 个真实 checkpoint 的原文**(覆盖 Code grader / Hybrid grader / LLM-judge grader 各 1 个)
- [ ] **670 checkpoint / 100 任务**的实际数字分布:每任务 checkpoint 数最小/中位/最大?是否均匀?
- [ ] **平均 27 个工具调用**怎么统计出来的(从 reference trace?从 task spec 估计?)?有真实 trace 文件吗?

**3.A.2 Checkpoint Taxonomy 落地**
- [ ] **4 类 taxonomy**(Data Retrieval / Clinical Reasoning / Action Execution / Documentation)在数据里如何标记?enum field / tag list / 文件夹结构 / 命名约定?
- [ ] Clinical Reasoning 下的 **4 个子类型**(计算临床评分 / 解释综合发现 / 临床判断 / 应用临床标准)是否在数据里有显式 marker,还是只在论文叙述里?

**3.A.3 Grader 实现**
- [ ] **Code grader**:贴一段真实 grader 代码(Python?),说明它接收什么、输出什么
- [ ] **Hybrid grader**(GT 确定性算 + LLM 抽 agent 输出):贴一段实例,说明"确定性 + LLM 抽取"的 stitching 方式
- [ ] **LLM-judge grader**(pass/partial/fail):rubric 文本是怎么存的?判定 prompt 是固定模板还是 per-checkpoint 定制?
- [ ] **主指标 Pass@1 (binary)** 的实际聚合公式是什么?"全部 checkpoint 通过才算"还是"通过 ≥ N%"?有没有加权部分得分版本?

**3.A.4 环境与依赖**
- [ ] **FHIR R4 / HAPI FHIR JPA Server / STARR 数据**接入方式:Docker 拉一下就能跑,还是要授权?有 demo task 能本地跑通吗?
- [ ] **patient 数据**:STARR 加扰动版是否在仓库里 / 在 Box / 需要 DUA / 需要 IRB?
- [ ] **依赖工具**:14 种 FHIR 工具(9 GET + 4 POST + 1 write_file)是 PhysicianBench 自带还是用户自己接?

**3.A.5 trace / trajectory 字段(我们最关心的)**
- [ ] **reference / oracle trajectory** 字段是否存在?若存在,长什么样(动作序列 / FHIR call log / 自然语言步骤)?
- [ ] **agent 跑过程中产生的 trace** 是否被 PhysicianBench runner 保存?保存的字段有哪些?

**3.A.6 License + 维护状况**
- [ ] Apache-2.0 是否覆盖**数据本身**(不只是代码)?有没有 NOTICE / DATA_USE 之类的附加条款?
- [ ] 最后一次 commit 时间?有没有 issue / PR 显示活跃维护?
- [ ] 有没有 fork / 学术界后续工作引用并扩展它?

**3.A.7 Retrofit 可行性判定** (基于 3.A.1–6 的实证回答,**非论文叙述**)
- 我们要在它基础上加:(a) 加权部分得分聚合(替换 Pass@1 binary)/ (b) **L0–L3 graded intervention 卡片**(它没有)/ (c) **药物 + 材料任务**(它只有医疗 EHR)
  - **加权部分得分**:在它的 checkpoint 结构上是否平滑可加?
  - **L0–L3 介入卡**:它的任务 prompt 是否预留了 prompt 详细度变化的空间?
  - **替换任务域**:它的 FHIR / EHR 环境耦合有多深?能否抽出环境 abstraction 接到药物/材料工具上?

---

### 3.B · AutoMat (`task6/audit_automat.md`)

**仓库 / 论文 / 项目页 / 数据集:**
- https://github.com/JHU-CLSP/AutoMat
- https://arxiv.org/abs/2605.00803
- https://jhu-clsp.github.io/AutoMat
- https://hf.co/datasets/jhu-clsp/AutoMat

**3.B.1 数据形态(基础事实)**
- [ ] **85 个 claim 用什么格式存?** JSON / YAML / 论文-claim mapping?**贴一个完整 claim 样例**(含 paper 引用 + claim text + 复现要求)
- [ ] **artifact / 中间结果**怎么存?HF dataset 里是只有 claim 还是连带 artifact?
- [ ] **claim 难度梯度**:3 种(from-artifact interpretation / from-artifact reproduction / from-paper reproduction)在数据里如何标记?分布是?
- [ ] **5 种验证类型**(标量匹配 / 比较排序 / 结构定性 / 范围边界 / 图形复现)如何在 claim 里标注?
- [ ] **4 个子领域**(统计/ML, DFT, MD, DDD)分布是?哪些子领域需要 HPC(supplement 说最长 72 小时)?

**3.B.2 5 维评分 rubric 落地**
- [ ] **5 维评分**(Methodological Fidelity / Execution Competence / Result Accuracy / Completeness / Scientific Rigor)在数据里如何存储?per-claim 固定还是 dynamic 生成?
- [ ] **每维 2–5 个 claim-specific sub-dimensions** 是怎么生成的(SME 撰写 / LLM 生成?)?贴一个完整 rubric 实例
- [ ] **总分 5 分制**(5=完全复现 ... 1=失败)是 holistic 一个分,还是 5 维各自 1–5 然后聚合?聚合公式是?
- [ ] **6 类失败模式**(Procedural Incompleteness ... Under-training)是评测 agent 自动检测,还是事后由 SME 标注?

**3.B.3 LLM Evaluator Agent 实现**
- [ ] **LLM evaluator agent 不是固定 prompt** —— 那它具体是什么 agent?用什么 LLM、什么工具集?能导航哪些 artifact 目录?贴 evaluator agent 的 prompt(论文 Appendix D 公开)
- [ ] evaluator agent 的输出 schema 是什么?
- [ ] evaluator agent 跑一次复现评测大概花多少时间 / token?

**3.B.4 校准协议**
- [ ] **κ=0.69(quadratic-weighted)** 是怎么算出来的?40 个 SME 是怎么招募的?他们重新评测了哪些 task 子集?
- [ ] "**within-1-point accuracy=80%**" 是 vs 哪个基线?
- [ ] SME 评测是单盲还是双盲?分层抽样还是均匀?

**3.B.5 环境与依赖**
- [ ] 复现一个 claim 需要的**计算环境**(HPC?具体 simulator,DFT 用什么 code,MD 用什么 code)是 spec 在 claim 里还是要用户自己装?
- [ ] **artifact 目录结构**长什么样?(claim → paper → artifacts? 还是按 sim type 分?)
- [ ] 跑 evaluator agent 需要的本地条件(GPU?HPC 账号?)

**3.B.6 License + 维护状况**
- [ ] **supplement 标 ⚠️ 无 LICENSE 文件** —— 实际查一下 GitHub repo root + HF dataset README,是不是真的无 license?有没有 implicit license(如 paper Appendix 提到 CC-BY)?
- [ ] HF dataset 的 license 字段是什么?
- [ ] 最后一次 commit 时间?
- [ ] 与作者(Ziyang Huang / Yi Cao / Daniel Khashabi / Benjamin Van Durme)联系的常用渠道(GitHub issue / paper 邮件)是什么(**不要联系**,只是记录路径,见 §6)

**3.B.7 Retrofit 可行性判定**
- 我们要在它基础上做:(a) 把"论文复现"转向 "**discovery 长程**"(任务定位本质不同)/ (b) 把 5 维 holistic LLM-judge 拆为 discrete checkpoint(我们要细颗粒度)/ (c) 加 L0–L3 介入卡 / (d) 把材料 + 药物作为同类项
  - **任务定位**:它的 claim 与 paper 强绑定 —— 我们要"discovery 长程",能否解耦?或者反向问:**我们是否应当借它的"复现"定位作 MVP 第一批,而不是 discovery?**(这是论文 sell point 决策,重要)
  - **5 维拆 checkpoint**:5 维的 sub-dimensions 是否天然就是 checkpoint 列表?能否直接借?
  - **License**:如果无 LICENSE,我们能用它做什么?(只引用 / 借代码 / 借数据?)

---

### 3.C · TheAgentCompany (`task6/audit_theagentcompany.md`)

**仓库 / 论文 / 项目页:**
- https://github.com/TheAgentCompany/TheAgentCompany
- https://arxiv.org/abs/2412.14161
- https://the-agent-company.com

**3.C.1 数据形态(基础事实)**
- [ ] **每个任务用什么格式存?** task config(JSON / YAML)+ 环境 setup?**贴一个完整任务样例**
- [ ] **每个 checkpoint 用什么格式存?**(state check + score + Python evaluator code?)**贴 3 个真实 checkpoint 原文**(覆盖 deterministic check / LLM-judge / Collaboration 类各一)
- [ ] **175 任务**的 checkpoint 数分布:每任务 cp 数最小/中位/最大?
- [ ] 4 域分布(SWE / HR / PM / Finance)各占多少?**Finance 域是否是 supplement 提到"David Wong RocketChat"那个 ⭐ 实例的所在?**

**3.C.2 Checkpoint 结构与评分**
- [ ] **部分得分公式 `S_partial = 0.5·(Result/Total) + 0.5·S_full`** 在代码里怎么实现?贴源码段
- [ ] **`S_full`** 是"全部 checkpoint 通过"的 binary,还是有其他定义?
- [ ] **checkpoint 加权**:每个 cp 的"分值"是怎么标的?固定 1 分 / SME 标定 / 自动估?
- [ ] **Collaboration 类别**:有显式 enum field / category tag 标 cp 类型吗?还是凭命名约定 / docstring 识别?贴最少 2 个 Collaboration cp 实例

**3.C.3 Grader 实现**
- [ ] **确定性 Python 评测器**接口契约:输入(环境状态 + agent 动作历史?)输出?调用时机(每 cp 一次 / 一次性?)
- [ ] **LLM-judge fallback**:什么情况下从确定性切到 LLM(Claude-3.5-Sonnet)?是 cp 类型决定还是 ad hoc?
- [ ] **RocketChat 消息检查**(Collaboration cp 用)如何实现?查 chat log 文件,还是连真实 RocketChat server?

**3.C.4 环境与依赖**
- [ ] **数字员工环境**:GitLab / RocketChat / OwnCloud / Plane 这些服务怎么起?是 docker-compose 一键起,还是要分别装?
- [ ] 跑一个完整任务的资源(CPU / RAM / 时间)?
- [ ] 是否支持 mock 模式(不起完整服务,只跑 cp 评测)?

**3.C.5 trace / trajectory 字段**
- [ ] **reference trace** 是否存在?(SME 在 3000 人时里标过 reference solution 吗?还是 cp 是从 outcome 反推、不绑路径?)
- [ ] **agent trace 保存**:runner 保存什么 fields?能否事后回放评测?

**3.C.6 License + 维护状况**
- [ ] 仓库 license(已知公开,确认是不是 MIT / Apache)
- [ ] **3000 人时**策展协议有公开吗?(scaffold annotator agreement / 双人复核流程文档?)
- [ ] CMU 团队是否还在维护?last commit / open issues 数量?

**3.C.7 Retrofit 可行性判定**
- 我们用它的**评分模板**而非数据(supplement 已明:非 AI4S)。要确认:
  - **部分得分公式**能否一字不改 lift 出来用?有没有依赖 TAC 特定的 cp metadata?
  - **Collaboration 类别概念**能否在我们的 cp taxonomy 里直接用?它的 cp 类型 enum 是不是天然适合 4 类(数据 / 推理 / 动作 / 协作)?
  - **确定性 + LLM-judge 双层 grader 架构**是否值得我们 1:1 复用?

---

### 3.D · 跨论文综合判定 (`task6/a_tier_synthesis.md`)

**在 3.A / 3.B / 3.C 三份 audit 完成后**,产出一份跨论文综合,**只回答下面 5 个问题**,不重复 per-paper 细节:

| Q | 综合判定 |
|---|---|
| **Q1** | 三者 checkpoint 数据 schema **谁最干净 / 最易移植到 AI4S**?(给排序 + 理由)|
| **Q2** | 三者 grader 接口 **谁最适合接药物/材料工具链**?(具体到接口签名级别)|
| **Q3** | **partial credit / aggregation 公式**应该 lift 哪一个?(TheAgentCompany 的 0.5+0.5 / PhysicianBench 的 Pass@1 / AutoMat 的 5 维聚合)给出选定 + 理由 + 是否需要本项目化的修改 |
| **Q4** | 三者**互补的能力**清单 —— 哪些是 PhysicianBench 已有的、AutoMat / TAC 重复造的?哪些只有一家有? |
| **Q5** | **最佳"组合 retrofit"策略**:给一份具体方案,如"用 PhysicianBench 的 cp 结构 + TAC 的 partial credit 公式 + AutoMat 的 κ-calibration 协议",并说明该方案下我们独有的 delta 还剩什么 |

---

## 4. 信息源汇总(必须基于一手源,**禁止**仅凭论文摘要回答)

| Paper | 主仓库 | arXiv | 其他 |
|---|---|---|---|
| PhysicianBench | github.com/HealthRex/PhysicianBench | arxiv.org/abs/2605.02240 | healthrex.github.io/PhysicianBench/ |
| AutoMat | github.com/JHU-CLSP/AutoMat | arxiv.org/abs/2605.00803 | jhu-clsp.github.io/AutoMat · hf.co/datasets/jhu-clsp/AutoMat |
| TheAgentCompany | github.com/TheAgentCompany/TheAgentCompany | arxiv.org/abs/2412.14161 | the-agent-company.com |

---

## 5. 交付物

### 5.1 主交付(4 份文档)

| # | 文件 | 结构要求 |
|---|---|---|
| 1 | `task6/audit_physicianbench.md` | 按 §3.A 的 7 子节,每子节用 ✅/❌ 标问题是否回答;每个回答附**至少 1 个文件路径 + 行号锚点**;必含 §3.A.1 的任务样例 + §3.A.3 的 grader 代码片段(至少 3 段)+ §3.A.1 的 3 个 checkpoint 原文 |
| 2 | `task6/audit_automat.md` | 按 §3.B 的 7 子节,同上规则;必含 §3.B.1 的 claim 样例 + §3.B.2 的 rubric 实例 + §3.B.3 的 evaluator agent prompt |
| 3 | `task6/audit_theagentcompany.md` | 按 §3.C 的 7 子节,同上规则;必含 §3.C.1 的 task 样例 + §3.C.1 的 3 个 cp 原文(其中至少 2 个 Collaboration 类)+ §3.C.2 的部分得分公式源码段 |
| 4 | `task6/a_tier_synthesis.md` | 按 §3.D 的 Q1–Q5 逐题回答;**只**回答这 5 个,不重复 1–3 内容;长度目标 ~80 行 |

### 5.2 各 audit 文档的统一要求
- **长度**:per-paper audit 目标 ~200–300 行;table 与 code block 为主,叙述从简
- **写作准则**:reader 不需要打开任一仓库就能读懂"它实际长什么样"
- **delta 区**:每份 audit 末尾**必须**有一节"与 supplement 二级描述的 delta",列出本次核查与 `task6/survey_landscape.md` 对应卡片(L246–284 PhysicianBench / L33–66 AutoMat / `../task4/benchmark_landscape_report.md` L64–75 TheAgentCompany)的所有不符 / 补充
- **不确定的标 ⚠️ 不要猜**

### 5.3 副交付:回执
在**本任务书 §7** 追加一行:`完成于 YYYY-MM-DD,产出 [4 个文件名]`,**不要**编辑本任务书的其他内容

---

## 6. 边界与约束(避免做过头)

- ❌ **不要**给三个仓库提 PR / open issue / 联系作者 —— 这次只看,不互动
- ❌ **不要**评价它们的科学价值 / 方法论好坏 —— 只描述数据形态
- ❌ **不要**展开到其他 benchmark(TRACE / AgentBoard / Mozi 等)—— A 类外的另 handoff,本任务书不管
- ❌ **不要**把三个仓库 clone 到 `/Users/jingwu/work/benchmark/` 里 —— clone 到你的工作目录 / `/tmp/`,在 audit 文档里**引用 GitHub URL + 行号**即可
- ❌ **不要**改本项目的其他文件(`goal.md` / `PROJECT_RESULT.md` / `task1..5/*` / `task6/handoff_a_tier.md` 本身等)—— **只**在 §7 回执区追加
- ❌ **不要**为了赶完整 audit 而在某个子问题上猜测 —— 标 ⚠️ 比猜对更有价值
- ✅ 如果发现 supplement 某条事实**与实际数据不符**,**显式列出**(reader 最关心这种 delta)
- ✅ 如果有问题 §3.X 没列但你在 audit 过程中发现重要,在该 audit 文档末尾"额外发现"段说明
- ✅ 三论文可**并行**或**串行**核查,**但综合判定 (§3.D) 必须在三 audit 都完成后做**

---

## 7. 回执区(由执行 agent 填写)

| 状态 | 时间 | 交付 | 备注 |
|---|---|---|---|
| (未开始)| | | |
| ✅ 完成 (PhysicianBench §3.A) | 2026-06-09 | task6/audit_physicianbench.md | 5 条核心断言 3✅ / 1✅+caveat / 1❌(checkpoint 4 类 taxonomy 数据里无显式 marker) / 1⚠️(数据 DUA-gated,非 Apache);Retrofit 可行性=有条件 |
| ✅ 完成 (TheAgentCompany §3.C) | 2026-06-09 | task6/audit_theagentcompany.md | 5 条核心断言 3✅(175 任务 / 多 cp / partial-credit 公式源码 bit-identical) / 1❌(`Collaboration` 类别在代码里 NO enum,只是论文 §4 prose;David Wong cp 存在但是 cp#1 1pt 而非 supplement 说的"最后 1 cp 3pt") / 1✅(3000 人时双人复核,但协议文档不在 repo);MIT;最后 commit 2025-11-17 活跃。Retrofit:partial-credit 公式可一字不改 lift,Collaboration 概念只能借不能取 enum |
| ✅ 完成 (AutoMat §3.B) | 2026-06-09 | task6/audit_automat.md | 5 条核心断言全部 ✅ (85 claims/4 子域/3 难度/5 维 rubric/6 失败模式/κ=0.69/within-1=0.80/LLM evaluator 可导航 artifacts) — 但 supplement L33-66 有 1 处事实错(把 `n=40 sampled runs` 误读为"40 个 SME")+ 1 处口径偏差(overall 是 holistic 分,不是 5 维算术聚合);GitHub repo 是空壳(除 README 外无代码),HF dataset gated 且本机网络无法亲见;Retrofit 可行性=有条件(只能借 idea,不能借代码/数据,prompt 逐字复用需作者许可);License=确认无 LICENSE,GitHub API 返回 `license: None` |

---

## 附录 · 本项目上下文(可选阅读,只在你需要更多 context 时打开)

- 项目级 GOAL:`/Users/jingwu/work/benchmark/goal.md` v1.0(三层目标 + 调研先行 DRBEκP 工作流)
- task6 上下文:`/Users/jingwu/work/benchmark/task6/{goal.md, survey_landscape.md, checkpoint_precedents.md}`
- 三论文在我们二级 summary 中的描述:
  - PhysicianBench:`task6/survey_landscape.md` L246–284
  - AutoMat:`task6/survey_landscape.md` L33–66
  - TheAgentCompany:`task4/benchmark_landscape_report.md` L64–75
- 我们对它们的 Retrofit 定位 + A 类划分依据:`task6/checkpoint_precedents.md` §2 + §6
