# E5-Semantic-Closure v1：unknown / failure / refusal 语义 smoke

日期：2026-10-01  
状态：执行前登记；只做 CPU semantic smoke，不产生效果排名，不读取 confirmation。

## 1. 要回答的问题

这轮只验证事件协议是否能把四种状态分开，并在 delayed selected-only 更新下保持可恢复：

1. scored return 能产生一次 chemical correctness 更新；
2. provider runtime failure 保留在 `N_attempt`，更新 MIRA/FamilyGuard 的 runtime availability/failure-streak，但不携带 `chemical_correct`；
3. unknown truth 不产生 numeric reward，不更新 learner/version、chemical ledger 或 failure streak；
4. post-call verifier 的主动 `refuse` 不被当成 provider failure 或错误答案。

不能由本轮证明 selector superiority、dynamic verifier efficacy、自然 drift、unknown 泛化或 confirmation。

## 2. 真实 fixture

使用仓库当前真实 `PredictionCache`、`TruthStore` 和 33-provider menu，不新增模型/API：

| event | molecule | selected provider | 事实 |
|---|---|---|---|
| scored | `z000282` | `b3clf_dtree_common` | cached status=`ok`，公共 BBB truth 可评分 |
| failure | `z001303` | `b3clf_dtree_common` | cached status=`error`，error code=`CACHED_VALUE_MISSING` |
| unknown | `z002970` | `b3clf_dtree_common` | cached status=`ok`，但 truth ledger 标为 unresolved |
| refusal | `z000282` | `admet_ai_1_4_0` | cached probability=`0.3752889407798648`，post-call confidence `2|p-0.5|` 低于固定阈值 `0.8` |

`z002970` 不属于 722 条 scored development input；它只作为独立 semantic fixture，不能回写 common-stratum manifest 或效果分母。unknown 的 fixture 坐标在运行前固定，不能由输出、truth 或策略动作决定。

## 3. 时间与动作合同

- `scored → failure → refusal` 是一条 stream；`unknown` 是单独的 censor stream，因为当前生效的 `stop_batch` 语义要求 unknown terminal 后立即结束该 stream。每条 stream 使用 `delay=0`，另用 `delay=1` 对 scored/failure stream 做反馈时钟检查。
- selector 使用 MIRA-FamilyGuard 的冷启动策略和一个固定的 64 维非泄漏 query；fixture 只检查 release/update 语义，不评价动作选择。
- unknown 采用当前生效的 `stop_batch` 语义：terminal 记录 `status=unknown` 后立即结束该 semantic stream，不进入后续 decisions、不排队 numeric feedback；其 terminal 必须保留 censor 记录。继续流语义仅由 `truth_recovered_bbb_runner.py` 支持，不与本收据混用。
- refusal 是独立 verifier extension：动作显式为 `accept`、`escalate` 或 `refuse`，只读取 selected output、confidence、runtime status 和已释放历史。`episode.abstain()` 只表示 provider failure 后无输出，不等价于主动 refusal。

## 4. 冻结分母

每个 semantic stream 记录：`N_attempt, N_scored, N_failure, N_unknown, N_answered, N_refused, N_correct, N_calls`。unknown 和 refusal 不进入 correctness 分子/分母；failure 进入 `N_attempt` 并计为任务失败。报告 `failure_rate=N_failure/N_attempt`、`unknown_rate=N_unknown/N_attempt`、`refusal_rate=N_refused/N_attempt`、accepted coverage、accepted accuracy、反馈版本、state hash 和 p50/p95 update cost。

## 5. 独立 verifier 与篡改测试

verifier 不导入 runner，逐事件检查：

- terminal → feedback → state 的 entity/provider/action identity、`due_tick=decision_tick+1+delay` 和 exactly-once；
- scored release 才能更新 chemical ledger；failure 可更新 runtime ledger 但 `chemical_correct=null`；unknown 的 learner/version、chemical ledger、failure streak 不变；
- refused event 有 `refusal_action/refusal_reason`，不携带 `answer_correct`，且不被计为 failure 或 wrong answer；
- 输入/cache/source hash 与真实 fixture 绑定；unknown terminal 后没有后续 decision，且 no numeric feedback；
- 对删除 failure、unknown→0、failure `chemical_correct=0`、提前 release、refusal→wrong 的 mutation，verifier 必须 fail closed。

## 6. 证伪条件

出现任一以下情况，semantic claim 失败并保留 receipt：unknown 被算成 0/1、failure 被删出 `N_attempt`、unknown 改变 learner state、failure 携带 chemical correctness、refusal 被当成 wrong/failure、或者 verifier 无法拒绝上述 mutation。

## 7. 通过后的下一步

smoke 通过后，才对已验证的 180-stream raw receipt 做只读统一 denominator 聚合；若要声称 refusal efficacy，另行预登记 5-fold fit-only verifier full table。该 smoke 本身不打开 confirmation、不申请 A800。
