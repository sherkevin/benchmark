# Task notes

## Goal
- Task 1: predict QED of `m05` (chain_len 11, never synthesized, probe_unavailable).
  Predict from its OWN congeneric series (scaffold `COc1ccc(cc1)C(=O)N` = methoxybenzamide).
- Task 2: pick ONE molecule: measured QED >= 0.6, psa >= 40, NOT in m05's series.

## Budget: 12 total. Final usage = 7 (5 Task1 + 1 list_pool page1 + 1 Task2 measure). Left: 5.

## Tool calls (in order)
1. group_by_scaffold (free) -> 4 scaffold groups, m05 in group 1 at len 11.
2. measure_qed m15,m24,m28,m17,m22 (5 units) + m05 (probe_unavailable, free).
3. fit_trend degree 1 & 2 on all 5 pts; qed_components on m15/m24/m22 (all free).
4. fit_trend deg 1/2 on len 3-6 only, deg 3 on all 5 (free) -> sensitivity check.
5. list_pool page 1 (1 unit) -> descriptors for m01-m04.
6. measure_qed m02 (1 unit) -> 0.6046.
7. qed_components m02 (free).

## Task 1 — m05's series (methoxybenzamide scaffold, group 1)
Measured QED vs chain_len:
| mol | len | QED    |
|-----|-----|--------|
| m15 | 2   | 0.7621 |
| m24 | 3   | 0.7919 |  <- peak
| m28 | 4   | 0.7518 |
| m17 | 5   | 0.7498 |
| m22 | 6   | 0.7377 |
| m05 | 11  | probe_unavailable (target)

Series peaks at len 3 then declines. len 11 is far beyond the measured range (max len 6).

### Fits (free), prediction at x=11
| fit            | pts      | pred    | RMSE     |
|----------------|----------|---------|----------|
| deg 1, all 5   | 2..6     | 0.6950  | 0.01308  |
| deg 2, all 5   | 2..6     | 0.5416  | 0.01188  |  <- chosen (best in-sample, default degree)
| deg 1, 3..6    | 3..6     | 0.6508  | 0.00883  |
| deg 2, 3..6    | 3..6     | 0.9378  | 0.00539  | (extrapolates upward - implausible)
| deg 3, all 5   | 2..6     | 2.1323  | 0.00835  | (overfits, nonsense)

### Choice: target_qed = 0.5416 (degree-2 through all 5 series points).
Reasoning: fit_trend's default degree is 2; this fit has the lowest in-sample RMSE among
sensible fits AND is the only one that represents the genuine peak-at-len-3 + accelerating
decline seen in the data. QED component analysis (free) supports continued decline to len 11:
- ROTB score drops 0.993 (len2) -> 0.533 (len6); at len 11 (~13 rot bonds) it will be far lower.
- ALERTS drops to 0.640 by len 6 and keeps falling.
- ALOGP rises toward ideal then falls off as logP climbs with 5 more methylenes.
So a value well below the linear extrapolation (0.695) is justified; 0.5416 is defensible.
(The deg-2-on-3..6 fit yielding 0.94 and the cubic yielding 2.13 are overfit artifacts and
discarded as implausible for a drug-likeness score bounded in [0,1].)

## Task 2 — lead selection
Need: measured QED >= 0.6, psa >= 40, scaffold != m05's (methoxybenzamide).

### Candidate screen from list_pool page 1 (m01-m04)
| mol | scaffold(group)      | mw    | alogp  | psa    | rotb | arom |
|-----|----------------------|-------|--------|--------|------|------|
| m01 | sugar (g3)          | 221.25| -2.022 | 110.02 | 7    | 0    |
| m02 | nitrobenzamide (g2) | 208.22| 1.735  | 72.24  | 4    | 1    |
| m03 | sugar (g3)          | 249.31| -1.242 | 110.02 | 9    | 0    |
| m04 | biphenyl amide (g4) | 253.34| 3.884  | 29.10  | 5    | 2    |

- m04: psa 29.1 < 40 -> FAILS constraint 2. (biphenyl amide has only 1 amide N+O.)
- m01/m03 (sugar): alogp ~ -2 (very polar) -> QED-penalized via ALOGP score.
- m02 (nitrobenzamide): alogp 1.735 (near-ideal), low rotb 4, psa 72.24 >= 40, arom 1.
  Most drug-like profile of the candidates -> best shot at QED >= 0.6. Picked.

### m02 measured QED = 0.6046 (>= 0.6 PASS). Budget after: 7/12.
qed_components(m02): MW 0.4975, ALOGP 0.9223, HBA 0.9848, HBD 0.9866, PSA 0.9302,
ROTB 0.9697, AROM 0.8273, ALERTS 0.2414. ALERTS (nitro flag) + MW are the main drags but
overall still clears 0.6.

### Constraint check for m02
1. measured QED 0.6046 >= 0.6  -> PASS
2. psa 72.24 >= 40            -> PASS
3. scaffold = nitrobenzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N` != m05's methoxybenzamide
   `COc1ccc(cc1)C(=O)N` -> NOT same congeneric series -> PASS
All three constraints satisfied.

## Final answers (written to answer.json)
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
