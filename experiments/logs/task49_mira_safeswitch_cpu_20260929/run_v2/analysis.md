# MIRA-SafeSwitch CPU replay v1

状态：开发期 CPU 证据；不进入 confirmation、主表或 A800。

共 180 条 stream；结果从原始事件按 stream 重算。

## 聚合结果

| 场景 | 方法 | delay | accuracy | pre accuracy | post accuracy | post target rate | first-failure 后 target rate |
|---|---|---:|---:|---:|---:|---:|---:|
| controlled_event | frozen_direct | 0 | 0.8699 | 0.8917 | 0.8485 | 0.0715 | 0.1192 |
| controlled_event | frozen_direct | 1 | 0.8699 | 0.8917 | 0.8485 | 0.0715 | 0.1211 |
| controlled_event | frozen_direct | 4 | 0.8699 | 0.8917 | 0.8485 | 0.0715 | 0.1187 |
| controlled_event | matched_online_sgd | 0 | 0.8893 | 0.8750 | 0.9036 | 0.0069 | 0.0000 |
| controlled_event | matched_online_sgd | 1 | 0.8865 | 0.8764 | 0.8967 | 0.0083 | 0.0000 |
| controlled_event | matched_online_sgd | 4 | 0.8948 | 0.8861 | 0.9035 | 0.0137 | 0.0000 |
| controlled_event | mira_safe_switch | 0 | 0.8879 | 0.8903 | 0.8855 | 0.0221 | 0.0164 |
| controlled_event | mira_safe_switch | 1 | 0.8851 | 0.8889 | 0.8814 | 0.0249 | 0.0151 |
| controlled_event | mira_safe_switch | 4 | 0.8817 | 0.8875 | 0.8759 | 0.0304 | 0.0118 |
| no_event | frozen_direct | 0 | 0.9017 | - | - | - | - |
| no_event | frozen_direct | 1 | 0.9017 | - | - | - | - |
| no_event | frozen_direct | 4 | 0.9017 | - | - | - | - |
| no_event | matched_online_sgd | 0 | 0.8914 | - | - | - | - |
| no_event | matched_online_sgd | 1 | 0.8886 | - | - | - | - |
| no_event | matched_online_sgd | 4 | 0.8969 | - | - | - | - |
| no_event | mira_safe_switch | 0 | 0.8989 | - | - | - | - |
| no_event | mira_safe_switch | 1 | 0.8976 | - | - | - | - |
| no_event | mira_safe_switch | 4 | 0.8976 | - | - | - | - |

## MIRA 相对 Frozen 的配对差值

- `mira_minus_frozen/controlled_event/post_accuracy/delay0`: point `0.0370`, bootstrap95 `[-0.013774733637747349, 0.10277777777777775]`
- `mira_minus_frozen/controlled_event/post_accuracy/delay1`: point `0.0329`, bootstrap95 `[-0.016533485540334868, 0.09592846270928461]`
- `mira_minus_frozen/controlled_event/post_accuracy/delay4`: point `0.0274`, bootstrap95 `[-0.012404870624048736, 0.08232543759512932]`
- `mira_minus_frozen/no_event/accuracy/delay0`: point `-0.0028`, bootstrap95 `[-0.008333333333333349, 0.002064176245210725]`
- `mira_minus_frozen/no_event/accuracy/delay1`: point `-0.0042`, bootstrap95 `[-0.009027777777777779, 0.0]`
- `mira_minus_frozen/no_event/accuracy/delay4`: point `-0.0042`, bootstrap95 `[-0.008290229885057477, 0.0006800766283524995]`

## 解释边界

MIRA 只使用 fit-only direct prior 和已释放 selected feedback。controlled_event 是预先注入的 provider version degradation 正控，不是自然漂移；no_event 也只是冻结 development panel。bootstrap 单位是 fold/seed protocol slot，不能当作 iid seed 证据。

下一步根据 P1–P6 判据检查：无事件是否保持 Frozen 的 ROPE、事件后是否在至少两个 delay 下降目标 provider 选择率并改善 suffix accuracy，以及独立 replayer 是否逐事件一致。
