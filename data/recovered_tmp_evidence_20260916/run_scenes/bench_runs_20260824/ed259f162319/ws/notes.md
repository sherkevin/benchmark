# Notes

## Data Analysis

### Congeneric Series Identification

The pool has 4 congeneric series (same scaffold, varying N-alkyl chain length):

**Series A - Methoxybenzamide** (scaffold: `COc1ccc(cc1)C(=O)N-`): m15, m24, m28, m17, m22, m05
**Series B - Nitrobenzamide** (scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N-`): m09, m02, m12, m32, m33, m29, m31, m06, m10
**Series C - Sugar-polyol** (scaffold: `OCC(O)C(O)C(O)C(=O)N-`): m14, m27, m01, m19, m03, m21, m18, m07, m25
**Series D - Biphenyl** (scaffold: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`): m13, m11, m04, m30, m23, m20, m26, m08, m16

**m05 is in Series A** (methoxybenzamide).

### Measurements (12 total, all used)

#### Series A (m05's series) - for Task 1 prediction:
| mol_id | N-alkyl C# | QED    | MW     | AlogP | rotb |
|--------|-----------|--------|--------|-------|------|
| m15    | 2         | 0.7621 | 179.22 | 1.445 | 3    |
| m24    | 3         | 0.7919 | 193.25 | 1.835 | 4    |
| m28    | 4         | 0.7518 | 207.27 | 2.225 | 5    |
| m17    | 5         | 0.7498 | 221.30 | 2.615 | 6    |
| m22    | 6         | 0.7377 | 235.33 | 3.005 | 7    |
| m05    | 11        | ?      | 305.46 | 4.956 | 12   |

#### Other series measured (Task 2 candidates):
| mol_id | Series | QED    | psa    | Qualifies? |
|--------|--------|--------|--------|------------|
| m09    | B      | 0.5826 | 72.24  | No (QED < 0.6) |
| m02    | B      | 0.6046 | 72.24  | **YES** ✓  |
| m12    | B      | 0.4709 | 72.24  | No (QED < 0.6) |
| m32    | B      | 0.4683 | 72.24  | No (QED < 0.6) |
| m33    | B      | 0.4591 | 72.24  | No (QED < 0.6) |
| m14    | C      | 0.3237 | 110.02 | No (QED < 0.6) |
| m01    | C      | 0.3187 | 110.02 | No (QED < 0.6) |

## Task 1: Predict QED of m05

m05 has 11 carbons in its N-alkyl chain (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`).

Using linear regression on all 5 measured members:

Data points (n_carbons, QED): (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)

- Mean x = 4.0, Mean y = 0.75866
- Slope = -0.00909 per carbon
- Intercept = 0.79502
- **Predicted QED(11C) = 0.79502 - 0.00909 * 11 = 0.695**

Cross-checks with alternative models:
- Linear fit on 3C-6C only: 0.651
- Linear fit on 4C-6C only: 0.704
- Log-linear on all 5 points: 0.697
- Average decline/carbon (3C-6C): 0.647

The overall trend shows a gentle decline. The all-points linear regression gives **0.695**.

## Task 2: Lead Molecule Selection

**m02** satisfies all three constraints:
1. Measured QED = 0.6046 >= 0.6 ✓
2. PSA = 72.24 >= 40 ✓
3. Not in m05's congeneric series (m02 is Series B / nitrobenzamide, not Series A) ✓
