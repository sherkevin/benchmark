# MIRA-Relay fit 优化器跨折稳定性诊断

日期：2026-10-03
范围：fold 0–4 / seed 0 的 fit-only residual 初始化；不读取 evaluation labels，不运行 replay。

## 固定设置

沿用 `optimizer_diagnostic_report.md` 中的完全相同目标、direct-logit offset、33 provider、
7 family、64 维 frozen encoder 和 family registry。每个 fold 比较：

- `L-BFGS-B`，`maxiter=1000`，仅作为数值上限诊断；
- `trust-exact`，解析 481×481 Hessian，`maxiter=100`。

没有把任何一组结果写成效果结果，也没有改变 preregistration 中主优化器合同。

## 结果

| fold | L-BFGS-1000 objective / grad∞ | trust-exact objective / grad∞ | trust status | 时间（LB / trust，秒） |
|---:|---:|---:|---|---:|
| 0 | 3340.2848 / 0.3738 | 3340.1063 / 2.33e-9 | status 2 | 17.98 / 4.24 |
| 1 | 4368.7138 / 0.2203 | 4368.6638 / 2.45e-8 | status 2 | 19.74 / 3.95 |
| 2 | 4824.6652 / 0.4502 | 4824.5478 / 2.29e-7 | status 2 | 20.49 / 3.81 |
| 3 | 4482.7339 / 0.2075 | 4482.6329 / 1.19e-7 | status 2 | 19.90 / 3.77 |
| 4 | 4107.6766 / 0.3243 | 4107.5469 / 1.30e-10 | success | 20.57 / 3.99 |

trust-exact 每折都把梯度降到约 `2.3e-7` 或更低，并且目标值比 L-BFGS-1000 低
`0.05–0.18`。L-BFGS-1000 五折都达到迭代上限，最终梯度仍在 `0.21–0.45`。
解析 Hessian 优化耗时约 3.8–4.2 秒/折，L-BFGS-1000 约 18–20.6 秒/折。

SciPy 的 trust-exact 在前四折返回 status 2（`A bad approximation caused failure to predict
improvement`），但这些收据的梯度已经接近数值停止点；fold4 正常返回 success。状态字段仍
按原样保留，不能把 status 2 改写为成功。

## 判断

跨折结果支持前一轮判断：固定 100 次 L-BFGS 未收敛主要是优化预算和目标条件数问题；解析
Hessian 的计算在五个 fold 上稳定工作，没有出现维度、非有限值或 family 映射错误。它可以
作为后续独立 verifier 的数值参考，也可能作为新的 fit 实现候选，但目前不能未经决议替换
预登记的 L-BFGS 主合同。

如果要采用 trust/Newton 实现，需单独确定：优化器、停止条件、SciPy status 2 的接受规则、
参数 hash 和与当前 L-BFGS 对照方式，然后更新决议和 preregistration。当前 full replay 仍
保持关闭。

## 证据

目录：`experiments/logs/task49_mira_relay_fit_diagnostic_20261003/optimizer_crossfold_v2/`

- `optimizers.jsonl`：10 条逐折逐优化器原始收据和 callback 曲线；
- `summary.json`：汇总结果及 raw JSONL hash；
- `config.json`：输入、源码 hash、固定目标和运行环境；
- `test.log`、`test_config.json`：py_compile 与 diff 检查。

`optimizer_crossfold_v1/` 保留了第一次运行的同数值收据；v2 修正了外层
`record_status` 字段，保留 SciPy 原始数值 `status`（1 或 2），避免外层完成状态覆盖
优化器状态。

本轮没有使用 GPU、没有读 evaluation/confirmation labels、没有跑 full matrix、没有产生
superiority 或收益 claim。
