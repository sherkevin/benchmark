# Notes: predict m05 QED & pick a backup lead

## Pool layout (33 molecules), grouped by scaffold

Target **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — 4-methoxybenzoyl amide, N-alkyl chain = **11 carbons**. psa 38.33, arom 1.

### Series A — methoxybenzoyl amide `COc1ccc(cc1)C(=O)N` (m05's series), arom 1, psa 38.33
- m15: NCC  → 2C
- m24: NCCC → 3C
- m28: NCCCC → 4C
- m17: NCCCCC → 5C
- m22: NCCCCCC → 6C
- m05: NCCCCCCCCCCC → 11C  (TARGET, never made)

Pool only holds short-chain members (2–6C) → must EXTRAPOLATE to 11C.

### Series B — nitrobenzoyl amide `O=[N+]([O-])c1ccc(cc1)C(=O)N`, arom 1, psa 72.24
- m09: 2C, m02: 3C, m12: 4C, m32: 5C, m33: 6C, m29: 7C, m31: 8C, m06: 9C, m10: 10C

### Series C — sugar/glycamide `OCC(O)C(O)C(O)C(=O)N`, arom 0, psa 110.02
- m14: 2C, m27: 3C, m01: 4C, m19: 5C, m21: 7C, m18: 8C, m07: 9C, m25: 10C

### Series D — biphenylcarbonyl amide `c1ccc(cc1)-c1ccc(cc1)C(=O)N`, arom 2, psa 29.1
- m13: 2C, m11: 3C, m04: 4C, m30: 5C, m23: 6C, m20: 7C, m26: 8C, m08: 9C, m16: 10C
(All fail psa≥40 → not lead-eligible.)

## Budget
Total 17, shared. Used 9 paging the catalogue. **8 left** for measurements.

## Job 1 — measured m05's series (methoxybenzoyl, chain 2–6) and extrapolated to 11

Measurements (chain length → QED):
- m15 (2C): 0.7621
- m24 (3C): 0.7919   ← peak
- m28 (4C): 0.7518
- m17 (5C): 0.7498
- m22 (6C): 0.7377

Trend: QED rises to a peak at chain ~3 then declines; the decline accelerates as the
lipophilic tail grows (long N-alkyl chain hurts drug-likeness — MW, ALogP climb,
rotatable bonds climb). m05 has chain 11, far beyond the 2–6 measured range, so this is
extrapolation, not interpolation.

Fits at x_query = 11:
- degree 2: prediction **0.5416**, coeffs [0.74932, 0.017024, -0.003264], RMSE 0.01188
- degree 1: prediction 0.6950, slope -0.00909, RMSE 0.01308

Degree 2 fits the measured points better (lower RMSE) and captures the downward
curvature that the data show (rise then accelerating fall). The quadratic's negative
x² term makes the predicted drop steeper as the chain grows past the peak, which is the
physically expected behaviour for a long lipophilic tail. **Adopt 0.5416 as m05 QED.**
(Line fit under-predicts the curvature, so it is too optimistic at 11.)

**m05 predicted QED = 0.5416**

## Job 2 — lead picked: m02

Strategy: constraints 2 (psa≥40) and 3 (not m05's series) are free, check first.
psa≥40 screen passes: m01,m02,m03,m06,m07,m09,m10,m12,m14,m18,m19,m21,m25,m27,m29,m31,m32,m33.
All methoxybenzoyl (m05's) series members have psa 38.33 < 40, so they're excluded by the
psa gate — constraint 3 is automatically satisfied for everything that passes the screen.
Remaining eligible scaffolds: nitrobenzoyl (psa 72) and sugar (psa 110).

QED weighs PSA and MW/polarity; lowest-PSA, lowest-MW candidates are most promising.
Nitrobenzoyl short chains are lowest MW among psa≥40 set. Measured the two shortest:
- m09 (nitrobenzoyl, 2C, MW 194): QED 0.5826 — just under 0.6 ✗
- m02 (nitrobenzoyl, 3C, MW 208): QED **0.6046** ≥ 0.6 ✓

m02 satisfies all three: QED 0.6046 ≥ 0.6 ✓; psa 72.24 ≥ 40 ✓; not m05's series (nitro-
benzoyl scaffold, not methoxybenzoyl) ✓. Lead = m02, measured QED 0.6046.

Budget: 9 (paging) + 5 (series) + 2 (lead candidates) = 16 of 17 used; 1 spare kept in reserve.
