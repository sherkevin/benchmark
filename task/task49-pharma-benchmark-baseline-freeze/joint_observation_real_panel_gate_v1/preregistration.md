# Joint observation real-panel gate v1

日期：2026-10-05  
目的：在现有 BBB development panel 上测试 paired-observation joint model 是否在 scaffold-disjoint held-out rows 上优于同观测合同的 Independent Evidence。

## 固定协议

- 输入：`fingerprint_diagnostic_v1/inputs.json`，722 rows、33 providers、已有 5 个 scaffold-disjoint fold。
- 只使用每个 fold 的 `fit` rows 拟合；`validation` 不用于调参，`evaluation` 只用于一次性评估。
- 观测编码：provider status=`ok` 时使用 `output_labels∈{0,1}`；status=`error` 时使用显式第三类 `⊥=2`，不使用失败行中可能存在的 label。
- Independent Evidence：每个 provider、每个 truth $y$ 独立估计三类观测概率。
- Joint Evidence：每个 truth $y$ 使用固定 2-mode mixture-of-products；Dirichlet smoothing `alpha=0.5`，EM restart `0..3`，最多 200 次迭代，按 fit likelihood 选 restart；不使用结果集调参。
- 评价两类输入：全部 33 provider 的 held-out full-panel NLL/Brier/accuracy；528 个 provider pair 的平均 held-out pair NLL/Brier/accuracy。Pair 指标只评估该 pair 的观测边际，防止 full-panel NLL 掩盖 pair 行为。
- 报告每 fold、macro mean 和 Joint−Independent；不把该诊断写成 sequential policy 或 online adaptation 结果。

## 通过条件

Joint 至少满足：

1. full-panel conditional NLL 在 5 folds 中至少 4 个 fold 下降，macro mean 下降；
2. pair NLL 在 5 folds 中至少 4 个 fold 下降，macro mean 下降；
3. pair posterior Brier 不比 Independent 恶化超过 `0.01`，并报告 ECE/accuracy 作为辅助。

若不通过，停止扩大 latent mode 或引入 planner；优先检查 data qualification、family memorization 和 missingness。若通过，也只授权进入同容量 pairwise control 和 matched finite-horizon planner 的 CPU gate。
