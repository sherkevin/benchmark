# Fresh confirmation state hash verifier audit

日期：2026-10-07
范围：只读审查 `src/probe_headless/verify_provider_fresh_confirmation_v1.py`、`src/probe_headless/run_provider_fresh_confirmation_v1.py`、fresh receipt 以及 `research/experiment_section_style_20261006/unified_receipt_schema_v1.md`。
本报告没有修改主论文、没有修改实验结果、没有重新运行科学实验，也没有提交代码。

## 1. 结论

当前 fresh receipt 已经写出一部分状态字段，但当前 verifier 不能证明 state transition 正确，也不能独立证明选择器没有读取禁止信息。

### 1.1 证据

- `v2_statehash_run3` natural fresh：270 streams、127,170 events，`verification.json` 为 `verified`。
- `v2_statehash_run2` controlled stress：120 streams；`v3_bayesian_statehash_run1`：150 streams；两者也有同样的 raw event 形状。
- fresh `decision` 有 `state_hash_before/after`；fresh `feedback` 有 `state_hash_before/after`；`terminal` 没有 state hash，也没有 `run_id/method_id/episode_id` 等统一身份字段。
- fresh receipt 没有逐事件 `state_bytes_before/after`；这与 unified schema 的 `MISSING` 结论一致。

### 1.2 关键否定

`verify_provider_fresh_confirmation_v1.py` 第 1–110 行中没有任何 `state_hash` 或 `state_bytes` 重算逻辑。它只重放生产 runner 的 `run_stream`，比较 provider sequence 和 `n_correct`：

- 第 12–16 行直接导入生产实现，包括 `run_stream`、policy classes、`make_policy`；
- 第 74–101 行调用生产 `run_stream`，并把 `predictions`、`truth`、`features` 全部传入；
- 第 97–100 行只比较 provider sequence 和 accuracy；
- 第 103–105 行的 hash 只是对 `events.jsonl`、`summary.json`、`config.json` 文件做 hash，不是逐事件 state hash 复核。

因此目前的 `verification.json: verified` 只能表示“当前 verifier 的事件计数、部分输出、provider sequence 和聚合正确率检查通过”，不能写成“状态转移已独立验证”或“选择前信息边界已被独立证明”。

## 2. 当前 receipt 的实际状态字段

以 `experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/events.jsonl` 为例，字段是：

| 事件 | 当前存在 | 当前缺失 | 说明 |
|---|---|---|---|
| `decision` | `menu`、`provider_id`、`scores`、`version`、`state_hash_before/after`、`decision_reads`、`feature_sha256` | `released_feedback_ids`、`version_before/after`、`run_id/method_id`、`state_bytes_before/after` | 当前 hash 是日志字段，尚未独立重算 |
| `terminal` | `provider_id`、`provider_status`、`selected_output_label`、`chemical_correct`、`due_tick` | `state_hash_before/after`、`output_status` 统一名、`valid_return`、`unknown/refusal` 字段 | terminal 是 evaluator-side 事件；必须和 policy replay 隔离 |
| `feedback` | `reward`、`decision_tick`、`due_tick`、`release_tick`、`version_before/after`、`state_hash_before/after`、`update_ns` | `selected_provider`、`feedback_status`、`released_feedback_ids`、`state_bytes_before/after` | 当前 hash 没有被 verifier 使用 |
| `stream summary` | `n_tasks`、`n_correct`、`n_feedback`、p50/p95、`max_state_bytes` | 逐事件状态和 unknown/refusal 分项 | `max_state_bytes` 不能替代逐事件 state bytes |

当前 runner 在 `run_provider_fresh_confirmation_v1.py`：

- 第 244–260 行 `policy_state(policy)` 只保存版本、权重/计数/后验等有限字段；不保存 pending queue、consumed IDs、随机数生成器状态；
- 第 299–316 行 feedback 生成 state hash，但 hash 只来自 runner 自己的 `policy_state`；
- 第 318–340 行 decision 生成 state hash；决策后的 hash通常与决策前相同；
- 第 344–349 行 terminal 没有 state hash；
- 第 350 行之后的最终 drain 才释放延迟 feedback。

这里有一个实质性边界：`RandomPolicy` 和 `BayesianRouterOnlineKernel` 的 RNG 会在 decision 中推进，但 `policy_state()` 不包含 `rng.bit_generator.state`。所以当前 hash 不是完整 policy state hash，最多是“学习参数状态 hash”。如果论文或报告称它是完整状态 hash，会过度解释证据。

## 3. 当前 verifier 的 fail-open 点

### P0-A：完全没有逐事件 state hash 核验

位置：`verify_provider_fresh_confirmation_v1.py:41–105`。

当前代码读取事件并检查 decision read set、terminal label、event ID 集合，但不读取 `state_hash_before` 或 `state_hash_after`。下列 mutation 在逻辑上不会触发任何现有检查：

1. 把一条 decision 的 `state_hash_before` 改成任意 64 字符；
2. 把一条 decision 的 `state_hash_after` 改成任意 64 字符；
3. 把一条 feedback 的 before/after hash 与另一事件交换；
4. 把 Frozen 的 feedback after hash 改成不同值，或把 adaptive method 的 after hash 改成旧值。

这不是实验结果问题，而是 verifier 没有消费这些字段的问题。

### P0-B：hash 不是独立重放所得

位置：`verify_provider_fresh_confirmation_v1.py:12–16,74–101`。

verifier 直接调用被测生产实现 `run_stream`。即使补上 `state_hash` 字段比较，如果 verifier 仍从同一 `run_stream` 得到 expected hash，就只能证明生产函数复现了自己，不能排除生产 runner 在状态序列化和更新顺序上同时出错。

最小要求是 verifier 内有一个不导入 `run_provider_fresh_confirmation_v1.py` 的纯 replay 实现。它可以复用固定输入和 checkpoint 的 evaluator-side 读取，但 policy replay 的输入必须只包含：

- 当前 molecule feature；
- 当前候选 menu；
- 已经 release 的 selected-only feedback；
- 预先固定的 seed / RNG draw contract。

它不能接收 truth、全 provider prediction panel 或 terminal evaluator 对象。

### P0-C：当前 verifier 不能证明没有读取 truth / unselected output

位置：

- runner `run_provider_fresh_confirmation_v1.py:326–332` 在 runner 内计算选中 provider 的 `pred` 和 `correct`；
- verifier `verify_provider_fresh_confirmation_v1.py:31–36,64–68,79–95` 构造并传递全量 `predictions`、`truth`；
- verifier `:90–95` 将它们直接交给生产 `run_stream`。

当前实现本身没有把未选 provider 的标签传给 policy update，但接口允许 `run_stream` 或某个 policy 读取这些对象。代码审查无法把“当前实现没有读”升级成“合同保证不能读”。

应将 evaluator 与 policy replay 分成两个明确对象：

```text
policy_replay(decision_input, released_feedback) -> action, next_policy_state
terminal_evaluator(selected_provider, selected_output, reference_truth) -> diagnostic
```

terminal evaluator 的 correctness 只能在 terminal/feedback release 记录生成；它不能进入 decision replay。feedback replay 只能使用已到期、已释放的 `reward`。

### P0-D：信息边界字段不完整且不是 exact schema

位置：`verify_provider_fresh_confirmation_v1.py:51–59`。

verifier 只检查：

- `decision_reads` 是否恰好等于固定列表；
- decision payload 是否包含少数几个 forbidden key。

它不检查：

- `menu` 是否等于 config 的完整 provider roster、顺序是否一致；
- 是否存在 `unselected_output`、`full_panel`、`oracle_label` 等未列入 forbidden set 的新字段；
- `released_feedback_ids` 是否只包含当前 tick 之前实际 release 的 event；当前 fresh decision 根本没有此字段；
- terminal/feedback 是否混入 evaluator-only 字段。

P0 修复应按 event kind 使用 exact allowlist：未知字段直接失败，而不是维护越来越长的 forbidden list。新 schema 应把 `menu` 改成 `menu_before_choice`，并原生写 `released_feedback_ids`。

### P0-E：事件顺序和 stream roster 没有闭合

位置：`verify_provider_fresh_confirmation_v1.py:38–50,71–73`。

当前代码按 `stream_id` 聚合事件，按事件类型建立集合，但没有独立检查：

- decision → terminal → feedback 的顺序；
- delay 队列的 due/release 顺序；
- 同一 due tick 的 FIFO ordinal；
- terminal 是否紧跟 decision；
- summary 中是否存在重复、缺失或未被事件覆盖的 stream；
- event ID 是否重复后被 `dict` 静默覆盖（第 51 行 `by_id = {...}`）。

state hash 的“逐事件”含义必须依赖固定事件顺序，否则可以把同一批 hash 重新排列后仍通过聚合检查。

## 4. 最小可实现的 state hash 合同

### 4.1 明确 hash 的范围

建议下一版把当前模糊的 `state_hash` 拆成两个明确定义的字段：

- `learned_state_hash_before/after`：学习参数、计数、block 状态和 protocol version；
- `rng_state_hash_before/after`：若 policy 使用随机决策，包含 bit-generator state；Frozen/Best Fixed 写 `null` 或固定 `none`。

若不想新增字段，必须把 RNG state 纳入 `state_hash`；不能继续把不含 RNG 的 hash 叫“完整 policy state hash”。

在当前论文声明范围内，最小实现可以暂时把 `state_hash` 定义为下面的 canonical state：

```text
S = {
  schema: "provider_fresh_state_v1",
  method_id,
  version,
  learned_parameters,
  selected_counts / correctness_counts / availability_ledger,
  blocked_flags,
  rng_state (若有),
  pending_event_ids (按固定 ordinal 排序)
}
H(S) = SHA256(canonical_json(S))
```

`canonical_json` 必须固定：递归把 NumPy scalar 转成 Python scalar；数组使用固定 little-endian dtype；拒绝 NaN/Inf；`sort_keys=True`、固定 separators、UTF-8 编码。只 hash 参数矩阵的 shape 或 `max_state_bytes` 不够。

如果为了保持现有结果不变而不把 RNG/pending 纳入 hash，则配置必须明确 `state_hash_scope=learned_parameters_only`，并另外记录 `rng_seed`、`draw_index`、`rng_draw_sha256`。不能在同一字段上混用两种含义。

### 4.2 逐事件 transition 规则

对一条 stream 维护 `current_hash` 和 `current_version`。所有 event 必须按 raw log 的协议顺序处理：每个 tick 先 release 到期 feedback，再 decision，再 terminal；最终 drain 按 `(due_tick, ordinal)` 释放剩余 feedback。

设处理前状态为 $S_t$，哈希为 $h_t=H(S_t)$：

1. **decision**
   - `state_hash_before == h_t`；
   - replay 只读 feature/menu/released feedback；
   - 如果 learned state 不在 decision 改变，则 `state_hash_after == h_t`；若 RNG 纳入 state，则 after 是消费该次 draw 后的 hash；
   - `version_before == current_version`，`version_after` 明确写出或等于 before；
   - verifier 独立重算 scores、tie-break 和 selected provider。

2. **terminal**
   - terminal 不更新 policy；
   - 增加 `state_hash_before/after`，两者都必须等于 decision 完成后的 current hash；
   - terminal evaluator 可读取 selected provider 的 output 和 reference truth，但这些值不得进入 replay state，也不得出现在 decision payload。

3. **feedback**
   - `release_tick >= due_tick` 且 `release_tick` 是 raw event 的实际顺序；
   - `state_hash_before == current_hash`；
   - update 只使用该 event 的 selected provider、feature、`reward`/合法 failure status；
   - verifier 独立执行该方法的 update，得到 $S_{t+1}$；
   - `state_hash_after == H(S_{t+1})`，`version_after` 与更新语义一致；Frozen 必须明确 `updated=false`、hash/version 不变，而不是删除 feedback。

4. **跨 event**
   - 下一条 event 的 before hash 必须等于上一条 event 的 after hash；
   - 不能只比较每类 event 的集合；
   - stream 结束时 pending queue 必须为空，最后的 current hash/version 必须与 stream summary 的 final state 字段一致。

### 4.3 MIRA 与 baseline 的最小 replay 顺序

P0 不需要重训 provider，也不需要重新跑 Nebula。只需要用已保存的 fit prior/checkpoint、sealed feature 和 raw event 做 CPU verifier replay：

- Frozen / Best Fixed / Oracle：验证固定权重、固定 provider、无参数更新；
- Matched SGD：在 feedback release 时，用 selected feature 和 reward 做一次既定 SGD update；
- Greedy / Bayesian：用 raw feedback reward 做 Gaussian rank-one update；验证 covariance 对称化、version 和 selected arm；
- MIRA-relative / MIRA-context：只更新 selected provider 的 selected-only counters、failure streak、blocked 状态和 version；不读取 unselected arm 的 outcome；
- Random / Bayesian 的随机动作：用 config seed 和固定 RNG draw replay；若不把 RNG 放入主 hash，至少逐 decision 比对 `draw_sha256` 或 `scores`。

这里的“独立”意味着实现公式不能从生产 `run_stream` 或生产 policy object 取 state；可以另写一个只读 `fresh_state_replay_v1.py`，复用数学定义，但不能导入 runner 的 `run_stream`。

## 5. 最小代码修改清单（不涉及主论文）

### Runner

1. `run_provider_fresh_confirmation_v1.py:334–340`
   - 保留 hash，但改成 canonical state contract；
   - 增加 `run_id/method_id/episode_id`；
   - 增加 `menu_before_choice`、`selected_provider`、`released_feedback_ids`、`version_before/after`；
   - 若 RNG 不进主 hash，增加 `rng_seed`、`draw_index`、`rng_draw_sha256`。

2. `run_provider_fresh_confirmation_v1.py:344–347`
   - terminal 增加 `state_hash_before/after`，且两者等于 terminal 前 current hash；
   - 将 `provider_status` 对齐为 schema 的 `output_status`；补 `valid_return`、`unknown`、`unknown_reason`、`refusal_action`，当前全为确定值时也不能省略。

3. `run_provider_fresh_confirmation_v1.py:309–316`
   - feedback 增加 `selected_provider`、`feedback_status`、`released_feedback_ids`、`decision_version`；
   - 明确 `updated`，Frozen 写 `false`；
   - 统一 event identity 字段。

4. `run_provider_fresh_confirmation_v1.py:244–260`
   - 把 canonical serializer 写进一个单独的 contract 模块，或至少固定 state scope；
   - 记录 pending queue / ordinal 和 RNG 的处理策略；
   - 不要用 `max_state_bytes` 代替逐事件状态。

### Verifier

1. `verify_provider_fresh_confirmation_v1.py:12–16`
   - 删除对生产 `run_stream`、生产 policy classes 的依赖；改为导入只读输入/模型 evaluator 和独立 `fresh_state_replay_v1`；
   - policy replay 不接 truth 或全量 predictions。

2. `verify_provider_fresh_confirmation_v1.py:38–50`
   - 对 stream roster、事件 cardinality、唯一 event ID、raw order、completion order 做 fail-closed 检查；
   - 不能使用 `setdefault` + 后续集合比较替代顺序检查。

3. `verify_provider_fresh_confirmation_v1.py:51–59`
   - exact allowlist 检查 decision 字段；`menu_before_choice == config.providers`；
   - `released_feedback_ids` 必须等于 verifier 已释放的 selected feedback 顺序；
   - 未知 key、`truth_proxy`、`oracle_label`、`unselected_output`、`full_panel` 一律失败。

4. `verify_provider_fresh_confirmation_v1.py:60–73`
   - terminal 只允许 evaluator-side selected output/correctness 字段，并核对 state hash 不变；
   - feedback 逐条核对 before/current/after hash、version、provider/event identity、due/release；
   - 不能只比较 event ID 集合。

5. `verify_provider_fresh_confirmation_v1.py:74–101`
   - 替换为独立 replay：从初始 fit state 和 raw released feedback 重算每次 state；
   - evaluator output/truth 只用于 terminal correctness validation，绝不输入 action replay。

## 6. 必须加入的 mutation / boundary tests

以下都是 CPU、本地、可在小 smoke receipt 上完成，不需要新的科学结果：

| 测试 | 变异 | 期望 |
|---|---|---|
| H1 | 第一条 decision `state_hash_before` 改值 | verifier fail |
| H2 | 第一条 decision `state_hash_after` 改值 | verifier fail |
| H3 | 一条 terminal hash 改值 | verifier fail |
| H4 | 一条 feedback before/after hash 改值 | verifier fail |
| H5 | feedback 提前到 due tick 之前 | verifier fail；后续 state 不被提前更新 |
| H6 | 交换同 due tick 的两个 feedback | verifier fail，除非 ordinal 也一致且完整重排仍满足合同 |
| H7 | 在 decision 增加 `truth_proxy`、`oracle_label`、`unselected_output` | verifier fail |
| H8 | 将 `menu_before_choice` 改成缺 provider/乱序 menu | verifier fail |
| H9 | feedback reward 改值 | 当前 feedback 后 hash、版本和后续 action 必须 fail |
| H10 | 只改未选 provider 的 evaluator output | action replay、decision hash、后续 state 必须完全不变；terminal evaluator 的独立检查可在相应 selected provider 被改时失败 |
| H11 | 删除 terminal state hash 或 decision released-feedback list | verifier fail |
| H12 | 复制/删除/跨 stream 搬移一个 event | verifier fail |
| H13 | Random/Bayesian 重放 seed 或 draw index | score/draw hash 不一致时 fail；不能把 RNG 漏出当作 state hash 已验证 |

其中 H10 是信息隔离的关键阳性/阴性对照：未选输出的改变不得影响 policy replay；选中输出或 reference truth 的改变只能影响 evaluator-side terminal check，不得改变 decision-before-release 的状态。

## 7. 证据边界与是否需要重跑

### 现在可以确认的

- fresh receipt 已经在 decision/feedback 写了 hash，并且 selected-only、delay、feature hash、provider output 和 correctness 有原始事件证据。
- `run_stream` 当前在实现层把 reward 更新放到 release 之后；但 v1 verifier 没有独立重放这一事实。
- 当前自然/stress receipts 没有 unknown/refusal/runtime failure 的真实样本，不能用本审查补出这些结果。

### 现在不能确认的

- 不能确认每个 logged state hash 对应正确的 policy state；
- 不能确认 hash 包含 RNG、pending queue 或全部状态；
- 不能确认 verifier 复现的是方法数学而不是生产 runner 自身；
- 不能仅靠当前 `verification.json=verified` 声称“没有读取 truth 或 unselected outputs”。

### 推荐决策

1. 先补 runner 字段和独立 replay/verifier；在 4–8 条小 smoke streams 上通过 H1–H13。
2. 然后只重跑 verifier/CPU receipt，或重新生成统一 raw receipt；不重训四个 provider、不重跑自然/stress 科学批次，不申请 Nebula。
3. 旧 fresh receipts 保留为历史 `state-hash-present-but-not-independent-verified` 证据；不能覆盖或改写成新 schema。
4. 只有新 receipt 通过独立 state replay、exact information boundary 和 mutation tests 后，才可把 state hash 作为论文中的可审计实现证据。它仍然不单独证明效果提升。
