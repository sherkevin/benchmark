# task30 report — T3 Nearest-Baseline Closure

> 日期：2026-09-21
> 状态：✅ 已完成
> 原则：先核最近邻，再设计 Ours；任何 pilot 结果在 fidelity/超参审计前不升级为论文正式数字。

## 当前输入状态

- T2 frozen incumbents 已完成。
- Classic T3 baselines 已完成，平均 IOU 全负。
- ACTS 第一轮结果已落盘，但尚待 fidelity 核验。
- PAK/RFF-UCB 64-RFF pilot 已落盘，但尚待 fidelity + calibration/tuning。
- P-LinUCB/P-TS 尚未完成。
- ParetoBandit-style newcomer comparator 尚未完成。

后续每条 baseline 的来源、适配、核验与结果均追加在此。
## 1. Baseline fidelity audit

### 1.1 Action-Centered Thompson Sampling (NeurIPS 2017)

一手论文的 Algorithm 1 直接学习相对 baseline 的 differential/treatment effect：初始化 `B=I, theta=0, b=0`，用 posterior positive-effect probability 得到 nonzero-action 概率 `pi_t`，并更新
`B += pi(1-pi)xx^T`、`b += x(a-pi)r`。

本仓 `challenger_onboarding_core.py` 的二元 ACTS adapter 与这三个核心式逐项一致；`pi_min=.2, pi_max=.8` 与论文实验常用约束一致。它没有使用 incumbent 的额外结构，只把 incumbent 当 action 0。

当前第一轮结果（现有 task29 并发线产物）：
- LLMRouterBench：IOU **-0.05523**，precision 5.95%，recall 36.84%。
- ASlib：IOU **-0.13360**，precision 12.34%，recall 35.53%。
- BBB：IOU **-0.01353**，precision 3.91%，recall 48.87%。

解释边界：ACTS 本身要求 nonzero action 概率有下界，天然不是“绝大多数 context 都回退 incumbent”的稀疏采用策略；其负结果不能被解释成 residual modeling 无效，只能说明 paper-style action-centering + 强制随机化并不足以解决当前 sparse onboarding。

来源：https://proceedings.neurips.cc/paper/2017/hash/4fa177df22864518b2d7818d4db5db2d-Abstract.html

### 1.2 PAK/RFF-UCB (ICML 2025)

官方 repo 已冻结到 `.srcprobe/dgm-online-select` HEAD `3ed2ead`。原 pilot 使用 64 RFF / gamma=1 / D features，不能作为正式数。
官方 README/test 的典型配置为 `num_rff_dim=200`、`gamma=5`、`rff_version=2D`。本轮新增 `t30_rff_official_equiv.py`，保持官方 RFFKernel、UCB 系数、未访问 arm 先探索、每 arm ridge/Sherman-Morrison 更新，并连官方实现里“首次 update 对 b 双加一次”的行为也逐字保留。

在随机 toy replay 上，与官方 `rff_ucb` class 逐 round 对照：
- selected arm 序列完全一致；
- reward trace 最大绝对差 = **0.0**。

因此下面结果可称“官方代码代数等价适配”，不是自造近似。

已完成：
- BBB：IOU **-0.03041**，challenger use 98.22%，precision 3.94%，recall 98.87%。
- LLMRouterBench：IOU **-0.15072**，challenger use 83.46%，precision 5.58%，recall 83.63%。
- ASlib：IOU **-0.45829**，challenger use 98.84%，precision 11.40%，recall 99.54%。

结论：fixed-pool prompt-aware nonlinear UCB 在 zero-history challenger setting 下同样受到 optimism 驱动的 over-exploration。这里使用官方典型超参，不声称已经对 onboarding setting 做过 test-optimal 调参。

来源：https://proceedings.mlr.press/v267/hu25b.html
官方代码：https://github.com/yannxiaoyanhu/dgm-online-select

## 2. P-LinUCB shared-affine-subspace transfer

官方 repo：`.srcprobe/Projected_Bandits` HEAD `f7eea02`。原论文把连续 contextual-bandit tasks 的参数 `theta*` 假设在共同低维 affine subspace 附近；用历史 task 的 theta 估计做 PCA，然后用 `lambda1 P_perp + lambda2 P` 作为新 task 的 projected regularizer。
本仓适配明确保留这一 transfer 核心，但不冒充原问题完全同构：历史 tools 当 meta tasks，新 challenger 当新 task；passive T3 的 context 自然到来，所以 P-LinUCB 只负责估 challenger reward function，是否采用则与 frozen incumbent 的 history-only expected reward 比较。

参数采用官方 README 对 real data 指定的 `lambda1=3.5, lambda2=0.1`；共享 rank 用论文 Remark 1 的最大 eigengap 自动选择。

paper-default exploration scale=1.0：
- LLMRouterBench：IOU **-0.21192**，challenger use 99.996%。
- ASlib：IOU **-0.46721**，challenger use 100%。
- BBB：IOU **-0.03065**，challenger use 100%。

BBB exploration-scale sensitivity：
- scale=0：IOU **-0.00400**；
- 0.001：**-0.00407**；
- 0.005：**-0.00486**；
- 0.01：**-0.00634**；
- 0.05：**-0.01407**；
- 0.1：**-0.02384**；
- 0.2：**-0.03006**；
- 1.0：**-0.03065**。

关键判断：即便完全关闭 optimism、只保留 shared-subspace greedy estimate，BBB 仍未转正，说明问题不只是 UCB coefficient 过大；历史工具学出的 affine structure 对稀疏 positive residual region 的 precision 仍不足。

来源：https://proceedings.mlr.press/v238/bilaj24a.html

## 3. ParetoBandit-style newcomer onboarding
ParetoBandit 官方 repo 已冻结到 `.srcprobe/ParetoBandit` HEAD `72ccbef`。其 model-onboarding 实验明确采用：运行中注册 newcomer，前 `BURNIN_PULLS=20` 次强制调用 newcomer，之后交给 disjoint LinUCB；生产参数 `alpha=0.01`、`forgetting_factor=0.997`。

主 T3 不允许 challenger tool-side competence metadata，因此本轮保留其 onboarding 机制，但使用 agnostic/cold challenger，不使用 T-shirt competence prior；incumbent 继续是 T2 已冻结 policy。

同时跑 B={1,4,8,16,20,32}，其中 B=20 是官方 onboarding 实验值。

LLMRouterBench IOU：
- B=1 **-0.00011**；
- B=4 -0.00053；
- B=8 -0.00111；
- B=16 -0.00172；
- B=20 **-0.00186**；
- B=32 -0.00282。

ASlib IOU：
- B=1 **-0.00300**；
- B=4 -0.01145；
- B=8 -0.02387；
- B=16 -0.04724；
- B=20 **-0.05865**；
- B=32 -0.09297。

BBB IOU：
- B=1 **-0.000067**；
- B=4 -0.00086；
- B=8 -0.00407；
- B=16 -0.00775；
- B=20 **-0.00836**；
- B=32 -0.01044。
现象很清楚：在 mature incumbent 下，固定 blind burn-in 越大，平均损失反而越大。ParetoBandit-style onboarding 比普通 UCB 更保守，但仍没有让 IOU 转正。这把 T5 的问题从“多 probe 是否有用”改成了“能否选择信息价值高且风险低的 probe context”。

来源：https://arxiv.org/abs/2604.00136
官方代码：https://github.com/ParetoBandit/ParetoBandit

## 4. 当前未关闭项

- P-LinUCB 的 LLMRouterBench / ASlib 低-exploration 极限诊断仍在跑，用于排除“只需把 UCB scale 调小”。
- ACTS 当前 fidelity 已确认核心式一致；后续若需要最强版本，可做 pi bounds sensitivity，但 paper-style comparator 已具备。
- RFF-UCB 正式结果已全部落盘；不继续为了让 baseline 好看而用 test set 调官方超参。
- 本任务全部关闭后，才根据共同 failure mode 决定 Ours。
## 5. Transfer family 的保守极限与 P-TS 复核

为了不给 P-LinUCB 一个“只是 UCB 半径太大”的弱基线口径，额外把 exploration scale 压到接近 0。这里是**诊断上限**，不是用 test set 选超参后的正式部署方案：如果连极端保守版仍不能获得有意义正 IOU，说明 shared-subspace 表示本身没有解决 positive-region identification。

当前最保守结果：
- LLMRouterBench：scale=0 IOU **-0.000226**；scale=0.01 **-0.000026**。后者 challenger use 0.82%，recall 0.995%。
- ASlib：scale=0 IOU **-0.000160**；scale=0.01 **-0.000201**。scale=0 challenger use 0.36%，recall 0.72%。
- BBB：完整 sensitivity 中最好是 scale=0，IOU **-0.00400**，challenger use 23.44%，precision 2.93%，recall 15.14%。

因此 P-LinUCB 可以通过“几乎不用 challenger”把 LLM/ASlib 的 harm 压到接近 0，但没有吃到 Challenger Oracle opportunity。

同一 AISTATS 2024 方法族的 Projected Thompson Sampling 也做了复核，posterior covariance scaling 按官方 `ts_function` 规则：
- BBB：IOU **-0.01436**，use 46.73%，precision 3.64%，recall 42.13%。
- ASlib：IOU **-0.07230**，use 20.04%，precision 13.83%，recall 24.98%。
- LLMRouterBench：运行中，结果落盘后补。

这避免了只挑 P-LinUCB 这一种 optimism 实现做弱 baseline；如果 P-TS 也不能转正，则 shared-affine-subspace transfer 风险可视为基本关闭。

## 6. Final closure update — 2026-09-22

Projected Thompson Sampling finished on the previously open LLMRouterBench run:

- LLMRouterBench: mean IOU **-0.030635**, challenger use 24.98%, precision 6.22%, recall 27.49%, harmful-adoption rate 21.24%.
- ASlib SAT12-ALL: mean IOU **-0.072302**, challenger use 20.04%, precision 13.83%, recall 24.98%.
- BBB: mean IOU **-0.014361**, challenger use 46.73%, precision 3.64%, recall 42.13%.

Artifacts:
- data/tool_results/divergence/t30_pts_llm_s1p0.json
- data/tool_results/divergence/t30_pts_aslib_s1p0.json
- data/tool_results/divergence/t30_pts_bbb_s1p0.json

The transfer-family objection is therefore closed at the comparator level: both projected UCB and projected TS fail to produce positive average IOU under the frozen mature-incumbent protocol.

### Task30 final conclusion

The nearest-baseline families now cover:

- relative/differential effect modeling: ACTS;
- nonlinear contextual optimism: official-equivalent RFF-UCB;
- cross-task/shared-subspace transfer: P-LinUCB and P-TS;
- explicit runtime newcomer handling: ParetoBandit-style burn-in + LinUCB.

No covered family gives positive mean IOU across the three frozen benchmarks. The common failure pattern is not simply insufficient model capacity: aggressive uncertainty-driven exploration overuses the zero-history challenger, while conservative variants can drive harm toward zero only by collapsing challenger recall. This closes Task30 and hands the problem to Task31/Task32 as a selective identification problem rather than another generic routing baseline search.
