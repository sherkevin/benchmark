# Notes

## Task Overview
Predict QED of m05 (unsynthesized) and find a lead molecule meeting three constraints.

## Scaffold Grouping (from group_by_scaffold())
4 scaffolds identified:
1. COc1ccc(cc1)C(=O)N (m05's series): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. O=[N+]([O-])c1ccc(cc1)C(=O)N (nitrobenzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. OCC(O)C(O)C(O)C(=O)N (sugar): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. c1ccc(cc1)-c1ccc(cc1)C(=O)N (biphenyl): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predicting QED of m05

Measured QED for m05's congeners (5 measurements):
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Trend fits (predicting at chain_len=11):
- Degree 1 (all 5 pts): 0.695, RMSE=0.01308
- Degree 2 (all 5 pts): 0.5416, RMSE=0.01188 -- best fit
- Degree 3 (all 5 pts): 2.13 (absurd, >1)

Chose degree 2 with all 5 points (0.5416) as the most chemically reasonable prediction.
The quadratic captures the slight hump near chain 3 and accelerating decrease for longer chains.
m05 has alogp=4.956 (very lipophilic), so reduced QED is expected.

## Task 2: Finding a Lead Molecule

Constraints: QED >= 0.6, PSA >= 40, not in m05's series.

PSA analysis:
- m05's series: PSA=38.33 (< 40, disqualified)
- Biphenyl series: PSA=29.1 (< 40, all disqualified)
- Nitrobenzamide series: PSA=72.24 (qualifies)
- Sugar series: PSA=110.02 (qualifies)

Measured candidates (7 measurements):
| mol_id | scaffold | PSA     | QED    | Qualifies?         |
|--------|----------|---------|--------|--------------------|
| m09    | nitro    | 72.24   | 0.5826 | No (QED<0.6)       |
| m02    | nitro    | 72.24   | 0.6046 | Yes                |
| m12    | nitro    | 72.24   | 0.4709 | No (QED<0.6)       |
| m32    | nitro    | 72.24   | 0.4683 | No (QED<0.6)       |
| m01    | sugar    | 110.02  | 0.3187 | No (QED<0.6)       |
| m03    | sugar    | 110.02  | 0.3282 | No (QED<0.6)       |
| m14    | sugar    | 110.02  | 0.3237 | No (QED<0.6)       |

Lead: m02 -- QED=0.6046, PSA=72.24, nitrobenzamide scaffold (different from m05).

## Budget Used
12/12 measurements: 5 for m05 congeners + 7 for Task 2 candidates.
