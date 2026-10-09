# Unknown / refusal 分母缺口审计 v1

日期：2026-10-01  
状态：开发阶段设计审计；没有新增效果实验，不打开 confirmation。

## 结论

当前 common-stratum 和 family-outage 回执已经把 provider runtime failure 计入分母，但还没有形成可审查的 unknown truth 和 agent refusal 共同口径。因此 E5 目前只能声称“failure accounting 已有”，不能声称“拒答策略已验证”。这不是把缺失的结果当成负结果，而是一个尚未执行的实验合同缺口。

## 三种事件必须分开

| 事件 | 发生时点 | 当前实现语义 | 能否计入 correctness |
|---|---|---|---|
| provider runtime failure | provider 调用结束 | `system_success=0`，`feedback_status=failure`；仍进入任务分母。对 MIRA/FamilyGuard，failure 可以更新 runtime availability/failure-streak/group ledger，但不能携带或更新 chemical correctness | 不能，按失败计 |
| unknown truth | 题目终止后无法获得可信真值 | common-stratum 配置写的是 `unknown_truth_handling=stop_batch`；当前 `single_call_record` 会拒绝 `unscorable` 并中止该预声明 cohort，因此没有可比较的 unknown 行 | 不能，须单独记 censoring |
| verifier refusal | 调用后风险门决定不采纳或不继续调用 | 当前 common-stratum selector 没有 `refuse` action；静态 confidence escalation 只实现 accept/escalate | 不能，必须同时记拒答代价和后续调用代价 |

`episode.abstain()` 只表示 provider 没有可用返回，不能自动解释成方法主动拒答。把这两种情况合并会掩盖 verifier 的真实成本。

## 当前收据能证明什么

1. common-stratum 的 `unknown_or_unscorable=0` 是开发输入的事实，不是 unknown 处理能力的证据；当前 `run_bbb_stream` 经 `single_call_record` 遇到 `unscorable` 会抛错中止，连一条显式 censor row 都没有，失败则留在分母。
2. FamilyGuard 的 159 条真实 selected runtime failure 已进入主分母；这部分可以用于 E5 failure accounting。
3. 静态 confidence escalation 有 `0.923823` development accuracy、平均调用 `1.206`，但它是 post-call accept/escalate 控制，不是 refusal policy，也没有 selected-only online update。
4. delayed dynamic verifier 的风险门回放没有通过成本闸门；它的失败应保留为“保守门在当前样本和预算下不实用”，不能改写成 refusal 已完成。

另有一条 `truth_recovered_bbb_runner.py` 已实现“unknown 入队、继续 stream、unknown 不更新”的语义，但它与当前 common-stratum 的 `stop_batch` 合同不一致。两种语义不能在同一 benchmark 结果中混用；要采用 continue 语义必须先写决议并重封存合同。

## 最小可执行补缺合同

下一轮只做 CPU development semantic smoke，不读取 confirmation，不改方法主结果：

1. **Unknown censoring control**：复用真实 unresolved molecule，按当前生效的 `stop_batch` 语义封存一个最小 fixture；另记录 continue 语义只能作为待裁决候选。unknown 只在 terminal 后出现，不能填成 0 或 1。若采用 continue runner，必须显式声明它是 semantic stress，不得接回 722 条 scored effect。
2. **Unknown mask 不得泄漏**：fixture 的 entity/tick mask 在 policy action 前按 `(fold, seed, stream, tick, entity_id)` 封存，不能由 truth、provider output 或方法动作决定；verifier 检查 decision read-set。当前 `run_bbb_stream` 不能直接执行 unknown，因为 `single_call_record` 会 raise；smoke 必须使用专门的 stop-batch adapter 或已审查的 truth-recovered queue。
3. **Refusal action contract**：把 post-call verifier extension 的动作显式编码为 `accept / escalate / refuse`。`refuse` 不得读取真值或未选 provider 输出；它只使用 selected output、confidence、runtime status 和已释放历史。它是 verifier 扩展，不把结果归给当前 MIRA selector。
4. **Matched controls**：至少保留 fixed confidence threshold、同 accepted/refusal rate 的 random policy、always-accept 和 always-refuse/zero-coverage 边界。direct selector 没有 verifier 时标为 `not_applicable`，不把零拒答当优势。
5. **冻结分母和 utility**：逐 episode 记录 `N_attempt/N_scored/N_failure/N_unknown/N_answered/N_refused/N_correct/N_calls`。unknown/refusal release 时，pending event 必须被消费，MIRA 的 learner/version、chemical ledger 和 failure streak 必须不变；failure 对 MIRA/FamilyGuard 可以更新 runtime availability/failure-streak，但 `chemical_correct` 必须保持 null。不能要求整个 queue/hash 对象不变。没有冻结 utility，就不能把 accuracy 提升叫作 verifier 改善。

## 证伪条件

- unknown mask 下任一方法改变 learner/version、chemical ledger 或 failure streak、释放 numeric reward 或把 unknown 算作 failure，独立 verifier 必须拒绝；pending event 仍必须 exactly-once 消费；failure 若没有按各方法合同更新 runtime ledger，或携带 `chemical_correct`，也必须拒绝；
- MIRA 的 refusal utility 不高于 matched random，或拒答率/追加调用超过预登记预算，则 refusal claim 失败；
- 对明确登记的 `no-postcall-signal` ablation，如果不提供 confidence/runtime signal，verifier 不能稳定超过同动作率 random；这条不能泛化到所有 MIRA history/features。

建议的最小 smoke fixture 是：1 个 scored return、1 个真实 cached failure、1 个真实 unresolved molecule，再加 1 个 post-call confidence refusal。该 smoke 只验证 accounting/recovery，不产生 selector superiority CI。

## 当前动作

本审计把 E5 的真正缺口固定为“事件语义和 utility 未封存”，而不是继续堆 selector 变体。下一步先写并审查该 stress 的 preregistration 与独立 verifier，再决定是否跑小规模 CPU smoke；在此之前不把 MIRA 的 `refuse` 写成已实现效果，也不申请 A800。
