# Baseline eligibility matrix v2：主表资格审计

- 日期：2026-10-04
- 状态：只读审计完成；没有重跑、没有读取 confirmation、没有上 Nebula。
- 唯一问题：当前 benchmark 和 baseline 能否合法组成一张主实验表？
- 机器结果：`matrix.json`
- 生成器：`src/probe_headless/build_baseline_eligibility_matrix_v2.py`

## 结论

严格 common-direct-prior 的核心行可以组成一张候选主表，但完整 baseline 矩阵还不能合并成一张总排名表。

| 分类 | 数量 | 方法 |
|---|---:|---|
| `strict_main` | 2 | Frozen direct、Matched Online-SGD |
| `candidate` | 2 | MIRA-relative、MIRA-FamilyGuard |
| `adapted_stratum` | 4 | reward-ridge Gaussian、Greedy LinUCB adapted、BayesianRouter native-BT、PILOT adapted |
| `anchor` | 3 | Random、Best Fixed、Oracle |
| `static_control` | 2 | MLP BCE、BT+CLS static router |
| `separate_control` | 1 | Replay SGD |
| `blocked` | 2 | GraphRouter、Laya online |

机器决策为 `strict_core_eligible_full_matrix_incomplete`：strict core 行的输入、菜单、selected-only、delay、family registry、独立 verifier 和 paired-analysis 指针均存在；其他行仍有明确阻塞项。因此这张表不能支持跨 strata raw accuracy 排名，也不能把 adapted 方法当作 strict baseline。

## 合法的主表边界

主表可以先放入同一 strict common-direct-prior 语境中的 Frozen direct、Matched Online-SGD，并将 MIRA-relative/FamilyGuard 作为 candidate-vs-control 行。它们共享：

- Task49 输入 SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`；
- 33-provider 完整菜单；
- frozen BT+CLS hidden-64 representation；
- fit-only direct BCE/logit warm start；
- selected-only feedback，delay `{0,1,4}`；
- external qualification family registry；
- 已封存的独立 verifier 和 paired analysis 收据。

这只说明比较合同相容，不说明 MIRA 已经优于 control。现有 paired 区间跨 0 时，结果只能写成 development evidence。

## 必须分表的行

- `greedy_reward_ridge`：有自洽的 strict Gaussian reward-ridge receipt，但 likelihood 与 logistic stratum 不同，且事件没有 `state_bytes_p95`；只能作为 Gaussian stratum。
- Greedy LinUCB、BayesianRouter native-BT、PILOT：有完整 adapted receipt 和 verifier，但 warm start/likelihood/update semantics 与 strict common prior 不同，不能和 strict core 混排；各自还缺 state bytes 或统一 cross-stratum likelihood 合同。
- Random、Best Fixed、Oracle：任务锚点或 evaluator 上界，不作为在线学习 baseline 排名。
- MLP/BT+CLS static：full-panel fit-only 静态能力控制，没有 selected-only online receipt。

## Separate control

Replay-SGD 的历史收据已通过只读 provenance 审计，但仍然不能进入 strict 主表。审计结果见
`replay_sgd_provenance_audit_v1/report.md`：旧 prior 精确对应历史 BT checkpoint 的
`score.weight`，64 维 feature 与当前 strict snapshot 数值一致到 `3.51×10^-7`；它与当前
common-direct-prior 的最大差异为 `0.3328`，且旧事件没有 `state_bytes_before/after`。
因此 Replay 只能与自己的历史 Frozen/Online/Replay 收据做 separate paired control，不能和
当前 Frozen direct / Matched Online-SGD 组成 strict raw-accuracy 总榜。

## 当前阻塞行

| 方法 | 阻塞原因 | 处置 |
|---|---|---|
| GraphRouter | provider-description/edge semantics、fit-only warm start、selected-only update、failure/unknown/refusal、成本和 verifier 均缺 | 保留 audit-only，不用改写版本充数 |
| Laya online | online optimizer/checkpoint 回写和 selected-only receipt 未闭合，成本/分母字段缺 | 保留 audit-only；静态表示审计不能冒充动态 baseline |

## 统计和结果边界

- seed slots `{0,1}` 是确定性 protocol slots；delay `{0,1,4}` 是同一事件序列的时钟条件，不能当独立重复；主统计单位仍是 molecule/scaffold/fold paired contrast。
- 现有 common-stratum bootstrap 和 FamilyGuard paired receipts 只能作 development uncertainty；不能把 pooled accuracy 写成 superiority。
- 所有方法都必须同时报告 joint task success、valid-return conditional correctness、runtime failure、unknown/refusal 分母、selection/update p50/p95 和 state bytes；缺失项保持 `MISSING`。
- provider 是 action pool，不是 33 个独立样本；family macro 只作分层诊断，不能直接跨方法排名。

## 对三份验收标准的影响

- **Benchmark + Baseline：** Gate B 的角色边界和 strict core 主表入口更清楚；Replay 已固定为 separate control，GraphRouter/Laya 和统一成本/分母仍未闭合，Gate B 继续 `11/16 HOLD`。
- **实验矩阵与结果：** Gate C 获得逐行资格清单，但 E1--E5 全矩阵和 frozen confirmation 仍缺，Gate C 继续 `10/16 HOLD`。
- **故事与方法：** 该审计支持“信息合同和反馈边界是主问题的一部分”，不把 baseline 之间的异质性包装成方法收益；Graph Relay 的负结果已经说明 action change 不等于 utility gain。

## 下一步

1. 不重复做已有 bootstrap，也不再为 Graph Relay 增加算力；
2. 按矩阵中 `blocked` 行各自补最小合同，当前只剩 GraphRouter 与 Laya online；
3. Replay 已固定为 separate control；若未来要进入统一成本表，必须新跑带 state bytes 的收据；
4. 只有 strict core 与候选的 paired uncertainty、failure/unknown/refusal 和成本字段全部闭合后，才考虑一次冻结 confirmation。
