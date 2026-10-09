# Notes: QED Prediction and Lead Selection

## Task Overview
1. Predict QED for m05 (target molecule) by measuring its congeneric series members
2. Select a lead compound meeting 3 constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's series
3. Total measurement budget: 12 measurements

## Initial Analysis
### Understanding m05's structure
m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Chain length: 11 carbons (N-alkyl chain)

### Confirmed congeneric series members for m05 (same scaffold):
All have scaffold `COc1ccc(cc1)C(=O)N`:
1. m15: chain length 2
2. m24: chain length 3  
3. m28: chain length 4
4. m17: chain length 5
5. m22: chain length 6
6. m05: chain length 11 (target, cannot be measured)

These are all the series members in the pool. Need to measure QED for chain lengths 2-6 and extrapolate to chain length 11.

### Measurement Strategy for Job 1:
- Used 5 measurements on m15, m24, m28, m17, m22
- Fit polynomial trend (degree 2 recommended in task)
- Extrapolate to chain length 11
- This leaves 7 measurements for lead selection

### QED Measurements for m05 Series:
Chain lengths and QED values:
1. Chain 2 (m15): 0.7621
2. Chain 3 (m24): 0.7919  
3. Chain 4 (m28): 0.7518
4. Chain 5 (m17): 0.7498
5. Chain 6 (m22): 0.7377

Pattern: QED increases from chain 2 to 3, then decreases gradually from chain 3 to 6.

### Trend Fitting Results:
1. Quadratic fit (degree 2):
   - Prediction for chain 11: 0.5416
   - RMSE: 0.01188
   - Equation: y = 0.74932 + 0.017024*x - 0.003264*x²

2. Linear fit (degree 1):
   - Prediction for chain 11: 0.6950
   - RMSE: 0.01308
   - Equation: y = 0.79502 - 0.00909*x

Quadratic has slightly lower RMSE and matches the observed curvature (peak at chain 3). Given task says "do not assume the relationship is a straight line", quadratic seems more appropriate.

## Job 2: Lead Selection

### Candidates passing PSA ≥ 40 constraint (18 molecules):
High PSA (110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
Medium PSA (72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

All should satisfy constraint 3 (not in m05's series) since m05 series has PSA = 38.33.

### Strategy:
- Have 7 measurements remaining
- Need to find at least one molecule with QED ≥ 0.6
- Should prioritize molecules likely to have high QED
- Could sample across different scaffolds

### Measurements:
1. m01: QED = 0.3187 (too low)
2. m02: QED = 0.6046 (meets all constraints!)
3. m03: QED = 0.3282 (too low)
4. m06: QED = 0.4011 (too low)
5. m09: QED = 0.5826 (too low, < 0.6)
6. m07: QED = 0.3233 (too low)
7. m10: QED = 0.3724 (too low)

Used all 12 measurements.

### Final Candidate: m02
m02 passes all 3 constraints:
1. QED = 0.6046 ≥ 0.6 ✓ (measured)
2. PSA = 72.24 ≥ 40 ✓ (from pool.tsv)  
3. Not in m05's series ✓ (scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` vs m05's `COc1ccc(cc1)C(=O)N`)

No other measured molecule met all criteria. m09 came close at 0.5826 but < 0.6.

## Final Answer Summary:

### Job 1: Predict QED for m05 (target)
- Measured QED for m05 series members (chain lengths 2-6): m15(0.7621), m24(0.7919), m28(0.7518), m17(0.7498), m22(0.7377)
- Fitted quadratic trend: y = 0.74932 + 0.017024*x - 0.003264*x²
- Extrapolated to chain length 11: **QED = 0.5416**
- All congener IDs in pool: m15, m24, m28, m17, m22

### Job 2: Select lead compound
- Screened PSA ≥ 40: 18 candidates
- Measured 7 candidates (used all 12 total measurements)
- Found **m02** meets all 3 constraints:
  1. Measured QED = 0.6046 ≥ 0.6
  2. PSA = 72.24 ≥ 40
  3. Not in m05's series (different scaffold)
- No other molecule met all criteria

### Measurements used: 12/12
1. m15, m24, m28, m17, m22 (Job 1 - 5 measurements)
2. m01, m02, m03, m06, m09, m07, m10 (Job 2 - 7 measurements)

