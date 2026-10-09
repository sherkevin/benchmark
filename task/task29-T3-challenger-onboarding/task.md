# task29 — Formal T3 Challenger-Onboarding Protocol + Baseline Adaptation Audit

> 日期：2026-09-21
> 对应：实验矩阵 v3 的 T3；承 T-287 / task28
> 状态：⚡️ 进行中

## 1. 只解决一个问题

在 T2 已冻结 incumbent policy 后，把 formal T3 的学习问题压成二元决策：action 0=delegate to frozen incumbent；action 1=call newly arrived challenger。challenger 到达前没有 reward history；每轮只观察实际选择路径的 reward。

定义 `delta_c(x)=mu_c(x)-mu_0(x)`。Challenger Oracle 只做 `1{delta_c(x)>0}` 的二元换挡，不重新优化旧工具池。

主 regret：`R_C(T)=sum_t[max(mu_0(x_t),mu_c(x_t))-r_t]`。
主收益：`IOU = U(onboarding policy)-U(matched frozen incumbent-only)`。

## 2. Baseline 准入原则

正式 comparator 必须同时满足：
1. Claim overlap：真正覆盖本文某个 claim；
2. Assumption compatibility：不要求主协议禁止的 privileged 信息；
3. Adaptation fidelity：适配到 incumbent-vs-challenger 后不能把原算法核心机制改没。

| Method | Peer-reviewed? | 原问题 | 与 T3 的映射 | 结论 |
|---|---:|---|---|---|
| Best Incumbent | — | production baseline | 永远 action 0 | **主表必需** |
| Beta-TS | classic | non-contextual MAB | incumbent 有历史、challenger 0 history | **主表必需** |
| LinUCB | classic | contextual linear bandit | 两个 action，共享 x，各自 reward model | **主表必需** |
| LinTS | classic | Bayesian linear CB | 同上 | **主表必需** |
| **PAK-UCB / RFF-UCB** | **ICML 2025** | prompt-aware model selection | frozen incumbent 成熟 arm；challenger 新 arm | **主表必需** |
| **P-LinUCB / P-TS** | **AISTATS 2024** | 多 CB tasks 共享 affine subspace | 历史 tools 作为 meta tasks | **若 Ours 使用 cross-tool transfer，则主表/T6 必需** |
| **FC-Entry** | **ICML 2024** | impatient arms + later arm entry | entry 可比，但 patience/exit/cycle 假设不匹配 | **Related / appendix** |
| **CLUCB** | **NeurIPS 2017** | baseline-safe contextual exploration | incumbent=baseline | **只有最终 claim 含显式 safety guarantee 时 mandatory** |
| **UCB-AA** | 2026 preprint | arriving arms + screening | non-contextual arrival screening | **concurrent nearest work** |
| ContextRoute/LQM | 2026 preprint | fixed same-function provider routing | 无 zero-history onboarding | **concurrent same-function reference** |
## 3. 一手来源与代码状态

### PAK-UCB / RFF-UCB
- ICML 2025 / PMLR 267。
- 官方代码：`https://github.com/yannxiaoyanhu/dgm-online-select`。
- local mirror：`.srcprobe/dgm-online-select`，HEAD `3ed2ead`。
- 官方实现维护 per-model kernel/RFF reward head；未访问 arm 会优先探索。
- T3 适配不改 select/update 公式：action set 仅 `{frozen incumbent, challenger}`，用成熟 incumbent history warm-start arm 0，challenger arm 1 保持 0 visitation。

### P-LinUCB / P-TS
- AISTATS 2024：`Meta Learning in Bandits within Shared Affine Subspaces`。
- 从历史 contextual-bandit tasks 学低维 affine subspace，再用 projected LinUCB / TS 降低新 task regret。
- 它不是 provider router，但与 cross-tool transfer claim 高度重叠，因此若 Ours 用 shared representation/subspace，它必须进入 transfer baseline。

### FC-Entry
- ICML 2024：`On Multi-Armed Bandit with Impatient Arms`。
- FC-Entry 是 impatient-arm setting 的 extension；允许 later arm entry 并给 dynamic-regret guarantee。
- 它证明 arm entry 不是新问题，但强行去掉 patience/feasible-cycle 后就不是 faithful implementation，所以不为“baseline 数量”硬塞主表。

### CLUCB
- NeurIPS 2017：`Conservative Contextual Linear Bandits`。
- 保证 cumulative reward 高概率下始终不低于 baseline policy 的 `(1-alpha)` 比例。
- 如果本文只报告 harmful-adoption rate 而不声称 hard safety guarantee，则不是 mandatory；若引入显式 safety constraint，则必须正面对比。

## 4. Formal T3 主表先冻结为 6+2

1. Frozen Incumbent-only
2. Beta-TS
3. LinUCB
4. LinTS
5. RFF-UCB（PAK-UCB family，官方实现）
6. P-LinUCB 或 P-TS（若 cross-tool transfer 是 Ours 的一部分）
7. Ours
8. Challenger Oracle

并行报告但明确 concurrent：UCB-AA、ContextRoute/LQM。FC-Entry / CLUCB 放 appendix/conditional column，依据最终 assumption 是否匹配决定。

## 5. 当前真正剩下的研究缝隙

不能再把以下单点叫创新：prompt-aware routing、shared affine subspace、arm entry、safe baseline exploration、arrival screening、same-function routing。

候选问题必须更窄：

> **已有成熟 incumbent capability policy 时，一个无 metadata、零行为历史的 same-function challenger 到来；其增量价值只集中在稀疏 context 子集。如何借历史工具形成的 task representation，在极少 challenger feedback 下识别局部正 residual advantage，同时避免 uncertainty-driven over-exploration。**

是否最终能形成方法创新，要由 formal T3 baseline replay 与 transfer ablation 决定。