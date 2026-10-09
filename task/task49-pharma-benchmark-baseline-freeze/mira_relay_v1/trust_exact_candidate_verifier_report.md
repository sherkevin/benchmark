# MIRA-Relay trust-exact candidate 与独立 verifier

日期：2026-10-03
状态：**fold0/seed0 candidate 已封存并通过独立 verifier；候选仍未进入生效 prereg 或 online replay。**

## Candidate receipt

候选使用与现有诊断完全相同的 fit-only 输入和数学目标：

- fold 0 / seed 0；444 fit molecules、133 validation molecules；33 providers、7 families、64 维 frozen encoder；
- direct provider head 作为冻结 total-logit offset；
- `summed Bernoulli NLL + 0.5 * 0.01 * (||U||² + ||b||²)`；
- `trust-exact`，`maxiter=100`，`gtol=1e-6`；
- 未读取 evaluation labels、未运行 online replay。

结果：SciPy `success=True/status=0`，12 次迭代，目标 `3340.106321841232`，gradient
无穷范数 `2.3267e-9`，Hessian 对称误差 `1.14e-13`。参数总数 481，已写入只含 `theta`
数组的压缩 NPZ，并在 receipt 中绑定 archive SHA-256 和参数数组 SHA-256。

## Independent verifier

新增 `verify_mira_relay_trust_exact_candidate_v1.py`，不调用 candidate optimizer 的目标函数；
它从冻结输入、fit feature/labels、direct offset 和 family registry 独立重算：

1. total-logit objective；
2. 481 维 gradient；
3. 解析 Hessian 与最大对称误差；
4. NPZ 参数 shape、有限性、字节级参数 SHA-256 和 archive SHA-256；
5. input/checkpoint/registry hash；
6. SciPy status 和候选接受条件。

`status=2` 只能在 `scipy_success=false`、gradient 无穷范数 `≤1e-6`、Hessian 对称误差
`≤1e-10` 且所有值有限时标记为 `numerically_stationary_status2`；否则 fail closed。当前
fold0 candidate 是 status 0 的普通 `accepted`，没有把 status 2 伪装成 success。

独立验证结果：

```text
status: verified
objective_recomputed: 3340.106321841232
gradient_inf_norm_recomputed: 2.326670661689789e-09
hessian_symmetry_max_abs_recomputed: 1.1368683772161603e-13
parameter_sha256_recomputed: 1fb5d7570f43e0644cf77b6e0d15cb7e42cd483586d3813280462fd4a95eacf4
```

## Evidence

Candidate 与 verifier 收据：

`experiments/logs/task49_mira_relay_fit_diagnostic_20261003/trust_candidate_v1/`

- `candidate_parameters.npz`
- `candidate_receipt.json`
- `config.json`
- `summary.json`
- `verification.json`

实现和测试：

- `src/probe_headless/run_mira_relay_trust_exact_candidate_v1.py`
- `src/probe_headless/verify_mira_relay_trust_exact_candidate_v1.py`
- `src/probe_headless/tests/test_mira_relay_trust_exact_verifier.py`

本次只证明 candidate fit 可以独立复核，不能证明 MIRA-Relay 的选择收益，也不改变当前
L-BFGS preregistration。进入 replay 前仍需对 candidate 优化器单独形成决议并完成至少跨折、
多 seed 的 candidate verifier 收据。
