# Notes

## Task Overview
- Predict QED of m05 (never synthesized, SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC)
- Pick one molecule from pool with: QED >= 0.6, PSA >= 40, not in m05's congeneric series

## Scaffold Analysis
Four scaffolds identified:
1. COc1ccc(cc1)C(=O)N (methoxybenzamide) - m05's series: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. O=[N+]([O-])c1ccc(cc1)C(=O)N (nitrobenzamide): PSA=72.24
3. OCC(O)C(O)C(O)C(=O)N (sugar-like): PSA=110.02
4. c1ccc(cc1)-c1ccc(cc1)C(=O)N (biphenyl): PSA=29.1 - excluded (PSA < 40)

## Measurements Used (12/12)

### m05's congeneric series (5 measurements):
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

### Task 2 candidates (7 measurements):
| mol_id | scaffold        | chain | QED    | PSA    | Qualifies? |
|--------|-----------------|-------|--------|--------|------------|
| m09    | nitrobenzamide  | 2     | 0.5826 | 72.24  | No (QED<0.6) |
| m02    | nitrobenzamide  | 3     | 0.6046 | 72.24  | YES |
| m12    | nitrobenzamide  | 4     | 0.4709 | 72.24  | No (QED<0.6) |
| m32    | nitrobenzamide  | 5     | 0.4683 | 72.24  | No (QED<0.6) |
| m14    | sugar           | 2     | 0.3237 | 110.02 | No (QED<0.6) |
| m27    | sugar           | 3     | 0.3330 | 110.02 | No (QED<0.6) |
| m01    | sugar           | 4     | 0.3187 | 110.02 | No (QED<0.6) |

## Task 1: m05 QED Prediction

Quadratic fit (degree=2) on all 5 series members:
- QED(chain) = 0.74932 + 0.017024*chain - 0.003264*chain^2
- QED(11) = 0.5416
- RMSE = 0.01188

Component analysis confirms the prediction:
- ROTB component extrapolates to ~0 at ROTB=12 (major drag on QED)
- MW component improves with chain length (MW=305 is in good range)
- ALOGP=4.956 starting to hurt
- ALERTS stabilized around 0.64 for chain>=4

Final prediction: m05 QED ~ 0.54

## Task 2: Lead Molecule

m02 is the only measured molecule satisfying all three constraints:
1. Measured QED = 0.6046 >= 0.6 (check)
2. PSA = 72.24 >= 40 (check)
3. Nitrobenzamide scaffold != m05's methoxybenzamide scaffold (check)
