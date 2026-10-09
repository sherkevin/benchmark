# trust-exact fit evidence 到 replay 的只读审查

日期：2026-10-03  
范围：仅审查 candidate/crossfold runner、verifier 和未来 replay 的 fit-state 边界；本次没有修改代码、没有读取 confirmation、没有运行 GPU 或 online replay。

## 已确认的证据边界

1. `trust_candidate_crossfold_v1/` 下有 fold0–4/seed0 各自的 NPZ、candidate receipt、
   config、summary 和 verification。
2. verifier 从冻结输入重新建立 fit feature、fit labels、direct offset 和 family registry，
   独立重算 objective、gradient、Hessian 对称误差和 parameter hash；不是只读 receipt 数字。
3. NPZ 目前只允许一个 `theta` key，并检查 finite、shape、archive SHA-256 和参数 SHA-256；
   archive 篡改已实际被拒绝。
4. config 已记录 fold、seed、L2、输入/encoder/registry/source hash，以及 fit feature、label、
   direct-prior 的 matrix hash；crossfold 的每一折输入 hash 与参数 hash 都单独保存。
5. NaN/non-finite 的参数、objective、gradient 或 Hessian 会 fail closed；status2 只有在
   `gradient_inf <= 1e-6`、Hessian 对称误差 `<=1e-10` 且 SciPy 原始状态满足候选规则时才接受。

## 进入 replay 前的最小修复点

### P0：生效前必须修

**P0-1. verifier 必须验证 config 身份，而不是只依赖外部 globals。** 目前 verifier 会从
调用者设置的 `base.FOLD/base.SEED` 重建 problem，但没有逐项检查 receipt 中的 `fold`、
`seed`、`l2`、objective、optimizer、maxiter 和 gtol。应要求：

- `config.fold/seed` 与 verifier 参数完全相等；
- `config.l2 == 0.01`、objective 字符串、optimizer=`trust-exact`、maxiter=100、gtol=`1e-6`；
- `config.input` 的 fit_n、validation_n、provider/family 数量和 feature_dimension 与重建值一致；
- input/checkpoint/registry、fit feature/label/direct-prior、fit-index hashes 全部重算并相等。

否则调用者可以把一份参数 receipt 绑定到错误的 fold/seed 或改写数值合同后仍得到 verified。

**P0-2. status0 接受规则应显式要求 `scipy_status == 0`。** 当前 `status_acceptance("accepted", ... )`
只检查 `scipy_success` 和数值门，没有同时检查 status 数字。最小修复是：

`accepted ⇔ scipy_status == 0 ∧ scipy_success ∧ finite ∧ grad_inf ≤ 1e-6 ∧ symmetry ≤ 1e-10`。

status2 继续只能走 `numerically_stationary_status2` 分支，不能走普通 accepted。

**P0-3. fit state 必须显式保存 diagonal precision/variance。** 当前 NPZ 只有 `theta`，而
`MIRARelayPolicy` 的构造需要 `fit_family_variance` 和 `fit_provider_bias_variance`。不能让
replay 临时猜初值或使用默认 1.0。应在 candidate receipt/NPZ 中保存并 hash 绑定：

- `family_residual[g,d]`、`provider_bias[k]`；
- `family_precision[g,d]`、`provider_bias_precision[k]`；
- `family_variance = 1 / family_precision`、`provider_bias_variance = 1 / provider_bias_precision`；
- precision 定义为 fitted total-logit Bernoulli Fisher + `lambda` 的 diagonal projection，
  与 verifier 重算的 Hessian 对角线一致。

同时保存 fit provider `valid_count[k]` 和 `failure_count[k]`。这两组计数是 hazard ledger
的 fit prior，不能在 replay 中由 evaluation 或 selected event 反推。

### P1：进入 replay 前补齐

**P1-1. NPZ dtype 需要 fail closed。** verifier 现在把 archive 数组 cast 成 float64 后再 hash；
建议要求 archive 中 `theta` 的 dtype 为 little-endian float64、shape 为 `[G*d+K]`，否则
float32 或 endian 变体可能被静默转换后通过。

**P1-2. source hash 需要实际核验。** candidate config 写了 candidate/objective/fit source
hash，但 verifier 当前只记录自己的 source hash，没有逐项验证 config 中的 source 文件 hash。
verifier 应拒绝 source 文件不存在或 hash 不匹配的 receipt。

**P1-3. candidate receipt 应绑定完整状态 hash。** 除 theta hash 外，增加 canonical JSON 的
fit-state hash（means、precision、counts、provider/family 顺序），防止只替换 variance 或
counts 而保留 theta hash。

## 推荐的最小 replay 加载接口

```text
load_mira_relay_fit_state(
    receipt_dir,
    expected_fold,
    expected_seed,
    expected_input_sha256,
    expected_registry_sha256,
) -> {
    providers, families, family_by_provider,
    fit_family_residual, fit_provider_bias,
    fit_family_variance, fit_provider_bias_variance,
    fit_valid_counts, fit_failure_counts,
    state_hash, source_hashes,
}
```

加载器必须先调用独立 verifier；`verification.status != "verified"` 时立即拒绝。随后按
`theta[:G*d].reshape(G,d)` 和 `theta[G*d:]` 拆参数，使用 verifier 封存的 diagonal precision
倒数得到 policy variance，绑定 provider/family 顺序和 expected fold/seed。加载器不得读取
evaluation truth、未选 provider output 或 online event，也不得以默认 variance/计数补字段。

## 当前结论

候选 fit 的逐折 objective/gradient/hash 证据链已经足够审查数值求解；但在补上 P0-1、P0-2
和显式 precision/variance/counts 之前，**不能把当前 NPZ 直接接入 MIRA-Relay online replay**。
当前 verified 只表示“参数优化收据可独立复算”，不表示“replay 可无歧义恢复完整 policy state”。

## 2026-10-03 复核更新

- P0-1（fold/seed、objective、L2、optimizer、迭代预算、输入矩阵和 source hash 绑定）已在
  `verify_mira_relay_trust_exact_candidate_v1.py` 实现并通过 fold0/seed0 复核。
- P0-2（普通 `accepted` 必须是 SciPy status 0）已实现；status2 仍单独标成
  `numerically_stationary_status2`，不改写为 success。
- P0-3 仍未转化为候选 archive 的持久字段。contract replay v2 只在 fit cells 上重算 Hessian
  对角线并记录 provenance；这满足实现核验，但不能替代独立 fit-state serializer/verifier。
