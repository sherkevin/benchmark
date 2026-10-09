# Matched effect matrix v3（五折 seed0 CPU gate）

状态：预登记草案，2026-10-04；不覆盖 v1/v2 收据，不打开 confirmation，不提交 Nebula。

## 目的

v2 在 fold0 的 145 条 evaluation 行上观察到 delay 0/1 时 MIRA-Relay 相对两个匹配对照各净多 1 条正确选择，delay 4 持平。这个信号很小，不能作为论文效果。v3 只检验它是否跨 scaffold-disjoint outer folds 重复，并定位 rescued/harmed 与 family transfer 的关系。

## 固定协议

- 输入：`fingerprint_diagnostic_v1/inputs.json`，SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 五个 outer fold、seed0；evaluation 行按输入文件固定顺序，合计 722 行。
- 每折只使用该折 fit/validation 得到的 frozen BT+CLS 表示、direct prior 和 MIRA fit candidate；evaluation truth 和未选 provider 输出只由回放 judge 使用，不能进入状态。
- 方法：`probe_mira_relay`、`probe_independent_residual`、`probe_no_update`。三者共享 menu、特征、初始化、probe seed、epsilon、hazard、fallback 和反馈时钟；只改变 residual 是否更新及是否在 family 层共享。
- 延迟：`D in {0,1,4}`，release tick 为 `decision_tick + 1 + D`。相同 fold×seed 的延迟复用同一 PRNG 派生值；延迟不是独立样本。
- 每个方法×延迟×fold 记录 decision、terminal、feedback 三类事件，并保存 selected provider、menu、state hash、feedback provider/reward、runtime validity、chemical correctness、update flag、family/provider state delta。

- replay 使用已独立 seal/verify 的 fit-state，通过 `load_sealed` 做 manifest/array/status 快速复核；不会在每条 stream 重复拟合 Hessian。若 seal/verify 失败，整个 fold receipt 阻塞。
- v3 事件合同额外保存 `released_feedback_ids`、`release_ordinal`、`pending_feedback_ids_before/after`、`pending_feedback_ids_before/after_release` 和 `pending_feedback_ids_before/after_schedule`；verifier 按原始 JSONL 顺序重建 queue，拒绝缺字段、乱序、重复释放或未清空 pending 的 receipt。

## fit-state 状态政策

fold0/1/3/4 使用独立验证的 status0 trust-exact candidate。fold2 的原始 receipt 是 SciPy `status=2`, `success=false`，但有限、Hessian 对称且 `gradient_inf <= 1e-6`；只有显式 `status0_or_numerically_stationary_status2` policy 才能纳入，receipt 中原始 status 不得改写为 success。若该显式 state contract 未在开跑前通过独立 verifier，v3 只运行 0/1/3/4 并把 fold2 标为 BLOCKED，不能事后选择性排除。

## 主读出

对每个 fold、delay 和方法报告：

1. selected `system_success` accuracy，另列 runtime-valid rate 和 `chemical_correct | valid_return`；
2. MIRA 相对 `ProbeIndependentResidual` 与 `ProbeNoUpdate` 的 paired rescued、harmed、net delta；
3. family-transfer events、provider-local updates、selected-only timing 与 unselected-state invariance；
4. scaffold-cluster bootstrap 95% CI（fold 内 paired molecule；总体同时报告 fold-level均值/中位数），不把三种 delay 当独立重复；
5. action change 后的错误类型与分子 ID 只作误差分析，不作为新增样本。

## 继续与停止

- 继续进入 seed1 或 A800 前置条件：五折全部完成且独立 verifier 通过；主比较的 pooled paired 点差方向不反复翻转；runtime-valid rate 无额外退化；family-transfer 与 action change 的事件绑定成立。
- 不进入 A800/confirmation：任一硬性 receipt/公平性检查失败；fold2 status policy 未封存；paired CI 明显跨 0 且没有稳定方向；MIRA 与 independent/no-update 都无可重复的正向 paired signal；或正信号只来自单折/单个分子。
- 负结果保留，不改 epsilon、kappa、delay、provider menu、fold、seed 或指标追分。若 v3 失败，下一步做表示/共享结构诊断，而不是直接扩大 GPU 规模。

## 与论文主张的边界

v3 是 development generalization gate；实现合同与独立核验入口分别为 `src/probe_headless/run_mira_matched_effect_matrix_v3.py` 和 `src/probe_headless/verify_mira_matched_effect_matrix_v3.py`。即使通过，也只能说明在冻结 BBB provider pool、固定反馈合同和这些 fold/seed 上存在可复核的在线适应信号；不能单独证明端点迁移、model-fresh 泛化、自然运行时漂移或 confirmation 效果。
