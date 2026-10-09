# MIRA-Relay fit stationarity certificate (v1)

**Status:** complete read-only numerical certificate for the existing fold 0--4, seed 0 trust-exact candidate artifacts. This is a fit-geometry certificate. It does not change the effective preregistration, the status0-only fit-state contract, benchmark scores, or any replay/confirmation decision.

## Scope and reproducibility

The certificate reads the immutable candidates under `experiments/logs/task49_mira_relay_fit_diagnostic_20261003/trust_candidate_crossfold_v1/fold{0..4}_seed0/`. It calls `diagnose_mira_relay_fit_optimizers_v1.make_problem` after setting that module's `FOLD` and `SEED` globals for each fold, then restores the previous values. `make_problem` deterministically reconstructs the already frozen `fit_fold_priors` direct-head offset and checks the candidate's complete input metadata (including `direct_prior_sha256`); it does not run the MIRA residual optimizer. The derivative function in the certificate independently assembles the logistic design matrix and computes the objective, gradient, and Hessian. No evaluation labels, replay, confirmation data, GPU job, or external API is accessed.

The exact command was:

```text
/Users/jingwu/work/benchmark/.srcprobe/beir_venv/bin/python -B -m src.probe_headless.certify_mira_relay_stationarity_v1 --output experiments/logs/task49_mira_relay_stationarity_20261003/v1
```

The complete pre-compute configuration, command, git commit, source hashes, input/checkpoint hashes, candidate archive/receipt hashes, fold identity hashes, runtime versions, and thread environment are in [`config.json`](../../../experiments/logs/task49_mira_relay_stationarity_20261003/v1/config.json). The config was written before any `make_problem` reconstruction or derivative evaluation. Streaming execution events and failures are in [`raw.jsonl`](../../../experiments/logs/task49_mira_relay_stationarity_20261003/v1/raw.jsonl). SHA-256 of the config is `70889f10967effd3a9006a618271222dfd733418c377901cacc99c412c16dbda`; SHA-256 of [`summary.json`](../../../experiments/logs/task49_mira_relay_stationarity_20261003/v1/summary.json) is `c0f6e01048bdcc46da8e2e3f9cd46250a0ceae14a53c03a42854b9348678c027`.

Main input hashes recorded before computation were:

| Input | SHA-256 |
|---|---|
| `fingerprint_diagnostic_v1/inputs.json` | `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf` |
| `task38-p01-provider-pool/external_qualification.json` | `9738363d3ad64d0e52ceffb71acfe7b4f3db5bea9ba007a475876f7e0f2ff365` |
| fold0 checkpoint | `971084523d8bee30019b16a895426df589a5179614971a7a93bc63f364bfc82b` |
| fold1 checkpoint | `ed87eb0de5e12921e942082128d5ca3bc4727f9b25cbe07c1eed95180f42edda` |
| fold2 checkpoint | `ccb136a21304aad9f7bd65d9ccafbe41c6e229ff408243364b6517cf7bbb2efb` |
| fold3 checkpoint | `af3c24cbf09b970c4aca70df55fae0f77b8933b5a32c2174fd2adc6e5a201406` |
| fold4 checkpoint | `4b6a14d47ad2cf604b35a0bd8100e2d5cd720148a7f88876a7834871fc661e46` |

The run ended `completed`: 5/5 folds were certified, all candidate/input/source files were unchanged, and every original candidate receipt status was retained. Per-fold JSON and derivative NPZ files are in the same log directory.

## Mathematical certificate

For each fit observation $j$, let $a_j$ contain the frozen family feature block and one-hot provider offset, let $c_j$ be the frozen direct-head logit, and let $z_j=c_j+a_j^\top\theta$. With $y_j\in\{0,1\}$, the fixed objective is

$$
f(\theta)=\sum_j\left[\log(1+e^{z_j})-y_jz_j\right]+\frac{\lambda}{2}\|\theta\|_2^2,
\qquad \lambda=0.01.
$$

Writing $A$ for the matrix whose rows are $a_j^\top$, $p=\sigma(A\theta+c)$, and $D=\operatorname{diag}(p_j(1-p_j))$,

$$
g(\theta)=A^\top(p-y)+\lambda\theta,
\qquad H(\theta)=A^\top D A+\lambda I.
$$

Because $D\succeq0$, every vector $v$ satisfies

$$
v^\top H(\theta)v=(Av)^\top D(Av)+\lambda\|v\|_2^2\ge\lambda\|v\|_2^2.
$$

Thus this objective is globally $0.01$-strongly convex, regardless of the rank or conditioning of $A$, and has one global minimizer $\theta^\star$. For any finite candidate point and its exact gradient $g$, strong convexity and $g(\theta^\star)=0$ give

$$
f(\theta)-f(\theta^\star)\le\frac{\|g\|_2^2}{2\lambda},
\qquad
\|\theta-\theta^\star\|_2\le\frac{\|g\|_2}{\lambda}.
$$
The run evaluates these expressions in float64. They are numerical estimates, not interval proofs: no rounding-error enclosure is claimed. An infinity-norm-only version is also valid via \(\|g\|_2\le\sqrt{d}\|g\|_\infty\), and is recorded in each fold file; the tighter table below uses the independently recomputed Euclidean norm.

These bounds concern proximity to the minimizer of this fixed fit objective only. They provide no guarantee for held-out prediction accuracy, provider ranking, action changes, online utility, calibration, or any other downstream benchmark quantity.

## Per-fold result (raw statuses preserved)

`objective` and gradients below are recomputed by the independent derivative function. `gap upper` is \(\|g\|_2^2/(2\lambda)\); `distance upper` is \(\|g\|_2/\lambda\). The status columns are copied from each immutable candidate receipt; `status2` is not relabelled as success.

| fold / seed | raw candidate status | SciPy status / success | objective | \(\|g\|_2\) | \(\|g\|_\infty\) | gap upper | distance upper | Hessian symmetry max |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| 0 / 0 | `accepted` | 0 / true | 3340.1063218412314 | 1.1341381224719963e-08 | 2.326657609630356e-09 | 6.431346404221524e-15 | 1.1341381224719963e-06 | 1.1368683772161603e-13 |
| 1 / 0 | `accepted` | 0 / true | 4368.663826179208 | 1.1424445733171515e-07 | 2.4503036544221146e-08 | 6.525898015509042e-13 | 1.1424445733171516e-05 | 2.2737367544323203e-13 |
| 2 / 0 | `numerically_stationary_status2` | 2 / false | 4824.5478357528 | 1.0051356462780543e-06 | 2.2856493960693447e-07 | 5.0514883370940095e-11 | 1.0051356462780543e-04 | 1.1368683772161603e-13 |
| 3 / 0 | `accepted` | 0 / true | 4482.632929475461 | 5.926846273596107e-07 | 1.1898711683999696e-07 | 1.7563753375420028e-11 | 5.926846273596107e-05 | 1.1368683772161603e-13 |
| 4 / 0 | `accepted` | 0 / true | 4107.546867096723 | 5.522456821839028e-10 | 1.2998604016073445e-10 | 1.524876467453821e-17 | 5.522456821839028e-08 | 1.1368683772161603e-13 |

Each row also verifies the explicit fold field, fit-index/entity identity, seed, provider/family order, direct-prior hash, checkpoint hash, candidate parameter shape/hash, objective and gradient against the original receipt, and source hashes. `fold2` retains the original `scipy_status=2`, `scipy_success=false`, and message (`A bad approximation caused failure to predict improvement.`).

## Is status2 a mathematical blocker?

For the fixed objective, no. Fold2's finite independently recomputed gradient is (\|g\|_\infty=2.2856494\times10^{-7}), and the strong-convexity certificate bounds its objective suboptimality by about (5.05\times10^{-11}) and parameter distance by about (1.01\times10^{-4}) in float64 arithmetic. SciPy's status2 is a solver termination report, not a proof that the objective lacks a minimizer or that this point is far from it.

Status2 remains a contract blocker wherever the active fit-state policy requires status0-only. The raw failure is therefore preserved and cannot be silently changed to `accepted`; using fold2 in an effect matrix, confirmation set, or revised preregistration would require a separately authorized status2 contract and state package. This certificate does not make that policy choice and does not establish any downstream accuracy or selection benefit.
