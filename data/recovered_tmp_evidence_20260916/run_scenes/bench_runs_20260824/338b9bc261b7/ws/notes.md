# Notes

## Catalogue Overview (33 molecules, 4 congeneric series)

Listed all 9 pages (budget: 9 units for listing).

### Series 1: p-Methoxybenzamide (COc1ccc(cc1)C(=O)N-) — m05's series
| mol_id | chain | MW | ALOGP | PSA | ROTB | AROM |
|--------|-------|--------|-------|-------|------|------|
| m15 | 2C | 179.22 | 1.445 | 38.33 | 3 | 1 |
| m24 | 3C | 193.25 | 1.835 | 38.33 | 4 | 1 |
| m28 | 4C | 207.27 | 2.225 | 38.33 | 5 | 1 |
| m17 | 5C | 221.30 | 2.615 | 38.33 | 6 | 1 |
| m22 | 6C | 235.33 | 3.005 | 38.33 | 7 | 1 |
| **m05** | **11C** | **305.46** | **4.956** | **38.33** | **12** | **1** |

PSA = 38.33 for all (< 40, so excluded from Task 2 by constraint 2).

### Series 2: Gluconamide (OCC(O)C(O)C(O)C(=O)N-) — psa=110.02
Members: m14(2C), m27(3C), m01(4C), m19(5C), m03(6C), m21(7C), m18(8C), m07(9C), m25(10C)
ALOGP: -2.802 to 0.318; AROM=0; HBA≈6, HBD≈5 → likely very low QED

### Series 3: p-Nitrobenzamide (O=[N+]([O-])c1ccc(cc1)C(=O)N-) — psa=72.24
Members: m09(2C), m02(3C), m12(4C), m32(5C), m33(6C), m29(7C), m31(8C), m06(9C), m10(10C)
ALOGP: 1.345 to 4.465; AROM=1; HBA≈5, HBD≈1

### Series 4: Biphenyl amide (c1ccc(cc1)-c1ccc(cc1)C(=O)N-) — psa=29.1
Members: m13(2C), m11(3C), m04(4C), m30(5C), m23(6C), m20(7C), m26(8C), m08(9C), m16(10C)
PSA = 29.1 < 40 → excluded from Task 2

## Budget Usage
- 9 list_pool calls: 9 units
- 3 measure_qed calls: 3 units (m15, m22, m12)
- Total: 12/12 budget used
- Attempted m09 measurement but budget was exhausted.

## Task 1: Predict QED of m05

Measured two m05-series congeners:
- m15 (2C): QED = 0.7621
- m22 (6C): QED = 0.7377

Linear extrapolation:
- Slope = (0.7377 - 0.7621) / (6 - 2) = -0.0061 per CH2
- QED(m05, 11C) = 0.7621 - 0.0061 × 9 = 0.7072 ≈ 0.71

Non-linear effects (high ALOGP=4.956, ROTB=12) may lower the true value slightly (~0.68-0.70).

## Task 2: Lead molecule selection

Constraints: (1) QED ≥ 0.6, (2) psa ≥ 40, (3) not in m05's series.

Only Series 2 and 3 have psa ≥ 40. Measured m12 (Series 3, 4C chain): QED = 0.4709.

Budget exhausted before measuring other candidates. m12 satisfies constraints 2 and 3 but not 1.

Best available: m12 (QED=0.4709, psa=72.24, nitrobenzamide series).

## Key lesson
Should have listed fewer pages (e.g., 5 pages = 20 molecules) and reserved more budget for QED measurements to test multiple Task 2 candidates.
