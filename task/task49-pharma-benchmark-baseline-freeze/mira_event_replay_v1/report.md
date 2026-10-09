# MIRA transient/persistent failure replay v1

日期：2026-09-30
状态：CPU development replay 已完成；不进入 confirmation，不提交 A800。

## 1. 收据和核验

- 合同：`mira_event_replay_v1/preregistration.md`；复用同一 inputs、20 个 fit/feature snapshots、5 folds、2 seeds、delay `{0,1,4}`。
- 权威收据：`experiments/logs/task49_mira_event_replay_cpu_20260930/full_v2/`。
- 矩阵：2 event kernels × 4 methods × 5 folds × 2 seeds × 3 delays = 240 streams；decision/terminal/feedback 各 34,656。
- event kernel：`transient_failure` 在 event tick 只让 target provider 失败一次；`persistent_failure` 从 event tick 起持续失败。target 是每个 fold 的 fit-only Best Fixed provider。
- schema/evaluator verifier：`verification.json`，240/240 streams、34,656/34,656 decisions，通过；它独立检查菜单、selected-only read-set、事件身份、terminal 状态、truth/label 对应关系、延迟释放、版本/state hash 连续性和 summary。
- relative-reliability action/state replayer：`relative_action_verification.json`，60/60 candidate streams、8,664/8,664 events，通过；不导入 policy，独立重算 score、reliability factor、argmax、availability streak、release state 和 runtime failure 不进入 Beta。
- raw events SHA-256：`918050e6f045fc5d5bfd358ab8fe9d9291c8ceee97fd03a9914b15f836383865`；receipt manifest 记录各文件大小和 SHA。

## 2. 结果

以下为 10 个 fold/seed stream 的均值。`post` 从固定 event tick 开始；target exposure 从首次 target failure release 起计算。它们不是同一个 estimand。

### Persistent failure

| method | overall d0/d1/d4 | post d0/d1/d4 | target exposure after release d0/d1/d4 |
|---|---|---|---|
| Frozen | 86.994/86.994/86.994% | 84.848/84.848/84.848% | 11.92/12.11/11.87% |
| streak-only | 89.135/88.996/88.996% | 89.104/88.828/88.828% | 1.67/2.27/1.54% |
| relative-reliability | 89.411/89.273/89.066% | 89.519/89.243/88.830% | 1.18/1.93/1.84% |
| matched Online-SGD | 88.931/88.651/89.479% | 90.358/89.669/90.352% | 0/0/0% |

relative-reliability 的 post gain 相对 Frozen 为 `+4.67/+4.39/+3.98 pp`，相对
streak-only 为约 `+0.41/+0.41/+0.00 pp`，仍低于 matched Online-SGD 的
`+5.51/+4.82/+5.50 pp`。它降低了 target exposure，但没有像 matched 那样
在一次 runtime failure 后立即把 target exposure 压到零。

### Transient failure

| method | overall d0/d1/d4 | post d0/d1/d4 | 首次 target failure release 比例 |
|---|---|---|---|
| Frozen | 90.173/90.173/90.173% | 91.176/91.176/91.176% | 0/0/0% |
| streak-only | 90.173/90.173/90.173% | 91.176/91.176/91.176% | 0/0/0% |
| relative-reliability | 90.311/90.311/90.242% | 91.315/91.315/91.176% | 0/0/0% |
| matched Online-SGD | 89.137/88.859/90.101% | 90.769/90.082/91.587% | 20/10/10% |

这个 transient kernel 没有形成相同的有效事件暴露：fit-only Best Fixed target
在 event tick 很少被 Frozen/MIRA 选中，relative candidate 三个 delay 的首次
failure release 比例都是 0。因此这部分不能支持“偶发失败后保持稳定”的强结论；它
暴露的是事件 target 与 policy action path 的 support 不足，而不是 MIRA 已经通过了
transient 机制检验。matched 的少量 failure release 也不足以构成单独结论。

## 3. 对预注册条件的判断

- persistent post gain：relative 没有在任何 delay 比 streak-only 低超过 1 pp，满足本轮的机制保留条件。
- stationary preservation：no-event 分母仍引用上一条 unified receipt；relative 相对 Frozen 为 `+0.14/+0.14/+0.07 pp`，保持在预登记 ±1 pp ROPE 内。
- transient 条件：由于 relative 没有实际 failure release，不能判定通过或失败，必须标为 **underpowered / unsupported**。
- relative 与 matched 的 persistent 差异符合预先提出的 failure-channel 诊断：matched 把 runtime failure 直接作为 reward 0 更新，relative 只通过 availability streak 处理 runtime failure；relative 的 correctness ledger 没有被污染。

## 4. 下一步

这批结果支持继续保留 MIRA 的目标 claim，但不支持直接进入 A800 full matrix。下一轮需要先登记一个 **action-supported event** 协议：event target 在运行前由 fit-only Frozen policy 在 event tick 的 incumbent 决定，保证 transient 事件确实作用于当前选择路径；不能根据本批结果事后挑 provider。该协议仍固定 snapshots、fold/seed/delay，并分别报告偶发失败恢复和持续失败恢复。

同时保留一个独立的 runtime-failure response channel 候选：它只能读 selected runtime failure，不能把 runtime failure 写入 chemical correctness ledger；其必要性由 matched 的 post 优势和 relative 的 residual target exposure 来检验。只有 action-supported transient/persistent CPU gate 通过后，才考虑 A800 full matrix；confirmation 继续关闭。

本报告不把 transient 的 underpowered 结果写成 positive result，也不把 persistent 的相对 Frozen 增益写成超过 matched 的普遍优越性。
