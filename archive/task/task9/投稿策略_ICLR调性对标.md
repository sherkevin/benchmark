# 投稿策略与 ICLR 调性对标

> **一句话结论**：**主投 ICLR 2027**（摘要 2026-09-18 / 正文 2026-09-25 AOE，官方 CFP 已列 "datasets and benchmarks" 为正式 subject area）。owner 设想的"双任务 benchmark + agentic RL"**完全合 ICLR 调性**——ICLR 2026 已收 44 篇 agent benchmark、29 篇 long-horizon、RLVMR/AgentGym-RL 就是"过程奖励 + 长程 RL"的现成范式。**唯一真实风险不是调性，而是"两个任务像两篇论文硬拼"**，解法是先立一个统一形式化（VIS-DAG）再落两个实例。
>
> **备用方案成立**：只出 benchmark 也能投 ICLR（2026 年 293 篇标题含 bench 的论文里绝大多数是纯 benchmark/资源型），但必须按本文 §4 的"加厚清单"补齐，否则会被 PC 今年新写进 CFP 的"我们要 slow science、不要半成品"那段话直接打中。
>
> 状态：2026-07-31 ｜ 上游：[`task/task9/task.md`](task.md) ｜ 证据：[`目标会议长程benchmark横评.md`](目标会议长程benchmark横评.md) + [`reference/`](../../reference/) 18 篇阅读报告

---

## 1. 结论先行：四会取舍

| 会议 | 判定 | 理由（一句话） |
|---|---|---|
| **ICLR 2027** | ✅ **主投** | 长程 agent benchmark 与 agentic RL 都是它当下的显学；CFP 明列 datasets & benchmarks 与 applications to physical sciences |
| **NeurIPS 2027 D&B** | ✅ **备用主渠道** | 纯 benchmark 的天然归宿（有独立 Datasets & Benchmarks track）；但 deadline 在 2027 年，太晚 |
| **WWW 2027** | 🟡 **Plan C** | "Agentic Web" 主题吃 agent 论文（ColorBench、AgentPRM 都在 WWW 2026），但我们的材料/药物题材与 web 主题连接弱 |
| **ICRA** | ❌ 不投 | 它的 long-horizon = 机器人 TAMP / 操作 / 导航；benchmark 类几乎全是感知数据集。我们没有本体、没有运动规划 |
| **AAMAS** | ❌ 不投 | 它的核心身份是**多智能体**；GOAL.md §3.1 硬边界 1 是**单 Agent**。改成多 agent 即撞红线 |

**关键时间**（来源：官方 `iclr.cc/Conferences/2027/CallForPapers`）：

| 事件 | 日期 |
|---|---|
| 摘要截止 | **2026-09-18 AOE** |
| 正文截止 | **2026-09-25 AOE** |
| 评审释放 | 2026-11-05 |
| 作者–审稿人讨论 | 2026-11-05 ~ 11-18 |
| 最终决定 | 2026-12-16 |

**距今约 7 周（摘要）/ 8 周（正文）。** 双盲；arXiv 预印允许。CFP 今年新增了关于**共同作者、互惠评审（reciprocal reviewing）、AI 使用**的政策，以及作者投稿配额（quota）——投稿前必须逐条读 author guidelines。

---

## 2. 调性对标：owner 的设想合不合 ICLR

### 2.1 逐条对标结果

| owner 的设想 | ICLR 调性判定 | 证据 |
|---|---|---|
| 造一套 long-horizon benchmark | ✅ **完全合** | ICLR 2026 标题含 bench 的 293 篇、agent+bench 44 篇、long-horizon 29 篇 |
| 任务一：传统的、带中间 checkpoint | ✅ 合，且**是明确空白** | 18 篇里没有一篇做到"中间产物级、数值真值比对、可执行"的过程判分（横评 §4 规律 3）。最接近的 ColorBench 的里程碑判的是**屏幕状态**，不是内容正确性 |
| 任务二：长上下文埋线索的发现类（大海捞针） | ✅ 合，但**赛道有强对手** | ICLR 2026 长上下文/deep research/信息搜寻相关 36+ 篇；NewtonBench（科学发现）、InfoMosaic-Bench（多源信息搜寻，含医学生物）都已在。**必须明确区分**（见 `发现类任务构造方案.md` §8 R6） |
| 再提一些 RL 方案优化整个流程 | ✅ **完全合，且是加分项** | RLVMR（过程级 verifiable reward）、AgentGym-RL（环境套件 + ScalingInter-RL）、WebRL（ICLR 2025）三篇都是同型；ICLR 2026 标题含 RL 的 414 篇 |
| 医药/材料领域 | ✅ 合 | CFP 明列 "applications to physical sciences (physics, chemistry, biology, etc.)"；ICLR 2026 科学×agent/bench 交叉 64 篇 |

**结论：调性不是问题。** 五条全部对得上，且每条都有同会同年的实例。

### 2.2 真实风险在别处（三条）

| 风险 | 表现 | 应对 |
|---|---|---|
| **R-拼盘**：两个任务 + RL 看起来像三篇论文硬拼 | 审稿人写 "the paper tries to do too many things; the connection between the two task types is unclear" | **先立统一形式化再落实例**：VIS-DAG（可验证中间状态图）—— checkpoint 型 = procedural 实例，发现型 = epistemic 实例。Abstract 第一句就是这个统一，两个任务是它的两个正交切面（详见 `发现类任务构造方案.md` §7） |
| **R-撞车**：与 NewtonBench / InfoMosaic / ColorBench 定位重叠 | "how is this different from X?" | 三句差异化必须写死：① 对 NewtonBench——它发现**方程**、≤10 轮、过程靠 LLM 事后标注；我们发现**分散记录的事实链**、上下文可达 1M、过程硬判。② 对 InfoMosaic——它有证据链但**不判过程**；我们把证据链升格为判分对象。③ 对 ColorBench——它的里程碑是**界面位置**；我们的是**科学内容** |
| **R-半成品**：CFP 今年明写"我们最想要 slow science 和最完整的工作，不确定就明年再来" | 一个只有题库、baseline 稀薄、无分析的 benchmark 会被这条政策精准命中 | 见 §4 加厚清单；**宁可砍任务数也不砍分析深度** |

---

## 3. Plan A（主推）：统一形式化 + 双任务 benchmark + checkpoint-reward RL

### 3.1 论文骨架（10 页正文）

| 章 | 内容 | 页 |
|---|---|---|
| 1 Intro | 长程 agent 评测的现状 → **全行业公认只判终态不够但都没给硬过程判分**（横评规律 3 的六条证据）→ 我们给 VIS-DAG + 两个实例 + 一个 RL 用法 | 1.5 |
| 2 VIS-DAG 形式化 | 节点=可验证中间状态、边=真实依赖、验证器、`reward∈[0,1]`、路径闸门；两个实例的定义 | 1.5 |
| 3 Benchmark 构造 | ① GT-first 反查（procedural）② 证据闭包 + haystack（epistemic）③ 五道质量闸门 ④ 防污染（匿名化 + 受控反事实） | 2 |
| 4 指标 | `H_s`（能以概率 s 无错走完多少节点）、条件通过率 `P(cp_i \| pre ✓)`、`cover`、`n_trap`、Cost；显式对齐 ICLR 2026《Illusion》与 ColorBench 的度量原则 | 1 |
| 5 实验（评测） | 前沿模型 + 开源模型 × 两类任务 × 难度分层；四档对照轨迹（金标准/次优/陷阱/空转）；pass@k 与方差；成本 | 2 |
| 6 实验（RL） | **checkpoint reward 用于 RL**：① 稠密 vs 稀疏奖励对照 ② `promise/progress` 的解析版（DAG 上直接算，无需训 PRM）③ checkpoint-curriculum（按 DAG 深度切课程，对标 ScalingInter 的轮数课程） | 1.5 |
| 7 Analysis | self-conditioning 在长程科学任务上的检验；失败归因（跳步/逆序/被 decoy 诱导/蒙对）；reward hacking 对照（我们的断言奖励 vs 学习型 PRM） | 0.5 |

### 3.2 RL 部分的最小可交付（这是 Plan A 成立的关键）

**不做"新 RL 算法"，只做"我们的 benchmark 让 RL 变可行"的验证性实验**。三个实验，任一个都够撑起 §6：

| 实验 | 设计 | 预期读数 | 工作量 |
|---|---|---|---|
| **E1 稠密 vs 稀疏** | 同一模型（Qwen-7B 级）、同一 GRPO、唯一差别是奖励用 checkpoint 稠密分还是仅终态 0/1 | 稠密显著更快收敛、更高终态成功率 | 低（一套代码两组配置） |
| **E2 解析 promise/progress** | 把 AgentPRM 的 promise/progress 换成 DAG 上可解析计算的版本作为优势整形 | 与 E1 同向，且**无需训任何奖励模型**（这是相对 AgentPRM 的卖点） | 中 |
| **E3 checkpoint-curriculum** | 按 DAG 深度分阶段放开（先只要求走到 cp2，再 cp4，再全程），对标 AgentGym-RL 的 ScalingInter | 缓解长 horizon 策略崩塌 | 中 |

**若只来得及做 E1，Plan A 依然成立**——一张"稠密 vs 稀疏"的收敛曲线就足以论证"过程奖励是我们 benchmark 的免费副产品且真的有用"。

---

## 4. Plan B（备用）：只出 benchmark 也能投 —— 但必须加厚

### 4.1 结论：能投，有大量先例

ICLR 2026 标题含 bench 的 **293 篇**中绝大多数是纯 benchmark/资源型论文，且我们落盘的这批里有 **6 篇纯 benchmark 无任何 RL**：Toolathlon、NewtonBench、AstaBench、InfoMosaic-Bench（ICLR 2026）、ScienceAgentBench、MLE-bench（ICLR 2025）。**"没有 RL"从来不是拒稿理由。**

### 4.2 加厚清单（12 项，逐项勾）

按 AstaBench 列出的"现有 benchmark 五大缺陷" + 其余论文的通行做法，整理成可勾验清单：

| # | 项 | 来源 | 我方状态 |
|---|---|---|---|
| B1 | 工具随 benchmark 出厂（统一环境，保证可控比较） | AstaBench | 🟡 取数网关已设计，需实现（T-16） |
| B2 | **报告 Cost**（每题 token / API 花费） | AstaBench、ScienceAgentBench | ⬜ |
| B3 | 防污染写成机制不是声明 | AstaBench 的语料日期截止 / 我方匿名化 | ✅ 有机制，需写成对比表一栏 |
| B4 | **充分的 baseline agent**（不只是直接调 GPT/Claude） | AstaBench（57 agents / 22 classes） | 🟡 四档轨迹已有雏形，需扩到"若干参考 agent 架构" |
| B5 | 逐 checkpoint 的**条件通过率**（分母排除未到达） | ColorBench 的 AC、《Illusion》的 step accuracy | ⬜ 设计缺口 G1 |
| B6 | **Horizon Length `H_s`** 作为一级指标 | 《Illusion》 | ⬜ |
| B7 | **pass@k 与方差**（长程任务方差极大） | MLE-bench（pass@8 ≈ 2× pass@1）、Toolathlon（pass^3） | ⬜ |
| B8 | **捷径过滤闸门**（trivial baseline 不得解掉） | InfoMosaic | ⬜ 设计缺口 G2 |
| B9 | **人类/专家基线**做反差 | GAIA（92% vs 15%） | ⬜ |
| B10 | 答案保留机制（公开 dev、隐藏 test 答案） | GAIA（166 公开 / 300 保留） | ⬜ |
| B11 | **难度分层**且分层依据可客观复核 | GAIA（步数/工具数）、MLE-bench（人力小时）、NewtonBench（双维正交） | 🟡 有难度概念，需给客观代理量 |
| B12 | 系统消融 + 采集成本经济学 | LoTa-Bench、RoboVQA | ⬜ |

**判定**：B1–B12 至少完成 9 项，Plan B 才有竞争力。**这 12 项的总工作量大于做 E1 实验。** 换言之——

> **"砍掉 RL"并不省时间，只是把时间从 RL 挪到加厚。真正省时间的降级方式是砍任务数量、砍难度档位，而不是砍分析深度。**

### 4.3 Plan B 的降级选项（按优先级从后往前砍）

| 优先级 | 可砍内容 | 代价 |
|---|---|---|
| 先砍 | 1M token 档（只做到 200k） | 低——分档报告本就允许子集 |
| 再砍 | 材料线（只保留药物线） | 中——领域覆盖从 2 降到 1，但 ScienceAgentBench 也只有 4 学科、NewtonBench 只有物理 |
| 再砍 | 发现类的题量（40 → 20，只做外部效度对照子集） | 中 |
| **不可砍** | 过程 checkpoint 判分（红线 1） | 丧失论文存在理由 |
| **不可砍** | 五道质量闸门（金标准全过 / 空转不过 / 唯一性 / 捷径 / decoy） | 判分可信度归零 |
| **不可砍** | 防污染机制（红线 5） | 会被 SWE-bench 泄漏的前例直接类比打死 |

---

## 5. 八周排期（2026-07-31 → 09-25）

| 周 | 里程碑 | 关键交付 |
|---|---|---|
| **W1**（8/1–8/7） | 形式化定稿 + 缺口补齐 | VIS-DAG 的 ADR；G1–G4 四个缺口的设计落地（条件通过率 / 捷径闸 / no_regression cp / 开放类分位标尺） |
| **W2**（8/8–8/14） | 引擎与网关可跑 | `src/benchmark/` 下取数网关 + 判分引擎跑通 procedural 全流程（T-10、T-16） |
| **W3**（8/15–8/21） | haystack 管线 | `haystack_gen.py` + `mss_solver.py` + 第一个 epistemic case 过五闸 |
| **W4**（8/22–8/28） | 题集成型 | procedural ≥60 题、epistemic ≥20 题；难度分层与客观代理量落定 |
| **W5**（8/29–9/4） | 主评测跑完 | 前沿 + 开源模型 × 两类任务 × 分层；Cost、pass@k、H_s、条件通过率全部产出 |
| **W6**（9/5–9/11） | RL E1（+ E2 若有余力） | 稠密 vs 稀疏收敛曲线 |
| **W7**（9/12–9/18） | **摘要截止（9/18）** | Abstract + Intro + 全部图表定稿；此时必须已知全部真实数值 |
| **W8**（9/19–9/25) | **正文截止（9/25）** | 五遍修改 + 附录 + 多角色并行评审（≥6 名差异化审稿人视角） |

**Go/No-Go 判定点（W5 末，9/4）**：若主评测未跑完 → 立即切 Plan B 并按 §4.3 从后往前砍；若 W6 末 E1 未出可用曲线 → 直接切 Plan B，**不带残缺的 RL 章节投稿**（半成品 RL 比没有 RL 更危险）。

---

## 6. 一页纸给 owner 的答复

**Q1：这个组合符不符合 ICLR 调性？**
符合，而且是当下最热的窗口。ICLR 2026 长程 agent 从 2024 年的 3 篇涨到 29 篇，agent benchmark 从 3 篇涨到 44 篇；"benchmark + 在其上验证 agentic RL"有 RLVMR、AgentGym-RL、WebRL 三个同型先例。**不需要为了迎合调性改题材。**

**Q2：两个任务这样分对不对？**
分法对，但**必须先给一个统一形式化**，否则会被判为拼盘。建议的统一是 VIS-DAG：checkpoint 型判"做对了吗"（procedural），发现型判"知道对了吗"（epistemic），两者共享"可验证中间状态 + 真实依赖 + 可执行验证器"的同一骨架。**这个统一本身就是论文最强的新颖性主张**——因为现有 18 篇里没有任何一篇把长程评测统一到过程状态层。

**Q3：备用方案（只做 benchmark）能投吗？**
能，ICLR 完全收纯 benchmark。但要注意两点：① 砍 RL 不省时间，要把省下的时间投进 §4.2 的 12 项加厚，否则会被今年 CFP 新增的"要 slow science 不要半成品"那段话打中；② 真正的降级顺序应该是**砍难度档位 → 砍领域 → 砍题量**，而**过程判分、五道闸门、防污染三件事一件都不能砍**。

**Q4：发现类到底怎么造？**
不要造一条 trace，要造**证据闭包**：把"为什么答案是它"拆成原子事实，算出多个最小充分集，打散埋进可控长度的合成长上下文，配四类干扰项，用"结论 + 证据覆盖 + 落地 + 抗诱导"四道门判分，其中**结论对但证据覆盖不过半直接给 0**——这是"猜对不算发现"的机器化表达。完整设计见 [`发现类任务构造方案.md`](发现类任务构造方案.md)。
