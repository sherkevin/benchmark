# Notes

## Task Overview
- 33 molecules in pool, 4 scaffold groups
- Budget: 17 units shared between list_pool and measure_qed
- Task 1: Predict QED of m05 (unsynthesized, chain_len=11, scaffold COc1ccc(cc1)C(=O)N)
- Task 2: Find molecule with QED>=0.6, psa>=40, not in m05's congeneric series

## Scaffold Groups (from group_by_scaffold, free)
1. **COc1ccc(cc1)C(=O)N** (m05's series): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N**: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N**: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N**: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: QED Prediction for m05

### Measured QEDs in m05's congeneric series:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Trend fits evaluated for chain_len=11:
| Model | Points | Degree | Prediction | RMSE |
|-------|--------|--------|------------|------|
| **Quadratic** | **all 5** | **2** | **0.5416** | **0.01188** |
| Linear | all 5 | 1 | 0.695 | 0.01308 |
| Cubic | all 5 | 3 | 2.1323 | 0.00835 (overfit) |
| Quadratic | chains 3-6 | 2 | 0.9378 | 0.00539 (unrealistic) |
| Linear | chains 3-6 | 1 | 0.6508 | 0.00883 |

### Decision:
Chose degree-2 polynomial on all 5 data points (prediction = 0.5416).
- Best RMSE among non-overfitting models
- Captures accelerating QED decline from increasing lipophilicity
- m05 has alogp=4.956, well outside drug-like range, supporting steeper decline

**Predicted QED for m05: 0.5416**

## Task 2: Lead Molecule Selection

### Constraints: QED >= 0.6, psa >= 40, not in m05's series

### Candidates measured (all from non-m05 scaffolds):
| mol_id | scaffold | psa     | QED    | Valid? |
|--------|----------|---------|--------|--------|
| m09    | nitro    | 72.24   | 0.5826 | No (QED<0.6) |
| m02    | nitro    | 72.24   | 0.6046 | **YES** |
| m12    | nitro    | 72.24   | 0.4709 | No |
| m14    | sugar    | 110.02  | 0.3237 | No |
| m32    | nitro    | 72.24   | 0.4683 | No |
| m33    | nitro    | 72.24   | 0.4591 | No |
| m27    | sugar    | 110.02  | 0.3330 | No |
| m29    | nitro    | 72.24   | 0.4446 | No |

### Selected: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- QED (measured): 0.6046
- psa: 72.24 (>= 40)
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's COc1ccc(cc1)C(=O)N)
- All three constraints satisfied

## Budget Usage
- Pages listed: 4 (pages 1-4, cost = 4)
- QED measurements: 13 (m15, m24, m28, m17, m22, m09, m02, m12, m14, m32, m33, m27, m29, cost = 13)
- Total: 17/17 units used
