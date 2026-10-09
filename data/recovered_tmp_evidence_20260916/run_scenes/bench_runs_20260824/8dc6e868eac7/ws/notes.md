# Notes on QED Prediction and Lead Selection

## Job 1: Predict QED for m05

### Identifying the Congeneric Series
m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` and chain_len = 11.

I used `describe_structure` to identify all molecules in the pool with the same scaffold:
- m15: chain_len = 2
- m24: chain_len = 3
- m28: chain_len = 4
- m17: chain_len = 5
- m22: chain_len = 6

These 5 molecules form the congeneric series with m05.

### Measuring QED for Congeners
I measured QED for all 5 congeners:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Fitting the Trend
The data shows QED peaks at chain_len=3 and then decreases. I fit polynomials of different degrees:

- **Degree 1 (linear)**: prediction = 0.695, RMSE = 0.01308
- **Degree 2 (quadratic)**: prediction = 0.5416, RMSE = 0.01188
- **Degree 3 (cubic)**: prediction = 2.1323 (invalid, >1), RMSE = 0.00835

The cubic fit overfits and gives an impossible QED value. The quadratic fit has the lowest RMSE among valid models and gives a reasonable prediction.

**Final prediction for m05 (chain_len=11): QED = 0.5416**

The quadratic equation is: QED = 0.74932 + 0.017024*x - 0.003264*x²

## Job 2: Selecting a Backup Lead Compound

### Constraints
1. measured QED ≥ 0.6
2. psa ≥ 40 (free constraint)
3. NOT in the same series as m05

### Screening
First, I used `screen_candidates()` to find molecules with psa ≥ 40. This returned 18 candidates:
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

None of these are in m05's series (m15, m24, m28, m17, m22 have psa = 38.33 < 40, so they don't pass the screen anyway).

### Measuring QED for Candidates
I measured QED for the first few candidates:
| mol_id | QED |
|--------|-----|
| m01    | 0.3187 (fails) |
| m02    | 0.6046 (PASSES) |
| m03    | 0.3282 (fails) |
| m06    | 0.4011 (fails) |
| m07    | 0.3233 (fails) |

**m02** satisfies all three constraints:
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

### Selected Lead
**m02** is the backup lead compound with measured QED = 0.6046.

## Budget Summary
- Measurements used: 10 (5 for congeners + 5 for candidates)
- Budget remaining: 2
