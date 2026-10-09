# Gate C unified E1--E6 coverage matrix v1

日期：2026-10-01  
状态：只读审计；用于明确缺口，不打开 confirmation。

## 这次审计做了什么

按照生效的 Benchmark 与 Baseline 评价标准 v005，把当前已经独立验证的三类收据放在同一张机器表中：

1. common-stratum full receipt：普通固定 BBB 菜单、六个方法、delay `{0,1,4}`；
2. family-outage full receipt：预登记的 persistent/transient family stress；
3. unknown/refusal semantic smoke：只验证事件记账和 fail-closed recovery。

本次重建还修正了两个证据口径：common-stratum family macro 改用运行配置声明的
`external_qualification.json` 做只读重聚合；baseline cost table 的 Replay-SGD p95 只
统计 Replay 子集，而不是把同一源回执中的 Frozen/Online 事件混入。两项修正都不重跑策略，
也不改变 Gate 的保守判定。

审计器只读取收据和已生成的机器结果。任何一个 scope 没有的指标都写成
`MISSING` 或 `NOT_APPLICABLE`，没有跨实验补数字。机器结果：
`matrix.json`。

## 覆盖表

| 收据 | scope | E1 | E2 | E3 | E4 | E5 | E6 |
|---|---|---|---|---|---|---|---|
| common-stratum full | feedback-adaptive fixed BBB menu | PARTIAL | PARTIAL | PARTIAL | PARTIAL | PARTIAL | MISSING |
| family-outage full | controlled persistent/transient family stress | N/A | N/A | PASS | PASS | PARTIAL | MISSING |
| semantic smoke v2 | accounting/recovery only | N/A | N/A | N/A | N/A | PARTIAL | MISSING |
| reward-ridge Gaussian full | strict Gaussian reward-scale selected-only stratum | N/A | PARTIAL | N/A | PARTIAL | PASS | MISSING |

### 分母结果

common-stratum 的六个方法各有 4,332 attempts/calls、4,332 terminal 和 4,332
feedback。`unknown=0`、`refused=0` 是这批输入没有这两类事件，不是 refusal 能力的效果结果。
runtime failure 仍在 attempts 分母内：Frozen 33、Matched 35、Greedy 23、Bayesian 25、
MIRA-relative 33、FamilyGuard 10。逐方法的 scored/correct/wrong 见
`common_stratum_semantic_denominator_v1/report.md`。

### 为什么不是全 PASS

- **E1**：任务有效性所需的 Best Fixed、Random、Oracle 与普通主回放不在一张统一收据中；
  现有结果可以分别审计，但不能把它们合并成一个新分数。
- **E2**：common-stratum 有六方法和配对分析，但普通主分母的区间跨 0，不能写稳定优越；
  成本账本已有逐方法记录，但 baseline fidelity、likelihood、warm-start、state、refusal
  和 unknown 合同仍未统一。
- **E3**：FamilyGuard 的受控修复和机制消融有证据，但不能外推为普通任务上的总体提升。
- **E4**：family-outage 只覆盖预登记的 feedback-adaptive controlled stress，未覆盖自然
  arrival/retire/version-shift。
- **E5**：failure、成本、恢复和语义 smoke 已有；refusal utility、固定接受率对照和五折
  verifier effect table 仍缺。
- **strict Gaussian baseline**：reward-ridge full receipt 已有 paired effect、failure、成本
  和 recovery，但它只补 Gaussian stratum，不能替代 logistic stratum 的主比较，也没有
  refusal utility 或 confirmation。
- **E6**：方法、菜单、阈值、epoch 和 baseline 尚未完成冻结，因此 confirmation 继续关闭。

## 对验收标准的动作

这张表把 C1/C2/C4 的缺口从叙述变成了可定位的 `MISSING` 项，但不改变现有评分：
Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16`，三门仍 HOLD。下一项实验前，
先补齐主 baseline 的统一 input/feature/warm-start/预算/成本合同，并单独登记 refusal
utility full table；没有这些条件，不申请新的效果型 A800，也不开 confirmation。

## 2026-10-01 post-call utility audit

新增 `postcall_utility_audit_v1` 行，范围是静态 fit-only post-call verifier 的 BBB development utility。五折、六个策略、4,332 条 policy receipt 和 4,332 条 evaluator-only outcome 均有独立恢复；拒答、最终 failure、runtime failure、调用数和 matched-rate conditional null 已分开。

该行的 E5 仍标为 `PARTIAL`：utility 和恢复链已通过，但没有 provider latency、update time、memory，也没有 unknown censoring effect 或 confirmation。E1/E2/E3/E4 不适用这条 post-call extension，不能把它接回 direct selection 主比较。矩阵机器副本：`matrix.json`，新增行的 recovery 状态为 `verified`。

这一步关闭了“只有 accepted accuracy、没有拒答/调用效用”的一个子缺口，但 Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16` 不变；C1/C2/C4 和 E6 仍 HOLD。下一步把该 utility receipt 接入统一 E5 主表，同时继续保留完整实时成本和冻结 confirmation 的缺口。

## 2026-10-02 证据指针修正记录

- `common_metrics` 现指向 `metrics_v2_external_registry.json`，其审计文件
  `metrics_v2_external_registry_audit.json` 记录了旧 registry 与新 registry 的 hash、源
  events hash 以及“只读重聚合、scientific_inputs_unchanged=true”。因此当前 family macro
  应读取 v2 数字；task success、valid-return、failure、latency、state bytes 不变。
- `gate_c_unified_matrix_v1/matrix.json` 的 C2 文案已改为“成本账本存在，但 baseline
  fidelity/likelihood/warm-start/state/refusal/unknown 合同不统一”，避免把已经补齐的成本
  问题继续写成缺成本。
- Replay-SGD 的成本 scope 修正和 Matched 的 direct-prior/历史 BT-prior 分线，分别在
  `baseline_cost_budget_v1/report.md` 与 `development_master_table_v1/report.md` 记录；这两条
  receipt 仍不能组成跨 prior 的 paired superiority 结果。
