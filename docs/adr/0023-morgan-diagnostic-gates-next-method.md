# ADR-0023：静态指纹诊断不升为方法，先转向真实表示与 selected-only 更新

- 状态：Accepted
- 日期：2026-09-26
- 依据：Task49 指纹诊断 v1 与 calibrated switch 分析

## 背景

当前主任务是给定分子、性质和具体 provider，先选一个工具，终止后收到该次选择的 0/1 system-success 反馈，再服务后续请求。Morgan 指纹诊断使用 722 个 BBB development 分子和 33 个具体 provider，按 scaffold 做 outer evaluation。

诊断中 train-only Best Fixed 的准确率为 `0.9155`。线性 BCE、MLP BCE 和 BT+CLS 都低于它；MLP 两个 seed 的准确率为 `0.8961/0.8906`。fixed 错误的 61 个分子中有 59 个存在可救 provider，但 MLP 同时损害了不少 fixed 正确选择。随后按 validation 选择阈值的 calibrated switch 仍为 `0.8947/0.9100`，10 个 outer fold 没有一个超过 fixed。

## 决议

1. 不把 Morgan + 普通离线 head 命名为 JEV，也不把它作为动态 JEV 的有效 backbone 或正向方法证据。
2. 不继续在这批 Morgan checkpoint 上调阈值、扫参或打开最终确认集。
3. 下一步先完成 Laya 的无标签表示提取，再在同一 selected-only、延迟反馈和更新预算合同下比较 Frozen、Online SGD、Replay SGD、PILOT 和 BayesianRouter。
4. 只有当这些普通近邻不能解释观察到的收益时，才提出新的动态表示更新机制；若它们已解释全部收益，就收窄创新主张。

## 理由

这条顺序把三个问题分开：表示是否能区分 provider 适用性，离线选择是否能安全离开强固定 provider，题后反馈是否带来额外在线收益。当前数据只支持 oracle headroom，不支持前两个问题已经解决，更不支持在线方法有效。继续在同一表示上调阈值会把 development 结果变成选择性报告，不能增加论文证据。

## 后果

- 论文摘要继续只描述问题和评价设计，不写未经确认的算法收益。
- BBB 继续作为 development 数据；hERG/AMES 真值和 frozen confirmation 仍未打开。
- Laya 资源入口是下一项工程依赖；模型目录不可达时，先推进无模型的 baseline adapter 和反馈流合同，不提交重复 GPU 任务。
- 本 ADR 不否定动态 JEV 方向，只要求新机制在表示和普通在线基线之后接受检验。

## 证据

- `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/report.md`
- `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/selective_switch_card.md`
- `experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_v2/summary.json`
- `experiments/logs/task49_fingerprint_diagnostic_20260926/selective_switch_mlp_v1/summary.json`
