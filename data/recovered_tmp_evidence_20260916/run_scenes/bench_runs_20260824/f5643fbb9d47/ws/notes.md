# Medicinal Chemistry Task Notes

## Initial Analysis
Target molecule m05 has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

This is an N-alkyl chain amide derivative. Need to:
1. Identify molecules in the pool that share the same scaffold as m05 (same congeneric series)
2. Measure their QED values to establish trend
3. Extrapolate to predict m05's QED
4. Find a lead compound meeting all three constraints

## Step 1: Understand m05's structure
Using describe_structure on m05:
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Chain length: 11 carbons
- Pool only contains short-chain members of this series, so we need to extrapolate.

## Step 2: Explore pool to find congeneric series
Pool has 33 molecules total, 4 per page. Budget used: 9/17 (list_pool pages 1-9)

Found molecules with scaffold `COc1ccc(cc1)C(=O)N` (same as m05):
- m15: chain length 2
- m24: chain length 3  
- m28: chain length 4
- m17: chain length 5
- m22: chain length 6
- m05: chain length 11 (target, not synthesized)

These are all members of the same congeneric series. Need to measure QED for m15, m24, m28, m17, m22 to establish trend.

## Step 3: Measure QED values for series members
Budget used: 14/17 (9 list_pool + 5 measure_qed). Remaining: 3 measurements.

QED measurements for series members:
- m15 (chain length 2): 0.7621
- m24 (chain length 3): 0.7919
- m28 (chain length 4): 0.7518
- m17 (chain length 5): 0.7498
- m22 (chain length 6): 0.7377
- m05 (chain length 11): need to predict

Pattern: QED peaks around chain length 3 (0.7919) then decreases as chain gets longer.

## Step 4: Predict m05's QED
Using fit_trend with all 5 points:
- Quadratic fit (degree=2): predicts QED = 0.5416 for chain length 11, RMSE = 0.01188
- Linear fit (degree=1): predicts QED = 0.695 for chain length 11, RMSE = 0.01308

Quadratic fits slightly better (lower RMSE). The decreasing trend suggests longer chains reduce QED, so 0.5416 seems plausible.

Will use quadratic prediction: target_qed = 0.5416

## Step 5: Find lead compound meeting all constraints
Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in same series as m05

Using screen_candidates(): 18 molecules have psa ≥ 40.

Checked series membership: Need to avoid m05's series (m15, m24, m28, m17, m22).

Measured candidates:
- m01 (polyol): QED = 0.3187 < 0.6 ❌
- m02 (nitro): QED = 0.6046 ≥ 0.6 ✓, psa = 72.24 ✓, not in m05 series ✓
- m06 (nitro): QED = 0.4011 < 0.6 ❌

Found valid lead: m02 meets all constraints.
m02 scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (different from m05's scaffold)
m02 QED: 0.6046 (measured)
m02 psa: 72.24 ≥ 40

## Final Results
- Predicted QED for m05 (target_qed): 0.5416 (quadratic extrapolation from series trend)
- Lead compound: m02 with measured QED 0.6046
- Congeneric series members (same as m05): m15, m24, m28, m17, m22

All budget used: 17/17 measurements.