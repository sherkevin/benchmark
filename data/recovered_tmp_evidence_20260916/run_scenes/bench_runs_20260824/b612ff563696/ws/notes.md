# QED Prediction Task - Notes

## Target Molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- This is a methoxybenzene derivative with an amide linkage to a long alkyl chain (12 carbons)
- The N-alkyl chain length varies in the congeneric series

## Initial Search Results
Search query: `COc1ccc(cc1)C(=O)N` (scaffold)
- Total matches: 6
- All molecules in the series (ordered by chain length):
  1. **m15**: `COc1ccc(cc1)C(=O)NCC` (C2 chain) - QED: 0.7621
  2. **m24**: `COc1ccc(cc1)C(=O)NCCC` (C3 chain) - QED: 0.7919
  3. **m28**: `COc1ccc(cc1)C(=O)NCCCC` (C4 chain) - QED: 0.7518
  4. **m17**: `COc1ccc(cc1)C(=O)NCCCCC` (C5 chain) - QED: 0.7498
  5. **m22**: `COc1ccc(cc1)C(=O)NCCCCCC` (C6 chain) - QED: 0.7377
  6. **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (C12 chain) - target molecule (unmeasurable)

## Strategy
1. Identify the common scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzene-amide)
2. Search for molecules containing this scaffold in the pool
3. Measure QED values for found congeners
4. Analyze trend of QED vs. alkyl chain length
5. Extrapolate to predict QED for C12 chain (m05)

## Trend Analysis

### Measured Data Points
Chain length (n) vs QED:
- n=2 (C2): QED = 0.7621
- n=3 (C3): QED = 0.7919 (maximum)
- n=4 (C4): QED = 0.7518
- n=5 (C5): QED = 0.7498
- n=6 (C6): QED = 0.7377

### Pattern Observation
1. QED peaks at n=3 (C3 chain)
2. After the peak, QED decreases as chain length increases
3. The decrease from n=3 to n=6 appears roughly linear
4. The trend suggests a quadratic relationship with a maximum around n=3

### Mathematical Analysis
Let's consider two approaches:
1. Quadratic fit to capture the peak at n=3
2. Linear fit for n≥3 (post-peak region)

Given that we're extrapolating to n=12 (far beyond measured range), we should use the trend from n=3 to n=6 as the most relevant.

### Linear Regression (n=3 to n=6)
Using points (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377):
- Slope ≈ (0.7377 - 0.7919) / (6 - 3) = -0.0542 / 3 = -0.01807 per carbon
- For n=12: predicted QED = 0.7919 + (12-3)*(-0.01807) = 0.7919 + 9*(-0.01807) = 0.7919 - 0.1626 = 0.6293

### Quadratic Fit
Using all 5 points: QED appears to follow a concave-down quadratic with maximum around n=3.
A rough quadratic through (2,0.7621), (3,0.7919), (6,0.7377) suggests continued decrease beyond n=6.

## Prediction for m05 (C12 chain)
Based on the linear trend from n=3 to n=6:
- Slope = -0.01807 per carbon atom
- Extrapolation: QED at n=12 = 0.7919 + (12-3)*(-0.01807) = 0.7919 - 0.1626 = 0.6293

Considering:
1. The non-linear nature of QED with molecular properties
2. Potential for accelerated decrease with longer hydrophobic chains
3. QED typically decreases with increasing molecular weight and lipophilicity beyond optimal ranges
4. The C12 chain is significantly longer than measured congeners (C2-C6)

**Final Prediction: 0.62**

This prediction is slightly more conservative than the linear extrapolation (0.6293) to account for potential non-linear effects in the unmeasured region (n=6 to n=12).

## Budget Status
- Budget used: 7 units (2 search pages + 5 measurements)
- Budget remaining: 5 units