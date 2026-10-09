# Notes — QED Prediction and Lead Selection

## Congeneric Series Identification

**m05** has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — a methoxybenzamide scaffold with an N-alkyl chain of length 11.

**Series A (methoxybenzamide, `COc1ccc(cc1)C(=O)N-R`)** — same scaffold as m05:
| mol_id | chain length | alogp | QED (measured) |
|--------|-------------|-------|----------------|
| m15    | 2           | 1.445 | 0.7621         |
| m24    | 3           | 1.835 | 0.7919         |
| m28    | 4           | 2.225 | 0.7518         |
| m17    | 5           | 2.615 | 0.7498         |
| m22    | 6           | 3.005 | 0.7377         |

**Other series (not same scaffold as m05):**
- Series B (nitrobenzamide): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Series C (biphenyl carboxamide): m04, m08, m11, m13, m16, m20, m23, m26, m30
- Series D (sugar-acid amide): m01, m03, m07, m14, m18, m19, m21, m25, m27

## Job 1 — QED Prediction for m05

### Observed Trend
QED rises from C2→C3 (0.762→0.792), peaks at C3, then steadily declines through C6 (0.738). The relationship is non-linear (concave-down curvature).

### Fitted Model
**Quadratic fit (degree=2) on all 5 points:**
- Coefficients (low to high): 0.74932, 0.017024, −0.003264
- RMSE on training points: 0.01188
- Predicted QED at chain length 11: **0.5416**

For comparison:
- Linear fit on all 5 points: predicted 0.695 (RMSE=0.013) — ignores the curvature
- Linear fit on C4–C6 only: predicted 0.704 (RMSE=0.002) — ignores the early peak

The quadratic is the most appropriate because:
1. The task explicitly says "do not assume the relationship is a straight line"
2. The data clearly shows curvature (rise then fall)
3. It has the best RMSE among full-range fits

### Extrapolation
m05 at chain length 11 is 5 units beyond the longest measured chain (C6). The quadratic model predicts continued decline, reaching QED ≈ **0.54** at C11. This is chemically reasonable: m05 has alogp=4.956, well into the range where QED penalizes high lipophilicity.

## Job 2 — Lead Selection

### Pre-screening (free constraints)
- **psa ≥ 40**: Eliminates all Series C molecules (psa=29.1). Series B (psa=72.24) and Series D (psa=110.02) pass.
- **Not in Series A**: Eliminates m15, m17, m22, m24, m28.

Remaining candidates: 9 from Series B, 9 from Series D.

### Measurements (budget: 12 total, 5 used for Job 1, 7 remaining)

**Series B (nitrobenzamide):**
| mol_id | chain | alogp | QED    | passes? |
|--------|-------|-------|--------|---------|
| m09    | 2     | 1.345 | 0.5826 | No (QED < 0.6) |
| m02    | 3     | 1.735 | 0.6046 | **Yes** |
| m12    | 4     | 2.125 | 0.4709 | No (QED < 0.6) |
| m32    | 5     | 2.515 | 0.4683 | No (QED < 0.6) |

**Series D (sugar-acid amide):**
| mol_id | chain | alogp  | QED    | passes? |
|--------|-------|--------|--------|---------|
| m14    | 2     | -2.802 | 0.3237 | No (QED < 0.6) |
| m01    | 4     | -2.022 | 0.3187 | No (QED < 0.6) |
| m19    | 5     | -1.632 | 0.3244 | No (QED < 0.6) |

### Chosen Lead: **m02**
- QED measured: 0.6046 ≥ 0.6 ✓
- PSA: 72.24 ≥ 40 ✓
- Series: B (nitrobenzamide) — not same as m05 ✓

m02 is the only molecule measured that satisfies all three constraints simultaneously. Series B QED peaks at short chain lengths (C3) and declines; Series D QED is uniformly low (~0.32) due to very negative logP values.

## Budget Summary
- 5 measurements for Series A (Job 1): m15, m24, m28, m17, m22
- 7 measurements for lead search (Job 2): m09, m02, m12, m32, m14, m01, m19
- Total: 12 / 12
