# Structured JEV CPU 预筛报告 v1

日期：2026-09-29  
状态：开发期负结果；未进入 confirmation，未提交新的 A800 效果任务。

## 运行范围

固定输入是 BBB development 的 722 个分子、33 个 provider 和 5 个 scaffold-disjoint outer folds。每个 fold/seed 使用已恢复的 `bt_cls` 1024→64 ReLU encoder；q、h 和 direct head 只用 fit 区 warm start，validation 只用于 best epoch。evaluation 只通过 selected-only evaluator facade 读取被选 provider 的一个 cell。

预注册配置为 6 个动态方法 × 5 folds × 2 seeds × 3 delays `{0,1,4}`，共 180 条流，25,992 个 evaluation decisions。反馈在 `decision_tick + 1 + delay` 释放；失败和 unknown 对结构化方法不生成恢复标签，A1 将失败记为 `system_success=0`。全量运行没有使用 confirmation 数据或 GPU。

## 证据完整性

- 180 条流均有 decision、terminal、feedback；原始 JSONL 共 78,166 行，其中 77,976 条是 decision/terminal/feedback sample events，另有 180 条 `stream_complete` 和 10 条 fit manifest。
- 原 verifier 状态为 `verified`，检查了 checkpoint/snapshot hash、事件守恒、provider 绑定、delay、terminal 信息边界、失败更新语义和 runtime 字段；它不是逐事件重放器，不能单独证明每次参数转移都正确。
- 25,992 个 sample event 由独立聚合器从 terminal identity 与 delayed feedback join 得到；primary accuracy 将 failure/unknown 计为 0。
- 现有日志没有发现未选 provider、完整 truth 或未来 correctness 被传给 policy 的字段；独立逐事件回放仍是进入最终论文前的待办。

## 主要结果

| 方法 | delay | primary accuracy | valid-return accuracy | failure/unknown |
|---|---:|---:|---:|---:|
| Frozen structured | 0/1/4 | 0.8982 | 0.9057 | 12/1,444 |
| Matched online SGD | 0 | 0.8913 | 0.8987 | 12/1,444 |
| Matched online SGD | 1 | 0.8885 | 0.8959 | 12/1,444 |
| Matched online SGD | 4 | 0.8968 | 0.9037 | 11/1,444 |
| Truth-only | 0/1/4 | 0.8975/0.8989/0.8975 | 0.9050/0.9064/0.9050 | 12/1,444 |
| Correctness-only | 0/1/4 | 0.8878/0.8940/0.8996 | 0.8953/0.9015/0.9071 | 12/1,444 |
| No-integration | 0/1/4 | 0.8906/0.8920/0.8913 | 0.8980/0.8994/0.8987 | 12/1,444 |
| Structured (candidate) | 0 | 0.8857 | 0.8932 | 12/1,444 |
| Structured (candidate) | 1 | 0.8927 | 0.9001 | 12/1,444 |
| Structured (candidate) | 4 | 0.8961 | 0.9036 | 12/1,444 |

Fit-only Best Fixed `admetlab_3_0` 的 evaluation accuracy 按 fold 为 `0.8966, 0.8897, 0.9514, 0.9444, 0.8958`，合并为 `0.9155`；它是 fit-only reference，不属于 180 条 adaptive stream。

## 结果解释与开发期判据

候选 Structured 相对 Matched online SGD 的全体样本差值为 `-0.00069`。原始聚合器给出的 scaffold bootstrap 95% CI `[-0.00633, 0.00518]`、`P(delta>0)=0.388` 只能作为暂存的描述性结果：它把同一分子的多个 seed/delay/方法复制进了独立重采样，正式不确定性需要按分子或 scaffold 做成对重分析；旧数值不作为最终显著性证据。

本轮运行前的 preregistration 只固定了协议、信息边界和停止条件，并没有写入 `+0.01` 或 `4/5` 的数值 go-signal。`+0.01` 与 `4/5` 是运行前后根据 proposal 使用的开发资源判据，不能称为预注册统计门槛。按该开发判据，三个 delay 都没有达到 `+0.01`，按 seed/delay 的正 fold 数最高为 `3/5`，所以候选 Structured 暂不进入下一轮资源投入。

独立 replay 与保守聚合 v2 给出的 structured−matched 差值为：delay 0 `-0.005540`（95% CI `[-0.015257, 0.004006]`），delay 1 `+0.004155`（`[-0.004406, 0.013418]`），delay 4 `-0.000693`（`[-0.008333, 0.008648]`）；三个 delay 合并为 `-0.000693`（`[-0.006294, 0.005377]`）。区间按全局 382 个 scaffold cluster 成对重采样，属于开发期复核，不改 preregistration，也不构成 confirmation 统计结论。

这不是“动态 JEV 已经失败”的泛化结论，而是本次固定表示、fit-only warm start、一步在线更新和全概率积分的具体候选没有显示可重复增益。不能据此打开 confirmation 或提交 A800 效果任务，也不能通过事后调学习率、探索率或表示来改写结果。

## 计算代价

全量日志保留每个 decision/update 的运行时间。按 method/delay 汇总时，Structured 的 decision p50 约为 `20–27 μs`，update p50 约为 `13–16 μs`；这些是当前 macOS CPU 开发环境的工程记录，不是跨硬件实时性结论。最终实时主张仍需在固定 A800 环境用同一 runtime schema 复核。

## 下一步

先做负结果诊断：区分“恢复标签没有信息”“在线更新过快导致 warm-start 漂移”“q/h 全概率积分的结构不适配”以及“没有统一探索导致 provider coverage 不足”。下一候选必须在相同输入和信息合同下预登记，优先采用有明确数学理由的先验锚定/保守更新或可恢复统计量；若不能在 CPU 上超过 matched baseline 或给出明确的成本/恢复优势，就停止该候选，不提交 A800。

## 可复核入口

- 预注册：[preregistration.md](preregistration.md)
- 原始事件：`experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/run/events.jsonl`
- 独立复核：`experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/run/verification.json`
- 独立聚合：[aggregate.md](../../../experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/aggregate.md)
- 聚合 JSON：`experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/aggregate.json`
