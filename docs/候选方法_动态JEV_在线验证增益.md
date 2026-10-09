# 候选方法：动态 JEV 的在线验证增益学习

**状态：** 候选主线，尚未冻结；M1 与 M2 gain-gate 合同 runner 已实现，尚未做方法效果结论。

## 1. 先把任务痛点说清楚

我们现在的任务不是简单地找“全局最准确的 provider”。真正的决策发生在已经看到一部分工具结果之后：

> 当前结果能不能直接用？如果再调用一个 provider，它带来的新增证据是否足以降低最终错误？

现有几类方法分别解决了其中一部分，但没有解决这个组合问题：

- 静态 provider accuracy 不知道当前历史下的条件收益；
- 普通 routing bandit 学的是选择某个 provider 的回报，不能直接区分“调用 provider 的绝对效果”和“相对当前停止动作的新增效果”；
- AnyJev 的闭式 head 可以做判断校准，但没有定义第二次工具调用的干预收益；
- Bellman/VOI 需要工具输出的条件分布，Task36 当前没有这个分布模型；
- 真值只对已调用工具返回，而且会延迟到达，未调用工具不能补标签。

因此最值得解决的缺口是：**在 selected-only、delayed feedback 下，在线学习 verification gain，并用它控制是否追加调用。**

## 2. 把追加工具调用视为一个干预

先固定一个基线终止策略。对 episode $i$ 的当前历史 $h_i$，定义：

- $L_i^0$：现在直接作答或拒答的损失；
- $L_i^p$：追加调用 provider $p$，再按固定终止规则处理的损失；
- $\Delta_i(p)=L_i^0-L_i^p$：追加 $p$ 的潜在验证收益。

真正想学的是条件收益：

$$
\tau_p(h)
=
\mathbb E[\Delta_i(p)\mid h_i=h].
$$

$\tau_p(h)$ 大于调用成本，才值得追加调用。它与“provider $p$ 自己有多准”不是同一个量。

**z002905 case：** CNN 返回标签 0 后，当前基线如果直接作答，损失为 1；追加 Morgan 后得到标签 1，损失为 0。这个 episode 的验证收益为 1。若 CNN 已经正确，追加 Morgan 可能没有收益，甚至因为调用成本使总损失变差。

## 3. 为什么必须有随机复核 probe

令 $A_i(p)$ 表示是否随机 probe provider $p$，并令：

$$
\Pr(A_i(p)=1\mid h_i)=\eta_i(p\mid h_i),
\qquad
\eta_i(p\mid h_i)\geq\eta_{\min}>0.
$$

只有 $A_i(p)=1$ 时，$\Delta_i(p)$ 才可观测。使用逆倾向权重：

$$
\mathbb E\left[
\frac{A_i(p)\Delta_i(p)}{\eta_i(p\mid h_i)}
\middle|h_i
\right]
=
\mathbb E[\Delta_i(p)\mid h_i]
=\tau_p(h_i).
$$

这个等式给出识别依据：在随机 probe、正概率覆盖和固定终止规则下，selected-only 反馈可以估计条件验证收益。没有随机 probe 时，模型只看到当前策略愿意调用的样本，不能把结果解释成通用 VOI。

## 4. 线性状态只是高效实现，不是主要创新

冻结 Laya/AnyJev 表示，用固定非负映射得到：

$$
k_i=\phi(h_i,p)\in\mathbb R_+^D.
$$

维护验证收益状态：

$$
G_{n,p}
=
\rho G_{n-1,p}
+
\sum_{i:\tau_i=n,\,A_i(p)=1}
\frac{k_i\Delta_i(p)}{\eta_i(p\mid h_i)},
$$

$$
Z^g_{n,p}
=
\rho Z^g_{n-1,p}
+
\sum_{i:\tau_i=n,\,A_i(p)=1}
\frac{k_i}{\eta_i(p\mid h_i)}.
$$

查询为：

$$
\widehat\tau_{n,p}(h)
=
\frac{\phi(h,p)^\top G_{n,p}}
     {\phi(h,p)^\top Z^g_{n,p}+\varepsilon}.
$$

$h$ 必须包含已经看到的工具返回。这样第二次调用 Morgan 时，$\phi(h_2,\mathrm{Morgan})$ 可以条件化于 CNN 的具体结果，模型不需要假设 provider 彼此独立。

该递推可以用 linear attention 的结合律证明为一个固定核下的加权局部估计器，更新和查询为 $O(D)$。它解决的是计算问题；方法创新在于它估计的是经过随机干预识别的 verification gain。

## 5. 风险门控

第一版用保守门控：

$$
\mathrm{CALL}(p)
\quad\Longleftrightarrow\quad
\widehat\tau_{n,p}(h)-\epsilon_{n,p}(\delta)
>
c_{\mathrm{call}}(p).
$$

其中 $\epsilon_{n,p}$ 来自有效样本量、bootstrap 或预注册的置信界。如果门控不通过，就沿固定规则直接作答或拒答。

更严格的版本再增加一个 after-call risk head，要求：

$$
\mathrm{UCB}\bigl(\Pr(\mathrm{unsafe}\mid h,p)\bigr)\leq r_{\max}
$$

这样工具调用同时受收益和安全风险约束。

## 6. 这条线的潜在创新点

随机化、propensity weighting 和条件处理效应本身也不是新工具；已有 causal bandit 和 delayed causal inference 工作。我们不能把 IPW 公式单独当贡献。能否成立，取决于它是否解决了 typed tool result、终止后真值、第二次调用的条件收益和主 Agent 风险门控这个具体组合问题。

如果实验和理论成立，主贡献可以写成三个层次：

1. **问题层：** 将工具复核定义为 delayed selected-only feedback 下的 conditional intervention gain，而不是静态 provider ranking。
2. **方法层：** 用随机 probe + inverse propensity 学习 verification gain，并通过固定大小的线性状态实时更新。
3. **决策层：** 用带置信界的 gain/risk gate 控制追加工具调用，在预算和风险约束下改善 risk–coverage–calls trade-off。

线性状态本身不作为主要创新，只作为可证明的实现机制。

## 7. 需要证明和验证的内容

### M1 合同已实现的边界

当前 `src/probe_headless/m1_dynamic_jev.py` 已实现固定首 provider、随机二次 probe、delayed gain queue 和 HT/IPW feedback 更新；`m2_dynamic_jev.py` 加入 HT 置信半径、alpha spending 和 gain/STOP gate。它把 `after_loss` 定义为“第二次 probe 的输出直接作为最终答案”的损失，因此目前仍是 provider-marginal 合同；还没有基于历史内容的 encoder，也没有最终的 context-conditioned JEV。

### 可以做成理论结果

- 随机 probe 下 IPW 对条件 verification gain 的识别式；
- 固定非负特征核下递推状态与加权局部目标最小解的等价性；
- 更新、查询和存储复杂度；
- overlap、bounded gain 和稳定分布下的误差界；
- 置信门控误开通的概率界；
- 延迟和漂移对跟踪误差的影响。

### 不能靠公式预先保证

- 一定优于 LinUCB、RLS、AnyJev 或 static JEV；
- 一定提高最终准确率；
- 一定减少调用次数；
- 一定能代表真实生产环境的墙钟实时学习。

## 8. 最小实验顺序

- **M0：** 只学习 selected provider correctness，验证在线状态是否比静态 head 更快适应；合同 runner 已实现；
- **M1：** 固定首个 provider 和终止规则，用随机 probe 学 verification gain；合同 runner 已实现；
- **M2：** 让带 HT 置信门的 gain gate 控制追加调用；合同 runner 已实现，当前只覆盖 provider-marginal 目标；
- **M3：** 加入 provider drift、延迟和冷启动，测试遗忘与恢复。

最关键的对照不是 random，而是：静态 JEV、普通 online linear head、LinUCB/LinearTS、AnyJev 周期性 `observe`、always-verify、never-verify。

## 9. 当前判断

这条线比“做一个实时更新的 JEV”更像一个完整研究问题，因为每个模块都对应一个真实痛点：

- 随机 probe 解决 verification gain 不可识别；
- IPW 解决 selected-only 反馈偏差；
- 线性状态解决延迟反馈下的历史存储和更新速度；
- gain/risk gate 解决主 Agent 什么时候应该继续调用工具。

但它仍是候选创新，必须先做近邻 causal bandit / active model selection 检索和 M0–M2 结果，才能决定是否足够支撑 AAMAS 主轨。
