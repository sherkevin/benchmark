# MIRA-Relay contract replay v1

状态：程序与单元测试完成；真实回放尚未执行。

## 目的

这个 runner 只验证 delayed selected-only 生命周期，以及已释放反馈是否会改变后续 action。它不是效果实验，不读取 confirmation 集，也不输出方法效果 claim。

固定设置：fold 0、seed 0、evaluation 原顺序前 32 条、delay `0/1/4`，三种方法共 9 条 stream。

| method | probe | residual update | runtime ledger |
|---|---:|---:|---:|
| `probe_mira_relay` | `epsilon=0.20` | selected scored/failure update | yes |
| `probe_no_update` | `epsilon=0.20` | frozen | yes |
| `no_probe_shield` | `epsilon=0` | frozen | yes |

## 初始化

候选参数来自 `trust_candidate_crossfold_v1/fold0_seed0`，必须通过 `verification.json` 且状态为 `accepted`。程序重新检查 candidate archive、little-endian float64、theta hash、archive hash 和 direct prior hash。direct prior 从已经封存的 fit snapshot 读取并与候选 receipt 精确相等。

历史 v1 runner 使用 theta-only candidate，并在同一 fit cells 上重算 Hessian 对角线；该版本保留作
边界证据。v4 runner 改用独立封存的 `mira_relay_fit_state_v2`，显式加载 precision/variance、
fit valid/failure counts、provider/family order 和 frozen candidate artifacts。v2 state verifier
独立重算 objective/gradient/Hessian diagonal、direct prior、runtime status hash 和 canonical
state hash；回放不再临时猜测或重算可变 fit state。

## JSONL 事件

每行结构：

```json
{
  "timestamp": "UTC ISO-8601",
  "stream": "fold0_seed0_delay1_probe_mira_relay",
  "method": "probe_mira_relay",
  "fold": 0,
  "seed": 0,
  "delay": 1,
  "event": "decision | terminal | feedback",
  "payload": {
    "event_id": "...",
    "provider_id": "...",
    "version": 3,
    "candidate_menu": ["..."],
    "eligible_providers": ["..."],
    "scores": [0.0],
    "propensity": 0.2,
    "selected_output": {"status": "ok", "label": 1},
    "feedback_status": "scored",
    "due_tick": 5,
    "state_hash_before": "sha256",
    "state_hash_after": "sha256",
    "state_before": {},
    "state_after": {}
  }
}
```

`decision` 只读 molecule feature、candidate menu 和已经 release 的 selected feedback；未选 provider 的输出不进入 policy。`terminal` 保存 selected output 和 evaluator diagnostic；`feedback` 只保存 policy-visible status/update/version，同时保存 release 后 state。状态 hash 覆盖 `MIRARelayPolicy.state_snapshot()` 全部字段。

`action_changes_after_feedback` 是诊断计数：仅当一个 decision 之前确实有一个或多个 feedback release 时，比较当前 `gate_action` 与上一 decision 的 `gate_action`（probe 分支也会保留为 `probe_forced`）。它不是准确率、收益或显著性估计。
