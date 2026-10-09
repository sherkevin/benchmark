# Notes for QED Prediction Task

## Task Summary
- Predict QED for molecule m05 (never synthesized, same series extrapolation needed)
- Pick a backup lead compound with: QED ≥ 0.6, psa ≥ 40, NOT in m05's series
- Budget: 17 measurements total

## Key Info
- m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (11-carbon chain: NCCCCCCCCCCC)
- Need to find congeners (same scaffold, different chain lengths)

## Series Identification

m05 scaffold: `COc1ccc(cc1)C(=O)N-` (methoxy-phenyl-amide)

Congeners in pool (same scaffold, different chain lengths):
- m15: `COc1ccc(cc1)C(=O)NCC` - 2 carbons (ethyl)
- m24: `COc1ccc(cc1)C(=O)NCCC` - 3 carbons (propyl)
- m28: `COc1ccc(cc1)C(=O)NCCCC` - 4 carbons (butyl)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` - 5 carbons (pentyl)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` - 6 carbons (hexyl)

m05 has 11 carbons - need to extrapolate from 2-6 to 11.

Other series (NOT m05 series):
- Sugar-like: m01, m03, m07, m14, m18, m19, m21, m25, m27 (psa=110.02)
- Nitro-phenyl: m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)
- Biphenyl: m04, m08, m11, m13, m16, m20, m23, m26, m30 (psa=29.1)

## Step 2: Measure QED for m05 congeners

Measurements:
- m15 (n=2): QED = 0.7621
- m24 (n=3): QED = 0.7919
- m28 (n=4): QED = 0.7518
- m17 (n=5): QED = 0.7498
- m22 (n=6): QED = 0.7377

## Trend Fitting

Fitted polynomial (degree 2) through points (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377):

Coefficients (low to high): [0.74932, 0.017024, -0.003264]
RMSE: 0.01188

Extrapolation to x=11 (m05 chain length):
- Degree 1 (linear): 0.695, RMSE 0.01308
- Degree 2 (quadratic): 0.5416, RMSE 0.01188 ← Best choice
- Degree 3 (cubic): 2.1323 (unrealistic, >1)

**Predicted QED for m05: 0.5416** (using degree 2 polynomial)

## Lead Compound Selection

Candidates with psa ≥ 40: 18 molecules (all pass constraint 3 since m05 series has psa=38.33 < 40)

Measured QED for potential leads:
- m02: QED = 0.6046 ≥ 0.6 ✓ (psa=72.24, nitro-phenyl series - not m05 series)
- m09: QED = 0.5826 < 0.6 ✗
- m12: QED = 0.4709 < 0.6 ✗

**Selected lead: m02**
- QED = 0.6046 (measured)
- psa = 72.24 (≥ 40)
- Not in m05 series (nitro-phenyl scaffold, not methoxy-phenyl)

## Final Answer
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"]
