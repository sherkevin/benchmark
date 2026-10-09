# MIRA veto ablation v1

日期：2026-09-30
状态：development CPU mechanism evidence；不进入 confirmation，不申请 A800。

## 目的与合同

v2 的 MIRA 同时有两种 veto：Beta error upper bound 和连续 runtime-failure streak。上一轮 controlled-event 的正向 suffix 不能直接说明是哪一个机制起作用，所以本轮只做机制拆分。

- 5 scaffold folds × 2 protocol slots × 2 scenarios × 3 delays × 5 variants = **300 streams**。
- 复用 `run_v2_fixed` 的 fit-only direct priors、64 维 feature snapshots、输入、evaluation order、target event、evaluator 和反馈队列；没有重新训练或新增 provider/API 预测。
- variants：Frozen、streak-only、beta-only、full MIRA v2、matched Online-SGD。
- 机制开关固定在预注册文件中：streak-only `n_min=10^9, streak=2`；beta-only `n_min=20, streak=10^9`；full `n_min=20, tau=.20, delta=.05, streak=2`。

## 独立核验

独立 verifier 重新读取 raw JSONL 和 sealed input，核对 43,320 条 decision、terminal、feedback，各类事件数量相等；重算 selected output、failure、correctness、due/release clock 和 provider binding。

full 条件与权威 `run_v2_fixed` 逐事件比较：60 条 full stream、8,664 个 decisions 的 provider/version，以及 feedback 的 provider、status、version before/after、block reason 和 MIRA state 全部一致。说明这批消融没有悄悄改变 v2。

把第一条 decision 的 provider 篡改后，verifier 以退出码 1 拒绝（`tamper_action_v1/expected_rejection.log`）。

收据：

- raw events SHA：`f0c51a0512647eaf6816e57c3f0626a223381892f8d38dc4ffe54bfa2fc81e85`
- config SHA：`ffb228ecfde2c01fd5d606117e69f66bfe51bd6223c6a809bd094a2efebf93ce`
- verification SHA：`e07732c9a2642b954c40bedae6bdce2d74268c7f0530b985bd5d5a98adbc094c`
- analysis SHA：`722258e969e29d7e474ee8a3e3e82354fb72f9b7886de8dea1d93fd0ce86801f`

## 结果

下表是 controlled-event 的 post accuracy，以及相对 Frozen 的 paired delta。每个点是 10 个 fold/slot 配对 stream 的均值。

| variant | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.8485 / 0.00pp | 0.8485 / 0.00pp | 0.8485 / 0.00pp |
| streak-only | 0.8910 / **+4.26pp** | 0.8883 / **+3.98pp** | 0.8883 / **+3.98pp** |
| beta-only | 0.7804 / **−6.81pp** | 0.8054 / **−4.31pp** | 0.7686 / **−7.99pp** |
| full MIRA v2 | 0.8855 / **+3.70pp** | 0.8814 / **+3.29pp** | 0.8759 / **+2.74pp** |
| matched Online-SGD | 0.9036 / **+5.51pp** | 0.8967 / **+4.82pp** | 0.9035 / **+5.50pp** |

No-event 的相对 Frozen delta：

| variant | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| streak-only | 0.00pp | 0.00pp | 0.00pp |
| beta-only | −0.28pp | −0.42pp | −0.42pp |
| full MIRA v2 | −0.28pp | −0.42pp | −0.42pp |
| matched Online-SGD | −1.04pp | −1.31pp | −0.49pp |

## 机制解释

1. **controlled-event 的响应来自 streak ledger。** Beta 只在 `status=ok` 的 scored feedback 上计数，注入的 runtime failure 不进入 Beta 统计。streak-only 已经恢复了大部分 full 的 event 后收益；beta-only 完全不能处理 provider unavailability，且继续选择退化 provider，结果低于 Frozen。
2. **Beta veto 当前过于激进。** 在 full MIRA 的 30 条 controlled streams 中，目标 provider 首次 block 有 25 条发生在 event 后的 `runtime_failure_streak`，5 条在 event 前已触发 `beta_error_upper`；按 delay 0/1/4，event 前 target block 分别为 2/2/1 条。no-event 的 full 与 beta-only 完全相同，说明静态损失主要由 Beta gate 引起。
3. **full 不是当前最强方法。** 三个 delay 上 matched Online-SGD 的 post accuracy 都高于 full MIRA。这个消融只定位机制，不能支撑 MIRA 超过强 online baseline 的 claim。
4. event tick 到第一次 failure release 之间仍有 pending decisions；报告同时保留 event-suffix 与 first-release-after 指标。first-release-after 的 target exposure 需要按有目标 failure 的 stream 报分母，不能把没有 failure 的 stream 当成 0。

## 方法修复方案

当前应把 MIRA 的两个功能拆成两条状态通道：

- **availability ledger**：连续 selected runtime failures 是高置信的可用性事件，允许 hard block 或 cooldown；它处理 provider/version 不可用。
- **correctness ledger**：selected chemical correctness 只作为软风险分数或候选排序惩罚，不再用固定 Beta upper bound 直接永久 block；若保留 veto，必须改为预注册的 anytime/alpha-spending 校准，并按 context/scaffold 分层。

下一轮只允许实现一个具体变体：`availability-hard / correctness-soft`，复用同一 snapshots 和 matched Online-SGD，对 no-event 与 controlled-event 做 CPU replay。若它仍不能超过 matched baseline，问题就不在 veto 开关，而在 fit-only representation/feedback credit assignment；再决定是否进入 A800。

## 边界

这批结果不是自然 drift、arrival、retirement 或 model-fresh 泛化；controlled event 是预先注入的 incumbent unavailability。它说明 MIRA v2 的正控信号主要来自连续失败切换，同时暴露 Beta gate 的静态损失。目标 claim 不下调，但下一步必须修正机制并通过同合同 CPU 闸门。
