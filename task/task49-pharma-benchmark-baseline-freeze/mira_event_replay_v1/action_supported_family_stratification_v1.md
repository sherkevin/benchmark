# Action-supported dynamic event v2：target family 分层

日期：2026-10-01
状态：只读分层完成；不新增事件、不打开 confirmation。

## 1. 目的

method-conditioned incumbent 让每个方法在自己的 event-tick target 上接受故障。先把已完成的 240 对 event/control 按 target provider family 分层，检查当前恢复信号是否被某一个 family 的集中暴露解释。

这一步只做 **方法内** 的 event-minus-no-event 分解。不同方法仍可能选择不同 provider，即使 family 相同，也不把这些 target 当成共同干预来排名。

机器结果：`experiments/logs/task49_mira_event_replay_cpu_20261001/action_supported_v2_full_v2/family_stratification_v1.json`。输入是已通过 prefix identity 检查的 `metrics_v2_checked_v2.json`；family registry 为 `task/task38-p01-provider-pool/external_qualification.json`。

## 2. target 覆盖

120 个唯一 `(method, fold, seed, delay)` target 坐标中：

| target family | 坐标数 | 比例 |
|---|---:|---:|
| b3clf | 106 | 88.3% |
| lgbm_bbb | 10 | 8.3% |
| admetlab | 4 | 3.3% |

没有 `fpadmet`、`admet_ai`、`deeppurpose`、`graphb3` target。该分层可以说明当前 event 主要是 b3clf incumbent 的恢复测试，但不能支撑 family 泛化 claim。

## 3. 方法内结果

在 b3clf target 上，persistent event 的 release 后 `Delta_adapt` 点估计如下：

| method | delay 0 | delay 1 | delay 4 | target exposure（约） |
|---|---:|---:|---:|---:|
| streak-only | -1.71 pp | -1.26 pp | -1.64 pp | 1.0–1.6% |
| relative-reliability | -1.09 pp | -0.95 pp | -0.99 pp | 1.3–1.4% |
| matched SGD | -1.00 pp | +0.47 pp | -0.16 pp | 0.4–0.8% |

Frozen 在同一 b3clf 分层中约为 `-57.8 pp`，说明持续失败事件确实改变了后续结果；它是 no-adaptation 对照，不是一个合理的恢复上界。relative 的 b3clf 恢复仍略优于 streak-only，但这个差值只适用于各自 method-conditioned b3clf target，不能解释为 cross-method causal superiority。

lgbm_bbb 和 admetlab 的若干格只有 1–2 个 pair、1 个 fold，结果保留用于覆盖率审计，不做显著性或泛化结论。

## 4. 对下一轮协议的约束

这一步没有解决 common Frozen-incumbent 问题，反而把约束说清楚了：如果保持 selected-only，不能在某方法没有选择 target 时强行注入 failure，否则会把未观察到的 provider feedback 变成 off-policy 信息。

下一轮只能在运行前选择以下一种协议并封存：

1. **支持集协议**：只在每个方法实际选择共同 target 的坐标上做 paired event，报告支持集大小和删失规则；
2. **方法内协议**：继续使用 method-conditioned target，但把 family-stratified recovery 作为主动态 estimand，并明确不做跨方法 raw post accuracy；
3. **共同动作协议**：显式允许一个预登记的 common target action，再把它作为独立的 controlled-action stress，不再称为自然 selected-only incumbent。

当前建议先采用第 2 种完成 baseline fairness contract，再决定是否值得为第 1/3 种另跑一条小规模 CPU 协议。Gate C 仍保持 `HOLD`。
