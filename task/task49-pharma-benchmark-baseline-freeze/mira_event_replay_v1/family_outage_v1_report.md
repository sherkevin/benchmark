# Family-outage full receipt v1

日期：2026-10-01

## 1. 这轮实验回答什么问题

这轮不是自然 provider drift，也不是 provider-level 排名。它固定 33-provider menu，在 no-event Frozen 的 event-tick family 上做 selected-only controlled outage，问：收到延迟 failure feedback 后，selector 能不能离开故障 family，并恢复任务成功率。

动态范围固定为 `feedback-adaptive`。不涉及 provider arrival/retire、version shift 或未选择 provider 的结果。

## 2. 回执和复核

- 240 streams = 5 folds × 2 seeds × 3 delays × 4 methods × 2 scenarios。
- 34,656 decisions，104,208 raw event rows。
- delay：`0, 1, 4`；scenario：`transient_failure`、`persistent_failure`。
- 共同支持集按运行前封存的 manifest 固定为 `S=23/30` 个 `(fold, seed, delay)` 坐标，全部来自 `b3clf` family。
- 226 条 exposed，14 条 unexposed；3,388 次 target-family 注入。
- 独立 cross-run verifier：`verified`。它没有导入 replay runner，重新检查了 manifest entry、event entity/action、prefix state/action、selected-only feedback、exactly-once release、自然 failure 对照和 unexposed 全流语义。

机器收据：

`experiments/logs/task49_mira_event_replay_cpu_20261001/family_outage_v1_full_v1/verification_v1.json`

主指标：

`experiments/logs/task49_mira_event_replay_cpu_20261001/family_outage_v1_full_v1/metrics_v1.json`

## 3. 主要结果

下面的 `release-after Δ` 是 event − no-event 的任务成功率差值，先按坐标配对，再按 fold 做 cluster bootstrap。数值为百分点。

### Persistent outage：共同支持集 S

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | -78.43 | -79.45 | -81.84 |
| streak-only | -14.02 | -15.43 | -25.98 |
| relative-reliability | -14.24 | -16.70 | -26.20 |
| matched-SGD | -10.19 | -11.51 | -25.24 |

`relative-reliability − streak-only` 的 paired 点差是 `-0.22 / -1.26 / -0.22 pp`；对应的 95% fold-cluster 区间分别为 `[-4.41, 3.25]`、`[-5.24, 1.88]`、`[-7.63, 4.98] pp`。delay1 的点估计超过 1 pp，但区间跨 0，所以没有触发“稳定低于 streak-only”的反证条件。

`relative-reliability − matched-SGD` 的点差是 `-4.04 / -5.18 / -0.96 pp`。当前 receipt 不支持 relative-reliability 超过 matched-SGD；这组差异仍需要在最终 baseline contract 和更大的确认矩阵中单独解释，不能包装成胜出。

### Family exposure 和切换速度

Persistent S 上，relative-reliability 释放后继续选择故障 family 的比例为 `17.23% / 20.77% / 30.09%`（delay 0/1/4），streak-only 为 `16.41% / 19.15% / 29.87%`，matched-SGD 为 `11.97% / 15.04% / 26.16%`。relative 相对 matched 的 exposure 差为 `+5.26 / +5.72 / +3.93 pp`。

这说明当前 relative-reliability 的 failure response 比 matched-SGD 慢；它能恢复，但没有达到我们想要的快速 family-level isolation。

Transient 场景中，relative-reliability 的 release-after correctness delta 为 0；这符合“一次偶发 failure 不应造成永久切换”的目标。该结果只说明没有额外的化学正确性损失，不等于已经证明所有动态场景都可靠。

## 4. 结果解释和问题定位

问题不在回执或统计单位：unexposed 流全部通过语义等价检查，event tick、release tick 和 selected-only 边界也逐条通过。当前差异来自更新规则本身。

现有 relative-reliability 的 hard availability ledger 是 provider-local：连续 runtime failure 只封锁被选中的 provider。family outage 会让 selector 在同一 family 的多个 provider 之间继续尝试，因此 family exposure 下降得慢。matched-SGD 对每次 failure 的 score 更新更强，因而更早离开 family。

这给出了一个明确的改进方向：把 runtime availability 从 provider-local 扩展成 **family hazard channel**，同时保留 selected-only 约束和 transient 的恢复能力。不能把这轮结果写成 MIRA 已经优于所有 baseline。

## 5. 下一步候选修复

新增一个 family-level failure ledger，只在 selected provider 的 feedback release 时更新：

1. 每个 family 维护 posterior failure rate 和 consecutive-failure streak；未选择 provider 不产生更新。
2. score 乘 family reliability factor；连续 failure 或 posterior upper bound 超阈值时，对整个 family 触发短期 gate。
3. 成功 feedback 清零 streak，posterior 保留；gate 使用 cooldown/hysteresis，避免 transient 单点故障造成永久切换。
4. 每次 release 仍是 O(1) 更新，选择阶段是 O(number of providers)，状态只增加 O(number of families)。

下一轮先做 preregistered CPU smoke，必须同时覆盖 exposed/unexposed、transient/persistent 和 delay4；通过独立 verifier 后再跑同一 240-stream 矩阵。旧 relative-reliability receipt 保留，不能用新规则覆盖旧结果。

## 6. 验收判断

- 协议可执行性：通过。
- 共同支持集和 baseline 可比性：通过到 `S=23/30`，仍需在最终主表中明确 ITT 与 S 两个 estimand。
- 当前方法效果：未通过“优于 matched-SGD”的目标；relative 与 streak 的差异没有稳定显著。
- Gate C：继续 HOLD。下一步是 family hazard channel 修复和同矩阵复验，不申请 A800，不打开 confirmation。
