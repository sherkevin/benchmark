# Task notes — QED prediction + lead pick

## Pool catalog (33 molecules, 4 congeneric series)

All series are `ArC(=O)N(alkyl chain)` benzamides (or sugar-amide); the only thing
that varies within a series is the N-alkyl chain length.

### Series A — para-METHOXY benzamide (m05's series, the TARGET series)
Scaffold: `COc1ccc(cc1)C(=O)N-`
- m15  `COc1ccc(cc1)C(=O)NCC`            C2   psa 38.33
- m24  `COc1ccc(cc1)C(=O)NCCC`           C3   psa 38.33
- m28  `COc1ccc(cc1)C(=O)NCCCC`          C4   psa 38.33
- m17  `COc1ccc(cc1)C(=O)NCCCCC`         C5   psa 38.33
- m22  `COc1ccc(cc1)C(=O)NCCCCCC`        C6   psa 38.33
- m05  `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`   C11  psa 38.33  ← TARGET, never made

### Series B — para-NITRO benzamide
Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N-`
- m09 C2, m02 C3, m12 C4, m32 C5, m33 C6, m29 C7, m31 C8, m06 C8, m10 C9   (psa 72.24)

### Series C — BIPHENYL benzamide
Scaffold: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`
- m13 C2, m11 C3, m04 C3, m30 C4, m23 C5, m20 C6, m26 C7, m08 C8, m16 C9   (psa 29.1)

### Series D — sugar amide `OCC(O)C(O)C(O)C(=O)N-`
- m14 C2, m27 C3, m01 C4, m19 C5, m03 C6, m21 C7, m18 C7, m07 C8, m25 C9   (psa 110.02)

## Job 1 — predict m05 (C11) QED
Congeners of m05 in pool (chain differs): m15(C2), m24(C3), m28(C4), m17(C5), m22(C6).
m05 is C11 → extrapolate BEYOND the measured C2–C6 range. Fit degree-2 trend.

## Job 2 — lead, three constraints
1. measured QED ≥ 0.6  (must spend a measurement)
2. psa ≥ 40  (free)
3. not in m05's series (free, from SMILES)

Constraint 2 (psa≥40) eliminates: Series A (38.33) and Series C (29.1).
Constraint 3 eliminates: Series A.
→ Eligible families: Series B (nitro, psa 72.24) and Series D (sugar, psa 110).

Series D has very low alogp (-2.8 … +0.3) and psa 110 → likely poor QED.
Series B short-chain members look most drug-like:
  m12(C4): MW222 alogp2.125 rotb5  ← most balanced (logP ~ideal 2)
  m32(C5): MW236 alogp2.515 rotb6
  m02(C3): MW208 alogp1.735 rotb4
  m09(C2): MW194 alogp1.345 rotb3 (MW/logP a bit low)
Lead search order: m12 → m32 → m02 → m09 … (stop when one clears 0.6).

## Budget
Total 17, shared. Used 9 on list_pool. Remaining 8.
Plan: 5 methoxy measurements (m15,m24,m28,m17,m22) + lead search (≤3 tries).

## Results

### Job 1 — methoxy series measured (chain length vs QED)
| chain | mol | QED    |
|-------|-----|--------|
| C2    | m15 | 0.7621 |
| C3    | m24 | 0.7919 |  ← peak
| C4    | m28 | 0.7518 |
| C5    | m17 | 0.7498 |
| C6    | m22 | 0.7377 |

Curved: rises C2→C3 (peak) then declines monotonically. Concave-down — NOT a
straight line, so degree-2 fit is the correct model (TASK warns against assuming
linearity and against quoting the nearest point).

fit_trend degree=2 on (C2..C6) → C11:
  coeffs [c0,c1,c2] = [0.74932, 0.017024, -0.003264]
  rmse on given points = 0.01188  (good fit)
  **prediction at C11 (m05) = 0.5416**
Degree-1 alternative gives 0.695 but is physically wrong: a line cannot capture
the observed peak and the subsequent monotonic decay, and it would falsely
predict a near-flat high value for an 11-carbon chain (MW 305, rotb 12, alogp 4.956
near QED's penalty edge) that is clearly less drug-like. Use 0.5416.

### Job 2 — lead search (constraint order: psa≥40 free, series≠A free, QED≥0.6 measured)
psa screen + SMILES → eligible = Series B (nitro, psa 72.24) and Series D (sugar,
psa 110). Series D excluded (alogp very negative + psa 110 → poor drug-likeness).
Tried nitro short chains (best-balanced first), stop on success:
- m12 (C4, nitro): QED 0.4709 ✗ (below 0.6)
- m02 (C3, nitro): QED **0.6046 ✓** (≥0.6)

m02 satisfies all three constraints:
1. measured QED 0.6046 ≥ 0.6 ✓
2. psa 72.24 ≥ 40 ✓
3. nitro scaffold ≠ m05's methoxy scaffold (Series B ≠ Series A) ✓
**Lead = m02, measured QED 0.6046.**

## Budget tally
9 (list_pool) + 5 (methoxy: m15,m24,m28,m17,m22) + 2 (m12, m02) = 16/17. 1 spare.
