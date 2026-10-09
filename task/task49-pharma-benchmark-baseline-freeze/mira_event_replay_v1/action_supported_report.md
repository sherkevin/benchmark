# Action-supported transient/persistent failure replay v1

日期：2026-09-30
状态：CPU development replay 已完成；协议收据通过；不进入 confirmation，不提交 A800。

## 1. 协议与核验

- target manifest：`experiments/logs/task49_mira_event_replay_cpu_20260930/action_supported_v3/target_manifest.json`，120 个 `(method, fold, seed, delay)` 条目，由 event 前 no-event prefix 生成；独立 manifest verifier 通过，SHA-256 `a347a341b01adee8b4a73d6bf00e4ef4c3d5ff576cecd076ed67de572bbcd9b0`。
- event runner：每个方法在自己的 event-tick incumbent 上注入 transient 或 persistent failure；event tick 的实际 action 与 manifest 不一致会直接抛错。权威收据：`action_supported_full_v1/run/`。
- 矩阵：2 kernels × 4 methods × 5 folds × 2 seeds × 3 delays = 240 streams；decision/terminal/feedback 各 34,656。
- schema/evaluator verifier 通过 240/240 streams；relative action/state replayer 通过 60/60 streams、8,664 events；menu/read-set、terminal identity、event-tick target、延迟 release、state/version 和 relative factor 均独立重算。
- raw events SHA-256：`932b3d353f1465c77f5c96bf15496a52d8e727ffdb33f7b03b1a98cf2795cea9`。完整文件 hash 和大小见 `receipt_manifest_v1.json`。

## 2. 成功关闭的协议问题

这轮解决了上一轮的 intervention support：四个方法在 120 个对应 event-tick stream 中都实际选择了 manifest target，transient 的首次 failure release 比例为 100%，不再出现“target 根本没被选中”的 underpowered 情况。

## 3. 结果的正确读法

paired control 使用已有同表示、同 fold/seed/delay 的 no-event unified receipt。下表是 event stream 相对 no-event 的配对差值；`post` 包含 event tick 本身，`recovery` 从 event tick 后一个任务开始，专门排除被注入的那一次 terminal。

### Transient failure

| method | post delta d0/d1/d4 | recovery delta d0/d1/d4 |
|---|---|---|
| Frozen | -1.38/-1.38/-1.38 pp | 0/0/0 pp |
| streak-only | -1.38/-1.38/-1.38 pp | 0/0/0 pp |
| relative-reliability | -1.38/-1.38/-1.38 pp | 0/0/0 pp |
| matched Online-SGD | -2.07/-0.28/-0.14 pp | -0.98/+0.98/+1.25 pp |

所有 non-learning 控制的 `post` 都下降约 1.38 pp，正好对应一次被强制注入的失败样本；因此这个数字不能用来区分恢复策略。排除 event tick 后，relative 与 Frozen 的 recovery delta 都为 0 pp，说明一次偶发失败没有造成额外的永久切换损失。matched 的波动来自它在 event tick 后会继续更新，不能据此写成普遍优越性。

### Persistent failure

| method | post delta d0/d1/d4 | recovery delta d0/d1/d4 |
|---|---|---|
| Frozen | -60.48/-60.48/-60.48 pp | -59.93/-59.93/-59.93 pp |
| streak-only | -2.90/-3.45/-5.93 pp | -1.54/-2.10/-4.61 pp |
| relative-reliability | -2.35/-3.18/-6.22 pp | -0.98/-1.82/-4.91 pp |
| matched Online-SGD | -1.93/-2.07/-4.56 pp | -0.84/-0.84/-3.23 pp |

relative 在 persistent failure 下保留了比 Frozen 大得多的恢复能力；delay 0/1 的 recovery loss 小于 streak-only，delay 4 略差于 streak-only，三个 delay 都落后 matched。relative 的 target exposure 在旧的“整条流第一次 failure release”口径下约为 `8.03/7.88/8.23%`，matched 约为 `0.56/0.70/0.59%`；这组 relative 数字不能再解释成“本次事件反馈后的 persistence”。旧分析器没有把 failure release 绑定到本次 event，fold1 的两个 seed 在 event tick 前已有自然 target failure；因此 delay0 仍会把事件前区间计入分母。event-bound v2 口径下 relative persistent 为约 `1.40/1.28/1.34%`，matched 为约 `0.56/0.70/0.59%`。这说明当前 relative reliability 仍是较保守的 failure response，但两组数字必须分开标注。

## 4. 对原预注册条件的修正记录

原预注册把 transient 的 `post` 相对 no-event 下降超过 1 pp 作为反证条件。action-supported 结果显示这个条件会把 Frozen 和 streak-only 的必然单次注入损失也判成失败，不能区分“被注入的失败样本”与“失败后的策略恢复”。本轮保留原始判断：按字面该条件触发；不把事后 `recovery` 指标改名为原预注册的通过结果。

后续协议必须在运行前把两项分开登记：

1. injected-terminal loss：event tick 的单次失败代价；
2. recovery-only：从 event tick 后一项开始的 accuracy、target exposure 和 release-to-switch。

这是指标定义的修 bug，不是为了保留正结果而改分母；本轮 raw events、原始 paired 分析和该修正理由全部保留。

## 5. 下一步

- relative 的核心方向仍保留：selected-only、延迟反馈、偶发失败不触发永久切换，持续失败会降低 target exposure。
- 目前不能声称 relative 超过 matched；matched 在 persistent recovery 和 target exposure 上仍更强。
- 已登记并执行指标修正版 v2 的 4-stream metric smoke；随后对本批 sealed raw receipt 做了只读分段审计，把反馈释放前盲区与释放后适应段拆开。审计报告：`metric_v2_reanalysis_report.md`，机器收据：`experiments/logs/task49_mira_event_replay_cpu_20260930/metric_v2_reanalysis_v2/`。这不是新的 effect run，不改变本报告 v1 数字，也不打开 confirmation。
- 按 v2 的 `Delta_adapt` 重读后，relative persistent 在三个 delay 都高于 streak-only，但仍低于 matched；v1 的 `post_recovery` 同时混入了 release 前盲区和未 event-bound 的历史 failure release，不能把差异归因给单一盲区。后续完整 effect 若采用 v2 指标，必须重新登记并生成新的 raw receipt。
- v2 还逐流计算了 release-to-switch；本批 relative persistent 的 30 条流全部在 event-bound release 后切换，0 条右删失，均值延迟为 0.80 ticks。由于完整矩阵并未事前登记当前分析器使用的 bootstrap seed offsets，相关区间只作 sealed raw 的 posthoc 描述，不能写成确认性区间。
- 另行设计 runtime-failure response channel 时，仍须与 chemical correctness ledger 分开，避免用 chemical label 解释 runtime failure 的快速恢复。
