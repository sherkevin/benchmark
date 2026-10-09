# Task49 baseline 原始日志 schema 审计草稿

日期：2026-09-30  
用途：Gate B 内部审查。只读检查已有 JSONL；不修改原始日志，不把能从输入推导出的字段冒充为原始证据，也不改变 baseline roster。

## 1. 审查范围

本审查看到的主 baseline 原始 JSONL：

| baseline | raw JSONL | 规模/事件类型 |
|---|---|---|
| Matched Online/Replay SGD | `experiments/logs/task49_matched_sgd_development_20260927/recovered_v2/events.jsonl` | 12,996 decision + 12,996 feedback；无 terminal |
| BayesianRouter-adapted | `experiments/logs/task49_fingerprint_online_20260927/recovered_v2/events.jsonl` | 8,664 decision + 8,664 feedback；无 terminal |
| PILOT-adapted | `experiments/logs/task49_pilot_adapted_development_20260927/run_v1/events.jsonl` | 8,664 decision + terminal + feedback |
| Greedy LinUCB | `experiments/logs/task49_greedy_linucb_development_20260929/full_v3_fix/events.jsonl` | 8,664 decision + terminal + feedback；最完整 |
| MIRA SafeSwitch v2 | `experiments/logs/task49_mira_safeswitch_cpu_20260929/run_v2_fixed/events.jsonl` | 25,992 decision + terminal + feedback；config 在旁路 `config.json` |
| Random | `experiments/logs/task49_random_development_20260930/run_v1/events.jsonl` | 4,332 decision + terminal + feedback；config 在旁路 `config.json` |

静态 MLP/BT+CLS、Best Fixed 和 Oracle 没有同一种在线事件流，不能塞进这个 raw-run schema；它们应使用各自的 fit/evaluation receipt，并在主表中标注为 static/evaluator-only。

## 2. 统一 raw-run schema（建议 v1）

schema 的目标是让独立 replayer 只读 `manifest + JSONL`，即可验证“选择前看到了什么、结果什么时候产生、反馈什么时候释放、状态如何变化”。字段缺失不能用输入文件或代码静默补齐。

### 2.1 run manifest/config（必须有）

`schema_version`, `run_id`, `method_id`, `scope`, `code_commit`, `source_sha256`, `input_sha256`, `provider_menu_sha256`, `feature_id/feature_sha256`, `fold`, `seed`, `delay`, `budget`, `selected_only`, `feedback_due_rule`, `failure_semantics`, `unknown_truth_handling`, `hardware`, `software`, `started_at`, `ended_at`, `expected_counts`, `raw_events_sha256`。

`fold/seed/delay` 可以在每个事件重复，也可以在 stream manifest 中给出，但必须能无歧义绑定每个 `event_id`。`run_id/method_id` 不能只靠目录名猜。

### 2.2 decision（每条选择一条）

`event_id`, `run_id`, `method_id`, `stream`, `episode_id/entity_id`, `fold`, `seed`, `tick`, `delay`, `menu_before_choice`（或 menu hash + 可复核 manifest）, `selected_provider`, `decision_reads`, `released_feedback_ids`, `version_before`, `state_hash_before`, `scores`（若算法有分数）, `propensity`（随机策略或采样策略）, `feature_sha256`, `selection_ns`。

`decision_reads` 必须明确读取了哪些 feature/state 字段；空集合也要写 `[]`。`released_feedback_ids` 必须是选择时已经释放的 feedback，不能用“当前版本号”替代。

### 2.3 terminal（每次工具调用终止一条）

`event_id`, `stream`, `episode_id/entity_id`, `provider_id`, `tick`, `terminal_tick`, `output_status`, `valid_return`, `selected_output` 或不可逆的 output digest, `chemical_correct`/`recovered_label`（可为空但须说明原因）, `system_success`, `feedback_status`（例如 `scored`, `unknown`, `failed`）, `provider_event_ids`, `unknown_truth_handling`。

terminal 不应把未选 provider 的当前 output 写入同一事件；如为回放评测保存全 panel，须放在 evaluator-only 文件并与 policy 事件隔离。

### 2.4 feedback（每次释放一条）

`event_id`, `decision_event_id`（当前很多日志把它复用成 `event_id`，新 schema 应显式区分）, `provider_id`, `decision_tick`, `due_tick`, `release_tick`, `delay`, `reward`（或 `reward=null + status`）, `feedback_status`, `version_before`, `version_after`, `state_hash_before`, `state_hash_after`, `update_ns/update_ms`, `unknown_truth_handling`。

### 2.5 stream summary/verification

`stream_id`, `n_decisions`, `n_terminals`, `n_feedback`, `unknown_count`, `failure_count`, `final_version`, `final_state_hash`, `accuracy`, `raw_events_sha256`, `verifier_sha256`, `status`。汇总不能代替逐事件字段。

## 3. 现有日志覆盖审计

符号：`✓` 原始字段存在；`△` 有近似字段但需显式别名/语义声明；`✗` 缺失；`—` 不适用。

| run | run manifest | decision identity/menu | pre-choice visibility | state hashes | terminal | feedback release | 主要缺口 |
|---|---|---|---|---|---|---|---|
| Matched Online/Replay SGD | △ config 有 input/source/code，但无 run_id/method_id | △ entity/tick/provider；无 event_id、menu、scores、propensity | ✗ 无 released ids、decision_reads | ✗ | ✗ 无 terminal | △ feedback 有 due/release/version；无 status/unknown | reward 被写进 decision；无法仅凭 raw JSONL 重建 terminal 和选择前边界 |
| BayesianRouter-adapted | △ config 有 input/source/code，但无 run_id/method_id | △ entity/tick/provider；无 event_id、menu、scores、propensity | ✗ | ✗ | ✗ 无 terminal | △ due/release/version；无 status/unknown | 同上；feedback 的 event_id 无 decision 对应 event_id |
| PILOT-adapted | △ config 有 input/source/code/provider list，但无 run_id/method_id | ✓ event/entity/tick/provider/menu | ✗ 无 released ids、decision_reads | ✗ | △ terminal 有 output/status/correctness，但无 unknown/state hashes | △ event/due/release/version；无 feedback status/unknown | 能复核输出时序，但不能逐事件证明选择前读取集合或状态转换 |
| Greedy LinUCB | ✓ config + fit manifest；run_id 通过事件字段 | ✓ event/episode/menu/action/tick | ✓ decision_reads + released ids | ✓ decision/terminal/feedback before/after | △ 有 terminal，但 terminal 无 `feedback_status` | ✓ release/due/status/version/update/state | 需把 config 的 `run_id`/stream manifest 与每事件绑定固定；terminal status 可标准化 |
| MIRA SafeSwitch v2 | △ 独立 config 完整，但 JSONL 无 config/run manifest 事件 | △ event/entity/tick/provider；无 menu、propensity、feature hash | ✗ | ✗ | △ output/status/selected label；correctness 只在 feedback 出现，无 unknown/state hashes | △ due/release/version；feedback 有 chemical_correct/recovered_label，但没有统一 reward/unknown 语义 | correctness 可从 feedback 读出，但无法独立重建选择前状态和统一 reward 语义；config 与 raw 未通过 manifest hash 绑定 |
| Random | △ 独立 config 完整，但 JSONL 无 run/stream 字段 | ✓ event/entity/tick/provider/menu/score/propensity/feature | ✗ | ✗ | ✓ output/status/correctness | △ release/due/version/reward；无 feedback status/unknown | 外层 wrapper 有 `stream`，但 payload/config 未显式绑定 `run_id/method_id`；缺 visibility/state hash |

补充：Greedy 的 state hash、`decision_reads`、`released_feedback_ids` 是当前唯一在 decision/terminal/feedback 三段都比较完整的主 baseline raw contract。它也不是“所有 baseline 都已合约化”的证据；其余运行需要统一 runner 或明确 legacy/audit-only 标记。

## 4. 必须补齐项（按 Gate B 优先级）

### P0：统一运行器必须具备

1. 在每个 raw package 写一个 manifest/config event，并通过 `raw_events_sha256`、`code_commit`、`input_sha256`、`provider_menu_sha256` 绑定。
2. 为每条 decision 生成唯一 `event_id`，同时写 `run_id/method_id/stream`；不要从目录名或事件顺序推断。
3. 写出 `menu_before_choice`、`released_feedback_ids`、`decision_reads`。固定菜单也要写 menu hash；没有已释放反馈写 `[]`。
4. 增加 terminal 事件。Matched/BayesianRouter 必须重跑；不能从 input 事后拼成原始 terminal。
5. 每次 feedback 显式写 `decision_event_id`、`release_tick`、`feedback_status`、`unknown_truth_handling` 和 `reward`（未知时写 null，不得把 unknown 当 0）。
6. decision/terminal/feedback 写 `state_hash_before/after`；至少对算法 state、反馈队列和版本号做确定性哈希。

### P1：对已有日志做 sidecar，而不是篡改原文件

- 为 Random、PILOT、MIRA 建立 `legacy_manifest.json`，把原始 events SHA、config SHA、代码 commit、输入 SHA、事件计数绑定起来；派生的 `decision_event_id`、menu hash、stream id 标成 `derived`。
- 为 Matched/BayesianRouter 建立缺失 terminal 的审计说明和 recovery 入口；派生字段只能支持“结果可由外部输入核对”，不能升级为“raw contract 已满足”。
- 对 Matched 的 decision `reward` 标为 `posthoc_audit_field`。在新的 runner 中，decision 事件禁止含 reward/chemical correctness；这些字段只能出现在 terminal/feedback。

### P2：再跑一轮统一 development matrix

只在 P0 schema 验证通过后，重跑主 roster 的固定 development slice（同 input、fold、seed、delay、菜单、feature 和随机种子）。先用本地 replay 验证：

- `n_decision == n_terminal == n_feedback`；
- 每个 feedback 的 `decision_event_id` 唯一且对应一个此前 decision；
- `release_tick >= due_tick` 且满足 delay 公式；
- decision 的 released ids 不含未来 feedback；
- selected-only 下未选 provider 不出现在 `decision_reads`；
- state hash 与独立 NumPy replayer 一致；
- unknown/failure 分母和 accuracy 可从 raw events 重算。

通过后再更新 Gate B；在此之前，已有 accuracy 仍可作为 development 结果，但不能写成“所有 baseline 已满足统一 raw-run contract”。

## 5. 结论

当前 raw logs 足以支持若干 development 数字，尤其 Greedy/Random/PILOT/MIRA 有 terminal/feedback 事件，且各自 verifier 能复核部分时序；但从统一的 AAMAS 可审计标准看，Gate B 仍 HOLD。主要原因不是事件数量，而是四个信息边界缺口：

- Matched/BayesianRouter 无 terminal、无 decision event id；
- 除 Greedy 外没有逐事件 `released_feedback_ids` 和 `decision_reads`；
- 除 Greedy 外没有 state transition hashes；
- MIRA 的 correctness 只在 feedback、没有统一 reward 字段，且 terminal/feedback 语义未规范化；Random/MIRA 等缺显式 run/method manifest binding。

下一步应先实现一个可复用的 unified raw-run writer/replayer，再一次性重跑小规模 development slice；不要分别给每个 baseline 打补丁，也不要通过事后推导让旧日志看起来满足新合同。

## 6. 可复用资产

仓库已有只读审计器草稿：`src/probe_headless/audit_baseline_raw_schema_v1.py`。它能识别旧字段 alias，但 alias 只用于审计，不等于契约字段已经满足。建议下一步将其扩展为每条事件的完整性检查器，并让统一 runner 直接输出此 schema。

## 7. 原始文件 SHA-256 收据

| baseline | raw JSONL SHA-256 |
|---|---|
| Matched Online/Replay SGD | `4f7bed1d53aabd093943ac18e525b5cf62ffe56250237ecb0b74f42d2d1c6b9e` |
| BayesianRouter-adapted | `cf66af547cae8d243ae173595fc63faada1d1b1872943b2d50f9547decc13fdf` |
| PILOT-adapted | `f885cdc79b95977624a623044de1f8a6e8e9a852cb4b8c9ffe69459482b53d1f` |
| Greedy LinUCB | `7d96d466f5ee5ae6098e19154a343077110860f1882e8e7d5551cb967f55ce1c` |
| MIRA SafeSwitch v2 | `fe3ee5edca1ab05e36505993037e0c7b9322730aad6e2627d0a9e7354e4d184a` |
| Random | `426eb9dd0c42459e693ee12d2ae144f85ec33868f76ea14802cf495b5356ae5d` |
