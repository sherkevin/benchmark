# Arrival-order empirical distribution audit

Status: **DESCRIPTIVE_EXISTING_DATA; qualified by recorded independent v2 verification**. This writing task performed read-only extraction and arithmetic. It did not execute a policy, runner, model, new inference, training, experiment or new interval estimation. No canonical paper files were edited.

## Qualification

The sealed `full_v1/summary.json` records completed orders **1–128**, **61,440 streams** and **9,646,080 decisions**. The existing `independent_full_v2/verification.json` is `verified` and binds the same run config and manifest. Its recorded audit covers all 17 receipt fields, 38,584,320 scores, 61,440 final numerical/complete instance states, 23,040 RNG-state records and 422,400 state arrays. Reported maximum errors are zero, and malformed-release and terminal-label mutations were rejected. Production results and independently reconstructed statistics are equal as parsed JSON.

The preserved v1 verification failed on secondary histogram counts at floating-point bin boundaries; v2 closure identifies 35/168 affected histograms and differences no larger than 1.42×10⁻¹⁴ pp. V2 preserves exact integer checks and applies the archived numerical operation order. Production results were preserved. Its full-run historical original-order counts are zero; original-order inheritance belongs to the separately recorded smoke audit. The broad scientific gate remains HOLD.

This extraction checks all **654 used source hashes**, all 128 distinct order rows, per-order/per-stream counts, actual Frozen rescue/harm, before/after counts and equal-scaffold contrasts against the independently recorded v2 metrics. It reads compressed receipt correctness/actions for paired summaries. It does not rerun the independent state replay or reload the 3 GB final-state arrays; those are covered by the existing v2 audit.

## Statistical object

One Monte Carlo draw is a **complete uniform arrival permutation of the already-viewed 157-molecule cohort**. Each draw is jointly shared across all methods, five frozen fit priors, two fixed policy seeds, delays 0/1/4 and both conditions. The primary is controlled MIRA−matched online SGD pooled correctness, equally averaging 30 configurations with **4,710 records per order**. The 128 order IDs are the Monte Carlo units. Molecules, fit priors, policy seeds and provider checkpoints do not supply additional independent units.

Controlled flips begin at tick 78. Reordering changes the observed learning history and which molecules encounter the flip. These results condition on that intervention, the same fixed cohort, providers, priors and seeds. They do not estimate new-scaffold, new-training or deployment uncertainty.

| Primary object | Existing verified value | Interpretation |
|---|---:|---|
| Mean MIRA−SGD | +4.736266 pp | Average over the declared arrival law |
| Order SD | 2.398819 pp | Heterogeneity across complete arrival orders |
| MCSE of mean | 0.212028 pp | Monte Carlo uncertainty in the average |
| Mean MC interval 95% | [4.316701, 5.155830] pp | Recorded Student-t/CLT interval, df=127 |
| Empirical order quantiles 2.5/97.5% | [0.449045, 8.612527] pp | Descriptive order distribution, not mean CI |
| Observed range | [−2.675159, 10.276008] pp | All 128 orders retained |
| Positive / negative / ties | 125 / 3 / 0 | Recorded direction frequency |
| Exact positive-frequency interval 95% | [0.933034, 0.995140] | Clopper–Pearson interval for P(Δ>0) under a fresh uniform order |

Negative primary orders are **69, 86, 90**, with contrasts −2.675159, −0.721868 and −0.849257 pp. Their rescue/harm counts versus SGD are **164/290, 80/114, 64/104**, respectively. Mean rescues and harms are **337.90625 and 114.828125** per order. Both preregistered directional criteria pass for the declared order law; this does not establish a positive practical-benefit margin.

## Joint method distributions

These summaries pool all three delays within each order. SD is across the same 128 orders.

| Method | Natural accuracy mean / SD | Controlled accuracy mean / SD |
|---|---:|---:|
| Frozen | 89.808917 / 0 pp | 75.753384 / 1.455553 pp |
| Matched SGD | 89.052880 / 0.607988 pp | 82.224323 / 2.872893 pp |
| LinUCB | 89.700438 / 0.330930 pp | 76.899217 / 2.480356 pp |
| BayesianRouter | 89.717854 / 0.566407 pp | 79.604731 / 1.635965 pp |
| MIRA query / Local | 89.521961 / 0.488086 pp | 86.960589 / 1.455417 pp |
| Matched permutation | 89.862991 / 0.462055 pp | 71.395303 / 3.723655 pp |

Natural Frozen is constant across orders. Tiny computed SDs around 10⁻¹⁴ pp reflect floating-point centering; its correlation with another method is undefined and stored as null. Constant natural fixed-provider and oracle-control vectors have the same interpretation.

| Matched contrast, all delays | Mean Δ | Actual paired SD | Covariance (pp²) | Correlation | SD if independence were assumed |
|---|---:|---:|---:|---:|---:|
| Natural MIRA−Frozen | −0.286956 | 0.488086 | 0 | Undefined | 0.488086 |
| Natural MIRA−SGD | +0.469082 | 0.733233 | +0.035124 | +0.118362 | 0.779665 |
| Natural Local−permutation | −0.341030 | 0.710078 | −0.026244 | −0.116369 | 0.672104 |
| Controlled MIRA−Frozen | +11.207205 | 2.459206 | −0.905409 | −0.427395 | 2.058367 |
| Controlled MIRA−SGD | +4.736266 | 2.398819 | +2.308709 | +0.552157 | 3.220521 |
| Controlled Local−permutation | +15.565287 | 4.394668 | −1.664630 | −0.307157 | 3.997981 |

For every pair/stratum, **Var(Δ)=Var(A)+Var(B)−2Cov(A,B)** was checked numerically. The controlled MIRA−SGD shared covariance substantially reduces its contrast SD. Negative covariance increases contrast variation for the other two controlled pairs. Treating methods as independently drawn gives the wrong distribution.

The natural MIRA−Frozen recorded mean Monte Carlo interval is **[−0.372324, −0.201587] pp**, while its empirical order quantiles are **[−1.181529, +0.544586] pp**. The latter crossing zero does not undo the negative estimated mean. The natural equal-scaffold MIRA−Frozen mean is +0.2110 pp; it uses different weights and must not replace the preregistered pooled measure. Secondary intervals are recorded **pointwise exploratory** intervals, with no simultaneous-coverage or selected-winner guarantee. A pooled Local−permutation mean must not borrow a recorded equal-scaffold interval.

## Artifacts and reuse

- `arrival_distribution_anchors.json`: primary interval objects, independently recomputed quantiles, all method/stratum distributions, paired covariance/correlation, exact variance identities and full joint covariance matrices. Each method retains its original interval object with its scope.
- `primary_order_rows.csv`: 128 actual primary differences and rescue/harm pairs.
- `order_method_counts.csv`: 8,192 order/condition/delay/method counts. `delay=all` is a derived aggregate of the same three delay rows.
- `order_paired_counts.csv`: 11,264 actual order-level 2×2 tables, including MIRA−Frozen/SGD/LinUCB/BayesianRouter/permutation; rescue−harm equals the recorded net difference.
- `order_scaffold_pairs.csv`: 380,928 matched scaffold-count rows for Local/Frozen/permutation contrasts. They support equal-scaffold sensitivity arithmetic using the original within-scaffold denominators.
- `actual_stream_counts.csv`: the 61,440 recorded stream metrics, retaining fit, seed, delay, before/after correctness and observed Frozen rescue/harm.
- `provenance.json` and `extract_arrival.py`: hashes, commands, runtime version and extraction scope. The script imports NumPy and standard-library arithmetic only, never production policies or runners.

The empirical rows can supply one Guide page showing same-order method dependence, contrast distributions and separately labeled mean intervals versus order quantiles. They can also support `C'=C+alpha×H−beta×R` assumption-driven design-response arithmetic. They do not supply a new candidate's future-performance interval. Root retains ownership of Guide writing and scientific decisions.
