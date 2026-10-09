# Availability-hard / correctness-soft：数学审查与下一步候选

日期：2026-09-30  
性质：候选方法的内部理论审查；不是双方已确认的决议，也不改变当前方法或实验结果。

审查对象：`availability-soft` 的 CPU development replay、预注册合同、runner、独立 verifier 和原始事件。当前候选保留连续两次 selected runtime failure 的 hard block，并把 selected chemical correctness 写成

$$
s_t(a\mid x)=q_0(a\mid x)-\eta[U_t(a)-\tau]_+,
$$

其中 $q_0(a\mid x)$ 是 fit-only direct score，$U_t(a)$ 是 selected correctness ledger 的 Beta 上界；当 $v_t(a)<n_{min}$ 时惩罚为零。

## 1. 当前结果先能说明什么

当前 CPU replay 的最干净对照是 `streak-only`。它在 no-event 下与 Frozen 完全相同，在 controlled availability event 下相对 Frozen 的 post accuracy 提升为 `+4.26/+3.98/+3.98pp`（delay `0/1/4`）。这说明正控中的主要响应来自 availability hard channel。

`availability-soft` 的 no-event 相对 Frozen 为 `-0.42/-0.48/-0.48pp`，controlled post 相对 Frozen 为 `+4.26/+3.43/+3.43pp`。它没有超过 `streak-only`；matched Online-SGD 的 controlled post accuracy 为 `0.9036/0.8967/0.9035`，也高于 availability-soft 的 `0.8910/0.8828/0.8828`。因此本轮只支持“hard availability response 可复现，soft correctness 会带来静态代价”，不支持优越性或动态环境泛化。

`beta-only` 在 controlled event 下的 post accuracy 为 `0.7804/0.8054/0.7686`，相对 Frozen 为 `-6.81/-4.31/-7.99pp`；它没有 runtime hard block，target post exposure 升到约 `0.138/0.111/0.153`。这项对照说明，当前正控的恢复不是 Beta ledger 自己完成的；没有 availability channel 时，correctness gate 甚至会让故障 incumbent 继续被选。它是机制归因的必要对照，不是一个可部署 baseline。

已有 full MIRA 与上一轮 `run_v2_fixed` 的 60 条 stream 逐事件等价；360-stream verifier 通过。Beta-only、full、availability-soft 的事件角色不同，不能把它们的 pooled 差值直接解释成同一种 correctness 学习。

## 2. 加性惩罚的结构性问题

### 2.1 它把“已观察”变成隐式探索信号

设当前 query 的 base winner 是 $a$，另一个 provider 是 $b$。若 $a$ 已有 selected feedback 而 $b$ 没有，当前规则通常给出 $m_t(a)=[U_t(a)-\tau]_+>0$、$m_t(b)=0$。只要

$$
q_0(a\mid x)-m_t(a)<q_0(b\mid x)<q_0(a\mid x),
$$

动作就从已有证据的 $a$ 切到没有反馈的 $b$。这个切换不是因为观察到了 $b$ 的正确性，而是因为 $b$ 的 penalty 尚未产生。它把“缺少证据”当成了“没有风险”。

从本轮 `events.jsonl` 重算的结果与这个推导一致：每个 scenario 的 4,332 次 decision 中，rank change 分别为 `681/626/488`（no-event）和 `683/627/477`（controlled，delay `0/1/4`）。这些 rank changes 都发生在 base argmax provider 有非零 penalty 的情况下；最终被选 provider 自身带非零 penalty 的次数只有 `16/13/10`，每个 scenario 合计 39 次。主要作用不是校正当前被选 provider，而是把动作让给 penalty 为零的 provider。

这不是实现错误，而是加性排序规则和 selected-only ledger 的必然结果。它也解释了 no-event 的约半个百分点损失。

### 2.2 $n_{min}$ 造成状态不连续

在 $v_t(a)=n_{min}-1$ 时惩罚为零；下一次 scored return 后突然启用 Beta 上界。即使计数只增加一次，动作分数也可能发生离散跳变。以当前参数为例，$v=20,f=1$ 时 $U=0.206725$，penalty 为 $0.006725$；$v=20,f=2$ 时 $U=0.270551$，penalty 为 $0.070551$。前者可能改变近 tie，后者足以跨过多个 provider。

因此 $n_{min}$ 不是单纯的“最小样本保护”，它改变了策略状态转移。延迟 feedback 还会让这个跳变在 release tick 发生，而不是在调用发生时发生。任何后续实验都应记录首次 penalty、首次 rank change 和 release tick，不能只报告最终 accuracy。

### 2.3 $U_t(a)$ 的统计对象与 $q_0(a\mid x)$ 不一致

Beta ledger 只看到被选择的 provider。它最多描述

$$
P(Y=0\mid A=a,\text{provider }a\text{ 被当前策略选中},\mathcal H_t),
$$

而不是当前分子上的 $P(Y=0\mid A=a,X=x)$，也不是所有分子上的 provider error rate。$q_0(a\mid x)$ 却是 query-specific score。把一个全局、选择后、上下文混合的上界直接从 $q_0$ 上减掉，没有一个共同的概率或 utility 推导。

更根本地，如果某 provider 在历史中从未被选择，$\pi_t(a\mid x)=0$，那么两个世界——一个世界中它在该上下文上可靠，另一个世界中它错误——会产生完全相同的 observed history。没有探索或额外 side information，任何算法都不能从 selected-only 数据识别这两个世界。加性 penalty 只是把不可识别性隐藏成了一个确定的排序动作。

### 2.4 Beta 上界不是退化证据

$U_t(a)\ge\tau$ 的含义是“当前证据不足以排除 error rate 至少为 $\tau$”，不是“provider 已被证明退化”。当前规则还在每次 release 后反复查看上界，并把 adaptive、context-mixed 的 selected labels 放进同一 arm ledger；没有 anytime coverage 或 alpha spending。因此这里可以使用“保守的风险排序量”或“证据不足惩罚”，不能使用统计安全保证的表述。

## 3. 一个更干净的数学对策

这里不提出若干启发式。先把可识别对象分成两个层次，再决定后续是否值得实现。

### 3.1 若不引入探索：只声称 selected-policy risk，并用相对可靠性修正

在不改变 selected-only 协议的前提下，唯一可直接识别的是 selected-policy 下的 arm-level 风险。为了不再引入一个未经验证的 provider 先验，直接把当前风险阈值 $\tau$ 作为 Beta 先验均值，把已有的 $n_{min}$ 解释为先验等效样本量：

$$
e_a\sim\operatorname{Beta}(n_{min}\tau,n_{min}(1-\tau)).
$$

观察到 $v_t(a)$ 个有效 selected return，其中 $f_t(a)$ 个 chemical error 后，后验均值为

$$
\widehat e_t(a)=
\frac{n_{min}\tau+f_t(a)}{n_{min}+v_t(a)}.
$$

把它写成相对可靠性因子，而不是从 $q_0$ 上减一个不同量纲的数。只惩罚高于风险阈值的后验均值，不给“看起来比先验好”的 arm 额外奖励：

$$
\rho_t(a)=\min\left\{1,\frac{1-\widehat e_t(a)}{1-\tau}\right\},
\qquad
\widetilde q_t(a\mid x)=q_0(a\mid x)\rho_t(a).
$$

在没有 selected feedback 时 $\rho_0(a)=1$；每次 release 只做连续的后验更新，不需要 $n_{min}$ 跳变。availability ledger 仍单独处理 runtime failure hard block。这个模型的有效结论必须限定为“在 arm-level selected-risk 稳定、且 $q_0$ 是可比较的成功排序分数时的相对排序”；它不能推出未选 provider 的当前分子质量。

这个修正不是当前已确认的方法。它的价值在于只使用已有 $\tau,n_{min}$，同时把统计量、尺度和信息边界写在同一个模型里：如果不愿意增加探索，它是 selected-only 协议下最小、可推导的目标。这里的后验均值是排序量，不是覆盖保证。

### 3.2 若要声称 context-specific risk：可识别性要求显式 propensity support

若目标是估计 $P(Y=1\mid A=a,X=x)$，则必须给每个可比较 provider 一个非零选择概率。令 $\pi_t(a\mid x)$ 是决策前可计算的 propensity，并要求 $\pi_t(a\mid x)\ge\epsilon/|\mathcal M_t|$。在一个低维 residual model 中写成

$$
\operatorname{logit}p_t(a\mid x)
=\operatorname{logit}q_0(a\mid x)+\phi(a,x)^\top\theta_t.
$$

selected feedback 到达时，对每个目标 provider $a$ 用 predictable propensity 的加权正则似然更新：

$$
\mathcal L_t(\theta)=
\sum_{s\le t}\frac{\mathbf 1\{A_s=a\}}{\pi_s(a\mid x_s)}
\log\operatorname{Bern}(y_s;p_s(a\mid x_s))
-\frac{\lambda}{2}\|\theta\|^2.
$$

动作使用更新后的 posterior mean 或 lower confidence score；availability hard block 仍是单独的 observed runtime state。这里的关键不是某个新的优化器，而是两个可证伪条件：propensity support 是否存在，以及 context residual 是否比 arm-level correction 更能预测未来 selected correctness。若坚持零探索，$\pi=0$ 的 provider 仍不可识别，任何更复杂公式都无法补上这个信息缺口。

## 4. 下一步只做一个有针对性的诊断

暂不换规则、不申请 GPU。先用已有 events 做只读分析，回答当前 soft channel 是否在做合理的风险修正：

1. 对每次 decision 计算 base argmax、soft argmax、被扣分 provider、其 $v_t,f_t,U_t$ 和 release tick；确认上面的 rank-change 不等式是否逐事件成立。
2. 将 rank changes 按“已观察 provider 被扣分后切到未观察 provider”“两个已观察 provider 间重排”“实际 selected provider 自身被扣分”三类计数，并按 provider family/scaffold 分层。
3. 把 Beta state 的首次启用时刻和第一笔错误分开；报告 $n_{min}$ 跳变造成的动作变化，不再把所有变化叫作实时学习。
4. 对 no-event 与 controlled-event 做同 stream 的 scenario difference；controlled event 只归因于 event_tick 后、首次 failure release 后的变化，预先发生的 soft penalty 单独列出。

若下一轮仍需验证数学对策，最小 CPU 设计应只比较 Frozen、当前 availability-soft 和第 3.1 节的相对可靠性模型，保持 hard availability ledger、snapshots、stream、delay 和 matched SGD 不变。预注册应包含 no-event `±1pp` ROPE、rank-change 率、selected-risk 分层和 event suffix；不应先调 $\kappa$ 或把 upper bound 换成另一个阈值来追求正点差。

## 5. 当前建议的结论边界

- availability-hard 的 controlled event 响应是可复现的机制信号；它针对的是已知 incumbent 的 synthetic unavailability。
- 当前 additive correctness-soft 有可观测决策作用，但主要通过惩罚已观察 provider、让动作流向未观察 provider；no-event 代价已被实验证明。
- selected-only、零 propensity 的 arm 不能被数学上“估计出来”；这是信息限制，不是换一个 Beta 公式就能修好的实现细节。
- 第 3.1 节是保留 selected-only 协议时的候选推导，第 3.2 节是若要做 context-specific claim 时的可识别性要求。二者都是候选建议，尚未进入决议或主方法。
