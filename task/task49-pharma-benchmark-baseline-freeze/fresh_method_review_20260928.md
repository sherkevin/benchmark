# 直接选择动态 JEV：第一性原理方法审查

日期：2026-09-28  
状态：方法审查稿；没有提交 Nebula，没有读取 confirmation，也没有把候选的预期当作结果。

## 结论先写清楚

在当前 benchmark 的信息合同下，我没有找到一个可以诚实地称为“新的动态 JEV 主方法”的候选。原因不是还缺一个优化器，而是直接选择阶段没有足够的新信息：决策前只有分子、性质、候选菜单和已经释放的所选反馈；未选 provider 的 evaluation 标签永远不可见。若环境没有可识别的漂移，更新只是在有限样本上重新估计已经有的固定分数。

因此当前建议分两条：

1. **直接 pre-call selection 主线先暂停方法创新。** 若必须再做一个 CPU 实验，只做本文第 5 节的 `DG-SafeJEV`，把它当作“保守决策规则”对照，不把它写成新后验模型。它通过不了预登记闸门就停止这条线，不再继续堆 SGD、Bayesian、memory 或新 backbone。
2. **把已有的正信号推进成 post-call verification/refusal 扩展。** provider 返回的 numeric probability 在调用后能预测 `system_success`，fit-only 阈值 verifier 的 pooled accepted accuracy 为 `0.961606`，实际 coverage 匹配的随机拒答为 `0.916230`。这可以做一个有清楚边界的动态 verifier 候选；它不是调用前选工具的效果，必须分开报告。

如果第 5 节 CPU 证伪失败，论文主线应诚实地收束为：独立真值的医药 direct-tool benchmark、强现代 baseline、selected-only 延迟反馈的系统负结果，以及调用后可复核/拒答信号。不能为了保留“动态 JEV”这个名字而把标准安全切换包装成创新。

## 1. 先固定问题和信息权限

一条请求在决策时写成 $x_t$（分子和性质）与候选菜单 $A_t$。策略只看到已经释放的历史

$$
H_t=\{(x_i,A_i,a_i,r_i):\ i<t,\ r_i\text{ 已到期释放}\},
$$

其中 $a_i$ 是实际调用的 provider，$r_i\in\{0,1\}$ 是该 provider 的 `system_success`。反馈延迟由 release tick 决定，等待中的事件不在 $H_t$ 中。

**实例：** 当前题是 BBB 分子 `z000282`，菜单包含 `admetlab_3_0` 和 `graphb3`。如果本题调用 `admetlab_3_0`，它在终止后最终释放 $r_t=1$；在释放前，learner 不能读取 `graphb3` 的输出，也不能读取本题参考真值。若 delay=4，前四次决策都看不到这条 $r_t$。

直接选择的目标是

$$
\max_\pi\ \mathbb E_\pi\left[\sum_{t=1}^{T}r_t\right],
\qquad a_t=\pi(x_t,A_t,H_t).
$$

**实例：** $T=100$ 时，策略的分数是 100 次实际 provider 调用中 `system_success=1` 的次数；拒答若作为扩展，不能把拒答自动记成 $r_t=1$。

调用返回后可能还有一个只属于扩展的信息 $c_t$，例如 numeric probability 的置信度。它满足

$$
 c_t=2\left|p_t^{\mathrm{raw}}-0.5\right|,
\qquad c_t\text{ 只在 provider 返回后可见}.
$$

**实例：** provider 返回原始概率 $p_t^{\mathrm{raw}}=0.95$ 时 $c_t=0.90$；这个值可以用于是否接受/拒答，但不能在调用前用来替这个 provider 选票。

### 可识别性限制

只用 selected-only 反馈时，未选 provider 的当前结果没有观测。可以构造两个环境 $P,Q$，使它们在当前策略产生的 $H_t$ 上分布完全一样，却让未选 provider 的正确率相反。于是任何只依赖 $H_t$ 的更新函数 $F(H_t)$ 在两个环境中都产生同一个状态，不能同时知道哪一个未选 provider 更好。

**实例：** 策略一直选 A，A 的反馈在两个环境中都按同一分布产生；环境 $P$ 中 B 的正确率是 $0.95$，环境 $Q$ 中 B 的正确率是 $0.05$。只看 A 的标签，策略无法区分 $P,Q$。要解决这个限制，必须引入至少一项：固定概率探索、provider 结构共享、可重复的局部上下文、工具/环境漂移，或调用后的额外观测。任何只换优化器的方案都没有新增信息。

## 2. 当前证据排除了什么

以下结果都在同一类 scaffold-disjoint BBB development 流上取得，不能被重新命名成正结果：

| 方向 | 已有观察 | 对方法设计的含义 |
|---|---|---|
| 普通 selected-only 更新 | Online-SGD `0.8991`、Replay-SGD `0.8964`，低于 Best Fixed `0.9155` | 不能把“有实时梯度”当贡献 |
| 独立 BayesianRouter-style 更新 | delay `0/1/4` 均低于 Frozen | 独立后验不是当前缺口的答案 |
| PILOT-adapted prior + cosine/UCB | pooled `0.882271/0.880194/0.885042`，未过预登记闸门 | 换成离线 preference prior 不能解决在线问题 |
| provider transfer、校准 transfer | 真实关系不优于 Frozen，且跨 delay 仍低于固定策略 | 不能无证据强行跨 provider 传播标签 |
| context residual memory | 校准 v2 在三个 delay 均低于 independent | “相似分子记忆”在当前 scaffold split 没有可用收益 |
| 固定审计/propensity residual | 两个预登记版本均低于同探索对照 | 当前支持不足，且探索代价明显 |
| confidence-bin residual | `2/3/4/5/8` 个区间的平均 delta 都为负 | 简单 confidence 分箱修正不成立 |
| round1/round2 审计 | 变化集中在 B3clf 执行抖动，未发现 checkpoint 或版本随时间更新 | 没有证据把主任务写成工具漂移适应 |
| Laya 静态诊断 | linear/MLP/BT+CLS/direct property 均低于 Best Fixed | 当前不能把“换 backbone”当作动态方法理由 |

### 仍然成立的正信号

调用后的原生 numeric probability 与正确性有稳定关系：160 个 provider-fold AUC 的 median 为 `0.760306`。在 fit-only 固定 threshold、目标 coverage `0.8` 的 verifier replay 中，五折 pooled accepted accuracy 为 `0.961606`；按 verifier 实际接受数匹配的随机拒答为 `0.916230`。进一步的固定阈值二次调用回放中，第一工具低置信度时追加 fit-only 第二 provider，pooled accuracy 为 `0.923823`，相同 fallback 数量的随机追加为 `0.904432`，平均调用次数为 `1.206`；独立恢复和 500 个随机 seed 的 null 检查均通过。它仍是调用后验证流程，不是 selected-only 在线学习。这说明**返回结果里面有可复核信息**，不说明调用前的 provider 选择已经变好。

## 3. 只做一个直接选择 CPU 候选：DG-SafeJEV

### 3.1 候选的定位

`DG-SafeJEV`（Drift-Gated Safe JEV）不是新的 Bayesian 后验。它是一个有明确决策规则的最后控制实验：保留 fit-only 的强固定 provider 作为 incumbent，只有在在线证据足够强时才允许切换；否则回退到 incumbent。它回答一个窄问题：此前方法失败是否主要因为更新过于激进，而不是因为没有可识别的漂移。

它不使用未选 provider 的 evaluation 标签；不读取本题真值；不把调用后 confidence 放进 pre-call 分数。随机探索若开启，必须按预先固定的概率完整记录。

### 3.2 冷启动分数和选定 incumbent

在 outer-fold fit 区训练/校准固定分数 $\ell_a^0(x)$，并选出 train-only Best Fixed provider

$$
a_0=\arg\max_{a\in\mathcal A}\frac{1}{|D_{\mathrm{fit}}|}\sum_{(x_i,y_{ia})\in D_{\mathrm{fit}}}y_{ia}.
$$

**实例：** 若 fit 区的 `admetlab_3_0` 正确率最高，则该 fold 的 $a_0$ 是 `admetlab_3_0`；evaluation 标签不能参与这个选择。

将每个 provider 的在线状态限制成一个 scalar drift offset $\delta_a$，初始为 $\mu_{a,0}=0$、$v_{a,0}=0.04$；数值稳定项固定为 $\epsilon_{\mathrm{num}}=10^{-6}$，而不是重新训练 encoder。一次已释放的 selected reward 先给出

$$
p_t=\sigma\bigl(\ell_{a_t}^0(x_t)+\mu_{a_t}\bigr),
\qquad h_t=p_t(1-p_t)+\varepsilon_{\mathrm{num}},
$$

再用 logistic 的局部二阶近似把二元反馈转成一个带噪声的 offset 观测

$$
z_t=\mu_{a_t}+\frac{r_t-p_t}{h_t},
\qquad R_t=h_t^{-1}.
$$

**实例：** 若冻结概率是 $p_t=0.8$、收到 $r_t=1$，则残差为 `0.2`，$h_t\approx0.16$，观测会把该 provider 的 offset 向正方向推；若 $r_t=0$，方向相反。该更新只能在 release tick 执行。

对应的 scalar Kalman/Laplace 递推为

$$
k_t=\frac{v_{a_t}}{v_{a_t}+R_t},
\qquad
\mu_{a_t}\leftarrow\mu_{a_t}+k_t(z_t-\mu_{a_t}),
\qquad
v_{a_t}\leftarrow(1-k_t)v_{a_t}.
$$

**实例：** 若当前 $v=0.04$、$R=1$，则 $k=0.03846$；一次标签只会产生很小的 offset 改变，避免单个延迟反馈把全局分数推坏。其他 provider 的状态不变。

这套 scalar 更新是 BayesianRouter/extended-Kalman 的特例，公式本身没有新颖性。它存在的理由只是作为安全切换的共同基线，不能单独写成 Proposed。

### 3.3 保守切换规则

令 $\beta=2$，将 logit 置信上下界转回概率

$$
L_{t,a}(x)=\sigma\left(\ell_a^0(x)+\mu_a-\beta\sqrt{v_a}\right),
\qquad
U_{t,a}(x)=\sigma\left(\ell_a^0(x)+\mu_a+\beta\sqrt{v_a}\right).
$$

**实例：** 对同一分子，若某候选的中心概率约为 `0.80`、$\beta\sqrt v=0.20$，其区间约为 `0.65` 到 `0.90`；这只是一条用于门控的保守区间，不是从 evaluation 标签算出的真实置信度。

非探索决策只允许在

$$
L_{t,a}(x_t)\ge U_{t,a_0}(x_t)+\eta,
\qquad \eta=0.02,
$$

时从 incumbent 切换到候选 $a$；否则调用 $a_0$。每个 batch（预先固定为 32 个决策）冻结 gate，释放的反馈只影响下一个 batch。为了避免候选永远没有反馈，另设固定探索概率 $\epsilon=0.10$，探索时在当前菜单均匀抽 provider；探索和利用都只调用一个 provider。

**实例：** 若候选 LCB 是 `0.94`、incumbent UCB 是 `0.90`，差值 `0.04` 大于 `0.02`，下一 batch 才允许切换；若差值是 `0.01`，即使候选中心分数更高也继续调用 incumbent。

### 3.4 与已有 baseline 的关系

- `Frozen`：$\mu_a=0$ 且永远调用冻结 argmax，是 DG-SafeJEV 的无更新对照。
- `BayesianRouter-style`：使用相同 selected-only scalar/向量近似，但没有 incumbent shield；DG-SafeJEV 的差异只在置信门和回退规则。
- `Online-SGD/Replay-SGD`：无显式安全回退，且允许一次反馈直接改 selected head。
- `Audit-Frozen`：使用同一 $\epsilon$ 探索率但永远沿用 Frozen，用来估计探索本身的代价。

因此 DG-SafeJEV 的额外内容是**决策规则**，不是额外观测；新颖性风险仍高。若它超过固定策略，也只能称“安全切换在该 benchmark 上有用”，不能声称发明了相关后验或置信序列。

### 3.5 最小 CPU 证伪卡

固定以下配置后再读取 evaluation 结果：5 个 scaffold outer folds、2 个 fit seeds、delay `0/1/4`、$\epsilon=0.10$、$\beta=2$、$\eta=0.02$、batch `32`、全菜单按当前 33-provider hash、每次只用 release 的 selected `system_success`。不调学习率、prior 方差、gate、探索率或菜单。

逐流保存 decision/terminal/feedback/update JSONL，并记录：累计错误、accuracy、切换率、探索率、gate-open 率、首次 gate tick、pending 数、更新耗时 p50/p95、源码和输入 hash。独立 NumPy 恢复器只从事件和冻结输入重算状态与选择。

**停止条件：**

1. 任意决策前读到未选输出、真值或未来 reward：该流作废。
2. 任意两个 delay 的 DG-SafeJEV accuracy 低于 `Audit-Frozen`：停止，不申请 A800。
3. 三个 delay 中没有至少两个达到或超过 Frozen，或 pooled accuracy 没有超过 train-only Best Fixed：停止直接 JEV 主线，不再调门槛。
4. gate 全程不开放：报告“当前数据没有可识别的 drift”，不把零切换当成功。

只有同时满足“至少两个 delay 超过 Frozen、且不低于 Best Fixed、独立恢复通过”才值得再提交**一条** A800 smoke；即使 smoke 成功，论文仍须把它作为安全决策规则而不是新后验。

## 4. 推荐的 post-call 动态 verifier 候选

### 4.1 为什么转到这里

这里有一个 pre-call selection 不具备的额外观测：provider 已经返回的 $c_t$。它不能帮助本次选择哪个 provider，却能决定当前答案是否接受、是否拒答或转交复核。已有静态 verifier 结果说明这条信息不是凭空构造的。

### 4.2 只使用 selected provider 的延迟校准状态

先在 fit 区固定一组置信度阈值

$$
\mathcal T=\{0.60,0.70,0.80,0.90\},
\qquad
\mathrm{accept}_{j,t}=\mathbf 1[c_t\ge\tau_j].
$$

**实例：** 某次返回 $c_t=0.86$，则对 $\tau=0.60,0.70,0.80$ 的假设接受记录为 1，对 $\tau=0.90$ 为 0；这些判断发生在 provider 返回后。

在 reward 到期时，对每个阈值维护假设接受样本数 $n_j$ 和错误数 $e_j$。若只允许使用 selected provider 的实际标签，则一次 release 对满足 $c_t\ge\tau_j$ 的阈值各更新一次：

$$
n_j\leftarrow n_j+\mathbf 1[c_t\ge\tau_j],
\qquad
 e_j\leftarrow e_j+\mathbf 1[c_t\ge\tau_j](1-r_t).
$$

**实例：** 若 $c_t=0.86,r_t=1$，则前三个阈值的 $n_j$ 各加 1，$e_j$ 不变；$\tau=0.90$ 不变。若 $r_t=0$，前三个阈值的 $e_j$ 各加 1。

对错误率使用透明的 Beta 后验作为风险状态

$$
\theta_j\mid H_t\sim\mathrm{Beta}(1+e_j,\ 1+n_j-e_j),
\qquad
U_j=Q_{1-\delta/|\mathcal T|}(\theta_j),
$$

其中 $Q_q$ 是 Beta 分布的 q 分位数，$\delta=0.05$ 只用于状态门控，不把它称成频率学置信保证。

**实例：** 若某阈值已有 $n_j=20$、$e_j=1$，其后验是 `Beta(2,20)`；$U_j$ 是这条阈值错误率的保守上界，具体数值由固定 Beta quantile 计算，不能用 evaluation 结果反调。

每个 batch 只在 release 后更新一次阈值。给定目标错误率 $\alpha=0.05$、最小支持 $n_0=20$，选择最小的已认证阈值

$$
\tau_{t+1}=\min\{\tau_j:\ n_j\ge n_0,\ U_j\le\alpha\};
$$

如果集合为空，使用 $\tau_{\max}=0.90$ 并将所有低于该值的答案拒答；这保持安全回退，不把支持不足当作低风险。

**实例：** 若 `0.60` 和 `0.70` 都没有通过风险门、`0.80` 通过，则下一 batch 使用 `0.80`；若没有任何阈值有足够支持，使用 `0.90`，而不是把阈值降到一个未经证实的值。

这条候选是**动态 verifier/refusal**，不是 direct pre-call selector。它的创新边界应写成“延迟 selected-only 标签下的风险门控”，不能写成发明 Beta 后验或 conformal prediction。

### 4.3 与已经通过的静态二次调用结果

已有的固定阈值回放显示：第一工具置信度低于 fit-only `0.8` coverage 阈值时，追加第二 provider 并采用第二结果，pooled accuracy 为 `0.923823`，同 fallback 数量的随机追加为 `0.904432`，平均调用次数为 `1.206`。两工具 oracle 只是上界，第二 provider 单独调用的 pooled accuracy 为 `0.889197`；因此该收益来自“何时追加验证”的规则。这个结果仍不包含在线状态更新，不能直接称动态 JEV，但它给出了第 4.4 节动态候选最清楚的起点：阈值和第二 provider 的可靠性必须在延迟 selected-only feedback 下更新，并用静态 escalation 作为强对照。

一题若发生 escalation，会有 first 和 second 两个已调用 provider。实现时必须把“first provider 假设接受集”的风险状态与“second provider 最终采用结果”的风险状态分开；不能把 first 被拒绝的标签直接当成最终答案标签，混进第二 provider 的错误率。两类事件都可以按 selected-only 延迟队列更新，但状态键至少要包含 provider 和角色。

### 4.4 最小 CPU 证伪卡

使用已经缓存的 BBB round1 numeric probability，不新增 provider 调用；5 folds、2 seeds、delay `0/1/4`，batch `32`，只在 release tick 更新。对照固定 fit-only `\tau=0.8` verifier、实际接受数匹配的随机拒答、始终接受和始终拒答。报告 accepted coverage、accepted accuracy、accepted error、拒答率、阈值路径、首次满足支持的位置、实际调用次数和端到端更新时间。动态阈值的平均调用次数上限固定为 `1.25`，与已经通过的静态 escalation（`1.206` 次调用）使用同一成本量级；超过上限的流不按准确率优先。由于 selected-only 反馈来自当前已经调用的工具，这个风险状态只对实际调用分布作条件描述，不宣称对未调用工具或任意未来分布有无偏保证。

预登记停止线：

- 任意 evaluation probability 在决策前泄漏，整流取消；
- 动态阈值在至少 2 个 delay 低于固定 fit-only verifier，停止这条动态扩展；
- 动态阈值没有达到 $n_0$ 而全程使用回退，报告“短流中没有可识别的在线校准”，不调小 $n_0$；
- 只有当动态阈值在至少 4/5 folds 保持不高于固定 verifier 的 accepted error、并且 coverage 没有下降超过预登记的 0.05、平均调用次数不超过 `1.25`，才考虑一条 A800 smoke。

这条实验即使通过，也只能支持“调用后可信度可被实时风险控制”；它不能替代 direct selection 的主指标。

## 5. 方法决策和论文故事

### 现在应该做什么

1. 不再提交普通 JEV、SGD、BayesianRouter、provider transfer 或 context memory 的新变体。
2. 若需要最后一次直接选择诊断，按第 3 节固定 DG-SafeJEV 在 CPU 上跑一次；不过闸门即停止 direct dynamic 主线。
3. 优先实现第 4.4 节动态 verifier 的 CPU replay；它有已经通过静态筛选的额外调用后信号，实验成本最低。
4. 只有预登记 CPU 闸门通过，才按“一次一任务”提交 Nebula A800；完整日志必须包含配置、事件、源代码 hash、输入 hash、失败和独立恢复。

### 可以复用到论文里的东西

- 33 个具体 provider、独立 reference truth、scaffold-disjoint splits 和失败计分规则；
- train-only Best Fixed、Frozen、Online-SGD、Replay-SGD、PILOT-adapted、BayesianRouter-style 现代对照；
- selected-only delayed feedback contract、pending queue、exactly-once release 和独立 NumPy recovery；
- round drift 审计和多条动态候选的负结果；
- post-call confidence 的 AUC 与 fit-only verifier 的独立复核；
- 如果动态 verifier CPU/A800 通过，可形成“选择—调用—复核”的两阶段 agent 工具可信性故事；如果不过，则保留静态 verifier 和系统负结果，避免虚构动态收益。

### 最终审稿口径

审稿人会问：工具为什么需要在线更新、更新看到了什么、为什么未选 provider 可以被纠正、是否超过强固定策略、拒答是否被错误算成正确。当前证据只能回答：直接 selected-only 更新尚未超过强固定策略；调用后 confidence 有真实复核信息。继续把标准安全切换改名为“动态 JEV”不会增加答案的可信度。先把信息边界、负结果和可复核 verifier 做干净，才有一条能经得住 AAMAS 审查的故事线。
