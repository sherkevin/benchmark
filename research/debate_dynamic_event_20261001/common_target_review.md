# Common Frozen family-outage 方案审查

日期：2026-10-01  
审查对象：`family_outage_v1_preregistration.md`、`run_mira_family_outage_replay_v1.py`、目标 manifest 生成与核验脚本，以及已有 action-supported v2 receipt。  
审查角色：方法审查；本文件不改变已生效的方法、benchmark 或 claim。

## 1. 审查结论

方案可以作为一项低成本的 development stress replay，但它的可识别对象必须写成：

> 固定菜单中，方法在事件 tick 选择了 Frozen incumbent 所属 family 后，收到 selected-only delayed failure feedback 时的恢复行为。

它不是无条件发生的真实共同 outage。事件是否“激活”由方法在事件 tick 的 action 决定；未选择目标 family 的 stream 不注入事件。这个 activation gate 保留了 selected-only 信息边界，却使事件分配依赖策略动作。因此：

- 全部 30 个坐标的 `event - no-event` 是 activation-gated ITT 描述量；
- 共同支持集 `S` 上的差值是同一 family incident 下的机制对照；
- exposed 条件差值不能称随机试验的 ATE，也不能直接称 provider-level superiority。

当前实现足以继续修正后跑一次 CPU 全矩阵，但在独立的 cross-run verifier 锁死前不应解释 full run 的效果。

## 2. 依据的已有证据

### 2.1 provider-level common target 不可用

已有 action-supported target manifest 的 30 个 `(fold, seed, delay)` 坐标中，四个方法没有一个坐标选择同一个 provider。Frozen provider target 对 matched SGD 的 event-tick 支持为 `0/30`。因此直接把 Frozen provider failure 施加给四个方法，无法形成公平的共同 provider intervention。

### 2.2 family-level support 可用但不是完美共同支持

以 Frozen no-event event-tick incumbent 的 family 为 `F_c`，已有 no-event action 计算得到：

| method | exposed 坐标数（30 个） |
|---|---:|
| Frozen | 30 |
| streak-only | 30 |
| relative-reliability | 30 |
| matched SGD | 23 |

四个方法同时 exposed 的共同支持集为 `|S|=23/30`，按 fold 为 `5,5,6,3,4`，全部属于 `b3clf`。这个集合只由事件前 action 生成，不能根据事件后的结果重选。

### 2.3 已有 smoke 的边界

`family_outage_v1_smoke_v2` 只含 Frozen 和 relative-reliability，fold0/seed0/delay1；该坐标两方法均 exposed，不能测试 unexposed 分支。另有 `family_outage_v1_unexposed_smoke` 的 matched SGD fold0/seed0/delay4 两个 scenario，覆盖了 unexposed 路径，但它们尚未由独立 cross-run verifier 与对应 no-event stream 做语义级全流比较。两份 smoke 不能合并成“验证通过”，除非把它们的覆盖范围和独立核验结果一起记录。

## 3. 必须保留的设计

### 3.1 target 选择

每个 coordinate 只使用 `no_event:frozen` 在 event tick 的 provider action，经过冻结的 family registry 映射得到 `F_c`。方法自己的 no-event event-tick action 只用于记录 expected action 和 `exposed`，不参与选择 `F_c`。manifest 必须绑定 input、control events、family registry、source commit 和 verifier receipt 的 hash。

### 3.2 activation-gated family incident

对 method `m`：

- `exposed=1` 当且仅当事件 tick 的 selected provider family 等于 `F_c`；
- transient 只让该 event-tick selected provider 返回一次 runtime failure；
- persistent 只在已激活 stream 中，让 event tick 之后被选中的 `F_c` provider 返回 runtime failure；
- `exposed=0` 时不改变任何 provider 的输出，不释放该事件，也不允许在后续第一次选到 `F_c` 时补激活。

最后一点使协议成为 activation-gated stress，而不是自然环境 outage。真实共同 outage 的替代协议会在事件 tick 后改变未选 provider 的潜在输出，并在首次选中时才释放反馈，release time 因方法而变；那是另一个 estimand，不能与当前协议混称。

### 3.3 paired estimand

每个 method、scenario 和 delay 使用同一 coordinate 的 no-event stream 作为 counterfactual：

- ITT：全部 30 个坐标的 release-after correctness 差值；未 exposed 坐标的 event contribution 预期为零，但不是要求它“没有自然 failure”；
- common-support mechanism contrast：只在运行前冻结的 `S=23` 上比较四个方法；
- 额外报告 exposure、blind window、injected terminal loss、leave-family latency、failure/unknown、selection/update p50/p95 和 state bytes。

未 exposed stream 的正确要求是：与它的 no-event control 在语义字段上完全一致，保留任何本来会发生的自然/cache failure。不能把“未注入事件”解释为“没有 failure”。

## 4. 改变科学结论前必须修复的事项

### P0：独立验证 unexposed 与 exposed 的整条配对流

现有 runner 只在 event tick 检查 action provider，不能证明：

1. event/control 在 event tick 前的 entity、provider、version、state hash、released feedback identity 和 menu 相同；
2. exposed stream 的 event terminal 是唯一注入 failure，release tick 为 `event_tick+1+delay`，feedback exactly once；
3. unexposed stream 在整个后缀都与 no-event control 语义相同。

必须有一个独立的 read-only cross-run verifier。它应 canonicalize event ID、run ID、timestamp 和 `decision_ns`，然后比较上述语义字段；不能直接逐字节比较 raw JSON。任何不一致都使该 stream 或整批 invalid。没有这一步，full receipt 只能说明 runner 完成，不能说明 activation gate 成立。

### P0：校验 event entity identity

runner 读取了 `event_entity_id`，但运行前没有强制检查当前 evaluation order 的 `inputs.rows[indices[event_tick]].entity_id` 等于 manifest entry。必须 fail closed；否则 provider action 偶然相同也可能把错误 molecule 归到事件。

### P1：修正 smoke 的覆盖声明

最小 smoke 可以仍保持四条 stream，但要选择同时含 exposed 和 unexposed 的 coordinate，例如 fold0/seed0/delay4 的 `relative_reliability + matched_sgd × {transient,persistent}`。如果保留现有两份 smoke，必须把它们视为合计 6 条 stream，并用同一个独立 verifier 分别核验；不能只凭 summary 中相同的 accuracy 判断 unexposed 正确。

### P1：明确 claims 的名称

报告中使用“common family outage”时，必须同时注明“activation-gated selected-only controlled incident”。不能写成自然 outage、environment nonstationarity、provider-level replacement 或对所有 provider family 的泛化。`S` 上的比较是共同 family incident 的机制对照，不是对方法效果的无条件因果估计。

### P2：family registry 完整性

独立核验应拒绝重复 provider id、空 family、未覆盖 menu provider 或 registry 中多余 provider。现有 `dict` 构造会静默覆盖重复 id；这虽不是当前 receipt 已发现的错误，但会改变 target family 和 exposure，必须 fail closed。

## 5. 不需要增加的内容

- 不需要重新训练任何 selector 或 provider；所有 priors、feature snapshots、fold、seed、delay 和 menu 沿用已封存 receipt。
- 不需要申请 Nebula/A800；CPU replay 足以检验协议和动态机制。
- 不需要把 unexposed stream 从 ITT 中删除，也不需要构造 off-policy failure。
- 不需要把 family outage 当作自然 drift；当前 `dynamic_scope=feedback-adaptive` 的范围已经足够清楚。

## 6. 跑不跑的决定测试

满足以下条件后，跑一次 240-stream CPU receipt 是有价值的：

1. cross-run verifier 能验证 exposed 与 unexposed 两条路径；
2. `S` 在运行前已封存，仍至少 `20/30` 且五个 fold 均有支持；
3. family registry、target manifest、control receipt 和 source commit 全部 hash 绑定；
4. full receipt 的主表同时给出 ITT、`S`、exposure、failure 和成本，而不是只给 accuracy。

若任一条件不满足，先停止跑 full matrix；现有 method-conditioned v2 结果保留为 within-method recovery diagnostic，family outage 只保留协议审查结果。

## 7. 中间讨论记录

1. 首先考虑 Frozen provider 作为共同 target，但已有 receipt 证明 matched 在 `0/30` 坐标选择同一 provider，无法公平比较。
2. 再考虑按 provider mode 选共同 provider；mode 对 matched 的支持仍只有 `1/30`，排除该方案。
3. 改为 Frozen incumbent 的 family，四方法共同 exposed 支持增至 `23/30`，因此保留 family-level controlled stress。
4. 需要区分“真实共同 outage”和当前方案的 activation gate：为了保留 selected-only 与固定 release clock，未 exposed stream 必须不注入事件；这改变了 estimand，必须在题目、方法和结果边界里明说。
5. smoke 初始只覆盖 exposed 路径；补充 matched unexposed smoke 是必要但还不充分的工程证据，最终仍需要独立语义 verifier。

## 8. 审查结论

**建议：修复 P0/P1 后跑一次 CPU family-outage receipt；不提交 A800，不打开 confirmation。** 该实验值得做的原因是它能在共同 family pressure 下检验 MIRA 是否只在 family 内换 provider，暴露 provider-family correlation 的真实弱点；它不能把当前结果升级成自然动态环境或跨 family 泛化证据。

## 9. 复审更新

在本文件初审后，以下工程项已在提交中补上：

- `8f88d045` 将当前 evaluation 的 `event_entity_id` 与 manifest 绑定；
- `9ed62edb` 在 receipt 中记录 deterministic `exploration=none` 和 `action_propensity=1.0`；
- `0e0cbe85`、`d23ac5be` 将 `S=23/30` common-support 坐标封存并由 verifier 重算；
- `d93b5a5c` 对 family registry 的重复 provider、空 family 做 fail-closed 检查。

smoke 已新增 `relative_reliability + matched_sgd` 的 fold0/seed0/delay4 两 scenario，覆盖 exposed 与 unexposed；这解决了原 smoke roster 没有 unexposed 的覆盖缺口。上述 smoke 仍没有替代 P0 的 full cross-run semantic verifier：它们尚未独立证明 unexposed 后缀与 no-event control 的逐语义字段一致，也尚未独立重算 exposed 的 failure/release exactly-once。因此当前仍只建议在 verifier 完成后运行 240 条 full CPU receipt。

### 9.1 对新 cross-run verifier 草稿的复核

`src/probe_headless/verify_mira_family_outage_replay_v1.py` 已开始补齐 P0，但 full run 前还必须处理以下问题：

1. 不能只检查 manifest self-hash 和 `verification.status`。每个 event record 的 target family、expected action provider、exposed 标志和 event entity 都要重新对照 manifest entry；`verification` receipt 的 manifest/input/control/registry hashes 也要与 run config 一一相等。event record 重复 key 不能被 dict comprehension 静默覆盖。
2. exposed transient 不能把 event tick 之后所有 `failure` 都视为额外注入。底层 provider cache 本来可能产生自然 failure；应比较 event 与 control，要求新增差异只来自声明的 target-family failure。
3. persistent 需要逐个 target-family selected decision 检查唯一 failure feedback 和 `release_tick=decision_tick+1+delay`；target family 之外的 terminal/feedback 要与 control 语义一致。只检查 event tick 不能证明 exactly-once。
4. `canonical_ids` 的分隔符应为 `":"`；当前草稿写成了冒号加换行，虽可能在两边同时使用时碰巧相等，但会让 canonical identity 不规范。
