# Baseline raw-run schema audit v1

日期：2026-09-30
状态：开发阶段合同审计；不改变 benchmark、baseline 或方法决议。
目的：检查现有主 baseline 的原始事件日志能否由独立程序重建一次完整的“选择 → 终止 → 延迟释放 → 更新”链路。

## 结论

现有日志足以证明多组 development replay 确实运行过，但还不能称为同一份统一 raw-run contract。Greedy LinUCB 是当前唯一把选择前读取、菜单、事件 ID、terminal、feedback、版本和每一步 state hash 都原生落全的主 baseline。Random、PILOT 和 MIRA 有完整 terminal/release 链路，但缺少至少一部分独立的信息边界或状态转换证据。Matched Online-SGD 与 BayesianRouter 的原始 decision 日志没有 native event ID、terminal 快照或 read-set；现有 recovery 可以用 sealed inputs、tick 和 entity 派生 event ID，并独立重算 selected status、reward、delay/release 和更新计数。因此缺口是 **raw JSONL 自包含性与证据等级不统一**，不是把旧 development 数字判为无效。

这不是说旧结果无效，也不是要求把历史日志改写成统一格式。它明确了 Gate B 当前仍 HOLD 的具体原因：**结果可读不等于证据合同统一**。下一步应补一个统一 runner/recovery 版本，先在 CPU 小批上验证，再决定是否重跑主 baseline；在此之前不应把六组方法合并成一张“完全同条件”的最终排名表。

### 新增的 Matched receipt

本审计之后，Matched Online-SGD 已新增一条不改写旧日志的统一 full-development
receipt：`task/task49-pharma-benchmark-baseline-freeze/matched_sgd_unified_raw_v1/report.md`。
它在当前 MIRA 使用的 frozen BT+CLS representation、direct fit snapshot、722
分子、5 folds、2 seeds 和三种 delay 上完成 60 条 stream，并由独立 NumPy
replayer 通过。该结果只关闭 Matched 的新合同证据，不自动升级旧日志，也不能
代表 PILOT、BayesianRouter、Greedy 或 MIRA 已全部统一。

### 后续统一收据（2026-09-30）

本审计的旧日志表保持不变；后续新建的收据不能倒写历史字段：

- BayesianRouter 新建 `bayesianrouter_native_bt_raw_v1`，使用 recovered BT+CLS
  checkpoint 的 `score.weight` 和同 checkpoint encoder，60 streams/8,664 events，
  独立 action/state verifier 和 6/6 source provenance 通过；旧
  `bayesianrouter_unified_raw_v1` 明确保留为 common-direct-prior control，不再标作
  native C8。
- MIRA/Matched 新建 `mira_unified_raw_v1`，4 methods × 2 scenarios × 5 folds × 2
  seeds × 3 delays，共 240 streams/34,656 events；统一 schema verifier 通过，
  relative candidate 的独立 NumPy/SciPy replayer 60/60 streams 通过，9/9 source
  provenance 通过。

这两条收据关闭了相应方法的事件级 raw-run 缺口，但不自动关闭跨方法
feature/warm-start/compute budget/cost 公平性、provider lineage 或 confirmation。

## 审计对象与方法

审计器是只读脚本 `src/probe_headless/audit_baseline_raw_schema_v1.py`。它直接读取下列已有 JSONL，不读取新的 provider 预测，不训练，也不填补缺失字段：

| 方法 | 原始日志 |
|---|---|
| Matched Online-SGD | `experiments/logs/task49_matched_sgd_development_20260927/recovered_v2/events.jsonl` |
| PILOT-adapted | `experiments/logs/task49_pilot_adapted_development_20260927/run_v1/events.jsonl` |
| BayesianRouter-adapted | `experiments/logs/task49_fingerprint_online_20260927/recovered_v2/events.jsonl` |
| Greedy LinUCB | `experiments/logs/task49_greedy_linucb_development_20260929/full_v3_fix/events.jsonl` |
| MIRA SafeSwitch v2 | `experiments/logs/task49_mira_safeswitch_cpu_20260929/run_v2_fixed/events.jsonl` |
| Random | `experiments/logs/task49_random_development_20260930/run_v1/events.jsonl` |

审计字段分成五层：

1. **运行身份**：输入/source hash、scope、delay 和 method/run identity；
2. **选择前证据**：event/entity/tick、候选菜单、选择动作、当时版本、已释放反馈或 decision-read 集合、propensity、feature hash、选择耗时；
3. **调用终止**：due tick、system success/output status、selected output、correctness/unknown 的显式语义；
4. **反馈更新**：decision/due/release tick、reward/status、version before/after、update latency；
5. **状态可复核性**：每步 state hash before/after，以及 stream summary。

脚本把旧字段名作为审计别名，例如 `select_ns` 和 `selection_ns`、`selected_provider` 和 `provider_id`。别名只表示“找到了语义近似字段”，不表示已经满足所有强合同字段。

## 结果摘要

| run | event 数 | decision 缺失的关键字段 | terminal | feedback | state hash |
|---|---:|---|---|---|---|
| Matched Online-SGD（旧日志） | 28,094 | native event ID、menu、scores、propensity、read-set、state hash | **缺失**（可由 recovery 派生重算） | 有，可由 recovery 重算；unknown/state hash 缺失 | **无** |
| Matched Online-SGD（统一 receipt） | 8,664 | 已补齐 | 有 | 有，selected-only/延迟/版本/state hash 均有 | **有** |
| PILOT-adapted | 26,054 | read-set、state hash | 有 | 有，unknown/state hash 缺失 | **无** |
| BayesianRouter-adapted | 19,390 | event ID、menu、scores、propensity、read-set、state hash | **缺失** | 有，但 unknown/state hash 缺失 | **无** |
| Greedy LinUCB | 26,145 | 无 | 有 | 有 | **decision/terminal/feedback 全有** |
| MIRA SafeSwitch v2 | 78,156 | menu、propensity、feature hash、read-set、state hash | 有；terminal correctness 未单独写出 | 有；unknown/state hash 缺失 | **无** |
| Random | 12,996 | read-set、state hash | 有 | 有；unknown/state hash 缺失 | **无** |

完整机器结果在 `audit.json`，生成表在 `generated_table.md`。运行 `python3 src/probe_headless/audit_baseline_raw_schema_v1.py` 可从当前 JSONL 重新生成。

## 对论文证据的影响

### 可以保留的结论

- 各方法的结果文件、配置、输入 hash 和已有独立 recovery 仍然是有效的 development evidence；这次审计没有发现“日志不存在”或“结果无法读取”。
- Greedy 的逐事件 state-hash verifier 可以作为强合同参考实现。
- Matched Online-SGD/BayesianRouter 的现有 independent recovery 仍可从 sealed inputs 和 raw decisions/feedback 派生重算 selected status、reward、delay/release 和更新计数；这两组数字不因缺少 native terminal 行而作废。本次 BBB development 分母没有 unknown truth，因此缺少 `unknown_truth_handling` 不改变当前数值。
- Random、PILOT、MIRA 的 terminal/release 事件足以支持已有报告中的事件计数和基础延迟语义，但不能自动升级为完整信息边界证明。

### 目前不能声称的结论

- 不能声称所有主 baseline 都由一套完全相同的事件 schema 和独立 replayer 复核。
- 对没有 terminal 的 Matched/BayesianRouter 日志，不能声称 raw JSONL 自身包含 output-status/unknown 的 terminal 快照；可以声称现有 recovery 结合 sealed inputs 派生重算了这些字段及 feedback release。
- 对没有 `decision_reads` 或 `released_feedback_ids` 的日志，不能仅凭 raw JSONL 逐事件证明选择前没有读取未选输出或未来反馈；只能引用相应 runner contract 和现有实现测试。
- 对没有 state hash 的日志，不能声称每次在线状态转移均可字节级审计。
- BayesianRouter 的正式 recovery 脚本原先没有内置逐次 Thompson action replay；本次旁路 verifier 已使用冻结 prior/features、checkpoint 和 policy seed 重放 60/60 streams、8,664/8,664 actions，0 mismatch。其 raw JSONL 仍缺 native terminal/read-set/state hash，但 action 复现证据已补齐；收据见 `fingerprint_diagnostic_v1/online_bayesianrouter_action_replay_v1.md`。Matched Online-SGD 同样有独立的 NumPy action/update recovery。

## 必须统一的 v1 raw-run contract

后续统一 runner 的每条 episode 至少写出以下事件：

```text
config: input/source/code hash, method_id, run_id, feature, fit/warm-start,
        feedback contract, delays, seed, hardware
decision: event_id, run_id, entity_id, tick, menu, selected provider,
          released_feedback_ids, decision_reads, version_before, scores,
          propensity, feature_sha256, selection_ns, state_hash_before
terminal: event_id, entity_id, provider, tick, due_tick, output_status,
          selected_output, correctness or explicit unknown, state_hash_after
feedback: event_id, decision_tick, due_tick, release_tick, provider,
          reward/status, version_before, version_after, update_ns,
          state_hash_before, state_hash_after
stream_complete: method, run_id, fold, delay, n, accuracy, failures,
                 unknowns, p50/p95 selection/update latency
```

其中 `event_id` 必须在 decision、terminal、feedback 中一致；unknown 必须有明确枚举值，不能用缺列或空值代替；`decision_reads` 与 `released_feedback_ids` 不能由 verifier 事后推断；state hash 必须在状态改变前后各写一次。对只支持即时 feedback 的旧方法也保留 `due_tick/release_tick`，令两者相等，而不是删掉字段。

## 修复顺序

1. 在本地 CPU 上实现一个只处理固定 cache 的统一事件 writer 和 replayer；先用 Greedy 的完整字段作为参考，不修改旧日志。
2. 把 Random 和 MIRA 作为最小迁移对象，补齐 decision read-set、state hash、显式 unknown 与 stream summary；用旧结果做 paired replay，确认迁移不改变动作和 reward。
3. 若要把所有方法纳入统一 raw contract，再迁移 Matched Online-SGD/BayesianRouter。它们旧日志不能事后补写成 native terminal/event ID；可以新建 run 目录重放，旧目录永久保留为 v1 evidence。这个重放是合同统一动作，不是对现有数值的否定。
4. 统一后重新生成 development master table 的“可统一复核”列；在此之前主表继续标注每个方法的证据强度，Gate B 不开 confirmation。
5. 只有统一 runner 通过独立篡改拒绝测试、延迟队列测试和跨方法同输入测试后，才考虑下一次 A800。这个审计本身不触发 GPU 任务。

## 收据

- 审计器：`src/probe_headless/audit_baseline_raw_schema_v1.py`
- 机器结果：`audit.json`（SHA-256 `f7e80ef75eeaf31f37d6cab8e5c9448f9a67c953288cfcb9fb4f303f0314eedf`）
- 生成表：`generated_table.md`（SHA-256 `9b7cc6489ed2b0f066153b14808bb95a519d526d05af7811f3654893de72bf43`）
- 当前 Gate 状态：`../current_acceptance_audit_v2/report.md` 与 `research/AAMAS2027_current_evaluation_scorecard_20260930.md`
