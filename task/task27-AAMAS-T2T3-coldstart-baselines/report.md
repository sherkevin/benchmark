# task27 report — Cold-start protocol 诊断与第二次收敛

> 日期：2026-09-21
> 状态：⚡️ baseline protocol 已发现关键结构问题；正式 T3 数字尚未冻结。

## 1. 最重要的新发现

把一个 arm 从完整 20/31-arm pool 里 hold out，再在到达时加回来，会严重稀释“新工具 onboarding”问题。
原因不是方法，而是集合覆盖：已有 19/30 个 provider 时，一个额外 provider 的独占 marginal value 很小。

第一版 full-pool replay 实测：
- LLMRouterBench：new arm 对 incumbent Oracle 的平均边际贡献只有 **0.00152**；
- ASlib SAT12-ALL：只有 **0.00282**。

在这种 protocol 下，最优策略很容易退化成“基本别碰新 arm”，而 overall regret 又被旧工具之间的 routing error 主导。
这会让 T3 测到错误的能力：不是 new-tool adaptation，而是重新学习整个旧工具池。

因此 full-pool held-out-arm 不再作为 T3 主协议，只保留 saturation appendix。
## 2. 为什么 capability-level subpool 才符合系统定义

本项目架构本来就是先按 function key / capability 分组，再在同功能 provider 内 routing。
因此 LLMRouterBench 的 20 个模型、ASlib 的 31 个 solver 是“候选母池”，不是默认一个 capability group 必须同时放 20/31 个 arms。

冻结候选 pool size：
- LLMRouterBench / ASlib：K = {3,5,9}；
- BBB：K = {3,5,7}；
- full K 只作 saturation control。

采样规则不 cherry-pick：每个 split × 每个 K 下，每个 arm 恰好轮流做一次 new arm；其余 K-1 个 incumbent 从剩余 arms 均匀无放回采样，seed 固定。

## 3. pool size 对新 arm 真实边际价值的影响

| Benchmark | K=3 | K=5 | K=9/full-ish | Full pool |
|---|---:|---:|---:|---:|
| LLMRouterBench | **0.0751** | **0.0297** | 0.00909 (K=9) | 0.00152 (K=20) |
| ASlib | **0.0874** | **0.0379** | 0.01374 (K=9) | 0.00282 (K=31) |
| BBB complete | **0.0150** | 0.00424 | — | 0.00241 (K=7) |

数值是：
`E[max(new arm, incumbent oracle) - incumbent oracle]`。

这说明 K=3/5 才能稳定实例化“新工具可能带来明显新能力”的 onboarding 问题；随着 pool saturation，new arm 独占增量自然衰减。
## 4. 更关键的修正：不要重新学习旧工具池，要把 incumbent router 当成已部署系统

现实 deployment 不是：新工具一来，把所有旧工具重新从零跑 bandit。
而是已经存在一个成熟 router `pi_old(x)`，新工具只需要回答：

> 当前 x 是否值得从 incumbent system 切到这个 zero-history new tool？

因此更干净的问题是：

`g_new(x) = r_new(x) - r_{pi_old(x)}(x)`

学习哪些 context 上 `g_new(x) > 0`。

这与 BBB 第一轮发现的 asymmetric safe-switching 完全一致：默认 incumbent 很强，错误切换成本高，真正困难是 precision 足够高地识别少数有收益的 switch。

用 history-trained per-arm Ridge 作为 mature incumbent router 后，新工具相对**已部署 router**的 Gate Oracle lift 仍然很明显：

| Benchmark | K=3 | K=5 | K=9 | Full |
|---|---:|---:|---:|---:|
| LLMRouterBench | **0.1126** | **0.1047** | 0.0676 | 0.0555 |
| ASlib | **0.1127** | **0.0506** | 0.0389 | 0.0204 |

Gate Oracle = `max(reward(pi_old(x)), reward(new,x))`。

所以 onboarding 不是没有价值；只是必须把它从“重学 K-arm routing”分离出来。
## 5. T3 主问题因此进一步收敛

候选正式定义：**Safe Incremental Tool Onboarding under Sparse Contextual Feedback**。

给定成熟 incumbent policy `pi_old` 与新工具 `T_new`：
- new tool 没有历史；
- 每次 probe/call 有成本；
- 只允许少量 behavioral feedback；
- 目标是识别 new tool 相对 incumbent 的正优势区域；
- 同时避免 harmful switches。

这比“dynamic K-arm routing”更精确，也更符合已有实测：
- LLMRouterBench / ASlib 给可学习 competence；
- BBB 给 high-stakes rare-rescue / safe-switching；
- BEIR 给 negative control：pool 没 specialization 时应当不切。

## 6. 旧 preliminary baseline 数字怎么处理

已跑的 full-pool Best Fixed / TS / LinUCB / LinTS 只作为 protocol diagnostic，不进入论文主表。
它们显示：
- full pool 下 new arm 独占贡献过小；
- naive UCB/TS 会过度探索新 arm，产生大量 harmful switches；
- 说明必须把 mature incumbent policy 固定后，单独测 onboarding gate。

这些数字保留在 `data/tool_results/divergence/t3_coldstart_baselines_*.json`，不删除、不包装成正结果。

## 7. 下一步

1. 冻结 v3 onboarding protocol；
2. 实现 mature incumbent router + new-tool gate baselines；
3. canonical baselines 改成“Never New / Random Explore / LinUCB Gate / LinTS Gate / Conservative contextual bandit / MTSS-style transfer / Ours”；
4. T3 主指标改成 Net Onboarding Gain、Gate Regret、Harmful Switch Rate、Opportunity Capture；
5. 再做 B={1,4,8,16,32}。