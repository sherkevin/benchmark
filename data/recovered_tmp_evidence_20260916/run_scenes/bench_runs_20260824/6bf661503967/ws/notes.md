# Notes

## Congeneric Series Identification

**Target m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxybenzamide scaffold, 11-carbon N-alkyl chain.

### Series A: Methoxybenzamide `COc1ccc(cc1)C(=O)N-` (same series as m05)
- m15: NCC (2C), m24: NCCC (3C), m28: NCCCC (4C), m17: NCCCCC (5C), m22: NCCCCCC (6C)

### Series B: Gluconamide `OCC(O)C(O)C(O)C(=O)N-`
m14(2C), m27(3C), m01(4C), m19(5C), m03(6C), m21(7C), m18(8C), m07(9C), m25(10C)

### Series C: Nitrobenzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N-`
m09(2C), m02(3C), m12(4C), m32(5C), m33(6C), m29(7C), m31(8C), m06(9C), m10(10C)

### Series D: Biphenyl amide `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`
m13(2C), m11(3C), m04(4C), m30(5C), m23(6C), m20(7C), m26(8C), m08(9C), m16(10C)

## Job 1 — Predicting QED of m05

### Measurements of Series A (methoxybenzamide)
| mol_id | Chain C | ALogP | QED (measured) |
|--------|---------|-------|----------------|
| m15    | 2       | 1.445 | 0.7621         |
| m24    | 3       | 1.835 | 0.7919         |
| m28    | 4       | 2.225 | 0.7518         |
| m17    | 5       | 2.615 | 0.7498         |
| m22    | 6       | 3.005 | 0.7377         |

m05 (11C): ALogP = 4.956, MW = 305.46, RotB = 12, PSA = 38.33

### Trend Analysis
QED peaks at ~3C (0.7919), then declines. The decline reflects increasing penalties from:
- ALogP approaching 5 (m05 = 4.956)
- Rotatable bonds = 12 (high)
- MW = 305 (still moderate)

### Model: Quadratic Least-Squares Fit (all 5 points)
Fitted: QED = -0.003264·x² + 0.017022·x + 0.74932

Fit quality (residuals):
- x=2: predicted 0.770, actual 0.762 (err +0.008)
- x=3: predicted 0.771, actual 0.792 (err -0.021)
- x=4: predicted 0.765, actual 0.752 (err +0.013)
- x=5: predicted 0.753, actual 0.750 (err +0.003)
- x=6: predicted 0.734, actual 0.738 (err -0.004)

### Extrapolation to m05 (x=11)
QED(11) = -0.003264×121 + 0.017022×11 + 0.74932
         = -0.3949 + 0.1872 + 0.7493
         = **0.542**

The downward-opening parabola reflects accelerating QED penalties as chain length (and thus ALogP, MW, RotB) increase. At 11C, ALogP ≈ 5 is at the edge of the QED penalty zone.

Cross-check with log model on points 3–6 gives ~0.69; the quadratic is preferred because the logP penalty accelerates at high values.

## Job 2 — Lead Compound Selection

### Pre-filtering (before spending measurements)
- Series D: PSA = 29.1 < 40 → all excluded (no measurements needed)
- Series B: PSA = 110.02 ≥ 40 → candidates
- Series C: PSA = 72.24 ≥ 40 → candidates

### Measurements of Job 2 candidates
| mol_id | Series | Chain C | QED    | QED ≥ 0.6? |
|--------|--------|---------|--------|------------|
| m09    | C      | 2       | 0.5826 | No         |
| m02    | C      | 3       | 0.6046 | **Yes**    |
| m12    | C      | 4       | 0.4709 | No         |
| m32    | C      | 5       | 0.4683 | No         |
| m33    | C      | 6       | 0.4591 | No         |
| m14    | B      | 2       | 0.3237 | No         |
| m27    | B      | 3       | 0.3330 | No         |

### Chosen Lead: m02
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Measured QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Series C (nitrobenzamide), not Series A ✓

Series B molecules have very low QED (~0.32–0.33) due to extremely negative ALogP values. Series C QED peaks at 3C (m02) then declines, matching the pattern seen in Series A.

## Total measurements used: 12/12
