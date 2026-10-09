# ProbeIndependentResidual：匹配对照设计 v1

日期：2026-10-03
状态：**已实现 CPU policy kernel，并完成独立 contract-only 回放；尚未跑效果矩阵，未产生效果结论。**

## 1. 目的和可证伪 claim

这不是第二个主方法。它是用来回答一个具体问题的 matched comparator：MIRA-Relay 的收益是否来自
family residual 的反馈共享，而不是仅仅来自 probe、延迟队列、hazard shield 或某个 provider 自己
的更新。

预登记的结构 claim 是：在完全相同的输入、fit 冷启动、probe schedule 和反馈时钟下，
`ProbeMIRA-Relay` 相对 `ProbeIndependentResidual` 的 paired system-success、harmed/rescued
和同族未选 provider 的 action change 能够识别 family sharing 的增量作用。

这个 claim 的可证伪条件是固定的：

1. 两个方法的 fit-only 初始 logits 不逐元素相等；
2. independent 方法的一次 selected challenger release 改变了未选同族 provider 的 residual 或分数；
3. unknown/refusal 或未选 provider 的 feedback 改变了 independent residual；
4. independent runner 的 decision/terminal/feedback/release 数量、due tick、state hash 或 selected-only
   约束不能被独立重算；
5. 正式 paired 结果中 MIRA-Relay 相对 independent 没有预登记的正 family-sharing 差异。

任一第 1--4 条都表示实现合同失败；第 5 条表示保留结果但不能宣称 family sharing 带来增益。

## 2. 与主方法的最小差异

每个 provider `a` 的冻结 direct head、fit provider bias、菜单、probe 分布、hazard ledger、family
block、fallback shield、事件绑定和 delayed release 都保持不变。只替换 residual 参数的索引：

- MIRA-Relay：每个 family 一个 `u_g∈R^d`，同族 provider 共享反馈更新；
- independent：每个 provider 一个 `u_a∈R^d`，每次只更新已选 provider。

family registry 在 independent 中仍用于相同的 probe 分层和 family-level runtime shield；这不表示
residual 在 family 间传递。`family_sharing=false` 仅指 correctness residual 和 residual uncertainty
没有 family transfer。

## 3. 严格匹配的冷启动

设 fit-only family state 为 `u_g^0`、variance 为 `s_g^0`。构造 independent state 时，对 provider
`a` 复制其 family 的 fit state：

$u_a^0=u_{g(a)}^0,\qquad s_a^0=s_{g(a)}^0.$

provider bias `b_a^0`、direct head `w_a^0` 和 fallback reference 都直接读取同一 sealed fit-state。
所以两种方法在第一个 decision 前的 provider logits 和 UCB scores 必须逐元素一致；差异只能从
release 之后出现。

对应的 provider score 是：

$z_a(x)= (w_a^0)^\top\phi(x)+u_a^\top\phi(x)+b_a.$

fallback 的 logit 仍固定为 fit-only reference；fallback 的 runtime ledger 可以变化，但不会被
residual update 改写。

## 4. 在线更新

对 selected challenger 的 delayed feedback，independent 使用与 MIRA-Relay 相同的 scalar local
MAP 和 diagonal-precision projection。若当前 selected provider 是 `a`，特征为 `x_t`，标签为
`y_t∈{0,1}`，只更新：

$z_0=(w_a^0)^\top x_t+u_{a,t}^\top x_t+b_{a,t},\qquad
v_t=\sum_j s_{a,t,j}x_{t,j}^2+s_{b,a,t}.$

$z^*=z_0+v_t(y_t-\sigma(z^*)),\qquad h^*=\sigma(z^*)(1-\sigma(z^*)).$

$u_{a,t+1,j}=u_{a,t,j}+s_{a,t,j}x_{t,j}(y_t-\sigma(z^*)),$

$s_{a,t+1,j}^{-1}=s_{a,t,j}^{-1}+h^*x_{t,j}^2.$

provider bias 按主方法同样更新；其他 provider 的 `u` 和 residual variance 保持 bitwise 不变。
`system_success=0` 的 runtime failure 仍进入 correctness residual 和独立 hazard ledger；successful
unknown/refusal 只更新 runtime ledger，不更新 correctness residual；这些规则与主方法完全一致。

## 5. 输入和输出 schema

### 输入

- `providers[K]`：固定 provider 顺序；
- `direct_prior[K,d]`：sealed fold/seed 的 direct head；
- `family_by_provider[K]`：provider registry，仅用于匹配的 menu/probe/shield；
- `fit_family_residual[G,d]`、`fit_family_variance[G,d]`、`fit_provider_bias[K]`、bias variance：
  只来自 fit 区；
- `fit_valid_counts[K]`、`fit_failure_counts[K]`：只用于 hazard reference；
- molecule feature `phi(x)∈R^d`、候选菜单和真实 terminal feedback；evaluation truth 不在 decision
  时读取；
- `delay∈{0,1,4}` 和同一 terminal queue contract。

runner 将 family fit state 复制为 `provider_residual_mean[K,d]` 和
`provider_residual_variance[K,d]` 后再开始 stream。没有额外的 evaluation warm start。

### 状态

每个 event 的 `state_before/state_after` 至少包含 inherited runtime/feedback ledger，以及：

- `provider_residual_mean[K,d]`；
- `provider_residual_variance[K,d]`；
- `provider_bias_mean[K]`、`provider_bias_variance[K]`；
- `residual_schema="provider_independent_residual_v1"`；
- `version`、runtime success/failure、blocked/family block 状态。

为了让现有 lifecycle 工具可以共用 state hash，snapshot 还保留兼容性的
`family_mean/family_variance` 字段；它们在该 policy 中是零/一占位，永远不参与 score 或 update。
审查和分析必须使用 provider residual 字段，不能把兼容字段解释成共享参数。

### 事件

`decision`、`terminal`、`feedback`、`release` 的字段和 MIRA-Relay runner 一致：`event_id`、
`query_id`、`selected_provider`、`candidate_menu`、`available`、`propensity`、`due_tick`、
`state_hash_before/after`、`state_before/after`。decision 额外写：

- `residual_schema`；
- `provider_residual_scores[K]` 和 `provider_residual_variances[K]`；
- `family_sharing=false`；
- `decision_reads` 只允许 molecule feature、candidate menu、已 release selected feedback。

`effectiveness_estimate` 在 contract receipt 中固定为 `null`，mechanism receipt 不能被引用为收益
结果。

## 6. 文件与验证范围

- policy：`src/probe_headless/mira_independent_residual.py`；
- tests：`src/probe_headless/tests/test_mira_independent_residual.py`；
- runner：`src/probe_headless/run_mira_independent_residual_contract_replay_v1.py`；
- contract receipt：`experiments/logs/task49_mira_independent_residual_contract_20261003/v2/`；v1 保留为
  provenance 错误的 superseded receipt；独立 verifier
  `verification_v2.json` 通过 3 delays × 32 queries，共 288 events。它逐条检查 selected-only
  时钟、provider/reward 绑定、state hash chain，以及未选 provider residual 不发生变化。
- 本设计文档：当前文件。

CPU 单测覆盖：冷启动 logits 匹配、selected-only provider 更新、未选同族不变、fallback 不变、
unknown/refusal 不更新 residual、无 probe 控制和非法 shape fail-closed。contract receipt 中每个 delay
有 22 次 provider-local residual update、10 次不更新；delay 只是同一 32-query 序列的生命周期复制。
这只是实现和机制边界证据，完整 independent/MIRA/NoUpdate matched effect matrix 仍未运行。
