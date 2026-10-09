# Existing-data joint distribution anchors

Status: **DESCRIPTIVE_EXISTING_DATA**. These quantities describe sealed observations. They are not candidate predictions, prospective confidence intervals, a preregistration or a new experiment. No training, provider calls, policy replay, confirmation decision changes, threshold tuning or bootstrap sampling were performed.

The reusable primary output is `distribution_anchors.json`. Sixteen CSVs preserve the matched configuration and actual molecule rows. `audit_existing.py` uses only the Python standard library and never imports a production runner. `provenance.json` records 19 source hashes, source sizes, the exact extraction command, script hash and output hashes. All ten original Guide sources match their recorded hashes. The corrected postcall ledger also matches the corrected analysis's source hash.

## Units and uncertainty

- E1/E2 use **157 unique molecules, 124 scaffolds**, evaluated under five development-fit folds, two seed labels and three delays. Every stream reuses all 157 molecules. The five fit folds are not five disjoint confirmation folds.
- MIRA, Frozen, matched SGD and LinUCB have **exactly duplicate terminal correctness/tick/provider vectors across seed 0 and seed 1**, in both conditions. BayesianRouter varies across the two seeds. Random varies in the natural block. Hence most seed labels are deterministic replicas, not two independent robustness tests.
- E3 uses a genuine five-fold OOF partition of **722 unique development molecules**. Its two learned seeds share the same molecules. E4 repeats the 722 OOF molecules over three delays.
- E6 repeats the same **157 molecules over five fit/validation folds**: 785 records. Its random comparator has one recorded random seed and is an evaluator-side matched-count analysis.
- The SDs below are **descriptive spread of five fit-fold mean accuracies**, averaging each fold's recorded seed/delay configurations. They are not standard errors, predictive SDs or a justification for high-confidence future intervals. Full stream ranges and within-fold dependence remain in JSON/CSV.

## E1: accuracy, errors and paired changes

Each supported method/condition has 4,710 recorded decisions. SD is in percentage points.

| Method | Natural correct / errors | Accuracy | Fold-mean SD | Controlled correct / errors | Accuracy | Fold-mean SD |
|---|---:|---:|---:|---:|---:|---:|
| Random | 3945 / 765 | 83.758% | 0.000 | Not recorded | — | — |
| Fit-only Best Fixed | 4152 / 558 | 88.153% | 0.570 | Not recorded | — | — |
| Frozen | 4230 / 480 | 89.809% | 1.192 | 3456 / 1254 | 73.376% | 5.827 |
| Matched online SGD | 4202 / 508 | 89.214% | 1.012 | 3958 / 752 | 84.034% | 5.978 |
| LinUCB | 4226 / 484 | 89.724% | 1.109 | 3822 / 888 | 81.146% | 5.251 |
| BayesianRouter | 4217 / 493 | 89.533% | 0.845 | 3781 / 929 | 80.276% | 2.643 |
| Marginal reliability | 4230 / 480 | 89.809% | 1.192 | Not recorded | — | — |
| MIRA query reliability | 4230 / 480 | 89.809% | 0.212 | 4124 / 586 | 87.558% | 1.861 |
| Existing fixed-provider oracle control | 4260 / 450 | 90.446% | 0.000 | Not recorded | — | — |

Random's zero fit-fold SD follows reuse of the same seed-specific action vectors across fit folds; it does not imply zero uncertainty. The existing 142/157 oracle control is a fixed provider control. The true native per-molecule panel oracle is **150/157 = 95.541%**, with 110 all-correct, 40 mixed and seven all-wrong molecules.

MIRA versus Frozen forms the following paired 2×2 tables. `rescued − harmed = net_correct` is mandatory for any scenario.

| Condition | Both correct | Rescued | Harmed | Both wrong | Net correct |
|---|---:|---:|---:|---:|---:|
| Natural | 4148 | 82 | 82 | 398 | 0 |
| Controlled | 3344 | 780 | 112 | 474 | +668 |

The natural Frozen errors contain 210 all-panel-wrong records and **270 avoidable records**. MIRA rescues 30.370% of those avoidable errors and harms 1.939% of Frozen-correct records. Under the recorded midpoint intervention, the full-panel counterfactual oracle becomes 154/157 for fit-fold 0 and 152/157 for folds 1–4. The exact aggregate cap is **4572/4710**, leaving 138 all-panel-wrong records and **1116 Frozen avoidable errors**. MIRA rescues 69.892% of those avoidable errors and harms 3.241% of Frozen-correct records. This panel calculation changes only evaluator-side correctness under the already-recorded intervention and was checked against every selected terminal; no policy was rerun.

MIRA natural per-fit-fold paired net changes are **−16, +4, −2, +14, 0** of 942 records per fold; controlled changes are **+56, +116, +154, +190, +152**. The natural aggregate equality hides mixed fit-fold effects. MIRA natural stream accuracy spans 88.535–91.083%; controlled spans 83.439–91.083%.

The same records provide a phase anchor at tick 78. Phase counts are repeated records, with 78 pre-event and 79 post-event molecules per stream.

| Method and condition | Pre correct / n | Post correct / n |
|---|---:|---:|
| Frozen natural | 2070 / 2340 | 2160 / 2370 |
| MIRA natural | 2046 / 2340 | 2184 / 2370 |
| Frozen controlled | 2070 / 2340 | 1386 / 2370 |
| MIRA controlled | 2046 / 2340 | 2078 / 2370 |

Before tick 78, all common methods have identical natural/control terminal vectors. MIRA controlled versus natural has 4,112 both-correct, 12 controlled-only correct, 118 natural-only correct and 468 both-wrong records. Its aggregate controlled drop of 106 correct records is a matched contrast, not an independent second sample.

## E2: preserve the scaffold-weighted joint contrast

The mechanism Local method matches the E1 MIRA count and the complete terminal correctness/tick/provider vector in **every fit-fold/seed/delay configuration**. Both mechanism conditions declare the seed labels as “two deterministic schedule replicas; not seed robustness”; complete terminal vectors across those seed labels were also checked equal.

| Condition and contrast | Equal-weight scaffold mean | Pooled net correct / 4710 | Rescued / harmed |
|---|---:|---:|---:|
| Natural Local − Frozen | +0.1030 pp | 0 | 82 / 82 |
| Natural permutation − Frozen | +0.4077 pp | +8 | 70 / 62 |
| Natural Local − permutation | −0.3047 pp | −8 | 106 / 114 |
| Controlled Local − Frozen | +14.4041 pp | +668 | 780 / 112 |
| Controlled permutation − Frozen | −1.2679 pp | −46 | 206 / 252 |
| Controlled Local − permutation | +15.6720 pp | +714 | 874 / 160 |

For Local−permutation, natural scaffold deltas are positive on six scaffolds, negative on seven and zero on 111. Controlled deltas are positive on 56, negative on eight and zero on 60. The natural descriptive scaffold SD is 6.260 pp and controlled SD 28.392 pp. These are outcome heterogeneity across 124 shared scaffolds, not uncertainty of a future method's mean.

Each scaffold's Local−permutation equals Local−Frozen minus permutation−Frozen. Keep this relation if a scenario shifts either arm. A pooled 15.1592 pp contrast must not be substituted for the primary 15.6720 pp equal-weight scaffold contrast. Historical intervals are copied and clearly labeled inside JSON; no new intervals were estimated.

## E3: the unique error budget is 39 molecules

| OOF fold | n | Corrected Frozen correct | All-panel-wrong | Avoidable errors |
|---|---:|---:|---:|---:|
| 0 | 145 | 108 | 28 | 9 |
| 1 | 145 | 111 | 25 | 9 |
| 2 | 144 | 119 | 19 | 6 |
| 3 | 144 | 125 | 12 | 7 |
| 4 | 144 | 116 | 20 | 8 |
| Total | 722 | 579 | 104 | 39 |

There are 471 all-correct and 147 mixed molecules. Corrected Frozen gets 108/147 mixed molecules right. Its errors split into **104 unavoidable panel errors and 39 rank-selectable errors**. A 591/722 scenario therefore requires at least 12 net improvements from the 39 selectable errors, jointly allowing harm to the 579 currently correct molecules.

The best-correct-minus-selected corrected score gap on those 39 errors ranges from −0.43038 to −0.00234, mean −0.08539. This diagnoses recorded rank gaps; it does not calibrate a rescue probability. In the delayed natural streams, 115/117 repeated avoidable-error records already have a released correct-candidate receipt; the other two have pending feedback. The 117 records are three repeated schedules of the 39 molecules.

| Existing learned candidate | Correct / 722 | Rescued / harmed vs Corrected | Five-fold net correct |
|---|---:|---:|---|
| BCE seed 0 | 567 | 16 / 28 | −6, −2, −3, +1, −2 |
| BCE seed 1 | 570 | 17 / 26 | +2, −5, 0, +1, −7 |
| BT seed 0 | 569 | 14 / 24 | −7, +1, −2, −1, −1 |
| BT seed 1 | 565 | 9 / 23 | +1, −6, −1, −1, −7 |

All recorded leave-one-fold-out paired differences are negative. Learned fold counts were obtained by adding the recorded paired difference to the independently counted molecule diagnostic anchor and cross-checked against each recorded verified fit count. No model forward pass was made.

## E4: same-prior online changes

Each cell/condition has 2,166 records = 722 unique molecules × three delays. Corrected/query versus corrected/frozen has natural **26 rescues / 23 harms = +3**, controlled **425 rescues / 92 harms = +333**. Query versus marginal is only +11 natural and +8 controlled. These comparator choices answer different questions.

| Cell | Natural correct / errors | Controlled correct / errors |
|---|---:|---:|
| raw/frozen | 1719 / 447 | 1497 / 669 |
| raw/marginal | 1715 / 451 | 1674 / 492 |
| raw/query | 1700 / 466 | 1645 / 521 |
| corrected/frozen | 1737 / 429 | 1371 / 795 |
| corrected/marginal | 1729 / 437 | 1696 / 470 |
| corrected/query | 1740 / 426 | 1704 / 462 |

Natural corrected/query−corrected/frozen net changes across delays 0/1/4 are: fold 0 **+1/0/+2**, fold 1 **0/+2/+1**, fold 2 **0/−1/0**, fold 3 **+2/+1/+2**, fold 4 **−4/−1/−2**. Controlled changes are: **+20/+19/+22**, **+25/+28/+24**, **+22/+26/+22**, **+25/+25/+22**, **+17/+18/+18**. Delay variation is not uniformly monotone.

Recorded cumulative-error arrays provide exact event correctness without reading another full raw ledger. They were reconstructed independently and all counts matched. If E3 changes the prior, these E4 anchors no longer represent the same new candidate automatically: a common-prior contract or an explicitly separate intervention is required.

## E6: matched acceptance includes heterogeneous fold budgets

| Fold | Accepted | Accepted wrong | Refused | Accepted accuracy |
|---|---:|---:|---:|---:|
| 0 | 119 | 5 | 38 | 95.798% |
| 1 | 134 | 7 | 23 | 94.776% |
| 2 | 137 | 8 | 20 | 94.161% |
| 3 | 137 | 8 | 20 | 94.161% |
| 4 | 136 | 8 | 21 | 94.118% |
| Total | 663 | 36 | 122 | 94.570% |

Always-accept errors are 17/19/19/19/19. The same-coverage random errors are 13/14/16/17/15. Calls are 157 per fold for all policies. Accepted accuracy and coverage must move jointly with accepted correct, accepted wrong and refusal counts; a target accepted accuracy alone does not identify a plausible selection rule. The fixed utility equation is `−(wrong + 0.25 × refusal + 0.01 × calls) / n`.

The confidence gate refuses **65 correct and 57 wrong** chosen-provider outputs, as determined from the matched always-accept ledger. Correct-refusal budgets by fold are **26/11/9/9/10**. Together with accepted-wrong counts **5/7/8/8/8**, the evaluator-only maximum same-count wrong-for-refused-correct swaps are **5/7/8/8/8**, totalling 36. This is a hindsight combinatorial bound, not an available deployable gate. Refusal rows retain `answer_correct=null`; the diagnostic counterpart is explicitly labeled evaluator-only.

## Compact row contracts for the revised Guide

- `e1_actual_molecule_rows.csv`: 65,940 recorded terminal decisions, with condition, fit-fold, seed, delay, entity, scaffold, tick, actual provider/correctness and Frozen provider/correctness. `rescued` and `harmed` are actual paired flags. `e1_scaffold_config_pairs.csv` aggregates those flags within each configuration/scaffold.
- `e2_actual_molecule_rows.csv`: 28,260 actual matched mechanism terminal decisions, with the same fields; `e2_paired_streams.csv` and `e2_scaffold_config_pairs.csv` preserve exact configuration and scaffold rescue/harm counts. Recomputed scaffold paired deltas match the recorded mechanism analysis. `e2_scaffold_contrasts.csv` retains the 744 original contrast/scaffold values.
- `e3_fold_accuracy_headroom.csv`: 35 rows covering the three static anchors and four learned candidate/seed combinations, with fold counts, all-correct/mixed/all-wrong partitions, mixed correct counts and avoidable-error headroom.
- `e4_paired_streams.csv`: 180 matched cell-pair configurations, with whole-stream and pre/post 2×2 counts. `e4_actual_molecule_correctness.csv` retains 25,992 correctness records, aligned by original fold tick. Natural raw/corrected Frozen vectors were cross-checked against the molecule diagnostics. The compact cumulative-error source does not contain actions; `action_available_in_this_source=false` prevents an invented action record.
- `e6_fold_accepted_errors.csv`: 20 policy/fold rows with A/W/R/C/N, accuracy/coverage/utility and hindsight swap budgets. `e6_actual_molecule_rows.csv` retains 3,140 evaluator decisions and their explicitly labeled always-accept correctness counterparts.

The rows permit a transparent **conditional design response** `C' = C + alpha × H − beta × R`, where `H` denotes existing harms reversed and `R` existing rescues lost. Apply the same explicit coefficients to matched rows before aggregating pooled counts or equal-weight scaffold differences. For each scaffold/configuration, retain its original molecule denominator; the primary E2 statistic averages the within-scaffold matched difference over configurations, then gives each of the 124 scaffolds equal weight. Derive Local−permutation from the two adjusted arms using their shared Frozen anchor. This formula alone is an assumption-driven sensitivity calculation. Neither its coefficients nor a high-confidence probability distribution are supplied by these data.

## What remains necessary for a predictive Guide

The sources provide reusable fit/seed/delay configuration rows, per-scaffold contrasts, error partitions, rescue/harm pairs, before/after counts and refusal distributions. They establish conservation constraints and plausible dependence structures. They do **not** establish a future candidate's high-confidence distribution. A future model requires a specified candidate intervention and prospective population, plus explicit assumptions for which errors are rescued and which currently correct answers can be harmed. A common molecule/configuration latent structure should couple E1–E4; confidence gating requires its own joint acceptance/error model. Latency and memory require separately supported measurements or clearly labeled budgets.

This audit makes no scientific-go decision and leaves the recorded HOLD status unchanged. The original extraction encountered a source naming mismatch (`method` is an implementation-class label for several methods, while stream IDs carry the display method ID); it was corrected to use the recorded stream ID. No source files or result definitions were altered.
