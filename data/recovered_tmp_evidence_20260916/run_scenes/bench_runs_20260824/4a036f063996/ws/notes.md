# Notes: QED Prediction for m05 and Lead Compound Selection

## Job 1: Predict QED for m05

### Step 1: Identify congeners in m05's series
m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` and chain_len=11.

Using `describe_structure` on all pool molecules, I identified the following molecules sharing the same scaffold (same series as m05):
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

### Step 2: Measure QED for all congeners
Measured QED values:
- m15 (chain 2): 0.7621
- m24 (chain 3): 0.7919
- m28 (chain 4): 0.7518
- m17 (chain 5): 0.7498
- m22 (chain 6): 0.7377

Pattern: QED peaks at chain length 3, then declines steadily.

### Step 3: Fit trend and extrapolate to chain_len=11
Used `fit_trend` with degree=2 (quadratic) since the task explicitly says not to assume a straight line.

Quadratic fit results:
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE: 0.01188
- Prediction at x=11: **0.5416**

For comparison, linear fit gave 0.695 with RMSE=0.01308. The quadratic fit has lower RMSE and better captures the curvature (peak at chain 3, then decline).

**Final prediction for m05 QED: 0.5416**

---

## Job 2: Select backup lead compound

### Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in m05's series (scaffold ≠ `COc1ccc(cc1)C(=O)N`)

### Step 1: Screen by psa ≥ 40
18 molecules passed: m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

Note: m05-series molecules (m15, m17, m22, m24, m28) have psa=38.33, so they all fail constraint 2 automatically.

### Step 2: Measure QED for candidates
- m01: 0.3187 (FAIL)
- m02: 0.6046 (PASS - ≥0.6)
- m03: 0.3282 (FAIL)
- m06: 0.4011 (FAIL)
- m07: 0.3233 (FAIL)
- m09: 0.5826 (FAIL)
- m10: 0.3724 (FAIL)

### Step 3: Verify m02 meets all constraints
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

**Selected lead: m02**

---

## Budget usage
- 5 measurements for m05 congeners (m15, m17, m22, m24, m28)
- 7 measurements for lead candidates (m01, m02, m03, m06, m07, m09, m10)
- Total: 12/12 measurements used
