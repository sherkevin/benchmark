# Common-stratum feedback 语义冒烟 v1

日期：2026-09-30  
状态：**协议验证通过；不是效果实验。**

## 目的

在进入 5 folds × 2 seeds × 3 delays 的 full receipt 前，先用真实 BBB 缓存核对两个最容易污染结果的边界：运行失败是否进入分母，以及 unresolved reference 是否被错误地变成数值标签。

## 真实样本

- provider：`b3clf_dtree_common`。
- 运行失败：`z001303`，缓存状态为 `error`，`CACHED_VALUE_MISSING`。
- unresolved reference：`z002970`，缓存状态为 `ok`，但真值 ledger 明确标为 `unresolved_reference`。
- 缓存来源 hash、输入 manifest hash 和事件原文保存在机器收据中。

## 核对结果

### 运行失败

- `score_status=scored`，但 selected output 没有化学正确性标签。
- `valid_return=0`、`chemical_correct=null`、`system_success=0`。
- 延迟 1 的 release 在 tick 2 产生 `reward=0`、`feedback_status=failure`。
- 因而失败进入任务分母，不会被误记为化学预测错误或被删除。

### unresolved reference

- `score_status=unscorable`、`answer_correct=null`、`tool_feedback=[]`。
- `recovered_label=None`。
- 进入 `single_call_record` 时明确拒绝，错误为 `unscorable sample stops this predeclared-cohort batch`；没有排队、没有 release、没有 numeric reward。

## 机器证据

- 运行收据：`experiments/logs/task49_common_stratum_feedback_semantics_20260930/v2/`。
- 独立 verifier：同目录 `verification.json`，状态 `verified`。
- 主脚本：`src/probe_headless/run_common_stratum_feedback_semantics_smoke_v1.py`。
- 独立复核：`src/probe_headless/verify_common_stratum_feedback_semantics_smoke_v1.py`。
- 结果范围：只证明 feedback/event contract，`effectiveness_estimate=null`，不能证明任何 selector 优于 Frozen。

## 对 full receipt 的约束

1. 所有方法必须记录失败的 `system_success=0`，保留在同一分母。
2. unresolved 样本不能进入数值效果表；当前 predeclared-cohort runner 继续采用显式 stop/censor 语义。
3. full receipt 仍需补齐 60 streams、三种 delay、paired uncertainty、调用/更新时间和成本记账；本冒烟不解除 Gate B。
