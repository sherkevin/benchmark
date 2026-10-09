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
| controlled_event | mira_safe_switch | 0 | 0.8941 | 0.8917 | 0.8966 | 0.0111 | 0.0000 |
| controlled_event | mira_safe_switch | 1 | 0.8900 | 0.8917 | 0.8883 | 0.0152 | 0.0000 |
| controlled_event | mira_safe_switch | 4 | 0.8844 | 0.8903 | 0.8787 | 0.0234 | 0.0000 |
| no_event | frozen_direct | 0 | 0.9017 | - | - | - | - |
| no_event | frozen_direct | 1 | 0.9017 | - | - | - | - |
| no_event | frozen_direct | 4 | 0.9017 | - | - | - | - |
| no_event | matched_online_sgd | 0 | 0.8914 | - | - | - | - |
| no_event | matched_online_sgd | 1 | 0.8886 | - | - | - | - |
| no_event | matched_online_sgd | 4 | 0.8969 | - | - | - | - |
| no_event | mira_safe_switch | 0 | 0.8976 | - | - | - | - |
| no_event | mira_safe_switch | 1 | 0.8962 | - | - | - | - |
| no_event | mira_safe_switch | 4 | 0.8962 | - | - | - | - |

## MIRA 相对 Frozen 的配对差值

- `mira_minus_frozen/controlled_event/post_accuracy/delay0`: point `0.0481`, bootstrap95 `[5.7077625570767235e-05, 0.10974172374429218]`
- `mira_minus_frozen/controlled_event/post_accuracy/delay1`: point `0.0399`, bootstrap95 `[-0.009455859969558611, 0.10036672374429187]`
- `mira_minus_frozen/controlled_event/post_accuracy/delay4`: point `0.0303`, bootstrap95 `[-0.016362728310502287, 0.08770976027397259]`
- `mira_minus_frozen/no_event/accuracy/delay0`: point `-0.0042`, bootstrap95 `[-0.009027777777777779, 0.0]`
- `mira_minus_frozen/no_event/accuracy/delay1`: point `-0.0055`, bootstrap95 `[-0.010397629310344861, -0.001379310344827589]`
- `mira_minus_frozen/no_event/accuracy/delay4`: point `-0.0055`, bootstrap95 `[-0.008295019157088124, -0.0027729885057471313]`

## 解释边界

MIRA 只使用 fit-only direct prior 和已释放 selected feedback。controlled_event 是预先注入的 provider version degradation 正控，不是自然漂移；no_event 也只是冻结 development panel。bootstrap 单位是 fold/seed protocol slot，不能当作 iid seed 证据。

下一步根据 P1–P6 判据检查：无事件是否保持 Frozen 的 ROPE、事件后是否在至少两个 delay 下降目标 provider 选择率并改善 suffix accuracy，以及独立 replayer 是否逐事件一致。
