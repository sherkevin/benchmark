# Notes

## Task Overview
- Task 1: Predict QED of m05 (never synthesized, chain_len=11, scaffold COc1ccc(cc1)C(=O)N)
- Task 2: Find one molecule with QED ≥ 0.6, PSA ≥ 40, not in m05's congeneric series

## Scaffold Groups (4 total)
1. **COc1ccc(cc1)C(=O)N** (m05's series): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (nitro): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N** (sugar): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (biphenyl): PSA=29.1 (all below 40, excluded from Task 2)

## Measurements (12/12 used)

### m05 series (for Task 1 trend):
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

### Other series (for Task 2 candidates):
| mol_id | chain | QED    | Series | PSA    |
|--------|-------|--------|--------|--------|
| m02    | 3     | 0.6046 | nitro  | 72.24  |
| m12    | 4     | 0.4709 | nitro  | 72.24  |
| m32    | 5     | 0.4683 | nitro  | 72.24  |
| m29    | 7     | 0.4446 | nitro  | 72.24  |
| m01    | 4     | 0.3187 | sugar  | 110.02 |
| m03    | 6     | 0.3282 | sugar  | 110.02 |
| m19    | 5     | 0.3244 | sugar  | 110.02 |

## Task 1: Predicting m05 QED

Used degree-2 polynomial fit on all 5 measured points in m05's series:
- Points: (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)
- Coefficients: 0.74932 + 0.017024*x - 0.003264*x²
- RMSE = 0.01188
- **Prediction at chain=11: 0.5416**

Supporting evidence from QED components:
- m15 (chain 2): ROTB=0.9927, ALERTS=0.8425, ALOGP=0.8736
- m22 (chain 6): ROTB=0.5332, ALERTS=0.6397, ALOGP=0.9973
- ROTB and ALERTS scores decline sharply with chain length
- m05 (chain 11): ROTB=12, ALOGP=4.956 — both much worse than chain 6
- Quadratic decline is chemically reasonable

Alternative fits checked:
- Linear (all 5 pts): 0.695 at chain 11
- Linear (chains 3-6): 0.6508 at chain 11
- Chose degree-2 with all 5 points as best model (lowest RMSE among full-series fits)

## Task 2: Lead Selection

Only measured molecule satisfying ALL three constraints:
- **m02**: QED=0.6046 ≥ 0.6 ✓, PSA=72.24 ≥ 40 ✓, different series (nitro) ✓

Other candidates failed:
- Sugar series: all QED < 0.35 (too low)
- Nitro series longer chains: QED < 0.5 (too low)
- Biphenyl series: PSA=29.1 < 40 (fails constraint 2)

m02 QED components: MW=0.4975, ALOGP=0.9223, HBA=0.9848, HBD=0.9866, PSA=0.9302, ROTB=0.9697, AROM=0.8273, ALERTS=0.2414
- ALERTS score is low (0.2414) due to nitro group structural alert, but overall QED still ≥ 0.6

## Final Answers
- target_qed: 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046
