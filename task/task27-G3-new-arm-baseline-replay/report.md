# task27 report — G3 Baseline-only New-Arm Cold-Start Pilot

> 日期：2026-09-21  
> 状态：第一轮 baseline-only phenomenon gate 已完成；formal T3 尚未开始。  
> 原则：Ours 尚未实现，本轮只测问题是否真实可测。

## 1. 本轮做了什么

统一实现一个 zero-history new-arm simulator：

- incumbent arms：拥有 history outcomes；
- new arm：history 完全隐藏；
- post-arrival 每轮只 reveal 被选择 arm 的 reward；
- all arms 轮流作为 new arm；
- context 表示只在 history/train 上 fit。

Pilot baseline：
- Best Fixed
- UCB1
- Beta-TS
- LinUCB
- LinTS

产物：
- `src/probe_headless/coldstart_core.py`
- `src/probe_headless/g3_llmrouterbench_baseline_pilot.py`
- `src/probe_headless/g3_aslib_baseline_pilot.py`
- `src/probe_headless/g3_bbb_baseline_pilot.py`
- `data/tool_results/divergence/g3_*_baseline_pilot.json`

## 2. 第一轮结果

### 2.1 LLMRouterBench

5 split/stream seeds × 20 held-out new models = 100 runs。

| Method | Avg reward ↑ | Mean regret/round ↓ | First-32 regret ↓ | New-arm selection |
|---|---:|---:|---:|---:|
| Best Fixed | 0.6720 | 0.2310 | 7.80 | 0% |
| UCB1 | 0.6507 | 0.2523 | 12.57 | 28.39% |
| Beta-TS | 0.6704 | 0.2327 | 8.98 | 6.41% |
| LinUCB | **0.6780** | **0.2250** | 12.34 | 30.43% |
| LinTS | 0.6622 | 0.2408 | 9.86 | 20.75% |

cold-start 轨迹：
- UCB1 new-arm use：前 25% **55.0%** → 后 25% **14.7%**
- LinUCB：**51.5% → 19.2%**
- LinTS：**28.2% → 16.1%**
- Beta-TS：**9.0% → 5.2%**

结论：新 arm 最大的不确定性会在早期触发明显 exploration spike，随后反馈才逐渐把使用率压下来。

### 2.2 ASlib SAT12-ALL

官方 10 folds × 3 stream seeds × 31 held-out solvers = 930 runs。

| Method | Avg utility ↑ | Mean regret/round ↓ | First-32 regret ↓ | New-arm selection |
|---|---:|---:|---:|---:|
| Best Fixed | **0.7426** | **0.2373** | **7.32** | 0% |
| UCB1 | 0.5504 | 0.4294 | 16.81 | 74.90% |
| Beta-TS | 0.7245 | 0.2554 | 8.66 | 8.94% |
| LinUCB | 0.7231 | 0.2568 | 12.69 | 53.40% |
| LinTS | 0.7321 | 0.2478 | 8.97 | 24.43% |

new-arm use 的 early → late：
- UCB1：**100% → 42.9%**
- LinUCB：**76.6% → 39.6%**
- LinTS：**29.8% → 20.7%**
- Beta-TS：**15.3% → 5.7%**

结论：“已有 arms 历史很多、新 arm 历史为零”的 arrival information discrepancy 非常明显。普通 optimism 会因为新 arm uncertainty 最大而投入大量早期 pulls。

### 2.3 BBB complete dense panel

为避免缺失输出干扰，本 pilot 用 652 molecules × 7 families 全密集面板；5 scaffold folds × 10 stream seeds × 7 held-out tools = 350 runs。

| Method | Accuracy ↑ | Mean regret/round ↓ | First-32 regret ↓ | New-arm selection |
|---|---:|---:|---:|---:|
| Best Fixed | **0.9277** | **0.0508** | **1.47** | 0% |
| UCB1 | 0.8970 | 0.0815 | 2.45 | **100%** |
| Beta-TS | 0.9178 | 0.0607 | 1.77 | 18.93% |
| LinUCB | 0.9002 | 0.0783 | 2.43 | 93.30% |
| LinTS | 0.9026 | 0.0759 | 2.30 | 32.65% |

LinUCB 的 new-arm use 前 25% 98.9% → 后 25% 86.5%；UCB1 在这个约 130-round 的短 horizon 内甚至始终 100%。

这不是在证明 UCB/LinUCB “差”，而是在暴露结构：incumbent history 很充足，而新工具完全无历史时，经典 uncertainty bonus 会把“未知”直接当成“值得大量探索”。

## 3. G3 phenomenon gate：通过

预先写下的四条判据：

1. arrival 会不会产生 early regret？——会，而且三个环境都清楚可见。
2. baseline adaptation curve 是否可区分？——可以。
3. new arm 是否既有“值得采用”又有“应拒绝”的 case？——有。BBB 7 个工具中 3 个在完整面板对 full oracle 没有独特增量；其余环境各 arm 的 marginal value 也高度不均。
4. feedback 增加后使用策略是否变化？——LLMRouterBench / ASlib 上 UCB、LinUCB、TS 的 new-arm use 均明显随时间下降。

所以 G3 作为“问题是否真实可测”的 Gate 已通过。

但这些数字不能直接成为论文 T3 主表，原因见下一节。

## 4. 必须加 matched incumbent-only counterfactual

当前比较：

`LinUCB + new arm` vs `Best Fixed`

会混入两个因素：
1. LinUCB 本来就能在旧 arms 之间 contextual route；
2. new arm onboarding 带来的额外收益/探索损失。

因此 formal T3 必须对每个 online method 再跑一个完全匹配的 incumbent-only counterfactual，使用相同 history、context、stream、hyperparameter、random seed，唯一差别是不开放 new arm。

真正的 onboarding 指标改为：

`Delta U_onboard = U(method with new arm) - U(matched incumbent-only method)`

这样才能回答：“让这个算法看见一个新工具，到底增加了价值还是制造了探索损失？”

这一条已经写入实验矩阵 v2；正式 T3 前必须完成。

## 5. 更关键的新发现：工具越多，单个新工具的边际价值越稀疏

另外做了一个零模型、纯 counterfactual 分析：对每个 new arm，随机采样不同大小的 incumbent pool，计算“加入这个新 arm 后 full oracle 能增加多少”。

产物：
- `src/probe_headless/g3_pool_size_marginal_value.py`
- `data/tool_results/divergence/g3_pool_size_marginal_value.json`

### LLMRouterBench

| incumbent 数 | 新 arm 平均 marginal gain |
|---:|---:|
| 1 | **0.1724** |
| 2 | 0.0819 |
| 4 | 0.0330 |
| 8 | 0.00978 |
| 19 | **0.00183** |

### BBB

| incumbent 数 | 新 arm 平均 marginal gain |
|---:|---:|
| 1 | **0.0501** |
| 2 | 0.0153 |
| 4 | 0.00439 |
| 6 | **0.00241** |

### ASlib

| incumbent 数 | 新 arm mean marginal gain | 新 arm uniquely useful context |
|---:|---:|---:|
| 1 | **0.1665** | 31.25% |
| 2 | 0.0873 | 23.64% |
| 4 | 0.0353 | 15.56% |
| 8 | 0.0133 | 9.41% |
| 16 | 0.00470 | 5.17% |
| 30 | **0.00219** | 2.91% |

这是目前最重要的经验现象：

> 工具生态越成熟/越拥挤，一个新工具相对于已有工具集合的“独特边际价值”会快速变稀疏；但它刚加入时 epistemic uncertainty 却最大。

于是经典 optimism 出现一个自然矛盾：

`uncertainty(new) 很高，但 P(new is incrementally useful) 随 incumbent pool 增大而下降。`

这解释了为什么 UCB/LinUCB 在 pilot 中会严重 over-explore arriving tools。

## 6. 问题应进一步收紧：selective onboarding / contextual screening

更准确的问题不再是：

> 如何尽快学会一个新工具的绝对能力函数？

而应是：

> **在已有成熟 incumbent router 的情况下，如何用极少的行为反馈，判断一个新工具是否值得纳入，以及它只在哪些 context 上提供增量价值，同时避免把大量在线流量浪费在最终会被拒绝的 arriving tools 上？**

定义 incumbent value：

`b(x) = max_i mu_i(x), i in incumbents`

新工具真正需要学的是 residual advantage：

`delta_new(x) = mu_new(x) - b(x)`

目标不是精确重建整个 `mu_new(x)`，而是尽快识别：

`S_new+ = {x : delta_new(x) > 0}`

这与真实 deployment 更贴近：Agent 已经有一套可用工具，新增 provider 是 challenger；大多数 challenger 可能冗余，少数只在局部 context 有价值。

## 7. 与最近工作的边界

- **PAK-UCB (ICML 2025)**：fixed provider pool 下的 prompt-aware contextual model selection；没有 zero-history arrival/onboarding。
- **FC-Entry (ICML 2024)**：peer-reviewed entering-arm bandit，但不是“已有 contextual incumbent router + 学局部 residual advantage”的 setting。
- **UCB-AA (2026 concurrent)**：已经明确提出 arrival information discrepancy，并通过 preliminary screening 减少 arriving-arm wasted pulls；这是必须正面比较的最近问题。
- **P-LinUCB/P-TS (AISTATS 2024)**：从历史 contextual bandit tasks 学共享低维结构，检验 transfer 是否已经足够。
- **ContextRoute/LQM (2026 concurrent)**：fixed same-function provider routing，不解决 zero-history challenger onboarding。

因此不能声称发现了“arriving arms 需要 screening”；更有区分度的候选技术增量是：

> **context-dependent challenger screening + cross-tool/task representation transfer + very small behavioral onboarding budget**。

## 8. 下一步

formal T3 之前只做三件事：

1. 实现并跑完 matched incumbent-only counterfactual，得到纯 onboarding delta；
2. 复现 / 适配三个 peer-reviewed baseline：P-LinUCB/P-TS、PAK-UCB、FC-Entry；UCB-AA/ContextRoute 作为 concurrent nearest work；
3. 基于 residual advantage `delta_new(x)` 写清数学 formulation，再决定 Ours 的算法结构。

在这三件事之前，不开始 LLM post-train RL，也不设计复杂 neural router。

## 9. 相关工作再校准：不能把“安全筛新工具”本身当创新

G3 pilot 后继续查近邻，发现三条必须纳入正式 baseline / related-work 的 peer-reviewed 线：

1. **PAK-UCB / RFF-UCB — ICML 2025**  
   直接研究 prompt-aware selection of generative models / LLMs，用 contextual bandit 根据 prompt 在线学习不同模型的得分函数。它覆盖“固定 provider pool 下 context-dependent routing”，因此我们的差异必须来自 **new-arm zero-history onboarding**，不能来自“prompt-aware model selection”。

2. **FC-Entry — ICML 2024, On Multi-Armed Bandit with Impatient Arms**  
   论文扩展允许新 arms 在游戏开始后进入，并给 performance guarantee。虽然其主问题还包含 impatient-arm/exit 机制，但它是 peer-reviewed 的 arm-entry comparator，正式 T3 至少需要讨论/适配。

3. **Conservative Bandits / CLUCB — ICML 2016 / NeurIPS 2017**  
   已经研究“已有 production baseline policy，探索新策略时不能让累计表现明显低于 baseline”。因此如果我们把“不要伤害 incumbent router”写成显式 safety constraint，CLUCB 必须成为 baseline，且这个安全约束本身不能当 novelty。

另：**UCB-AA (2026 concurrent)** 已经把 arriving-arm 的核心困难命名为 arrival information discrepancy，并用 preliminary screening 避免每个新 arm 都进入 full competition。它与本轮实测的 UCB/LinUCB over-exploration 完全同向，所以我们不能声称首次发现该现象。

由此候选新问题的区分点进一步收窄到：

> **context-dependent challenger screening**：新工具整体可能冗余，但在稀疏 context 子集上有正 residual advantage；如何借 incumbent tools 学到的 task representation，在极少 behavioral feedback 下识别这些局部正增益区域，而不是用全局均值把它整体淘汰、也不是因高 uncertainty 过度探索。

正式 baseline 分层因此建议：
- classic：Best Fixed / UCB1 or TS / LinUCB / LinTS；
- fixed-pool contextual provider：PAK-UCB；
- cross-task transfer：P-LinUCB/P-TS；
- arm entry：FC-Entry；
- baseline-safe exploration：CLUCB（仅当最终 formulation 加 safety constraint 时 mandatory）；
- concurrent nearest：UCB-AA、ContextRoute；
- Oracle / Incumbent Oracle。

## 10. Challenger framing 的 opportunity 不是“太小”：相对 realistic incumbent 明显更大

上一节的“new arm marginal value 随 pool 增大变稀疏”是相对于 **incumbent oracle ensemble** 的最严格口径；它回答“这个新工具有没有别人完全替代不了的独特价值”。

但真实部署并没有 incumbent oracle，而是一个已有 production router。为了判断 challenger formulation 是否还有足够实验信号，本轮又用最保守、可复现的 **train-selected Best Fixed incumbent** 做一次零模型 counterfactual：

[
	ext{opportunity}(x)=max(r_{new}(x)-r_{inc}(x),0)
]

结果：

| Benchmark | new tool 相对 incumbent 的平均 incremental gain | new tool 更好的 context 比例 |
|---|---:|---:|
| LLMRouterBench | **+0.0691** | **6.91%** |
| ASlib SAT12-ALL | **+0.0369 utility** | **26.15%** |
| BBB dense | **+0.0392 accuracy** | **3.92%** |

产物：
- `src/probe_headless/g3_challenger_opportunity.py`
- `data/tool_results/divergence/g3_challenger_opportunity.json`

所以两件事同时成立：

1. 相对于“所有 incumbent 取逐样本 Oracle”，单个新工具的**不可替代独特价值**在大 pool 中确实很稀疏；
2. 相对于现实 production incumbent，新工具仍有 **4–7pp 量级甚至更大的局部提升空间**。

这正好说明论文不应该用 full tool oracle 作为唯一 onboarding benchmark。formal T3 应同时报告：

- **Challenger Oracle**：`max(incumbent-policy reward, new-tool reward)`，作为 onboarding 的主 oracle；
- **Full Tool Oracle**：所有旧工具+新工具逐样本最优，只作为 end-to-end upper bound。

主 onboarding regret 应相对 Challenger Oracle 定义，这样不会把 incumbent router 自己原本的错误混进“认识新工具”的能力。

更自然的实验链因此是：

1. **T2** 先训练/选择一个 frozen incumbent router；
2. **T3** 把某个 held-out tool 在 t0 作为 challenger 加入；
3. onboarding manager 每个 context 只决定：
   - delegate 给 frozen incumbent router；
   - 或尝试/使用 new challenger；
4. 评估是否用少量 feedback 接近 Challenger Oracle。

这使 T2 → T3 不再是两张互不相干的表，而形成完整实验流水线。

## 11. Matched incumbent-only counterfactual 已真正重跑完成

task27 §4 要求的 matched counterfactual 已不是“待做”：三套 pilot 脚本已更新并全量重跑。每个方法都使用相同 split / stream / seed，分别比较“可访问 challenger”与“完全不开放 challenger”的同方法运行。

平均 `Delta U_onboard = U(with challenger) - U(matched incumbent-only)`：

| Benchmark | UCB1 | Beta-TS | LinUCB | LinTS |
|---|---:|---:|---:|---:|
| LLMRouterBench | **-0.02141** | **-0.00164** | **-0.02257** | **-0.02277** |
| ASlib SAT12-ALL | **-0.19213** | **-0.01550** | **-0.12457** | **-0.06633** |
| BBB dense | **-0.02559** | **-0.00335** | **-0.01669** | **-0.00180** |

这使 G3 的解释更干净：当前 classic baselines 从新工具访问权本身获得的平均净价值都是负的，尤其 optimism 型 UCB/LinUCB 受新 arm 零历史高 uncertainty 驱动，明显 over-explore。Beta-TS 在三套环境中伤害最小，但也没有形成正的平均 onboarding utility。

注意：这仍不是 formal T3，因为这些方法同时在旧 arms 之间继续在线 route。v3 已进一步要求 T2 先冻结一个 incumbent router，正式 T3 只学二元 `incumbent vs challenger`。本轮 matched counterfactual 的价值是证明：**把 incumbent 自身 routing 能力抵消后，新工具 cold-start 的独立探索损失仍真实存在。**

重跑产物仍为：
- `data/tool_results/divergence/g3_llmrouterbench_baseline_pilot.json`
- `data/tool_results/divergence/g3_aslib_sat12all_baseline_pilot.json`
- `data/tool_results/divergence/g3_bbb_baseline_pilot.json`
## 12. 再校准：residual advantage / baseline-relative modeling 也不是新概念

继续查 `delta_new(x)=mu_new(x)-b(x)` 的最近祖先后，必须加入一个非常重要的 peer-reviewed baseline：

**Action Centered Contextual Bandits — NeurIPS 2017, Greenewald et al.**

它明确把 reward 分成复杂的 baseline reward 与相对 baseline 的 treatment effect，并指出最优动作只依赖 differential/treatment effect；算法通过 action-centering 隔离并学习相对 baseline 的增量，而不要求把 baseline reward 本身建模得很简单。

这和我们刚收敛出的“不要重建新工具完整绝对能力，而学习 challenger 相对 incumbent 的 residual advantage”在建模思想上高度接近。因此：

- **residual / baseline-relative modeling 本身不能作为 novelty；**
- formal T3 若 Ours 直接学习 `delta_new(x)`，Action-Centered TS 必须进入 baseline / related work；
- 我们的区分点必须继续来自：**zero-history newly arriving challenger + incumbent/tool-history transfer + very small behavioral budget + sparse positive-advantage region identification**，而不是“首次用相对 baseline 的 effect”。

它与我们的 setting 仍有实质区别：原论文的 baseline action / treatment set 从开始就已知，依靠受控随机化估 treatment effect；我们的 challenger 是中途进入且零历史，真正难点是 onboarding / arrival 与跨旧工具迁移。但这个区别必须通过实验和 formulation 体现，不能靠措辞。

另外 Safe Exploration for Optimizing Contextual Bandits (TOIS 2020) 已经从 production baseline policy 出发，用高置信 off-policy evaluation 决定何时部署新策略。因此如果后续加“只有证明不差于 incumbent 才允许使用”的 deployment gate，这条线也必须进入 related/baseline，不能当新贡献。