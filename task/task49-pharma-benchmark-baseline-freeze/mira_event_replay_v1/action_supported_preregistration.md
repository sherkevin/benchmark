# Action-supported failure replay v1

日期：2026-09-30
状态：预注册；尚未运行。

## 目的

`full_v2` 的 fit-only Best Fixed target 在 event tick 很少是当前选择，导致 transient kernel 对 MIRA 没有实际 failure exposure。本协议只修正 intervention support，不改输入、方法或反馈权限。

## Target 生成规则

对每个 `(method, fold, seed, delay)`，先在 frozen no-event panel 上按原顺序运行到固定 `event_tick=floor(n_evaluation/2)`。使用同一 fit-only direct prior、feature snapshot、provider menu、feedback due rule 和 method hyperparameters；只消费 event tick 之前已经释放的 selected-only feedback。`target_provider` 定义为该方法在 event tick 的实际 argmax provider。

- target manifest 由 prefix replay 独立生成，包含 method/fold/seed/delay/event_tick/target_provider、prefix event IDs、prefix event SHA 和 source/snapshot hashes；生成后冻结。
- target 生成不读取 event tick 之后的 terminal、accuracy 或任何候选结果；不能根据 full replay 的结果挑 target。
- 同一 method/fold/seed/delay 的 transient 与 persistent stream 使用同一 target manifest。不同方法可以有不同 target，因此不能比较未经归一化的跨方法 raw post accuracy。

## Event kernels

- `transient_failure`：在 event tick 对 manifest target 的 selected call 返回一次 runtime failure，下一 tick 恢复原 frozen panel。
- `persistent_failure`：从 event tick 起对 manifest target 的每次 selected call 返回 runtime failure。
- 未选中的 provider、分子特征、truth、menu 和其他 provider output 不变；runtime failure label 和 chemical correctness 均为 null。

如果某方法在 event tick 的实际 action 与 manifest target 不一致，stream 直接标为 engineering failure，不能静默改 target 或继续统计。

## 矩阵和对照

- 方法：Frozen、streak-only、relative-reliability、matched Online-SGD；fold `{0..4}`、seed `{0,1}`、delay `{0,1,4}`。
- 主矩阵：`2 kernels × 4 methods × 5 folds × 2 seeds × 3 delays = 240 streams`。
- no-event control 使用同方法同坐标的 prefix/no-event receipt；不重复解释 stationary denominator。
- 辅助共同目标矩阵：Frozen event-tick incumbent，只有实际选择该 target 的 stream 才计算事件 response；无 exposure 写入表格，不补造 failure。

## 主指标

对每个 method/fold/seed/delay 配对：

1. transient overall/post accuracy 相对同坐标 no-event control；
2. persistent post-event accuracy 相对同坐标 no-event control；
3. 首次 target failure release 后 target exposure；
4. release-to-switch tick、failure count、selected-only cost。

跨方法只比较配对 delta、target exposure 和调用成本，并分层报告 target provider identity；不把不同 target 的 raw accuracy 直接写成方法 superiority。

## 固定反证条件

- prefix replay 无法重现 manifest target，或 event tick action 不一致：engineering failure，整批不解释。
- transient 中 relative 相对其 no-event control 下降超过 1 pp：当前保守性不成立。
- persistent 中 relative 的 post recovery 相对其 no-event control 在至少两个 delay 低于 streak-only 超过 1 pp：relative factor 没有保留 failure recovery。
- relative 在 persistent 首次 failure release 后仍保留明显 target exposure，而 matched 归零：保留结果，进入独立 runtime-failure response channel 设计，不把 chemical correctness ledger 解释成快速恢复。

只有 prefix/target manifest、240 条 raw stream、独立 schema/evaluator verifier 和 relative action/state replayer 全部通过，且前两条反证条件不触发，才考虑 A800 full matrix。confirmation 继续关闭。
