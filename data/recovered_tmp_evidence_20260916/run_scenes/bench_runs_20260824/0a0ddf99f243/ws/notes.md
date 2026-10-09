# Notes

## Task Overview
Predict QED of m05 (unsynthesized) and find a lead molecule satisfying QED≥0.6, PSA≥40, and not in m05's congeneric series.

## Congeneric Series Identification

**m05 series** (scaffold: `COc1ccc(cc1)C(=O)N-`, methoxybenzamide):
- m15 (2C), m24 (3C), m28 (4C), m17 (5C), m22 (6C), m05 (11C)

**Other series:**
- Nitrobenzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N-`: m09, m02, m12, m32, m33, m29, m31, m06, m10 (PSA=72.24)
- Biphenyl-amide `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`: m13, m11, m04, m30, m23, m20, m26, m08, m16 (PSA=29.1 — excluded, PSA<40)
- Sugar alcohol `OCC(O)C(O)C(O)C(=O)N-`: m14, m27, m01, m19, m03, m21, m18, m07, m25 (PSA=110.02)

## Task 1: Predict QED of m05

### Measured QED values in the m05 series:
| mol_id | Chain | alogp | rotb | QED    |
|--------|-------|-------|------|--------|
| m15    | 2C    | 1.445 | 3    | 0.7621 |
| m24    | 3C    | 1.835 | 4    | 0.7919 |
| m28    | 4C    | 2.225 | 5    | 0.7518 |
| m17    | 5C    | 2.615 | 6    | 0.7498 |
| m22    | 6C    | 3.005 | 7    | 0.7377 |
| m05    | 11C   | 4.956 | 12   | ???    |

### Trend analysis:
- QED peaks at 3C then decreases with chain length
- Per-CH2 changes: +0.030 (2→3), -0.040 (3→4), -0.002 (4→5), -0.012 (5→6)
- Linear regression (all 5 pts): slope = -0.00909/CH2, predicts QED(11C) ≈ 0.695
- Linear regression (3C–6C): slope = -0.01646/CH2, predicts QED(11C) ≈ 0.651
- Exponential decay (3C–6C): rate = 0.9766/CH2, predicts QED(11C) ≈ 0.655

m05 has alogp=4.956 (high) and rotb=12 (high), both penalizing QED nonlinearly.
Using the 3C–6C linear regression as the primary model: **QED(m05) ≈ 0.65**

## Task 2: Lead Selection

### Constraints: QED≥0.6, PSA≥40, not in m05 series
Biphenyl series excluded (PSA=29.1 < 40). Candidates from nitro and sugar series.

### Measured candidates:
| mol_id | Series | Chain | PSA    | QED    | Qualifies? |
|--------|--------|-------|--------|--------|------------|
| m09    | nitro  | 2C    | 72.24  | 0.5826 | No         |
| m02    | nitro  | 3C    | 72.24  | 0.6046 | **YES** ✓  |
| m12    | nitro  | 4C    | 72.24  | 0.4709 | No         |
| m32    | nitro  | 5C    | 72.24  | 0.4683 | No         |
| m33    | nitro  | 6C    | 72.24  | 0.4591 | No         |
| m01    | sugar  | 4C    | 110.02 | 0.3187 | No         |
| m03    | sugar  | 6C    | 110.02 | 0.3282 | No         |

### Selected lead: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- Measured QED: 0.6046 (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Different scaffold from m05 ✓

## Budget: 12/12 measurements used
