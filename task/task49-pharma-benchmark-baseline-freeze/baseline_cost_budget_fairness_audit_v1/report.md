# Baseline cost / budget fairness audit v1

日期：2026-10-01  
状态：只读审计；没有重跑、调参或修改任何 receipt。

## 目的

这份审计把现有 baseline receipt 的输入权限、表示、warm-start、likelihood、更新预算、调用/失败分母和策略开销放到同一张清单中。它区分“同一统计模型层内可比”和“只可单独报告”的结果，不把同一个 menu 当成公平的充分条件。

机器结果：[`audit.json`](audit.json)。来源包括 `common-stratum FamilyGuard full_v1`、`reward-ridge Gaussian full_v1`、Random、PILOT、native BayesianRouter、旧 Greedy 和 Matched/Replay receipt。所有延迟均为 `{0,1,4}`，输入 hash 为 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`，menu 为 33 个 provider；p95 单位为微秒，state 为字节。

## Receipt-level ledger

`calls` 是每个方法的 selected provider call 总数；`failure` 是 provider runtime failure，保留在 attempts 分母；`unknown/refused` 是 receipt 中明确记账的事件数。当前 fixed-pool receipts 的 unknown/refused 为 0，不表示已经测试了拒答能力。`MISSING` 表示原始 receipt 没有该字段，不能从代码内存布局推断。

| method | role / stratum | feature | warm-start | likelihood / score | update budget | calls | failure | unknown | refused | selection p95 | update p95 | state p95 |
|---|---|---|---|---|---|---:|---:|---:|---:|---:|---:|---:|
| Frozen direct | strict main / binary-logit | BT+CLS hidden-64 | sealed direct BCE/logit head | fixed logit score | 0 parameter updates | 4,332 | 33 | 0 | 0 | 36.435 | 2.000 | 16,896 |
| Matched Online-SGD | strict main / binary-logit | BT+CLS hidden-64 | same direct head | Bernoulli logistic/BCE | 1 selected-feedback update, lr=.05 | 4,332 | 35 | 0 | 0 | 35.333 | 13.209 | 16,896 |
| MIRA-relative | candidate / reliability | BT+CLS hidden-64 | same direct head | sigmoid score + Beta reliability | 1 ledger update; delta=.05, n_min=20, tau=.20, streak=2 | 4,332 | 33 | 0 | 0 | 85.018 | 47.750 | 17,721 |
| MIRA-FamilyGuard | candidate / family ledger | BT+CLS hidden-64 | same direct head | provider/family Beta and gate | 1 provider+family update; streak=2, cooldown=4, prior 1/9 | 4,332 | 10 | 0 | 0 | 118.685 | 91.079 | 17,945 |
| Greedy reward-ridge | strict Gaussian | BT+CLS hidden-64 | fit-only reward ridge, lambda_fit=1 | Gaussian reward, noise=1, precision=50 | 1 rank-one update, UCB alpha=.5 | 4,332 | 28 | 0 | 0 | 161.083 | 26.583 | MISSING |
| Greedy adapted direct-logit | adapted separate | BT+CLS hidden-64 | old fit-only ridge prior | Gaussian surrogate with adapted prior | 1 rank-one update, alpha=.5 | 4,332 | 23 | 0 | 0 | 168.034 | 29.833 | MISSING |
| BayesianRouter native-BT | adapted separate | BT+CLS hidden-64 | recovered BT score weight | Gaussian TS, variance=.02, noise=1 | 1 rank-one update + Thompson sample | 4,332 | 25 | 0 | 0 | 243.204 | 78.579 | MISSING |
| PILOT-adapted | adapted separate | projected 64D | projection/provider vectors, 100 fit epochs | Gaussian surrogate + cosine/UCB | 1 rank-one update, alpha=.5 | 4,332 | 30 | 0 | 0 | 176.685 | 27.560 | MISSING |
| Random | task anchor | hidden-64 field, not used for score | none | uniform menu | 0 model updates | 4,332 | 36 | 0 | 0 | 80.227 | 1.393 | MISSING |
| Replay-SGD | matched control, separate receipt | 64D field not explicit in receipt | direct logistic warm-start, field needs recheck | Bernoulli logistic/BCE | selected update + replay capacity 64, batch 8, lr=.05 | 4,332 | 11 | 0 | 0 | 50.714 | 149.029 | MISSING |
| Best Fixed | static task anchor | MISSING online receipt | fit-only full-panel provider | N/A | fixed action | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING |
| Oracle | evaluator upper bound | full panel + truth | evaluator-side | N/A | privileged, non-deployable | N/A | N/A | N/A | N/A | N/A | N/A | N/A |
| MLP/BT+CLS static router | static separate | Morgan/full-panel diagnostic | fit-only, frozen evaluation | BCE or BT+CLS pair objective | no online update | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING |
| GraphRouter | audit-only | provider graph input not frozen | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING |
| Laya online | audit-only | static Laya representation only | no selected-only optimizer receipt | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING | MISSING |

## Fairness interpretation

### 可作为同层主比较的部分

- `Frozen direct` 与 `Matched Online-SGD` 共享 query、direct warm-start、selected-only feedback、延迟和调用预算，构成当前最干净的 binary-logit control stratum。
- `Greedy reward-ridge` 是语义自洽的 Gaussian stratum，但不能与 binary-logit stratum 的 raw accuracy 直接混排。它补齐了 Gaussian baseline 的合法 prior 和 full receipt，不代表 MIRA superiority。
- MIRA-relative / FamilyGuard 使用 binary direct score 加可靠性状态，应与 Frozen/Matched 做 paired mechanism comparison；它们是候选方法，不是外部 baseline。

### 只能单独报告的部分

- 旧 Greedy、native BayesianRouter 和 PILOT 的 input/menu/delay 与主协议一致，但 warm-start、likelihood 或 projected representation 不同；只能报告各自 online−frozen，不能组成一条跨方法总榜。
- Replay-SGD 有独立 selected-only receipt，但 recovered config 没有把 query/feature 与 strict common manifest 逐字段绑定；在补齐前保持 separate receipt。
- Best Fixed、Oracle 和 static router 没有 online cost receipt。Best Fixed 是静态 anchor，Oracle 是 evaluator 上界，不能排在部署策略中。
- GraphRouter 的 provider description/graph semantics 尚未冻结；Laya 没有 selected-only online optimizer 和 checkpoint 回写合同。二者维持 audit-only。

## 缺口与 Gate 影响

这份清单把“有哪些字段缺失”固定下来，但不改变当前评分：

- **Gate B 仍为 11/16 HOLD。** 角色分层和 reward-ridge 合法 Gaussian receipt 补强了 baseline fidelity，但 adapted baseline 仍没有统一 likelihood/先验，Replay 的 strict-contract 字段未核齐，且部分 receipt 没有 state bytes 或完整 refusal 字段。不能通过排除异质方法来虚增公平分。
- **Gate C 仍为 10/16 HOLD。** E5 的成本和 failure 记账在 common-stratum 已有，但所有主 baseline 还没有一张统一的成本/预算/unknown/refusal 表；现有 refusal=0 是覆盖缺失，不是拒答效果。
- **不允许的结论：** 不能把 p95 差异写成 deployment cost superiority；这些是不同 Python/cache runner 的策略计算时间，没有统一硬件/服务端 token 账本。

## 最小后续动作（不新增实验）

1. 将 [`audit.json`](audit.json) 作为 baseline roster 的唯一成本字段来源；所有 `MISSING` 保持显式，不从代码推算。
2. 给下一版 raw-run schema 增加 `state_bytes_before/after`、`refusal_action`、`unknown_reason`、`feature_sha256`、`warm_start_sha256`、`likelihood_id` 和 `budget_id`；现有 receipt 不回填伪值。
3. 对 Replay-SGD 补齐 strict manifest 的 feature/warm-start/update-budget 字段；完成前只作 separate matched control。
4. 主表按 stratum 分组：binary-logit、strict Gaussian、adapted modern、static/audit-only；不要用 raw accuracy 或 p95 跨组排序。

本审计没有启动新实验，也没有改变任何已有 receipt 或 confirmation 状态。

## 2026-10-01 后续指针校正

本报告完成后，项目又生成了统一机器表
`task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_v1/table.json`
（`baseline_cost_budget_contract_v2`）。因此上文“还没有一张统一成本/预算表”只描述本审计
当时的状态；当前成本字段的唯一机器入口已经改为该表。原报告中的缺口判断仍然有效：
`MISSING` 不补伪值，adapted receipt 不能跨 stratum 排名，Replay 的 strict manifest、
state bytes、refusal/unknown 覆盖和 confirmation 仍未闭合。

native-BT Bayesian 行的主 `evidence` 与 direct-prior 修复收据已分开；后者只在机器表的
`audit_evidence` 字段作为 raw-contract 审计旁证。该指针校正不改变 Gate B=`11/16 HOLD`
或 Gate C=`10/16 HOLD`。

## 2026-10-02 Replay scope 与 family registry 修正

独立复核发现 Replay-SGD 的旧 p95 是把 90 条 source streams（Frozen、Online、Replay）
整体 pooled 后写入 Replay 行，和该行的 4,332 decisions/30 Replay streams 不一致。生成器
现按 `stream_suffix=replay_sgd_matched` 过滤，Replay-only selection/update p95 为
`50,713.9/149,029.45 ns`；机器表的 `receipt_scope` 明确写出 `stream_count=30`、
`source_stream_count=90`。旧 pooled 数字只保留在历史现场，当前表不再使用。

同时，FamilyGuard common-stratum 的旧 metrics 使用了与 config 不同的 registry。已使用
声明的 external qualification registry 在同一 events 上只读重聚合，结果和 hash 见
`metrics_v2_external_registry.json` 与 `metrics_v2_external_registry_audit.json`。两项修正
都不重跑策略或读取 confirmation；Gate B/C 仍为 `11/16`、`10/16 HOLD`。
