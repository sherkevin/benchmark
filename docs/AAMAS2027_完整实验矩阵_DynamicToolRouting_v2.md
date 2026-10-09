# AAMAS 2027 完整实验矩阵 v2 — Safe / Sample-Efficient New-Tool Onboarding

> 2026-09-22：本矩阵保留为通用新工具接入方向的历史协议，不是当前医药具体模型选择的执行清单。当前对象、贡献与进度见 [论文主线与进度](AAMAS2027_论文主线与进度.md)，待办见 [任务清单](任务清单.md)。下文原状态和数值仅在原协议内解释，不删除或重包装旧结果。

> 日期：2026-09-21
> 状态：**Frozen Candidate v2**。v1 保留为 T1 qualification 前的预注册候选。
> 原则：benchmark qualification 已完成；后续不得因为方法结果不好临时换 benchmark、删 baseline 或改变主指标。
> 对应证据：`task/task26-AAMAS2027-T1-benchmark-characterization/report.md`。

## 0. 论文只测一个主能力

**Rapid Context-Dependent Onboarding of Newly Arriving Equivalent Providers**

上游 Agent 已经决定需要 capability f。该 capability 后面已有成熟 provider，新 provider T_new 在 t0 加入，但没有行为历史 D_new(t0)=∅。

我们测的是：系统能否在 sparse selected-arm feedback 下，用尽量少的探索损失快速学到新 provider 的 context-dependent competence，以及它相对现有 provider/policy 的 advantage。

**不测**：4 万工具里的 semantic retrieval、API 参数生成、多工具 workflow planning、多智能体协作。

### 0.1 两条必须同时满足的能力

1. **Contextual competence**：provider 的相对效果确实随 context 变化，不能只靠全局排名。
2. **New-tool adaptation**：新 provider 没历史时，系统能快速判断“什么时候值得用、什么时候应该回退 incumbent”。

### 0.2 Novelty guard

以下单独拿出来都不算新：contextual bandit routing、hierarchical/shared prior、warm-start contextual bandit、conservative contextual bandit、arriving arms。

候选创新必须落在这些约束的交叉处：**functionally-equivalent tool onboarding + zero behavioral history + context-dependent competence + limited feedback + incumbent-safe adaptation**。
# I. Benchmark 资格审查结果（T1 已完成）

| Benchmark | #contexts | #arms | reward/objective | Best Fixed | Oracle | Routing headroom | Simple held-out contextual | 角色 |
|---|---:|---:|---|---:|---:|---:|---:|---|
| **LLMRouterBench Performance** | **11,481** | 20 | per-query score ↑ | **0.6923** | **0.9186** | **+0.2263** | **+0.0220** macro | **主 benchmark** |
| **ASlib SAT12-ALL** | 1,614 | 31 | PAR10 ↓ | **3079.9** | **241.3** | 2838.6 PAR10 | Ridge **1486.9**；vs fixed ↓51.72% | **主 benchmark** |
| **BBB scientific tools** | 722 usable | 7 | correctness ↑ | **0.9155** | **0.9806** | **+0.0651** | **−0.0208** | **hard scientific stress test** |
| BEIR-SciFact dense | 300 | 3 | NDCG@10 ↑ | 0.7618 | 0.8012 | +0.0394 | **−0.00344** mean / 20 seeds | negative control / appendix |

T1 结论：LLMRouterBench 与 ASlib 同时满足“有 headroom + 简单 context model 可利用”，通过 qualification。BBB 有真实 headroom 但简单 selector 不过 Best Fixed，因此保留为 hard safe-onboarding case。BEIR-SciFact 降为 negative control。

### 1.1 Frozen authorities

LLMRouterBench：repo HEAD `c77cb05`；official bundle 1,283,503,080 bytes；SHA256 `b79f8cde1a6f029c2efa663a3a3b6f7748defb22341fe59f328cebef6648c8f1`；当前公开 bundle = 229,620 records = 11,481 prompts × 20 models。

BBB：T1 用 722 usable truth-connected molecules；T3/T4 all-arm replay 只用 652 complete dense panel，避免缺失 arm output 带来 availability bias；主 split 为 Bemis–Murcko scaffold OOD。
# II. 一个关键 protocol 修正：不能把新 arm 直接塞进饱和 full pool

T1 后进一步做了 arrival-value audit。结果显示 provider pool 越大，新 arm 对 incumbent oracle 的独特边际价值越小。

### LLMRouterBench
| incumbent K0 | new arm 平均 unique improvement rate |
|---:|---:|
| 2 | **7.51%** |
| 4 | **3.03%** |
| 8 | **0.93%** |
| 16 | **0.27%** |
| 19（full pool） | ~**0.18–0.19%** |

### ASlib SAT12-ALL
| incumbent K0 | new solver 严格改善 context 比例 |
|---:|---:|
| 2 | **23.64%** |
| 4 | **15.85%** |
| 8 | **9.38%** |
| 16 | **5.09%** |
| 30（full pool） | ~**2.9%** |

### BBB dense panel
| incumbent K0 | new tool 平均 unique improvement rate |
|---:|---:|
| 2 | **1.53%** |
| 4 | **0.44%** |
| 6（full pool） | **0.24%** |

full-pool leave-one-arm-out 会发生结构性饱和：已有 provider 越多，新 provider 对 incumbent oracle 的独特贡献越小。此时实验主要测 redundancy，而不是 onboarding ability。
# III. Main onboarding protocol

## 3.1 Provider pool size

主表固定：**K0=4 incumbents + 1 new provider**。

这不是挑最好看的结果：same-function provider 文献本来就是小 K pool；K0=4 已有非平凡 incumbent competition；qualification audit 显示 LLMRouterBench 与 ASlib 仍有可测 arrival opportunity；full-pool saturation 另做敏感性。

敏感性必须报告：
- LLMRouterBench / ASlib：K0 ∈ {2,4,8,full}
- BBB：K0 ∈ {2,4,6}

## 3.2 Scenario sampling

对每个 candidate new arm：
1. 从其余 arms 中随机采 incumbent subset；
2. 主表每个 new arm × 至少 10 个 incumbent-subset seeds；
3. 每个 subset × 10 个 post-arrival stream seeds；
4. 不允许只挑新 arm 很强的 scenario；
5. 主表同时报告所有 arrivals；按 arrival-value 分层只作 analysis。

## 3.3 History visibility

- Incumbent arms：拥有 pre-arrival historical outcomes，可视为成熟 provider。
- New arm：所有 pre-arrival outcomes 完全隐藏。
- Post-arrival：learner 只看到被选择 provider 的 reward。
- Offline evaluator 可以看到 dense counterfactual matrix，仅用于计算 regret / safety 指标。

## 3.4 Side information

主协议不给 new arm 任何能直接泄露 competence 的 tool-side metadata。B=0 时 specialization 本来就不可识别；这是一条信息边界，不要求模型“零样本猜中”。metadata-aware warm start 只作 appendix。
# IV. Baseline 固定

| ID | Baseline | 目的 |
|---|---|---|
| BL0 | **Best Fixed incumbent** | 最简单成熟生产策略 |
| BL1 | **Thompson Sampling** | 非 contextual online learner |
| BL2 | **LinUCB** | 经典 contextual optimistic learner |
| BL3 | **LinTS** | 经典 Bayesian contextual learner |
| BL4 | **MTSS** | hierarchical/meta transfer；检验 shared prior 是否已足够 |
| BL5 | **UCB-AA / arriving-arm baseline** | 只解决 arriving arms |
| BL6 | **CLUCB / conservative LinUCB** | 以 incumbent baseline 为安全约束，检验已有 safe exploration |
| BL7 | **ContextRoute / LQM quality head** | 最近 same-function provider routing；在接口自然的 benchmark 上跑 |
| UB | **Oracle** | upper bound，不是可部署方法 |

Random 只作 sanity lower bound，不占正文主表。

## 4.1 Matched incumbent-only control

对每个 online method M，都额外跑同 seed、同 history 的 `M(incumbent set only)`。这样可以计算：

**DeltaU_new = U(with-new) − U(incumbent-only)**。

这项把“旧 pool router 本来就强/弱”和“新工具真的带来了什么”分开。
# V. 统一指标

## M1. Post-arrival cumulative regret

`R_post(H) = Σ [oracle reward over incumbent+new − chosen reward]`。Lower is better。报告 H ∈ {32, 25%, 50%, 100%}。

## M2. Incremental utility over matched incumbent-only policy

`DeltaU_new = U_with-new − U_incumbent-only`。

- >0：接入新工具产生净收益；
- <0：探索或错误采用新工具反而伤害系统。

## M3. Beneficial adoption precision

离线 dense matrix 可计算：`P(new tool better than matched incumbent action | choose new tool)`。

同时报告 harmful adoption rate、new-arm selection rate、mean loss when harmful adoption occurs。

## M4. Sample efficiency

`B ∈ {0,1,4,8,16,32}`。加入 B=1，因为无 metadata 时 B=0 specialization 不可识别。

报告固定 horizon 下的 post-arrival regret、DeltaU_new、beneficial adoption precision。

## M5. Native quality

- LLMRouterBench：macro AvgScore / Accuracy；
- ASlib：PAR10；
- BBB：Accuracy + Balanced Accuracy。
# VI. 完整实验矩阵 v2

| ID | Mandatory | 实验 | 唯一回答的问题 |
|---|---:|---|---|
| **T1** | ✓ 已完成 | Benchmark qualification | 问题在哪些环境中真实存在？ |
| **T2** | ✓ | Stationary incumbent routing | 没有 new arm 时，各 router 的成熟能力是多少？ |
| **T3** | ✓ | **K0=4 Passive New-Arm Onboarding Main Table** | 零历史新工具加入后，谁能最快获得净增益且少伤 incumbent？ |
| **F1** | ✓ | Post-arrival regret / uplift curves | cold-start spike 与 recovery 长什么样？ |
| **T3b** | ✓ | Pool-size sensitivity | 工具池变大时，新工具边际价值与探索难度如何变化？ |
| **T4** | ✓ | B=0/1/4/8/16/32 sample efficiency | 要多少 feedback 才能认识新工具？ |
| **T5** | ✓ | Active calibration | 允许 calibration pool 时，主动 probe 是否优于随机？ |
| **T6** | ✓ | Transfer + safety ablation | shared transfer / conservative gate / online update 各贡献多少？ |
| **T7** | ✓ | OOD | 是否只是记住 context/tool identity？ |
| **T8** | ✓ | Post-trained LLM Agent integration | 插到真正 Agent 上是否仍有 end-to-end value？ |
| A1 | appendix | Cost / latency | 加 deployment cost 后是否成立？ |
| A2 | appendix | BEIR negative control | 当 provider 几乎被全局强 arm 支配时，方法是否保持克制？ |
| A3 | optional | hERG / AMES | scientific endpoint robustness |

## T2
LLMRouterBench / ASlib / BBB 上先跑 Best Fixed / TS / LinUCB / LinTS / ContextRoute（适用时），得到 mature incumbent 正常水平。

## T3 主表
K0=4 incumbents + 1 new arm；new arm 无 pre-arrival history；contexts 自然到达；只反馈 selected arm；所有 new arms 与 subset seeds 轮换。

Rows：Best Fixed / TS / LinUCB / LinTS / MTSS / UCB-AA / CLUCB / ContextRoute / Ours / Oracle。

Columns：Native Quality、R_post@32、R_post@100%、DeltaU_new、New-arm Selection Rate、Beneficial Adoption Precision、Harmful Adoption Rate。
# VII. 其余 Mandatory 实验

## F1 — Curves
每个 positive benchmark 画 cumulative regret 与 cumulative incremental utility vs incumbent-only。主画 LinUCB / LinTS / MTSS / UCB-AA / CLUCB / Ours。

## T3b — Pool-size sensitivity
比较 K0 增大时 incumbent-only quality、with-new DeltaU、new-arm pulls、regret。目标是验证大而冗余的 pool 中方法是否自动变保守，而不是浪费探索预算。

## T4 — Feedback budget
Passive setting 严格使用 B ∈ {0,1,4,8,16,32}；前 B 次反馈来自自然 stream 中算法真实探索，不能主动挑 context。

## T5 — Active calibration
主 benchmark：Synthetic / ASlib / BBB。Probe policies：Random、Diversity/k-center、Uncertainty、Information Gain（若最终方法采用）。所有 probe 策略之后必须接同一个 online router。

## T6 — Ablation / safety
固定 K0=4, B=8。至少：Full、−Transfer/shared prior、−Context、−Online update、−Conservative fallback、Random probe、Independent-arm initialization。

## T7 — OOD
LLMRouterBench：official/category OOD；BBB：Bemis–Murcko scaffold OOD；ASlib 继续官方 CV，只有存在预先定义 scenario metadata 时才加 family OOD。

## T8 — LLM Agent integration
Main LLM 真正参数更新：Base → SFT → post-train RL。LLM 只负责 capability call / verify / answer / abstain / stop，provider identity 仍由 Tool Manager 管。

至少比较：Base+incumbent；SFT+incumbent；SFT+RL+incumbent；SFT+RL+Ours；Oracle router（可选 upper bound）。
# VIII. Full-pool pilot 的解释边界

full-pool held-out-arm pilot 已跑出：
- LLMRouterBench：new arm 对 19-arm incumbent oracle 的 unique value 约 **0.19%**；
- ASlib：约 **2.9% contexts**；
- BBB：约 **0.24%**。

对应地，UCB/LinUCB 会为了高不确定度大量探索 new arm，却通常没有足够 unique benefit，甚至低于不探索的 Best Fixed。

**这不是 bandit 不工作，而是 full-pool protocol 在测饱和 redundancy。**

full-pool pilot 只作 T3b saturation evidence，不作为最终主结果。

# IX. 实验完成判据

- [x] T1 qualification
- [ ] T2 stationary incumbent
- [ ] T3 K0=4 passive onboarding 主表
- [ ] F1 curves
- [ ] T3b pool-size sensitivity
- [ ] T4 B=0/1/4/8/16/32
- [ ] T5 active calibration
- [ ] T6 ablation/safety
- [ ] T7 OOD
- [ ] T8 post-trained Agent
- [ ] A1 cost-aware
- [ ] 所有 new arms 轮换，不 cherry-pick
- [ ] 所有主结果 paired seeds + 95% CI
- [ ] split、scenario seeds、reward transformation 全冻结
- [ ] 每个数字都可由脚本从 frozen data 重算

当这些格子填完，Introduction 中每个 claim 都必须能指向唯一一张表/图。