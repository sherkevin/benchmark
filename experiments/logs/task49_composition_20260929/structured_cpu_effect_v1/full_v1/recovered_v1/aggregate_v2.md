# Structured CPU effect v2：回放恢复后的保守聚合

v2 先通过独立 replay 恢复事件，再以 scaffold 为全局 cluster 重采样；同一实体在 seed/delay 重复观察保持在同一 cluster 内。该结果仍是 CPU development pre-screen。

## 完整性

- streams: 180; sample events: 25992; replay/event/input hash 已写入 JSON。
- 失败/unknown 在 primary accuracy 中计 0；valid-return accuracy 单独报告。

## 方法汇总

| method | delay | n | accuracy | valid-return accuracy | failures/unknown |
|---|---:|---:|---:|---:|---:|
| correctness_only | 0 | 1444 | 0.8878 | 0.8953 | 12 |
| correctness_only | 1 | 1444 | 0.8940 | 0.9015 | 12 |
| correctness_only | 4 | 1444 | 0.8996 | 0.9071 | 12 |
| frozen_structured | 0 | 1444 | 0.8982 | 0.9057 | 12 |
| frozen_structured | 1 | 1444 | 0.8982 | 0.9057 | 12 |
| frozen_structured | 4 | 1444 | 0.8982 | 0.9057 | 12 |
| matched_online_sgd | 0 | 1444 | 0.8913 | 0.8987 | 12 |
| matched_online_sgd | 1 | 1444 | 0.8885 | 0.8959 | 12 |
| matched_online_sgd | 4 | 1444 | 0.8968 | 0.9037 | 11 |
| no_integration | 0 | 1444 | 0.8906 | 0.8980 | 12 |
| no_integration | 1 | 1444 | 0.8920 | 0.8994 | 12 |
| no_integration | 4 | 1444 | 0.8913 | 0.8987 | 12 |
| structured | 0 | 1444 | 0.8857 | 0.8932 | 12 |
| structured | 1 | 1444 | 0.8927 | 0.9001 | 12 |
| structured | 4 | 1444 | 0.8961 | 0.9036 | 12 |
| truth_only | 0 | 1444 | 0.8975 | 0.9050 | 12 |
| truth_only | 1 | 1444 | 0.8989 | 0.9064 | 12 |
| truth_only | 4 | 1444 | 0.8975 | 0.9050 | 12 |

## Structured − matched online SGD

- `delay0`: pooled delta `-0.005540`, positive fold/seed pairs `3/10`.
- `delay1`: pooled delta `0.004155`, positive fold/seed pairs `5/10`.
- `delay4`: pooled delta `-0.000693`, positive fold/seed pairs `4/10`.

## Global scaffold-cluster bootstrap

- clusters: `382`; replicates: `10000`; seed: `20260928`
- point delta: `{'delay0': -0.00554016620498615, 'delay1': 0.004155124653739612, 'delay4': -0.0006925207756232687, 'all_delays': -0.0006925207756232687}`
- 95% CI: `{'delay0': [-0.015256628769380035, 0.004005689257702656], 'delay1': [-0.004406054058279314, 0.013418197588442723], 'delay4': [-0.008333333333333333, 0.008648053990035117], 'all_delays': [-0.0062936307335434176, 0.005376524985889808]}`
- P(delta>0): `{'delay0': 0.1088, 'delay1': 0.8072, 'delay4': 0.4104, 'all_delays': 0.3868}`

This uncertainty estimate addresses repeated seed/delay observations; it does not reopen confirmation or change the preregistered CPU decision.
