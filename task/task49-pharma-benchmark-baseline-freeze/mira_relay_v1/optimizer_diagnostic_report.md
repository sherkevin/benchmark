# MIRA-Relay fit 优化器数值诊断

日期：2026-10-03
范围：fold 0 / seed 0，444 条 fit 行；只诊断 fit-only residual 目标，不运行 replay，不读取 evaluation labels。

## 目的

前一轮固定合同的 L-BFGS（100 次迭代）没有收敛。这里保持完全相同的目标：

`summed Bernoulli NLL + 0.5 * 0.01 * (||U||² + ||b||²)`

direct provider head 继续作为冻结 total-logit offset。只比较数值优化器，不能把上限诊断
误写成方法改进。

## 数据与参数

- 输入 SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`
- encoder SHA-256：`971084523d8bee30019b16a895426df589a5179614971a7a93bc63f364bfc82b`
- family registry SHA-256：`9738363d3ad64d0e52ceffb71acfe7b4f3db5bea9ba007a475876f7e0f2ff365`
- 33 providers、7 families、64 维 feature，参数总数 481
- 观测 fit cells：`444 × 33 = 14,652`
- 目标、梯度、Hessian 都使用同一份 fit feature/label/direct-head hash；没有 evaluation feature 或 label 进入优化。

## 优化器比较

| 配置 | SciPy 状态 | 迭代 | 目标值 | 最终梯度无穷范数 |
|---|---|---:|---:|---:|
| `L-BFGS-B`, maxiter=100（合同预算） | `TOTAL NO. of ITERATIONS REACHED LIMIT` | 100 | 3444.2792905806 | 12.5075153750 |
| `L-BFGS-B`, maxiter=1000（数值上限诊断） | `TOTAL NO. of ITERATIONS REACHED LIMIT` | 1000 | 3340.2848238915 | 0.3737611275 |
| `trust-exact`, maxiter=100，解析 Hessian | SciPy status 2，bad approximation；数值上已停滞 | 12 | 3340.1063218412 | `2.33e-9` |

`trust-exact` 的 SciPy `success=false` 不能被写成“优化器成功”，但其梯度已经达到
预设 `gtol=1e-9` 附近，目标值也比 L-BFGS-1000 更低。因此目前证据支持：**100 次
L-BFGS 主要受数值预算和目标条件数限制，前一轮目标/梯度实现没有被诊断出错误。**

## 解析 Hessian 检查

在零参数点，解析 Hessian：

- 形状 `481 × 481`
- 对称误差最大值 `2.27e-13`
- 最小特征值 `0.1723`，最大特征值 `31389.47`
- 5 个坐标的中心差分梯度误差最大约 `2.07e-9`

Hessian 正定但条件数约为 `1.82e5`，与 L-BFGS 在固定 100 次预算下缓慢下降一致。原始
检查收据见 `hessian_check.json`。

## 结论和边界

本次诊断把问题缩小为优化数值预算/条件数，而不是发现目标函数或解析梯度不一致。它没有
证明 MIRA-Relay 的效果，也没有改变预登记的 L-BFGS 合同。`trust-exact` 只能作为数值
上限和 verifier 参考，不能直接替换主方法。

如果保留当前合同，下一步应继续让 `fit_not_converged` fail closed；如果要采用同一目标的
解析 Newton/trust 实现，必须先记录一项新的决议，明确优化器、停止条件和与已有 L-BFGS
结果的公平对照，再重新封存 fit receipt，不能静默换优化器。

## 原始证据

目录：`experiments/logs/task49_mira_relay_fit_diagnostic_20261003/optimizer_v1/`

- `config.json`：固定输入、目标、源码 hash
- `lbfgs_100.json`、`lbfgs_1000.json`、`trust_exact.json`：逐配置原始优化收据
- `hessian_check.json`：解析 Hessian 对称性、特征值和有限差分检查
- `summary.json`：组合结果

本轮未使用 GPU、未运行 full matrix、未读取 confirmation/evaluation labels、未产生效果 claim。
