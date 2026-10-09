# MIRA availability-hard / correctness-soft v1

日期：2026-09-30  
状态：development CPU mechanism evidence；不进入 confirmation，不申请 A800。

## 本轮做了什么

本轮把 v2 的两个 veto 拆成两条通道：连续两次 selected runtime failure 仍然 hard block；selected chemical correctness 不再 hard block，只通过预注册的 Beta 上界 soft penalty 影响排序。

固定规则为：

$$s_t(a\mid x)=q_0(a\mid x)-[U_t(a)-0.20]_+,$$

且只有 $v_t(a)\ge20$ 时才启用 penalty；$U_t$ 使用 $δ=0.05$ 的 Beta 上界。复用上一轮全部输入、fit/feature snapshots、fold/seed、episode order、evaluator、event 和 delay，没有重训、调参或新 API 预测。

## 收据与独立复核

- 6 variants：Frozen、streak-only、availability-soft、beta-only、full MIRA v2、matched Online-SGD；
- 5 folds × 2 protocol slots × 2 scenarios × 3 delays × 6 variants = **360 streams**；
- decision/terminal/feedback 各 **51,984**，stream_complete **360**；
- verifier 逐事件核对 selected-only、provider binding、terminal status、chemical correctness、due/release clock 和事件守恒；
- full MIRA 与 `run_v2_fixed` 的 60 streams、8,664 decisions 的 action/version/block state 完全一致；
- provider 篡改测试被 verifier 以 exit 1 拒绝。
- 独立 NumPy/SciPy replayer `verify_mira_availability_soft_actions_v1.py` 不导入策略实现，重算 60 条 candidate streams、8,664 条 decision/terminal/feedback；base/penalty/soft/masked score、argmax、version、Beta state、streak、release tick 和 terminal correctness 全部通过。runtime failure 108 次均未改变 `n_valid/n_fail`，correctness hard block 全部为 false，all-blocked fallback 为 0；反馈 state 篡改和 decision score `+0.001` 篡改均以 exit 1 拒绝。收据：`experiments/logs/task49_mira_availability_soft_cpu_20260930/verification_v1/verification.json`。

权威收据目录：`experiments/logs/task49_mira_availability_soft_cpu_20260930/run_v2/`。

| 文件 | SHA-256 |
|---|---|
| `events.jsonl` | `953601f888cc37439e78f7d802adc28d4d686ab76cccb906e718acf0fb883362` |
| `config.json` | `f2c5363a18d4619e14dcec9fe797c86e0e7fd8c848ce8b05f9d8066403935d2d` |
| `analysis.json` | `25d586a8b359a7bd3a6587059260d533b84d690cd86d17b770e29124453b61af` |
| `verification.json` | `2ca75813fec6fcc4285e6d29c0ab9c654db522f5de72022a0fb5a458aea80e66` |

## 结果

表中数值是每个 delay 下 10 条 fold/slot 配对 stream 的均值；括号是相对 Frozen 的 paired delta。

### no-event

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.9017 / 0.00pp | 0.9017 / 0.00pp | 0.9017 / 0.00pp |
| streak-only | 0.9017 / 0.00pp | 0.9017 / 0.00pp | 0.9017 / 0.00pp |
| availability-soft | 0.8976 / **−0.42pp** | 0.8969 / **−0.48pp** | 0.8969 / **−0.48pp** |
| beta-only | 0.8989 / −0.28pp | 0.8976 / −0.42pp | 0.8976 / −0.42pp |
| full MIRA v2 | 0.8989 / −0.28pp | 0.8976 / −0.42pp | 0.8976 / −0.42pp |
| matched Online-SGD | 0.8914 / −1.04pp | 0.8886 / −1.31pp | 0.8969 / −0.49pp |

### controlled availability event

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.8485 / 0.00pp | 0.8485 / 0.00pp | 0.8485 / 0.00pp |
| streak-only | 0.8910 / **+4.26pp** | 0.8883 / **+3.98pp** | 0.8883 / **+3.98pp** |
| availability-soft | 0.8910 / **+4.26pp** | 0.8828 / **+3.43pp** | 0.8828 / **+3.43pp** |
| beta-only | 0.7804 / −6.81pp | 0.8054 / −4.31pp | 0.7686 / −7.99pp |
| full MIRA v2 | 0.8855 / +3.70pp | 0.8814 / +3.29pp | 0.8759 / +2.74pp |
| matched Online-SGD | 0.9036 / **+5.51pp** | 0.8967 / **+4.82pp** | 0.9035 / **+5.50pp** |

availability-soft 没有 target provider 的 event 前 hard block（0/0/0 条）；delay 0/1/4 下实际出现 target failure release 的 stream 数为 7/8/6。它保留了连续 failure 的主要切换收益，但没有超过 streak-only 或 matched Online-SGD。

## soft 通道是否真的改变了决策

不是一个空更新：在 availability-soft 的 8,664 条 decisions 中，penalty 非零的 decisions 为 **4,748** 条，base argmax 与 soft argmax 改变的次数为 **3,582**（按 delay 0/1/4，controlled event 为 683/627/477，no-event 为 681/626/488）。实际被选 provider 自身带非零 penalty 的次数只有 **78** 条，说明多数变化来自已观察 provider 被降权后把选择让给未观察 provider。

该统计解释了 no-event 损失：当前 additive penalty 会把 selected-only 的局部风险变成隐式探索，但没有 propensity 或校准保证，导致静态选择被扰动。

## 结论与下一步

1. availability-hard 通道是可复现、可观察的 controlled-event 响应来源；Beta hard veto 被移除后，event 前误 block 消失。
2. 当前 additive correctness-soft 规则仍有 no-event 代价（约 −0.5pp），所以它还不是可以直接进入主实验的最终方法。
3. matched Online-SGD 仍是更强的同合同在线对照；本轮没有证明 MIRA 优于它。
4. 下一候选不改 availability ledger，改 correctness 合并方式：使用 bounded multiplicative reliability factor，或只对已成为 incumbent 的 provider 应用校准风险惩罚；先做小型 CPU replay，必须预注册 rank-change、no-event ROPE 和 event suffix，再决定是否申请 A800。

本轮不声称 anytime coverage、统计安全保证、自然 drift 泛化、cooldown/recovery、未选 provider 质量估计或超过强 baseline 的结果。

## 只读机制审计补充

没有重新运行策略，直接用 `analyze_mira_availability_soft_mechanism_v1.py` 从权威 `events.jsonl` 重算 8,664 条 candidate decisions 的 base/soft argmax、penalty、selected ledger 和 `n_min` release。审计状态为 `verified_read_only_audit`，0 个状态或排序一致性违规；收据为 `run_v2/mechanism_audit.json`，SHA-256 为 `2e03d3c225783646ddf1290ac23bd39b2e5ca88e185a351b02472f23bb6201af`。

- rank change 共 3,582 次，其中 3,554 次是“base winner 有 penalty，soft winner penalty 为 0”，28 次是两个已有 penalty 的 provider 间重排；没有一次发生在两个 provider 都低于 `n_min` 时；
- `n_min` 从 19 到 20 的 release 共 192 次，但没有一次 rank change 能归因于冷启动阈值前后两个 provider 都未满 `n_min`；
- 实际 selected provider 自身带非零 penalty 只有 78 次（每个 scenario 的 delay 0/1/4 均为 16/13/10）；
- 因而本轮 additive penalty 的主要作用可以明确写成：惩罚已有 selected evidence 的 provider，让动作流向 penalty 为零的 provider，而不是对当前 provider 的 context-specific correctness 做校正。

这条审计结果支持理论复核中的下一候选：不再使用硬 `n_min` 开关和跨尺度相减，先验证以 $τ,n_{min}$ 构造连续 Beta 后验均值，再形成有界相对可靠性因子 $ρ_t(a)$ 并乘到 $q_0(a\mid x)$。它仍是候选，不自动进入主方法或 A800。
