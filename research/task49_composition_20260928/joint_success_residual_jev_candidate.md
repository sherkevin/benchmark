# Joint-Success Residual JEV 候选

日期：2026-09-28  
状态：候选；尚未进入 CPU 效果预筛。

## 动机

因子化 PAR-JEV 需要 $V_a\perp Y\mid v$。fit-only availability audit 在五个
scaffold fold 上观察到 provider 的两组有效返回率差异约为 0.18–0.21（provider
平均），最大约为 0.40–0.46，因此不把该前提当作当前任务的默认事实。

更干净的建模方式是直接预测任务真正关心的端到端成功事件：

$$
S_{t,a}=1\quad\Longleftrightarrow\quad
\text{provider }a\text{ 有效返回且返回值正确}.
$$

冻结模型提供每个 provider 的 query-specific logit $\ell_{a,0}(v)$，在线只
维护一个 residual $c_{t,a}$：

$$
\hat s_{t,a}=\sigma(\ell_{a,0}(v_t)+c_{t,a}).
$$

它不拆开 availability 和 property，也不需要 provider 彼此独立或
$V_a\perp Y\mid v$。反馈仍是 selected-only：失败给 $S=0$；有效且 correctness
为 1 给 $S=1$；有效但 correctness 为 0 给 $S=0$；unknown 不更新。

## 在线递推

每个 provider 保存 offset $c_a$ 和信任精度 $I_a$。令
$p_t=\sigma(\ell_{a_t,0}(v_t)+c_{a_t})$，释放一个可评分目标 $b_t$ 时：

$$
I_{a_t}^{+}=I_{a_t}+p_t(1-p_t),\qquad
c_{a_t}^{+}=c_{a_t}+\frac{b_t-p_t}{I_{a_t}^{+}}.
$$

这是曲率锚定的一步 online update，不是精确 logistic posterior 或 MLE。决策时
缓存选中 provider 的冻结 logit，因此释放更新为 $O(1)$；菜单评分仍为 $O(K)$，
动态状态为 $O(K)$。

## 现有实现

- kernel：`src/probe_headless/joint_success_residual_jev.py`
- 合同测试：`src/probe_headless/tests/test_joint_success_residual_jev.py`
- 19 项候选 kernel 测试（含 PAR 因子化候选）已通过。

## 必须做的对照

1. Frozen joint-success logits；
2. 相同冻结 logits、相同 selected-only 反馈的 direct Online-SGD；
3. 相同标量 residual 但不使用曲率锚定的 fixed-step offset；
4. Best Fixed 和 Oracle 作为控制。

CPU 预筛固定 5 scaffold folds × 2 seeds × delay `{0,1,4}`，先验证回放、支持数、
失败/unknown 语义和 paired uncertainty；没有通过前不申请 A800。

## 边界

它解决了因子化假设问题，但仍可能只是一个高效的低维在线校准器。是否形成论文
方法，必须由 matched baseline、更新成本、跨 provider 支持和动态事件上的证据决定，
不能仅凭公式或通过合同测试宣布创新成立。
