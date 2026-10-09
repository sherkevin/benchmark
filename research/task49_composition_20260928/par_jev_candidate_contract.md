# PAR-JEV 候选实现合同 v0

状态：候选；只用于 CPU 合约测试，不是生效方法，也没有效果结论。

实现：\`src/probe_headless/candidate_par_jev.py\`。

## 数学边界

冻结输入为每个 query 的 logits：

$$
\ell^q_0(v),\qquad \ell^h_{a,0}(v,0),\qquad
\ell^h_{a,0}(v,1),\qquad \ell^u_{a,0}(v).
$$

在线状态只保存 $O(K)$ 个标量 offset 和 precision：一个 $q$、每个 provider 的两个 $h(a,y)$、每个 provider 的一个 $u(a)$。选择分数是

$$
s_a(v)=\hat u_a(v)
\left[\hat q(v)\hat h_a(v,1)+(1-\hat q(v))\hat h_a(v,0)\right].
$$

这只是 $V_a\perp Y\mid v$ 条件下的因子化近似。一般模型应使用 $u_a(v,y)$：

$$
P(S_a=1\mid v)=
\sum_y q_y(v)u_a(v,y)h_a(v,y).
$$

代码要求调用者显式传入 \`assume_availability_independent=True\`；没有该声明就拒绝构造对象，不把近似冒充恒等式。若 availability 与性质相关，当前 selected-only 失败事件没有 $Y$，此候选不能直接修复。

每个标量状态的更新是：

$$
I^+=I+p(1-p),\qquad
c^+=c+\frac{b-p}{I^+},\qquad
p=\sigma(\ell_0+c).
$$

这是曲率锚定的单步 online update，不是精确 logistic posterior 或正则化 MLE。初始 $I_0$ 是 trust precision，必须预先固定。

## 反馈合同

\`ParJEVFeedbackQueue\` 只在 due tick 调 \`observe\`，并强制

$$
\mathrm{due\_tick}=\mathrm{decision\_tick}+1+\mathrm{delay}.
$$

反馈只绑定当前 selected provider，且 event exactly once：

- \`failure\`：没有输出标签、没有 correctness；只更新 $u$，目标 0；
- \`unknown\`：有二元输出但 reference 未评分；只更新 $u$，目标 1；
- \`scored\`：有二元输出和 correctness；更新 $u=1$，用
  $\tilde y=z$（correct）或 $1-z$（wrong）更新 $q$，并更新所选 provider 对应 $\tilde y$ 条件的 $h$，目标为 correctness。

未选 provider、当前题真值和完整 provider panel 不进入 kernel。决策时只把所选 provider 的四个 baseline logits 存进 pending，因此释放更新为 $O(1)$；全菜单打分仍为 $O(Kd)$（冻结 logits 的计算不计入 kernel）。

## 证据边界

合约测试只验证公式、状态和时钟，不证明效果、创新或 selected-only 识别。效果前仍需：

1. fit/validation 上做 $u(v)$ 与 $u(v,y)$ availability audit；
2. 报告每个 $(a,y,V)$ selected support；
3. 与 Frozen $q/h/u$、q-only、h-only、u-only、direct system-success SGD 预注册比较；
4. 在有明确 drift/version 事件时才使用 dynamic adaptation 叙述。

