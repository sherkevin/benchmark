# Notes — QED Prediction and Lead Selection

## Congeneric Series Identification

**m05 scaffold**: `COc1ccc(cc1)C(=O)N-` (4-methoxyphenyl amide), N-alkyl chain = 11 carbons.

Pool molecules sharing this scaffold (differ only in chain length):

| mol_id | chain length | QED (measured) |
|--------|-------------|-----------------|
| m15    | 2           | 0.7621          |
| m24    | 3           | 0.7919          |
| m28    | 4           | 0.7518          |
| m17    | 5           | 0.7498          |
| m22    | 6           | 0.7377          |
| **m05**| **11**      | *never made*    |

**congener_ids**: m15, m24, m28, m17, m22

## Trend Fitting (Job 1)

Observed pattern: QED peaks at chain 3 (0.7919), then steadily declines as chain grows longer. This is chemically sensible — longer N-alkyl chains increase MW and logP, eventually hurting drug-likeness.

### Quadratic fit (degree 2, chosen):
- Coefficients: 0.74932 + 0.017024·x − 0.003264·x²
- RMSE on training points: 0.01188
- **Prediction at x=11: 0.5416**

### Linear fit (degree 1, rejected):
- Coefficients: 0.79502 − 0.00909·x
- RMSE: 0.01308 (worse)
- Prediction at x=11: 0.695 (misses the curvature; overestimates)

### Cubic fit (degree 3, rejected):
- RMSE: 0.00835 (slightly better on training data)
- Prediction at x=11: 2.1323 (wildly extrapolates beyond 0–1 range; overfitting)

**Chosen prediction for m05: QED ≈ 0.5416** (quadratic fit, the only degree that both fits the curvature and extrapolates sensibly).

## Lead Selection (Job 2)

### Free constraint screening:
- **psa ≥ 40**: eliminates Series A (psa=38.33) and Series D (psa=29.1). Candidates come from Series B (psa=110.02) and Series C (psa=72.24).
- **Not in m05's series**: Series A eliminated (already failed psa anyway).

### QED measurements on candidates (7 remaining budget):

**Series C** (nitrophenyl, psa=72.24):
| mol_id | chain | QED    | ≥0.6? |
|--------|-------|--------|-------|
| m09    | 2     | 0.5826 | No    |
| m02    | 3     | 0.6046 | **Yes** |
| m12    | 4     | 0.4709 | No    |
| m32    | 5     | 0.4683 | No    |

**Series B** (sugar amide, psa=110.02):
| mol_id | chain | QED    | ≥0.6? |
|--------|-------|--------|-------|
| m14    | 2     | 0.3237 | No    |
| m27    | 3     | 0.3330 | No    |
| m01    | 4     | 0.3187 | No    |

Series B molecules have very low QED (~0.32) due to extremely high PSA (110.02).

### Chosen lead: **m02**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Series C (nitrophenyl), not in m05's series ✓

## Budget usage
All 12 measurements used: 5 for m05's series, 7 for lead candidates.
