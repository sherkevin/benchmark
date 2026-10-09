# Action-supported failure replay：指标 v2 只读审计

日期：2026-09-30  
状态：只读 metric repair；不是新的 effect run，不打开 A800 或 confirmation。

## 1. 证据边界

本报告只把已经封存的 action-supported v1 raw receipt 按 v2 预注册的分段指标重算一次。没有重训、调参、改 target manifest、改 provider menu 或改变反馈权限；因此它不能被写成一批新的确认实验。

- event receipt：`experiments/logs/task49_mira_event_replay_cpu_20260930/action_supported_full_v1/run/`
- no-event control：`experiments/logs/task49_mira_unified_raw_20260930/full_v1/`
- v2 analyzer：`src/probe_headless/analyze_mira_action_supported_metrics_v2.py`
- paired records：240 条 event/control stream，四方法、两种 kernel、5 folds、2 seeds、delay `{0,1,4}`。
- 输出：`experiments/logs/task49_mira_event_replay_cpu_20260930/metric_v2_reanalysis_v2/metrics_v2_readonly.json`；SHA-256 `5d0d375d0eda43f920b86a1c2a2b1eff1bf6eb521b670829c6274f3d16ddf9fd`
- event raw SHA-256：`932b3d353f1465c77f5c96bf15496a52d8e727ffdb33f7b03b1a98cf2795cea9`

另跑了 4-stream metric smoke（frozen/relative、transient/persistent、fold0/seed0/delay0），确认 `delay=0` 的 blind window 为 `NA`，release tick 和 event target 都能从 raw feedback/decision 独立重算。权威 smoke 收据保留在 `experiments/logs/task49_mira_event_replay_cpu_20260930/metric_v2_smoke_v4/`；早先错误 declared commit 的 `smoke_v2` 作为 engineering failure 单独保留。

## 2. v2 主量

令 $T$ 为 event tick，$R=T+1+delay$ 为 failure feedback release tick。`L_inject` 只记 event tick 的一次配对任务损失；`Delta_blind` 统计 $T+1,\ldots,R-1$；`Delta_adapt` 统计 $R,\ldots,N-1$。区间是按 5 个 fold 聚类、保留两个 seed 的 paired bootstrap，20,000 次；这里只作不确定性描述。

### Transient failure：$Δ_{adapt}$

| method | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.00 pp | 0.00 pp | 0.00 pp |
| streak-only | 0.00 pp | 0.00 pp | 0.00 pp |
| relative-reliability | 0.00 pp | 0.00 pp | 0.00 pp |
| matched Online-SGD | -0.98 pp | +0.99 pp | +1.33 pp |

relative 的 transient recovery-only 没有出现下降；v1 中约 `-1.38 pp` 的 `post` 下降来自被强制注入的单个失败 terminal。它应记录为 `L_inject`，不能作为恢复失败。

### Persistent failure：$Δ_{adapt}$

| method | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | -59.93 pp | -59.78 pp | -60.07 pp |
| streak-only | -1.54 pp | -1.13 pp | -1.48 pp |
| relative-reliability | -0.98 pp | -0.85 pp | -0.89 pp |
| matched Online-SGD | -0.84 pp | +0.43 pp | +0.89 pp |

在反馈释放后的窗口里，relative 三个 delay 都高于 streak-only；但 d1/d4 仍低于 matched。这个结果修正了 v1 `post_recovery` 把 feedback release 前盲区混入适应段造成的误读，不等于 relative 已经超过 matched。

### 盲区、注入损失和 target exposure

- `delay=0` 没有 blind window，输出 `NA`；不能填 0。
- persistent 的 `Delta_blind` 在 delay1/4 约为 `-70/-57.5 pp`（不同方法的精确值保留在 JSON），它是反馈尚未到达时的共同窗口，不能归因给更新公式。
- relative 的 `L_inject` 为 1.0（控制流在 event tick 正确而事件流被强制失败）；matched 的 0.8/0.9/1.0 反映其控制动作本身有时已经错误。这个量是环境代价，不是 recovery score。
- relative persistent 的 release 后 target exposure 约为 `1.40/1.28/1.34%`，matched 约为 `0.56/0.70/0.59%`；差距不到预注册的 5 pp runtime-channel 诊断阈值。旧 v1 的较大 exposure 不是单纯由 feedback release 前的盲区造成：旧分析器在整条流中寻找 target 的第一次 failure release，没有把 release 绑定到本次 event。fold1 的两个 seed 在 event tick 之前已有自然 failure release，因此即使 delay=0、blind window 长度为零，旧分母仍从 tick 5/7 开始计数。v2 将窗口锚定到本次事件的 `R`，才对应本次反馈后的 persistence。

### Release-to-switch 完整性

`release_to_switch` 只在 event-bound failure feedback 的 `R` 之后寻找首次切换；找不到时记为右删失，而不是填入有限延迟。本批 10 个 `(fold, seed)` slot × 3 个 delay 的每个 aggregate 都是 `10/10 switched, 0/10 censored`。persistent relative 的均值延迟为 `0.80/0.80/0.80` ticks（delay `0/1/4`），matched 为 `0.20/0.30/0.40` ticks；这些是 sealed raw 上的描述性量，不是独立效果终点。逐 pair 的原始字段和删失标记保存在 `metrics_v2_readonly.json`。

本报告使用的 paired-bootstrap 区间也必须标为 posthoc 描述性区间：v2 分析器对不同指标使用了额外的 seed offsets（`+1/+2/+3`），而完整 240-stream 矩阵并未在看到 v1 结果前按这些 offsets 重新登记。因此点估计不变，但区间不能写成完全预注册的 confirmatory interval。

## 3. 对 v2 反证条件的读法

1. transient relative 的 $Δ_{adapt}$ 在三个 delay 都为 0，没有触发“至少两个 delay 低于 -1 pp”。
2. persistent relative 相对 streak-only 没有在两个 delay 低于 -1 pp，反而分别高约 `0.56/0.28/0.59 pp`；没有触发 recovery 反证。
3. relative 与 matched 的 release 后 target exposure 差小于 5 pp，没有触发独立 runtime-failure channel 的阈值。

这些是对已见 receipt 的指标审计结果，不是对方法做确认性背书。旧 v1 数字、原预注册条件和“指标混淆”的失败记录全部保留。

## 4. 下一步

- 不修改 MIRA 更新式，不因这次指标修正直接提交 A800。
- 将 v2 的 blind/adapt 定义和 paired bootstrap 固定到后续完整协议；如果要把它作为新的 effect 证据，必须重新登记完整矩阵并生成新的 raw receipt。
- 在 Gate A/B/C 仍 HOLD 的前提下，继续补 unified PILOT/Greedy effect、provider lineage、endpoint equivalence、成本/特征公平和独立 confirmation；不能用这次只读审计替代这些缺口。
