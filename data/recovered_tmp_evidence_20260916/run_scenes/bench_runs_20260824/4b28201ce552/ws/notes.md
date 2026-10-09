# Notes on QED Prediction and Lead Selection

## Task Overview
1. Predict QED for m05 (unmeasurable - never synthesized)
2. Pick a backup lead compound with: QED ≥ 0.6, psa ≥ 40, NOT in m05's series

## m05 Analysis
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxy-phenyl amide `COc1ccc(cc1)C(=O)N-`
- Chain: 13 carbons (CCCCCCCCCCC)

## Identifying Congeners (same scaffold, different chain length)

Looking at pool SMILES for `COc1ccc(cc1)C(=O)N-` pattern:
- m15: `COc1ccc(cc1)C(=O)NCC` - chain length 2
- m17: `COc1ccc(cc1)C(=O)NCCCCC` - chain length 5
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` - chain length 6
- m24: `COc1ccc(cc1)C(=O)NCCC` - chain length 3
- m28: `COc1ccc(cc1)C(=O)NCCCC` - chain length 4

Congeners: m15, m17, m22, m24, m28 (5 molecules)

## Plan
1. Measure QED for all 5 congeners to establish trend
2. Fit polynomial trend and extrapolate to chain length 13
3. For lead selection: find molecules with psa ≥ 40, not in m05 series, measure QED until finding one ≥ 0.6

## Budget: 12 measurements total

## Measurements Taken (Congeners)
- m15 (chain 2): QED = 0.7621
- m24 (chain 3): QED = 0.7919
- m28 (chain 4): QED = 0.7518
- m17 (chain 5): QED = 0.7498
- m22 (chain 6): QED = 0.7377

## Trend Fitting
Used quadratic fit (degree=2) as instructed ("Do not assume the relationship is a straight line").

Coefficients (low to high): [0.74932, 0.017024, -0.003264]
RMSE: 0.01188

Extrapolation to chain length 13: **QED = 0.419**

(Linear fit gave 0.6768 but quadratic is preferred per task instructions)

## Lead Selection
Screened 18 candidates with psa ≥ 40. None were in m05's series (those have psa=38.33).

Measured QED for candidates:
- m01: 0.3187 (fails QED ≥ 0.6)
- m02: 0.6046 ✓ PASS (QED ≥ 0.6, psa=72.24 ≥ 40, not in m05 series)
- m03: 0.3282 (fails)
- m09: 0.5826 (fails)
- m12: 0.4709 (fails)

**Selected lead: m02**
- QED: 0.6046
- psa: 72.24
- Series: nitro-phenyl amide (different from m05's methoxy-phenyl amide)

## Final Answer
- target_qed: 0.419
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]
