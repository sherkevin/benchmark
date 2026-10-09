# MIRA capability v1 implementation card

日期：2026-10-05  
状态：目标实现规格；尚未进入效果实验。

## 目标

实现一个最多两次调用的工具证据决策器：第一次调用选择最合适的 provider；收到结果后，在 `ACCEPT / REFUSE / CALL(provider)` 中选择；真实 correctness 只在 release 后更新跨任务状态。

## 状态结构

```text
ScopeState L:
  provider_correctness_adapter[A, d + 1]
  provider_runtime_hazard[A, d + 1]
  family_registry / provider lineage
  joint_evidence_parameters (frozen unless paired audit is released)

EvidenceState E:
  called = [{provider_id, output in {0,1,⊥}, status, confidence?, source}]
  posterior_y = [P(y=0|E), P(y=1|E)]
  remaining_calls
  current_answer / refusal_loss

ClockState:
  pending terminal events
  released selected-only feedback
  decision_tick / release_tick
```

决策输入只能读取当前分子的 `x,e,menu`、`ScopeState L`、当前 `EvidenceState E` 和释放历史。未选 provider 的当前 output、当前 truth 和未来 release event 必须在 adapter 层屏蔽。

## Offline fit

1. 固定 scaffold-disjoint folds、冻结 `h=φ(x,e)`、provider menu 和 truth `y`。
2. 对每个 provider 保存 `output o∈{0,1}`、`status s∈{ok,error}`，并构造：`r_chem=1[o=y]`（仅 `s=ok`）、`r_run=1[s=ok]`；error 的 `r_chem=null`，不改写为 0。
3. 在 fit 区拟合 endpoint prior、JEV correctness head `P(r_chem=1|h,E,a)`、runtime head `P(r_run=1|h,E,a)` 和 Joint Evidence；Joint 的 latent mode 固定 `K=2`，使用 provider/family shrinkage，不能靠 evaluation 选择 mode 数量。
4. 用 validation 区做 posterior calibration；evaluation 之前冻结参数和校准器。
5. 保存 source hashes、fit/validation/evaluation molecule ids、provider lineage、label counts、参数 hash 和 calibration hash。

同一 fit panel 上至少拟合以下对照：

- Independent Evidence：去掉共享 mode；
- family-only：只允许 family 级关系；
- same-capacity pairwise conditional：隔离 latent mode 与额外参数容量；
- no-prior：不使用 endpoint prior；
- direct-property control：只预测 truth，不读取 provider evidence。

## Runtime pseudocode

```text
begin_episode(x, e, menu, L):
    h = frozen_encoder(x, e)
    E = empty_evidence()
    for a in menu:
        q0[a] = call_value(a, E, L, horizon=0)
    return CALL(argmin(q0))

on_return(E, L, result):
    append selected result to E                 # output/status only
    posterior = joint_posterior(E, L)
    stop = min(refusal_loss, 1 - max(posterior))
    for b in menu - E.called:
        q1[b] = call_cost(b) + sum_o P(o|E,b,L) * stop_loss(E + (b,o), L)
    if stop <= min(q1):
        return ACCEPT(argmax(posterior)) or REFUSE
    return CALL(argmin(q1))

on_terminal(event):
    put event in pending queue

on_release(event, L):
    update provider scope and runtime hazard with released selected feedback
    update joint relation only when event is a registered paired audit
    return new serializable L
```

`verify` 与 `complement` 不作为两个独立策略头，也没有人工 label。对 `CALL(b)` 的事后解释使用 conditional agreement、预期熵下降和 posterior change：主要确认当前答案时标记 `verification`，主要可能改变答案或降低不确定性时标记 `complementary evidence`。这两个标签不能替代 utility。

## 数值和实时性要求

- H=2 使用 exact enumeration；输出域为 3 类，K=2，单次候选评估为 $O(K|O|)$，整菜单为 $O(|A|K|O|)$。
- Scope adapter 使用冻结特征上的 local-MAP / diagonal-precision rank-one update，单个 release 为 $O(d)$。
- 每个 decision 保存 posterior、每个候选 `Q`、预算、read-set、state hash；每个 release 保存 source event id、delay、update time 和 state hash。
- 报告 selection/update p50、p95、峰值内存、序列化恢复时间；恢复后重算 action 必须与原 receipt 一致。

## CPU 验收门

1. 状态机：pending、release、乱序、重复、重启后结果一致。
2. 信息边界：注入未选 output、未来 truth 或未 release event 必须被 verifier 拒绝。
3. 观测语义：`⊥` 不被改写成 chemical 0/1；failure 和 correctness 分账。
4. 数值：Joint/Independent posterior、`Q`、`V` 与独立 NumPy 枚举一致。
5. 公平性：Frozen Joint、Independent、fixed cascade、Best Fixed 共用 encoder、fit panel、menu、budget、delay 和 calibration 合同。
6. 机制：同一 paired panel 上 Joint pair NLL/Brier/ECE；同成本 H=2 replay 上 final utility、wrong delivery、refusal、calls、runtime failure。
7. 动态：无 controlled reliability/version shift 时只报告 delayed-clock bookkeeping；只有 shift stress 中 online 优于 Frozen 才能声称 adaptation。

## 结果处理

- Joint 只提升 NLL：作为证据模型诊断，不能写成能力提升。
- Joint 提升 posterior 但不提升 matched fixed cascade utility：不能写成智能验证/互补。
- online 只改变 action 但不提升 final utility：保留为机制日志，不能当作成功。
- 任一 baseline 访问了未选 panel、evaluation truth 或不同预算：该 receipt 作废，不进入主表。
