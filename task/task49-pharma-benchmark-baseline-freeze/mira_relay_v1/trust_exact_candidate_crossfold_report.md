# MIRA-Relay trust-exact candidate 跨折可行性

日期：2026-10-03
状态：**5 个 fold、seed 0 的 fit-only candidate 全部通过独立 verifier；仍未进入生效 prereg 或 online replay。**

## 固定边界

沿用 fold0 candidate 的目标和接受规则：

- direct head 是冻结 Bernoulli total-logit offset；
- fit-only summed Bernoulli NLL + `0.5 * 0.01 * (||U||² + ||b||²)`；
- 7 family、64 维 residual、33 provider bias；
- `trust-exact`，`maxiter=100`，`gtol=1e-6`；
- 接受条件为有限参数、Hessian 对称误差 `≤1e-10`、gradient 无穷范数 `≤1e-6`，并且 SciPy
  status 0 success，或 status 2 且满足同样的数值停止条件。

没有传入 evaluation features、labels 或 online event；每个 fold 都保存独立 NPZ 参数、receipt
和 verifier JSON。

## 结果

| fold | candidate 状态 | SciPy status | objective | gradient∞ | verifier |
|---:|---|---:|---:|---:|---|
| 0 | accepted | 0 | 3340.1063218412 | 2.33e-9 | verified |
| 1 | accepted | 0 | 4368.6638261792 | 2.45e-8 | verified |
| 2 | numerically_stationary_status2 | 2 | 4824.5478357528 | 2.29e-7 | verified |
| 3 | accepted | 0 | 4482.6329294755 | 1.19e-7 | verified |
| 4 | accepted | 0 | 4107.5468670967 | 1.30e-10 | verified |

5/5 candidate receipt 通过 verifier。fold2 的 status 2 保留为
`numerically_stationary_status2`，没有改写成 SciPy success；它满足固定 gradient/Hessian
数值门，因此被 verifier 接受为数值停点。

## 独立 verifier 做的检查

`verify_mira_relay_trust_exact_candidate_v1.py` 不导入 candidate 的 objective 函数，而是从
冻结 fit feature/labels、direct offset 和 registry 重新计算：

- objective、gradient、解析 Hessian；
- Hessian 最大对称误差；
- 参数 NPZ 的唯一 key、shape、finite、archive SHA-256 和参数数组 SHA-256；
- input、checkpoint、registry SHA-256；
- SciPy status 及 status 2 接受规则。

测试还验证：修改参数数组后 archive hash 会被拒绝；status 2 在梯度/对称误差/finite 条件
不满足时 fail closed。

## 证据位置

跨折收据：

`experiments/logs/task49_mira_relay_fit_diagnostic_20261003/trust_candidate_crossfold_v1/`

每个 `fold{0..4}_seed0/` 下包含 `candidate_parameters.npz`、`candidate_receipt.json`、
`config.json`、`summary.json` 和 `verification.json`；根目录保存 `crossfold.jsonl`、
`summary.json`、`config.json`、测试日志和源码 hash。

实现：

- `src/probe_headless/run_mira_relay_trust_exact_candidate_v1.py`
- `src/probe_headless/verify_mira_relay_trust_exact_candidate_v1.py`
- `src/probe_headless/run_mira_relay_trust_exact_candidate_crossfold_v1.py`

该结果只证明 fit-only 数值候选可封存、可独立复核，不能证明选择收益、在线实时性或
MIRA-Relay superiority。下一步若要采用该候选，仍需双重确认后更新决议/preregistration，
再设计 no-effect contract replay；当前论文主表和生效方法均不变。
