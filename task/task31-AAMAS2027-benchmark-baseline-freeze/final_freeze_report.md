# Task31 Final Freeze Report

## Purpose

Freeze the evaluation setting before designing Ours.

The objective is to prevent benchmark/baseline selection from changing after seeing proposed methods.

## 1. Frozen Problem Formulation

Selective challenger onboarding:

Given:

- context x;
- incumbent tool a_i;
- arriving challenger tool a_c;
- reward r(x,a).

Define challenger advantage:

Δ(x)=r(x,a_c)-r(x,a_i)

The goal is to identify contexts where Δ(x)>0 under limited feedback.

## 2. Frozen Benchmark Set

Main benchmarks:

1. LLMRouterBench
   - modern LLM/tool routing.
   - context: query/task description.
   - action: candidate model/tool.
   - reward: task-level performance.

2. ASlib SAT12-ALL
   - classical algorithm selection.
   - context: instance features.
   - action: solver algorithm.
   - reward: solver performance.

3. BBB
   - black-box tool selection stress test.
   - context: task instance.
   - action: candidate tool.
   - reward: benchmark score.

Appendix:

- synthetic sparse challenger advantage benchmark.

Reason:
The synthetic setting validates controlled assumptions but should not replace real-world benchmarks.


## 3. Frozen Baseline Set

Main paper candidates:

| Family | Method |
|---|---|
| Current deployment | Incumbent-only |
| Linear contextual | LinUCB |
| Bayesian contextual | LinTS |
| Advantage modeling | ACTS |
| Nonlinear contextual | RFF-UCB |
| Transfer | P-LinUCB |
| Runtime onboarding | ParetoBandit-style |

Appendix:

- UCB1
- Beta-TS
- P-TS
- conservative variants

## 4. Evaluation Dimensions

Primary:

- cumulative regret;
- challenger precision;
- positive-region identification.

Safety:

- incumbent performance degradation;
- unnecessary challenger adoption.

## 5. Remaining Before Ours

The remaining work is not adding more baselines.

Required next step:

Analyze failure modes of frozen methods and derive the precise unresolved problem definition.

## 6. Exact Frozen Evaluation Protocol v1

### 6.1 LLMRouterBench

- Dataset split: within each of 15 datasets, shuffle and split 70/30.
- Split seeds: 3407, 3408, 3409, 3410, 3411.
- Challenger episodes: every one of the 20 candidate models is held out once as the zero-history challenger.
- Frozen incumbent: train-only TF-IDF (1-2 grams, min_df=2, max_features=60000, sublinear_tf) + per-arm Ridge(alpha=20, solver=lsqr), challenger removed before fitting/selection.
- Onboarding-manager context: train-only TF-IDF -> TruncatedSVD(32) -> L2 normalization -> bias coordinate.
- Stream order: one seeded permutation of the held-out test rows per split seed.
- Reporting: macro-average over the 15 datasets, then aggregate across challengers and split seeds.

### 6.2 ASlib SAT12-ALL

- Split: official cv.arff, repetition=1, 10 folds.
- Stream seeds per fold: 0, 1, 2.
- Challenger episodes: all 31 solvers are held out once per fold/stream.
- Reward: utility = 1 - clipped(PAR10, 0, 12000)/12000.
- Frozen incumbent: 113 usable features; train-fold median imputation + standardization; per-arm ExtraTreesRegressor(n_estimators=200, min_samples_leaf=4, max_features=sqrt, random_state=42+arm) on log1p(PAR10); challenger removed before predicted-min selection.
- Manager context: median imputation -> standardization -> PCA32 -> RMS scaling -> bias.

### 6.3 BBB Dense Panel

- Population: 652 molecules with complete truth/tool panel after unresolved-truth exclusion.
- Tools: 7 families (admet_ai, admetlab, b3clf, deeppurpose, fpadmet, graphb3, lgbm_bbb).
- Split: 5-fold Bemis-Murcko GroupKFold.
- Stream seeds per fold: 0..9.
- Challenger episodes: each of the 7 tool families is held out once per fold/stream.
- Frozen incumbent: among non-challenger tools, choose the highest train-fold accuracy tool and freeze it for that episode.
- Manager context: Morgan radius-2 / 1024-bit fingerprint -> train-only TruncatedSVD(32) -> L2 normalization -> bias.
- Reward: binary correctness against experimental BBB truth.

### 6.4 Feedback model

The main protocol is passive-context, bandit feedback rather than active choice of x:

1. contexts arrive in the frozen stream order;
2. the manager chooses incumbent or challenger;
3. only the reward of the selected action is exposed to the online learner;
4. incumbent history is available before arrival; challenger reward history is zero;
5. the counterfactual action reward is retained only by the evaluator.

This distinction is critical: the task is not arbitrary active level-set querying. The learner controls whether to probe/adopt the challenger at an arriving context, not which context arrives.

### 6.5 Few-feedback evaluation

A hard total challenger-pull cap is NOT added to the existing protocol because doing so would silently change Task29/30 experiments.
Instead, sample efficiency is frozen as an anytime curve over the existing checkpoints:

- challenger-feedback counts / early horizons: 1, 4, 8, 16, 32;
- plus quarter-stream, half-stream, and full-stream endpoints when applicable.

Primary paper reporting should therefore include early-checkpoint IOU/regret and adoption precision, not only the final stream average.
A separate fixed-budget mechanism experiment may be added only under a new task/protocol version.

## 7. Frozen Baseline Parameters

- Incumbent-only: frozen T2 policy; no challenger calls.
- UCB1: incumbent initialized with full train history; challenger count=0; standard mean + sqrt(2 log(total+1)/n), forcing an unobserved arm once.
- Beta-TS: Beta(1,1) challenger prior; incumbent pseudo-posterior initialized from bounded train rewards.
- LinUCB: lambda=1, alpha=1; incumbent linear sufficient statistics initialized from train history; challenger starts from prior only.
- LinTS: lambda=1, posterior scale v=1; same history asymmetry.
- ACTS: pi_min=0.2, pi_max=0.8, v=1.0; binary challenger vs baseline action-centering.
- RFF-UCB: official-equivalent adapter; num_rff_dim=200, gamma=5, reg_alpha=1, version=2D; official repo HEAD 3ed2ead.
- P-LinUCB: lambda1=3.5, lambda2=0.1, delta=0.1, meta_ridge=1, paper-default ucb_scale=1; conservative-scale sweeps are diagnostics only.
- ParetoBandit-style: burn-in B=20 is the official-style main comparator; alpha=0.01, forgetting_factor=0.997; B={1,4,8,16,20,32} retained as sensitivity.

## 8. Frozen Metrics and Success Interpretation

Primary metrics:

1. Incremental Onboarding Utility (IOU): mean selected reward minus the matched frozen-incumbent reward.
2. Challenger Regret: gap to the per-context oracle max(r_inc, r_new).
3. Beneficial-adoption precision: P(Delta(x)>0 | challenger selected).
4. Beneficial-adoption recall: fraction of positive challenger contexts actually selected.

Safety/diagnostic metrics:

- challenger selection rate;
- harmful-adoption rate;
- cumulative IOU/regret at early checkpoints.

A method is not considered successful merely because it reduces challenger usage. The intended evidence is positive IOU together with materially higher beneficial-adoption precision, while preserving non-trivial recall. BBB is retained as a hard sparse-opportunity stress test; a near-zero result there may be diagnostically useful, but negative IOU is not relabeled as a win.

## 9. Freeze Boundary

Task31 freezes benchmark set, incumbents, splits, feedback semantics, main baseline families/parameters, and metrics. Any later change to these items requires a new task and an explicit protocol version. Ours must be designed against this frozen setting rather than changing the evaluation after seeing its results.
