# MIRA-Relay fit optimizer candidate: exact-Hessian Newton

状态：候选合同，未生效；不改变当前 preregistration，也不能用于宣称效果。
日期：2026-10-03

## 1. 为什么提出这个候选

当前生效的 fit 合同是 full-batch L-BFGS-B，`max_iter=100`。在 fold0/seed0 上目标从 `5363.2964` 降到 `3444.2793`，但 `grad_inf=12.5075`；fold0–4 的 L-BFGS-1000 仍在 `grad_inf=0.207–0.450` 停止。解析 Hessian 的五折诊断把梯度降到 `2.3e-7` 或更低，目标也更低，说明现有失败主要来自条件数和数值预算。

## 2. 保持不变的数学对象

对 fit cell $(x_i,a)$，总 logit 仍是冻结 direct head offset、family residual 和 provider bias 的和。优化目标仍为

$\displaystyle \sum_{(i,a)\in\mathcal O}\left[\log(1+e^{z_{ia}})-y_{ia}z_{ia}\right]+\frac{\lambda}{2}(\lVert U\rVert_2^2+\lVert b\rVert_2^2)$

其中 `lambda=0.01`，只读 fit cells，direct head 不回训，family registry 不变。候选只替换数值求解器，不增加标签、特征或在线信息。

## 3. 候选求解器

- 481 个参数：7 个 family residual × 64 维，加 33 个 provider bias。
- 解析 gradient 和解析 Hessian；Hessian 包含 family block、provider bias 对角块及同 provider 的交叉块，另加 `lambda I`。
- `trust-exact`，`maxiter=100`，`gtol=1e-6`；保留 SciPy 原始 `status/message`。
- 接受条件不能只看 SciPy `success`：必须同时满足有限参数、目标有限、Hessian 对称误差、`grad_inf <= 1e-6`，并通过独立 verifier 的 objective/gradient/parameter hash 重算。`status=2` 只能在这些数值条件全部通过时标记为 `numerically_stationary_status2`，不得改写为 success。
- 若不满足接受条件，fit receipt 为 `fit_not_converged`，禁止进入 replay。

## 4. 进入 replay 前的门

1. 五折/至少两 seed 的 candidate fit receipts 全部通过独立 verifier。
2. 与当前 L-BFGS-100 receipt 并列保存，不能覆盖旧失败证据。
3. 在同一 fold、seed、菜单、顺序、delay 和 probe schedule 下，先跑 no-effect contract replay；检查 action、release、state hash、failure/unknown 分母。
4. 再决定是否写 ADR 并更新 preregistration；在双重确认前，本候选不进入论文主矩阵。

## 5. 不允许的解释

该候选不能被描述为 MIRA-Relay 的创新，也不能把更低 fit objective 当作下游选择收益。它只是使同一 fit 目标可稳定求解的工程/数值候选；最终方法价值仍需 matched online controls、harmed/rescued 分解和 confirmation 证明。
