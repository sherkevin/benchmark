# QED Prediction and Lead Selection Notes

## Initial Analysis

The target molecule m05 has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

From the task description:
1. Need to predict QED of m05 (target molecule)
2. Need to select a lead compound meeting 3 constraints:
   - measured QED ≥ 0.6
   - psa ≥ 40 (free descriptor)
   - not in same series as m05

## Step 1: Identify m05's congeneric series

First, I need to examine m05's structure and identify which molecules share the same scaffold.

The SMILES pattern for m05 appears to be: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
This appears to have a methoxybenzene core with an amide linkage to a long alkyl chain.

Let me use describe_structure on m05 to understand its scaffold and chain length:

Using describe_structure on m05:
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Chain length: 11 carbons

Potential congeneric series members (same scaffold):
- m15: `COc1ccc(cc1)C(=O)NCC` (chain length 2) ✓ confirmed same scaffold
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain length 5) ✓ confirmed same scaffold  
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain length 6)
- m24: `COc1ccc(cc1)C(=O)NCCC` (chain length 3)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (chain length 4)

So congener_ids for m05 series: m15, m17, m22, m24, m28 (and m05 itself, though not synthesized).

## Step                                                2: Measure QED for m05 series

Measurements taken:
1. m15 (chain length 2): QED = 0.7621
2. m17 (chain length 5): QED = 0.7498  
3. m22 (chain length 6): QED = 0.7377
4. m24 (chain length 3): QED = 0.7919
5. m28 (chain length 4): QED = 0.7518

Observations: QED seems to decrease as chain length increases, though not perfectly monotonic. Highest QED at chain length 3 (0.7919), lowest so far at chain length 6 (0.7377).

Budget used: 5 measurements, 7 remaining.

## Step 3: Fit trend for m05 prediction

Using fit_trend with degree=2 (quadratic):
- Chain lengths (x): [2, 3, 4, 5, 6]  
- QED values (y): [0.7621, 0.7919, 0.7518, 0.7498, IIII, 0.7377]
- Target: chain length 11
- Prediction: 0.5416
- RMSE: 0.01188
- Coefficients: [0.74932, 0.017024, -0.003264] (constant, linear, quadratic)

Comparison with linear fit (degree=1):
- Prediction: 0.695
- RMSE: 0.01308
- Coefficients: [0.79502, -0.00909] (intercept, slope)

The quadratic fit has slightly better RMSE and captures the curvature in the data (peak at chain length 3). Given task instruction "Do not assume the relationship is a straight line", using quadratic prediction of 0.5416 for m05.

Target QED prediction: 0.5416

## Step 4: Job 2 - Find lead compound

Using screen_candidates(): 18 molecules pass psa ≥ 40 constraint.

Measured QED for candidates:
1. m01 (polyhydroxy series): QED = 0.3187 ✗ below 0.6
2. m02 (nitrobenzene series): QED = 0.6046 ✓ meets all criteria!
3. m06 (nitrobenzene, longer chain): QED = 0.4011 ✗
4. m09 (nitrobenzene, shorter chain): QED = 0.5826 ✗ (slightly below 0.6)
5. m03 (polyhydroxy): QED = 0.3282 ✗
6. m10 (nitrobenzene, long chain): QED = 0.3724 ✗
7. m12 (nitrobenzene, medium chain): QED = 0.4709 ✗

Budget: Used all 12 measurements.

Best candidate found: m02
- QED = 0.6046 (≥ 0.6) ✓
- psa = 72.24 (≥ 40) ✓  
- Not in m05 series (scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` vs m05's `COc1ccc(cc1)C(=O)N`) ✓

All constraints satisfied.

## Summary

Job 1 - m05 QED prediction:
- Measured all 5 available congeners in m05 series (m15, m17, m22, m24, m28)
- Observed QED trend: peaks at chain length 3 (0.7919), decreases with longer chains
- Fitted quadratic polynomial (degree=2) as instructed
- Predicted QED for m05 (chain length 11): 0.5416

Job 2 - Lead compound selection:
- Screened for psa ≥ 40: 18 candidates
- Measured 7 candidates (using remaining budget)
- Found m02 meets all criteria: QED=0.6046 (≥0.6), psa=72.24 (≥40), not in m05 series
- Alternative candidates either had QED < 0.6 or were in polyhydroxy series with very low QED

Final answer:
- target_qed: 0.5416  
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]