# Benchmark / Baseline 再收敛调研：我们到底测什么能力

> 日期：2026-09-20  
> 目标：把 AAMAS 2027 新主线从“动态工具路由”进一步收敛到一个可验收的能力定义，并用已发表论文的实验规模反推本项目最终 benchmark / baseline 数量。  
> 状态：研究结论；最终执行矩阵见 `docs/AAMAS2027_完整实验矩阵_DynamicToolRouting_v1.md`。

## 1. 核心能力：不是“会选工具”，而是“新工具快速适应”

本文真正要测的不是泛化的 tool-use ability，也不是 semantic tool retrieval，更不是多工具 workflow planning。

**唯一主能力定义：**

> **Rapid Context-Dependent Adaptation to Newly Arriving Tools**：  
> 当一个与已有工具功能等价、但没有历史记录的新工具进入系统时，Agent/Tool Manager 能否利用已有工具的共享经验与少量新反馈，快速判断“这个新工具在什么 context 下值得用”，并以较低的在线 regret 把它纳入长期使用。

形式化：

- capability (f) 下，时刻 (t) 的工具池为
  [
  mathcal A_f^{(t)} = \{T_1,\ldots,T_{K_t}\}.
  ]
- context 为 (x_t)。
- 工具 (i) 的未知 context-dependent utility：
  [
  mu_i(x)=\mathbb E[r\mid x,T_i].
  ]
- 新工具在 (t_0) 加入：
  [
  T_{new}\in\mathcal A_f^{(t_0)},\qquad D_{new}(t_0)=\varnothing.
  ]

目标不是学一个全局工具排名，而是最小化新工具到来后的 online regret。

## 2. 拆成四个必须测到的子能力

### C1. Contextual discrimination
同功能工具中，能否根据 (x) 判断“当前任务上谁更合适”。

必要证据：
[
\text{Oracle} > \text{Best Fixed}
]
并且一个简单 contextual router 在严格 split 下能超过 Best Fixed。

### C2. Zero-/few-history cold start
新工具历史为 0 或很少时，能否利用旧工具知识形成比独立初始化更好的初始判断。

必要证据：
- B=0/4/8/16/32 时的 post-arrival regret curve；
- shared prior 比 independent prior 更快恢复。

### C3. Online adaptation under bandit feedback
每轮只看到被调用工具的 reward，能否持续更新、逐渐从 exploration 转向 exploitation。

必要证据：
- cumulative regret；
- post-arrival regret；
- oracle gap closed 随在线交互增加。

### C4. Probe/sample efficiency
若部署方允许一个 calibration pool，新工具上线前只给有限 B 次 probing，能否挑更有信息量的 context，而不是随机试。

必要证据：
- active probe vs random probe；
- 相同 B 下更低 post-arrival regret。

**C4 是增强 setting，不是所有环境都必须成立。** 对自然到来的用户 query，主 setting 是 C1–C3 的 passive online adaptation。

## 3. 明确不测什么

以下不是本文主 claim：
- 从 4 万个工具中检索 relevant tool（ToolRet 类问题）；
- 复杂 multi-tool composition / dependency planning（ToolExpNet 的另一部分）；
- LLM 是否会正确写 API 参数；
- tool output 本身是否绝对正确；
- 多智能体协作。

这些可以作为 motivation / integration，但不能稀释核心能力。

## 4. 经典/代表论文的 benchmark 与 baseline 量级

这是代表性 audit，不是对所有论文的正式 meta-analysis。目的只是判断“一篇方法论文合理需要多少 benchmark / baseline”。

| 论文 | 类型 | Benchmark / 场景规模 | Baseline 规模 | 启示 |
|---|---|---:|---:|---|
| AAMAS 2024, Learning to Schedule Online Tasks with Bandit Feedback | AAMAS 方法+理论 | 1 synthetic family + 1 real ML scheduling application | 4：TS, UCB, ω-UCB, Oracle | AAMAS theory-heavy 方法论文可以非常克制，关键是 formulation + regret + real validation |
| ICLR 2021, Neural Thompson Sampling | contextual bandit 方法 | 6 datasets：Adult, Covertype, Magic, MNIST, Mushroom, Shuttle | 7 baselines：Linear UCB/TS, Kernel UCB/TS, BootstrapNN, ε-greedy, NeuralUCB | 6 数据集 + 7 强 baseline 已足以支撑一篇完整方法论文 |
| ACL Findings 2025, ToolExpNet | tool-learning 方法 | 2 benchmark suites / 3 scenarios：RestBench-TMDB, RestBench-Spotify, ToolBench-I3 | 3：ReAct, EasyTool, DRAFT | Agent/tool 方法不需要十几个 benchmark；场景与 failure mode 对齐更重要 |
| ICML 2019, Warm-starting Contextual Bandits | robustness / warm-start 方法 | 524 OpenML classification datasets | 约 5 个 comparator variants：Bandit-only, Majority, Sup-only, Sim-bandit, ARROW-CB reduced | 524 是研究设计决定的“大规模 robustness study”，不是普通方法论文的最低要求 |
| ACL Findings 2026, LLMRouterBench | benchmark paper | 21+ datasets, 33 models | 10 routing algorithms + Random/Best Single/Oracle | benchmark paper 才需要这种宽度；不能拿它要求普通 8 页方法论文 |
| AAMAS 2026, Truthful Reverse Auctions + contextual MAB | theory-heavy AAMAS | 主要 synthetic simulator，40 seeds | 重点是理论与 clairvoyant comparison | AAMAS 接受很窄的 empirical scope，只要新 formulation 与理论贡献够强 |

### 量级结论

对**方法论文**，代表性范围大致是：
- benchmark families：**2–6 个**；
- 真正主 baseline：**3–8 个**；
- 再加 1 个 Oracle / privileged upper bound 很常见。

对本项目，最合理的量级不是 10+ benchmark，而是：

> **1 个 controlled synthetic + 3 个 real benchmark families；6 个跨场景核心 baseline + 2–3 个问题专属 baseline + Oracle。**

这既不显得实验单薄，也不会让 8 页 AAMAS 论文失去中心。

## 5. 最终 benchmark 收敛

### B0. Synthetic Dynamic Tool Bandit（controlled testbed）

不是为了“刷数据集”，而是测机制和理论假设。

控制变量：
- arms K；
- context dimension；
- tool similarity；
- prior mismatch；
- reward noise；
- new-arm arrival time；
- warmup budget B；
- context shift。

测 C2/C3/C4。

### B1. LLMRouterBench Performance Setting（generic provider routing）

固定使用 performance setting：
- 15 datasets；
- 20 lightweight LLM arms；
- per-instance query / output / ground truth / score；
- 已有统一 baseline framework；
- 支持 ID/OOD。

主作用：
- 测 C1：query-dependent provider competence；
- 测 C2/C3：held-out model 作为 new arm；
- 大规模验证方法不是 BBB-specific。

主要 reward：
[
r_{t,i}=\text{benchmark score}(x_t,T_i)\in[0,1].
]

主结果按 dataset macro-average，防止大数据集支配。

Performance-Cost 的 13 flagship models / 10 datasets 只作为 appendix cost-aware extension，不进入主 claim。

### B2. BEIR Same-Function Retriever Pools（真正普通工具）

固定两个 pool，直接对齐最近 same-function routing 工作：

**SciFact**
- 300 test claims；
- 3 retrievers：GTE-base / BGE-base / MiniLM；
- reward：per-query NDCG@10。

**NFCorpus**
- 323 test queries；
- 3 retrievers：TF-IDF fusion / word TF-IDF / title-only；
- reward：per-query NDCG@10。

主作用：
- 排除“我们其实只是在做 LLM model routing”的质疑；
- 三个 provider 真正暴露同一 retrieval interface；
- qrels 给客观 reward，不依赖 LLM judge。

测 C1/C2/C3，active calibration 可测 C4。

### B3. Scientific Tool Routing（真实科学工具）

**主论文 mandatory：BBB**
- 722 usable molecules；
- 7 tool families；
- 外部 experimental/reference truth；
- 当前 best fixed ≈ 91.55%；
- per-instance oracle ≈ 98.06%；
- 47/722 为 best-fixed 错但其他工具至少一个正确。

reward：
[
r_{t,i}=\mathbb I(T_i(x_t)=y_t).
]

split：
- 必须按 molecular scaffold / OOD 做；
- 不允许 random molecule split 作为主结果。

主作用：
- 给出真实 scientific tool application；
- 外部 truth 而不是 router 自己的 judge；
- 测 C1/C2/C3/C4。

**hERG / AMES：appendix / robustness，不作为 AAMAS 主论文完成的硬前置。**
若能在截止前真值与工具矩阵接齐，则作为跨 endpoint robustness；否则不拖住主线。

## 6. 为什么不再把 ASlib / xRouteBench 放主正文

ASlib 很适合 methodology robustness，但 agent/tool story 弱；放 appendix 即可。

xRouteBench 数据结构很好，但 LLMRouterBench 已 peer-reviewed、baseline ecosystem 更成熟；两个都放正文会重复证明“model routing”。

ToolRet 用于 Introduction 支撑 tool ecosystem scaling，不进入 Bandit 主实验。

## 7. Baseline 最终分层

### 跨所有 benchmark 的 6 个核心 baseline

B0 Random  
用途：sanity lower bound。

B1 Best Fixed  
定义：只用 history/train 部分估计全局平均 reward，测试期永远选同一 arm。  
用途：回答“是否真的需要 routing”。

B2 Thompson Sampling  
非 contextual online baseline。  
用途：回答“只学每个工具全局均值是否足够”。

B3 LinUCB  
经典 contextual online baseline。  
用途：回答“简单 context-dependent routing 是否已经解决问题”。

B4 LinTS  
经典 Bayesian contextual baseline。  
用途：与 shared-prior Bayesian 方法公平比较。

B5 Oracle  
每个 x 都选择真实 reward 最优 arm，只作 upper bound。  
用途：定义 routing headroom / oracle gap，不作为可部署 baseline。

### 问题专属的 3 个 baseline

B6 MTSS — Meta Thompson Sampling for Structured Bandits（AISTATS 2023）  
必须跑。该方法用 Bayesian hierarchical model 在 item/arm 间共享信息，并明确把 cold-start new items 作为适用场景。  
用途：回答“shared hierarchical prior + feature-guided cold start”是否已经足够。  
**直接后果：shared prior 本身不能作为我们的 novelty。**

B7 UCB-AA / arriving-arm baseline  
只在 new-arm arrival 主实验中跑。  
用途：回答“只处理新 arm 到达、不使用 context transfer”能做到什么。  
注：若最终版本仍只有 arXiv，则论文中明确标 concurrent baseline。

B8 LQM-ContextRoute / ContextRoute  
LLMRouterBench + BEIR 上必须跑；scientific 若接口适配自然则跑。  
用途：最近的 functionally-equivalent provider routing 对手。

### 不作为主 baseline、只做 appendix/related
- Clustered Thompson Sampling（IJCAI 2021）：放 appendix / robustness；若 MTSS 适配失败则升为主 baseline；
- MetaTS（ICML 2021）：作为“跨任务学 prior”的相关/备选 transfer baseline；
- NeuralTS：若 LinUCB/LinTS 显著受线性假设限制，再加；
- ARROW-CB：用于 warm-start discussion，但其 supervised-warmstart 假设与 new-arm transfer 不完全同构；
- ToolExpNet：放最终 Agent integration / related comparison，不直接与 bandit regret 做 apples-to-apples。

## 8. 最终 baseline 数量

主表最多同时出现：
- Random
- Best Fixed
- TS
- LinUCB
- LinTS
- MTSS
- UCB-AA
- ContextRoute/LQM
- Ours
- Oracle

即 **8 个可比较 baseline + 1 upper bound + ours**。

这与 NeuralTS 的 7 baseline、LLMRouterBench 的 10 router、AAMAS 方法论文常见 3–5 baseline 是同一数量级；不会显得少，也不会失控。

## 9. 统一评价指标

### M1. Normalized Post-Arrival Regret（主指标）

新工具在 t0 到达后：

[
R_{post}(H)=\sum_{t=t_0}^{t_0+H-1}(r_t^*-r_t).
]

为跨 benchmark 可比，报告 normalized 版本：
[
NR_{post}=\frac{R_{method}-R_{oracle}}{R_{random}-R_{oracle}}.
]

Lower is better。

### M2. Oracle Gap Closed（主解释指标）

[
OGC=\frac{U_{method}-U_{bestfixed}}{U_{oracle}-U_{bestfixed}}.
]

表示方法吃掉了多少可路由空间。

### M3. Cumulative Regret Curve

画从 new-arm arrival 开始的 regret-vs-round curve。

用途：
看 cold-start spike 与恢复速度，不把所有动态信息压成一个终值。

### M4. Warmup Sample Efficiency

对 B ∈ {0,4,8,16,32}：
- 记录 onboarding 后固定 horizon 的 post-arrival regret；
- 画 regret vs B。

这是 C2/C4 的核心指标。

### M5. Native Task Quality

用于解释：
- LLMRouterBench：macro AvgScore / Accuracy；
- BEIR：mean NDCG@10；
- BBB：Balanced Accuracy + Accuracy。

主结论仍以 regret 为中心，native quality 只是帮助读者理解实际效果。

### M6. Cost / Probe Count

主文统一把“主动 probe 次数”作为探索成本。

provider monetary cost / latency 只在 LLMRouterBench performance-cost appendix 与 BEIR latency appendix 报：
- PerfGain；
- CostSave；
- ParetoDist / latency-quality frontier。

避免一个人为 λ 同时绑死所有 benchmark。

## 10. 实验重复与统计

Offline replay 很便宜：
- 每个 held-out arm 都轮流作为 new arm；
- 每个 arm 至少 10 个 stream-order seeds；
- synthetic 50 seeds；
- 主结果报告 mean + 95% bootstrap CI。

LLM post-train RL 昂贵：
- 至少 3 independent training seeds；
- 不与 cheap replay 的 seed 数混淆。

## 11. 最终能力—benchmark 映射

| 能力 | Synthetic | LLMRouterBench | BEIR Retriever | BBB |
|---|---:|---:|---:|---:|
| C1 Contextual discrimination | ✓ | ✓ | ✓ | ✓ |
| C2 New-arm cold start | ✓ | ✓ | ✓ | ✓ |
| C3 Online adaptation | ✓ | ✓ | ✓ | ✓ |
| C4 Active probing | ✓ | Calibration setting | ✓ | ✓ |
| Cost/latency robustness | controlled | appendix | appendix | probe-count only |
| OOD generalization | controlled shift | ID/OOD | cross-domain | scaffold split |

## 12. 论文实验量结论

完成下列四类环境即可认为 benchmark 层完整：
1. Synthetic mechanism；
2. LLMRouterBench generic；
3. BEIR same-function tools；
4. BBB scientific real-world。

不再因为“benchmark 数量焦虑”继续加 ASlib/xRouteBench/ToolBench。
如果核心 claim 在这四层都站不住，增加更多 benchmark 也救不了方法。
