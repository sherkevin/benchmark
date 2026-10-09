# Delayed dynamic verifier/escalation v1：CPU 回放报告

日期：2026-09-28
状态：停止，不提交 Nebula A800

## 这次要验证什么

静态 confidence escalation 在开发集上有正信号：第一工具的 confidence 低时追加第二工具，准确率高于同数量随机追加。这次把它改成一个有延迟反馈的风险状态：第一、第二工具分别维护 Beta 状态，反馈只在 release tick 到达后更新；每 32 个决策重新判断下一批的 confidence 阈值。

这不是 direct pre-call JEV 的新主方法，也没有调用新工具。它只检验调用后 verifier 是否能在同样的调用预算内从 selected-only 延迟反馈中安全地调整。

## 固定协议

- 输入：冻结的 722 个 BBB development 分子、33 个具体 provider、五个 scaffold-heldout fold。
- 每折第一、第二 provider 由 fit 区正确率排序得到；evaluation 标签只用于回放计分和释放后的状态更新。
- confidence 为调用后 $c=2|p-0.5|$；候选阈值是第一 provider fit confidence 的 $qin\{0.5,0.8,0.9,1.0\}$ 分位数。
- 风险门使用错误率后验 $\mathrm{Beta}(1+e,1+n-e)$，$n_0=20$、$\alpha=0.05$、$\delta=0.05$。选数值最小且 95% 上界不超过 $\alpha$ 的阈值；没有通过时使用数值最大的阈值。
- 第一工具 confidence 低于阈值，或调用失败没有 numeric confidence 时，追加第二工具。第二工具也失败时最终 reward 为 0；没有 numeric confidence 的事件不进入 confidence-bin 状态。
- 延迟为 0、1、4；反馈 exactly once；confirmation 关闭。

## 结果

独立 recovery 从冻结输入、缓存和序列化决策重新计算阈值、延迟队列、每条决策、状态计数和 pooled 指标，逐项通过。三次结果逐字节一致。

| delay | dynamic final accuracy | static q=.8 accuracy | dynamic 平均调用 | 阈值门通过批次 |
| ---: | ---: | ---: | ---: | ---: |
| 0 | 0.900277 | 0.923823 | 1.490305 | 0/5 |
| 1 | 0.900277 | 0.923823 | 1.490305 | 0/5 |
| 4 | 0.900277 | 0.923823 | 1.490305 | 0/5 |

所有 stream 都回退到最大 numeric confidence 阈值（fold 0 的初始阈值为 0.9999995）；风险上界始终高于 0.05，因此没有一次放宽阈值。总共 1062 次追加调用，其中 15 次第二工具没有 numeric confidence，按预注册的失败语义记为最终错误。

## 闸门裁定

- 动态 error 不低于静态 q=.8 的 delay 数：`0/3`，要求至少 `2`。
- 每个 delay 中，五折有 `4/5` 落在静态 accuracy 减 0.05 以内；这个条件本身达到最低要求。
- 最大平均调用次数：`1.490305`，超过成本上限 `1.25`。
- 总闸门：**不通过**。

结论是支持不足和成本失败，不是延迟实现失败。风险门的保守回退在当前样本量、目标错误率和调用预算下无法同时提供可靠保证与可用覆盖；它还把第二工具调用扩大到约一半题目，反而低于已经固定的静态 escalation。

## 证据与复现

- 预注册：`task/task49-pharma-benchmark-baseline-freeze/dynamic_verifier_v1/preregistration.md`
- 回放：`experiments/logs/task49_dynamic_verifier_20260928/run_v2/results_raw.json`
- 独立恢复：`experiments/logs/task49_dynamic_verifier_20260928/run_v2/recovered_v2.json`
- 恢复器：`src/probe_headless/recover_pharma_dynamic_verifier.py`
- 输入 SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`
- 没有新 provider/API 调用，没有 Nebula 任务，没有读取 confirmation。

## 下一步

不调 $α$、$δ$、$n_0$、batch 或阈值分位数来追分，也不提交 A800。动态 verifier 作为一次负结果保留；主线继续使用已经通过 CPU 闸门的静态 confidence verifier/escalation，并把 direct selected-only 动态 JEV 停留在负证据和一次安全控制的位置。后续只有出现新的独立可观测信号，才重新打开动态状态设计。
