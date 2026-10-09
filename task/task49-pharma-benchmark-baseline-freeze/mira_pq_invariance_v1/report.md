# MIRA P/Q hidden-label invariance receipt

日期：2026-09-29
状态：通过协议检查；不是效果实验，也不支持 confirmation。

## 目的

检查 MIRA-SafeSwitch 是否偷读未选 provider 的标签。构造两个世界 P/Q：菜单、分子特征、选中 provider 的状态、selected feedback 和事件顺序完全相同；只有未选 provider 的标签在 P/Q 中反转。若动作或内部更新不同，候选违反 selected-only 信息边界。

## 固定配置

- provider：3 个；direct prior：`[[2,0],[1,0],[0.5,0]]`。
- `delta=0.05`、`n_min=20`、`tau_fail=0.20`、连续两次 runtime failure 才阻断。
- 5 个事件；其中前 3 个事件让 incumbent 经历一次成功和连续两次运行失败，后 2 个事件验证切换后的状态仍保持一致。
- P/Q 每个事件都有不同的 hidden unselected labels；这些字段只写入收据，未传给 policy。

## 结果

运行收据：`experiments/logs/task49_mira_pq_invariance_20260929/run_v1/`。

- 5/5 事件的 hidden labels 不同。
- P/Q 的 provider action、score vector、version 和 feedback state 逐事件完全相同。
- runner 状态 `passed`；独立 verifier 状态 `verified`。
- 篡改收据让第一个事件的 P/Q hidden labels 相同后，verifier 以退出码 1 拒绝：`tamper_hidden_v1/expected_rejection.log`。

## 证据哈希

| 文件 | SHA-256 |
|---|---|
| `config.json` | `50e6b69b6a85304e3c3847bfdb48c7c8ad0dc6a88f7555f4353ac343a46da1b1` |
| `events.jsonl` | `6920003210f463cd04513bfb6ae6002dd1ba5c9330839be417d4107451347c57` |
| `summary.json` | `d06370a95226f4604682b8b42b58bead888c394c76457e23c48401ed8fe45e7b` |

## 边界

这只证明当前实现没有使用收据中未传入的隐藏标签。它不证明 MIRA 在真实分子上的准确率、动态 provider drift 下的收益或统计显著性；这些仍由同合同 CPU matrix 和后续 confirmation 负责。

## 下一步

把 P/Q 作为方法闸门证据后，继续收口 Gate B 的强 baseline fidelity，并将 MIRA-SafeSwitch 与 matched Frozen/Online-SGD/Replay-SGD 放入同一合同的唯一 Proposed CPU matrix；未通过前不提交新的效果型 A800。
