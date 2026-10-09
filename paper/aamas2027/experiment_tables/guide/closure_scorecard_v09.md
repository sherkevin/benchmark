# Closure scorecard v09 — TEMPLATE / NOT SIGNED

This file is the machine-assisted scoring form required by Guide v09. It is not a current PASS decision. Fill it only after real E1–E6 runs, independent recovery, and the freeze receipts are complete.

- Guide source: `versions/v09_20261008/guide.tex`
- Effective standard: `docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md` v006
- Main artifact: `paper/aamas2027/main.tex#tab:confirmation`
- Candidate method id: `mira_context_reliability`
- Primary Bayesian router id: `bayesianrouter`
- E6 cohort: four fresh providers × final sealed scaffold-disjoint confirmation cohort (preferred 157 only with exposure receipt)

## Hard conditions

| ID | Status (PASS/FAIL/UNKNOWN) | Evidence path | SHA-256 / log line | Gap and repair |
|---|---|---|---|---|
| H1 | PENDING |  |  |  |
| H2 | PENDING |  |  |  |
| H3 | PENDING |  |  |  |
| H4 | PENDING |  |  |  |
| H5 | PENDING |  |  |  |
| H6 | PENDING |  |  |  |
| H7 | PENDING |  |  |  |
| H8 | PENDING |  |  |  |
| H9 | PENDING |  |  |  |
| H10 | PENDING |  |  |  |

## B1–B15 rubric

Score each item 0–4 using v006. `3` requires a replayable contract, raw evidence, and independent review; `4` additionally requires pressure/negative evidence and explicit claim scope.

| ID | Score author | Score replicator | Effective=min | Evidence path/hash | Gap / repair |
|---|---:|---:|---:|---|---|
| B1 |  |  |  |  |  |
| B2 |  |  |  |  |  |
| B3 |  |  |  |  |  |
| B4 |  |  |  |  |  |
| B5 |  |  |  |  |  |
| B6 |  |  |  |  |  |
| B7 |  |  |  |  |  |
| B8 |  |  |  |  |  |
| B9 |  |  |  |  |  |
| B10 |  |  |  |  |  |
| B11 |  |  |  |  |  |
| B12 |  |  |  |  |  |
| B13 |  |  |  |  |  |
| B14 |  |  |  |  |  |
| B15 |  |  |  |  |  |

- B1–B8 minimum: 3 each.
- B9–B15 minimum: 2 each and no zero.
- Total minimum: 49/60.

## Top-level gates

The `Gate-*` names below are deliberately different from the B1–B15 rubric IDs.

| Gate subitem | Score 0–4 | Evidence path/hash | Decision note |
|---|---:|---|---|
| Gate-A1 public truth |  |  |  |
| Gate-A2 routing protocol |  |  |  |
| Gate-A3 split/confirmation independence |  |  |  |
| Gate-A4 provider lineage/family correlation |  |  |  |
| Gate-B1 baseline role coverage |  |  |  |
| Gate-B2 task/dynamic-scope match |  |  |  |
| Gate-B3 source/adapter fidelity |  |  |  |
| Gate-B4 fairness and common contract |  |  |  |
| Gate-C1 experiment coverage |  |  |  |
| Gate-C2 common-contract closure |  |  |  |
| Gate-C3 direction/utility/claim scope |  |  |  |
| Gate-C4 uncertainty/failure/recovery |  |  |  |

Gate A/B/C each require all four subitems ≥3 and total ≥12/16. A, B and C cannot compensate for one another.

## Experiment closure

| Experiment | Raw/config/hash | Independent recovery | Target check | Status |
|---|---|---|---|---|
| E1 task validity |  |  | Best Fixed−Random ≥+2.0 pp, CI lower ≥0, ≥3/5 folds ≥+1.0 pp | PENDING |
| E2 primary comparison |  |  | MIRA−matched Online-SGD ≥+2.0 pp, CI lower ≥+0.5 pp | PENDING |
| E3 mechanism ablation |  |  | Full−mira_relative_reliability ≥+1.0 pp, CI lower ≥0 | PENDING |
| E4 dynamic/pressure |  |  | Pooled E2 margin and each D∈{0,1,4} ≥+1.0 pp after Holm | PENDING |
| E5 errors/cost/recovery |  |  | Cost/failure/utility contract | PENDING |
| E6 frozen confirmation |  |  | One-shot final sealed Stage-B receipt with exposure receipt | PENDING |

## Final decision

- H1–H10: **PENDING**
- B total: **PENDING / 60**
- Gate A: **PENDING / 16**
- Gate B: **PENDING / 16**
- Gate C: **PENDING / 16**
- Author recommendation: **PENDING**
- Replicator recommendation: **PENDING**
- Machine status: **PENDING**

## Primary statistical contract (must be filled before scoring)

- E1: outer-fold equal-weight paired molecule/scaffold effect; Best Fixed vs Random.
- E2: stream/order-stratum equal-weight MIRA vs `matched_online_sgd`; controlled primary.
- E3: same contract MIRA vs `mira_relative_reliability`; only context-relative residual differs.
- E4: same primary comparator by delay; Holm--Bonferroni q=.05 across three delays.
- CI: paired scaffold-cluster bootstrap, 10,000 draws, fixed seed; no event-row bootstrap.
- Candidate checker: `run_id`, `method_id`, contract/input/source hashes, primary comparator, exposure status.
- E6: final sealed exposure receipt required; `previously_inspected` is not final E6 without a decision-impact audit.
