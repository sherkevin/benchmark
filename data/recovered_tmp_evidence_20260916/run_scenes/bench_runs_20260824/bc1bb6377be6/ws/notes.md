# Notes

## Series Identification

**m05 SMILES**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxybenzamide with 11-carbon chain.

### Series 1 (methoxybenzamide, same scaffold as m05): `COc1ccc(cc1)C(=O)N-`
- m15: NCC (2 carbons)
- m24: NCCC (3 carbons)
- m28: NCCCC (4 carbons)
- m17: NCCCCC (5 carbons)
- m22: NCCCCCC (6 carbons)
- **m05**: NCCCCCCCCCCC (11 carbons) — TARGET, not synthesized

### Series 2 (nitrobenzamide): `O=[N+]([O-])c1ccc(cc1)C(=O)N-`
- m09 (2C), m02 (3C), m12 (4C), m32 (5C), m33 (6C), m29 (7C), m31 (8C), m06 (9C), m10 (10C)

### Series 3 (sugar alcohol amide): `OCC(O)C(O)C(O)C(=O)N-`
- m14 (2C), m27 (3C), m01 (4C), m19 (5C), m03 (6C), m21 (7C), m18 (8C), m07 (9C), m25 (10C)

### Series 4 (biphenyl amide): `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`
- m13 (2C), m11 (3C), m04 (4C), m30 (5C), m23 (6C), m20 (7C), m26 (8C), m08 (9C), m16 (10C)

## PSA Filter for Lead (PSA >= 40)
- Series 1: PSA = 38.33 -- FAIL (also same as m05, excluded anyway)
- Series 2: PSA = 72.24 -- PASS
- Series 3: PSA = 110.02 -- PASS
- Series 4: PSA = 29.1 -- FAIL

Lead must come from Series 2 or Series 3.

## Measurements

### m05 Series (Series 1) — Job 1:
| mol_id | Chain C | alogp | QED measured |
|--------|---------|-------|--------------|
| m15    | 2       | 1.445 | 0.7621       |
| m24    | 3       | 1.835 | 0.7919       |
| m28    | 4       | 2.225 | 0.7518       |
| m17    | 5       | 2.615 | 0.7498       |
| m22    | 6       | 3.005 | 0.7377       |
| m05    | 11      | 4.956 | unavailable  |

### Lead candidates (Job 2):
| mol_id | Series | Chain C | alogp | QED measured | Qualifies? |
|--------|--------|---------|-------|--------------|------------|
| m09    | 2      | 2       | 1.345 | 0.5826       | No (QED<0.6) |
| m02    | 2      | 3       | 1.735 | 0.6046       | YES         |
| m12    | 2      | 4       | 2.125 | 0.4709       | No (QED<0.6) |
| m32    | 2      | 5       | 2.515 | 0.4683       | No (QED<0.6) |
| m03    | 3      | 6       | -1.242| 0.3282       | No (QED<0.6) |
| m07    | 3      | 9       | -0.072| 0.3233       | No (QED<0.6) |

**Lead chosen: m02** — only molecule meeting all 3 constraints (QED=0.6046 >= 0.6, PSA=72.24 >= 40, not in m05's series).

## Job 1: QED Prediction for m05

### Observed Trend
QED peaks at 3C (0.7919) then declines: 0.7621 → 0.7919 → 0.7518 → 0.7498 → 0.7377.
First differences: +0.0298, -0.0401, -0.0020, -0.0121.
The decline accelerates slightly from 4C→5C→6C.

### Model: Quadratic Fit on All 5 Points
Fitting QED(n) = a + b*n + c*n^2 via least-squares:

Normal equations with n = {2,3,4,5,6}, Q = {0.7621, 0.7919, 0.7518, 0.7498, 0.7377}:
- Sum n = 20, Sum n^2 = 90, Sum n^3 = 440, Sum n^4 = 2274
- Sum Q = 3.7933, Sum nQ = 15.0823, Sum n^2Q = 67.5065

Solution:
- c = -0.003264
- b = 0.01702
- a = 0.74933

**QED(n) = 0.74933 + 0.01702*n - 0.003264*n^2**

Residuals: n=2: -0.008, n=3: +0.021, n=4: -0.013, n=5: -0.003, n=6: +0.004 (RMS = 0.012)

Peak of quadratic at n = -b/(2c) = 2.6, consistent with observed peak near 3C.

### Extrapolation to n=11
QED(11) = 0.74933 + 0.01702*11 - 0.003264*121
        = 0.74933 + 0.18722 - 0.39494
        = **0.54**

### Why Quadratic?
- Task says "do not assume a straight line" — the data clearly shows curvature (peak at 3C).
- Physical reasoning: as chain length grows, alogp increases linearly (~0.39/carbon) and QED penalizes high logP nonlinearly. At 11C, alogp = 4.956 (very high) and rotb = 12 (high), both strongly penalizing QED.
- Cross-check with Series 2: m02 (3C, alogp=1.735) → QED=0.6046; m12 (4C, alogp=2.125) → QED=0.4709. Sharp drops with increasing chain length are real.

### Alternative Models Considered
- Linear (all 5): 0.695 — ignores curvature
- Linear (3C-6C only): 0.651 — ignores early data
- Log model: 0.726 — doesn't capture peak well
- Sqrt model: 0.713 — same issue as log

## Budget Used
11 of 12 measurements used (m05 attempt was free since unavailable).

## Congeneric Series IDs for m05
m15, m17, m22, m24, m28
