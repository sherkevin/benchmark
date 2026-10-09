# Notes: QED Prediction for m05

## Task Overview
- Target: m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - never synthesized
- Need to find congeners (same scaffold, different chain lengths)
- Measure their QED, fit a trend, extrapolate to predict m05's QED
- Budget: 12 units total (search pages + measurements)

## Strategy
The scaffold is `COc1ccc(cc1)C(=O)N` - searching for this should find all congeners.
The chain varies: CCCCCCCCCCC (11 carbons for m05).

## Search Results

### Search 1: scaffold "COc1ccc(cc1)C(=O)N"
Found 6 total molecules in 2 pages (cost: 2 budget units).

| mol_id | SMILES | Chain | Chain Length |
|--------|--------|-------|--------------|
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | 11C | 11 (TARGET) |
| m15 | COc1ccc(cc1)C(=O)NCC | 2C | 2 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | 5C | 5 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | 6C | 6 |
| m24 | COc1ccc(cc1)C(=O)NCCC | 3C | 3 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | 4C | 4 |

Congeners in same series as m05: m15, m24, m28, m17, m22 (chain lengths 2-6)

## QED Measurements

Measured all 5 congeners (cost: 5 budget units, total used: 7/12):

| mol_id | Chain Length | QED |
|--------|-------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Trend Analysis

The QED trend shows:
- Peak at chain length 3 (QED = 0.7919)
- Decreasing trend from chain 3 to 6
- Drops: 3→4: -0.0401, 4→5: -0.0020, 5→6: -0.0121

The pattern suggests a non-linear decrease after the peak. QED typically penalizes both very small and very large molecules; as chain length increases beyond the optimum, lipophilicity and molecular weight increase, reducing drug-likeness.

## Model Fitting

Tested several extrapolation models for chain length 11:

1. **Linear extrapolation** from chain 3-6: ~0.65
   - Average drop: 0.0181 per carbon
   - 0.7377 - 5*0.0181 = 0.647

2. **Quadratic fit** to last 3 points (chains 4-6): ~0.53
   - y = -0.00505u² + 0.00305u + 0.7518 (u = x-4)
   - At x=11: 0.526

3. **Exponential decay** with asymptote: ~0.66-0.67
   - y = ymin + (ymax-ymin)*exp(-k*(x-xpeak))
   - More physically realistic for QED (has natural lower bound)

## Final Prediction

The exponential decay model is most appropriate for QED because:
- QED has a natural lower bound (~0.3-0.5 for very large lipophilic molecules)
- The decay rate should slow as it approaches the asymptote
- This matches the observed pattern where the decrease isn't strictly linear

Using exponential decay model with ymin ≈ 0.4-0.5:
- Predicted QED for m05 (chain 11) ≈ 0.66-0.67

Given the uncertainty in extrapolating from chain 2-6 to chain 11, I'll use a conservative estimate in the middle of the model range.

**Final predicted QED for m05: 0.62**

This accounts for:
- The clear downward trend from the peak at chain 3
- The non-linear nature of QED degradation with increasing chain length
- Uncertainty in extrapolating 5 carbons beyond the measured range
