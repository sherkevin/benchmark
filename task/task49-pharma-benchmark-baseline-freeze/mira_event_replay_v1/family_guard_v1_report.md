# MIRA-FamilyGuard v1：family-outage 修复回执

日期：2026-10-01
状态：development CPU receipt；候选修复的首次完整结果。未读取 confirmation，未使用 A800，不是最终确认结果。

## 1. 这轮要回答的问题

上一轮 family-outage 回放定位出一个具体问题：provider-local availability ledger 会在同一 family 内逐个尝试 provider，导致 persistent family failure 下重复试错。FamilyGuard 只增加一个 selected-only 的 family hazard channel，保留原有 provider-local correctness/availability channel、同一 fit-only prior、同一菜单和反馈时钟。

FamilyGuard 的 group 先验为 $a_0=1,b_0=9$；连续两次 selected runtime failure 后，对该 family 触发 4 个 release version 的 gate，成功 selected feedback 清零连续失败计数但保留 posterior。transient 单次 failure 不触发 group gate。

## 2. 封存协议和收据

- 输入：722 个 BBB development molecules，33-provider menu，5 scaffold folds，2 seeds，delay `{0,1,4}`。
- 事件：Frozen no-event event-tick 的 provider family；selected-only、activation-gated；transient 与 persistent 两种 kernel。
- 目标 manifest：150 个方法条件坐标。旧四方法共同支持集 $S=23/30$；加入 FamilyGuard 后 candidate common support 为 `18/30`。主比较仍在运行前封存的旧四方法 $S$ 上进行，另报 candidate support sensitivity。
- no-event candidate control：30 streams、4,332 decisions，独立 verifier `verified`。
- event receipt：5 methods × 5 folds × 2 seeds × 3 delays × 2 scenarios = 300 streams，43,320 decisions，130,260 raw event rows 由原始 JSONL SHA 绑定（events_sha256=`d9e00720d95199b1186c65bc530709f812bc892418cf655eb8ff138ddd8f6d65`）。其中 274 条 exposed、26 条 unexposed、3,605 次注入失败。
- 独立 replay verifier：`300/300 verified`；检查 manifest/action/entity、事件前 prefix、unexposed 与 no-event 完全等价、自然 failure 对照、persistent exactly-once 注入、selected-only delayed feedback 和 state hash。
- 原始回执：`experiments/logs/task49_mira_family_guard_cpu_20261001/event_full_v1/`。
- 目标验收：`.../family_guard_target_verification_v1.json`；no-event 验收：`.../no_event_full_v1/verification_v2.json`；事件验收：`.../event_full_v1/verification_v1.json`；机器指标：`.../event_full_v1/metrics_v2.json`；摘要：`.../event_full_v1/family_guard_v1_summary.json`。

## 3. 预登记主结果（common support S）

指标是 event 相对 no-event control 的 release-after correctness delta；括号内为五-fold paired bootstrap 95% 区间。单位为百分点。

| persistent delay | FamilyGuard | current relative | FamilyGuard − relative | FamilyGuard target-family exposure | relative exposure |
|---:|---:|---:|---:|---:|---:|
| 0 | -6.63 [-11.97,-2.60] | -14.24 [-22.77,-7.49] | **+7.60 [+3.74,+13.70]** | 8.23% | 17.23% |
| 1 | -4.46 [-8.57,-1.26] | -16.70 [-24.90,-9.25] | **+12.24 [+5.87,+19.82]** | 6.77% | 20.77% |
| 4 | -3.73 [-6.30,-1.30] | -26.20 [-34.54,-17.51] | **+22.47 [+13.93,+29.76]** | 5.77% | 30.09% |

FamilyGuard 的 exposure 相对 current relative 分别下降 `9.00/14.00/24.32 pp`；leave-family latency 的点估计分别少 `4.17/4.50/5.83` 个 release ticks。相对 matched-SGD 的 release-after delta 为 `+3.56/+7.05/+21.51 pp`（delay 0/1/4；delay 0 区间跨 0，不能写成所有 delay 都显著优于 matched）。

transient 在三个 delay 的 release-after delta 都是 `0 pp`，未观察到单点 failure 的额外损失。26 条 unexposed stream 均通过和 no-event control 的逐事件等价检查。

## 4. 资源代价

事件回执中按每条 stream 的 raw decision/update 时间计算的 pooled p95：

- relative：selection `78.63 µs`、update `67.51 µs`；
- FamilyGuard：selection `111.99 µs`、update `114.13 µs`。

相对 relative 的两个 p95 比值分别为 `1.42×` 和 `1.69×`，满足候选预登记的 `2×` 上限。数值状态数组为 `18,737 bytes`，relative 为 `18,513 bytes`，比值 `1.012×`，低于 `4×` 上限。这里没有把 pending event 对象算入状态大小；这个口径与已有 common-stratum 成本报告一致。

## 5. 预登记条件核对

- persistent S 上相对 current relative 的 release-after 改善在三个 delay 都超过 1 pp，paired bootstrap 区间均在 0 以上：通过。
- family exposure 相对 current relative 在三个 delay 都下降超过 5 pp：通过。
- transient release-after delta 未低于 -1 pp：通过。
- selection/update p95 未超过 relative 的 2 倍，状态数组未超过 4 倍：通过。
- manifest、selected-only、feedback clock、unexposed equivalence、state hash：通过。

这说明 FamilyGuard 解决了上一轮收据中定位的 family-level repeated-trial failure mode。它还不是 confirmation，也没有证明在 model-fresh provider、其他 endpoint 或自然环境漂移下成立。下一步应把它放进 matched/common-stratum 的主实验合同，补充 mechanism ablation 和 failure/unknown/cost 主表，再决定是否冻结进入 confirmation。

## 6. 一个工程失败保留

`no_event_smoke_v1` 曾使用人工拼出的伪 commit 字符串，虽然回执跑完但 provenance 不可信；它已写入 `failure_summary.json`，不进入任何指标。之后使用实际提交 `3d1b60714d908c89683a6cd5b90aff4deab64c39` 的 full no-event receipt，并由独立 verifier 通过。这个失败不能从日志中删除。
