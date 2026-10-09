# AAMAS 2023–2026 论文调性与 Dynamic Tool Routing 选题调研

> 日期：2026-09-20  
> 目的：为 AAMAS 2027 主投路线确定问题定义、方法调性、benchmark 与 baseline。  
> 范围：重点阅读 2023–2026 AAMAS Main Track / Full Research Paper 中与 LEARN、bandit、online learning、RL、agent evaluation、GAAI 最相关的论文；不是全会论文计量统计。  
> 状态：研究调研文档，不是 ADR。

## 1. 结论

AAMAS 并不要求论文必须包含多个智能体。AAMAS 2026 的 LEARN area 官方明确覆盖 single-agent and multi-agent learning，topics 包括 reinforcement learning、reasoning and learning under uncertainty、learning agent capabilities、few-shot learning；GAAI 则覆盖 LLM/agentic systems、agentic workflows、verification/evaluation 等。

对本项目，最自然的主投 area 是 **LEARN**，GAAI 作为问题背景与应用语境。当前新问题的技术核心应收敛为：

> 在持续变化的工具生态中，Agent 如何用有限、昂贵的反馈，快速认识一个新加入工具在不同 context 下的能力，并在 exploration / exploitation 之间取得平衡。

AAMAS 2026 已接收 **Truthful Reverse Auctions for Adaptive Selection via Contextual Multi-Armed Bandits**，直接研究 query-dependent LLM provider selection。因此“用 contextual bandit 做 provider/tool routing”本身已经不够新。我们的增量必须放在：

**dynamic tool pool + new-arm cold start + limited active probing + online adaptation**。

## 2. 官方 scope 与投稿约束

AAMAS 2027 Main Track：
- Abstract：2026-10-01
- Paper：2026-10-08
- Rebuttal：2026-11-20 ~ 11-24
- Notification：2026-12-21
- Main paper：8 pages，参考文献另计

相关 area：
- LEARN — Learning and Adaptation
- GAAI — Generative and Agentic AI
- SOPS — Search, Optimization, Planning, and Scheduling
- IA — Innovative Applications

本项目建议：
1. **Primary: LEARN**
2. Secondary framing: GAAI
3. 不需要硬造 MAS 或多 Agent 协作。

官方来源：
- https://warwick.ac.uk/fac/sci/dcs/aamas2027/
- https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/instructions/
- https://cyprusconferences.org/aamas2026/call-for-papers-main-track/

## 3. 代表论文与调性

### 3.1 AAMAS 2024 — Learning to Schedule Online Tasks with Bandit Feedback

论文先钉死三个困难：
- reward unknown；
- cost unknown；
- task arrival distribution unknown。

方法：double-optimistic learning + Robbins–Monro；理论上给出 sublinear regret O(T^{3/4})。

实验：
- synthetic；
- real-world ML task scheduling。

Baseline：
- Thompson Sampling；
- UCB；
- ω-UCB；
- Oracle。

典型结构：

**technical bottleneck → formal model → targeted algorithm → regret analysis → synthetic + real application → canonical baselines**

官方 PDF：
https://www.ifaamas.org/Proceedings/aamas2024/pdfs/p1975.pdf

### 3.2 AAMAS 2025 — Truthful Mechanisms for Linear Bandit Games with Private Contexts

问题不是再发明 contextual bandit，而是增加一个现实困难：context 是 private/strategic，agent 可能 misreport。

方法：
- 分析 UCB/TS 在该设定下不能保证 truthfulness；
- LP-based truthful mechanism；
- O(log T) regret。

启示：AAMAS 接受“成熟范式 + 一个新的、现实且可形式化的困难”，前提是方法、理论、实验都围绕这一困难展开。

来源：
https://arxiv.org/abs/2501.03865

### 3.3 AAMAS 2025 — Human-Aligned Skill Discovery

问题：传统 unsupervised skill discovery 追求 diversity，但可能发现不安全或没用的技能。

方法：把 alignment 作为 skill discovery 的共同目标。

实验：
- 2D navigation；
- SafetyGymnasium；
- DIAYN / LSD / SMERL 等 baseline；
- diversity–alignment trade-off。

启示：AAMAS 不要求必须有庞大系统。问题清楚、方法直接、机制解释完整即可。

来源：
https://arxiv.org/abs/2501.17431

### 3.4 AAMAS 2026 — Truthful Reverse Auctions for Adaptive Selection via Contextual MAB

这是与我们最接近的一篇。

问题：
- context = query；
- arm = LLM provider；
- reward/utility = quality 与 cost；
- 在线学习 query-dependent provider quality；
- 同时处理 strategic cost / truthful reverse auction。

对我们的直接约束：
- “同功能 provider + contextual bandit”已有近邻；
- 不能把“用 bandit 选工具”当 novelty；
- 必须突出 **new tool arrival / zero-history arm / limited probing / fast adaptation**。

来源：
https://arxiv.org/abs/2602.14476
Proceedings：
https://www.ifaamas.org/Proceedings/aamas2026/forms/contents.htm

### 3.5 2026 其他相关方向

AAMAS 2026 proceedings 还包括：
- A Novel Framework for Uncertainty-Driven Adaptive Exploration
- ReAcTree: Hierarchical LLM Agent Trees with Control Flow for Long-Horizon Task Planning
- Diverse Mini-Batch Selection in RL for Efficient Chemical Exploration
- SAT: Sequential Agent Tuning for Coordinator-Free Plug-and-Play Multi-LLM Training

说明 AAMAS 已明确吸收：
- LLM agent；
- agentic workflow；
- uncertainty/exploration；
- RL in scientific application；
- plug-and-play/adaptation。

所以“不含多智能体”不是 scope 风险；真正风险是技术问题是否只是工程路由器。

## 4. AAMAS 偏好的 technical difficulty

近年相关 paper 的共同模式：
1. 找一个已有方法在现实条件下会失败的**具体原因**；
2. 把它变成 formal assumption / variable；
3. 推导它改变了什么；
4. 针对性设计方法；
5. 用 canonical baseline 与 controlled experiment 证明。

常见 difficulty：
- reward/cost unknown；
- private/strategic context；
- unknown arrival distribution；
- exploration timing；
- safety；
- non-stationarity；
- expensive evaluation。

对我们：

> “工具越来越多”只是 motivation；  
> **new-arm cold start under costly sparse feedback** 才是 technical difficulty。

## 5. 行文风格

### Abstract
setting → precise difficulty → method → guarantee/result → empirical result。

### Introduction
general setting → bottleneck → why existing methods fail → formal problem → contributions。

### Method
必须清楚定义：
- context；
- arms；
- reward/cost；
- feedback；
- objective/regret；
- assumptions。

如果核心是 bandit/online learning，理论是明显加分项：
- regret；
- convergence；
- sample complexity；
至少争取一种。

### Experiments
常见：
- controlled synthetic；
- 少量标准 benchmark；
- 1 个 real application；
- canonical baseline；
- oracle/privileged upper bound；
- ablation。

不以“跑很多 benchmark”本身作为主要贡献。

## 6. 建议论文问题

不建议以以下为主：
- Scientific Tool Reliability Benchmark
- Multi-tool Conflict Diagnosis
- LLM Agent Tool Selection
- Contextual Bandit for Tool Routing

建议：

**Dynamic Contextual Tool Routing under New-Tool Cold Start**

或：

**Learning to Use Newly Arriving Tools in Evolving Agent Tool Ecosystems**

一句话：

> Given a dynamically evolving pool of functionally equivalent tools, how can an autonomous agent learn where a newly arriving tool works using only a few costly interactions?

## 7. 数学问题

对 capability f：

A_f^(t) = {T_1, ..., T_Kt}

context：

x_t ~ X

工具能力：

mu_i(x) = E[r | x,T_i]

带成本：

u_i(x) = mu_i(x) - lambda c_i

选择：

i_t in A_f^(t)

仅观察被选择 arm 的反馈。

目标：

Regret(T) = sum_t [u_i*(x_t) - u_i_t(x_t)]

动态事件：

T_new arrives at t0,
D_new(t0) = empty

给有限 probing budget B，目标是以尽量小 probing cost 恢复低 regret。

## 8. 方法路线建议

第一版优先：

**Hierarchical Contextual Bandit + Active Warmup**

共享 context：

z = phi(x)

同一 capability 下：

w_i ~ N(m_f, Sigma_f)

二值 reward：

P(r=1 | x,T_i) = sigmoid(z^T w_i)

新工具：

w_new ~ N(m_f, Sigma_f)

有限 warmup budget B 下主动选 probe：

x* = argmax_x I(w_new ; r | x,D_new)

然后进入 contextual TS/UCB exploitation。

要证明：
1. shared prior 降低 cold-start regret；
2. active warmup 比 random warmup 更 sample-efficient；
3. online update 比 static best-tool 更适合 dynamic pool。

## 9. Benchmark 建议

正文主实验三层：

### A. Synthetic Dynamic Tool Environment
控制：
K、context dimension、arm similarity、prior mismatch、noise、arrival time、B、cost。

### B. LLMRouterBench
generic same-function provider routing；模拟 new-model arrival。

### C. Scientific Tool Routing
现有 BBB：
- usable 722；
- 7 tool families；
- experimental truth；
- best fixed tool ≈ 91.55%；
- per-instance oracle ≈ 98.06%；
- best fixed 错但其他工具至少一个对：47/722。

必须补：
- scaffold/OOD learnability；
- hERG/AMES truth；
- cold-start replay。

补充：
- ASlib：classic algorithm selection robustness；
- ToolRet：motivation / front-end retrieval，不作为 Bandit 主 benchmark。

## 10. Baseline 建议

Trivial/static：
- Random
- Best Fixed Tool
- Majority Vote（science）
- Per-instance Oracle

Non-contextual online：
- epsilon-Greedy
- UCB1
- Thompson Sampling

Contextual：
- LinUCB
- LinTS / Contextual TS

Cold-start：
- random warmup + shared prior
- no shared prior
- no warmup/uniform prior
- privileged full-history oracle initialization

Nearby：
- AAMAS 2026 SupLinUCB-style provider routing
- ToolExpNet（若可复现）
- LLMRouterBench 代表 router 1–2 个

Ablation：
- no shared prior
- no active warmup
- no online update
- no cost
- random vs uncertainty vs information-gain probing

## 11. 指标

Primary：
- cumulative regret；
- post-arrival regret spike；
- probe cost；
- pulls/time to recovery；
- quality-cost utility。

Secondary：
- final routing accuracy；
- per-tool selection frequency；
- new-tool utilization curve；
- oracle gap closed %。

核心图：t0 引入新工具后，pre-arrival → cold-start spike → recovery → stable exploitation。

## 12. LLM post-train RL 的位置

保留真实模型训练，但不作为第二个同等大的算法贡献。

主 Agent M_theta 通过 SFT + post-train RL 学：
- request capability；
- verify；
- answer；
- abstain；
- stop。

具体 provider identity 不进入 LLM action space：

M_theta -> capability f -> Bandit_f -> T_i

因此：
- Bandit = 核心算法贡献；
- LLM post-train RL = Agent integration / end-to-end validation。

## 13. Go / No-Go

Gate 1 — Routing headroom：
best fixed < oracle。

Gate 2 — Learnability：
严格 OOD/scaffold split 下训练简单模型预测 P(T_i correct | x)。
若多数 tool AUC ≈ 0.5，则复杂 contextual routing 没意义。

Gate 3 — Cold-start replay：
held-out new arm + B={0,4,8,16,32}，先看 random warmup 是否形成可测 learning curve。

只有前三条成立，再做 shared prior + active warmup。

## 14. 一句话主线

> Large tool ecosystems make relevance retrieval necessary but insufficient: among functionally equivalent tools, competence is context-dependent and newly arriving tools have no history. We study how an autonomous agent can integrate such new tools with only a few costly interactions through shared priors, active warmup, and online contextual routing.

## 15. 风险

1. 近邻太近：AAMAS 2026 已有 contextual LLM provider selection。
2. Oracle gap 不可学习。
3. BBB 7 arms 太少。
4. active warmup 不优于 random。
5. LLM RL 抢走论文中心。

当前 AAMAS fit：**强，主投 LEARN**。  
最重要的审稿风险：**是否只是已有 contextual routing 的增量变体**。  
因此后续工作必须围绕“新工具冷启动与有限 probing”展开。
