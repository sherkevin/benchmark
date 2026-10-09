# 跨折 fit-state v3：development 数值状态合同

日期：2026-10-04。范围：5 folds × seed 0 的 CPU matched matrix 初始化；不改变 v2、主线决议或论文 claim。

## 输入、输出与 API

`seal(output=Path, candidate_dir=Path, inputs_path=..., checkpoint_root=..., registry_path=..., status_policy="status0_only")`：读取已有 candidate receipt 的 fold/seed，重建该折 fit problem；保存 `fit_state.npz`、`config.json`、`manifest.json`、原始 candidate 副本和本次 `verification.json`。已存在 output 拒绝覆盖。不会调用 residual optimizer。

`verify(state_dir=Path, inputs_path=..., checkpoint_root=..., registry_path=..., status_policy="status0_only", expected_fold=None, expected_seed=0)`：不信任已有 verification 标志，重新核对候选/输入/源码/数组/数值门，返回验证结果。不会修改输入包或源 candidate。

`load(...)`：接受与 verify 相同参数，每次先 verify；返回 `arrays`、`providers`、`families`、`family_by_provider`、`direct_prior`、`fit_kwargs`、`config`、`verification`。`fit_kwargs` 可直接注入 MIRARelayPolicy；IndependentResidual 继续使用 from_family_fit。`load_sealed(...)` 只用于 replay：先前的 seal/verify 已独立重算 objective/Hessian；回放阶段只重查 manifest、源码/候选/数组 hash、status policy 和 canonical state hash，不在每条 stream 重复拟合。

## 两种明确策略

默认 `status0_only` 仅接受原始 `status="accepted", scipy_status=0, scipy_success=true`。

显式 `status0_or_numerically_stationary_status2` 另外接受原始 `status="numerically_stationary_status2", scipy_status=2, scipy_success=false`。fold 2 的原始失败码及消息完整保留；不得改写为 optimizer success。

两个分支都要求重算 objective、gradient、Hessian 全部有限，gradient infinity norm ≤ $10^{-6}$、Hessian 最大不对称项 ≤ $10^{-10}$，并与候选回执匹配。默认 verifier 不会因为包内写着允许 status2 而自行升级策略。

## 重建及完整性

已有 candidate verifier 使用 optimizer 模块的 FOLD/SEED 全局变量；v3 在互斥的 context 中按 receipt 设置，并在成功或失败后恢复，避免跨折污染。使用临时 candidate 副本承接旧 verifier 写 verification 的行为，原始 receipt 不改。

fit problem 使用现有固定 encoder、fit/validation split 和 direct-head 重建流程；这是已有 head 的确定性重建与 hash 核对，未新增 residual 拟合、搜索、evaluation 训练或 replay。runtime counts 只从当前 fold.fit 的 output_statuses 读取；ok 是 valid，error/failure 是 failure，不能把错误答案计作运行失败。

archive 包含 theta、direct_prior、family mean/precision/variance、provider bias/precision/variance、provider_family、fit valid/failure counts。浮点固定 little-endian float64，计数/索引固定 little-endian int64。参数必须匹配 candidate；precision 必须匹配独立重算的 Hessian diagonal，variance 为其倒数。

manifest 绑定 config、archive、candidate receipt/npz；config 绑定当前实现源码、输入/encoder checkpoint/registry、fit feature/label/index/direct-head metadata、status policy、原始 solver 状态与重算数值。manifest 是完整性收据，不是防恶意同时篡改的数字签名；verify 仍从源输入重算。

## 继续门

任一 fold/seed、input/source/checkpoint/meta hash、status、array shape/dtype、counts、objective/gradient/Hessian、precision 或参数绑定不一致均拒绝。所有源码/数值状态校验通过只说明可复用初始化正确；不能代替 online 效果、family sharing 或 benchmark 认可度证明。下一步是固定 matched matrix，不据此修改超参。
