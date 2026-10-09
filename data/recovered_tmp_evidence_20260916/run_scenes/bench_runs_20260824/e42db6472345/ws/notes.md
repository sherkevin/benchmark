# Task Notes

## Understanding the Task

We have 33 molecules in the pool (`m01` to `m33`). The target molecule `m05` has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

### Job 1: Predict QED of m05
1. Identify which pool molecules belong to the same series as `m05` (same scaffold, only N-alkyl chain length differs)
2. Measure QED for members of this series
3. Fit a trend and extrapolate to predict `m05`'s QED
4. The pool only has short-chain members of the target's series, so we're extrapolating beyond measured range

### Job 2: Pick a backup lead compound
1. Must have measured QED ≥ 0.6
2. Must have psa ≥ 40 (free to check from pool.tsv)
3. Must NOT be in same series as `m05`
4. Total measurement budget: 12 measurements shared between both jobs

## Initial Observations

Looking at `m05` SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Contains methoxy group (`COc`)
- Contains benzamide-like structure with N-alkyl chain
- The N-alkyl chain appears to be `NCCCCCCCCCCC` (11 carbons after N)

## Identifying Congeners of m05

Based on SMILES pattern `COc1ccc(cc1)C(=O)N`, I've identified these molecules as sharing the same scaffold:
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (target, chain_len = 11)
- m15: `COc1ccc(cc1)C(=O)NCC` (chain_len = 2)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain_len = 5)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain_len = 6)
- m24: `COc1ccc(cc1)C(=O)NCCC` (chain_len = 3)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (chain_len = 4)

All have the same psa value of 38.33, confirming they share the same scaffold.

## Strategy for Job 1
1. Measure QED for available congeners (m15, m24, m28, m17, m22) - 5 measurements
2. Fit polynomial trend (degree 2 likely) to chain_len vs QED
3. Extrapolate to chain_len = 11 to predict m05 QED

## Job 1: Measurements for m05 Series

I've measured QED for all available congeners of m05:
- m15 (chain_len = 2): QED = 0.7621
- m24 (chain_len = 3): QED = 0.7919
- m28 (chain_len = 4): QED = 0.7518
- m17 (chain_len = 5): QED = 0.7498
- m22 (chain_len = 6): QED = 0.7377

Observations: QED peaks at chain_len=3 (0.7919) then decreases.

## Trend Analysis for m05 Prediction

Using `fit_trend` with all 5 points:
- Linear fit (degree=1): predicts QED = 0.695 for chain_len=11
- Quadratic fit (degree=2): predicts QED = 0.5416 for chain_len=11

The quadratic fit has slightly lower RMSE (0.01188 vs 0.01308) and shows a concave down pattern (negative quadratic coefficient = -0.003264), which matches the observed peak and subsequent decrease. Given that we're extrapolating far beyond measured range (6 → 11), the quadratic prediction of 0.5416 seems more conservative and realistic.

**Decision:** I'll use the quadratic prediction of 0.5416 for m05's QED.

## Job 2: Lead Compound Selection

From `screen_candidates()`, 18 molecules have psa ≥ 40:
- m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

None of m05's series molecules have psa ≥ 40, so all 18 candidates satisfy constraint 3 (not in same series as m05).

I used 7 measurements to test candidates:
- m01: QED = 0.3187 < 0.6
- m02: QED = 0.6046 ≥ 0.6 ✓
- m06: QED = 0.4011 < 0.6
- m03: QED = 0.3282 < 0.6
- m09: QED = 0.5826 < 0.6
- m10: QED = 0.3724 < 0.6
- m12: QED = 0.4709 < 0.6

**Lead compound choice:** m02 satisfies all constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in same series as m05 (different scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` vs m05's `COc1ccc(cc1)C(=O)N`) ✓

I used all 12 measurements: 5 for m05 series + 7 for lead search.

## Summary of Results

### Job 1: m05 QED Prediction
- Congeners of m05: m15, m17, m22, m24, m28
- Measured QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377] for chain lengths [2, 3, 4, 5, 6]
- Quadratic fit (degree=2) predicts QED = 0.5416 for m05 (chain_len=11)
- Linear fit predicts 0.695, but quadratic has better fit to observed curvature

### Job 2: Lead Compound Selection
- Tested 7 candidates: m01, m02, m06, m03, m09, m10, m12
- Found m02 meets all constraints:
  - Measured QED = 0.6046 ≥ 0.6
  - psa = 72.24 ≥ 40
  - Different scaffold from m05 (`O=[N+]([O-])c1ccc(cc1)C(=O)N` vs `COc1ccc(cc1)C(=O)N`)

### Final Answer
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]