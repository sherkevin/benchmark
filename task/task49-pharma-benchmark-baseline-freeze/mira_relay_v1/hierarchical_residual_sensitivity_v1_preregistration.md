# Hierarchical residual sensitivity v1

状态：候选机制的 CPU development 预登记，2026-10-04；不替代 v3，不打开 confirmation，不提交 Nebula。

## 1. 目的

v3 transfer diagnostic 显示 MIRA 的 family residual 大多发生变化，但 MIRA 与 provider-independent residual 的 action difference 很少。这个候选检验一个明确的原因：**family-only 更新可能把 provider-specific correction 丢掉，导致共享信号不能稳定改变后续选择。**

## 2. 候选机制

对 provider $p$ 所属 family $f$，维护 family residual $r_f$ 和 selected-provider local residual $u_p$。当前 logit 为

$$z_{p,t}=h_p(x_t)+r_f^\top x_t+u_p^\top x_t+b_p.$$

对 selected-only feedback $y_t\in\{0,1\}$，对角先验协方差为 $V_f,V_p$。先令

$$v_t=x_t^\top(V_f+V_p)x_t+V_b,$$

并求一维局部 MAP $z_t^\star$：

$$z_t^\star-z_{p,t}-v_t\bigl(y_t-\sigma(z_t^\star)\bigr)=0.$$

令 $\kappa_t=\sigma(z_t^\star)(1-\sigma(z_t^\star))$，候选采用一次对角 Laplace 更新：

$$r_f\leftarrow r_f+V_f\odot x_t\bigl(y_t-\sigma(z_t^\star)\bigr),\qquad u_p\leftarrow u_p+V_p\odot x_t\bigl(y_t-\sigma(z_t^\star)\bigr),$$

$$V_f\leftarrow V_f\oslash(1+\kappa_t V_f\odot x_t^2),\qquad V_p\leftarrow V_p\oslash(1+\kappa_t V_p\odot x_t^2).$$

这里 $V_b$ 是 provider bias 的标量方差。$h_p$、菜单、安全门、probe 和延迟队列保持 v3 不变。family-only MIRA 和 provider-independent residual 是 $V_p=0$ 与 $V_f=0$ 的边界构造；候选不增加标签、调用或历史 full-panel 读取。

为保证 matched cold start，fit 得到的 family mean 保留在 $r_f$，$u_p$ 的初始 mean 为零；每个 family 的 fit variance 平分给 $V_f$ 与 $V_p$，因此初始 logit mean、总 residual variance 和 fallback score 与 v3 MIRA 相同。这个固定的 $1/2$ 分解只用于匹配初始化，不在 evaluation 结果上调节。

实现必须保持每个反馈 $O(d)$ 时间和 $O((|F|+|P|)d)$ 状态；不得重训 backbone 或重扫历史。

## 3. 最近邻与公平比较

同一 fold/seed、输入、provider menu、frozen representation、fit-only 初始化、probe seed、epsilon、hazard、fallback、delay `{0,1,4}` 和 selected-only feedback：

- `probe_mira_relay`：family-only residual；
- `probe_independent_residual`：provider-only residual；
- `probe_hierarchical_residual`：本候选；
- `probe_no_update`：不更新控制。

先跑 fold0/seed0 全 evaluation rows 作为实现/机制闸门；若合同、状态 hash 或数值测试失败，不扩展到其余 folds。若 fold0 通过，再按同一预登记协议跑 folds1–4；不挑选正折。

## 4. 主指标与停止条件

主指标是 paired selected `system_success`：候选相对 MIRA 和 Independent 的 rescued、harmed、net delta，按 fold 和 delay 分开。辅助指标：action changes、family/local update counts、runtime-valid rate、每次 update 的 wall time 和状态内存。

继续到五折的条件（实现门，不是成功承诺）：

1. fold0 三个 delay 的 raw/verifier/状态链全部通过；
2. 候选至少在一个 pre-registered comparison 上产生可解释的 action-level consequence（action change 不能只来自日志字段变化）；
3. update 仍为 $O(d)$，runtime-valid rate 不下降，且没有额外反馈或 full-panel 读取；
4. 若候选在 fold0 相对两个 matched update controls 都是 0 rescued/0 harmed，或收益只来自 single molecule，则停止该候选并回到机制/benchmark slice 诊断，不申请 seed1/A800。

五折后才计算 scaffold-cluster paired CI；三个 delay 不是独立重复。

## 5. 记录与边界

每条 decision/terminal/feedback 记录 family/local state snapshot、state hash、selected-only read set、release tick、update_ns 和 action distribution。失败、unknown、status2、参数错误和 verifier rejection 全部保留。

候选通过只支持“在冻结 BBB provider pool 上，层次 residual 更新有可复核的 online signal”。它不能自动支持自然 provider drift、hERG/AMES 迁移或 confirmation 主结果。若候选不通过，不修改 v3 的结果和目标主张。
