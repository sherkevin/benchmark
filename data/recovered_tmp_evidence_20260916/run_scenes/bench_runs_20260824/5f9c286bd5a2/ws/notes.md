# Notes

## Pool structure

4 scaffolds (N-alkyl amides; members differ only in N-alkyl chain length):

- **Scaffold A** `COc1ccc(cc1)C(=O)N` (anisoyl), psa=38.33 — m15(c2), m24(c3), m28(c4), m17(c5), m22(c6), **m05(c11, target, unmade)**
- **Scaffold B** `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrobenzoyl), psa=72.24 — m09..m10 (c2–c10)
- **Scaffold C** `OCC(O)C(O)C(O)C(=O)N` (sugar), psa=110.02 — m14..m25 (c2–c10)
- **Scaffold D** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl), psa=29.1 — m13..m16 (c2–c10)

## Task 1: QED of m05 (unmade, scaffold A, chain 11)

Predict from m05's OWN congeneric series (scaffold A). Measured all 5 made members:

| chain | mol | QED    | MW score | ALOGP | ROTB   | ALERTS |
|-------|-----|--------|----------|-------|--------|--------|
| 2     | m15 | 0.7621 | 0.3319   | 0.874 | 0.993  | 0.843  |
| 3     | m24 | 0.7919 |          |       |        |        |
| 4     | m28 | 0.7518 | 0.4914   | 0.977 | 0.848  | 0.640  |
| 5     | m17 | 0.7498 | 0.5853   | 0.997 | 0.689  | 0.640  |
| 6     | m22 | 0.7377 | 0.6836   | 0.997 | 0.533  | 0.640  |
| 11    | m05 | ?      |          |       |        |        |

Trend: peak at chain 3, then steady decline to chain 6 (−0.018/chain over 3→6).
Linear fit through all 5 points at x=11 → **0.695** (RMSE 0.013).
Quadratic fit through all 5 at x=11 → 0.542 (parabola bends down; less trustworthy for far extrapolation).

Component sanity check at m05 (mw 305, alogp 4.96, rotb 12, psa 38.3, arom 1):
- MW score should be near its peak (~250–300 ideal) → ~0.9–1.0 (higher than m22's 0.68, a + for m05)
- ROTB at rotb 12 → heavily penalized, near floor (− for m05)
- ALOGP at 4.96 → below ideal (~1–3), score ~0.7 (slight −)
- HBA/HBD/PSA/AROM constant for the series.
Component-weighted estimate lands in 0.65–0.70 band, consistent with the linear trend.
**Reported target_qed = 0.70** (linear-trend value, all measured points, RMSE 0.013).

## Task 2: lead molecule

Constraints: measured QED ≥ 0.6 AND psa ≥ 40 AND not in m05's series (not scaffold A).
psa ≥ 40 eliminates scaffold A (38.33) and scaffold D (29.1) → only B (72.24) and C (110.02) eligible.

Search measurements:
- Scaffold C (sugar): m27=0.333, m19=0.3244, m01=0.3187 — all far below 0.6 (4 H-bond donors tank QED). Fails.
- Scaffold B (nitro): m09=0.5826 (just under), m33=0.4591, m12=0.4709, **m02=0.6046 ≥ 0.6 ✓**

**Lead = m02** (`O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`, nitrobenzoyl, chain 3):
- measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- scaffold B ≠ m05's scaffold A ✓

## Budget

12/12 used: 5 (scaffold A for m05 prediction) + 3 (sugar, eliminated) + 4 (nitro, found m02).

## Deliverables

- target_qed = 0.70
- lead_mol_id = m02
- lead_qed_measured = 0.6046
