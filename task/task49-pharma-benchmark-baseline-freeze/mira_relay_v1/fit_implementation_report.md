# MIRA-Relay fit-only 初始化实现报告

日期：2026-10-02
状态：**CPU 合同单测已通过；尚未运行真实 benchmark replay，未申请 A800。**

## 1. 本次完成的范围

新增 `src/probe_headless/mira_relay_fit.py`，实现 MIRA-Relay 预登记的 fit-only
warm start。它只接受一个 fold 的 fit 特征、冻结 direct head 和 fit 标签矩阵；不读取
validation/evaluation/confirmation 数据，也不负责事件 replay。

输出是 `MIRARelayFit`，可直接通过 `policy_kwargs()` 注入现有
`MIRARelayPolicy`。同时保存 family residual、provider bias、对角 precision/variance、
每个 provider 的 fit cell 数、优化状态和目标函数收据，便于后续独立 verifier 重算。

## 2. 与固定合同的对应关系

| 合同要求 | 实现 | 证据 |
|---|---|---|
| 冻结 direct head | `direct_prior[a] @ x` 作为 Bernoulli total-logit offset；优化梯度不含 direct head | `test_direct_head_is_not_optimized...` |
| family-shared residual | 每个 provider 的 residual 使用其 registry family 的同一 `u_g` | `provider_family` 索引和 `family_residual[g,d]` |
| provider bias | 每个 provider 单独的 `b_a` | `provider_bias[k]` |
| fit-only BCE/L2 | 观测 fit cells 上的 summed Bernoulli NLL 加 `0.5*l2‖(u,b)‖²`；固定 L-BFGS 参数 `max_iter=100, ftol=1e-12, gtol=1e-9` | `fit_mira_relay_initialization` |
| 对角 Fisher precision | `I_u[g,j]=l2+Σ p(1-p)x_j²`，`I_b[a]=l2+Σ p(1-p)`；输出 variance 为倒数 | `test_masked_cells...` |
| 缺失标签隔离 | `mask` 或 NaN 只保留观测 cell；观测 cell 必须是 0/1 | invalid-panel tests |
| 可重放性 | 同样输入使用确定性零初值和 L-BFGS，输出状态/目标/梯度范数/迭代数 | `test_fit_is_deterministic...` |

这里的 Fisher 是 fit 后 total logit 的 Bernoulli curvature；它是后续 online diagonal-
precision projection 的初始 precision。没有把 diagonal 元素称为完整 Laplace 逆矩阵。

## 3. 数值和生命周期检查

本次只做 CPU 单元和输入合同检查，没有跑科学大批次：

```text
10 passed in 1.03s
py_compile passed
git diff --check passed
GPU used: false
```

原始日志与配置：

- `experiments/logs/task49_mira_relay_fit_20261002/v1/test.log`
- `experiments/logs/task49_mira_relay_fit_20261002/v1/config.json`

测试覆盖：目标函数能下降、总 logit offset 不更新 direct head、family 参数共享、masked
cell 不进入 fit、Fisher variance/precision 互为倒数、非法标签/维度/l2 fail closed，以及
同输入确定性。

## 4. 尚未完成的 runner/verifier

这次没有声称 MIRA-Relay 有效果。进入 full common-stratum replay 前还缺：

1. 将现有 `fit_fold_priors` 的 hidden fit cells 和 direct head 接入本函数，并保存每个
   fold×seed 的 fit state、输入 hash 和 source hash；
2. 把 `MIRARelayPolicy` 接入预登记的 `NoProbeShield`、`ProbeNoUpdate`、
   `ProbeIndependentResidual`、`ProbeMIRA-Relay` 同 schedule runner；
3. 写独立 verifier，重新计算 offset、family/provider 参数、Fisher precision、safe menu、
   propensity、hazard、delay release 和 state hash；
4. 先运行 preregistered state→action sensitivity 和 145-row fit-only switchability
   stop tests，再考虑完整 CPU receipt；
5. 只有 CPU 机制门、对照门、收益门和成本门都通过，才讨论 Nebula A800。

当前没有修改 `main.tex`、预登记合同或 confirmation 数据，也没有调参追求正结果。
