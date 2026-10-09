# Notes — QED Prediction and Lead Selection

## Series Identification

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, scaffold `COc1ccc(cc1)C(=O)N`, chain length 11.

**Four congeneric series identified in the pool (33 molecules):**

| Series | Scaffold | Members |
|--------|----------|---------|
| A (methoxy-benzamide) | `COc1ccc(cc1)C(=O)N` | m15(2), m24(3), m28(4), m17(5), m22(6) + target m05(11) |
| B (sugar-like) | `OCC(O)C(O)C(O)C(=O)N` | m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) |
| C (nitro-benzamide) | `O=[N+]([O-])c1ccc(cc1)C(=O)N` | m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) |
| D (biphenyl) | `c1ccc(cc1)-c1ccc(cc1)C(=O)N` | m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10) |

## Job 1 — QED Prediction for m05

### Measured congeners (same scaffold as m05):

| mol_id | chain_len | QED (measured) |
|--------|-----------|----------------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Trend observed
QED peaks at chain 3 (0.7919), then declines steadily: 0.7518 → 0.7498 → 0.7377. The decline accelerates slightly, suggesting curvature.

### Fit (degree 2, all 5 points):
`QED(chain) = 0.74932 + 0.017024·chain − 0.003264·chain²`
RMSE = 0.01188

### Extrapolation to chain 11:
`QED(11) = 0.5416`

### Alternative fits explored:
- Linear (degree 1, all 5 points): 0.695 (RMSE 0.01308)
- Quadratic (chains 3–6 only): 0.9378 (unrealistic — curves upward)
- Linear (chains 3–6 only): 0.6508
- Quadratic (chains 4–6 only): 0.5257

The degree-2 fit on all 5 points is the most principled: it uses all available data, has the best RMSE among full-range fits, and the downward curvature is physically reasonable (QED penalizes high lipophilicity from long chains). **Prediction: 0.5416**.

## Job 2 — Lead Compound Selection

### Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same series as m05

### Approach
The m05 series has PSA = 38.33 (< 40), so all PSA-passing candidates are automatically not in the m05 series. From `screen_candidates()`, 18 molecules pass PSA ≥ 40.

I prioritized the nitro-benzamide series (series C, PSA=72.24) with moderate alogp values, as these resemble drug-like molecules most closely.

### Measurements:
| mol_id | series | alogp | QED (measured) | Pass? |
|--------|--------|-------|----------------|-------|
| m02 | C | 1.735 | 0.6046 | ✓ |
| m12 | C | 2.125 | 0.4709 | ✗ (QED < 0.6) |
| m09 | C | 1.345 | 0.5826 | ✗ (QED < 0.6) |

### Selected lead: **m02**
- QED = 0.6046 (≥ 0.6 ✓)
- PSA = 72.24 (≥ 40 ✓)
- Series C (nitro-benzamide), not in m05's series ✓

## Budget Usage
- Paging: 9 units (pages 1–9)
- Congener measurements: 5 units (m15, m24, m28, m17, m22)
- Lead candidate measurements: 3 units (m02, m12, m09)
- Total: 17/17
