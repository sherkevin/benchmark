# Task Notes

## Job 1: Predict QED of m05

### Step 1: Identify congeneric series members
Target molecule m05:
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Scaffold: COc1ccc(cc1)C(=O)N
- Chain length: 11 carbons

Pool molecules with same scaffold identified so far:
1. m15: COc1ccc(cc1)C(=O)NCC (chain length 2)
2. m24: COc1ccc(cc1)C(=O)NCCC (chain length 3) 
3. m28: COc1ccc(cc1)C(=O)NCCCC (chain length 4)
4. m17: COc1ccc(cc1)C(=O)NCCCCC (chain length 5)
5. m22: COc1ccc(cc1)C(=O)NCCCCCC (chain length 6)

These 5 molecules form the congeneric series with m05. The pool contains only short-chain members (2-6 carbons), while m05 has 11 carbons, so we need to extrapolate beyond measured range.

### Step 2: Measure QED values for series members
Budget: 17 measurements total for both jobs. Need to measure these 5 molecules to establish trend.

Measured QED values:
1. m15 (chain length 2): QED = 0.7621
2. m24 (chain length 3): QED = 0.7919  
3. m28 (chain length 4): QED = 0.7518
4. m17 (chain length 5): QED = 0.7498
5. m22 (chain length 6): QED = 0.7377

Observations: QED peaks around chain length 3 (0.7919) then decreases as chain gets longer.

### Step 3: Fit trend and predict m05 QED
Will use fit_trend with polynomial degree 2 (quadratic) as relationship may not be linear.

Trend fitting results:
- Quadratic fit (degree=2): Predicted QED for m05 (chain length 11) = 0.5416
- Linear fit (degree=1): Predicted QED = 0.6950

Decision: Use quadratic fit (0.5416) because:
1. RMSE is slightly lower (0.01188 vs 0.01308)
2. Quadratic better captures the curvature observed in data (peak at chain length 3 then decrease)
3. Extrapolation to chain length 11 (far beyond measured range 2-6) should account for likely continued downward trend

Target QED prediction: 0.5416

## Job 2: Select backup lead compound

### Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40 (free to check)
3. not in same series as m05 (free to check)

### Step 1: Screen for psa ≥ 40
18 molecules pass psa ≥ 40 screen:
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

### Step 2: Exclude m05 series members
m05 series members (m15, m24, m28, m17, m22) all have psa = 38.33 < 40, so already excluded.

### Step 3: Need to measure QED for candidates
Remaining budget: 3 measurements (14 used so far: 9 for listing pages, 5 for m05 series measurements)

Strategy: Need to find at least one molecule with QED ≥ 0.6.

### Measurements:
1. m01: QED = 0.3187 < 0.6 (not suitable)
2. m02: QED = 0.6046 ≥ 0.6 ✓
   - Check scaffold: "O=[N+]([O-])c1ccc(cc1)C(=O)N" ≠ m05's scaffold
   - psa = 72.24 ≥ 40 ✓
   - All constraints satisfied!

m02 qualifies as backup lead compound.

## Summary

### Job 1 completed:
- Identified 5 congeneric series members: m15, m24, m28, m17, m22
- Measured their QED values: 0.7621, 0.7919, 0.7518, 0.7498, 0.7377
- Fitted quadratic trend (degree=2) to chain length vs QED data
- Predicted m05 (chain length 11) QED: 0.5416

### Job 2 completed:
- Screened for psa ≥ 40: 18 candidates
- m05 series members automatically excluded (psa = 38.33 < 40)
- Measured m02: QED = 0.6046 ≥ 0.6
- Verified m02 scaffold different from m05
- m02 satisfies all 3 constraints

### Budget usage:
- Total budget: 17
- Used: 16 (9 for listing pages,针7 for measurements)
- Remaining: 1

Answer written to answer.json.