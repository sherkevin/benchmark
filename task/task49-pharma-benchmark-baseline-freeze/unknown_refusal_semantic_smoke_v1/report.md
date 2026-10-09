# E5-Semantic-Closure v1：执行报告

日期：2026-10-01  
状态：semantic accounting receipt verified；不产生效果结论。

## 结果

- v1 runner 在第一条真实 stream 暴露工程错误：structured `Decision` 没有 `feature_sha256` 字段，收据在输出前中止；失败配置保留在 `experiments/logs/task49_unknown_refusal_semantic_smoke_20261001/v1/`。
- v2 使用修正后的提交 `81cf9d08d66ff6852aa93a64a66c2375548e4c09` 重跑，4 条主 stream（scored/failure/refusal 的 delay `0/1`，unknown stop-batch 的 delay `0/1`），20 条 JSONL 事件，真实缓存和真实 unresolved `z002970` 均被使用。
- 每条 scored/failure stream 的冻结分母为 `N_attempt=3, N_scored=1, N_failure=1, N_refused=1, N_correct=1, N_calls=3`；unknown stream 为 `N_attempt=1, N_unknown=1, N_calls=1`，没有 numeric feedback，也没有后续 decision。
- 独立 verifier `verification_v2.json` 通过：6 个事件 stream（含两个独立 refusal stream）、20 条事件、2 条 refusal；source-tree hash、provider/entity identity、delay、state hash 和 version 均通过。
- 4 个篡改用例（unknown→0、failure 带 chemical correctness、提前 release、refusal→wrong）在重新绑定 event hash 后全部被 verifier 拒绝：`mutation_tests_v2.json`。

## 语义核对

- scored feedback 更新 chemical ledger/version；
- runtime failure 更新 MIRA/FamilyGuard 的 runtime availability/failure-streak，但 `chemical_correct` 保持 null；
- unknown 使用当前生效的 stop-batch 语义：terminal 后消费 pending lifecycle、learner/version/chemical ledger/failure streak 不变，不释放 numeric feedback；
- refusal 是独立 post-call verifier action，既不记成 failure，也不记成 wrong answer。

## 边界

这轮只证明事件 accounting 和 recovery 可以区分 scored、failure、unknown 和主动 refusal。它没有估计 MIRA/FamilyGuard 的 selector 效果，没有证明 refusal utility、dynamic verifier efficacy、unknown 泛化、自然 drift 或 confirmation。

下一步可以对已验证的 common-stratum raw receipt 做只读 failure/unknown 汇总；若要写 refusal efficacy，必须另行登记五折 fit-only verifier full table，并使用同 accepted/refusal rate 的 random、always-accept 和 zero-coverage controls。

