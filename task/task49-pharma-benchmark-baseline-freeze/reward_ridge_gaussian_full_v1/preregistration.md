# Reward-ridge Gaussian baseline full development receipt v1

日期：2026-10-01  
状态：执行前合同；开发阶段 baseline 资格，不是 confirmation。

## 研究问题

在固定 BBB development provider menu、同一 hidden-64 query 和 selected-only delayed
feedback 下，一个语义自洽的 Gaussian reward baseline 能否被完整回放和公平记账？

主结果只报告该 baseline 相对自身 Frozen control 的 paired online−frozen，另外报告
failure、valid-return、family macro、selection/update latency 和 state bytes。它不承担
MIRA superiority claim，也不把 adapted direct-logit Gaussian receipt 与本实验混排。

## 固定输入和权限

- `fingerprint_diagnostic_v1/inputs.json`，SHA=`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`；
- 5 scaffold folds × 2 deterministic seed slots × delays `{0,1,4}`；
- 33-provider menu，722 development molecules，每题最多一次 selected call；
- hidden-64 BT+CLS encoder，fit-only reward prior；evaluation 不读完整 panel；
- feedback due tick=`decision_tick+1+delay`，failure 保留在分母，unknown 按 `stop_batch`；
- confirmation closed，所有方法和阈值不从 evaluation 结果调节。

## 方法合同

对每个 fold/seed：

$$W_R=(X_{fit}^{\top}X_{fit}+\lambda_{fit}I)^{-1}X_{fit}^{\top}Y_{fit},\qquad
\lambda_{fit}=1.0,$$

其中 $Y_{fit}$ 是 selected provider 的 binary `system_success` panel。在线 Gaussian
state 使用 $\Sigma_0=0.02I$（precision `50.0`）、observation noise variance `1.0`、
LinUCB `alpha=0.5`。`greedy_frozen` 消费 feedback 但不更新；`greedy_online` 每条释放
feedback 做一次 rank-one update。Gaussian score 不隐式 clip。

## 必须输出

- 原始 decision/terminal/feedback JSONL；
- config、fit prior/snapshot、input/checkpoint/source hash；
- `greedy_frozen` 与 `greedy_online` 60 streams、8,664 decisions；
- 独立 NumPy verifier，逐事件重放 prior、covariance、score、queue、state hash；
- paired molecule/scaffold 描述性分析、failure/unknown/refusal 分母和成本表；
- 任何失败 attempt 都保留，不覆盖 smoke 或旧 Greedy receipt。

## 解释与否定条件

- verifier 失败、信息泄漏、prior/covariance 参数混淆或事件不守恒：baseline receipt 不合格；
- verifier 通过但 online−frozen 区间跨 0或为负：保留为合法负/不确定 baseline 结果，不调参
  追分；
- 该 receipt 不用于证明 MIRA 已提升，也不打开 confirmation；它只决定 strict Gaussian
  baseline 是否具备可审计的正式开发证据。
