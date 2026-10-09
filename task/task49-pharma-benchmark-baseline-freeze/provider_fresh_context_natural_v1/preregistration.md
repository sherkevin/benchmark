# Provider-fresh natural MIRA-Context confirmation v1

日期：2026-10-06  状态：冻结，执行前。

目的：补齐方法一致性证据。此前 fresh natural receipt 运行的是已经停止的 MIRA-Relative，controlled drift receipt 运行的是 MIRA-Context；本批只把 MIRA-Context 加入同一自然 fresh 合同，不改变数据、阈值、先验、baseline 或统计口径。

固定合同：157 个 scaffold-disjoint BBB candidate；四个 sealed provider checkpoints；5 个 development scaffold folds；seeds `{0,1}`；delays `{0,1,4}`；selected-only release at `decision_tick + 1 + delay`；one call per molecule；Morgan64 plus intercept selector feature；prior count `n0=20`；cold start exactly Frozen；only released selected correctness updates the selected provider residual.

比较：MIRA-Context 与 Frozen 的 paired correctness、scaffold bootstrap CI、action changes、rescued/harmed、selection/update p50/p95。已有 natural receipt 的 Random、Best Fixed、MIRA-Relative、Matched SGD、LinUCB、BayesianRouter、Retrospective Best Fixed 作为同合同参照，不重写或删除旧收据。

停止规则：不满足事件守恒、信息隔离、独立回放或 checkpoint hash 时批次作废；不根据本批结果调参数、改分母、改 candidate menu 或替换 baseline。自然结果无论正负都保留；其作用是确定 MIRA-Context 在 stationary fresh menu 上是否退化为 Frozen 或产生额外损失。
