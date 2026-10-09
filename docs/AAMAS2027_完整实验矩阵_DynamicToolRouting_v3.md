# AAMAS 2027 完整实验矩阵 v3 — Safe Incremental Tool Onboarding

> 2026-09-22：本矩阵保留为通用新工具接入方向的历史协议，不是当前医药具体模型选择的执行清单。当前对象、贡献与进度见 [论文主线与进度](AAMAS2027_论文主线与进度.md)，待办见 [任务清单](任务清单.md)。下文原状态和数值仅在原协议内解释，不删除或重包装旧结果。

> 日期：2026-09-21  
> 状态：Working Frozen Candidate v3；v1/v2 保留审计。  
> 触发原因：T3 full-pool pilot 发现“把新 arm 加入 19/30 个 incumbents 后重新学习整个 K-arm routing”严重稀释 onboarding 信号，且测到的主要是 incumbent routing error，不是 new-tool adaptation。

## 0. 最终主能力进一步收敛

**Safe Incremental Tool Onboarding under Sparse Contextual Feedback**

系统已有成熟 incumbent router：

[
pi_0(x)
ightarrow T_{
m incumbent}.
]

新工具 (T_n) 在 (t_0) 到达，没有行为历史。本文不重新学习整个旧工具池，而只学习：

[
g_n(x)=mu_n(x)-mu_{pi_0}(x),
]

即“当前 context 是否值得从成熟系统切到新工具”。

动作收敛为：

[
a_tin{	ext{fallback to }pi_0, T_n}.
]

Gate Oracle：

[
a_t^*=argmax{mu_{pi_0}(x_t),mu_n(x_t)}.
]

这与部署语义一致：新增工具不应让已稳定运行的旧 router 被迫重新探索。

## 1. 与已有文献的边界

不能声称“首次 safety against baseline”：
- NeurIPS 2017 CLUCB 已研究 Conservative Contextual Linear Bandits；
- AAAI 2020 CLUCB2 改进 conservative exploration；
- ICLR 2025 C-SquareCB / C-FastCB 已把 conservative contextual bandit 扩到 non-linear regression oracle。

不能声称“首次 cold-start/warm-start”：
- 2023 warm-start contextual bandits 已系统研究 LinTS/LinUCB prior warm start；
- AISTATS 2023 MTSS 已研究 hierarchical sharing 与 frequently added new items；
- 2026 UCB-AA 直接研究 arriving arms。

本文剩余的候选技术缺口是这些条件的交叉：
1. **dynamic new tool arrives after a mature baseline policy already exists**；
2. new tool **没有可靠 tool-side competence features**；
3. competence 是 **context-dependent**；
4. 只能通过少量 **behavioral probes / bandit feedback**识别；
5. onboarding 期间要限制相对 incumbent 的 harmful regression；
6. optional active calibration 要在有限 B 下选最有信息量且风险可控的 contexts。

因此“shared prior”“conservative bandit”“arriving arm”任何一个单独都不是创新。

## 2. Capability pool size 固定

function-key 分组后的 same-function pool 才是实验单位，不把母池全部 arms 默认塞进同一 capability。

主 pool size：
- LLMRouterBench：K={3,5,9}
- ASlib：K={3,5,9}
- BBB：K={3,5,7}

Full pool 仅作 saturation appendix。

采样纪律：
- 每个 split × K 下，每个 arm 恰好一次作为 new arm；
- 其余 K-1 incumbents 从剩余 arms 均匀无放回采样；
- random seed 冻结；
- 不按结果挑 pool。

结构实测说明这一选择不是为了“挑好看结果”：
- LLMRouterBench new-arm marginal oracle value：K3 0.0751 / K5 0.0297 / K9 0.00909 / K20 0.00152；
- ASlib：K3 0.0874 / K5 0.0379 / K9 0.0137 / K31 0.00282；
- BBB：K3 0.0150 / K5 0.00424 / K7 0.00241。

这反映真实 saturation：incumbent 越多，一个新 provider 的独占增量自然越小。

## 3. Mature incumbent router

每个 episode 先用 history 数据训练固定 (pi_0)。

主实现：
- 每 arm 一个简单 reward model；
- history-only fit；
- test 时 (pi_0(x)) 只在 incumbents 中选 predicted-best；
- onboarding methods **不允许改写 incumbent router**，只决定 fallback 或 new tool。

这使实验只测 onboarding，而不是把 incumbent routing 能力混进结果。

结构诊断（history Ridge incumbent）：

| Benchmark | K | deployed incumbent reward | gate-oracle lift |
|---|---:|---:|---:|
| LLMRouterBench | 3 | 0.5829 | **+0.1126** |
| LLMRouterBench | 5 | 0.6257 | **+0.1047** |
| LLMRouterBench | 9 | 0.6840 | +0.0676 |
| ASlib | 3 | 0.5755 | **+0.1127** |
| ASlib | 5 | 0.7077 | **+0.0506** |
| ASlib | 9 | 0.7595 | +0.0389 |

Gate lift：

[
mathbb E[max(r_{pi_0},r_n)-r_{pi_0}]
]

是真正可被 onboarding 模块获得的上界。

## 4. T3 主 baseline 重新冻结

### Deployable / trivial
1. **Never-New**：永远 fallback 到 incumbent router。
2. **Uniform Probe-then-Commit**：前 B 次随机/均匀 probe new，随后 plug-in exploitation。
3. **epsilon-Greedy Gate**。

### Classic contextual cold-start
4. **Cold-start LinUCB Gate**。
5. **Cold-start LinTS Gate**。
6. **Action-Centered Thompson Sampling (NeurIPS 2017)**：直接学习相对 baseline 的 treatment / advantage effect，是当前 formulation 的关键近邻。
7. **Warm-start LinTS / LinUCB (2023 framework)**。

### Transfer / dynamic-arm
8. **MTSS-style hierarchical prior**。
9. **UCB-AA**（concurrent 2026；标注 non-contextual arriving-arm baseline）。

### Safety / production-policy baseline
10. **CLUCB / CLUCB2**。
11. **SEA (Safe Exploration Algorithm, TOIS 2020)**：production baseline + high-confidence off-policy evaluation 后才切换。
12. **C-SquareCB / C-FastCB**（若 implementation 成本可控，至少选一个 non-linear conservative baseline）。

### Upper bounds
- Gate Oracle
- Incumbent Oracle（diagnostic，不可部署）

ContextRoute/LQM 不再作为 new-tool cold-start baseline；它更适合用来构造/替换 mature incumbent router 或 fixed-pool comparison。

## 5. T3 主指标

### M1 Net Onboarding Gain
[
G_{
m onboard}=mathbb E[r_{
m method}-r_{pi_0}].
]

正值表示加入新工具后**净提升**，不会被 incumbent routing error 污染。

### M2 Gate Regret
[
R_T^{gate}=sum_t left(max(r_{pi_0,t},r_{n,t})-r_{a_t,t}
ight).
]

这是第一主指标。

### M3 Opportunity Capture
[
OC=rac{U_{
m method}-U_{pi_0}}
{U_{
m gate oracle}-U_{pi_0}}.
]

### M4 Harmful Switch Rate
在 method 选择 new tool 的 rounds 中：

[
HSR=P(r_n<r_{pi_0}mid a_t=n).
]

### M5 Beneficial Switch Precision / Recall
- precision：选 new 时真正优于 incumbent 的比例；
- recall：所有 new-tool 可救 context 中，被方法抓到多少。

这两个指标直接对应 BBB 已发现的 rare-rescue asymmetric switching。

## 6. T4 few-history

Behavioral feedback：

[
Bin{1,4,8,16,32}.
]

每个 B 后冻结当前 new-tool posterior，测后续相同 test horizon：
- Gate Regret
- Net Onboarding Gain
- Opportunity Capture
- HSR
- beneficial-switch precision/recall

B=0 只报 prior behavior，不声称识别能力。

核心问题：
> 用多少次真实反馈，才能安全定位“新工具在哪些 context 上值得切过去”？

## 7. T5 active calibration

只允许从预先存在的 calibration pool 选择 probe context。

Baselines：
- Random
- Diversity / k-center
- uncertainty
- information gain
- safety-aware information gain（若 Ours 采用）

目标不是最大化 calibration reward，而是：
[
	ext{information gained per unit safety budget}.
]

Active setting 与 passive user-query stream 分开报告。

## 8. T6 safety / transfer ablation

- no transfer
- no safety constraint
- no active selection
- no online update
- misspecified prior
- noisy/delayed feedback
- incumbent router strong vs weak

特别要求：
**Ours 若只在 incumbent 很弱时有效，不足以支撑“安全 onboarding”主张。**

## 9. Benchmark 角色

### Main positive
- LLMRouterBench — modern provider routing
- ASlib SAT12-ALL — canonical algorithm-selection methodology

### Hard application
- BBB scientific tools — strong incumbent / rare rescue / scaffold-OOD

### Negative control
- BEIR SciFact dense pool — same-function 但 specialization 弱；正确算法应学会少切甚至不切。

### Synthetic
只为 theory 与 controlled misspecification / safety-budget sensitivity。

## 10. 实验完成矩阵

- [x] T1 benchmark qualification
- [x] protocol dilution diagnosis
- [ ] T2 mature incumbent stationary quality
- [ ] T3 safe onboarding main table
- [ ] F1 Gate regret curves
- [ ] T4 B={1,4,8,16,32}
- [ ] T5 active calibration
- [ ] T6 safety/transfer ablations
- [ ] T7 OOD / prior misspecification
- [ ] T8 post-trained LLM Agent integration
- [ ] A1 full-pool saturation
- [ ] A2 cost/latency
- [ ] 95% CI + all new arms rotated + frozen seeds

当以上表格填满，Introduction 中每个主张必须能指向唯一一张表/图。
