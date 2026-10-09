# Delayed dynamic verifier/escalation v1

日期：2026-09-28

## 问题

静态 confidence escalation 已经显示低置信度结果值得追加第二工具。本实验检验：在相同调用预算约束下，能否用已经释放的 selected-only correctness 更新下一批的 confidence risk gate。

## 状态与信息权限

- 每个 fold 的第一、第二 provider 只由 fit 区正确率排序确定。
- confidence 是调用后 $c=2|p-0.5|$。用第一 provider fit confidence 的分位数 $q\in\{0.5,0.8,0.9,1.0\}$ 定义四个固定候选阈值。
- 每个阈值对第一 provider 维护独立的成功/失败 Beta 状态，初始化为 fit 区该阈值假设接受集的计数加 Beta$(1,1)$。
- 第二 provider 也维护自己的 Beta 计数，但只记录第二次真实调用的 confidence 和 correctness；它的标签不更新第一 provider 状态。
- 如果第一 provider 调用失败、没有 numeric confidence，则强制追加第二 provider；该第一调用不进入 confidence-bin 状态。第二 provider 同样失败时，最终 reward 为 0，且该事件不进入第二 provider 的 confidence-bin 状态。
- evaluation 决策只读当前已释放状态。反馈按 delay `0/1/4` 到期后 exactly-once 更新；等待中的反馈不可见。每 32 个决策冻结一次阈值。

## 风险门与动作

对阈值 $j$ 维护错误率后验 $\mathrm{Beta}(1+e_j,1+n_j-e_j)$。固定 $\delta=0.05$、最小支持 $n_0=20$、目标错误率 $\alpha=0.05$。批次开始时选择数值最小且满足

$$
n_j\ge n_0,\qquad Q_{0.95}\!\left(\mathrm{Beta}(1+e_j,1+n_j-e_j)\right)\le\alpha.
$$

没有阈值通过时使用四个候选中数值最大的阈值，作为保守回退。第一 confidence 低于当前阈值时追加第二 provider，并采用第二 provider 输出；否则采用第一 provider 输出。每次追加调用计入成本。

## 对照与闸门

同一 stream 同时计算 fit-only 静态 q=0.8 escalation、始终接受、动态候选和两工具 oracle。动态候选必须在每个 delay 报告最终答案 accuracy、fallback rate、平均调用次数、阈值路径、第一/第二角色更新数。

只有同时满足以下条件才进入 A800 smoke：至少两个 delay 中动态 pooled final-answer error 不高于静态 escalation，至少 `4/5` folds 的最终答案 accuracy 不低于静态值 `0.05` 以内，平均调用次数不超过 `1.25`。若阈值全程回退或成本超限，报告支持不足/成本失败，不调 $\alpha$、$\delta$、$n_0$ 或 batch。

没有新 provider 调用，没有 confirmation，失败结果照常保留。
