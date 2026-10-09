# MIRA relative-reliability v1：下一步科学优先级

日期：2026-09-30  
状态：只读复盘；不改目标 claim、不修改已确认决议、不执行新实验。

## 结论先行

当前最值得推进的问题不是继续给每个 baseline 单独补日志字段，而是把 **“静态稳定性”和“事件后恢复速度”拆成两个估计量**，确认 MIRA relative-reliability 到底提供了什么价值。

现有证据已经给出一个清楚但容易被混淆的结果：

- relative-reliability 在 no-event 下基本保持 Frozen，且比 matched Online-SGD 稳定；
- 在 controlled event 的 post-event suffix 上，relative-reliability 三个 delay 都低于 matched Online-SGD；
- relative 的 controlled-event overall accuracy 在 delay 0/1 略高于 matched，原因主要是 matched 在 event 发生前就付出了更大的静态代价，不能解释成 relative 的事件后适应更强。

因此当前不能问“relative 是否比 matched 更强”。正确的问题是：

> 在 selected-only 延迟反馈下，能否用不牺牲 stationary performance 的保守更新实现足够的 persistent-failure recovery；如果不能，损失来自反馈延迟、runtime-failure 更新权限，还是两次 failure 才 hard block 的安全阈值？

这个问题直接服务于方法主线，也能被现有 raw evidence 和一个小型、固定合同的后续 replay 证伪。

## 1. Raw schema 工作是否已经收益递减

### 已经得到的收益

raw contract 工作已经跨过了“完全没有可复核事件链”的阶段：

1. MIRA contract smoke 已记录 menu、decision read-set、provider binding、terminal、feedback release、version 和 state hash；独立 verifier 能拒绝 menu/state 篡改。
2. unified schema audit 已覆盖 C0/C4–C8 与 MIRA contract smoke，共 21 条 stream，字段合同和恢复路径已经被机器检查。
3. matched Online-SGD 已有统一 raw full receipt：60 条 stream、8,664 条 decision/terminal/feedback，6/6 source provenance 通过。
4. relative-reliability 自己的 CPU receipt 已有 360 条 stream、51,984 条 decision/terminal/feedback；relative 子集由独立 NumPy/SciPy replayer 逐事件复核，60/60 stream 通过。

这几项已经足以支持下一轮**机制问题诊断**。继续把 Random、PILOT、BayesianRouter、Greedy 一个一个地迁移到同一 writer，而不先决定要回答哪个科学问题，会产生越来越多工程收据，却不会改变当前方法选择。

### 仍然必须完成的工作

这不等于 baseline contract 已经可以在论文里宣称全部闭合。根据生效的 Benchmark/Baseline 标准，进入最终主表前仍需：

- 至少一个现代最近邻（PILOT 或 BayesianRouter）在同一任务合同下完成 full raw run 和 recovery；
- Proposed relative-reliability 与 matched baseline 使用同一 unified writer/replayer，避免旧 effect log 和新 contract log 混表；
- provider lineage、family correlation、hERG/AMES endpoint 资格和 confirmation freeze 仍要单独闭合。

所以优先级应改成 **按科学问题成组迁移**：下一次只让 Proposed、Frozen、streak-only 和 matched Online-SGD 共用一个 full raw run；随后选择一个最强现代近邻完成同一合同。不要在没有主问题的情况下继续逐方法铺基础设施。

## 2. 先固定估计量，避免把结果混成“强弱”

令 $m$ 表示方法，$s\in\{N,C\}$ 分别表示 no-event 和 controlled-event，$d\in\{0,1,4\}$ 表示反馈 delay。对每条 fold/seed stream 在 event tick 切成 pre 和 post，定义：

$$
A_{m,s,d}^{\mathrm{all}}=\text{该 stream 的 overall selected accuracy},
$$

$$
A_{m,s,d}^{\mathrm{pre}},\qquad A_{m,s,d}^{\mathrm{post}}=\text{event tick 前后 suffix 的 selected accuracy}.
$$

Frozen 的 controlled-event post gain 是

$$
\Delta_{m,d}^{\mathrm{event}}=A_{m,C,d}^{\mathrm{post}}-A_{F,C,d}^{\mathrm{post}}.
$$

它回答“受控 provider version event 后，方法相对 Frozen 恢复了多少”。它不是方法之间的直接排名。

relative 与 matched 的直接 post 比较是

$$
G_{R,M,d}^{\mathrm{post}}=A_{R,C,d}^{\mathrm{post}}-A_{M,C,d}^{\mathrm{post}}.
$$

stationary cost 则是

$$
C_{m,d}^{\mathrm{static}}=A_{m,N,d}^{\mathrm{all}}-A_{F,N,d}^{\mathrm{all}}.
$$

由于 controlled stream 的 overall 同时包含 event 前后的样本，若 $\alpha$ 是 pre 样本比例，则

$$
A_{m,C,d}^{\mathrm{all}}=\alpha A_{m,C,d}^{\mathrm{pre}}+(1-\alpha)A_{m,C,d}^{\mathrm{post}}.
$$

因此 $A^{\mathrm{all}}$ 不能替代 $A^{\mathrm{post}}$ 来判断 event adaptation。当前每个 fold 的 pre/post 切分固定，不能因为 overall 看起来更高就改变 estimand。

## 3. 现有 raw 的只读结果

来源：`experiments/logs/task49_mira_relative_reliability_cpu_20260930/run_v1/events.jsonl`，SHA-256 `8ceae41fa4ba444e03014bc94123becf8790f288ede3c8e2fb855a070000517f`。下面均为 10 个 fold/seed slot 的均值；这里只做聚合，不产生新预测。

### 3.1 No-event：relative 的优势是静态稳定性

| delay | Frozen overall | relative overall | matched SGD overall | relative − matched |
|---:|---:|---:|---:|---:|
| 0 | 90.17% | 90.31% | 89.14% | **+1.17 pp** |
| 1 | 90.17% | 90.31% | 88.86% | **+1.45 pp** |
| 4 | 90.17% | 90.24% | 89.69% | **+0.56 pp** |

relative 相对 Frozen 为 `+0.14/+0.14/+0.07 pp`，在预注册的 ±1 pp ROPE 内。它的主要可辩护性质是 cold-start 与 stationary preservation，而不是已证明的普遍 accuracy 提升。

### 3.2 Controlled-event post：matched 的恢复更快

| delay | Frozen post | relative post | relative event gain | matched post | matched event gain | relative − matched post |
|---:|---:|---:|---:|---:|---:|---:|
| 0 | 84.85% | 89.52% | +4.67 pp | 90.36% | **+5.51 pp** | **−0.84 pp** |
| 1 | 84.85% | 89.24% | +4.39 pp | 89.67% | **+4.82 pp** | **−0.43 pp** |
| 4 | 84.85% | 88.83% | +3.98 pp | 90.35% | **+5.50 pp** | **−1.52 pp** |

relative 在三个 delay 都保留了相对 Frozen 的 event suffix 收益，但没有超过 matched。以 fold 为单位做 paired bootstrap，relative−matched 的 95% 复采样区间分别为：

- delay 0：`[-2.78, +0.82] pp`；
- delay 1：`[-2.92, +1.79] pp`；
- delay 4：`[-4.28, +1.36] pp`。

这些区间不能支持 relative 在 post-event 上优于 matched；点差也不能支持“matched 一定显著更强”，因为只有 5 个 scaffold fold。可确认的方向是：relative 没有达到 matched 的 suffix 水平。

### 3.3 Controlled overall：不能反推 post adaptation

controlled overall 的 relative−matched 差值是：

| delay | relative overall | matched overall | relative − matched overall |
|---:|---:|---:|---:|
| 0 | 89.41% | 88.93% | +0.48 pp |
| 1 | 89.27% | 88.65% | +0.62 pp |
| 4 | 89.07% | 89.48% | −0.41 pp |

这看似与上一表冲突，其实不冲突。两者在 controlled pre 段的 relative−matched 差值是 `+1.81/+1.67/+0.69 pp`；这正是 matched 在 event 前已经在线更新、牺牲了 stationary accuracy 的代价。把 pre 优势和 post 劣势加权后，才得到 overall 的小幅正负变化。

因此论文中必须分别报告：

1. stationary preservation：no-event overall vs Frozen；
2. event recovery：controlled post vs Frozen；
3. direct adaptation comparison：controlled post relative vs matched；
4. total utility：overall accuracy 与调用/失败/更新成本。

不能用第 4 项替代第 2、3 项，也不能把 relative 的 overall 正点差写成“event adaptation stronger”。

## 4. 机制解释：为什么 relative 的 suffix 不如 matched

这不是一个需要马上换公式的猜测，现有代码和 raw 已经给出可检验的机制解释：

1. `MatchedOnlineSGDPolicy` 在每个 release 对 selected reward 更新；runtime failure 以 reward 0 进入 `_release_failure`，因此一次失败就能改变该 provider 的 score。
2. MIRA relative-reliability 明确把 runtime failure 排除在 correctness ledger 外，只有同 provider 连续两次 runtime failure 才 hard block；chemical correctness 只通过 $\rho_t$ 做 selected-only 的相对降权。
3. 在 controlled post 段，relative 选择 event target provider 的总次数为 `12/13/15`（delay `0/1/4`，10 个 slot 合计），matched 为 `5/6/10`；relative 在首次 failure release 后仍有 target calls，matched 的 target calls 全部发生在首次 failure release 之前。
4. 这与 matched 的更激进 runtime-failure update 相一致，因此这是当前首要待验证的原因；raw 结果本身还不能把优势归因成 context-specific chemical correctness 学习。相应代价是 no-event overall 比 relative 低 `1.17/1.45/0.56 pp`。

这形成了一个明确的 two-axis trade-off：

$$
\text{stationary preservation}\quad\leftrightarrow\quad\text{post-failure recovery speed}.
$$

当前证据支持把 relative 讲成“保守的 selected-only 相对可靠性修正”，不能讲成“比 matched Online-SGD 更强的实时学习器”。这不会降低目标问题的重要性，只是把下一步要解决的技术缺口说清楚。

## 5. 下一步唯一优先科学问题

### 问题

**在不牺牲 no-event stationary preservation 的前提下，MIRA 是否能在 persistent provider failure 下达到可接受的 post-release recovery；如果不能，瓶颈是 delayed feedback、runtime failure 是否进入更新，还是两次 failure hard-block 的阈值？**

它比“再加一个 baseline”更关键，因为它直接决定方法的核心接口：runtime failure 是独立 availability channel，还是应该进入一个受控、可校准的快速恢复 channel。

### 数学诊断

对每个方法同时报告四个量：

$$
\left(C_{m,d}^{\mathrm{static}},\;\Delta_{m,d}^{\mathrm{event}},\;G_{R,M,d}^{\mathrm{post}},\;E_{m,d}^{\mathrm{after\ release}}\right),
$$

其中 $E$ 是首次 target failure release 后继续选择 target 的比例。候选的机制预测是：

- relative：$C_{R,d}^{\mathrm{static}}\in[-1,+1]$ pp；persistent event 下 $E$ 在第二次 runtime failure release 后下降；
- matched SGD：$E$ 下降更快，但 $C_{M,d}^{\mathrm{static}}$ 更负；
- 若只改变 event 持续性而不改变 snapshot、menu、delay、seed，post 差异应反映 failure channel，而不是 backbone。

把 transient 和 persistent 分开：transient 是单次 runtime failure 后恢复原输出，persistent 是 event tick 后连续 failure。两者共同回答“安全保守”是否有意义：一次偶发错误不应永久换 provider，持续错误必须离开 incumbent。

### 最小后续方案（只登记，不在本次执行）

固定现有 `run_v2_fixed` snapshots、输入、5 folds、2 seeds 和 delay `{0,1,4}`，不调参、不重新训练。只新增两个事件 kernel：

1. `transient_failure`：event tick 仅让 target provider 产生一次 runtime failure，随后恢复原 panel；
2. `persistent_failure`：复用当前 controlled event，target provider 在 event tick 后持续 runtime failure。

方法只保留四个：Frozen、streak-only、MIRA relative-reliability、matched Online-SGD。no-event 结果复用现有 raw，不重复跑；新增 `2 event kernels × 4 methods × 5 folds × 2 seeds × 3 delays = 240 streams`。所有 stream 仍使用同一 selected-only queue 和独立 replayer。

预注册唯一主指标为：

- transient 的 overall accuracy 相对 Frozen；
- persistent 的 post accuracy 相对 Frozen；
- 首次 failure release 后 target exposure $E$；
- no-event stationary cost（直接读取现有 receipt，不因新结果改定义）。

可证伪条件：

1. 若 relative 在 transient 场景相对 Frozen 下降超过 1 pp，说明当前 hard availability channel 不能保护偶发失败，relative 方案的“保守性”不成立；
2. 若 relative 在 persistent 场景的 post gain 在三个 delay 中至少两个低于 streak-only 超过 1 pp，说明连续乘性 correctness 修正没有保留已有 hard-switch 机制的恢复能力；
3. 若 relative 在 persistent 场景仍明显保留 target exposure，而 matched 的优势只来自一次 failure update，则需要决定是否引入单独的 failure-response channel；不得继续用 correctness ledger 解释该收益；
4. 若 relative 通过前两项且 stationary cost 保持在 ±1 pp 内，才值得进入一次 A800 full matrix 和冻结 confirmation；否则停止这条 correctness 变体，保留其负结果并回到 failure-channel 建模。

这里的“通过”不是超过 matched。matched 仍是强在线对照；本方案只判断 relative 是否满足它自己的稳定性—恢复性合同。

## 6. 当前不做的事

- 不再为每个方法单独添加字段，除非该字段服务于上述四个估计量或 baseline fidelity 闸门；
- 不把 relative 的 controlled overall 正点差写成 post-event 优势；
- 不把 selected-only Beta/relative factor 写成 context-specific provider quality estimator；
- 不用当前开发结果打开 confirmation，也不根据 matched 差值调整 $\tau$、$n_{min}$、failure streak 或 delay；
- 不因为 suffix 低于 matched 就撤销动态适应目标；这只是指出当前 failure channel 的具体缺口。

## 7. 对主线的影响

故事线仍然是：工具调用前只能依据 fit-only prior，调用后结果可能延迟、失败或错误；主 agent 需要一个能在真实反馈到达后更新选择的动态模块。当前新增的科学事实是：动态模块至少有两个不同目标——保持稳定和快速恢复——不能用一个 pooled accuracy 隐去二者的冲突。

下一步应围绕这两个目标构造清楚的机制证据，再决定 A800 full matrix 的最终版本。raw schema 工作服务于这个问题时继续；脱离这个问题的逐方法迁移暂缓。

