# Notes

## Strategy
- Listed all 33 molecules (9 pages × 4/page = 9 budget units used)
- Identified 4 congeneric series (scaffolds)
- Used 3 measurements on key molecules

## Scaffold Analysis

### Scaffold 1: Sugar — `OCC(O)C(O)C(O)C(=O)N-` (psa=110.02)
m01(4C), m03(6C), m07(9C), m14(2C), m18(8C), m19(5C), m21(7C), m25(10C), m27(3C)

### Scaffold 2: Nitrobenzene — `O=[N+]([O-])c1ccc(cc1)C(=O)N-` (psa=72.24)
m02(3C), m06(9C), m09(2C), m10(10C), m12(4C), m29(7C), m31(8C), m32(5C), m33(6C)

### Scaffold 3: Biphenyl — `c1ccc(cc1)-c1ccc(cc1)C(=O)N-` (psa=29.1)
m04(4C), m08(9C), m11(3C), m13(2C), m16(10C), m20(7C), m23(6C), m26(8C), m30(5C)

### Scaffold 4: Methoxybenzene — `COc1ccc(cc1)C(=O)N-` (psa=38.33) — m05's series
m15(2C), m24(3C), m28(4C), m17(5C), m22(6C), m05(11C)

## Measurements Made

| mol_id | Series | Chain | QED |
|--------|--------|-------|------|
| m15 | Methoxy (m05 series) | 2C | 0.7621 |
| m22 | Methoxy (m05 series) | 6C | 0.7377 |
| m09 | Nitrobenzene | 2C | 0.5826 |

## Task 1: QED Prediction for m05

Linear extrapolation from m05's congeneric series:
- m15 (2C): QED = 0.7621
- m22 (6C): QED = 0.7377
- Rate = (0.7377 - 0.7621) / (6 - 2) = -0.0061 per CH₂
- m05 (11C): QED = 0.7621 + 9 × (-0.0061) = 0.7072

Predicted QED for m05 ≈ 0.71

## Task 2: Lead Selection

Constraints: QED ≥ 0.6, psa ≥ 40, not in m05's series

- Biphenyl series excluded (psa=29.1 < 40)
- Methoxybenzene series excluded (same as m05 + psa=38.33 < 40)
- Only sugar (psa=110.02) and nitrobenzene (psa=72.24) qualify on psa

Only non-m05-series measurement: m09 (QED=0.5826)
Budget exhausted before additional measurements possible.

Best available: m09 with measured QED = 0.5826

## Budget Usage
- list_pool: 9 pages = 9 units
- measure_qed: 3 measurements = 3 units
- Total: 12/12
