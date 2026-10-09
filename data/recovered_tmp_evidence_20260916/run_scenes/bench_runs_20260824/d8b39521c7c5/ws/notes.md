# Notes

## Task 1: Predict QED for m05

m05 is in the scaffold group "COc1ccc(cc1)C(=O)N" with chain length 11.
Since m05 cannot be measured directly, I predicted its QED by measuring other molecules
in the same congeneric series and fitting a polynomial trend.

### Measurements in m05 series:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Trend fitting
Used fit_trend with degree=2 polynomial on chain lengths [2,3,4,5,6] and QED values.
Predicted QED at chain_len=11 (m05): **0.5416**

## Task 2: Find a lead molecule

Criteria:
1. Measured QED >= 0.6
2. psa >= 40
3. Not in same congeneric series as m05

### Scaffold analysis
- m05 series (COc1ccc...): psa = 38.33 (excluded - same series as m05)
- Nitro series (O=[N+]...): psa = 72.24 (valid for psa >= 40)
- Sugar series (OCC(O)...): psa = 110.02 (valid for psa >= 40)
- Biphenyl series (c1ccc...): psa = 29.1 (excluded - psa < 40)

### Measurements for candidate search:
| mol_id | series | psa | QED |
|--------|--------|-----|-----|
| m01    | Sugar  | 110.02 | 0.3187 (fails QED >= 0.6) |
| m02    | Nitro  | 72.24  | 0.6046 (PASSES all criteria) |
| m09    | Nitro  | 72.24  | 0.5826 (fails QED >= 0.6) |
| m14    | Sugar  | 110.02 | 0.3237 (fails QED >= 0.6) |

### Selected lead: m02
- QED measured: 0.6046 >= 0.6 ✓
- psa: 72.24 >= 40 ✓
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's scaffold) ✓

## Budget used
Total measurements: 9 out of 12
