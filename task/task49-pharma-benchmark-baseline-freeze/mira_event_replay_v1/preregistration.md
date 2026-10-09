# MIRA transient/persistent failure replay v1

日期：2026-09-30
状态：预注册；尚未运行。

## 1. 要回答的问题

上一轮 relative-reliability replay 把两种目标分开后，留下一个具体冲突：MIRA 在 no-event 流上接近 Frozen，但在 persistent provider failure 的 post-event suffix 上低于 matched Online-SGD。下一轮只回答：

> 当 selected provider 发生一次偶发 runtime failure 和持续 runtime failure 时，MIRA 的 selected-only 更新是否能同时保持 stationary 行为并在持续失败后恢复？

这是机制诊断，不是最终 confirmation，也不改变题目、MIRA 主张或 benchmark/baseline 决议。

## 2. 唯一主假设与最近邻

**主假设 H1：** availability hard channel 应区分偶发和持续故障：一次 transient failure 不应造成永久切换，persistent failure 应在反馈释放后降低目标 provider exposure。relative-reliability 的 correctness ledger 不应把 runtime failure 当成 chemical error。

最近邻是 matched Online-SGD：它把 selected runtime failure 作为 reward 0 进入同一个在线更新；因此它预计恢复更快，但可能在 stationary 流上付出更高代价。streak-only 是 MIRA 的机制消融，Frozen 是静态下界/稳定性控制。四者使用同一 fit-only snapshot、feature、provider menu、order、delay 和 evaluator。

## 3. 固定输入与信息边界

- 输入：`fingerprint_diagnostic_v1/inputs.json`，722 个 BBB development molecules、33 providers、5 个 scaffold-disjoint evaluation folds。
- 快照：复用 `experiments/logs/task49_mira_safeswitch_cpu_20260929/run_v2_fixed/` 的 20 个 fit/feature snapshots；不重新训练、不重新拟合、不调参。
- 每 fold 的 event target 固定为 fit-only Best Fixed provider；target 身份和 event tick 在运行前写入 `event_records.json`。
- event tick 固定为该 fold evaluation order 的 `floor(n/2)`；policy 不读取 event flag，只有 selected terminal 经过 `decision_tick + 1 + delay` 才释放。
- delays 固定为 `{0,1,4}`；反馈仍是 selected-only，未选 provider 输出不进入 policy。
- runtime failure 的 terminal label 为 null、chemical correctness 为 null、feedback status 为 `failure`；不得把该事件改写成 chemical error。

## 4. 事件 kernel

1. `transient_failure`：在 event tick 仅令 target provider 的一次 selected call 返回 runtime failure；从下一 evaluation task 起恢复原 frozen panel 输出。若该 tick 没选中 target，不补造一次选择，事件仍按固定 tick 记录。
2. `persistent_failure`：从 event tick 起，target provider 的每次 selected call 都返回 runtime failure；未选中 provider 的缓存输出不改变。

两种 kernel 只改变 evaluator 的 terminal 状态，不能改变分子特征、menu、truth、其他 provider 输出或事件时间。当前已有 controlled persistent event 作为同一 persistent kernel 的历史对照；本轮重新生成统一 raw receipt，以避免跨 writer 混表。

## 5. 方法与矩阵

方法固定为：

- `frozen`：fit-only direct prior，不更新；
- `streak_only`：只使用 MIRA availability failure streak，连续两次 selected runtime failure 才 hard block；
- `relative_reliability`：上一轮候选，保持 availability hard channel，chemical correctness 使用有界相对可靠性因子；runtime failure 不进入 correctness ledger；
- `matched_sgd`：selected-only Online-SGD，runtime failure 以 reward 0 更新。

矩阵为 `2 event kernels × 4 methods × 5 folds × 2 seeds × 3 delays = 240 streams`。no-event 不重跑，直接引用已有统一 receipt，避免用重复 replay 改写 stationary 分母。

## 6. 预注册指标

所有指标先按每条 stream 计算，再按 fold/seed 配对汇总：

- transient overall accuracy 相对 Frozen；
- persistent event 后 post-suffix accuracy 相对 Frozen；
- persistent 首次 target failure release 后的 target exposure；
- 首次 failure release tick、首次 hard block tick/reason、failure 分母；
- no-event stationary cost 直接读取既有 receipt，定义仍为 $A_{m,N,d}^{all}-A_{F,N,d}^{all}$。

主比较只报告：relative vs Frozen、relative vs streak-only、relative vs matched 的配对差值及 fold-level bootstrap 区间。overall accuracy 不能替代 persistent post-suffix accuracy；transient 与 persistent 不合并成一个事件平均分。

## 7. 证伪条件与处置

以下任一条件成立，就停止把当前 relative-reliability 版本推进到 A800 full matrix，并保留 raw 结果与失败解释：

1. transient 场景中 relative 相对 Frozen 下降超过 1 pp；说明当前 availability channel 对偶发失败不够保守。
2. persistent 场景中 relative 的 post gain 在至少两个 delay 低于 streak-only 超过 1 pp；说明连续乘性 correctness 修正没有保留已有 hard-switch 恢复能力。
3. persistent 场景在首次 failure release 后 relative 仍持续选择 target，而 matched 的差异仅来自一次 runtime-failure update；此时不能再用 correctness ledger 解释恢复优势，必须回到独立 failure-response channel 建模。
4. event kernel 改变了未选 provider、truth、feature、menu 或 feedback timing；该批标为 engineering failure，不解释效果。

只有在前两项均未触发、stationary cost 仍在预先登记的 ±1 pp ROPE 内，才进入一次固定版本 A800 full matrix 评估；这也不自动打开 confirmation。

## 8. 运行和证据合同

运行前必须写入 `config.json`：输入和 snapshot SHA、source commit、方法、event kernel、fold/seed/delay、Python/NumPy、硬件和 UTC 时间。运行中逐条写 `events.jsonl`，至少包含 decision、terminal、feedback release、menu、selected-only read-set、provider binding、due/release tick、version、state hash 和 event identity。运行后写 `summary.json`、`event_records.json` 和独立 NumPy/SciPy action/state replayer。

任何一条 stream 的时序、selected-only、状态或 provenance 校验失败，都不读取其 accuracy 进入结果表；失败目录必须保留。实验只使用本地冻结回放，不调用新的工具 API，不提交 Nebula 任务。

## 9. 反向检查

- 反证：若 transient 与 persistent 的相对顺序完全相同，说明事件 kernel 没有真正改变 terminal 语义或指标不足，不能支持 H1。
- 反证：若 relative 和 streak-only 逐事件 action/state 完全相同，说明本轮没有测到 correctness factor；应检查 selected scored return 是否存在，而不是宣称两机制等价。
- 反证：若 matched 在 no-event 与 event 两类均无 stationary cost，却又比所有方法更快恢复，应优先检查 reward、release clock 和 target exposure 的分母；不直接把它写成新方法失败。
