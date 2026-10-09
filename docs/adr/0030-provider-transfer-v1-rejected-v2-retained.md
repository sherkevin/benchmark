# ADR-0030：低秩 provider-transfer 淘汰，scalar transfer 暂留

- 状态：Accepted
- 日期：2026-09-27
- 依据：Task49 provider-transfer replay v2

## 背景

provider residual information screen 在五个 outer folds 都显示了 held-out transfer 信号，但这不保证任意参数化都能把信号转成选择收益。我们用已恢复的真实冻结 BT prior、64 维 hidden features 和同一 delayed selected-only 反馈做了不占 GPU 的 replay。

## 结果

- 低秩共享状态 $W h(x)$：delay 0/1/4 为 `0.8893/0.8893/0.8886`，低于 Frozen `0.8914`。
- scalar provider-transfer：`0.8955/0.8941/0.8927`，三个 delay 都高于 Frozen，但低于 independent bias `0.8983/0.8948/0.8989`。
- provider 行置换后的 scalar transfer 为 `0.8893/0.8893/0.8907`，真实 fit-only provider 几何有可观察差异。

## 决策

1. 不再把低秩 $W h$ 写成主方法，也不为它申请 A800 调参。
2. scalar provider-transfer 只保留为信息利用对照；它尚未达到现代 baseline 竞争线。
3. 如继续，只做一次预注册的安全门版本：独立 selected-arm 更新作为主状态，只有 selected feedback 足够意外且当前选择 margin 足够小才施加 provider transfer。若该版本仍不超过 independent 或 Best Fixed，停止扩展 provider-transfer 故事。

证据：`task/task49-pharma-benchmark-baseline-freeze/provider_transfer_replay_v2/report.md`、`experiments/logs/task49_provider_transfer_replay_20260927/v2/summary.json`。
