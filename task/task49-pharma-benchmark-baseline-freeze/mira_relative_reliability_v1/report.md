# MIRA relative-reliability v1

日期：2026-09-30
状态：development CPU mechanism evidence；独立回放通过；尚未进入 confirmation，尚未申请 A800。

## 本轮目的

上一轮 additive correctness penalty 的主要问题是：它把已经观察到的 provider 直接减分，动作容易流向没有 selected evidence 的 provider，而且 `n_min` 带来开关式变化。本轮只改 correctness 合并方式，保留 availability hard channel 和所有实验输入。

候选使用 fit-only score

$$q_0(a\mid x)=\operatorname{sigmoid}(w_a^\top x).$$

对 selected scored return 维护 $v_t(a)$ 个有效返回和 $f_t(a)$ 个 chemical error。用 $\tau=0.20$、$n_{min}=20$ 构造连续后验均值：

$$\widehat e_t(a)=\frac{n_{min}\tau+f_t(a)}{n_{min}+v_t(a)}.$$

再用有界相对可靠性因子调整排序：

$$\rho_t(a)=\min\left\{1,\frac{1-\widehat e_t(a)}{1-\tau}\right\},\qquad \widetilde q_t(a\mid x)=q_0(a\mid x)\rho_t(a).$$

因此冷启动时 $\rho=1$，没有 $n_{min}$ 的 discontinuous switch；availability failure 仍由连续两次 selected runtime failure 触发 hard block。runtime failure 不进入 correctness ledger，unknown 不计数。

## 实验合同

- 复用 `run_v2_fixed` 的输入、fit/feature snapshots、5 个 scaffold folds、2 个 seed、episode 顺序、evaluator 和 controlled provider-version event；不重训、不调参、不调用新 API。
- 对照为 Frozen、streak-only、relative-reliability、availability-soft、full MIRA v2 和 matched Online-SGD。
- 场景为 no-event 与 controlled-event，反馈 delay 为 $0,1,4$；共 $5\times2\times2\times3\times6=360$ 条 stream。
- 预注册闸门：no-event 相对 Frozen 保持在 $\pm1$ pp ROPE 内；controlled-event 至少在两个 delay 保留 streak-only 的切换收益；独立回放、selected-only、反馈时钟和 provider binding 必须全部通过。

## 收据与复核

权威目录：`experiments/logs/task49_mira_relative_reliability_cpu_20260930/run_v1/`。

- decision/terminal/feedback 各 **51,984**，stream_complete **360**。
- 通用 verifier 通过；relative-reliability 与旧 full MIRA v2 的 60 条 stream、8,664 个决策逐事件等价检查通过。
- 独立 NumPy/SciPy replayer `verify_mira_relative_reliability_actions_v1.py` 不导入策略实现，重新计算 base score、$\rho$、masked score、argmax、selected-only state、release tick、version 和 terminal correctness，**60/60 candidate streams、8,664/8,664 events 全部通过**。
- runtime failure 没有污染 $v_t,f_t$；correctness hard block 为 0；all-blocked fallback 为 0。
- 故意篡改 decision 的 reliability-factor 向量和 feedback state 的 reliability factor，verifier 均以 exit code 1 拒绝。收据：`verification_v1/verification.json`、`verification_v1/tamper_tests_v1.json`。

主要文件哈希：

| 文件 | SHA-256 |
|---|---|
| `config.json` | `f271854938375251b46d8f39c94f44d8180344d805c6a18297f55910d73673ca` |
| `events.jsonl` | `8ceae41fa4ba444e03014bc94123becf8790f288ede3c8e2fb855a070000517f` |
| `analysis.json` | `bb25f33c03e8e9fd59b3b0b7daa4e51fb28d577a565b542cb7b3824463984b00` |
| independent `verification.json` | `f45d8fc782642a5e370792203853198e1483660eb835dfd07b90d61f5115e374` |

## 结果

表中是 10 条 fold/seed 配对 stream 的均值；括号为相对 Frozen 的 paired delta。

### No-event

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.9017 / 0.00pp | 0.9017 / 0.00pp | 0.9017 / 0.00pp |
| streak-only | 0.9017 / 0.00pp | 0.9017 / 0.00pp | 0.9017 / 0.00pp |
| relative-reliability | 0.9031 / **+0.14pp** | 0.9031 / **+0.14pp** | 0.9024 / **+0.07pp** |
| availability-soft | 0.8976 / −0.42pp | 0.8969 / −0.48pp | 0.8969 / −0.48pp |
| full MIRA v2 | 0.8989 / −0.28pp | 0.8976 / −0.42pp | 0.8976 / −0.42pp |
| matched Online-SGD | 0.8914 / −1.04pp | 0.8886 / −1.31pp | 0.8969 / −0.49pp |

Relative-reliability stays inside the pre-registered no-event ROPE. It removes the cross-scale subtraction and the $n_{min}$ activation switch, but it does not remove selected-only selection bias; the mechanism audit below keeps that boundary explicit.

### Controlled provider-version event

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.8485 / 0.00pp | 0.8485 / 0.00pp | 0.8485 / 0.00pp |
| streak-only | 0.8910 / +4.26pp | 0.8883 / +3.98pp | 0.8883 / +3.98pp |
| relative-reliability | 0.8931 / **+4.67pp** | 0.8924 / **+4.39pp** | 0.8883 / **+3.98pp** |
| availability-soft | 0.8910 / +4.26pp | 0.8828 / +3.43pp | 0.8828 / +3.43pp |
| full MIRA v2 | 0.8855 / +3.70pp | 0.8814 / +3.29pp | 0.8759 / +2.74pp |
| matched Online-SGD | 0.9036 / **+5.51pp** | 0.8967 / **+4.82pp** | 0.9035 / **+5.50pp** |

候选在三个 delay 都保留了受控 event 后的切换收益，并在 delay 0/1 略高于 streak-only；matched Online-SGD 仍然更强，不能据此声称 MIRA 已经优于最强在线对照。

## 机制读数

Relative-reliability 在每个 scenario/delay 的 1,444 个 candidate decisions 中，base argmax 到 adjusted argmax 的变化约为 465/353/287（no-event）和 468/356/290（controlled-event）。变化都来自已有 selected evidence 的 provider 被相对降权，不能解释为对未选 provider 当前质量的估计。它仍是 selected-only arm-level adaptation，不是 context-specific risk estimator。

相对 additive candidate，本轮结果说明“连续、乘性、冷启动保持不变”的合并方式值得继续验证；它还没有证明最终方法，也没有证明自然 drift、长期恢复、未选 provider 质量识别或 anytime safety。

## 下一步

1. 将该候选作为唯一 Proposed 的 development CPU 版本，补齐与主 baseline 相同的 raw-run/recovery schema。
2. 进行 source-fidelity 和 provider-lineage 闸门审查；通过后再申请一次 A800 full matrix，仍保持一次一个任务。
3. A800 前固定参数和确认集，不用本轮结果调参；A800 后只做一次冻结 confirmation。

本轮的结论是：候选通过 CPU 机制闸门，可以进入下一阶段；它不是最终主结果。
