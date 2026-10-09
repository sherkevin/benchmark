# Notes for QED Prediction Task

## Target Molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- This is an amide with a methoxy-phenyl group and a dodecyl (C12) chain
- QED cannot be measured directly (probe_unavailable)

## Strategy
1. Search for molecules with the scaffold `COc1ccc(cc1)C(=O)N` to find congeners
2. Identify which molecules share the same scaffold but have different chain lengths
3. Measure QED for each congener
4. Fit a trend and extrapolate to predict m05's QED

## Step 1: Search for scaffold matches
Searched with substructure `COc1ccc(cc1)C(=O)N` to find all molecules with this scaffold.

**Page 1 results (4 of 6):**
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (C12 chain, rotb=12) - TARGET
- m15: `COc1ccc(cc1)C(=O)NCC` (C2 chain, rotb=3)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (C5 chain, rotb=6)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (C6 chain, rotb=7)

**Page 2 results (2 of 6):**
- m24: `COc1ccc(cc1)C(=O)NCCC` (C3 chain, rotb=4)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (C4 chain, rotb=5)

## Step 2: Measure QED for congeners
Measured QED for all 5 congeners in the same series (excluding m05):

| mol_id | Chain | Chain Length (n) | QED |
|--------|-------|------------------|-----|
| m15 | NCC (ethyl) | 2 | 0.7621 |
| m24 | NCCC (propyl) | 3 | 0.7919 |
| m28 | NCCCC (butyl) | 4 | 0.7518 |
| m17 | NCCCCC (pentyl) | 5 | 0.7498 |
| m22 | NCCCCCC (hexyl) | 6 | 0.7377 |

## Step 3: Analyze the trend
The QED values show:
- Peak at n=3 (QED=0.7919)
- Decline from n=3 to n=6

Rate of change:
- n=2 to n=3: +0.0298 (increase to peak)
- n=3 to n=4: -0.0401 (sharp decrease)
- n=4 to n=5: -0.0020 (small decrease)
- n=5 to n=6: -0.0121 (moderate decrease)

Overall trend from n=3 to n=6: decline of 0.0542 over 3 carbons = ~0.0181 per carbon

## Step 4: Extrapolate to n=12
Target m05 has a C12 chain (n=12), which is 6 carbons longer than the longest measured congener (n=6).

**Linear extrapolation from n=3 to n=6:**
- Slope = (0.7377 - 0.7919) / (6 - 3) = -0.01807 per carbon
- At n=12: QED = 0.7919 + (-0.01807 × 9) = 0.629

**Consideration of non-linear effects:**
The QED formula penalizes high molecular weight and lipophilicity (alogP). For m05:
- MW = 305.46 (vs 235.33 for n=6)
- alogP = 4.956 (vs 3.005 for n=6)
- rotb = 12 (vs 7 for n=6)

These values are significantly higher than optimal for QED, suggesting the penalty may accelerate for very long chains.

**Final prediction:**
Accounting for the observed linear trend (~0.63) and potential non-linear penalties at high chain lengths, I estimate the QED of m05 to be **0.60**.

## Congeners in the same series as m05
The congeners (same scaffold, different chain lengths) are: m15, m17, m22, m24, m28
