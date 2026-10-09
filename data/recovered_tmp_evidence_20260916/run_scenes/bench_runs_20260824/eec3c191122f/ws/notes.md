# Notes: QED Prediction and Lead Selection

## Congeneric Series Identification

The target m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — a **methoxyphenyl amide** scaffold with an 11-carbon N-alkyl chain.

I identified 4 congeneric series in the pool (33 molecules total):

### Series A — Methoxyphenyl (m05's series): `COc1ccc(cc1)C(=O)N–`
| mol_id | Chain length | alogp | PSA |
|--------|-------------|-------|-----|
| m15 | 2 | 1.445 | 38.33 |
| m24 | 3 | 1.835 | 38.33 |
| m28 | 4 | 2.225 | 38.33 |
| m17 | 5 | 2.615 | 38.33 |
| m22 | 6 | 3.005 | 38.33 |
| **m05** | **11** | **4.956** | **38.33** |

Congener IDs: **m15, m24, m28, m17, m22**

### Series B — Nitrophenyl: `O=[N+]([O-])c1ccc(cc1)C(=O)N–`
m09(C2), m02(C3), m12(C4), m32(C5), m33(C6), m29(C7), m31(C8), m06(C9), m10(C10)

### Series C — Sugar-like: `OCC(O)C(O)C(O)C(=O)N–`
m14(C2), m27(C3), m01(C4), m19(C5), m03(C6), m21(C7), m18(C8), m07(C9), m25(C10)

### Series D — Biphenyl: `c1ccc(cc1)-c1ccc(cc1)C(=O)N–`
m13(C2), m11(C3), m04(C4), m30(C5), m23(C6), m20(C7), m26(C8), m08(C9), m16(C10)

---

## Job 1: Predicting QED of m05

### Measurements on m05's series
| mol_id | Chain (x) | QED (y) |
|--------|-----------|---------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Observed trend
QED peaks at chain length 3 (0.7919) then declines monotonically from C4 to C6. The relationship is **non-linear** — a simple linear model misses the C2→C3 rise.

### Fitted model (degree-2 polynomial)
Using `fit_trend` with all 5 measured points:

**QED(x) = 0.74932 + 0.017024·x − 0.003264·x²**

- RMSE on training points: 0.01188
- The quadratic term is negative, capturing the peak-then-decline behavior

### Extrapolation to m05 (chain length 11)
QED(11) = 0.74932 + 0.017024×11 − 0.003264×121
         = 0.74932 + 0.18726 − 0.39494
         = **0.5416**

For comparison, a linear fit gives 0.695 (RMSE 0.0131, worse fit). The degree-2 model better captures the observed curvature. The decline is chemically reasonable: very long alkyl chains push alogp to ~5 (m05's alogp = 4.956), which heavily penalizes QED.

**Predicted QED for m05: ~0.54**

---

## Job 2: Lead Selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05's series (not methoxyphenyl)

### Screening
- PSA screen (free): 18 molecules pass (PSA ≥ 40)
- Removing m05's series members (all have PSA = 38.33, so none passed anyway): 18 remain
- Biphenyl series members all have PSA = 29.1, so none pass either
- Remaining candidates: nitrophenyl (9) + sugar-like (9)

### Measurement strategy
With 3 measurements left, I prioritized short-chain nitrophenyl members (moderate alogp, low MW, few rotatable bonds — best drug-likeness profile):

| mol_id | Chain | alogp | QED measured | Pass? |
|--------|-------|-------|-------------|-------|
| m09 | 2 | 1.345 | 0.5826 | No (< 0.6) |
| m02 | 3 | 1.735 | **0.6046** | **Yes** |
| m12 | 4 | 2.125 | 0.4709 | No (< 0.6) |

### Selected lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Measured QED: 0.6046 ≥ 0.6 ✓
- PSA: 72.24 ≥ 40 ✓
- Series: nitrophenyl (not m05's methoxyphenyl series) ✓

All three constraints satisfied.
