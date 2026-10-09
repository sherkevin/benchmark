# Notes

## Overview
Task: Predict QED of m05 (never synthesized) and find a lead molecule meeting three constraints.

## Scaffold Grouping
Four scaffolds identified:
1. **COc1ccc(cc1)C(=O)N** (methoxy-benzamide) — m05's series: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (nitro-benzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N** (sugar-alcohol): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (biphenyl): PSA=29.1 — all below 40, excluded from Task 2

## Task 1: m05 QED Prediction

### Measurements (m05 congeneric series)
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

### QED Components Analysis
- m15 (chain=2): MW=0.33, ALOGP=0.87, ROTB=0.99, ALERTS=0.84
- m22 (chain=6): MW=0.68, ALOGP=1.00, ROTB=0.53, ALERTS=0.64

Key trends as chain grows:
- MW desirability increases (MW moves toward optimal range)
- ALOGP desirability peaks then drops sharply (alogp > 3.5 is bad)
- ROTB desirability drops (more rotatable bonds)
- ALERTS desirability decreases

For m05 (chain=11): alogp=4.956 (very high), rotb=12 (very high) → both pull QED down significantly.

### Polynomial Fits (chain_len → QED, predicting at chain=11)
| Model              | Points   | Prediction | RMSE   |
|--------------------|----------|------------|--------|
| Quadratic (deg=2)  | 5 (2-6)  | **0.5416** | 0.0119 |
| Linear (deg=1)     | 5 (2-6)  | 0.6950     | 0.0131 |
| Linear (deg=1)     | 4 (3-6)  | 0.6508     | 0.0088 |
| Quadratic (deg=2)  | 4 (3-6)  | 0.9378     | 0.0054 |
| Cubic (deg=3)      | 5 (2-6)  | 2.1323     | 0.0084 |
| Quadratic (deg=2)  | 3 (4-6)  | 0.5257     | 0.0000 |
| Linear (deg=1)     | 3 (4-6)  | 0.7041     | 0.0024 |

### Chosen prediction: **0.5416** (quadratic, all 5 points)
Rationale:
- Uses all available data
- Captures accelerating decline (negative curvature, physically motivated by increasing alogp and rotb)
- Consistent with quadratic on last 3 points (0.5257)
- Cubic overfits (prediction > 1, unphysical)
- Linear fits underestimate the decline rate — at alogp=4.956 and rotb=12, desirability functions drop non-linearly

## Task 2: Lead Molecule Selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05's congeneric series

### Candidates Measured
| mol_id | Series  | PSA     | QED    | Qualifies? |
|--------|---------|---------|--------|------------|
| m09    | nitro   | 72.24   | 0.5826 | No (QED < 0.6) |
| m02    | nitro   | 72.24   | 0.6046 | **Yes** |
| m12    | nitro   | 72.24   | 0.4709 | No (QED < 0.6) |
| m32    | nitro   | 72.24   | 0.4683 | No (QED < 0.6) |
| m14    | sugar   | 110.02  | 0.3237 | No (QED < 0.6) |
| m01    | sugar   | 110.02  | 0.3187 | No (QED < 0.6) |
| m27    | sugar   | 110.02  | 0.3330 | No (QED < 0.6) |

### m02 Components
- MW=0.50, ALOGP=0.92, HBA=0.98, HBD=0.99, PSA=0.93, ROTB=0.97, AROM=0.83, ALERTS=0.24
- The low ALERTS score (0.24, nitro group) pulls QED down, but other components are strong

### Chosen lead: **m02** (QED = 0.6046)
Only molecule in the measured set that satisfies all three constraints.

## Budget Usage
12/12 measurements used:
- 5 for m05 series (m15, m24, m28, m17, m22)
- 7 for Task 2 candidates (m09, m02, m14, m12, m32, m01, m27)
