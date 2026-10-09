# AAMAS 2027 完整实验矩阵 v3 — Selective Contextual New-Tool Onboarding

> 2026-09-22：本矩阵保留为通用新工具接入方向的历史协议，不是当前医药具体模型选择的执行清单。当前对象、贡献与进度见 [论文主线与进度](AAMAS2027_论文主线与进度.md)，待办见 [任务清单](任务清单.md)。下文原状态和数值仅在原协议内解释，不删除或重包装旧结果。

> 日期：2026-09-20  
> 目的：本文件是论文实验的“验收清单”。**当所有 Mandatory 表/图填完并通过预设判据，实验部分视为完成。**  
> 状态：**Problem-qualified v3**。T1 benchmark qualification 与 G3 baseline-only phenomenon gate 已通过；主问题从泛化“dynamic routing”收紧为 **selective contextual onboarding of a newly arriving challenger tool**。后续 benchmark/baseline 不因 Ours 结果临时替换。

## 0. 论文只测一个主能力

**Selective Contextual Onboarding of Newly Arriving Challenger Tools**

给定一个已经可用、冻结的 incumbent router，当一个零历史新工具作为 challenger 加入时，系统能否：
1. 用极少 behavioral feedback 判断它是否值得纳入；
2. 学到它相对 incumbent 的局部 residual advantage，而不是重建完整绝对能力函数；
3. 只在真正有增量价值的 context 上采用它；
4. 避免因为“新工具 uncertainty 最大”而浪费大量在线流量。

定义 frozen incumbent policy 的 reward 为 `b_t = r(x_t, pi_inc(x_t))`，新工具 reward 为 `r_new,t`，核心学习对象为

\[\delta_{new}(x_t)=\mathbb E[r_{new,t}-b_t\mid x_t].\]

真正需要识别的是 `S+ = {x : delta_new(x)>0}`。

不把 semantic retrieval、API 参数生成、多工具 workflow planning 当主能力。

**Novelty guard：AISTATS 2023 MTSS 已经用 hierarchical prior 做 cold-start new items，因此“shared prior”本身不算创新。本文真正要额外测的是：当新工具缺少可信 tool-side competence features 时，能否通过极少的行为 probe 识别其 context-dependent competence profile，并快速进入 exploitation。**

---

# I. 实验总览

| ID | 产物 | Mandatory | 回答的唯一问题 | 支撑 claim |
|---|---|---:|---|---|
| T1 | Benchmark Characterization Table | ✓ | 这些 benchmark 真的存在 routing headroom 吗？ | 问题成立 |
| T2 | Stationary Routing Table | ✓ | 能否得到一个可靠、可冻结的 incumbent router，供 T3 只测 onboarding 增量？ | C1 |
| T3 | Passive New-Arm Cold-Start Main Table | ✓ | 相对同一个 frozen incumbent，新 challenger 带来的净 onboarding 价值/损失是什么？ | C2+C3 主结果 |
| F1 | Post-Arrival Regret Curves | ✓ | regret spike 与恢复过程是什么形状？ | C2+C3 |
| T4 | Warmup Budget Table | ✓ | B=0/1/4/8/16/32 条反馈能换来多少收益？ | sample efficiency |
| T5 | Active Calibration Table | ✓ | 能主动挑 probe 时，比随机 warmup 是否更省样本？ | C4 |
| T6 | Transfer / Ablation Table | ✓ | 增益到底来自 shared prior、context 还是 online update？ | 机制归因 |
| T7 | OOD / Unseen Context Table | ✓ | 是否只是记住训练 context / tool ID？ | 泛化 |
| T8 | Agent Post-train RL Integration | ✓ | 插到真正经过训练的 LLM Agent 上还有收益吗？ | Agent relevance |
| A1 | Cost-aware Routing | Appendix | 加 monetary cost / latency 后是否还成立？ | 部署扩展 |
| A2 | BEIR negative control | Appendix | 同功能工具但 context signal 很弱时，方法是否避免无意义切换？ | boundary / negative control |
| A3 | hERG/AMES extension | Appendix | 是否跨 scientific endpoint？ | domain robustness |

---

# II. Benchmark 固定

## B0 Synthetic Dynamic Tool Bandit

用途：机制 + 理论。

默认主设置：
- K ∈ {5,10,20}
- d ∈ {16,32}
- horizon T = 5000
- new arm arrival t0 = 0.4T
- reward noise 3 档
- shared-prior match / mismatch 3 档
- 50 seeds

不以某一个 synthetic 配置的数值作为论文卖点，主要看趋势与 regret scaling。

## B1 LLMRouterBench — Performance setting

固定：
- 15 performance datasets
- 20 lightweight model arms
- per-instance score ∈ [0,1]
- dataset macro-average

History/Calibration/Test：
- 使用 benchmark adaptor 的固定 split 逻辑；
- 动态 arm 实验中，某个 model 的全部 pre-arrival reward 对 learner 隐藏；
- 所有 20 arms 轮流 held out。

主 reward：
[
r(x,T_i)=score(x,T_i).
]

## B2 ASlib SAT12-ALL — Classic Contextual Algorithm Selection

固定：
- 1,614 SAT instances
- 31 solver arms
- 113 usable instance features
- official `cv.arff` repetition=1 split
- native objective = PAR10 runtime（越低越好）

统一 bandit reward：

\[
r = 1 - \frac{\text{PAR10}}{12000} \in [0,1].
\]

Native metric 仍报告 PAR10；bandit regret 在归一化 reward 上计算。

T1 已实测：固定 alpha=10 的 per-arm Ridge 即把 PAR10 从 3079.886 降到 1486.937，说明这里有强、可泛化的 context-dependent competence structure。

所有 31 solvers 轮流作 new arm。

### Appendix negative control: BEIR-SciFact dense pool

GTE-base / BGE-base / MiniLM 已本地精确复现论文 aggregate NDCG，但 20 个 CV seeds 的简单 contextual selector 平均 **低于 Best Fixed 0.00344 NDCG**。因此 BEIR 不再承担 positive benchmark 职责；保留为边界条件：**并非所有同功能 provider pool 都值得 contextual routing。**

## B3 Scientific BBB

固定：
- usable 722 molecules
- 7 tool families
- external truth
- binary reward

切分：
- molecular scaffold/OOD split 作为主结果；
- random split 只允许 appendix sanity check。

所有 7 tool families 轮流作 new arm。

Native metrics：
- Accuracy
- Balanced Accuracy

---

# III. Baseline 固定

## A. T2 stationary incumbent references

| Method | 作用 |
|---|---|
| Best Fixed | 最简单 production incumbent；也是 BBB 当前最强简单 frozen policy |
| Thompson Sampling / Beta-TS | non-contextual online reference |
| LinUCB | 经典 contextual online reference |
| LinTS | Bayesian contextual reference |
| Offline per-arm reward regression | history-only frozen contextual incumbent candidate |
| PAK-UCB / RFF-UCB (ICML 2025) | peer-reviewed prompt-aware fixed-pool contextual model selection |

T2 的目的不是挑“对 Ours 最有利”的 incumbent，而是得到一个真实可用的 frozen policy。正式 T3 的 incumbent 只允许由 history data 训练并在 challenger 到达前冻结。

## B. T3/T4 challenger-onboarding 主 baseline

| ID | Method | 类型 | 它回答的问题 |
|---|---|---|---|
| BL0 | Frozen Incumbent-only | no-onboarding | 完全不尝试新工具的部署基线 |
| BL1 | Thompson Sampling | non-contextual exploration | 不看 context、只根据 challenger 全局反馈是否够用 |
| BL2 | LinUCB | contextual online | 标准 optimism 是否会因 zero-history uncertainty 过度探索 |
| BL3 | LinTS | contextual Bayesian | 标准 contextual posterior sampling 能否更稳健 |
| BL4 | PAK-UCB / RFF-UCB (ICML 2025) | fixed-pool prompt-aware routing | 强 contextual provider selector 在 challenger setting 下能到什么程度 |
| BL5 | P-LinUCB / P-TS (AISTATS 2024) | cross-task transfer | 从历史 bandit tasks 学共享低维结构是否已经足够 |
| BL6 | FC-Entry (ICML 2024) | entering-arm bandit | peer-reviewed arm-entry 方法能否处理 arrival，但不利用局部 context specialization |
| BL7 | UCB-AA (2026 concurrent) | arriving-arm screening | 最近的 arrival-information-discrepancy / screening 思路 |
| BL8 | ContextRoute/LQM (2026 concurrent) | same-function provider routing | fixed same-function router 作为最近 Agent/tool 对照；正文篇幅不足可放 supplementary |
| BL9 | Action-Centered Thompson Sampling (NeurIPS 2017) | baseline-relative contextual bandit | 直接学习相对 mature baseline 的 treatment/residual effect；检验 residual-advantage modeling 本身是否已足够 |
| UB1 | Challenger Oracle | onboarding upper bound | `max(frozen incumbent reward, challenger reward)`；**主 Oracle** |
| UB2 | Full Tool Oracle | end-to-end upper bound | 所有 incumbent tools + challenger 逐 context 最优；只作系统上界 |

**CLUCB / conservative bandit 只有在最终 formulation 加显式 safety constraint 时才 mandatory；否则只在 related work。**

**Novelty guard：Action-Centered Contextual Bandits 已经把 reward 分成 complex baseline + simpler treatment effect，因此“学习 residual advantage 而非 absolute reward”本身也不算创新。本文候选增量必须来自 arrival/onboarding + transfer + few behavioral probes + sparse positive region identification 的组合。**

## C. Ours 必须做的 ablation

- no cross-tool/task transfer
- no context（退化成 global challenger screening）
- no online update
- random probing vs active probing（仅 T5）
- no residual-advantage modeling：直接预测 challenger absolute reward
- tool identity / metadata disabled（主协议默认就没有 competence metadata）

---

# IV. T1 — Benchmark Characterization Table

**目的：先证明问题存在；这张表不过，后面全部实验没有意义。**

每个 benchmark 填：

| Benchmark | #contexts | #arms | reward | Best Fixed | Oracle | Headroom | Simple Contextual Gain | Qualification |
|---|---:|---:|---|---:|---:|---:|---:|---|
| LLMRouterBench | **11,481** | 20 | score | 0.6923 | 0.9186 | +0.2263 | **+0.0220** prompt-only | **PASS / main** |
| ASlib SAT12-ALL | 1,614 | 31 | PAR10 ↓ | 3079.9 | 241.3 | 2838.6 | **51.72% PAR10 reduction** | **PASS / main** |
| BBB | 722 | 7 | correctness | 0.9155 | 0.9806 | +0.0651 | **-0.0208** | **HARD / scientific stress test** |
| BEIR-SciFact dense | 300 | 3 | NDCG@10 | 0.7618 | 0.8012 | +0.0394 | **-0.00344 mean over 20 seeds** | **FAIL positive gate / appendix negative control** |

### Simple Contextual Gain
T1 是**离线可学习性诊断**，不把它与 T2 的在线 LinUCB baseline 混在一起。固定原则是：只用 context 特征与 train outcome 学每个 arm 的 expected reward/correctness，再在 held-out split 选 predicted-best arm；模型保持简单、不得为过 Gate 反复调大模型。当前实跑：BBB 用 Morgan + per-arm Logistic（另用 RF/ExtraTrees 作一次复核），BEIR lexical proxy 用 query TF-IDF + per-arm Ridge。T2 才正式使用 LinUCB/LinTS 等在线 baseline。

### Gate
至少 B1/B2/B3 三个 real families 中，2 个满足：
- Oracle > Best Fixed 有非平凡 gap；
- simple contextual method > Best Fixed。

**2026-09-21 最终状态：PASSED（benchmark qualification 完成）。** Gate 没有降级：LLMRouterBench 与 ASlib 同时满足“non-trivial headroom + held-out simple contextual gain”；BBB 与 BEIR 的失败结果原样保留，并据此分别定位为 scientific hard case 与 negative control。详见 `task/task26-AAMAS2027-T1-benchmark-characterization/report.md`。

否则不进入复杂 method。

---

# V. T2 — Stationary Routing Table

**目的：分离“context 有用”与“新工具 cold-start”两个问题。**

设置：
- 工具池固定；
- 没有 arrival；
- 所有方法相同历史预算。

方法：
BL0/BL1/BL2/BL3/BL4/BL5/BL7/Ours/Oracle。

主指标：
1. normalized cumulative regret ↓
2. Oracle Gap Closed ↑
3. native task quality ↑

结果按 benchmark block 汇总。

如果 Ours 在 stationary setting 比 LinUCB/LinTS 明显更差，则 new-arm 优势不能以牺牲常态性能换取。

---

# VI. T3 — Passive Challenger Onboarding Main Table

**这是整篇论文最重要的一张表。**

### 固定实验链

1. T2 先训练并冻结 incumbent router `pi_inc`；
2. 某个 held-out tool 在 `t0` 作为 challenger 加入；
3. challenger 的 history reward 完全隐藏；
4. 每轮 onboarding manager 只做二元决策：
   - delegate to frozen incumbent router；
   - use/probe challenger；
5. 只 reveal 实际选择路径的 reward；
6. 每个 tool 都轮流做 challenger，禁止 cherry-pick。

### 两个必须同时报告的 counterfactual

**Matched Incumbent-only**：相同 history / context / seed / incumbent router，但 challenger 不开放。

**Challenger Oracle**：

\[r_t^{C*}=\max(b_t,r_{new,t}).\]

Full Tool Oracle 只保留 end-to-end upper bound，不作为 onboarding 主 oracle。

### 主方法表

Best Incumbent / TS / LinUCB / LinTS / P-LinUCB(or P-TS) / PAK-UCB / FC-Entry / UCB-AA(concurrent) / Ours / Challenger Oracle。

ContextRoute 作为 fixed-pool nearest work 放 supplementary 或同表备注；若投稿时仍仅 preprint，明确 concurrent。CLUCB 只有在最终 formulation 加显式 safety constraint 时才 mandatory。

### 主指标

1. **Incremental Onboarding Utility (IOU)** ↑
\[\Delta U_{onboard}=U(method+challenger)-U(matched\ incumbent-only).\]

2. **Challenger Regret** ↓
\[R_C(H)=\sum_{t=1}^{H}(r_t^{C*}-r_t).\]

3. **Beneficial-adoption Precision / Recall** ↑
   - Precision：选 challenger 时它真比 incumbent 好的比例；
   - Recall：challenger 真更好时，有多少被成功采用。

4. Harmful-adoption rate ↓；new-tool usage rate 作为诊断，不作为越高越好的指标。

### Horizon

报告 arrival 后 25% / 50% / 100% relative horizon；正文主表报 100%，F1 展示全过程。

**主 claim：Ours 不是“更爱用新工具”，而是用更少 trial 精确找到 challenger 的局部正增益区域，获得正 IOU 并降低 Challenger Regret。**

---

# VII. F1 — Post-Arrival Regret Curves

每个 real benchmark family 一张曲线或同图不同 panel：

x-axis：arrival 后 round  
y-axis：mean cumulative **Challenger Regret**；附图可画 cumulative IOU

至少画：
- Frozen Incumbent-only
- LinUCB
- LinTS
- UCB-AA
- ContextRoute
- Ours
- Oracle=0 reference

95% CI。

这张图回答：
- 谁最先发生 cold-start spike；
- 谁恢复快；
- 是否只是终点相同但前期损失不同。

---

# VIII. T4 — Warmup Budget / Sample Efficiency

**目的：把“少量反馈就能认识新工具”量化。**

B：`{0,1,4,8,16,32}`。B=0 只作 zero-information sanity；**sample-efficiency claim 从 B>=1 开始**。

Passive warmup：只从到来的自然 stream 中累计前 B 次新工具 feedback，不主动挑 x。

方法：
- LinUCB
- LinTS
- MTSS
- UCB-AA
- Ours

指标：
- fixed evaluation horizon 的 Challenger Regret ↓
- Incremental Onboarding Utility ↑
- beneficial-adoption precision / recall ↑

报告：
每个 B 的 mean ± 95% CI。

核心结论不是 B=32 最好，而是：
> 在相同 Challenger Regret / IOU 目标下，Ours 需要更少 behavioral feedback。

---

# IX. T5 — Active Calibration Table

**只在允许 calibration pool 的 setting 做。**

Benchmark：
- Synthetic
- ASlib SAT12-ALL
- BBB

不把 LLMRouterBench 自然 query setting 强行解释成可主动选用户 query。

B：
4 / 8 / 16 / 32。

Probe policy：
1. Random
2. Diversity / k-center
3. Uncertainty
4. Information Gain（若最终方法采用）
5. Oracle probe（仅 upper bound，可选）

所有 probe policy 后使用同一个 online router，避免把 probe 策略与后续 policy 混淆。

指标：
- post-calibration Challenger Regret
- Incremental Onboarding Utility
- improvement per probe
- beneficial-adoption precision / recall

**这张表决定 active warmup 是否有资格成为主 contribution。**
若 information gain 不稳定优于 random，则从主方法中降级，不硬写创新。

---

# X. T6 — Transfer / Ablation Table

**目的：证明 method 的增益来自哪里。**

固定主 benchmark：
- LLMRouterBench
- ASlib SAT12-ALL
- BBB
- Synthetic

设置：
new arm + B=8（中等预算）。

Ablation：
- Full
- – Shared Prior
- – Context
- – Online Update
- – Transfer from Old Arms（新 arm 完全独立）
- Random Probe（active setting）
- Frozen Tool Embedding / Identity-only（若使用 metadata）

指标：
- NR_post ↓
- OGC ↑

如果 “– Transfer from old arms/tasks” 几乎不掉，则 transfer 不能作为 contribution。
如果 “– Context” 不掉，则问题退化成 non-contextual arriving-arm screening。

---

# XI. T7 — OOD / Unseen Context Table

**目的：防止学成 tool ID / dataset ID 映射。**

LLMRouterBench：
- 使用其 ID/OOD setting；
- 额外按 dataset category leave-one-domain-out 汇总。

BBB：
- scaffold/OOD split；
- 不报 random split 作为主证据。

ASlib：
- 主结果使用官方 CV；不额外制造 OOD split。ASlib 不承担 T7 的 OOD claim。

BEIR：
- 仅作为 negative control / appendix，不承担 OOD claim。

方法：
Best Fixed / LinUCB / LinTS / ContextRoute / Ours / Oracle。

指标：
- NR_post
- OGC
- native quality

---

# XII. T8 — LLM Post-train RL Agent Integration

**目的：证明本方法不是一个脱离 Agent 的离线路由器。**

这一张表必须真实更新 LLM 参数。

训练对象：
Main LLM capability-level policy。

动作：
- request capability
- verify
- answer
- abstain
- stop

具体 provider 不进 LLM action space，由 Bandit 决定。

至少四组：
1. Base LLM + Best Fixed router
2. SFT Agent + Best Fixed router
3. SFT + Post-train RL Agent + Best Fixed router
4. SFT + Post-train RL Agent + Ours Online Router
5. SFT + Post-train RL Agent + Oracle Router（upper bound，可选）

Benchmark：
- BBB scientific Agent environment 的 held-out test；
- 引入一个 held-out/new tool 的 episode。

指标：
- final task accuracy / balanced accuracy
- answer coverage / abstain rate
- mean capability calls
- total probe/tool calls
- end-to-end utility
- new-tool usage rate

**这张表只证明 integration value，不承担 Bandit 算法主 claim。**

训练至少 3 independent seeds。

---

# XIII. A1 — Cost-aware Appendix

LLMRouterBench proprietary performance-cost setting：
- 10 datasets
- 13 flagship model arms

指标沿用 benchmark 原生：
- PerfGain ↑
- CostSave ↑
- ParetoDist ↓

BEIR：
- mean NDCG@10
- latency
- latency-quality Pareto frontier

目的：
证明方法加入实际 provider cost/latency 后仍然成立。

不把单一人为 λ 作为全文唯一 cost trade-off。

---

# XIV. 统计与显著性

Offline replay：
- 每个 arm 都轮流 held out；
- 每个 arm × 10 stream seeds；
- synthetic 50 seeds；
- paired bootstrap 95% CI。

主比较：
Ours vs strongest non-oracle onboarding baseline，且所有方法必须与自己的 matched incumbent-only counterfactual 成对比较。

额外显著性：
paired permutation / bootstrap difference，按 benchmark family 报，不对几十个小格逐一刷 p-value。

---

# XV. 实验完成判据

论文实验只有在下列全部完成后才算“做完”：

- [x] T1 benchmark qualification 完成：LLMRouterBench + ASlib PASS；BBB hard case；BEIR negative control
- [x] T2 stationary routing / frozen incumbent policy classes
- [ ] T3 passive challenger-onboarding 主表（含 matched incumbent-only + Challenger Oracle）
- [ ] F1 regret curves
- [ ] T4 B=0/1/4/8/16/32 sample-efficiency
- [ ] T5 active calibration
- [ ] T6 ablation
- [ ] T7 OOD
- [ ] T8 real LLM post-train RL integration
- [ ] A1 cost-aware appendix
- [ ] 全部 held-out arms 轮换，不 cherry-pick
- [ ] 所有主数字有 95% CI
- [ ] 代码与 split 固定、结果可重复

hERG/AMES 是 robustness bonus，不作为主论文完成的硬前置。BEIR 固定为 negative control，不再升回主 benchmark。

---

# XVI. 从表格到论文段落的一一对应

| 论文论断 | 必须由什么实验支撑 |
|---|---|
| 同功能工具存在 context-dependent complementarity | T1 |
| incumbent router 已经可用，但新 challenger onboarding 仍有独立困难 | T2 + T3 |
| new-arm uncertainty 会造成 over-exploration，而 incremental value 稀疏 | T3 + F1 |
| 少量 behavioral feedback 后更快识别 challenger 的局部正增益 | T4 |
| active probing 有价值 | T5 |
| shared prior 是必要机制 | T6 |
| 不是记忆 tool/dataset ID | T7 |
| 可以服务真正的 post-trained Agent | T8 |
| 部署成本下仍成立 | A1 |

当这张映射右侧全部有数字，Introduction 中的每一个 claim 都有对应实验证据，实验部分即闭环。
