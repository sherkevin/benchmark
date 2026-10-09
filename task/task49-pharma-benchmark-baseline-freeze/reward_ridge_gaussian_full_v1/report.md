# Reward-ridge Gaussian baseline full development receipt v1

日期：2026-10-01  
状态：已完成，独立核验通过；开发结果，不是 confirmation，也不是 MIRA superiority 结果。

## 收据

- 原始目录：`experiments/logs/task49_reward_ridge_gaussian_full_20261001/v1/`
- 机器绑定：`receipt_binding.json`
- 独立 verifier：`verification.json`，`60/60` streams verified，8,664 decisions。
- 独立分析：`analysis.json`；脚本：`src/probe_headless/analyze_reward_ridge_gaussian_full_v1.py`。
- 声明代码 commit：`7a8fd127913c13f41cfff5e9e522ad1911613b5c`。
- 输入仍为 BBB development 722 molecules、33 providers、5 scaffold folds、2 seed slots、
  delay `{0,1,4}`；confirmation closed。

## 方法合同

每个 fold/seed 的 prior 是 fit-only binary reward ridge：

$$W_R=(X_{fit}^{\top}X_{fit}+1.0I)^{-1}X_{fit}^{\top}Y_{fit}.$$

在线状态初始 covariance 为 `0.02 I`（precision=`50.0`），observation noise=`1.0`，
LinUCB alpha=`0.5`。Frozen 消费反馈但不改参数，Online 每次 selected-only feedback 释放
后做一次 rank-one Gaussian update。没有隐式 clipping，也没有读取未选 provider 输出。

## 主结果

| 方法 | attempts | correct | task success | valid-return correctness | failure rate | family macro |
|---|---:|---:|---:|---:|---:|---:|
| reward-ridge Frozen | 4,332 | 3,711 | 0.856648 | 0.862021 | 0.006233 | 0.893501 |
| reward-ridge Online | 4,332 | 3,706 | 0.855494 | 0.861059 | 0.006464 | 0.862370 |

Online−Frozen 的 molecule-paired 平均差为 `−0.001154`（`−0.1154 pp`）。按 molecule
先平均 seed/delay、再按 scaffold cluster bootstrap（10,000 次，seed=`20261001`），
95% CI 为 `[-0.011242, +0.009707]`，跨 0。这个结果是一个合法但没有在线收益的 Gaussian
baseline 结果；不调参追分。

策略开销也已记账：Frozen selection/update p95=`163,227/1,459 ns`，Online
`161,083/26,583 ns`。这些是本机 Python/cache replay 的策略开销，不是线上 provider
价格、token 或服务延迟。

## 解释边界

1. 这份 receipt 证明 reward-ridge Gaussian baseline 的 fit prior、precision、feedback
   时钟和恢复链可以完整运行；它不证明 Gaussian 方法优越。
2. Online 没有超过自身 Frozen，paired 区间跨 0；应作为负/不确定现代 baseline，不能被包装
   成 MIRA 的反面或正面证据。
3. Family macro 从 `0.893501` 降到 `0.862370`，说明只报 pooled task success 会掩盖
   provider-family 分布变化；主表必须同时保留两类指标。
4. 旧 direct-logit→Gaussian Greedy/Bayesian receipt 仍是 adapted/out-of-stratum；本实验
   不能追溯改写旧结果，只能新增一个语义自洽的 Gaussian comparator。

## 对验收标准的影响

E2/E5 的 Gaussian comparator、failure 分母、成本和独立 recovery 入口已补齐；但 Gate B/C
仍不自动通过。统一主表还缺所有 baseline 的同一成本/预算汇总、refusal utility、完整
E1--E5 矩阵和冻结 confirmation。下一步把本 receipt 接入 baseline role roster 和统一主表，
再决定是否需要新的方法效果实验。
