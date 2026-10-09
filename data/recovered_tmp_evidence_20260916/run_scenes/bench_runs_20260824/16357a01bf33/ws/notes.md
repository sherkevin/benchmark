# Predicting QED of `m05` (unmeasurable target)

## 1. Target structure & scaffold

`m05` SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

- Scaffold = `COc1ccc(cc1)C(=O)N` = **4-methoxybenzamide** (anilide N-acylated, OMe para).
- After the amide N: `CCCCCCCCCCC` = **11-carbon (undecyl) N-alkyl chain**.
- So m05 = 4-methoxy-N-undecylbenzamide. The congeneric series shares the
  4-methoxybenzamide scaffold and differs ONLY in N-alkyl chain length.

Cheap scaffold substring `COc1ccc(cc1)C(=O)N` is therefore the right search query.

## 2. Search (2 page-units spent, 10 remain)

Query `COc1ccc(cc1)C(=O)N` → 6 total matches.

| mol_id | SMILES tail (after N) | chain C | MW    | ALogP  | PSA   | ROTB | AROM |
|--------|-----------------------|--------|-------|--------|-------|------|------|
| m15    | CC                    | 2      | 179.22| 1.445  | 38.33 | 3    | 1    |
| m24    | CCC                   | 3      | 193.25| 1.835  | 38.33 | 4    | 1    |
| m28    | CCCC                  | 4      | 207.27| 2.225  | 38.33 | 5    | 1    |
| m17    | CCCCC                 | 5      | 221.30| 2.615  | 38.33 | 6    | 1    |
| m22    | CCCCCCC               | 6      | 235.33| 3.005  | 38.33 | 7    | 1    |
| **m05**| CCCCCCCCCCC           | **11** | 305.46| 4.956  | 38.33 | 12   | 1    |

All 5 non-target members are SHORT-chain (C2–C6). m05 (C11) is far outside the
measured range → **extrapolation, not interpolation**, as the task warns.

Across the series PSA (38.33) and AROM (1) are constant; HBD=0, HBA=2,
ALERTS=0 are also constant (N is fully substituted, no toxicophore). So QED
variation is driven by the three changing properties: **MW, ALogP, ROTB**.

## 3. Measurements (5 units spent; total 7/12)

| n (chain C) | mol_id | QED    |
|-------------|--------|--------|
| 2           | m15    | 0.7621 |
| 3           | m24    | 0.7919 |
| 4           | m28    | 0.7518 |
| 5           | m17    | 0.7498 |
| 6           | m22    | 0.7377 |

`measure_qed("m05")` → probe_unavailable (never synthesized), as expected.

## 4. Trend observed

- QED rises from n=2 (0.7621) to a **peak at n=3 (0.7919)**, then declines:
  n=4 → 0.7518, n=5 → 0.7498, n=6 → 0.7377.
- So the curve is **non-monotonic (hump-shaped)**, not linear. From n=3 onward
  the trend is a steady but **decelerating** decline (per-step drops ≈ 0.040,
  0.002, 0.012), consistent with QED desirability functions approaching their
  lower floors as ALogP and ROTB move out of the ideal range.

## 5. Extrapolation to n=11

The m05 descriptors that move versus the last measured point (n=6/m22):
- MW 235 → 305 (moves **toward** the MW ideal ≈ 300, slight ↑ benefit)
- ALogP 3.005 → 4.956 (well past the ALogP ideal ≈ 2, strong ↓ penalty)
- ROTB 7 → 12 (past the ROTB ideal, ↓ penalty)

Two of the three drivers pull QED down; the ALogP jump to ~5 is the strongest
single penalty. Net: QED at n=11 is below the n=6 value of 0.7377.

Fits considered (n = chain length):
- Linear regression on the declining region n=3..6: slope ≈ −0.0165/step →
  QED(11) ≈ 0.7377 − 5×0.0165 ≈ **0.655**.
- Quadratic through all 5 points (vertex ~n=2.6, matches the observed peak):
  QED(11) ≈ **0.54**, but this parabola is concave-down (accelerating
  decline). Real desirability functions *saturate* toward a floor, i.e. the
  decline must **decelerate**, so the quadratic **overstates** the drop.
- Last-two-point slope (n=5→6, −0.0121/step): ≈ **0.677**.

The saturating/physical shape rules out the accelerating quadratic; the local
linear fits give ~0.65–0.68. Two corrections to pure linear: (a) decay should
decelerate (floor) → pull value **up**; (b) ALogP jumping from 3.0 to 4.96
enters the steeply-penalized lipophilicity zone → pull value **down**. These
partly offset. Combining the linear baseline (0.655) with the ALogP-penalty
downward bias, I predict:

### Predicted QED(m05) ≈ 0.62

This sits below the nearest measured point (0.7377 at n=6), reflecting the
continued decline, but above the (over-stated) quadratic floor, reflecting the
saturating nature of QED desirabilities.

## 6. Congeners found in the same series as m05

m15, m17, m22, m24, m28 (all 4-methoxybenzamide scaffold, C2–C6 chains).
(m05 itself is the target, not a measured congener.)
