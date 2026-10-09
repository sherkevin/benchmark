# PAR-JEV 数学与方法审查

- 审查日期：2026-09-28
- 范围：只审查 research/task49_composition_20260928/negative_diagnosis.md 的 PAR-JEV 候选；不修改生效主线、benchmark、baseline 或决议。
- 判断性质：区分已由代码/日志支持的事实、公式成立所需假设和下一步检验。PAR-JEV 还没有实现或实验结果，不能称为已验证方法。

## 1. 总判断

PAR-JEV 是一个可实现的“冻结上下文模型 + 低维 provider 校准残差”候选，工程上比当前 64 维全量在线 SGD 更容易控制更新幅度。其提议的单 provider 分数为

$$
P(S_a=1\mid v)
=
u_a(v)\left[q(v)h_a(v,1)+(1-q(v))h_a(v,0)\right],
$$

其中 $S_a$ 是 provider 有效返回且预测正确，$u_a=P(V_a=1\mid v)$，$q=P(Y=1\mid v)$，$h_a=P(S_a=1\mid V_a=1,v,Y=y)$。

这里有一个必须修正的数学前提：把 $u_a(v)$ 提到求和号外，隐含了 $V_a\perp Y\mid v$。一般的全概率分解应为

$$
P(S_a=1\mid v)=\sum_{y\in\{0,1\}}q_y(v)u_a(v,y)h_a(v,y),
$$

其中 $u_a(v,y)=P(V_a=1\mid v,Y=y)$。只有在“有效返回与真实性质条件独立”被数据或设计支持时，才能退化为候选中的 $u_a(v)[q h_1+(1-q)h_0]$。这不是 provider 之间的独立性问题，而是同一 provider 的 availability 与 property 的条件独立假设；当前日志尚未证明它。必须把 $h_a$ 定义为条件于有效返回的正确率，否则乘 $u_a$ 会重复或漏算失败概率。

当前候选不能支撑三个较强说法：

1. 标量递推不是 logistic 后验或精确 MLE；最多是带曲率锚定的 one-step online-Newton/ONS 风格更新。
2. selected-only 反馈不能识别未被选择 provider 的在线残差，也不能保证 $q$ 的样本对总体无偏。
3. “更新 $O(1)$”只适用于释放时已缓存 baseline logits 且 offset 状态很小；菜单评分仍为 $O(Kd)$，pending 队列和日志空间随未释放事件增长。

建议只做一轮 CPU 证伪，不提交 A800；以下边界修正前，不应写入主方法。

## 2. 标签与反馈合同

令 $Y$ 为二元性质，$Z$ 为已选 provider 的二元输出，$C$ 为 chemical correctness，$V$ 为有效返回。可恢复标签只在 $V=1$ 且反馈可评分时存在：

$$
\tilde Y=
\begin{cases}
Z,&C=1,\\
1-Z,&C=0.
\end{cases}
$$

例如 $Z=1,C=1$ 得 $\tilde Y=1$；$Z=1,C=0$ 得 $\tilde Y=0$；timeout 没有 $Z$，只能给 $u$ 目标 0。输出非二元、reference 冲突或 provider/version 不一致时，整条事件不进入恢复分支。

释放事件的三个目标应是：

- $b_u=1$（有效返回）或 $0$（失败）；
- $b_q=\tilde Y$，仅在成功且可评分时；
- $b_h=C$，仅在成功且可评分时，对被选 provider 和对应 $Y$ 条件更新。

fit 区也要保持相同语义：$u$ 用 status 标签，$h$ 只用有效返回行，不能用失败的 system-success 0 伪装成 $C=0$。这条是可验证的合同，不是实验结论。

若不愿接受 $V_a\perp Y\mid v$，就不能只维护一个 $u_a(v)$：需要联合的 $u_a(v,y)$，但失败事件没有 $Y$，无法按现有 selected-only 合同更新其失败分支。此时必须改写模型或把结论明确限制在条件独立假设下；不能在实验后把这项假设当作实现细节。

## 3. 标量递推并非精确后验

PAR-JEV 提议

$$
I_{t+1}=I_t+p_t(1-p_t),\\
c_{t+1}=c_t+\frac{b_t-p_t}{I_{t+1}},
\qquad p_t=\sigma(\ell_0(v_t)+c_t).
$$

这是对当前标量做一次 Newton 方向更新、用累计曲率缩放的递推；它不是在每一步求解

$$
\arg\min_c \left[
\frac{\lambda c^2}{2}
+\sum_{i\le t}\operatorname{BCE}(\sigma(\ell_{0,i}+c),b_i)
\right].
$$

历史梯度在新 $c$ 处会改变，单独保存 $I$ 不足以重算它；二元 logistic 也没有共轭的精确 Gaussian posterior。应删掉 Fisher posterior/MLE 暗示，称为曲率锚定的一步在线更新。若要声称 MAP 或 Laplace 近似，必须保存累计梯度/局部二次模型并给出近似定义。

$I_0$ 若定义为 fit 区 Fisher 曲率和，还需说明 $c_0=0$ 为何是 fit 目标中心；当前无截距 baseline 一般不满足。稳妥做法是把 $I_0$ 定义为预先固定的 trust precision，不称 fit posterior；或同时实现正则化 Newton/批量校准控制，报告 PAR 只是近似递推。

CPU 前应做 scalar synthetic check：固定 logits 和 binary labels，比较 PAR 轨迹、每步高精度梯度/二阶近似和批量正则化解，覆盖 $I_0\to0$、大 $I_0$、全 0/全 1、交替标签、反馈乱序。检查应验证声明的 one-step 递推，而不是假设等于后验。

## 4. selected-only 可识别性和实时复杂度

无探索时，$h_{a,y}$ 与 $u_a$ 只在 provider $a$ 被选择时得到观测；永远未选 provider 的 offset 恒为 0，这不是可靠性证据。$q$ 的恢复标签来自当前 policy 选中的分子和成功 provider，不能当作全体分子上的无偏监督；unknown/reference arrival 又会造成选择性缺失。

可识别性应限定为：

> 在当前 policy 产生的 selected-and-scored 流条件下，对有足够支持的 provider/condition 做在线校准。

若要做 provider 全体或跨策略主张，须提前固定统一探索/propensity 规则，并加入同探索 Frozen；否则报告每个 $(a,y,V)$ 的 support、选择比例和 unknown 比例，零支持单元不作动态结论。已有 provider transfer AUC 只说明全 panel 存在信息，不能证明 selected-only 无偏；PAR 的独立 offset 也不利用该相关性。

复杂度应写成：

- 选择：读取 $q$、每个 $h_a(0/1)$、$u_a$ 的冻结 logits，$O(Kd)$；
- 释放：决策时缓存所选 provider 的三个 baseline logits时为 $O(1)$，否则至少 $O(d)$；
- 静态模型内存 $O(Kd)$，动态 offset/曲率 $O(K)$；
- pending 队列空间随未释放事件数增加。

延迟合同还要明确缓存哪个 logit/版本。释放时若用更新后的 $c$ 重新解释旧事件，仍可定义，但应称 delayed online gradient，不能说是决策时后验回放。

## 5. 反事实与 baseline 公平性

PAR 同时加入可用性标签和低维更新，不能只和没有 $u$ 的 structured 比。最小匹配组应含：

1. Frozen $q/h$；
2. Frozen $q/h/u$，隔离静态 $u$；
3. q-only、h-only、u-only、q+h；
4. PAR q+h+u；
5. 使用相同 status 反馈、相同表示和初始化的 direct system-success SGD；
6. Best Fixed、Oracle 只作控制。

所有方法共享 fit rows、validation 规则、encoder/checkpoint、菜单、delay、随机数、失败和 unknown 处理。PAR 在失败时更新 $u=0$，而旧 structured 不更新；若 direct baseline 不同步接收失败 0，任何增益都无法归因于残差校准。报告 provider coverage、选择一致率以及 primary system-success，不能只报 accuracy。

需要主动排除的替代解释：

- 增益可能只来自把失败单独放进 $u$，而不是 q/h 残差；Frozen q/h/u 和 u-only 能拆开这两件事。
- 稳定性改善可能只来自标量、小步长和锚定；同样标量的 direct-success offset 才能检验是否需要真值恢复。
- delay 或事件顺序可能被误写成动态适应；同流 shuffled-order、无更新 Frozen 和固定版本块能区分它。
- provider family 的共错或 transfer 信号没有进入独立 offset；效果不存在时，不能用全 panel 相关性补写成在线 transfer。

## 6. CPU gate 与停止条件

- **G1 合同：** 独立 verifier 能拒绝失败生成 $\tilde Y$、unknown 更新 q/h、错绑 provider/query、重复 release、过期 version。
- **G2 数学：** synthetic scalar replay 与声明递推逐事件一致，并输出其与正则化 batch 解的差异。
- **G2a 因子分解：** 只在 fit/validation 做 availability 的 $u(v)$ 与 $u(v,y)$ 对照，并按 scaffold 报 status-by-$y$ 差异；若 $u(v,y)$ 有稳定增益，拒绝无条件的 PAR 因子分解，不能把它藏在实现里。
- **G3 冷启动：** 全部 offset 为 0 时，PAR 分数逐字节等于 Frozen q/h/u；只开 u 时只改变 status 项。
- **G4 支持：** 固定报告每个 provider、每个 $y$ 和 status 的 selected support；无支持单元不作在线效果主张。若使用 propensity，提前锁定审计概率并加入 Audit-Frozen。
- **G5 机制反事实：** 同流、同 delay $\{0,1,4\}$、5 scaffold folds × 2 seeds，比较上述控制；报告 system-success、valid-return accuracy、Brier/log loss、coverage、decision/update p50/p95 和 scaffold-cluster paired uncertainty。
- **G6 动态性：** 当前静态 BBB 回放没有自动证明环境漂移。至少按预登记 provider version/arrival 或块状漂移做一项，并做同流 shuffled-order 对照；没有 drift 时的收益只能叫校准/回放收益。

停止条件应在实验前写入卡：G1/G2 任一失败，或 PAR 在两个以上 delay 不超过同探索 Frozen，停止候选；若只超过 Frozen 而未超过 matched direct SGD/Best Fixed，结论限于安全校准对照，不升格为动态 JEV。

## 7. 审查结论

PAR-JEV 值得一次低成本 CPU 证伪，因为它针对全向量更新造成排序破坏，提出冻结上下文排序、只校准少量 provider 状态的机制假设。但它目前不是“实时 Fisher JEV”，也没有解决 selected-only 的 off-policy 识别。CPU 通过也只能说明低维校准在已声明信息合同下有信号；还需新鲜 endpoint/version 事件和独立 confirmation 才能支持更强故事。若不通过，应保留负结果，不通过调小 $I_0$、改支持阈值或反复重排 baseline 追分。
