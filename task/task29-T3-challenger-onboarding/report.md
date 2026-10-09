# task29 report — Formal Challenger-Onboarding Baseline Phase

> 日期：2026-09-21
> 状态：classic baselines 已完成；PAK-UCB/RFF-UCB 与 transfer baseline 仍待补。Ours 尚未设计。

## 1. T2 incumbent 已正式冻结

| Benchmark | Frozen incumbent | Stationary quality | Challenger Oracle opportunity |
|---|---|---:|---:|
| LLMRouterBench | full TF-IDF 60k + per-arm Ridge | 0.71550 macro | +0.05715 macro |
| ASlib SAT12-ALL | full 113-feature ExtraTrees | 0.92483 utility / PAR10 902.01 | +0.00932 utility |
| BBB dense | train-derived Best Fixed | 0.93098 accuracy | +0.03921 accuracy |

正式 T3 中，incumbent 的 action policy 不再在线变化。每个 challenger 只和该 frozen incumbent 做二元竞争。

## 2. Classic baseline 结果

### LLMRouterBench — 100 challenger runs

| Method | Reward | IOU vs incumbent | Challenger Regret | Challenger use | Precision | Recall | Harmful rate |
|---|---:|---:|---:|---:|---:|---:|---:|
| Incumbent-only | 0.70426 | 0 | 0.05580 | 0% | — | 0 | — |
| UCB1 | 0.69475 | **-0.00951** | 0.06532 | 7.66% | 6.43% | 8.44% | 26.96% |
| Beta-TS | 0.70302 | **-0.00124** | 0.05705 | 0.73% | 4.97% | 0.73% | 31.81% |
| LinUCB | 0.68742 | **-0.01684** | 0.07265 | 18.85% | 5.70% | 18.91% | 19.42% |
| LinTS | 0.67979 | **-0.02447** | 0.08027 | 17.70% | 5.60% | 17.76% | 22.60% |
### ASlib SAT12-ALL — strong ExtraTrees incumbent

| Method | Utility | IOU | Challenger Regret | Challenger use | Precision | Recall | Harmful rate |
|---|---:|---:|---:|---:|---:|---:|---:|
| Incumbent-only | 0.92270 | 0 | 0.00932 | 0% | — | 0 | — |
| UCB1 | 0.80384 | **-0.11886** | 0.12818 | 29.40% | 11.21% | 29.34% | 80.84% |
| Beta-TS | 0.91602 | **-0.00668** | 0.01600 | 1.57% | 7.36% | 1.53% | 85.47% |
| LinUCB | 0.80437 | **-0.11833** | 0.12765 | 39.96% | 15.19% | 55.51% | 77.26% |
| LinTS | 0.82835 | **-0.09435** | 0.10367 | 24.11% | 12.70% | 27.67% | 79.70% |

### BBB dense — 350 challenger runs

| Method | Accuracy | IOU | Challenger Regret | Challenger use | Precision | Recall | Harmful rate |
|---|---:|---:|---:|---:|---:|---:|---:|
| Incumbent-only | 0.92768 | 0 | 0.03921 | 0% | — | 0 | — |
| UCB1 | 0.89703 | **-0.03065** | 0.06986 | 100% | 3.92% | 100% | 6.99% |
| Beta-TS | 0.92384 | **-0.00383** | 0.04305 | 19.63% | 3.24% | 20.18% | 14.80% |
| LinUCB | 0.90020 | **-0.02748** | 0.06669 | 95.82% | 3.82% | 94.48% | 6.77% |
| LinTS | 0.91492 | **-0.01276** | 0.05197 | 46.95% | 3.96% | 48.47% | 6.92% |
## 3. 这张表真正说明了什么

三套环境中，**所有 classic online baselines 的平均 IOU 都是负的**。这不是因为 challenger 没价值：Challenger Oracle opportunity 明确为正；而是 standard exploration 无法在稀疏正增益区域里获得足够 adoption precision。

现象非常一致：

- optimism 型 UCB / LinUCB 倾向把 zero-history uncertainty 当作价值，recall 高但 precision 极低，过度试新工具；
- Beta-TS 更保守，IOU 伤害最小，但几乎不使用 challenger，recall 很低；
- 在 BBB，真正 positive challenger context 只有约 3.92%，所以“多试”天然很危险；
- 在 ASlib，强 incumbent 把剩余 challenger opportunity 压到约 0.93 utility point，普通探索产生的损失远大于可获得的 uplift。

因此 formal target 已经很清楚：

> **不是提高 challenger usage，而是在 very few feedback 下提高 beneficial-adoption precision，并在不过度牺牲 recall 的情况下让 IOU 首次变正。**

这也给 Ours 设了不能回避的门槛：若最终方法不能在至少 LLMRouterBench + ASlib 上把平均 IOU 从负数推到正数，就没有足够证据支撑“onboarding improvement”。BBB 允许作为 hard stress test 报接近 0 / 显著减少 harm，但不能把负 IOU 包装成成功。

## 4. 现阶段不进入 Ours 的原因

PAK-UCB/RFF-UCB(ICML 2025) 是最直接的 prompt-aware nonlinear baseline，必须先跑；如果它已经能做到正 IOU，论文方法必须再解释 new-arm / transfer 的增量。

P-LinUCB/P-TS(AISTATS 2024) 与任何 shared-subspace / cross-tool-transfer idea 正面重叠；如果我们最终方法使用 transfer，它必须先成为 baseline。

所以顺序固定为：**PAK/RFF-UCB → transfer baseline → 再定 Ours**，不反过来。

## 5. 一手 baseline 边界

- PAK-UCB/RFF-UCB：ICML 2025，官方 repo 已拉到 `.srcprobe/dgm-online-select` HEAD `3ed2ead`。
- P-LinUCB/P-TS：AISTATS 2024，shared affine-subspace meta contextual bandit。
- FC-Entry：ICML 2024，但核心 impatient-arm/cycle 假设不匹配，降 related/appendix。
- CLUCB：NeurIPS 2017；只有最终引入显式 cumulative safety constraint 时 mandatory。
- UCB-AA / ContextRoute：2026 concurrent preprints，若投稿时仍未 peer-reviewed 必须明标 concurrent。
## 6. Calibration-tuned conservative gating 仍没有解决 mature-pool onboarding

另做了一个 deliberately simple 的 sanity baseline：先用 calibration pseudo-new episodes 调 `epsilon / LinUCB confidence / LinTS scale`，测试时仍只在 `incumbent vs challenger` 之间做 gate；这不是 Ours，只是检验“把 exploration 调保守一点”是否已经足够。

在小 incumbent pool（K=3/5）上，调参后的 gate 确实能拿到正 IOU；但随着 incumbent pool 变成熟，opportunity 稀疏后又失效：

| Benchmark | Mature K | Best calibrated gate | IOU | Challenger opportunity |
|---|---:|---|---:|---:|
| LLMRouterBench | 20 | LinTS | **-0.00076** | 0.05689 |
| ASlib | 31 | epsilon gate | **-0.00650** | 0.01702 |

因此“只把 UCB/TS 的探索强度调小”不能解释我们要做的方法；pool-size marginal-value collapse 与 sparse positive region 仍然存在。对应产物：`t3_safe_onboarding_calibrated.py/json`。

这也加强了后续方法约束：真正需要的是**借旧工具/旧任务经验，把极少的 challenger feedback 转成对局部增益区域的识别**，而不是单纯 global conservatism。