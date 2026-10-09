# MIRA-Relay context-relative reliability v1

日期：2026-10-06  
状态：开发诊断已完成；controlled drift stress 已完成；自然 fresh confirmation 结果已保留，不再调参。

## 方法

冻结 JEV/判别式 prior 给出 molecule-conditioned provider score $q_0(a|x)$。每个 provider 在 development fit 上有固定 success rate $ar r_a$。selected-only、release 后，维护 $n_a$ 次已释放反馈和 $s_a$ 次 correctness success：

$$\hat r_t(a)=rac{n_0ar r_a+s_t(a)}{n_0+n_t(a)},\qquad\Delta_t(a)=\hat r_t(a)-ar r_a.$$

动作分数是

$$q_t(a|x)=q_0(a|x)+\Delta_t(a),$$

并对已观测 runtime failure 的 provider 施加 hard availability gate。$n_0=20$，只更新 selected provider；冷启动时 $\Delta_0=0$，精确退化为 Frozen。反馈的 release clock 仍为 $	au_t=t+1+D$。

## 事前否证

- selector 读取 truth、未选输出或未到期反馈；
- cold-start score 与 Frozen 不相同；
- state/update 无法由 selected-only release 事件独立重放；
- controlled drift 中 MIRA 相对 Frozen 的 scaffold-cluster CI 不为正；
- natural fresh menu 上出现负面损失超过 1 pp。

自然批次不是调参集；其结果固定为 MIRA=Frozen，作为稳定性边界。
