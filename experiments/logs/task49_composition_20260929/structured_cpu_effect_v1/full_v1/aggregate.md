# Structured CPU effect v1：全量预筛聚合

状态：CPU development pre-screen；不等同于 confirmation，也不支持跨端点泛化。

## 数据完整性

- streams: 180; sample events: 25992; input/events/summary hash 已写入 JSON。
- 结果由独立聚合器从 JSONL terminal identity 与 delayed feedback join 得到；primary accuracy 将 failure/unknown 计为 0。

## 方法汇总

| method | delay | n | accuracy | valid-return accuracy | failures/unknown | decision p50 ns | update p50 ns |
|---|---:|---:|---:|---:|---:|---:|---:|
| correctness_only | 0 | 1444 | 0.8878 | 0.8953 | 12 | 24011 | 14625 |
| correctness_only | 1 | 1444 | 0.8940 | 0.9015 | 12 | 23833 | 14604 |
| correctness_only | 4 | 1444 | 0.8996 | 0.9071 | 12 | 26750 | 16052 |
| frozen_structured | 0 | 1444 | 0.8982 | 0.9057 | 12 | 29552 | 8073 |
| frozen_structured | 1 | 1444 | 0.8982 | 0.9057 | 12 | 28021 | 7958 |
| frozen_structured | 4 | 1444 | 0.8982 | 0.9057 | 12 | 28114 | 7958 |
| matched_online_sgd | 0 | 1444 | 0.8913 | 0.8987 | 12 | 10625 | 10500 |
| matched_online_sgd | 1 | 1444 | 0.8885 | 0.8959 | 12 | 10448 | 10677 |
| matched_online_sgd | 4 | 1444 | 0.8968 | 0.9037 | 11 | 10229 | 10292 |
| no_integration | 0 | 1444 | 0.8906 | 0.8980 | 12 | 26636 | 18094 |
| no_integration | 1 | 1444 | 0.8920 | 0.8994 | 12 | 25292 | 17250 |
| no_integration | 4 | 1444 | 0.8913 | 0.8987 | 12 | 24854 | 16885 |
| structured | 0 | 1444 | 0.8857 | 0.8932 | 12 | 24083 | 16188 |
| structured | 1 | 1444 | 0.8927 | 0.9001 | 12 | 22771 | 14469 |
| structured | 4 | 1444 | 0.8961 | 0.9036 | 12 | 20166 | 13636 |
| truth_only | 0 | 1444 | 0.8975 | 0.9050 | 12 | 26958 | 9688 |
| truth_only | 1 | 1444 | 0.8989 | 0.9064 | 12 | 28281 | 10146 |
| truth_only | 4 | 1444 | 0.8975 | 0.9050 | 12 | 26490 | 9583 |

## Structured 对 matched online SGD

按 seed/delay 的五个 scaffold fold 配对差值：
- `seed0_delay0`: mean fold delta `0.0000`, positive folds `2/5`;
  fold0=-0.0069, fold1=0.0069, fold2=0.0069, fold3=-0.0069, fold4=0.0000
- `seed1_delay0`: mean fold delta `-0.0111`, positive folds `1/5`;
  fold0=0.0138, fold1=-0.0138, fold2=-0.0139, fold3=-0.0139, fold4=-0.0278
- `seed0_delay1`: mean fold delta `0.0069`, positive folds `3/5`;
  fold0=0.0069, fold1=-0.0069, fold2=0.0139, fold3=0.0278, fold4=-0.0069
- `seed1_delay1`: mean fold delta `0.0014`, positive folds `2/5`;
  fold0=-0.0414, fold1=0.0276, fold2=0.0278, fold3=-0.0069, fold4=0.0000
- `seed0_delay4`: mean fold delta `0.0097`, positive folds `3/5`;
  fold0=0.0276, fold1=-0.0138, fold2=0.0139, fold3=0.0347, fold4=-0.0139
- `seed1_delay4`: mean fold delta `-0.0111`, positive folds `1/5`;
  fold0=-0.0345, fold1=-0.0069, fold2=0.0069, fold3=-0.0208, fold4=0.0000

## Scaffold-cluster bootstrap

- paired streams: 30; replicates: 10000; seed: 20260928
- point structured−matched delta: `-0.0006925207756232687`
- 95% CI: `[-0.0063291496979254016, 0.005181376337658516]`; P(delta>0): `0.388`

Bootstrap 是开发期证据，不改变预注册的 go/kill 门槛；如需进入 confirmation，仍需冻结方法、补齐 baseline roster 和独立 confirmation。
