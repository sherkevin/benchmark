# Medicinal Chemistry Task Notes

## Task Overview
1. Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - never synthesized
2. Pick a backup lead compound meeting:
   - Measured QED ≥ 0.6
   - PSA ≥ 40 (free descriptor)
   - Not in same series as m05

Budget: 17 total measurements shared between both jobs.

## Initial Plan
1. First, need to identify the series that m05 belongs to by examining SMILES pattern
2. List pool molecules to find congeneric series members
3. Measure QED for series members to establish trend
4. Use fit_trend to extrapolate to m05's chain length
5. Screen candidates for PSA ≥ 40
6. Identify series memberships from SMILES
7. Measure QED for promising candidates to find one with QED ≥ 0.6

## Step 1: Understand m05's SMILES pattern
m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

Breaking down:
- `COc1ccc(cc1)C(=O)N` - scaffold with methoxy phenyl group, carbonyl, amine
- `CCCCCCCCCCCC` - 12-carbon alkyl chain (C12)

Key features:
- Methoxy phenyl (`COc1ccc(cc1)`)
- Carbonyl group (`C(=O)`)
- Amine (`N`)
- Long alkyl chain (C12)

Need to find molecules with same scaffold but different chain lengths.

## Step 2: Identified Congeneric Series for m05

Found 6 molecules in the same methoxy phenyl series:
1. m15: `COc1ccc(cc1)C(=O)NCC` - C2 chain (2 carbons)
2. m24: `COc1ccc(cc1)C(=O)NCCC` - C3 chain (3 carbons)  
3. m28: `COc1ccc(cc1)C(=O)NCCCC` - C4 chain (4 carbons)
4. m17: `COc1ccc(cc1)C(=O)NCCCCC` - C5 chain (5 carbons)
5. m22: `COc1ccc(cc1)C(=O)NCCCCCC` - C6 chain (6 carbons)
6. m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - C12 chain (12 carbons, target)

All share scaffold: `COc1ccc(cc1)C(=O)N`

Note: m05 is in the pool list but "never been synthesized" according to task description.

Budget status: 9 used, 8 remaining (17 total budget).

## Step 3: Measured QED for series members

Measured QED for methoxy phenyl series members:
1. m15 (C2 chain): QED = 0.7621
2. m24 (C3 chain): QED = 0.7919  
3. m28 (C4 chain): QED = 0.7518
4. m17 (C5 chain): QED = 0.7498
5. m22 (C6 chain): QED = 0.7377

Chain lengths (x) and QED values (y):
- x=2, y=0.7621
- x=3, y=0.7919
- x=4, y=0.7518
- x=5, y=0.7498
- x=6, y=0.7377

Target m05 has chain length x=12.

Budget status: 14 used, 3 remaining.

## Step 4: Fit trend and predict QED for m05

Tried different polynomial fits:
1. Degree 1 (linear) using all points: prediction = 0.6859, RMSE = 0.01308
2. Degree 2 (quadratic) using all points: prediction = 0.4836, RMSE = 0.01188 (best RMSE)
3. Degree 3 (cubic) using all points: prediction = 2.8995 (unrealistic >1), RMSE = 0.00835
4. Degree 4 using all points: prediction = -21.9274 (negative, unrealistic), RMSE = 0.0

Also tried linear fits on subsets:
- Using last 3 points (C4-C6): prediction = 0.6971
- Using C3-C6: prediction = 0.6343

The quadratic model (degree 2) has the best RMSE among realistic models and captures the peak at C3 followed by decrease. Given we're extrapolating from max x=6 to x=12 (far beyond range), the quadratic prediction of 0.4836 seems reasonable.

**Prediction for m05 (C12 chain): 0.4836**

## Step 5: Find backup lead compound meeting all constraints

Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free descriptor)
3. Not in same series as m05 (methoxy phenyl)

Screened candidates with PSA ≥ 40: 18 molecules pass.

Checked series membership: None of the methoxy phenyl series (m15, m24, m28, m17, m22) have PSA ≥ 40 (all have PSA=38.33), so all 18 passing molecules automatically satisfy constraint 3.

Measured candidates:
1. m09 (nitro phenyl C2): QED = 0.5826 < 0.6 ❌
2. m02 (nitro phenyl C3): QED = 0.6046 ≥ 0.6 ✓

**Selected lead compound: m02**

m02 satisfies all constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓  
3. Not in same series as m05 (nitro phenyl vs methoxy phenyl) ✓

Budget status: 16 used, 1 remaining.

## Step 6: Prepare final answer

Filled answer.json with:
- target_qed: 0.4836 (predicted for m05 using quadratic fit with degree 2 on all 5 measured points)
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046 (measured value for m02)
- congener_ids: ["m15", "m24", "m28", "m17", "m22"] (all methoxy phenyl series members in pool except m05 itself)

## Final Summary

1. **Predicted QED for m05**: 0.4836
   - Based on quadratic fit (degree 2) to measured QED values of congeneric series members (chain lengths 2-6)
   - Extrapolated to chain length 12
   - Rationale: Quadratic model captures peak at C3 and subsequent decrease, has best RMSE (0.01188) among realistic models

2. **Selected lead compound**: m02
   - Satisfies all three constraints:
     - Measured QED = 0.6046 ≥ 0.6
     - PSA = 72.24 ≥ 40 (from catalogue)
     - Not in same series as m05 (nitro phenyl vs methoxy phenyl)
   - SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`

3. **Congeneric series members for m05**: m15, m24, m28, m17, m22
   - All share scaffold `COc1ccc(cc1)C(=O)N` with varying alkyl chain lengths (C2-C6)
   - m05 has C12 chain (extrapolation target)