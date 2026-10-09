# Matched effect matrix v3：五折 CPU development 设计审查

**审查范围：** 只读检查 v2 receipt、五个 trust-exact candidate 收据和当前
Benchmark + Baseline v005 验收标准。本文不改代码、不重跑实验，也不把 v2
结果写成最终主结果。

## 1. 先给结论

这批实验值得跑，但它的作用是回答“v2 的小信号是否跨 scaffold fold
存在”，不是证明 MIRA-Relay 已经优于在线 baseline。

v2 在 fold 0、145 个 evaluation 分子上得到：

| delay | MIRA | Independent | NoUpdate | MIRA vs Independent | MIRA vs NoUpdate |
|---:|---:|---:|---:|---:|---:|
| 0 | 127/145 | 126/145 | 126/145 | 1 rescued / 0 harmed | 2 / 1 |
| 1 | 127/145 | 126/145 | 126/145 | 1 / 0 | 2 / 1 |
| 4 | 126/145 | 126/145 | 126/145 | 0 / 0 | 2 / 2 |

因此 +1/145 只说明在一个 fold、一个 seed 上有一个净正确样本，不能排除
分子组成、到达顺序、provider family 聚集或偶然切换。它也没有形成 delay
越大越差或越稳的清晰关系。五折是最小的合理下一步：它能把同一合同放到
722 个 development 分子、五个 scaffold-disjoint evaluation fold 上，保留
所有折，不挑正折。

建议的矩阵是 5 folds × seed 0 × 3 methods × delays `{0,1,4}`，共 45 条
stream、6,498 个 decision、每条 stream 432/435 个 event、19,494 个 raw events。当前
v2 的 3,915 events 的 `events.jsonl` 实际约 439 MB（每条约 112 KB）；按同一
完整 state snapshot 格式估计，v3 原始 JSONL 约 2.2 GB，CPU 运行时间约为
数分钟到十几分钟。应在开跑前确认磁盘
空间，并保持一条任务、一个 receipt，不能用压缩后的摘要替代 raw JSONL。

## 2. fit candidate 与 status2 边界

已有五个 fold/seed0 trust-exact candidate 都通过独立数值复核：fold 0、1、3、4
是 SciPy status 0；fold 2 原始 status 是 2、`scipy_success=false`，但有限、
梯度无穷范数 $2.29\times10^{-7}$、Hessian 对称误差 $2.27\times10^{-13}$，
满足预先写明的数值停点门。收据保留在
`experiments/logs/task49_mira_relay_fit_diagnostic_20261003/trust_candidate_crossfold_v1/`，
详见 [trust-exact 跨折报告](trust_exact_candidate_crossfold_report.md)。

这不等于五个 fit-state 已经可以直接进入 replay：当前生效的 fit-state v2
默认仍是 status0-only，fold2 的 status2 不能静默序列化成 `accepted`。在
v3 开跑前必须有一个显式的 status2-aware fit-state contract，至少保存：

1. `candidate_status=numerically_stationary_status2`、`scipy_status=2` 和
   `scipy_success=false`；
2. `status2_allowed=true` 以及 gradient/Hessian/finite 数值门；
3. fold、fit entity、provider/family 顺序、direct-prior、checkpoint、参数
   和 state hash；
4. verifier 能分别检查 status0 与 status2，而不是把两者统一成 success。

fold2 必须进入主矩阵；同时报告一份“排除 fold2 的数值敏感性”作为审计，
但不能看完结果后决定是否留下 fold2。若 status2 contract 尚未封装，v3 的
阻塞原因是 fit-state 合同未闭合，而不是把 fold2 换成正向 fold。

## 3. 这批实验能给验收标准增加什么

当前 Benchmark + Baseline v005 的 Gate C 通过线是 `≥12/16`，且 C1--C4
每项至少 3 分、E1--E5 不缺。五折 v3 的贡献如下：

| 验收项 | v3 能补的证据 | 仍不能解决的缺口 |
|---|---|---|
| **C1 矩阵覆盖** | 同一 selected-only/delay 合同覆盖五个 scaffold-disjoint evaluation fold；比单 fold 更接近 E2/E3 的完整开发矩阵 | 仍只有 seed0、一个 BBB development endpoint；不是 E6 confirmation，也没有全主 baseline |
| **C2 baseline 闭合** | MIRA、provider-independent residual、NoUpdate 三个 matched kernel 在同一菜单、输入、延迟和初始化下闭合 | 不能替代 Greedy LinUCB、PILOT、BayesianRouter、Online/Replay SGD 及静态强 router 的统一 raw run |
| **C3 结果合理性** | 逐 fold paired rescued/harmed、family-transfer、action change 和 delay 分解，能检验 +1 是否由单折驱动 | 一个 seed 的小差异仍不能称稳定提升；delay4 的 null 结果必须保留，不能只报 delay0/1 |
| **C4 paired uncertainty / recovery** | 722 个分子、五 fold 和完整 raw event 允许做 fold/scaffold-cluster paired CI；独立 verifier 能查状态和 summary 绑定 | seed0 不是随机性充分性；若不先登记 CI 和 fold 聚合规则，不能事后挑 pooled mean |
| **H5/H6/H9/H10** | 继续强化选择前信息边界、同权限比较、主指标/分母冻结和逐事件恢复链 | H1--H4（公共 truth、标签冲突、provider lineage、confirmation overlap）不会因为 v3 自动通过 |

所以 v3 是 Gate C 的必要开发证据，但不是 Gate C 通过本身，也不打开
confirmation。跑完应更新评分证据，不应把 `41/60` 或 `Gate C 10/16 HOLD`
自动改成通过。

## 4. 原始 event 必须保留什么

所有字段都要有“policy-visible”或“evaluator-only”标记。evaluator-only
字段可以进入审计日志，但不能在 decision 前提供给策略。

### 4.1 decision

必须保留：

- `fold/seed/delay/stream`、`event_id`、`entity_id/query_id`、`tick`、
  `version` 和 feature/checkpoint hash；
- 完整 `candidate_menu`、`decision_reads`、`released_feedback_ids`（包括
  同一 tick 的释放顺序）；
- `provider_id`、`selected_provider`、`gate_action`、`is_probe`、
  `probe_family`、`probe_propensity`、`propensity`、`action_distribution`；
- `scores`、`logit_means`、`logit_variances`、`available`、
  `feasible_challengers`、`eligible_families/providers`、`hazard`、
  `family_blocked`、`fallback_provider`、`fallback_logit_immutable`；
- `state_hash_before/after` 和状态快照，或能从独立 state archive 精确恢复
  的等价引用；`selection_ns`。

这些字段可以区分：是 probe 造成的动作变化，还是 exploit 分数变化；是
菜单/availability 变化，还是 family residual 变化；是旧反馈是否已经释放，
还是纯粹的分子顺序差异。

### 4.2 terminal

必须保留：

- `event_id`、`entity_id`、selected `provider_id`、`tick`、`due_tick`；
- 原始 `selected_output.status/label`、`valid_return`、`system_success`、
  `chemical_correct`、`recovered_label`，并明确它们是 evaluator-only；
- `selected_output` 的 provider、版本、失败原因和输出 hash；
- terminal 前后的 state hash（terminal 不应提前改变 learner state）。

terminal 记录不能在 policy 的 decision 前读取；它只是为了之后核对
provider/reward 绑定、失败分母和错误归因。

### 4.3 feedback/release

必须保留：

- `event_id`、`provider_id`、`decision_tick`、`decision_version`、
  `due_tick`、`release_tick`、同 tick 的 `release_ordinal`；
- policy-visible `feedback_status`、`reward`（scored correctness）、
  `updated`、`residual_updated`、`runtime_failure`；
- `version_before/after`、`update_ns`、`feature_sha256`；
- `state_hash_before/after` 和完整更新前后 state，或可独立恢复的 state
  archive；
- pending queue 中本次释放的 event ids，便于验证 delay 没有被缩短。

反馈日志需要把 runtime success 和 chemical correctness 分开。failure 的
`chemical_correct` 必须为 null；unknown/refusal 不能被改成 0。当前 BBB
v3 可能没有 unknown，但字段和语义仍须保持，以免把开发结果误当成完整
动态合同。

### 4.4 stream/run manifest

必须保留 input/row/split/scaffold hash、fold entity order、provider/family
registry hash、candidate/state/checkpoint hash、源码 commit、Python/NumPy、
线程环境、所有超参、probe seed、时间、硬件和失败收据。summary 只能作为
索引；主表每个数字都必须能由 raw JSONL + 独立 verifier 重算。

### 4.5 v2 receipt 已知字段缺口

v2 已经保存了完整 state snapshot、decision read-set、delay tick 和 hash
chain，但当前样例的 `decision` 没有显式 `released_feedback_ids`，`feedback`
也没有 `release_ordinal` 或 pending queue 快照。v3 不能只依赖 JSONL 行顺序
来推断这些关系：同一 release tick 可能有多个 pending event，误序会直接影响
延迟归因。开跑前应在统一 schema 中补这三个字段；若暂时不补，必须在
preregistration 里把“按文件顺序恢复 queue”写成已知限制，并不得声称已经
完成完整 delay replay audit。

## 5. 预先冻结的分析与分支

不建议用“总共多了一条正确”作为继续标准。开跑前应在 preregistration
写清以下输出：

1. 每 fold、每 delay、每 baseline 的 `n_correct`、accuracy、rescued、
   harmed、net paired delta；
2. fold-level paired delta 的均值和 95% CI，以及按 scaffold group 聚类的
   paired CI；不能把 722 个 episode 当作 722 个独立 seed；
3. action-change → reward-change 的对应关系，probe/exploit 分层，family
   transfer 与 provider-local update 分层；
4. delay 0/1/4 分开报告，不因为 delay4 为 null 而删掉它；
5. full-panel Best Fixed/Oracle、dynamic stream 和 controlled event 分表。

建议的继续/诊断分支（必须在看到 v3 数字前写入 prereg）：

- **继续 seed1：** 所有五折合同、status2、summary 和 recovery 均通过，且
  MIRA 相对同一 matched baseline 的正向方向不是由单一 fold 贡献；在开跑前
  预先声明的主延迟（建议先取 `delay=0` 和 `delay=1`）上，至少 4/5 fold
  对两个 matched baseline 都满足 `net paired delta >= 0`，同时没有一个
  fold 出现 `harmed > rescued` 的相反主导。`delay=4` 作为压力结果单独报告，不得
  因为它为 null 而从表中删除。此分支只说明值得测随机性，不能称方法成立。
- **进入机制诊断：** paired 符号在 folds 间混杂、正向主要来自一个 fold、
  action changes 很多但 reward changes 很少、或 family transfer 没有转化为
  sibling action。优先按 scaffold/family、probe/exploit、release lag 和
  availability 分解；不换 cohort、不调阈值补正结果。
- **停止当前算法扩展：** verifier/fairness/status2 任一硬门失败，或五折
  大多数 delay 都是 net negative 且不是单个 provider failure 造成。保留全量
  receipt，先修改机制或重新审查 claim；不提交 A800，也不打开 confirmation。

delay4 若稳定为 null，应把 claim 限定为“在较短延迟下的开发信号”，并诊断
队列滞后、更新衰减和 family transfer 的有效窗口；不能写成一般的 delayed
outcome robustness。

## 6. 与下一步的关系

v3 之后再扩 seed1 是合理顺序。seed1 不能先于五折 seed0，因为 seed0 才能
先回答是否存在 fold-specific effect；seed1 也必须复用同一五折 entity order、
fit-state policy、候选菜单和事件 schema。只有五折/多 seed 的 paired CI 与
现代主 baseline 完成后，才有理由讨论 Nebula 或 confirmation。

本设计不改变 benchmark、baseline 或创新点决议，也不把 fold2 status2
改名为成功；它只把 v2 的小信号推进到可审查的跨折诊断。
