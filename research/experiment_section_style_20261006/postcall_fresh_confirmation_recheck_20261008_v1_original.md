# Post-call verification/refusal extension recheck

日期：2026-10-08  
对象：`task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/`  
主稿：`paper/aamas2027/main.pdf`（本轮不改主版）

## 结论

这轮关闭了此前“post-call verification/refusal 只有 development evidence”的一个明确缺口：157 个 scaffold-disjoint fresh confirmation 分子、5 个 fit-only folds、4 个 provider、3,140 policy rows 均有独立恢复。confidence-refuse 相对同 coverage random 的 pooled accepted-accuracy 提升 5.88 pp，五折均胜出；scaffold bootstrap 95% CI 为 [+3.34,+9.72] pp。相对 always-accept 提升 6.42 pp，固定 $(\rho,\kappa)=(0.25,0.01)$ 的 utility 改善为 +0.0338。

这只证明一个 post-call 风险门扩展：provider 已返回概率后，可以用 fit-only 阈值拒答高风险返回。它没有证明 MIRA-Relay 的 online gain、escalation 的净收益、自然 provider drift、端到端 agent gain 或 hERG/AMES 泛化。

## 三份标准的证据更新

### 故事线与创新点

- S2/S3 的 extension evidence 增强：故事不再只有“未来可以验证/拒答”的接口描述，已有 fresh utility 结果把“调用—评估风险—拒答”链条落地。
- S11 边界仍保持：post-call 结果不能回填 pre-call selector 的信息合同；主稿不能把 accepted accuracy 写成 selector accuracy。
- 故事主线不改：论文仍以 direct selection + selected-only delayed feedback 为中心，verification/refusal 作为能力扩展单独报告。

### 方法论

- M4（标签/失败/拒答语义）：extension receipt 将 accept/refuse、answer_correct、terminal 分开，并由独立 verifier 用 sealed truth 重算；在 extension scope 可记为 4/4。
- M11（延迟/偏差/成本）：本轮没有 escalation 或 online feedback，不能给 M11 的 one-call 主方法加分；调用后拒答成本在 utility grid 中完整报告。
- M13（消融/失败边界）：confidence-refuse、same-coverage random、always-accept、always-refuse 构成最小可识别对照；action/evaluator mutation 均 fail-closed。extension scope 可记为 4/4，不能回填为 MIRA 机制消融。
- M14/H7：policy receipt 与 evaluator ledger 分离，独立脚本逐行重算 provider、threshold、action、correctness 和 terminal；full 3,140 rows verified。extension artifact gate PASS。

### Benchmark + Baseline

- E6 fresh confirmation 在 verification/refusal extension 上完成：阈值、provider 选择、random seed 和输入在 confirmation 前冻结；truth 未进入 policy receipt。
- baseline 角色闭合：always-accept 是全覆盖锚点，same-coverage random 是拒答选择 null，always-refuse 是零覆盖边界；不把这些策略排成新的 selector baseline。
- Gate C 的 extension 子项可记为通过；主 benchmark/baseline declared-scope 的 Gate A/B/C 分数仍按原主稿合同维护，不因 extension 结果改写 direct-selection 主表。

## 仍未关闭

1. confidence gate 只使用返回概率，尚未证明第二 provider escalation 的净收益；
2. 没有自然 provider arrival/retire/version lifecycle；
3. 四个 fresh provider 为 singleton family，不能给 family-shared credit 下结论；
4. 没有端到端 LLM agent 最终答案或拒答决策实验；
5. hERG/AMES 与完整 33-provider model-fresh confirmation 仍不在证据范围。

## 证据路径

- 协议：`task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/experiment_card.md`
- 报告：`task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/report.md`
- full result：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/result.json`
- utility：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/analysis.json`
- 独立核验：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/verification_v2.json`
- mutation：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/mutation_eval.log` 与 smoke mutation log。

## 主版处理

本轮不直接替换 `paper/aamas2027/main.pdf`。若后续把 extension 纳入正文，只能从新 source build 开始，保持正文 8 页/参考文献第 9 页，完成 `make submission`、warning gate、页数和页面 QA 后再按主版 promotion 规则归档旧 PDF。
