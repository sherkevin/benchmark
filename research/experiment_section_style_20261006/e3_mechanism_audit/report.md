# E3 mechanism audit：interface / independent residual / family permutation

日期：2026-10-06
范围：`task49-pharma-benchmark-baseline-freeze/mechanism_ablation_matrix_v1/v2` 与一次最小 family-label permutation CPU replay。此报告不改变主方法 claim。

## 审查结论

### 1. interface removal

现有 veto ablation 的 `frozen` 在同一 selected-only runner 中保持 fit-only state，不释放更新，且与其他变体共用输入、clock、evaluator 和 event schedule。它可以作为“去掉在线更新”的控制。它不是 literal 的 API-null implementation；因此文档应写成 `Frozen/no-update control`，不能写成已完成独立空接口 runner。

### 2. independent residual

现有 `matched_sgd` 是每 provider 独立的 selected-only online logistic update，失败按 reward 0，使用同一 feedback clock 和同一 frozen prior。它是独立残差/在线更新的最近邻控制。它足以回答“收益是否只是任意 provider-local online update”，但不能证明与 MIRA residual 数学等价，也不能把 matched-SGD 的高分隐藏掉。v1 已有 300 streams、43,320 decisions/terminals/feedback 的 verified receipt。

### 3. family permutation

可以在不生成新预测的情况下做 CPU replay：family registry 只影响 FamilyGuard 的 group state；provider output/status/label 全部来自已封存 input panel，event failure 是 deterministic evaluator-side injection。普通 target generator 的 20-coordinate common-support gate 对 synthetic permutation 不适用，因此单独写了 diagnostic manifest/verifier，保留生产 verifier 的不通过原因。

## 实际 replay

坐标：`fold=4, seed=0, delay=0`，transient + persistent，5 methods，10 streams，1,440 decisions/terminals/feedback；自定义 verifier `verified_permutation_replay` 通过。输入 SHA `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`，synthetic registry SHA `4fcb0a5a2bfbc628afb47e65af8654fbe1828092c12ee7c9980330550f12b85f`，manifest SHA `ded0cbbad03baf3ae33d68bf37d01e521a0316829979438848365a4c3d871a57`。

在 persistent case 中，permuted FamilyGuard 发生 35 次相对原 no-event control 的 action change，注入 5 次 synthetic failures，accuracy `0.8681`；transient case 改变 0 次动作，注入 1 次 synthetic failure，accuracy `0.8958`。这些是单一 development coordinate 的机制描述，不能外推为 superiority。Frozen persistent accuracy `0.5556` 是该 synthetic event 下的控制分母，不应与主 natural confirmation 表混用。

## 复核异常

生产 `verify_mira_family_guard_replay_v1` 在 family permutation 上拒绝 `prefix decision`，这是预期的语义边界：family label 被置换后，selected correctness 会更新不同 family bucket，事件前 state 已不同。该 stderr 保存在 `production_verifier_failure_fold4_v3.txt`；没有修改生产 verifier，也没有把 custom verifier 的结果升级为标准 FamilyGuard receipt。

## 可复用结论

E3 的主轴可以采用：Frozen/no-update、matched independent update、full MIRA、FamilyGuard-vs-relative 的同 runner 证据；family permutation 只作为可选 adversarial sensitivity。正文不要放 hash、manifest 或 verifier 细节；放到 appendix/supplement 的 audit ledger。
