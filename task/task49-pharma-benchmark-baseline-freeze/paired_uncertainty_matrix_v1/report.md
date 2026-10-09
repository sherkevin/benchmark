# Paired uncertainty matrix v1

日期：2026-10-06  
状态：**verified aggregation；只收集已预计算区间；不重新抽样；不读取 confirmation**

## 目的

把已有 common-stratum、controlled family-outage 和 post-call utility 的 paired/bootstrap 区间集中到一个机器表，统一记录 estimand、单位、分母和 bootstrap seed。没有区间的收据保持缺失，不用 pooled standard error 补齐。

机器结果：[`result.json`](result.json)；生成器：[`build_paired_uncertainty_matrix_v1.py`](../../../src/probe_headless/build_paired_uncertainty_matrix_v1.py)。

## 已收集的区间

- common-stratum：Frozen/Matched/Greedy/Bayesian/MIRA-relative/FamilyGuard 的 molecule-paired、scaffold-cluster bootstrap；722 molecules、382 scaffolds、10,000 bootstrap；
- controlled family-outage：persistent release-after 与 target-family exposure 的共同支持集区间，单位明确为百分点；
- post-call utility：refusal 与 escalation 的 utility difference、break-even 参数，5,000 bootstrap。

几个当前最关键的区间：

| scope / comparison | estimate | 95% interval | 解释 |
|---|---:|---:|---|
| common Matched − Frozen | −0.9457 pp | [−2.2906,+0.2189] pp | strict online control 未稳定超过 Frozen |
| common MIRA-relative − Frozen | +0.1149 pp | [−0.3728,+0.8063] pp | no-event 差异在 ROPE 内 |
| common FamilyGuard − Frozen | +0.5977 pp | [−0.6204,+1.5988] pp | 普通任务 superiority 未确认 |
| family-outage FamilyGuard recovery | +7.60/+12.24/+22.47 pp | 已有逐 delay 区间 | 受控 persistent stress，不是自然 drift |
| post-call refusal − accept utility | +0.293 pp | [−1.456,+2.218] pp | extension，不能回填 pre-call selector |

## 统计边界

- common-stratum 和 family-outage 是不同 estimand，不能合并 bootstrap；
- family-outage 只报告共同支持集，另有 unexposed/ITT 口径，不能把 exposed conditional 当全体；
- post-call utility 与 online routing 的单位和 action space 不同，单独保留；
- Random 没有 paired CI，结果仍只作 E1 下界；
- 该矩阵不重新计算 CI，所以不会掩盖源 receipt 的错误或缺失。

## 对 Gate C 的作用

这项工作补上了 C3/C4 要求中的 paired uncertainty 入口，并把当前负结果和受控 stress 的区间放在可定位的机器表里。它没有使所有 baseline 都有同一 CI、cost/state/unknown/refusal 字段，也没有补齐 Random CI 或 E6 confirmation。因此 Gate C 仍需重审但不能直接判通过；下一步是把该矩阵与 `e5_unified_accounting_v1` 和 `role_qualified_main_table_v1` 绑定，形成一份最终 E1–E5 coverage table。
