# Common-stratum 两流协议冒烟 v1

日期：2026-09-30  
状态：**协议验证通过；不是效果实验。**

## 做了什么

按封存的 common-stratum manifest，使用同一套 7 个方法、同一 direct prior 规则和 delay=1，跑了两个 scaffold fold 的小流：每个 fold 8 个 development evaluation 分子。没有读取 confirmation，也没有计算 online−frozen 效果。

| 流 | fold/seed | 分子数 | 事件数 | 真实 failure terminal |
|---|---:|---:|---:|---:|
| v4 | 0/0 | 8 | 168 | 0 |
| v5 | 1/0 | 8 | 168 | 21 |

v5 的 21 个 failure terminal 来自 7 个方法各自选择 `b3clf_dtree_common` 后遇到的 3 个真实缓存失败分子：`z001303`、`z001377`、`z001405`。它们均保留在事件和分母中。

## 验收

- 每个方法每题都有 decision、terminal、feedback，事件数守恒。
- 每次 selected output 都由独立 verifier 重新绑定到缓存记录。
- delay=1 的释放顺序、冻结方法参数不变、在线方法消费 feedback 均通过已有 verifier。
- 两个 fold 使用 manifest 绑定的 checkpoint/direct snapshot；confirmation 保持关闭。

## 机器证据

- v4：`experiments/logs/task49_common_stratum_smoke_20260930/v4/`
- v5：`experiments/logs/task49_common_stratum_smoke_20260930/v5/`
- aggregate verifier：同目录 `two_stream_verification_v1.json`，状态 `verified`。
- runner：`src/probe_headless/run_common_stratum_smoke_v1.py`。
- aggregate verifier：`src/probe_headless/verify_common_stratum_two_stream_smoke_v1.py`。

## 结论边界

这一步只关闭了“共同 prior、延迟反馈、真实失败和事件账本能否同时运行”的工程风险。样本太小，且两个 fold 只有一个 seed，不能据此比较方法优劣。下一步仍是完整 60-stream receipt，并在 receipt 中加入 paired uncertainty、更新时间/调用账本和预注册的结果判据。
