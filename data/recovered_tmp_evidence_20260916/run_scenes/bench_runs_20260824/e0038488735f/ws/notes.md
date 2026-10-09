# Task Notes

## Initial Analysis

**Target molecule m05:**
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Scaffold: COc1ccc(cc1)C(=O)N
- Chain length: 11 carbons

**Budget:** 17 measurements total for both jobs

## Plan

### Job 1: Predict QED for m05
1. Find all molecules in pool with same scaffold as m05
2. Measure their QED values
3. Fit trend to observed QED vs chain length
4. Extrapolate to chain length 11

### Job 2: Find backup lead compound
Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free descriptor)
3. Not in same series as m05 (different scaffold)

Approach:
- First filter pool for PSA ≥ 40 (using screen_candidates)
- Check scaffolds of promising candidates
- Measure QED for final candidates

## Progress Log

### Pool exploration:
- Viewed all 9 pages (33 total molecules)
- Budget used for list_pool: 9/17

### Job 1: Measuring m05's congeneric series
Molecules with same scaffold as m05 (COc1ccc(cc1)C(=O)N):
1. m15 - chain length 2 - QED: 0.7621
2. m24 - chain length 3 - QED: 0.7919  
3. m28 - chain length 4 - QED: 0.7518
4. m17 - chain length 5 - QED: 0.7498
5. m22 - chain length 6 - QED: 0.7377

Measurements used: 14/17 (5 measurements for series + 9 for list_pool)
Remaining budget: 3/17

### Job方式 1: Trend fitting for m05
Data points (chain length, QED):
- (2, 0.7621)
- (3, 0.7919)  
- (4, 0.7518)
- (5, 0.7498)
- (6, 0.7377)

Quadratic fit (degree 2) prediction for chain length 11: 0.5416
Linear fit (degree 1) prediction: 0.695
Quadratic has lower RMSE (0.01188 vs 0.01308) and fits curved trend better.

**Tentative prediction for m05 QED: 0.5416**

### Job 2: Finding backup lead
PSA ≥ 40 filter yields 18 candidates:
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

None of m05's series members (m15, m24, m28, m17, m22) pass PSA filter (PSA=38.33 < 40), so all 18 automatically satisfy constraint 3.

Need to measure QED for at least one candidate with QED ≥ 0.6.

### Measurements for Job 2:
1. m02 (nitrobenzene scaffold, chain length 3): QED = 0.6046 ✓ (meets ≥ 0.6)
2. m01 (polyol scaffold, chain length 4): QED = 0.3187 ✗ (too low)
3. m09 (nitrobenzene scaffold, chain length 2): QED = 0.5826 ✗ (< 0.6)

**Selected lead candidate: m02**
- QED: 0.6046 ≥ 0.6 ✓
- PSA: 72.24 ≥ 40 ✓  
- Not in m05's series: different scaffold ✓

Budget fully used: 17/17

## Final Results

### Job 1: Prediction for m05 QED
- **Predicted QED: 0.5416** (quadratic extrapolation)
- Based on 5 congeneric molecules: m15, m24, m28, m17, m22
- Chain lengths: 2, 3, 4, 5, 6
- QED trend shows peak at chain length 3 (0.7919), then gradual decline
- Quadratic fit chosen over linear due to curved trend and lower RMSE

### Job 2: Backup lead compound
- **Selected lead: m02**
- Measured QED: 0.6046 ≥ 0.6 ✓
- PSA: 72.24 ≥ 40 ✓
- Not in m05's series: different scaffold (O=[N+]([O-])c1ccc(cc1)C(=O)N vs COc1ccc(cc1)C(=O)N) ✓

### Answer.json submitted with above values.