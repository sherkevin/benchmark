# Random baseline full development replay

日期：2026-09-30
状态：development baseline；confirmation 关闭。

## 目的

补齐 Gate B 缺失的 Random 主表收据。Random 不是 Proposed 方法，只用于给 33-provider 菜单、失败处理和延迟反馈提供一个同合同下界。它和已有 baseline 使用同一 BBB development 输入、scaffold folds、selected-only feedback 和 delay `{0,1,4}`。

## 配置

- 722 个 BBB development molecules、33 个 provider、5 个 scaffold-disjoint evaluation folds。
- 2 个确定性 protocol slots × 3 个 delay，共 30 条 stream；每条 evaluation fold 144 或 145 个分子。
- RandomPolicy 使用 `seed=20260930 + 1009*fold + slot`，每次从当前完整菜单均匀采样；propensity 记录为 `1/|menu|`。
- 选择器收到冻结 BT/CLS 64 维 query representation，但 RandomPolicy 不读取其数值；反馈只在 release tick 到达。
- 无 GPU、无 LLM、新预测或 confirmation。

## 结果

独立 verifier 重新读取 raw JSONL、冻结 cache 和 inputs，核对 provider 输出、truth/judge、propensity、事件顺序、延迟和版本时钟。

| delay | streams | accuracy | decisions | feedback |
|---:|---:|---:|---:|---:|
| 0 | 10 | 0.844971 | 1,444 | 1,444 |
| 1 | 10 | 0.844971 | 1,444 | 1,444 |
| 4 | 10 | 0.844971 | 1,444 | 1,444 |

延迟不改变 Random 的动作分布，因此三个点相同是预期现象。这个结果明显低于 development Best Fixed `0.9155`，也低于当前 Greedy LinUCB `0.8511/0.8449/0.8629` 的三个点；它不支持任何方法优越性 claim。

## 收据

权威运行目录：`experiments/logs/task49_random_development_20260930/run_v1/`。

- raw events：`426eb9dd0c42459e693ee12d2ae144f85ec33868f76ea14802cf495b5356ae5d`
- config：`745d20ce478442708e0cb09df5e7b4be857a9a494e1d88d5404ae007cb5a77be`
- 独立 verifier：`verification.json`，30/30 streams `verified`
- 篡改第一条 propensity 后，`tamper_propensity_v1/expected_rejection.log` 以退出码 1 拒绝

## 边界

这是 Random baseline 的完整 development 收据，不是最终 confirmation，也不替代静态强 router、Laya 资格和多端点 benchmark 资格。下一步把该结果加入 development master table，并继续补 Gate B 的强 router fidelity。
