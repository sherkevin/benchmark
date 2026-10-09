# MIRA-Relay CPU 预登记

日期：2026-10-02
状态：候选预登记；尚未实现，尚未运行。

## 固定输入

- development common stratum：722 evaluation molecules，5 个 scaffold-disjoint folds；使用已封存的 64 维 frozen encoder、33-provider menu、provider family registry 和 prediction cache。
- 只在 fit 区拟合 `w_a^0,u_g^0,b_a^0`；evaluation truth 不进入 policy、阈值、seed 或 provider menu。
- `runtime_success` 表示工具返回了可消费的结果；`chemical_correct` 表示该结果相对冻结真值
  是否正确；单调用端到端标签为 `system_success = runtime_success * chemical_correct`。
  因此错误答案和 runtime failure 都是 `system_success=0`，但只有 runtime failure 将
  `chemical_correct` 记为 null。runtime failure 用 `system_success=0` 更新端到端 residual，
  并单独进入 hazard ledger；unknown/refusal 不更新 correctness residual；成功返回的
  unknown/refusal 仍可更新 runtime/availability 诊断账本。
- `c` 为每个 fold fit-only Best Fixed provider；其 fit score 是不可变 fallback reference。

## 固定算法常数

| 项目 | 固定值 |
|---|---|
| residual fit | full-batch L-BFGS，fit cells 上的 summed Bernoulli NLL + `0.5*lambda*(||u||^2+||b||^2)`；`max_iter=100`，`ftol=1e-12`，`gtol=1e-9`；direct head 作为冻结 logit offset |
| residual L2 | `lambda=0.01` |
| online covariance | diagonal-precision projection；每个坐标独立保存 precision/variance |
| local MAP | 80 次二分，根残差 `1e-12` |
| probe probability | `epsilon=0.20` |
| UCB coefficient | `kappa=1.0`，logit space |
| runtime upper bound | Beta posterior，`delta_h=0.05` |
| runtime slack | `eta_h=0.02` |
| probe seed | `base_seed=20261003`；每个 fold×seed 派生一次并在 delay `{0,1,4}` 间复用 |

Probe 的候选集合在决策时由 menu、provider hard block、family block 和 runtime hazard 共同确定；
不读取当前 molecule 的 truth 或未选 provider 输出。先在 eligible family 中均匀抽 family，再在
该 family 的 eligible provider 中均匀抽 provider。若 $\mathcal G_t$ 是 eligible family 集合、
$P_{g,t}$ 是 family $g$ 的 eligible providers，则 propensity 为
`epsilon / (|𝒢_t| * |P_{g,t}|)`；完整 action probability 记录为
`pi_t(a)=epsilon*q_t(a)+(1-epsilon)*I[a=a_exploit]`，safe menu 为空时 fallback 概率为 1。
同一 fold×seed 的不同 delay 共用随机数组只是 common-random-number coupling，不保证 action
相同；delay 的主比较按 policy condition 解释。必须按 family 和 provider 同时报告 probe 分母，
不能把 provider-uniform 结果解释为 family-neutral exploration。

## 预登记矩阵

每个 fold、seed、delay 跑五种动态策略：`NoProbeShield`、`ProbeNoUpdate`、
`ProbeIndependentResidual`、`ProbeMIRA-Relay`、
已停止的 `ContextProbe` 负对照；另外保留 Frozen 和 fit-only Best Fixed。所有方法共享 molecule
顺序、candidate menu、provider registry、probe PRNG 和 feedback clock；只改变是否 probe、是否
更新 residual 和如何使用 release 后 state。`ProbeMIRA-Relay` 只更新 selected challenger；
fallback selected feedback 不更新 residual mean/variance 或 provider bias，但可更新 runtime/availability 与诊断正确计数，并记录 `residual_update=false`。

## 主要 estimand

1. **机制效应**：同一 probe schedule 下，`ProbeMIRA-Relay − ProbeNoUpdate` 的 release 后
   exploitation action change 数和 action change 后的 paired success；实现门不等于收益门。
2. **共享结构效应**：`ProbeMIRA-Relay − ProbeIndependentResidual` 的 paired system-success、
   harmed/rescued 和 family-level action change。
3. **探索代价**：`ProbeNoUpdate − NoProbeShield` 的 probe direct success、harmed、rescued、
   runtime failure 和 selection/update p50/p95。
4. **总体任务值**：每种策略的 task success、valid return、runtime failure、unknown/refusal、
   switch-away；相对 fit-only fallback 逐分子记录 both-correct、both-wrong、harmed、rescued。

所有区间按 molecule 聚类；scaffold bootstrap 作为敏感性分析。delay 只作为同 schedule 的反馈
clock 处理，不把不同 PRNG assignment 的差异解释成纯 delay effect。每个分支还必须拆出
`runtime_success rate`、`chemical accuracy | valid_return`、端到端 `system_success`，避免把
工具执行失败和化学判断错误混在一个数字里。科学继续门固定为：`ProbeMIRA-Relay` 相对
`ProbeNoUpdate` 的 paired system-success 点差至少 `+0.5 pp` 且 molecule-cluster 95% CI
下界大于 `0`；相对 `ProbeIndependentResidual` 也至少 `+0.5 pp` 且 CI 下界大于 `0`。若
任一门不满足，保留结果但不进入 A800/confirmation，不用调 probe、UCB 或 family registry
追分。runtime failure 点差不得恶化超过 `+0.2 pp`，否则同样停止。

## 停止门

- 在 real-cache 之前，state→action sensitivity 单测必须通过：同一 fit-derived feature 上分别
  release 合法的 `y=0` 与 `y=1`，独立重算后至少一个 challenger 的 score 或 ranking 改变；否则候选
  不进入 full replay。
- fit-only 145 行静态诊断中，非 fallback exploitation action 可行率为 0；
- `ProbeMIRA-Relay` 的 release 后 shield action change 为 0，或与 `ProbeNoUpdate` 没有可复核差异；
- `ProbeMIRA-Relay` 未达到上面固定的 `+0.5 pp`/cluster-CI science gate，或 shared 与
  independent residual 无正差异；
- 作用只来自 probe 直接命中，shared family block 做 provider/family permutation 后效果不变；
- harmed 不低于 `ProbeNoUpdate` 且 rescued 明显下降；
- 独立 verifier 无法重算 fit residual、family block、online diagonal state、propensity、hazard、
  feedback delay 或 decision payload。

任何停止门触发，都保留 receipt，不调 `epsilon`、`kappa`、`eta_h`、seed 或 fit optimizer；不提交
Nebula A800，不读取 confirmation。
