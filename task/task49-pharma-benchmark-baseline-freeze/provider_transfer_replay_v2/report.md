# Provider-transfer replay v2

## 这次测什么

v1 的低秩共享状态 $W h(x)$ 在真实冻结 BT 先验上三个 delay 都低于 Frozen，所以不再调它。v2 换成一个更小的 provider-level residual graph：状态是 $m\in\mathbb R^K$，fit-only 残差协方差的第 $a$ 列把 selected provider $a$ 的反馈转移到其它 provider。这个回放仍然不训练、不调用新工具，使用 matched-SGD 任务已经恢复的 `(fold, seed)` 冻结 prior 和 64 维 hidden features。

完整结果在 `experiments/logs/task49_provider_transfer_replay_20260927/v2/results.json`，汇总在 `summary.json`。配置和输入 hash 在 `config.json`。

## 结果

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.8914 | 0.8914 | 0.8914 |
| Independent bias | 0.8983 | 0.8948 | 0.8989 |
| 低秩 $W h$ v1 | 0.8893 | 0.8893 | 0.8886 |
| 低秩置换 v1 | 0.8886 | 0.8886 | 0.8886 |
| scalar provider-transfer v2 | **0.8955** | **0.8941** | **0.8927** |
| scalar 置换 v2 | 0.8893 | 0.8893 | 0.8907 |

scalar v2 在三个 delay 都高于 Frozen，真实 transfer matrix 也 consistently 高于 provider 行置换；这说明 fit-only provider 几何确实进入了选择轨迹。但它三个 delay 都低于独立 bias，尤其 delay 4 低 `0.0062`。因此目前只能说“有可用的跨 provider 信号”，不能说已经得到优于现代 baseline 的动态 JEV。

## 裁定

1. 删除低秩 $W h$ v1 作为主方法候选。它把正确的 provider 关系压进共享 context 参数后，反而低于 Frozen。
2. scalar provider-transfer v2 保留为候选诊断，但暂不申请完整 A800 方法任务；它还没有超过 independent selected-only baseline 或 Best Fixed。
3. 下一步只允许做一次有明确理由的安全门版本：保留 independent selected-arm 更新，只在反馈足够意外且当前选择 margin 足够小时才施加跨 provider transfer。门控参数必须提前固定，不能看 evaluation 调出来。

## 限制

这是同一 development 回放上的零训练诊断，不是最终效果实验。反馈只在 selected provider 释放后更新；完整 evaluation panel 只由回放 evaluator 用来生成该次 selected reward。结果仍需在统一 runner 的正式 baseline 表中复核，confirmation 集保持关闭。
