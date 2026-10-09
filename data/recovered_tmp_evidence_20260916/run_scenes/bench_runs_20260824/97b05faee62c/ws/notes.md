# Notes on QED Prediction and Lead Selection

## Job 1: Predict QED of m05

### Identifying the Congeneric Series
m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` and chain_len=11.

I used `describe_structure` to find all pool molecules with the same scaffold:
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6
- m05: chain_len=11 (target, not synthesizable)

### Measuring QED for Congeners
I measured QED for all 5 available congeners:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

The trend shows QED peaks at chain_len=3 (m24) and then decreases as chain length increases.

### Fitting the Trend
I fit a quadratic polynomial (degree=2) through the 5 measured points:
- xs = [2, 3, 4, 5, 6]
- ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

The fitted polynomial has coefficients (low to high): [0.74932, 0.017024, -0.003264]
RMSE on given points: 0.01188

Extrapolating to chain_len=11 gives **QED = 0.5416**.

For comparison, a linear fit (degree=1) gave 0.695 with RMSE=0.01308. The quadratic fit is better (lower RMSE) and the task explicitly says not to assume a linear relationship.

## Job 2: Selecting a Lead Compound

### Constraints
1. measured QED >= 0.6
2. psa >= 40
3. not in the same series as m05

### Screening
First, I used `screen_candidates()` to find molecules with psa >= 40. This returned 18 molecules.

Note: All m05-series congeners (m15, m17, m22, m24, m28) have psa=38.33, which fails the psa >= 40 constraint. Therefore, any molecule passing the psa screen automatically satisfies constraint 3 (not in m05 series).

### Measuring Candidates
I measured QED for the first 5 candidates from the passing list:
| mol_id | psa    | QED     | Passes all? |
|--------|--------|---------|-------------|
| m01    | 110.02 | 0.3187  | No (QED < 0.6) |
| m02    | 72.24  | 0.6046  | **YES** |
| m03    | 110.02 | 0.3282  | No (QED < 0.6) |
| m06    | 72.24  | 0.4011  | No (QED < 0.6) |
| m07    | 110.02 | 0.3233  | No (QED < 0.6) |

**m02** is the lead compound:
- QED = 0.6046 >= 0.6 ✓
- psa = 72.24 >= 40 ✓
- Scaffold is `O=[N+]([O-])c1ccc(cc1)C(=O)N` (different from m05's scaffold) ✓

## Summary
- **Predicted QED for m05**: 0.5416 (quadratic extrapolation)
- **Lead compound**: m02
- **Lead QED (measured)**: 0.6046
- **Congeners in m05 series**: m15, m24, m28, m17, m22
