# MIRA-ContextShield CPU 小流预登记

日期：2026-10-02  
状态：待实现；本文先于实验。

## Claim

在 common BBB development cache 上，context-conditioned logit-surrogate shield
能在 fixed fit-only fallback 上做可逆的 provider 切换，并减少 SafeSwitch 的锁定型
误伤。该 claim 只覆盖 feedback-adaptive、selected-only、延迟 `{0,1,4}` 的开发协议。

## 固定输入和对照

- fold/seed/feature/provider menu/cache 沿用 common-stratum manifest；confirmation 关闭；
- Frozen direct、fit-only Best Fixed、Matched Online-SGD、MIRA-FamilyGuard、SafeSwitch
  作为对照；ContextShield 只在小流实现完整前运行；
- fit-only Fisher ridge $\lambda=0.01$、fit 平均曲率（与 direct BCE mean loss 同尺度）、Bernoulli local-MAP bisection 80 iterations、
  $\delta=.05$、$\delta_h=.05$、runtime hazard slack $\eta_h=.02$；不在 evaluation 上搜索；
- fallback 固定为每 fold fit-only Best Fixed；安全门永远比较不可变的 fit-only 后验参考，不能因一次在线选择改写 reference。由于当前协议没有 refusal action，fallback 始终 action-eligible；其 runtime failure 只记入 hazard ledger。
- 运行只记录 selected feedback，延迟释放仍为 `decision_tick + 1 + delay`。FamilyGuard 只保留 hard availability block，不使用 soft family reliability factor。
- 运行失败账本使用 $h^U_{a,t}=\operatorname{BetaQuantile}_{.95}(1+F^{fit}_a+F_{a,t},1+V^{fit}_a+V_{a,t})$；fallback 比较使用固定的 fit-only $h^U_{c,0}$。
- `direct_prior` 只作为 BCE/logit surrogate 的 fit warm start；在线目标统一为 `system_success`：有效返回时 `y=chemical_correct`，runtime failure 时 `y=0`，unknown/refusal 不更新。runtime failure 同时只在独立 hazard ledger 计数。

## 必须的机制对照

进入 full receipt 前，必须在同一 fold/seed/delay/菜单上补齐：同一 Fisher 初值但无 gate、同一
gate 但冻结 surrogate、immutable fit fallback 与 mutable incumbent、以及去掉 family availability
前置约束的版本。还要补 no-hazard 与 hazard-only/no-Gaussian 两个故障语义消融。否则无法把收益归因于 shield 的不确定性门，而不是初始协方差或可用性规则。

## 进入回放前的停止测试

先用 fit snapshot 和 evaluation features 做静态 gate-open 诊断，不读取 evaluation labels。若
预登记的 $(\delta,\delta_h,\eta_h)$ 在整个 fold 没有任何可行 challenger，则停止该候选；
不能事后调阈值让门打开。

## 必须记录的指标

task success、fixed-reference harmed/rescued/both 四格、reference switch-away、首次
release 后的 action 改变、posterior state change、runtime failure、unknown、selection/update
p50/p95、state bytes，以及 scaffold-cluster paired uncertainty。所有数字必须由独立
verifier 重算。

## 否定条件

1. ContextShield 在小流没有任何 posterior/state/action 的 release 后变化；
2. fixed-reference harmed 不低于 SafeSwitch v5 的同合同探索结果，且 rescued 没有保留；
3. 任何未选 provider 的标签进入 action 或 update；
4. Fisher 初值、posterior 更新或 event clock 无法独立恢复。

否定条件触发时停止该构造，保留负结果；不通过调 $\lambda$、$\delta$ 或 episode 顺序
追求正数。小流通过后再决定是否登记 full common-stratum receipt。
