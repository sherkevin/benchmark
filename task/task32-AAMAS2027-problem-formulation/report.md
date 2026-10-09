# task32 report — Problem Formulation Closure

> 状态：⚡️进行中

## 当前输入

- Task30 已关闭 nearest-baseline families：ACTS、RFF-UCB、P-LinUCB/P-TS、ParetoBandit-style。
- Task31 已冻结 benchmark、incumbent、split、feedback semantics、baseline 与 metric。
- 当前任务只做问题定义与 novelty boundary，不做 Ours algorithm。

## 开始结论

当前 setting 的关键不是“新 arm”三个字，而是：

- mature frozen incumbent；
- arriving zero-history challenger；
- passive context stream；
- selected-action-only feedback；
- positive advantage region sparse；
- deployment value由 relative improvement 与 harmful adoption共同决定。

下一步：完成邻近 formulation review，并据此收紧数学定义。

## 邻近领域 closure

Task32 已完成对 contextual pure exploration、treatment-effect policy learning、selective prediction、active level-set estimation、conservative policy improvement、apple tasting、arriving-arm bandits、risk-sensitive abstention、dynamic LLM routing 的近邻审查。

关键更正：不能把 region identification、relative advantage、高 precision selective adoption、new arm arrival、baseline fallback 中的任一项单独写成 novelty。

尤其 Liaw & Plaut (AISTATS 2026) 的 two-action risk-sensitive abstention 与本问题非常近：若 incumbent value 已知，把 challenger reward 减去 incumbent mean 后，incumbent 等价于 residual reward 0 的 abstain action。

## Train-only hypothesis tests

### H1 relative-residual transfer — FAIL

src/probe_headless/t32_residual_transfer_diagnostic.py / data/tool_results/divergence/t32_residual_transfer_trainonly.json。

relative-shared representation 在 LLMRouterBench / ASlib / BBB 的 4/8/16/32 few-shot budgets 下均未形成稳定优势，IOU 全负；budget=32 时三套分别 -0.01058 / -0.00213 / -0.00144，均弱于对应 absolute-shared。

### H2 historical context receptivity — FAIL as universal assumption

src/probe_headless/t32_context_receptivity_diagnostic.py / data/tool_results/divergence/t32_context_receptivity_trainonly.json。

ASlib 有 binary precision lift，但 probe residual value 仍显著为负；LLM/BBB 无稳定小预算 lift。说明 probability-of-positive 不足，必须 magnitude-aware。

## 当前结论与阻塞

Task32 已把问题边界收紧为：**estimated context-dependent baseline 下的 zero-history challenger selective onboarding**。但这个边界本身仍不能直接宣称新，因为 close-neighbor risk-sensitive abstention 尚未做正式 applicability/fidelity audit。

因此本任务不直接进入 Ours。按 Task31 freeze 规则，新建 amendment task 去关闭该 comparator；冻结协议本身不被就地修改。

## task33 close-neighbor amendment result

Risk-sensitive contextual abstention has now been audited rather than left as an untested related-work objection. The official AISTATS 2026 MainAlgorithm was imported directly from audited HEAD 69994acc6679bdc368a26ed9d2612aac67db4fee and replayed under the frozen three-benchmark protocol using only challenger reward minus frozen incumbent prediction as commit feedback.

Result: the method is noncompetitive in the frozen learned-context geometry. For all L>0 sensitivity points it effectively commits everywhere; mean IOU is -0.21195 on LLMRouterBench, -0.46715 on ASlib, and -0.03065 on BBB. L=0 remains negative. The cause is assumption mismatch / high-dimensional bin sparsity and confidence correction, not a hidden counterfactual leak.

Task33 decision: related work + appendix diagnostic, not mandatory main table. This closes the newly discovered comparator without changing Task31 v1.

The remaining problem is therefore not the generic two-action abstention formulation. We still need a validated structural assumption that works in learned/high-dimensional context spaces with estimated baseline value and sparse challenger advantage.

## 2026-09-22 — 隔离修复完成；上文 H1/H2 判定更正

本轮代码审计发现：r1 的历史 residual/receptivity 中，实际 challenger j 虽被直接删除，但历史 pseudo-k 的 incumbent 仍可使用 j。两个旧 JSON 保留，不再作为 H1/H2 有效证据。缺陷及修复前登记见 `history_isolation_audit.md`，四个原文件 seal 在 `r1_artifact_snapshot.json`。

真实 j / pseudo-k 双重排除的 r2 已全量执行并验证。三套数据 58 个 challenger、4640 条 few-shot method/budget/seed 结果；58 个 heldout/evaluation-label poisoning 检查通过。串行版独立执行两次逐字节相同，验签见 `data/tool_results/divergence/t32_history_isolation_verification.json`。完整数字仅在 `history_isolation_results.md` 维护。

上文 H1 “FAIL”更正为：指定 rank-8 线性 residual-transfer 构造未得到稳定支持，而不是整个 H1/方法族已被证伪。上文 H2 “FAIL as universal assumption”不能理解成所有局部结果皆负：BBB top-4/top-8 probe IOU 为正，必须保留；ASlib binary enrichment 与负增益并存。

还关闭了两个解释错误：realized matrix advantage 不等于 stochastic conditional-mean advantage；正的 hindsight oracle headroom 不自动证明可学习。推导和反例见 `estimand_audit.md`，相关 research 文档已追加更正，不改变 Task31 主指标。

下游 Task34 使用旧 pack 的辅助校准中有同类间接泄漏，已独立修复并重跑；代码 `src/probe_headless/t34_meta_calibrated_gate_isolated.py`，验签 `data/tool_results/divergence/t34_isolated_calibration_verification.json`。Task32/34 共 15 条定向测试通过；不宣称全仓测试通过。

### 当前可交接结论

历史隔离修复子任务完成；问题定义任务仍进行中。未验证可支撑 Ours 的通用结构假设，未冻结 Ours，未换 benchmark/metric/incumbent。下一步需区分可学习的相对均值信号、baseline 估计偏差和少量反馈成本，避免把启发式失败泛化成理论不可能。
