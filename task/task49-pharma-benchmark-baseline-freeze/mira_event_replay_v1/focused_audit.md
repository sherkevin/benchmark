# Action-supported failure replay：v2 指标聚焦审计

日期：2026-09-30  
状态：只读审计；不改 runner、主方法或 v1 结果，不运行训练。

## 结论

`8%` 到 `1.4%` 的 relative persistent target exposure 差异不是由
feedback blind window 造成的。真正原因是 v1 分析器把事件前已经发生的
自然 runtime failure 当成了本次注入事件的第一次 failure release。

在 `delay=0` 时 blind window 长度是零，但旧值与 v2 值仍分别为 `8.0255%`
和 `1.4006%`。受影响的只有 fold 1 的两个 seed；它们的 target 在 event
tick `T=72` 之前已经有 failure release（`5/7/44` 或 `7/44`），所以 v1
从 tick 5 或 7 开始计算 exposure。把窗口锚定到 event failure 的 release
tick `R=73` 后，exposure 才回答“收到本次事件反馈后还选了多少 target”。

这说明 `metric_v2_reanalysis_report.md` 对 blind window 的解释不完整：
blind window 仍是指标混淆的一部分，但 `delay=0` 的主要差距来自**事件前
failure release 未被 event-bound**，不能写成“主要由 blind window 造成”。

## 1. 可复核来源

- event raw：`experiments/logs/task49_mira_event_replay_cpu_20260930/action_supported_full_v1/run/events.jsonl`
- v2 output：`experiments/logs/task49_mira_event_replay_cpu_20260930/metric_v2_reanalysis_v2/metrics_v2_readonly.json`
- 分母核对 JSON：`experiments/logs/task49_mira_event_replay_cpu_20260930/metric_v2_reanalysis_v2/exposure_denominator_audit.json`
- 本审计 JSON：`experiments/logs/task49_mira_event_replay_cpu_20260930/metric_v2_reanalysis_v2/focused_audit_v1.json`
- 旧分析器：`src/probe_headless/analyze_mira_event_replay_v1.py`
- v2 分析器：`src/probe_headless/analyze_mira_action_supported_metrics_v2.py`

权威 event SHA-256 为
`932b3d353f1465c77f5c96bf15496a52d8e727ffdb33f7b03b1a98cf2795cea9`。
核对 JSON 逐条保存了所有 relative persistent 流的 event tick、target、旧/新
release、分子、分母和 exposure。

## 2. 真实原因：旧窗口没有绑定事件

旧分析器先找到事件 tick：

```python
event_tick = int(summary["event_tick"])
```

随后却在整条流中寻找 target 的最早 failure release：

```python
for f in sorted(feedback, key=lambda p: int(p["release_tick"])):
    if f["provider_id"] == target and f["feedback_status"] == "failure":
        first_failure_release = int(f["release_tick"])
        break
```

它没有要求 `decision_tick >= event_tick`，也没有要求 failure 的 event ID
等于 event tick 的注入 terminal。v2 则从事件 tick 对应的 failure feedback
读取 release，并核对

$$
R=T+1+d.
$$

因此两套分母的含义不同：旧值是“该 target 在整条流第一次失败后被选中的
比例”，v2 值是“本次注入事件反馈释放后被选中的比例”。旧值不能用来回答
本次 persistent event 的恢复行为。

## 3. 逐流证据

| stream | $T$ | v1 first release | v2 event release | v1 exposure | v2 exposure | v1 denominator | v2 denominator |
|---|---:|---:|---:|---:|---:|---:|---:|
| fold1 seed0 delay0 | 72 | 5 | 73 | 37.14% | 1.39% | 140 | 72 |
| fold1 seed1 delay0 | 72 | 7 | 73 | 31.88% | 1.39% | 138 | 72 |
| fold1 seed0 delay1 | 72 | 6 | 74 | 37.41% | 2.82% | 139 | 71 |
| fold1 seed1 delay1 | 72 | 8 | 74 | 32.85% | 1.41% | 137 | 71 |
| fold1 seed0 delay4 | 72 | 9 | 77 | 37.50% | 1.47% | 136 | 68 |
| fold1 seed1 delay4 | 72 | 11 | 77 | 34.33% | 1.47% | 134 | 68 |

这 6 行是 3 个 delay 下的 2 个受影响 seed；其中每个 seed 在事件前已有
2 或 3 次 target failure release。其余 relative persistent 流的 v1 first
failure release 与 event-bound release 一致。

按 delay 汇总：

| delay | v1 exposure | v2 exposure | v1−v2 | blind 长度 | 事件前 failure release 流数/次数 |
|---:|---:|---:|---:|---:|---:|
| 0 | 8.0255% | 1.4006% | 6.6249 pp | 0 | 2 / 5 |
| 1 | 7.8788% | 1.2757% | 6.6031 pp | 1 | 2 / 5 |
| 4 | 8.2254% | 1.3367% | 6.8887 pp | 4 | 2 / 5 |

## 4. v2 实现检查

### 已通过

- 240 条 event/control 配对均能按 entity order 对齐。
- 每条 event stream 的 target 在 event tick 被实际选择。
- event terminal 是 failure；对应 failure feedback 唯一且 release tick 满足
  `T+1+delay`。
- `delay=0` 的 blind window 正确记为 `NA`，不是零。
- v2 的 `Delta_blind`、`Delta_adapt` 按 stream 先求均值，再按 10 个
  `(fold, seed)` slot 等权汇总。
- fold-cluster bootstrap 每次重采样 5 个 fold，同时保留每个被抽中 fold 的
  两个 seed；没有把 seed 当作独立 cluster。
- `release_to_switch` 没有把未切换伪装成有限延迟；代码存为 `None` 并标记
  `censored=True`。本批 relative 的 60 条 event streams 中，审查到的各流均在
  release 后切换，但未来完整表仍必须报告删失比例。

### 需要纠正的证据边界

1. **bootstrap seed 并未完全按 v2 preregistration 固定。** 预注册给出
   `20261000 + 1000*I[persistent] + 100*method_id + 10*delay`；实际分析器
   对 `delta_blind`、`delta_adapt`、`target_exposure` 又分别加了 `+1`、
   `+2`、`+3`。点估计不受影响，但这些区间是封存 raw 上的 posthoc 描述性
   区间，不能写成完全预注册的 confirmatory interval。分母核对 JSON 记录了
   每个 group 的 expected/observed seed。
2. **当前报告没有汇总 release-to-switch。** 原始 v2 JSON 已逐 pair 保存
   `release_to_switch` 和 `release_to_switch_censored`，但
   `metric_v2_reanalysis_report.md` 没有给出 switch/censor 表。因此目前
   只能声称该字段被正确计算，不能声称已经完成该指标的统计报告。
3. **v2 是 sealed raw 的只读重分析。** `action_supported_metric_v2_preregistration.md`
   只登记了 metric smoke，完整 240 条流并没有在看到 v1 结果前按 v2 指标
   重新运行。v2 数字可以作为明确标注的审计结果，不能伪装成新的实验或
   预注册确认结果。

## 5. 对当前结论的修正

- relative persistent release 后 exposure 应引用 v2 的约 `1.40/1.28/1.34%`
  （delay `0/1/4`），不是 v1 的 `8.03/7.88/8.23%`。
- v1 的 exposure 数字必须保留为历史结果，但解释改为“未 event-bound 的旧
  exposure estimand”，不能继续解释成事件反馈后的 target persistence。
- “8% 与 1.4% 的差距主要来自 blind window”是不正确的；delay0 的 blind
  长度为零，真实主因是 fold1 两个 seed 的事件前 failure release。
- v2 `Delta_adapt` 仍能支持一个有限的机制描述：relative 在 persistent
  的 release 后恢复段高于 streak-only，但低于 matched；这不是 confirmation，
  也不能证明 relative 已经超过 matched。

## 6. 下一步的最小修复

不修改 raw、runner 或主方法，只做三件事：

1. 在新的完整运行前，把四类 metric 的 bootstrap seed 显式写进预注册；如果
   沿用当前 analyzer，则登记 metric offsets `0/1/2/3`，否则统一使用 group
   seed，不能运行后再选。
2. 从已有 v2 JSON 汇总 `release_to_switch` 的 switched/censored 数量；未来
   结果表必须同时列出删失比例。
3. 任何论文或 gate 文档都把本批 v2 标为“sealed raw read-only metric audit”，
   不把它算作新的 effect run 或 confirmation。

本审计发现的是分析窗口和证据标签问题，不是 MIRA 方法或 benchmark 的新
结果；v1 raw、v2 JSON 和所有失败记录继续保留。
