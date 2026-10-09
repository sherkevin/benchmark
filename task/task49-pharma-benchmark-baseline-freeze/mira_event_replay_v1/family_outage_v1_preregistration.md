# Action-supported family-outage replay v1

日期：2026-10-01
状态：协议草案；尚未运行。

## 1. 唯一问题

在不同 selector 共享同一 provider family outage 时，延迟 selected-only feedback 是否让后续动作离开故障 family？这是一项共同干预压力测试，用来补齐 method-conditioned incumbent 不能做跨方法比较的问题。

`dynamic_scope=feedback-adaptive`。family outage 是预先注入的 controlled event；不声明自然 provider drift、version shift、provider arrival/retire 或 environment-nonstationary。

## 2. 固定输入

- 输入、33-provider menu、五个 scaffold evaluation folds、两个 seed、delay `{0,1,4}` 和 20 个 fit/feature snapshots 与 action-supported v2 相同。
- 方法：`frozen`、`streak_only`、`relative_reliability`、`matched_sgd`。
- family registry：`task/task38-p01-provider-pool/external_qualification.json`，运行前封存 SHA-256。
- Frozen no-event prefix receipt：`experiments/logs/task49_mira_unified_raw_20260930/full_v1/`，只读取 event tick 之前的 provider action、version、state hash 和 released feedback identity，不读取 terminal truth、post-event action 或未选 provider 输出。

## 3. 共同 target family 的封存规则

对每个 `(fold, seed, delay)`，从 no-event `frozen` stream 的 `event_tick=floor(n/2)` action 得到唯一 `target_family F_c`。再对每个 method 的同一坐标记录 event tick action provider/family 和 `exposed_c,m = 1[action_family=F_c]`。

- `exposed=1`：该方法在 event tick 实际选择的 provider 属于 `F_c`，允许 selected-only 注入；transient 只在 event tick 失败，persistent 从 event tick 起每次选择 `F_c` 内 provider 都失败。
- `exposed=0`：该方法没有选择 `F_c`，不注入任何未选择 provider 的反馈，event stream 应与 no-event 完全相同；此坐标保留在 ITT 分母并标记 unexposed。
- family 不明、event tick action 与封存 manifest 不一致、或 prefix identity 不一致，整批记 engineering failure。

这个规则把共同干预定义为 family outage，而不是强行让每个方法调用同一个 provider。它保留 selected-only 边界，也让 matched 的未暴露坐标可以明确计入 ITT，而不是偷偷删掉。

另外在运行前固定共同支持集 `S={c: 四个方法在 event tick 都暴露于 F_c}`。现有 no-event prefix 计算得到 `|S|=23/30`，且全部是 `b3clf` family；该集合在运行前封存，不能按 event 结果挑选。跨方法主比较只在 `S` 上做，另报全 30 坐标 ITT；不把 event 后重新选择出的 exposed 子集当主结果。

## 4. 主估计量

先按 `(method, fold, seed, delay, scenario)` 聚合，再按 5 个 scaffold folds 做 cluster bootstrap。

1. **ITT release-after delta**：所有坐标的 event−no-event correctness 差值；unexposed 坐标的 event contribution 应为 0。
2. **Exposure**：`exposed_c,m` 的比例；必须和 ITT 同表报告。
3. **Exposed conditional delta**：只作补充诊断；只有预先封存的支持集 `S` 满足 `≥20/30` 且五个 fold 均有支持时才解释跨方法结果。其余 family cell 只列 coverage/点估计，CI 记 `NA`。
4. **Leave-family latency**：release 后首次选择不属于 `F_c` 的 tick；未离开右删失。
5. **Feedback blind window、注入 terminal loss、runtime failure、selection/update p50/p95、state bytes**：沿用 action-supported v2。

不把 ITT 或 exposed conditional delta 称为 provider-level superiority；它们只回答共同 family outage 下的适应性。

## 5. 证伪条件

- relative 在 persistent family outage 的共同支持集 `S` 上比 streak-only 低超过 `1 pp`，且五个 fold 的 cluster bootstrap 支持该差异；
- relative 在 transient family outage 的 ITT release-after delta 低于 `-1 pp`（至少两个 delay）；
- 任一方法的 unexposed stream 出现 failure、feedback release 或 prefix divergence；
- target family/provenance/hash、selected-only、event tick control-valid terminal、queue/update exactly-once 任一核验失败。

任何证伪条件触发时，不打开 confirmation；先回到 MIRA 更新规则和 baseline fairness contract。

## 6. 运行闸门

先做 4-stream smoke（fold0/seed0、delay4、两 scenario、relative/matched）；该坐标中 relative 是 exposed、matched 是 unexposed，独立 verifier 必须同时确认两种路径。smoke 通过后才允许一次 240-stream CPU receipt；不提交 Nebula/A800。
