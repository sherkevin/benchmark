# Gated provider-transfer replay v3

## 这次测什么

v2 的 scalar provider-transfer 有 fit-only 几何信号，但低于 independent bias。v3 保留 selected arm 的 independent 更新，只在反馈足够意外且选择 margin 足够小时，追加 off-diagonal provider transfer：$|p-r|\ge0.25$ 且 margin $\le0.2$。参数和阈值预先固定，没有看 evaluation 调整。

结果在 `experiments/logs/task49_provider_transfer_replay_20260927/v3_results.json`。

## 结果

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.8914 | 0.8914 | 0.8914 |
| Independent bias | **0.8983** | **0.8948** | **0.8989** |
| scalar transfer v2 | 0.8955 | 0.8941 | 0.8927 |
| gated transfer v3 | 0.8941 | 0.8948 | 0.8934 |
| gated transfer with permuted graph | 0.8941 | 0.8934 | 0.8893 |

gated transfer 没有超过 independent bias：三个 delay 中两个低于它，delay 4 低 `0.0055`。因此“provider residual graph + 一个简单安全门”仍不足以构成主方法。置换图在 delay 4 更差，但 delay 0 与真实图相同，不能把这个差异写成稳定的图结构收益。

## 裁定

停止 provider-transfer 主线的继续扩展。保留 information screen、低秩失败、scalar replay 和 gated replay 作为负证据：有 provider 间可迁移的标签信息，但把它直接写成全局 bias 或 gate 传播，不能稳定打过独立 selected-only 更新。

下一步转向另一种信息结构：只把已释放的 selected feedback 保存在有界的 context residual memory 中，让相似未来分子获得局部修正。它需要单独预注册，不能把本次 provider graph 的失败包装成动态 JEV 成功。

confirmation 集继续关闭。
