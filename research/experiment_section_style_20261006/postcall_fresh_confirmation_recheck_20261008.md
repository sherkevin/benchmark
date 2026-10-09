# Post-call verification/refusal extension recheck (corrected)

日期：2026-10-08
对象：`task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/`
状态：**secondary/post-hoc analysis；prospective untouched-confirmation gate 不通过。**

## 结论

本轮对已有留出 cohort 做了 post-call verification/refusal 分析。157 个分子与 development fit/validation rows 按 scaffold 分离，但它们在本次分析前已经出现在此前的 holdout routing 工作中，因此不能称作新的、未触碰的 confirmation 集。provider checkpoint、fit-only provider 选择和阈值在本次 replay 前已冻结，未进行 policy/provider 调参。

confidence-refuse 相对同 coverage random 的 pooled accepted-accuracy 提升为 5.88 pp，五折均为正；正确的 scaffold bootstrap 95% CI 为 **[+2.77,+9.29] pp**。相对 always-accept 的点差为 6.42 pp，CI 为 **[+3.34,+9.72] pp**。固定 $(\rho,\kappa)=(0.25,0.01)$ 时，相对 always-accept 的 utility 点改善为 +0.0338，cluster bootstrap 95% CI 为 **[+0.0088,+0.0609]**；相对 same-coverage random 的点改善为 +0.0497，CI 为 **[+0.0292,+0.0718]**。

`same-coverage random` 是 evaluator-side conditional null：它使用 confidence gate 在完整 cohort 中的 accepted count，再随机抽取相同数量的位置。因此它可用于 matched comparison，但不是可部署的 online policy，也不是随机种子稳健性实验。5 folds 共享同一 157 个分子，所以 3,140 个 action rows 不能解释为独立重复实验。

## 三份标准的证据更新

### 故事线与创新点

- post-call 风险门现在有一个可核验的 secondary observation：provider 返回后，confidence gate 可以把拒答动作与 accepted accuracy/utility 分开计量。
- 不能把这轮写成新的 prospective confirmation，也不能把 accepted accuracy 当成 pre-call selector accuracy。
- 主线仍是 direct selection + selected-only delayed feedback；verification/refusal 是能力扩展，且当前证据属于已有留出 cohort 的事后分析。

### 方法论

- 在 extension scope 中，accept/refuse、answer_correct、terminal 和 calls 分开保存，并由校验器用 sealed truth 绑定 accepted 行。
- utility 的 $(\rho,\kappa)$ 已明确，拒答错误率和调用成本使用独立分母。
- 本轮不提供 online update、escalation、自然 drift 或 endpoint generalization 的证据；不能回填 MIRA-Relay 主方法的 broader gate。

### Benchmark + Baseline

- E6 的 **prospective untouched confirmation** 在本 extension 中不通过；scaffold-disjointness 和 frozen inputs 仍是有效的次级审计条件。
- always-accept 是全覆盖锚点；always-refuse 是零覆盖边界；same-coverage random 只作为 evaluator-side conditional null，不应列为可部署 baseline。
- Gate C 的“post-call secondary analysis”子项可以记录为有证据；主 benchmark/baseline declared-scope 的 direct-selection 主表不因本轮重写。

## 证据等级限制

原始 full_v1 没有执行前 source hash 和流式配置日志，因此不能据此把 M14/H7 打成完整 4/4。当前独立校验器和统计重算能复原给定原始 receipt 的动作、truth binding 和汇总，不等于证明历史 provider inference 或进程级数据隔离。具体失败和 provenance 局限见修正报告；本次不再据此提升 extension 的完备性分数。

## 仍未关闭

1. 没有第二 provider escalation 的净收益；
2. 没有自然 provider arrival/retire/version lifecycle；
3. 四个 fresh provider 为 singleton family，不能给 family-shared credit 下结论；
4. 没有端到端 LLM agent 最终答案或拒答决策实验；
5. hERG/AMES 与完整 33-provider model-fresh confirmation 仍不在证据范围。

## 证据路径

- 执行前协议：`task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/experiment_card.md`
- 修正说明：`task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/correction_amendment_20261008.md`
- 原始报告快照：`task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/report_v1_original.md`
- 修正报告：`task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/report.md`
- 原始结果：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/result.json`
- 修正分析：`experiments/logs/task49_postcall_fresh_confirmation_20261008/analysis_audit_v3/analysis_audit_v2.json`
- 首轮 verifier：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/verification_v2.json`

## 主版处理

这次修正需要重新绑定正文并单独构建；在 `make submission`、warning gate、页数和页面 QA 通过前，不替换 `paper/aamas2027/main.pdf`。旧主版和本轮历史报告均保留。

## 增强独立核验完成

`strict_audit_v3` 已验证完整 receipt/evaluator 3,140+3,140 行、232 个原始汇总字段与输入 hash，26 个测试通过。原结果的 fit 阈值实际覆盖率与 tie-inclusive 重算一致；曾经怀疑的 tie 计数偏差在这批数据上不存在。CI 误抄及 cohort 状态错误仍由本修正报告撤回。新增统计分析保留全部 bootstrap draws；这些校验补强算术和恢复证据，不恢复 prospective E6，也不自动提高创新/agent 指标。
