# Notes

## Strategy
- Used `group_by_scaffold()` (free) to identify 4 congeneric series
- m05 belongs to Group 1 (scaffold `COc1ccc(cc1)C(=O)N`), chain_len=11
- Other Group 1 members: m15(2), m24(3), m28(4), m17(5), m22(6)

## Task 1: Predicting m05 QED

Measured all 5 available Group 1 members:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Trend: QED peaks at chain 3 then declines steadily. Extrapolating to chain 11.

Fits tried:
- Quadratic (deg=2), all 5 pts: **0.5416** (RMSE=0.012) <- chosen
- Linear (deg=1), all 5 pts: 0.6950 (RMSE=0.013)
- Linear (deg=1), chains 3-6 only: 0.6508 (RMSE=0.009)
- Quadratic (deg=2), chains 3-6: 0.9378 (unphysical, overfitting)
- Cubic (deg=3), all 5 pts: 2.1323 (absurd, overfitting)

The quadratic with all 5 points is the most principled (uses all data, captures curvature, physical). m05 has alogp=4.956 and rotb=12 which are penalized by QED, consistent with ~0.54.

**Predicted m05 QED: 0.5416**

## Task 2: Finding a lead molecule

Constraints: QED >= 0.6, psa >= 40, not in m05's series (Group 1).

PSA by group:
- Group 1: 38.33 (m05's series, excluded anyway)
- Group 2: 72.24 (pass)
- Group 3: 110.02 (pass)
- Group 4: 29.1 (below 40, all disqualified)

Measured candidates from Groups 2 and 3:
| mol_id | group | chain | QED    | psa     | pass? |
|--------|-------|-------|--------|---------|-------|
| m09    | 2     | 2     | 0.5826 | 72.24   | QED too low |
| m02    | 2     | 3     | 0.6046 | 72.24   | ALL PASS |
| m12    | 2     | 4     | 0.4709 | 72.24   | QED too low |
| m32    | 2     | 5     | 0.4683 | 72.24   | QED too low |
| m14    | 3     | 2     | 0.3237 | 110.02  | QED too low |
| m27    | 3     | 3     | 0.3330 | 110.02  | QED too low |
| m01    | 3     | 4     | 0.3187 | 110.02  | QED too low |

**Best lead: m02** with measured QED = 0.6046, psa = 72.24, not in m05's series.

## Budget used
- 5 measurements (Group 1) + 3 pages + 7 measurements (candidates) + 1 free m05 probe = 15 of 17 units
