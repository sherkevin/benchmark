# Context residual memory replay v1

## 这次测什么

provider-transfer 的全局传播没有超过独立更新，所以换成局部信息结构：每个 selected provider 保存已经释放的 `(hidden feature, frozen residual)`，未来只对同一 provider 的相似分子读取 top-5 cosine 邻居。记忆容量 64，cosine 非负，score correction clip 到 `[-0.5,0.5]`。随机 memory 使用同样的容量和反馈，只随机取历史记录。

回放使用 matched-SGD 已恢复的真实冻结 BT prior 和 64 维 hidden features；没有新训练、新工具调用或 confirmation。结果在 `experiments/logs/task49_context_memory_replay_20260927/results_v2.json`，汇总在 `summary_v2.json`，配置在 `config_v1.json`。

## 结果

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.8914 | 0.8914 | 0.8914 |
| Independent bias | **0.8948** | 0.8948 | 0.8927 |
| Random memory | 0.8920 | 0.8948 | 0.8907 |
| Context residual memory | 0.8927 | **0.8969** | **0.8976** |

context memory 在 delay 1/4 超过 independent bias `0.0021/0.0049`，也超过随机 memory；delay 0 低于 independent `0.0021`。真实相似邻居的收益不是简单保存历史就能得到的，至少在这条冻结 hidden 表示上出现了局部 residual 结构。

## 现在能说什么

这通过了候选方法的零机时筛选，但还不是正式效果结论。它没有在所有 delay 都超过 independent，也还没有和 Best Fixed、完整 Online-SGD、Replay-SGD、BayesianRouter-style 在同一正式任务表中比较。memory 的成本也要计入完整 hidden 编码、相似度读取和 FIFO 维护，不能只报一次小矩阵查询。

## 下一步

把方法卡冻结成 `context_memory_method_card_v1.md`，接入统一 runner，先跑一条小 smoke 验证事件、延迟和 memory snapshot 的独立恢复；然后再按同一 development 协议做 5-fold、2-seed、3-delay 对照。confirmation 集保持关闭。若正式 runner 中两个以上 delay 仍不低于 independent，才继续做多端点和论文主表。
