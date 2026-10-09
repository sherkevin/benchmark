# Notes: QED Prediction and Lead Selection

## Job 1: Predict QED for m05

### Step 1: Identify congeneric series
Used `describe_structure` to determine which molecules share the same scaffold as m05.
- m05 scaffold: `COc1ccc(cc1)C(=O)N`, chain_len=11

Molecules in the same series (same scaffold, different chain lengths):
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6
- m05: chain_len=11 (target, not synthesizable)

### Step 2: Measure QED for series members
Measured all 5 available molecules in the series:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Step 3: Fit trend and extrapolate
The data shows QED peaks at chain_len=3 then decreases. Fitted a quadratic polynomial (degree=2):
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE: 0.01188

Extrapolated to chain_len=11 (m05):
- **Predicted QED for m05: 0.5416**

(Linear fit gave 0.695 but had higher RMSE of 0.01308, so quadratic is preferred.)

---

## Job 2: Select backup lead compound

### Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in same series as m05

### Step 1: Screen by psa ≥ 40
18 molecules pass the psa filter.

### Step 2: Exclude m05 series
The m05 series molecules (m15, m24, m28, m17, m22) all have psa=38.33 < 40, so they're already excluded.

### Step 3: Measure QED for candidates
Measured 7 candidates to find one with QED ≥ 0.6:
| mol_id | psa | QED | Passes all? |
|--------|-----|-----|-------------|
| m01    | 110.02 | 0.3187 | No (QED < 0.6) |
| m02    | 72.24  | 0.6046 | **YES** |
| m03    | 110.02 | 0.3282 | No (QED < 0.6) |
| m09    | 72.24  | 0.5826 | No (QED < 0.6) |
| m12    | 72.24  | 0.4709 | No (QED < 0.6) |
| m06    | 72.24  | 0.4011 | No (QED < 0.6) |
| m10    | 72.24  | 0.3724 | No (QED < 0.6) |

### Selected lead: m02
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

---

## Budget usage
- 5 measurements for m05 series (m15, m24, m28, m17, m22)
- 7 measurements for lead candidates (m01, m02, m03, m09, m12, m06, m10)
- Total: 12/12 measurements used
