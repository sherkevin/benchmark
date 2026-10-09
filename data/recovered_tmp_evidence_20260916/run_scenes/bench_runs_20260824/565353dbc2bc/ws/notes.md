# Task Notes: Predicting QED and Selecting Lead Compound

## Initial Analysis

Task: Predict QED for m05 (unsynthesized) and select a backup lead compound meeting three constraints.

Target molecule m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxy-substituted phenyl ring with amide linkage to alkyl chain
- Chain length: N-alkyl chain has 12 carbons (CCCCCCCCCCC after NCC)

Total measurement budget: 12 measurements shared for both jobs.

## Step 1: Identify Congeners of m05

Looking at SMILES patterns, I need to identify molecules with the same scaffold as m05:
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (12C chain)
- m15: `COc1ccc(cc1)C(=O)NCC` (2C chain)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (5C chain)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6C chain)
- m24: `COc1ccc(cc1)C(=O)NCCC` (3C chain)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (4C chain)

These all share the `COc1ccc(cc1)C(=O)N` scaffold with varying alkyl chain lengths.

Also need to check: m06 has similar pattern but with nitro group (`O=[N+]([O-])`) instead of methoxy.
m04, m08, m11, m13, m16, m20, m23, m26, m30 have biphenyl scaffold.

So congeners of m05 are: m15, m17, m22, m24, m28.

## Step 2: Plan QED Measurements

For Job 1: Need to measure QED for m05 congeners to establish trend. Chain lengths:
- m15: 2C chain (shortest)
- m24: 3C chain
- m28:钓 fishing for answer
- m17: 5C chain
- m22: 6C chain
- m05: 12C chain (target, unavailable)

Chain lengths in congeners: need to count carbons in alkyl chain after "N" in "C(=O)N..."
Actually, looking at SMILES: after "C(=O)N" comes the alkyl chain.

For m15: `C(=O)NCC` - after N: C C (2 carbons)
m24: `C(=O)NCCC` - after N: C C C (3 carbons)
m28: `C(=O)NCCCC` - after N: C C C C (4 carbons)
m17: `C(=O)NCCCCC` - after N: C C C C C (5 carbons)
m22: `C(=O)NCCCCCC` - after N: C C C C C C (6 carbons)
m05: `C(=O)NCCCCCCCCCCC` - after N: 12 carbons

So chain lengths: m15=2, m24=3, m28=4, m17=5, m22=6, m05=12

We have 5 measurable congeners (all except m05).

## Step marvin: Job 2 - Lead Selection Constraints

Need to find molecule meeting:
1. measured QED ≥ 0.6
2. psa ≥ 40 (free to check)
3. not in same series as m05

Used screen_candidates(): Found 18 molecules with psa ≥ 40:
- Polyol-amide series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitro-phenyl-amide series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

All are NOT in m05's series (methoxy-phenyl-amide).

## Measurement Strategy

Total budget: 12 measurements. Need to:
1. Measure enough m05 congeners to establish trend (Job 1)
2. Measure candidate leads to find one with QED ≥ 0.6 (Job 2)

For Job 1: Should measure at least 3-4 congeners to fit polynomial trend (degree 2). For 5 congeners, might measure all to get good curve.

## Job 1: QED Measurements for m05 Congeners

Measured all 5 congeners:
- m15 (2C chain): QED = 0.7621
- m24 (3C chain): QED = 0.7919
- m28 (4C chain): QED = 0.7518
- m17 (5C chain): QED = 0.7498
- m22 (6C chain): QED = 0.7377

Chain lengths: 2, 3, 4, 5, 6 carbons
Corresponding QEDs: 0.7621, 0.7919, 0.7518, 0.7498, 0.7377

Pattern: QED seems to peak around 3 carbons (m24: 0.7919) then decreases as chain lengthens.

Target m05 has 12C chain (much longer).

For Job 2: Need to find at least one candidate with QED ≥ 0.6. Might need to measure several candidates.

Remaining budget: 7 measurements (12 total - 5 used).

## Job 1: Trend Analysis and Prediction for m05

Using fit_trend with degree=2 (quadratic):
- Chain lengths (x): [2, 3, 4, 5, 6]
- QEDs (y): [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- Prediction for chain length 12: 0.4836

Using fit_trend with degree=1 (linear):
- Prediction for chain length 12: 0.6859

The data shows QED peaks at 3-carbon chain (0.7919) then decreases. Quadratic fit captures this curvature and predicts lower QED (0.4836) for longer 12-carbon chain. Linear fit predicts 0.6859.

Given the clear downward trend after peak, quadratic prediction (0.4836) seems more realistic for extrapolation.

## Job 2: Lead Candidate Search

Measured several candidates from psa ≥ 40 pool:

Polyol-amide series:
- m01: QED = 0.3187 (< 0.6)
- m03: QED = 0.3282 (< 0.6)

Nitro-phenyl-amide series:
- m02: QED = 0.6046 (≥ 0.6) ✓
- m06: QED = 0.4011 (< 0.6)
- m09: QED = 0.5826 (< 0.6)
- m10: QED = 0.3724 (< 0.6)
- m12: QED = 0.4709 (< 0.6)

m02 satisfies all constraints:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. not in m05 series (nitro vs methoxy scaffold) ✓

No other measured candidate had QED ≥ 0.6.

Budget used: 12/12 measurements.

## Final Answer

Job 1 - Predicted QED for m05:
- Using quadratic fit (degree=2) based on 5 congeners (chain lengths 2-6C)
- Predicted QED for 12C chain (m05): 0.4836
- Congener series: m15, m24, m28, m17, m22

Job 2 - Selected lead compound:
- m02 meets all three constraints:
  1. Measured QED = 0.6046 ≥ 0.6
  2. psa = 72.24 ≥ 40 (free descriptor)
  3. Not in m05 series (nitro-phenyl-amide vs methoxy-phenyl-amide scaffold)
- No other measured candidate had QED ≥ 0.6

Answer.json has been updated accordingly.