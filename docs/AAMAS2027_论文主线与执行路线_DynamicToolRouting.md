# AAMAS 2027 论文主线与执行路线：Dynamic Tool Routing

> 当前完整说明见 [医药论文主线与进度](AAMAS2027_论文主线与进度.md)，执行状态见 [任务清单](任务清单.md)。下文保留历史路线，不能用其“立即下一步”替代当前任务。

> **2026-09-22 范围更正：本稿的通用动态工具接入路线不再作为当前执行主线。** Owner 指定的主线是医药科学工具可靠性：主 Agent 真实 SFT/RL 学习请求功能、复核、作答与弃答，同功能 Bandit 学习当前输入选哪个具体工具。意图依据为 `/Users/jingwu/Desktop/科学工具可靠性_Agent阶段汇报_v7_20260918.md` §2.5、§2.6、§4.3 及当日澄清。新工具冷启动属于泛化支线；不能用 ASlib 的试用损失替代医药证据，也不能把主 Agent 的参数训练降级为可选展示。下文保留作原始路线草案，不据此扩大跑批。

> 日期：2026-09-20  
> 状态：Working Draft / 路线草案。  
> 注意：本文件用于规划新论文主线，**暂不自动 Supersede 已 Accepted 的 ADR-0006 / ADR-0007 / inherited ADR-0028**。现有 benchmark 工程仍按既有 ADR 执行；若本路线正式成为仓库权威，需要另立 ADR 明确迁移关系。

## 0. 论文目标

目标 venue：AAMAS 2027 Main Track  
Primary area：LEARN  
Secondary framing：GAAI

Important dates：
- Abstract：2026-10-01
- Paper：2026-10-08
- Rebuttal：2026-11-20 ~ 11-24
- Notification：2026-12-21
- Main text：8 pages

核心是 single autonomous agent 在动态工具生态中的 online adaptation，不以“多智能体”作为卖点。

## 1. 核心问题

### 1.1 Motivation

现有 tool retrieval：

query -> relevant tools

但多个工具都能完成同一功能时，relevance 不能回答：

“当前具体任务上，哪个工具最好？”

真实工具生态还会持续变化：
- 新工具进入；
- 工具版本升级；
- provider 质量/成本变化；
- 新工具没有历史。

### 1.2 Technical problem

对 capability f：

A_f^(t) = {T_1,...,T_Kt}

任务：

x_t ~ X

工具能力：

mu_i(x) = E[r | x,T_i]

选择 i_t，只观察 selected arm feedback。

新工具：

T_new arrives at t0,
D_new(t0)=empty

一次 probing 有 cost，不能大规模采样恢复 mu_new(x)。

核心问题：

> 在有限 probing budget 下，如何尽快判断新工具在哪些 context 上有价值，并把它纳入长期 exploitation？

## 2. 与已有工作的边界

已有：
1. 大规模 tool retrieval；
2. fixed-pool model/tool routing；
3. UCB/TS/LinUCB 等 contextual bandit；
4. AAMAS 2026 contextual LLM provider selection。

不声称：
- 首次用 contextual bandit 选工具；
- 首次做 LLM routing；
- 首次发现工具越来越多。

候选贡献：
1. dynamic same-function tool pool 的 new-arm cold-start formulation；
2. shared capability prior；
3. active warmup；
4. online adaptation；
5. generic + scientific routing 验证。

理论目标：优先争取 regret / adaptation / sample-efficiency guarantee。

## 3. 系统架构

### 3.1 Capability organization

轻量 router/LLM 把 raw tools 组织成：

f1 : {T11,T12,...}
f2 : {T21,T22,...}

这层是接口，不作为主要 novelty。

### 3.2 Main Agent

LLM 只决定：
- request capability；
- verify；
- answer；
- abstain；
- stop。

不直接输出具体 provider identity。

### 3.3 Online Tool Manager

每个 capability 对应一个 Online Contextual Bandit：

Bandit_f(x_t,history_t) -> T_i

反馈到达后在线更新。

完整链：

Main LLM -> function key -> Bandit -> concrete tool -> result -> Main LLM

## 4. 第一版方法

### 4.1 Shared representation

z = phi(x)

第一版先冻结 phi：
- molecule：Morgan / molecular encoder；
- text：frozen sentence/LLM embedding。

先避免 representation learning 与 bandit learning 混在一起。

### 4.2 Hierarchical prior

同一 capability：

w_i ~ N(m_f,Sigma_f)

二值 reward：

P(r=1|x,T_i)=sigmoid(z^T w_i)

新工具：

w_new ~ N(m_f,Sigma_f)

而不是完全从零。

### 4.3 Active warmup

probe budget：

B in {0,4,8,16,32}

策略：
- random；
- uncertainty；
- information gain。

目标：

x* = argmax_x I(w_new;r|x,D_new)

每次 probe 后 posterior update。

### 4.4 Online phase

warmup 后 contextual TS/UCB。

带成本 utility：

u_i(x)=mu_i(x)-lambda c_i

持续更新：

D_i <- D_i U {(x_t,r_t,c_t)}

### 4.5 Dynamic pool

第一版只把 **new-arm arrival** 做透。
tool drift / retire / versioning 先作为扩展，不一次塞满。

## 5. Benchmark 路线（2026-09-21 T1 后冻结）

最终 main benchmark 不再继续扩张：

### B0 Synthetic Dynamic Tool Bandit
机制 / 理论环境；控制 K、context dimension、arm similarity、noise、arrival、probe budget。

### B1 LLMRouterBench Performance
- 当前官方 frozen bundle：11,481 prompts × 20 models；
- 本地 Best Fixed 0.6923 / Oracle 0.9186；
- prompt-only TF-IDF + Ridge 5 seeds 比 fold Best Fixed **+2.20 pp**；
- T1 **PASS**。

### B2 ASlib SAT12-ALL
- 1,614 instances × 31 solvers；113 instance features；
- 官方 CV repetition=1；
- Best Fixed 3079.9 PAR10 / Oracle 241.3；
- 固定 Ridge alpha=10 将 PAR10 降到 1486.9（−51.72%）；
- T1 **PASS**。

### B3 Scientific BBB — hard application
- 722 usable × 7 tool families；
- Best Fixed 0.9155 / Oracle 0.9806；
- 有 6.51pp routing headroom，但 simple scaffold-OOD selector 低于 fixed；
- 保留为真实 scientific **hard stress test**，不冒充 simple-learnability PASS。

### Appendix / boundary control
**BEIR-SciFact dense**：GTE/BGE/MiniLM 本地 NDCG@10=0.7618/0.7404/0.6451，与最近 same-function routing 文中数值逐项吻合；但 20 split seeds 下 simple contextual gain 平均 −0.00344。因此它证明“同功能 provider ≠ 必然值得 contextual routing”，降为 negative control，而不是 main positive benchmark。

完整验收矩阵以 `docs/AAMAS2027_完整实验矩阵_DynamicToolRouting_v2.md` 为准。

## 6. Baseline（v2）

### Universal / canonical
1. Random
2. Best Fixed Tool
3. Thompson Sampling（non-contextual）
4. LinUCB
5. LinTS / Contextual TS

### Transfer baseline
6. **P-LinUCB / P-TS（AISTATS 2024 shared affine subspace）**：学习跨 tools/tasks 的共享 context structure，直接检验“历史工具能否让新工具 few-shot 更快”。

### Nearest dynamic / provider routing
7. **PAK-UCB / RFF-UCB（ICML 2025）**：peer-reviewed prompt-aware contextual model routing。
8. **FC-Entry（ICML 2024）**：peer-reviewed entering-arm bandit，作为 arrival-only baseline。

### Concurrent nearest work（正文空间允许则加，至少 appendix）
- UCB-AA：2026 arriving-arm screening，直接针对 arrival information discrepancy；若仍未正式发表则明标 concurrent。
- ContextRoute / LQM-ContextRoute：fixed same-function provider routing；若仍为 preprint 则明标 concurrent。

### Upper bound
9. Per-instance Oracle

### Protocol / ablation
- Random Warmup
- No Shared Transfer
- No Context
- No Online Update
- Independent New-arm Model
- Active Probe → Random Probe

主协议**不提供 new tool 的 tool-side competence metadata**。因此依赖 item-side metadata 的 MTSS 不作为 universal main baseline，只放 metadata-enabled appendix。B=0 时新工具的个体 specialization 不可识别；主比较是 B∈{0,1,4,8,16,32} 个 behavioral outcomes 后的 adaptation speed。

## 7. 主指标

Primary：
- cumulative regret；
- post-arrival regret；
- probing cost；
- pulls to recovery；
- quality-cost utility。

Secondary：
- final routing accuracy；
- new-tool utilization；
- oracle gap closed %。

主图：
t0 加入新工具后，regret/utility 的 cold-start spike 与 recovery curve。

## 8. Go / No-Go

### G0 Dataset audit — ✅
LLMRouterBench / ASlib / BBB 已实盘；BEIR 作为 negative control 已实跑。

### G1 Routing headroom — ✅
LLMRouterBench / ASlib / BBB / BEIR 均存在不同幅度的 Best Fixed→Oracle headroom。

### G2 Learnability — ✅ benchmark qualification 已完成
- LLMRouterBench：prompt-only simple contextual **PASS**（+2.20pp）；
- ASlib：official-CV simple contextual **PASS**（PAR10 −51.72%）；
- BBB：simple routing **FAIL / hard stress**；
- BEIR-SciFact：simple routing **FAIL / negative control**。

因此不是声称“所有 tool pools 都可 context-route”，而是只在具有 context-dependent competence structure 的 pool 上研究 new-tool adaptation。

### G3 Cold-start replay
held-out new arm：
B=0/1/4/8/16/32；
先 random warmup。

先证明 new-tool cold-start 真的是可测问题。

### G4 Shared prior
independent prior vs group/shared prior。

### G5 Active warmup
random vs uncertainty vs information gain。

只有 G1–G5 成立，才把 active warmup 写成核心 contribution。

### G6 Agent integration
最后再做 LLM SFT/RL。

## 9. LLM Post-train RL

必须有真实参数训练，但放在方法后段。

LLM action：
- capability call；
- verify；
- answer；
- abstain；
- stop。

训练：
Base LLM -> SFT warm start -> post-train RL。

Reward：
- task correctness；
- abstention cost；
- capability call cost；
- final utility。

Bandit 在环境侧负责 concrete provider selection。

作用：
证明 Bandit 可以嵌入 post-trained autonomous agent，而不是离线 routing toy。

## 10. 实验矩阵

### E1 Synthetic
regret / B sensitivity / prior mismatch / active probing。

### E2 Generic provider routing
LLMRouterBench：
static vs contextual；
held-out new model；
shared prior；
active warmup。

### E3 Scientific routing
BBB + 后续 hERG/AMES：
oracle headroom；
scaffold learnability；
cold-start；
majority/static/global-best。

### E4 Agent integration
- no bandit
- static best provider
- online bandit
- trained Agent + online bandit

## 11. 8 页论文结构

1. Introduction — 1p
2. Related Work — 0.7p
3. Problem Formulation — 1p
4. Method — 1.5–2p
5. Theory / Analysis — 1p
6. Experiments — 2–2.5p
7. Conclusion / Limitations — 0.3p

正文必须放 problem、method、最强 theorem/claim、主结果，不能依赖 supplement 才能理解。

## 12. Contribution 草案

1. **Problem**: formulate dynamic contextual tool routing with newly arriving zero-history tools and costly feedback.
2. **Method**: hierarchical shared-prior contextual routing with active warmup.
3. **Analysis**: derive regret/adaptation or sample-efficiency guarantee under explicit assumptions.
4. **Evidence**: synthetic + generic provider routing + real scientific tools.
5. **Agent integration**: plug into a post-trained LLM Agent without binding its policy to concrete provider identities.

## 13. 时间线

当前日期：2026-09-20。

### 09-20 ~ 09-22
- benchmark audit
- G1/G2
- baseline 复现可行性
- problem statement v1

### 09-23 ~ 09-25
- G3/G4
- shared prior prototype
- synthetic environment
- initial regret curves

### 09-26 ~ 09-28
- G5 active warmup
- generic + BBB main experiments
- method go/no-go

### 09-29 ~ 09-30
- 主表/主图
- theory/analysis
- abstract v1

### 10-01
- abstract submit

### 10-01 ~ 10-05
- LLM Agent integration
- hERG/AMES as time allows
- ablation

### 10-06 ~ 10-07
- compress paper
- proofread
- reproducibility package

### 10-08
- full paper submit

这是高风险时间线。若 09-25 前 G2/G3 不成立，应调整 claim/venue，而不是继续堆实现。

## 14. 与现有 benchmark 仓库的关系

可复用资产：
- 49 tools inventory；
- tool-property matrix；
- BBB 722 truth-connected cases；
- divergence / pseudo-consensus；
- task21–25 分析与 pipeline；
- hERG/AMES 真值路线。

冲突：
现有 ADR-0006 把“选哪个工具”降为评测目标；新路线重新把 dynamic tool routing 提升为核心算法问题。

因此若 owner 正式批准本路线，需要新 ADR：
1. 旧 benchmark 作为数据/环境资产继续保留；
2. 新论文核心改为 dynamic contextual tool routing；
3. 明确哪些旧训练目标 supersede；
4. LLM RL 改为 capability-level integration。

在新 ADR 前，本文件仅作为路线草案，不修改既有 Accepted 决议。

## 15. 立即下一步

只做四件事：
1. 拉/审 LLMRouterBench 与 ASlib schema；
2. BBB 做 scaffold-split tool correctness learnability；
3. 做 held-out-arm cold-start replay；
4. 根据结果决定 shared prior + active warmup 是否进入正式 method。
