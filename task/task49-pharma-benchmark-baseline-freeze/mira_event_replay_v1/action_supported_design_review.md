# Action-supported event target design review

日期：2026-09-30
状态：下一轮协议草案；尚未运行。

## 现有问题

上一批把 event target 固定为每个 fold 的 fit-only Best Fixed provider，但该 provider 在 event tick 不是 Frozen、streak-only 或 relative-reliability 的当前选择。`full_v2` 中 transient 的 relative 首次 failure release 比例为 `0/0/0`，所以该批没有真正检验 relative 对偶发失败的行为。这个问题是 intervention support 不足，不是方法正负结果。

## 候选定义

### A. Fit-only Best Fixed target

所有方法共享同一个 target，最容易做横向比较，但不能保证 target 在 event tick 被任何 policy 选中。上一批已经证明它会让 transient 事件无效，因此不再作为主协议。

### B. Frozen event-tick incumbent

target 是 fit-only Frozen policy 在 event tick 对该 molecule 的 provider。它保证 Frozen 受到事件，但 adaptive policy 可能在 event 前已经离开该 provider。可作为共同 target 的 secondary stress；如果某方法没有选择 target，要报告 no exposure，不能把它当作恢复失败。

### C. Method-conditioned incumbent（推荐主协议）

对每个 `(method, fold, seed, delay)`，先在相同 no-event prefix 上运行到 event tick；target 固定为该 method 在 event tick 实际选择的 provider。然后从同一 event tick 重播 transient/persistent kernel。target 由运行前冻结的 policy action 和 event tick 决定，不使用 event 后结果，也不按效果挑 provider。

它保证每个方法的事件作用在自己的当前 incumbent 上，真正回答“该方法收到自己所选工具的失败后如何更新”。代价是不同方法可能承受不同 provider 的事件，不能把 raw post accuracy 直接当成跨方法同一干预下的排名。主比较应使用同方法的 event-minus-no-event 配对恢复曲线、target exposure 和 release-to-switch；跨方法只比较归一化后的恢复量和成本。

### D. 共同 provider 交集

只选所有方法在 event tick 都选择的 provider。该交集可能为空或样本很少，并且会以 action agreement 选择事件，统计功效差；不作为主协议。

## 推荐协议

采用 **C 为主、B 为辅**：

1. 主矩阵使用 method-conditioned incumbent；target manifest 在 event replay 前生成并 hash，按 method、fold、seed、delay 单独记录。
2. transient kernel 只在 event tick 令该 target 返回一次 runtime failure，随后恢复；persistent kernel 从 event tick 起持续失败。
3. 主指标是每个方法相对其自身 no-event prefix/同方法 no-event stream 的 post accuracy delta、首次 failure release 后 target exposure、release-to-switch tick 和 transient recovery。不能把不同 target 的 raw post accuracy 宣称成方法 superiority。
4. secondary 矩阵使用 Frozen incumbent 作为共同 target，保留同一 menu、features、delay 和 evaluator；没有选中 target 的方法标记 no exposure，不补造失败。
5. no-event prefix 和 target manifest 只能使用 event tick 前的反馈；target 选择规则在运行前固定，禁止查看 post-event accuracy 后挑 target。

## 需要额外记录的字段

`target_source=method_incumbent_prefix`、`target_method`、`target_fold`、`target_seed`、`target_delay`、`target_event_tick`、`prefix_events_sha256`、`target_manifest_sha256`。Verifier 需要重放 prefix，确认 target 等于 event tick 的实际 action，并检查 event 后第一次 terminal 的 failure status、due/release clock 和 selected-only boundary。

## 能支持和不能支持的 claim

- C 能支持“每个方法对自身当前 incumbent 的 delayed failure adaptation”以及 transient/persistent 两种 failure response 的机制比较。
- B 能支持“在共同 Frozen incumbent 事件下的压力测试”，但只能在实际选中 target 的 stream 上解释 post response。
- C 不能支持不同 provider 质量完全相同，也不能直接支持跨方法 raw accuracy superiority；需要 paired within-method deltas 和 provider identity 分层。
- 这两种 protocol 都不能把 synthetic event 写成自然 drift，也不能替代 provider lineage、endpoint equivalence、cost fairness 或 frozen confirmation。
