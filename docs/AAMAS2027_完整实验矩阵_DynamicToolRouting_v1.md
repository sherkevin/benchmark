# AAMAS 2027 完整实验矩阵 v1 — Dynamic New-Tool Adaptation

> 2026-09-22：本矩阵保留为通用新工具接入方向的历史协议，不是当前医药具体模型选择的执行清单。当前对象、贡献与进度见 [论文主线与进度](AAMAS2027_论文主线与进度.md)，待办见 [任务清单](任务清单.md)。下文原状态和数值仅在原协议内解释，不删除或重包装旧结果。

> 日期：2026-09-20  
> 目的：本文件是论文实验的“验收清单”。**当所有 Mandatory 表/图填完并通过预设判据，实验部分视为完成。**  
> 状态：**Superseded by `AAMAS2027_完整实验矩阵_DynamicToolRouting_v2.md`（2026-09-21）**。v1 保留作为 benchmark qualification 前的预注册候选；T1 实测后按原 Gate 淘汰 BEIR 正向主 benchmark、引入 ASlib，并修正 full-pool new-arm 饱和问题。

## 0. 论文只测一个主能力

**Rapid Context-Dependent Adaptation to Newly Arriving Tools**

给定一批功能等价工具，当一个零历史新工具加入时，系统能否：
1. 根据当前 context 判断工具价值；
2. 利用旧工具的共享经验形成合理 cold-start prior；
3. 从 selected-arm bandit feedback 在线更新；
4. 在有限 probing budget 下快速降低 post-arrival regret。

不把 semantic retrieval、API 参数生成、多工具 workflow planning 当主能力。

**Novelty guard：AISTATS 2023 MTSS 已经用 hierarchical prior 做 cold-start new items，因此“shared prior”本身不算创新。本文真正要额外测的是：当新工具缺少可信 tool-side competence features 时，能否通过极少的行为 probe 识别其 context-dependent competence profile，并快速进入 exploitation。**

---

# I. 实验总览

| ID | 产物 | Mandatory | 回答的唯一问题 | 支撑 claim |
|---|---|---:|---|---|
| T1 | Benchmark Characterization Table | ✓ | 这些 benchmark 真的存在 routing headroom 吗？ | 问题成立 |
| T2 | Stationary Routing Table | ✓ | 没有新工具时，context 本身是否有用？ | C1 |
| T3 | Passive New-Arm Cold-Start Main Table | ✓ | 零历史新工具自然到来后，谁适应最快？ | C2+C3 主结果 |
| F1 | Post-Arrival Regret Curves | ✓ | regret spike 与恢复过程是什么形状？ | C2+C3 |
| T4 | Warmup Budget Table | ✓ | B=0/4/8/16/32 条反馈能换来多少收益？ | sample efficiency |
| T5 | Active Calibration Table | ✓ | 能主动挑 probe 时，比随机 warmup 是否更省样本？ | C4 |
| T6 | Transfer / Ablation Table | ✓ | 增益到底来自 shared prior、context 还是 online update？ | 机制归因 |
| T7 | OOD / Unseen Context Table | ✓ | 是否只是记住训练 context / tool ID？ | 泛化 |
| T8 | Agent Post-train RL Integration | ✓ | 插到真正经过训练的 LLM Agent 上还有收益吗？ | Agent relevance |
| A1 | Cost-aware Routing | Appendix | 加 monetary cost / latency 后是否还成立？ | 部署扩展 |
| A2 | ASlib robustness | Appendix | 是否只对 LLM/science 数据成立？ | robustness |
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

## B2 BEIR Same-Function Retriever

固定两组：

### SciFact
- 300 test claims
- GTE-base / BGE-base / MiniLM
- reward = per-query NDCG@10

### NFCorpus
- 323 test queries
- TF-IDF fusion / word TF-IDF / title-only
- reward = per-query NDCG@10

每个 retriever 轮流作 new arm。

> **执行注（2026-09-20）**：上述 SciFact GTE/BGE/MiniLM 与 NFCorpus provider identities 仍是正式冻结候选；T1 第一轮为避免环境/下载问题阻塞，只额外跑了 BM25 / word-TFIDF / char-TFIDF 的本地 lexical proxy，用于 sanity audit，**不得静默替代正式 B2**。正式 pool 需在隔离环境复现后再更新 Gate。

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

## Core

| ID | Method | 类型 | 回答的问题 |
|---|---|---|---|
| BL0 | Random | static lower bound | 随机有多差 |
| BL1 | Best Fixed | static | 是否根本不需要 routing |
| BL2 | Thompson Sampling | non-contextual online | 只学 arm 全局均值够不够 |
| BL3 | LinUCB | contextual online | 简单线性 context 是否已足够 |
| BL4 | LinTS | contextual Bayesian | 简单 Bayesian contextual 是否已足够 |
| BL5 | MTSS (AISTATS 2023) | hierarchical/meta transfer | shared hierarchical prior 是否已解决 cold start |
| BL6 | UCB-AA / arriving-arm baseline | dynamic arms | 只处理新 arm 到达、不做 context transfer 能到什么程度 |
| BL7 | ContextRoute / LQM-ContextRoute | same-function routing | 最近同功能 provider router |
| UB | Per-instance Oracle | upper bound | routing headroom |

Ours 永远与以上同表比较。

## Protocol baselines / ablations

- Random Warmup + Ours online stage
- No Shared Prior
- No Context
- No Online Update
- Independent-arm Prior
- Active Probe → Random Probe
- Tool Identity Shuffled（若表示中含工具 metadata）

---

# IV. T1 — Benchmark Characterization Table

**目的：先证明问题存在；这张表不过，后面全部实验没有意义。**

每个 benchmark 填：

| Benchmark | #contexts | #arms | reward | Best Fixed | Oracle | Headroom | Simple Contextual Gain | Cost available? |
|---|---:|---:|---|---:|---:|---:|---:|---|
| LLMRouterBench | 11,480 | 20 | score | 0.6801 | 0.9164 | +0.2363 | **local pending**；published routers +0.032~+0.039 | yes |
| BEIR-SciFact **lexical proxy** | 300 | 3 | NDCG@10 | 0.6623 | 0.7454 | +0.0831 | **-0.0010** | latency |
| BEIR-NFCorpus **lexical proxy** | 323 | 3 | NDCG@10 | 0.3150 | 0.3538 | +0.0388 | **+0.0015 vs fold-fixed（近零）** | latency |
| BBB | 722 | 7 | correctness | 0.9155 | 0.9806 | +0.0651 | **-0.0208**（Logistic；best tree -0.0139） | probe count |

### Simple Contextual Gain
T1 是**离线可学习性诊断**，不把它与 T2 的在线 LinUCB baseline 混在一起。固定原则是：只用 context 特征与 train outcome 学每个 arm 的 expected reward/correctness，再在 held-out split 选 predicted-best arm；模型保持简单、不得为过 Gate 反复调大模型。当前实跑：BBB 用 Morgan + per-arm Logistic（另用 RF/ExtraTrees 作一次复核），BEIR lexical proxy 用 query TF-IDF + per-arm Ridge。T2 才正式使用 LinUCB/LinTS 等在线 baseline。

### Gate
至少 B1/B2/B3 三个 real families 中，2 个满足：
- Oracle > Best Fixed 有非平凡 gap；
- simple contextual method > Best Fixed。

**2026-09-20 第一轮状态：NOT PASSED。** 三类环境均有 Oracle headroom，但当前本地 BBB 与 BEIR lexical proxy 的 simple contextual selector 没有稳定超过 Best Fixed；LLMRouterBench 仅有 peer-reviewed published routing gain，本地 replay 尚未完成。Gate 不降级、不因结果修改；详见 `task/task26-AAMAS2027-T1-benchmark-characterization/report.md`。

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

# VI. T3 — Passive New-Arm Cold-Start Main Table

**这是整篇论文最重要的一张表。**

设置：
- 新工具在 t0 到达；
- D_new(t0)=∅；
- 不允许主动选择 context；
- x_t 按 benchmark test stream 自然到达；
- 每个 arm 都轮流当 new arm；
- 10 stream-order seeds。

方法：
Best Fixed / TS / LinUCB / LinTS / MTSS / UCB-AA / ContextRoute / Ours / Oracle。

主指标：
- NR_post@H ↓
- OGC@H ↑
- native quality ↑

H 固定为 benchmark test stream 的前 25% / 50% / 100% 三个相对 horizon，避免不同数据规模不可比。

表格主列只报 100% horizon；25/50% 放 appendix，F1 展示全过程。

**主 claim：Ours 在 new-arm arrival 后显著降低 post-arrival regret。**

---

# VII. F1 — Post-Arrival Regret Curves

每个 real benchmark family 一张曲线或同图不同 panel：

x-axis：arrival 后 round  
y-axis：mean cumulative regret / normalized regret

至少画：
- Best Fixed
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

B：
[
\{0,4,8,16,32\}.
]

Passive warmup：只从到来的自然 stream 中累计前 B 次新工具 feedback，不主动挑 x。

方法：
- LinUCB
- LinTS
- MTSS
- UCB-AA
- Ours

指标：
- fixed evaluation horizon 的 NR_post ↓
- OGC ↑

报告：
每个 B 的 mean ± 95% CI。

核心结论不是 B=32 最好，而是：
> 在相同目标 regret 下，Ours 需要更少 feedback。

---

# IX. T5 — Active Calibration Table

**只在允许 calibration pool 的 setting 做。**

Benchmark：
- Synthetic
- BEIR SciFact/NFCorpus
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
- post-calibration NR_post
- improvement per probe
- OGC

**这张表决定 active warmup 是否有资格成为主 contribution。**
若 information gain 不稳定优于 random，则从主方法中降级，不硬写创新。

---

# X. T6 — Transfer / Ablation Table

**目的：证明 method 的增益来自哪里。**

固定主 benchmark：
- LLMRouterBench
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

如果 “– Shared Prior” 几乎不掉，则 shared prior 不能作为 contribution。
如果 “– Context” 不掉，则问题退化成 non-contextual MAB。

---

# XI. T7 — OOD / Unseen Context Table

**目的：防止学成 tool ID / dataset ID 映射。**

LLMRouterBench：
- 使用其 ID/OOD setting；
- 额外按 dataset category leave-one-domain-out 汇总。

BBB：
- scaffold/OOD split；
- 不报 random split 作为主证据。

BEIR：
- SciFact ↔ NFCorpus 跨域迁移作为 robustness，不要求同一个绝对 reward scale。

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
Ours vs strongest non-oracle baseline。

额外显著性：
paired permutation / bootstrap difference，按 benchmark family 报，不对几十个小格逐一刷 p-value。

---

# XV. 实验完成判据

论文实验只有在下列全部完成后才算“做完”：

- [ ] T1 四个 benchmark block 全填，headroom / learnability gate 通过
- [ ] T2 stationary routing
- [ ] T3 passive new-arm 主表
- [ ] F1 regret curves
- [ ] T4 B=0/4/8/16/32 sample-efficiency
- [ ] T5 active calibration
- [ ] T6 ablation
- [ ] T7 OOD
- [ ] T8 real LLM post-train RL integration
- [ ] A1 cost-aware appendix
- [ ] 全部 held-out arms 轮换，不 cherry-pick
- [ ] 所有主数字有 95% CI
- [ ] 代码与 split 固定、结果可重复

ASlib/hERG/AMES 是 robustness bonus，不作为主论文完成的硬前置。

---

# XVI. 从表格到论文段落的一一对应

| 论文论断 | 必须由什么实验支撑 |
|---|---|
| 同功能工具存在 context-dependent complementarity | T1 |
| 现有 fixed/non-contextual routing 不够 | T2 + T3 |
| 新 arm cold-start 是真实痛点 | T3 + F1 |
| 方法更 sample-efficient | T4 |
| active probing 有价值 | T5 |
| shared prior 是必要机制 | T6 |
| 不是记忆 tool/dataset ID | T7 |
| 可以服务真正的 post-trained Agent | T8 |
| 部署成本下仍成立 | A1 |

当这张映射右侧全部有数字，Introduction 中的每一个 claim 都有对应实验证据，实验部分即闭环。
