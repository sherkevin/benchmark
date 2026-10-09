# Structured CPU effect v1：审计修正

日期：2026-09-28

这份修正不改原始 JSONL、原始 verifier 输出或 preregistration；它说明哪些旧叙述不能继续使用。

## 1. 数值门槛的地位

运行前的 `preregistration.md` 固定了输入、信息合同、方法、延迟、日志和停止条件，但没有写入 `+0.01` 或 `4/5`。这两个数字来自 proposal 的开发资源判据，不能称为预注册统计门槛。报告改称“开发期判据”，结果仍然没有满足它们。

## 2. 旧 bootstrap 的地位

原聚合器把同一分子的多个 seed、delay 和方法结果放进独立重采样，所得区间存在伪重复风险。原始数值保留，改作描述性暂存；最终论文不能用它作为正式置信区间或显著性证据。后续分析必须按分子或 scaffold 做配对重采样，并明确在线轨迹相关性。

## 3. verifier 的地位

原 verifier 检查了字段、数量、绑定、延迟和版本递增等工程契约，但没有从初始状态逐事件重算每次 score、选择、梯度/更新和状态 hash。因此 `verified` 只代表契约检查通过，不代表算法状态已被数学上独立重放。独立 replay 是最终结果前的必要补件。

## 4. affine 诊断

affine retry_v2 没有完成，只有 fold0 部分事件，无 summary 和完整状态快照。它只作为中止尝试保存，不能支持任何关于截距或表示改动的效果判断。

## 5. 原始事件计数

原始 `events.jsonl` 共 78,166 行：25,992 decision、25,992 terminal、25,992 feedback、180 `stream_complete` 和 10 `fit_complete`。此前的 77,976 只指三类 sample events，不是完整 JSONL 行数；报告现已明确区分。

## 6. 独立回放补充

新的 recovery v2 在不导入 runner 的情况下增加了 score/menu/argmax 一致性检查，并重新生成 `recovered_v2/recovery.json` 与 `recovered_v2/aggregate_v2.json`。该检查通过，但仍只验证日志中的分数和选择一致，不能替代从 checkpoint 重新计算模型分数。
