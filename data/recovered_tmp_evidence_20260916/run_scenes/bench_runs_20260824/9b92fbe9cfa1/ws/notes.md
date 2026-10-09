# Notes: QED Prediction and Lead Compound Selection

## Task Overview
- Predict QED for m05 (never synthesized, probe_unavailable)
- Select a backup lead compound with QED ≥ 0.6, psa ≥ 40, not in m05's series

## Step 1: Identify m05's Congeneric Series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: 4-methoxybenzamide (`COc1ccc(cc1)C(=O)N`)
- Chain length: 11 carbons

Congeners in the same series (same scaffold, different chain lengths):
- m15: `COc1ccc(cc1)C(=O)NCC` (chain=2)
- m24: `COc1ccc(cc1)C(=O)NCCC` (chain=3)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (chain=4)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain=5)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain=6)

## Step 2: Measure Congeners to Establish Trend

Measured QED values:
| mol_id | chain length | QED |
|--------|--------------|-----|
| m15    | 2            | 0.7621 |
| m24    | 3            | 0.7919 |
| m28    | 4            | 0.7518 |
| m17    | 5            | 0.7498 |
| m22    | 6            | 0.7377 |

## Step 3: Fit Trend and Extrapolate

The QED trend shows:
- Peak at chain length 3 (QED=0.7919)
- Decline thereafter: 0.7518 (4) → 0.7498 (5) → 0.7377 (6)

Decline rates:
- 3→4: -0.0401
- 4→5: -0.0020
- 5→6: -0.0121

Overall decline from peak (chain 3) to chain 6: 0.0542 over 3 steps = ~0.018/step average

For extrapolation to chain 11 (5 steps beyond chain 6):
- Using average decline rate: 0.7377 - 5×0.018 ≈ 0.65
- Considering non-linearity and that longer chains typically reduce drug-likeness more due to increased MW and logP

The relationship is not purely linear - QED penalizes high lipophilicity and molecular weight increasingly. m05 has MW=305.46 and alogp=4.956, significantly higher than m22 (MW=235.33, alogp=3.005).

**Prediction: QED(m05) ≈ 0.62**

This accounts for:
- The observed declining trend from the peak
- The accelerating penalty for very long alkyl chains on drug-likeness

## Step 4: Select Backup Lead Compound

Constraints:
1. QED ≥ 0.6 (must measure)
2. psa ≥ 40 (free from catalogue)
3. Not in m05's series (methoxybenzamide)

Candidates with psa ≥ 40, excluding methoxybenzamide series:
- Sugar amide series (OCC(O)C(O)C(O)C(=O)N*): m01, m03, m07, m14, m18, m19, m21, m25, m27 (psa=110.02)
- Nitrobenzamide series (O=[N+]([O-])c1ccc(cc1)C(=O)N*): m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)

Measurements (budget: 17 total, 5 used for congeners + 5 for m15/m24/m28/m17/m22 = 10 used):
- m01: QED=0.3187 ✗ (below 0.6)
- m02: QED=0.6046 ✓ (meets all criteria!)
- m03: QED=0.3282 ✗ (below 0.6)

**Selected lead: m02**
- QED = 0.6046 (≥ 0.6 ✓)
- psa = 72.24 (≥ 40 ✓)
- Series: nitrobenzamide, not methoxybenzamide ✓

## Budget Summary
- Total budget: 17 measurements
- Congener measurements: m15, m24, m28, m17, m22 (5 measurements)
- Lead candidate measurements: m01, m02, m03 (3 measurements)
- Total used: 8 measurements (well within budget)

Wait, let me recount - the budget showed 14 used after the 5 congener measurements, then 15, 16, 17 after m01, m02, m03. So total 8 measurements for QED.

Actually looking at the budget_used values:
- After listing 9 pages: budget_used = 9 (list_pool and measure_qed share budget)
- After m15: 10
- After m24: 11
- After m28: 12
- After m17: 13
- After m22: 14
- After m01: 15
- After m02: 16
- After m03: 17

Total: 8 QED measurements + 9 list_pool calls = 17 budget units. Perfect.
