# Unified baseline receipt schema v1

日期：2026-10-06
状态：**规范草案，供下一次 raw-run 使用；不回写、不改写历史收据。**

## 1. 目的和边界

现有实验已经有两类收据：

1. `baseline_cost_budget_v1/table.json` 及其 development raw receipts。它们能汇总输入、先验、似然、更新预算和部分运行成本，但不同方法并不是同一套原始事件格式。
2. fresh confirmation 的 `v2_statehash_run3`（natural）和 `v3_bayesian_statehash_run1`（controlled stress）。它们已经原生记录 `decision / terminal / feedback`，并记录 state hash、选择/更新耗时和释放时钟；但还没有逐事件 state bytes、unknown/refusal 原因，也没有显式的 warm-start/likelihood/budget ID。

本规范把两类收据需要共同具备的字段固定下来。它不是把旧日志“补齐”为新格式：历史日志中的缺失字段继续写作 `MISSING`，不得从哈希、模型维度、代码推测或聚合数字反推。

## 2. 状态标记

覆盖表使用以下标记：

- `P`：原始收据中存在，且语义和粒度满足该字段要求。
- `A`：只存在聚合值或文字合同（例如 `max_state_bytes`、`unknown_truth_handling`），不能做逐事件审计。
- `H`：有 state hash，但没有对应的 state bytes；不能声称字节级状态审计。
- `MISSING`：原始收据没有该字段。
- `V`：独立 verifier 已检查字段语义；仅在另有 `V` 时才算验证通过。字段存在不等于字段已被独立验证。

`0` 是真实观测到的零事件；`MISSING` 是没有记录。两者永远不能互换。

## 3. 收据分层

### 3.1 `config`（每次 run 一条）

`config` 必须显式给出以下身份和协议字段：

| 字段 | 类型 | 约束 |
|---|---|---|
| `schema_version` | string | 固定为 `unified_receipt_schema_v1` |
| `run_id`, `method_id`, `scenario_id` | string | 全局唯一；不可由目录名事后推导 |
| `input_sha256` | hex string | 输入分子/标签/折分清单的 hash |
| `menu_sha256` | hex string | 本次候选 provider 菜单及顺序的 hash |
| `feature_id`, `feature_sha256`, `feature_dim` | string/string/int | 选择器实际看到的表示；不得只写“64D” |
| `warm_start_id`, `warm_start_sha256` | string/string | fit-only 参数快照、checkpoint 或显式 `none` |
| `likelihood_id`, `likelihood_config` | string/object | 例如 `binary_logistic_bce`、`gaussian_reward_ridge`，以及噪声/先验等参数 |
| `budget_id`, `provider_call_budget`, `selected_only` | string/int/bool | 每次 decision 允许的 provider 调用数、反馈权限和信息边界 |
| `delay_set`, `feedback_due_rule` | array/string | 例如 `{0,1,4}` 和 `decision_tick + 1 + delay` |
| `truth_visible_to_selector`, `unselected_outputs_visible` | bool/bool | 必须显式为 `false`，或明确说明为何适用 |
| `code_commit`, `source_sha256`, `hardware` | string/object/object | 代码、源文件、硬件和运行环境的可复核身份 |

`warm_start_id`、`likelihood_id`、`budget_id` 是三个不同概念，不能用方法名代替，也不能因为参数写在脚本里就省略。

### 3.2 `decision`（每个选择一条）

```json
{
  "event": "decision",
  "run_id": "...",
  "event_id": "...",
  "entity_id": "...",
  "tick": 0,
  "menu_before_choice": ["provider_a", "provider_b"],
  "selected_provider": "provider_a",
  "released_feedback_ids": [],
  "decision_reads": ["molecule_features", "candidate_menu", "released_selected_feedback"],
  "scores": [0.1, -0.2],
  "propensity": null,
  "feature_sha256": "...",
  "version_before": 0,
  "state_hash_before": "...",
  "state_bytes_before": 1234,
  "selection_ns": 1000
}
```

`decision` 不得包含 truth、selected output、chemical correctness、future feedback 或未选 provider 的输出。若算法没有 propensity，必须写 JSON `null` 并在 `config` 标注 `propensity_semantics: none`，不能省略字段。

### 3.3 `terminal`（每次 provider 调用一条）

```json
{
  "event": "terminal",
  "run_id": "...",
  "event_id": "...",
  "entity_id": "...",
  "tick": 0,
  "due_tick": 1,
  "provider_id": "provider_a",
  "output_status": "ok",
  "system_success": 1,
  "valid_return": 1,
  "selected_output_label": 1,
  "failure_code": null,
  "unknown": false,
  "unknown_reason": null,
  "refusal_action": "none",
  "chemical_correct": 1
}
```

- runtime failure 写 `system_success=0`、`output_status=error` 和 `failure_code`；不可把它写成 chemical error。
- unknown 或拒答写 `unknown=true`、`unknown_reason` 和 `refusal_action`，并令 `selected_output_label`、`chemical_correct`、`reward` 为 `null`。
- scored output 才能写 `chemical_correct ∈ {0,1}`。`chemical_correct=0` 是真实错误，不是 unknown。

### 3.4 `feedback`（每条释放的 selected-only feedback 一条）

```json
{
  "event": "feedback",
  "run_id": "...",
  "event_id": "...",
  "provider_id": "provider_a",
  "decision_tick": 0,
  "due_tick": 1,
  "release_tick": 1,
  "feedback_status": "scored",
  "reward": 1,
  "updated": true,
  "unknown_reason": null,
  "version_before": 0,
  "version_after": 1,
  "state_hash_before": "...",
  "state_hash_after": "...",
  "state_bytes_before": 1234,
  "state_bytes_after": 1250,
  "update_ns": 500
}
```

`release_tick` 是实际进入 selector 状态的时钟，`due_tick` 是合同规定的最早释放时钟；`release_tick < due_tick` 必须使 verifier 失败。没有参数更新的 Frozen 仍要写 feedback event，并明确 `updated=false`（同时可以保持版本不变），不能删掉更新记录。

### 3.5 `stream_complete`（每条 stream 一条）

必须至少汇总 `n_decisions`、`n_terminals`、`n_feedback`、`n_runtime_failures`、`n_unknown`、`n_refused`、`n_scored`、`n_chemical_correct`、`n_chemical_wrong`、`selection_ns_p50/p95`、`update_ns_p50/p95` 和 `max_state_bytes`。summary 不能替代逐事件字段。

## 4. 当前覆盖审计

### 4.1 development baseline receipt

以下结论来自 `baseline_cost_budget_v1/table.json`、`adapted_baseline_receipt_field_audit_v1/audit.json` 和对应 raw JSONL。`warm_start`/`likelihood` 在机器表中有方法说明，但许多 raw `config.json` 中仍为 `null`；因此它们不是可追溯的 config-level ID。

| 方法 | feature | warm-start | likelihood | budget / timing | state bytes **before/after** | unknown / refusal | failure / chemical correctness | release clock |
|---|---|---|---|---|---|---|---|---|
| Frozen direct | `P`（feature 名称/维度；部分 raw event 无 hash） | `A`（表中描述；raw config 未给 ID/hash） | `A`（表中描述；raw config 未给 ID） | `P`（selected-only、delay、selection/update p50/p95） | `MISSING`（只有单值 `state_bytes` 或聚合 p95） | `A`（`0/0` 或 `stop_batch`；无逐事件 reason/action） | `P`（terminal `system_success`/`chemical_correct`） | `P`（due/release tick） |
| Matched Online-SGD | `P`（同 Frozen） | `A`（同 Frozen） | `A`（BCE/lr 文字描述，未形成 ID） | `P` | `MISSING` | `A` | `P` | `P` |
| Greedy LinUCB（adapted receipt） | `P`（冻结 64D 表示） | `A`（method-specific prior；无 snapshot ID） | `A`/`MISSING`（raw config 未固定跨方法 likelihood ID） | `P`（selection/update timing；calls/failure） | `MISSING`（`state_bytes_p95` 在表中为 missing；hash 有） | `A`（unknown/refusal 未逐事件记录） | `P`（runtime status/failure；chemical correctness 字段依收据） | `P` |
| BayesianRouter native-BT | `P` | `A`（native BT prior 可追到 evidence，但无统一 warm-start ID） | `A`/`MISSING`（Gaussian surrogate/adapter 描述，未形成统一 ID） | `P` | `MISSING`（state bytes 未写入该统一 receipt；hash 有） | `A` | `P`（runtime status/failure；chemical correctness 字段依收据） | `P` |
| MIRA-relative / FamilyGuard | `P` | `A`（fit-only direct head + MIRA state；无统一 snapshot ID） | `A`（selected reward + reliability ledger；无 likelihood ID） | `P` | `MISSING`（表中的 `state_bytes_p95` 是聚合，不是 before/after） | `A` | `P`（terminal status、failure 和 chemical correctness） | `P` |

`P` 不代表这些方法已经同一统计层可比；分层和比较权限仍以 `baseline_cost_budget_v1` 为准。尤其是 adapted Greedy/Bayesian 的 `state_bytes_p95` 与 unknown/refusal 字段不能用 common-stratum 的数字替代。

### 4.2 fresh confirmation receipt（natural v2 / stress v3）

fresh runner 对所有方法使用同一事件形状，因此以下是每个方法（Frozen、Matched SGD、Greedy、BayesianRouter、MIRA-Context）的共同覆盖：

| 字段组 | raw receipt | 当前 verifier | 证据位置与说明 |
|---|---|---|---|
| feature / menu / information boundary | `P` | `V`（decision reads、menu、feature hash；truth 未泄漏检查） | `events.jsonl` decision；`verification.json` 的 `forbidden_decision_fields` |
| warm-start identity | `A` | `R` | config 有 provider checkpoint hashes、fit-best/scores，但没有 `warm_start_id`/`warm_start_sha256` |
| likelihood identity | `A` | `R` | config 有部分方法超参（MIRA/Greedy/Bayesian/SGD），没有统一 `likelihood_id` |
| budget / selected-only / delay | `P` | `V`（event cardinality、selected-only 合同、due tick） | config protocol + event stream |
| state hash before/after | `P` | `R`（verifier 尚未独立重算每一步 hash） | decision/feedback 各有 hash；terminal 没有 hash |
| state bytes before/after | `MISSING` | `MISSING` | summary 只有 `max_state_bytes`，且不等于 before/after 或完整运行内存 |
| unknown / refusal / reason | `MISSING` | `MISSING` | terminal 只有 `provider_status=ok`、label 和 correctness；没有 unknown/refusal 枚举 |
| selection/update timing | `P` | `V`（事件存在；summary 聚合） | `selection_ns`、`update_ns` 及 summary p50/p95 |
| runtime failure | `P`（`provider_status` 字段；当前两次 fresh run 全部 observed `ok`） | `V`（输出/事件身份重放） | natural `42,390` terminals、stress `23,550` terminals |
| chemical correctness / reward | `P` | `V`（依据 sealed predictions/truth 重算） | terminal `chemical_correct`；feedback `reward` |
| release clock | `P` | `V`（`due_tick = tick + 1 + delay`） | terminal/feedback `due_tick`, `release_tick` |

当前 fresh verifier 的 `V` 只表示它做过的检查：它重放 action sequence、output、correctness、event identity 和 due clock；它没有把 raw `state_hash_before/after` 与独立序列化状态逐条对照，也没有验证 state bytes（因为不存在）。因此不能把 `verification.json: verified` 写成“所有 schema 字段均已独立验证”。

### 4.3 当前结论

- Frozen、Matched、Greedy、BayesianRouter 和 MIRA 都已有真实的选择、终止、反馈和延迟事件；这部分可以作为论文的 event-level evidence。
- 所有五种方法都缺少下一版所需的逐事件 `state_bytes_before/after` 和 `unknown/refusal` 语义。Fresh receipts 还缺显式 warm-start、likelihood、budget ID。
- Frozen/Matched/MIRA 的旧 development 表有聚合 state p95；Greedy/Bayesian 的 adapted 表明确为 `MISSING`。这些数字不能跨 receipt 借用。
- Fresh natural 的 `provider_status` 全为 `ok`，所以它证明了当前 cohort 没有 runtime failure，不证明 failure/unknown/refusal 能力。

## 5. 下一次 raw-run 最小验收规则

下一次 run 只有在以下规则全部通过时，才可进入统一 baseline 主表；否则保留为 development/audit-only：

1. **身份完整**：config 的 `schema_version`、run/method/scenario ID、input/menu/feature/warm-start/likelihood/budget/code hashes 均非空；不存在用方法名、维度或目录名代替 hash 的情况。
2. **事件守恒**：每个 `event_id` 恰好有一条 decision、一条 terminal、一条 feedback；`stream_complete` 的计数与 raw JSONL 一致；重复、缺失或跨 stream 绑定立即失败。
3. **信息边界**：decision 不能出现 truth、selected output、chemical correctness、future feedback 或未选 provider output；`decision_reads` 和 `released_feedback_ids` 必须原生写出，不能由 verifier 事后推断。
4. **状态可复核**：decision/terminal/feedback 均写 `state_hash_before/after`；decision 和 feedback 写 `state_bytes_before/after`，并由独立 verifier 重新序列化后核 hash。字节数必须为非负整数；不得从 hash、矩阵维度或 p95 反推。
5. **结果语义互斥**：`output_status`、`system_success`、`valid_return`、`unknown`、`refusal_action`、`chemical_correct`、`reward` 的组合必须通过枚举校验。unknown/refusal 时 correctness/reward 为 null；runtime failure 不得被计作 chemical wrong。
6. **释放时钟**：`due_tick = decision_tick + 1 + delay`；`release_tick >= due_tick`；feedback 中的 event/provider/version 与 decision/terminal 一致。即时反馈也保留 due/release 字段。
7. **时间口径**：`selection_ns` 只计 policy decision，`update_ns` 只计 feedback update；两者均非负整数，I/O、provider 服务和 evaluator 时间不得混入。summary 的 p50/p95 必须由 raw event 重算。
8. **失败封闭**：unknown、refusal、runtime failure 各自按 event 计数并进入分母；字段缺失不按 0 处理、不静默删行。任何缺字段的 run 不得升级为“统一可比”。
9. **独立复核**：verifier 不得直接调用被测 runner 的 `run_stream`、policy state 或结果 summary 来“自证”；至少要有独立的 event conservation、信息泄漏、延迟队列、动作序列和 state hash 重放检查。
10. **不可变证据**：保存 config、raw JSONL、summary、verification、输入/代码/模型 hash 和失败日志；验证结果与源文件 hash 写入 `verification.json`。旧收据永久保留，不能覆盖历史目录。

## 6. 可复用性

该 schema 可以同时承载 binary-logit、Gaussian reward、MIRA reliability 和未来 refusal/verification 扩展。它只规定可观察事件和身份，不规定某个算法必须使用哪一种后验或更新器；因此不会把 baseline 的统计层误当成公平条件，也不会妨碍方法创新。
