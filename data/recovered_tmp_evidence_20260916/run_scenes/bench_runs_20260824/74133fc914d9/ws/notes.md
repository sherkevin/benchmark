# Notes for QED Prediction Task

## Task Summary
Predict QED for molecule m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) which cannot be directly measured.

## Strategy
1. Search for congeners using a scaffold substructure
2. Measure QED of congeners in the same series
3. Fit a trend and extrapolate to predict m05's QED

## Step 1: Finding congeners

Searched using scaffold substructure: `COc1ccc(cc1)C(=O)N`

Found 6 total matches:
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - 13 carbons (TARGET - cannot measure)
- m15: `COc1ccc(cc1)C(=O)NCC` - 2 carbons (ethyl)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` - 5 carbons (pentyl)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` - 6 carbons (hexyl)
- m24: `COc1ccc(cc1)C(=O)NCCC` - 3 carbons (propyl)
- m28: `COc1ccc(cc1)C(=O)NCCCC` - 4 carbons (butyl)

**Congeners in the same series as m05**: m15, m17, m22, m24, m28

## Step 2: Measuring QED of congeners

| mol_id | Chain Length | SMILES suffix | QED |
|--------|--------------|---------------|-----|
| m15 | 2 | NCC (ethyl) | 0.7621 |
| m24 | 3 | NCCC (propyl) | 0.7919 |
| m28 | 4 | NCCCC (butyl) | 0.7518 |
| m17 | 5 | NCCCCC (pentyl) | 0.7498 |
| m22 | 6 | NCCCCCC (hexyl) | 0.7377 |

## Step 3: Trend Analysis

The QED trend shows:
- Peak at chain length 3 (propyl): QED = 0.7919
- Decreasing trend for chains 3-6

Chain length vs QED:
- 2 → 0.7621
- 3 → 0.7919 (peak)
- 4 → 0.7518
- 5 → 0.7498
- 6 → 0.7377

For extrapolation to chain 13, I focus on the longer chains (4, 5, 6) as they provide the most relevant trend for long-chain extrapolation.

### Linear Fit on chains 4-6

Using least squares regression on points (4, 0.7518), (5, 0.7498), (6, 0.7377):

- Slope: -0.00705 QED units per carbon
- Intercept: 0.78165

**Fitted trend: QED = 0.78165 - 0.00705 × chain_length**

### Extrapolation to chain 13

QED(13) = 0.78165 - 0.00705 × 13 = 0.78165 - 0.09165 = **0.690**

## Prediction

**Predicted QED for m05: 0.69**

This accounts for:
1. The observed decreasing trend in QED with increasing chain length
2. The significant extrapolation from chain 6 to chain 13
3. The fact that longer chains increase lipophilicity (alogp) and molecular weight, both of which reduce drug-likeness

The congener IDs in the same series: ["m15", "m17", "m22", "m24", "m28"]
