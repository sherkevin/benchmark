# Online policy order-sensitivity diagnostic v1

日期：2026-09-29
状态：已完成，development diagnostic；不进入主结果、confirmation 或 A800。

## 目的

前一轮 full-panel block audit 没有发现 evaluation 顺序本身形成 order-only drift。这里进一步固定同一 fold、seed、fit-only warm start 和 145 个 evaluation 分子，只打乱到达顺序，直接检查带状态的 selected-only policy 是否会因为更新顺序改变结果。

## 冻结协议

- 输入：`fingerprint_diagnostic_v1/inputs.json`，SHA-256 为 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- fold/seed：fold 0、seed 0；原始顺序加 19 个由 RNG seed `20260929` 生成的 within-fold permutation。
- warm start：复用已核验的 `structured_cpu_effect_v1/full_v1/run/fit_snapshots/fold0_seed0.npz` 与 `fold0_seed0_bt_cls.pt`，没有重新拟合。
- 方法：`frozen_structured`、`correctness_only`、`structured`。
- feedback delay：0、4；只把 selected provider 的结果和延迟 feedback 交给 policy。
- 没有把 truth、未选 provider 或完整 panel 交给 policy；terminal 记录不含 correctness。

## 结果

| delay | 方法 | 原始顺序 accuracy | 19 个随机顺序均值 | 原始减均值 | 原始相对随机顺序分位 |
|---:|---|---:|---:|---:|---:|
| 0 | frozen_structured | 0.8690 | 0.8690 | 0.0000 | 1.00 |
| 0 | correctness_only | 0.8345 | 0.8599 | -0.0254 | 0.16 |
| 0 | structured | 0.8276 | 0.8581 | -0.0305 | 0.11 |
| 4 | frozen_structured | 0.8690 | 0.8690 | 0.0000 | 1.00 |
| 4 | correctness_only | 0.8966 | 0.8704 | +0.0261 | 0.95 |
| 4 | structured | 0.8828 | 0.8675 | +0.0152 | 0.79 |

相对 Frozen 的 paired 差值为：

- delay 0：Structured 原始 `-0.0414`，随机均值 `-0.0109`；Correctness-only 原始 `-0.0345`，随机均值 `-0.0091`。
- delay 4：Structured 原始 `+0.0138`，随机均值 `-0.0015`；没有出现同样的低尾异常。

## 核验

共 120 条 stream、52,200 个 raw events。独立脚本从 raw terminal 事件和冻结输入重算 selected reward，复核输入 hash、decision/terminal 对齐、provider identity、order roster 和 accuracy，全部通过：

```text
src/probe_headless/verify_structured_cpu_effect_v1.py
src/probe_headless/analyze_order_sensitivity_diagnostic.py
```

原始证据在：

- `experiments/logs/task49_composition_20260929/order_sensitivity_diagnostic_v1/run_v1/events.jsonl`
- `experiments/logs/task49_composition_20260929/order_sensitivity_diagnostic_v1/run_v1/config.json`
- `experiments/logs/task49_composition_20260929/order_sensitivity_diagnostic_v1/run_v1/orders.json`
- `experiments/logs/task49_composition_20260929/order_sensitivity_diagnostic_v1/run_v1/verification.json`
- `experiments/logs/task49_composition_20260929/order_sensitivity_diagnostic_v1/analysis.json`
- `experiments/logs/task49_composition_20260929/order_sensitivity_diagnostic_v1/analysis.md`

关键文件 SHA-256：`events.jsonl=04af4bbbc1035d1e06054f746b8758deb5623b71e23115f146452ff4581207ec`，`verification.json=96d69baa6c31d837068422746911ac51e0ff032b8a6266306d1cf6f09475c923`，`analysis.json=750b97b55ac08bc5b630015b9544cedaad45040f3532724ee7ba756b9a29c9d7`。

## 结论和边界

evaluation 顺序不是数据层面的 order-only change point；已有 block permutation audit 仍然支持这一点。但在一个 fold/seed 的 delay=0 重放中，原始顺序会放大 provider head 的负更新，Correctness-only 与 Structured 同向，说明这更像 selected-only provider head 的更新方差问题。delay=4 没有复现同样的低尾异常。

这个诊断不能证明方法效果，也不能把一次 fold/seed 的顺序放大推广成总体结论。后续主实验必须固定并报告到达顺序，同时加入顺序敏感性和 selected-only provider support 的统计；方法修复优先控制 provider head 的高方差更新，而不是引入未经证实的 regime switch。
