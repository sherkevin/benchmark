# MIRA-SafeSwitch CPU replay v2

日期：2026-09-29
状态：development CPU 证据；不进入 confirmation、主 benchmark 或 A800。

## 本轮改动

相对 v1，只修改 runtime failure 的阻断条件：同一 provider 必须连续两次被选中并收到 runtime failure，才会触发 `runtime_failure_streak` veto；一次成功返回会清零 streak。fit-only prior、Beta 风险阈值、输入、fold、slot、事件、delay、Frozen 和 matched Online-SGD 对照均未变。

预注册判据写在 `task/task49-pharma-benchmark-baseline-freeze/mira_safeswitch_cpu_v2/preregistration.md`：no-event 相对 Frozen 的配对点差值不得低于 -1.0pp；controlled-event 至少两个 delay 的 suffix 提升至少 1.0pp，且第一次合法 failure release 后目标 provider 选择率不超过 2%。

## 结果

| 场景 / 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| no-event Frozen | 0.9017 | 0.9017 | 0.9017 |
| no-event MIRA v2 | 0.8989 | 0.8976 | 0.8976 |
| controlled-event Frozen | 0.8699 | 0.8699 | 0.8699 |
| controlled-event MIRA v2 | 0.8879 | 0.8851 | 0.8817 |

MIRA v2 相对 Frozen：

- no-event：`-0.28 / -0.42 / -0.42pp`，三个 delay 都在预注册 -1.0pp ROPE 内；
- controlled-event 事件后 suffix：`+3.70 / +3.29 / +2.74pp`，三个 delay 均达到预注册 +1.0pp；
- 第一次合法 target failure release 后，target selection rate：`1.64% / 1.51% / 1.18%`，均低于 2%。

因此 v2 通过了这轮 development 判据。它不是最终确认：controlled event 是预先注入的 provider-version degradation，不能外推为自然环境 drift；10 个 fold/slot 配对单元的 bootstrap 区间仍较宽，不能把点差写成普遍显著性。

## 规模与复核

- 5 scaffold folds × 2 deterministic protocol slots × 2 场景 × 3 方法 × 3 delay = 180 streams。
- 未使用 GPU、LLM 或新工具预测；confirmation 关闭。
- 独立 replayer 从 fit-only snapshot、feature snapshot、terminal/release 顺序重算 MIRA masked scores、Beta 状态、failure streak、block reason、version 和动作；180 条 stream 的事件守恒通过，180 条 MIRA stream 逐事件重放通过。
- verifier 额外检查了 no-event 原始缓存输出、controlled-event 的目标 provider 注入、terminal/feedback status 和 due tick。复制收据篡改 terminal 后，verifier 以退出码 1 拒绝。

收据：`experiments/logs/task49_mira_safeswitch_cpu_20260929/run_v2_fixed/`。此前的 `run_v2/` 保留为版本字段修复前的现场，不作为本报告的权威收据。

- events：`fe3ee5edca1ab05e36505993037e0c7b9322730aad6e2627d0a9e7354e4d184a`
- summary：`13c9a91b3e8c32e6b29dad9b0ef09d782f4b27a415b5eed03e9b303e49a44f13`
- config：`ed236ec840031e75cc81cbeeb2ed8a59af509a95e3181319cf102d87f283cd21`
- independent verification：`verification_v3.json`，`verified`（180/180 streams，180/180 MIRA streams replayed）
- analysis：`analysis.json` / `analysis.md`；analysis SHA=`cbc3e528806ff1ea64b5afc6e11fcab358a6483ab564a7066fc69a7f1a93ff83`
- tamper rejection：`tamper_terminal_v2_fixed/expected_rejection.log`，退出码 1

## 方法判断

v2 支持一个更精确的机制判断：selected-only delayed feedback 可以在已知 provider-version degradation 中驱动安全切换，而连续失败门槛减少了静态面板上的瞬时 failure 误阻断。它仍没有证明未选 provider 的当前质量可识别，也没有证明 MIRA 已超过 Best Fixed 或所有现代 router。

下一步不是继续调 `n_min`、`tau_fail` 或 streak 直到得到更高数字，而是补两项证据：先做 MIRA v2 的 P/Q 隐藏未选标签不变性测试，再把 C1–C8 的统一主表和 dynamic event 结果映射回 Gate B/C。只有通过这两项，才讨论一次 A800 的 model-fresh/confirmation 任务。

## 2026-09-30 机制复核更正

对原始 feedback 逐事件复核后，controlled-event 的正向 suffix 不能直接归因于 Beta gate，也不能写成 MIRA 优于在线学习 baseline：

- Beta 计数只在 `status=ok` 的 scored feedback 上更新；注入的 runtime failure 不进入 `n_fail`。因此 event 后对不可用 provider 的直接响应来自 runtime-failure streak veto。
- 30 条 controlled MIRA streams 中，目标 provider 首次 block 有 25 条是 event 后 `runtime_failure_streak`，5 条在 event 前已经触发 `beta_error_upper`。所以 pooled suffix 必须按“event 前已 block / 未 block”分层；当前报告的 +3.70/+3.29/+2.74pp 只能作为 development signal，不能作为纯 event 因果增益。
- `n_min=20, tau_fail=.20, delta=.05` 时，20 次有效返回中出现 1 次错误，Beta 95% upper bound 约为 0.2067，便会触发 veto。它表达的是“当前证据不足以排除错误率达到阈值”，不是 provider 已被证明退化；该自适应重复查看也没有 anytime coverage 或 alpha spending，不能称统计安全保证。
- event tick 到第一次 feedback release 之间仍有 pending decisions。`post_accuracy` 从 event tick 起算，包含这段旧状态窗口；应同时报告 event-suffix 和 first-release-after suffix。
- 强同合同对照 Matched Online-SGD 的 controlled-event post accuracy 为 `0.9036/0.8967/0.9035`（delay 0/1/4），高于 MIRA 的 `0.8855/0.8814/0.8759`。因此 MIRA v2 当前只通过了受控机制检查，尚未通过强 baseline 效果门槛。

本更正不撤销原始收据，也不降低 MIRA 的目标；它把下一轮消融问题定得更精确：Frozen、streak-only、beta-only、full MIRA 四条件共享同一 snapshots、事件、pending queue 和 delay，并按 event 前 block、首次 failure release 和强 matched baseline 分层。`research/debate_story_method_20260929/round1/method_theory_auditor_mira_method_card.md` 的下一候选审查以此更正为准。

该消融已完成并独立核验，见 `../mira_veto_ablation_v1/report.md`。结果显示 streak-only 在三个 delay 上保留主要 event 后收益，beta-only 显著低于 Frozen；下一候选固定为 availability-hard / correctness-soft，先做 CPU replay。
