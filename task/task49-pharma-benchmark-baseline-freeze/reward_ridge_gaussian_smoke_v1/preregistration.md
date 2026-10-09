# Reward-ridge Gaussian baseline semantic smoke v1

日期：2026-10-01  
状态：执行前合同；只验证 baseline 的数学/事件接口，不估计效果，不打开 confirmation。

## 要解决的问题

现有 Greedy/Bayesian receipt 的 direct-logit→Gaussian 组合把 likelihood/scale 语义混在
一起。这里登记一个最小合法 Gaussian 对照：用 fit 区二元 `system_success` 拟合
reward-scale prior，再用独立的 Gaussian posterior precision 做在线更新。

这一步不声称 reward-ridge 比 MIRA 好，也不把 10 个 evaluation 分子当作效果样本。
它只回答：这个合法 Gaussian baseline 能否在同一 selected-only 生命周期、同一 feature/menu
和同一延迟合同下运行，并被独立 verifier 重放。

## 固定合同

- 输入：`fingerprint_diagnostic_v1/inputs.json`，722 molecules、33 providers；
- 表示：sealed BT+CLS hidden-64，fold 0 / seed slot 0；
- smoke cohort：fold 0 evaluation 的前 10 个分子，顺序不改；
- 方法流：`greedy_frozen` 与 `greedy_online`，delay=0；每题最多一次 selected call；
- fit prior：只看 fold 0 fit rows 的 binary `system_success`，
  $W_R=(X^\top X+\lambda_{fit}I)^{-1}X^\top Y$，`lambda_fit=1.0`；
- Gaussian state：初始 covariance $\Sigma_0=0.02I$，因此
  `lambda_prior=50.0`；observation noise variance `1.0`；
- exploration：LinUCB alpha=`0.5`；failure reward=`0`；
- feedback：只释放 selected provider 的 terminal reward，due tick=`decision_tick+1+delay`；
- 不读取未选输出、未来反馈或 confirmation；unknown 继续采用 `stop_batch`，本 smoke 输入无
  unresolved row。

`lambda_fit` 和 `lambda_prior` 是两个独立参数，不能合并成一个“regularization”。
Gaussian 预测均值允许越过 `[0,1]`，不做隐式 clipping；收据必须记录 prior 语义、协方差、
source hash、state hash 和完整 decision/terminal/feedback 生命周期。

## 独立核验

沿用但扩展 `verify_greedy_unified_raw_v1.py`：验证 fit-only reward prior、初始
`0.02I` covariance、UCB score、selected-only read set、terminal/release clock、rank-one
update、state hash、事件守恒和 source tree。旧 `lambda_prior=1.0` receipt 仍必须保持可验。

## 通过/失败条件

通过：独立 verifier 在声明 commit 的 immutable tree 上 `verified`，20 个 decision 各有
terminal/feedback，prior 和 covariance 能独立重算，且没有 outcome leakage。

失败：任何 fit/evaluation 混用、`lambda_fit` 与 `lambda_prior` 混淆、初始 covariance
不是 `0.02I`、非法提前 release、state hash 不一致或 verifier 无法重放。失败 receipt
保留，不重命名为成功。

## 后续决策

- smoke 通过：才登记同一合同的 60-stream full receipt；
- smoke 失败：修复接口或放弃 strict Gaussian 分支，保留 Matched Online-SGD 为合法 logistic
  强对照；不申请 A800。
