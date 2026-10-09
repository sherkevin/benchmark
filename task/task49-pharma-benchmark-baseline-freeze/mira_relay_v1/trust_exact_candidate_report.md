# MIRA-Relay trust-exact 候选 fit 收据

日期：2026-10-03
状态：**候选已独立验证；未进入 online replay，也未改变生效 preregistration。**

## 收据

- fold 0 / seed 0；444 fit rows、33 providers、7 families、64 维 frozen encoder。
- 数学目标与当前 preregistration 相同：summed Bernoulli NLL + `0.5 * 0.01 * (||U||² + ||b||²)`；direct provider head 是冻结 offset。
- `trust-exact`，解析 Hessian，`maxiter=100`，候选停止阈值 `gtol=1e-6`。
- SciPy 原始状态：`success=true`、`status=0`，12 次迭代。
- 目标值：`3340.106321841232`；梯度无穷范数：`2.326670661689789e-09`。
- 独立 verifier 重算得到相同 objective、gradient、参数 hash；Hessian 对称误差 `1.1369e-13`。

## 文件

- 生成器：`src/probe_headless/run_mira_relay_trust_exact_candidate_v1.py`
- 独立 verifier：`src/probe_headless/verify_mira_relay_trust_exact_candidate_v1.py`
- 原始参数与收据：`experiments/logs/task49_mira_relay_fit_diagnostic_20261003/trust_candidate_v1/`
- 其中 `candidate_parameters.npz` 只含 fit 参数向量；`verification.json` 为独立重算结果。

## 边界

这只证明同一个 fit 目标可以被稳定求解并被独立恢复。它没有读取 evaluation/confirmation label，没有执行 provider replay，没有证明选择收益、在线适应收益或安全性。

在 fold/seed 矩阵、同 schedule 对照和 preregistration 更新完成前，trust-exact 仍是候选数值实现，不能替代当前 L-BFGS-100 合同，也不能把更低 fit objective 写成方法效果。
