# Fresh mechanism H7 receipt closure v1

状态：**设计冻结，待执行**（2026-10-08）。这是 policy-layer receipt/recovery audit，不是新效果实验；不改 benchmark、方法公式、v2 数字或主版 PDF。

## 1. 要回答的问题

当前 v2 已能由独立 verifier 重算 decision score、selected output、feedback timing、credit destination 和 state hash，但 terminal/final state 不是原生 receipt 字段，也没有对断点重启、声明的 wire 乱序、缺包和重复包做系统性拒绝测试。本卡只验证：

> 在同一 sealed checkpoint → provider prediction → fit prior → selected-only policy chain 下，每条 decision/terminal/release 的 action、state 和 summary metric 是否能由 receipt 独立重建；允许的进程重启和传输重排是否保持同一轨迹；缺失、重复或篡改是否 fail closed。

这项证据的声明范围是 four-provider、one-call、selected-only delayed feedback、natural/controlled evaluator-side replay。它不证明 provider 重新训练、训练数据 lineage、自然部署 drift、verification/refusal 或 agent-level gain。

## 2. 近邻与否定条件

最近比较对象是 `fresh_mechanism_ablation_v2` 的 hash-only receipt/verifier；可复用的完整状态语义来自 `fresh_context_state_replay_v2`，checkpoint/fit-prior 绑定语义来自 `mira_relay_v1` 的 contract replay。它们是审计参照，不是新的效果 baseline。

以下任一项失败，H7 不通过：

1. 任意 decision score、action、terminal label、feedback state、final state 或 summary metric 不能被独立 replayer 精确重算；
2. 断点重启或声明的 logical-clock wire reorder 与 uninterrupted trajectory 的任一 state/action/final hash 不同；
3. 缺一个 decision/terminal/feedback、重复 event、篡改 reward/state hash/due tick、截断或错误 schema 仍生成 summary；
4. provider prediction 或五折 fit prior 的重建超过预登记容差，或 fit/confirmation 成员发生重叠；
5. 未来 feedback 在逻辑时钟规则之外提前影响 decision。

## 3. 固定输入、范围和运行矩阵

- policy replay 的只读输入：`experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/provider_predictions.json`、`experiments/logs/task49_fit_score_reference_20261007/v1/prior_cache.json`、`experiments/logs/task49_gate_a_20260929/bbb_confirmation_candidate_manifest_v1/manifest.csv`、v2 natural/controlled receipts；不调用 provider、LLM、网络或 GPU。
- 可选 provenance audit 的输入另外固定为 `experiments/logs/task49_provider_fresh_menu_20260930/cpu_v3/checkpoints/`、`experiments/logs/task49_fit_score_reference_20261007/v1/development_provider_probabilities.jsonl`、`development_selector_features.npy`、`fit_scores.jsonl`、`oof_scores.jsonl`、`fit_reference_means.json`、`provider_train.csv`、`provider_validation.csv` 及 `src/probe_headless/run_provider_fresh_confirmation_v1.py` 的 `load_checkpoint_predictions`/`fit_prior`/`selector_feature`。这些文件的 SHA、依赖和命令必须在 provenance manifest 中记录；若任一文件或命令不可用，provenance 只记为 `UNAVAILABLE`，不把 cache audit 写成 forward/fit reconstruction 通过。
- policy replay：5 folds × 2 deterministic schedule replicas × 3 delays (`D=0,1,4`) × 3 methods（Frozen、MIRA-Relay/Local、fixed cyclic permutation credit）= 90 streams；natural 与 controlled 各一份。
- 受控条件仍为 v2 已冻结的 evaluator-side label flip；不得重新选择 drift tick、provider、分子或超参。
- CPU 环境、Python/RDKit/NumPy/Torch 版本、输入/实现 SHA、代码 commit 全部写入 config；结果写入新的 `experiments/logs/task49_fresh_receipt_closure_v1_20261008/`，不覆盖 v2。

## 4. Receipt v3 合同

每个 run 写入 schema/version、source/input/checkpoint/manifest/prior hashes、provider forward hash、fit config/code hash、依赖版本和 logical-clock contract。

每条 event 至少写入：

- `event_id`, `event_kind`, `logical_tick`, `phase`, `logical_ordinal`, `wire_seq`, `prev_event_hash`；
- `decision_tick`, `due_tick`, `release_tick`（适用时）；
- `state_hash_before`, `state_hash_after`, `state_ref_before`, `state_ref_after`；
- decision 的完整 score vector、menu、selected provider/read set；
- terminal 的 provider status、selected output digest/label、chemical correctness、terminal receipt hash；
- feedback 的 reward/status/credited provider/source delta/version。

每条 stream 写入 immutable state store（可去重）及 SHA、`initial_state_hash`、`final_state_hash`、`final_state_bytes_sha256`、event-chain hash、counts、summary metric 和最后的 `stream_complete` 逻辑事件。state bytes 的 canonical JSON 编码、数组 dtype 和排序规则写入 schema，避免只保存不可解释的摘要 hash。

## 5. 恢复和故障测试

1. **Provenance reconstruction / cache audit**：先执行预登记的 native helper 命令并保存 provenance manifest。若全部输入可用，则概率逐项最大绝对误差为 `1e-8`，labels 和 selected action exact，五折 prior 同样为 `1e-8` 且 fit/confirmation disjoint；超限保留失败，不换容差。若输入不全，只报告 sealed cache 的路径、SHA 和成员审计，不计为 reconstruction 通过。
2. **Canonical replay**：import-free NumPy/RDKit replayer 重算 90 streams 的完整状态、action、terminal、release、summary，并与 v2 只读绑定；summary 由 replayer 独立生成到临时文件，全部检查通过后才原子 rename；不重算或改写效果 estimand。
3. **Checkpoint restart**：90 streams 全部做 canonical replay；restart 是预登记的代表性压力覆盖。对 Local natural/controlled 的三种 delay，以及 Frozen/permutation 的 `D=4` 抽样，在 logical **event ordinal 31、79、143 之后**保存 checkpoint（state bytes、pending queue、terminal/consumed IDs、metric、last event hash、suffix first event id），从 checkpoint 接续 suffix；要求逐事件 action/state/final hash/metric 与 uninterrupted run 相同。
4. **Logical-clock reorder**：固定三种 wire permutation，并保存 permutation manifest；从 immutable spool 读取，以 `(logical_tick, phase, logical_ordinal)` 排序，规则是同 tick 先 release、再 decision、terminal 紧随 decision；重排后 trajectory/final hash 必须相同。声明“允许的 logical-clock reorder”，不声称任意乱序可恢复。
5. **Fail-closed mutation**：分别删除一个 decision/terminal/feedback，复制 event，篡改 state hash/reward/due tick，截断日志，改 schema；每种都必须拒绝且不产生最终 summary。把被删除 event 从 immutable spool 按 `event_id` 重新插入后，要求 exact recovery，并保留每个拒绝现场。

## 6. 通过标准和可写结论

通过标准：natural/controlled 两组各 90/90 streams complete；state/action/terminal/summary exact；terminal/final hash 100% 有值且匹配；代表性 checkpoint suffix exact；所有 mutation fail closed；若 provenance 输入齐全则 forward/prior reconstruction 通过，否则明确标 `UNAVAILABLE` 而不把它写成通过；所有原始日志、失败现场、config、manifest、verifier 输出和 hashes 留存。

若通过，可把 H7 写为 **PASS_FOR_DECLARED_POLICY_REPLAY**，并在方法验收表中将 receipt/recovery 项从部分通过提升；仍不能把它写成端到端 provider inference/offline-fit reproduction，也不能自动解锁完整方法 Gate。若失败，保留失败现场，主版不改，按具体失败条件修正下一版卡。
