# Notes: QED Prediction for m05 and Lead Compound Selection

## Job 1: Predict QED of m05

### Identifying the Congeneric Series
Target m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` and chain length 11.

Using `describe_structure`, I identified all pool molecules sharing the same scaffold:
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

### Measuring the Series
Measured QED for all 5 congeners:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Fitting the Trend
The QED peaks at chain_len=3 (0.7919) and then decreases monotonically. This curvature suggests a quadratic fit is appropriate.

Using `fit_trend` with degree=2:
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE on given points: 0.01188
- Prediction at x=11: **0.5416**

Compared alternatives:
- degree=1 (linear): prediction=0.695, RMSE=0.01308
- degree=3 (cubic): prediction=2.1323 (unrealistic, >1), RMSE=0.00835

The quadratic fit has the best balance: lower RMSE than linear, and produces a realistic prediction unlike the cubic (which overfits and extrapolates poorly).

**Final prediction for m05 QED: 0.5416**

## Job 2: Select a Backup Lead Compound

### Constraints
1. measured QED >= 0.6
2. psa >= 40 (free constraint)
3. NOT in the same series as m05

### Screening
18 molecules pass the psa >= 40 constraint: m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33.

None of these are in m05's series (m15, m17, m22, m24, m28), so all 18 are eligible for constraint 3.

### Measurement Strategy
With limited budget (3 measurements remaining after Job 1), I measured candidates from the nitrobenzene series:
- m02: QED = 0.6046 ✓ (passes all constraints!)
- m12: QED = 0.4709 ✗ (fails QED constraint)
- m32: QED = 0.4683 ✗ (fails QED constraint)

### Selected Lead
**m02** satisfies all three constraints:
- QED = 0.6046 >= 0.6 ✓
- psa = 72.24 >= 40 ✓
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

## Congener IDs (same series as m05)
m15, m24, m28, m17, m22
