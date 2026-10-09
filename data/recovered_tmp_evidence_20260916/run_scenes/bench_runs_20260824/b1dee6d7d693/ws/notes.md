# QED Prediction Task - Notes

## Target Molecule
- ID: m05
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

## Initial Analysis
The target molecule has a methoxybenzamide scaffold with a long alkyl chain (12 carbons). The SMILES pattern shows:
- `COc1ccc(cc1)` - Methoxybenzene ring
- `C(=O)N` - Amide group
- `CCCCCCCCCCC` - 11-carbon alkyl chain (plus one carbon from the amide nitrogen, making 12 total carbons in the chain)

The key is to find molecules with the same scaffold but different chain lengths. The scaffold substring should be the part before the variable alkyl chain: `COc1ccc(cc1)C(=O)NC`

## Search Results
Using query `COc1ccc(cc1)C(=O)NC`, found 6 molecules total (2 pages, budget used: 2):

### Page 1:
1. m05 - Target molecule: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (12 carbons in alkyl chain)
2. m15 - `COc1ccc(cc1)C(=O)NCC` (2 carbons in alkyl chain)
3. m17 - `COc1ccc(cc1)C(=O)NCCCCC` (5 carbons in alkyl chain)
4. m22 - `COc1ccc(cc1)C(=O)NCCCCCC` (6 carbons in alkyl chain)

### Page 2:
5. m24 - `COc1ccc(cc1)C(=O)NCCC` (3 carbons in alkyl chain)
6. m28 - `COc1ccc(cc1)C(=O)NCCCC` (4 carbons in alkyl chain)

## Congeneric Series Analysis
All molecules share the same scaffold `COc1ccc(cc1)C(=O)NC` with different alkyl chain lengths:
- m15: C2 chain
- m24: C3 chain
- m28: C4 chain
- m17: C5 chain
- m22: C6 chain
- m05: C12 chain (target)

## Measured QED Values
Measured QED for 5 congeners (budget used: 5 measurements + 2 searches = 7 total, remaining budget: 5):
- m15 (C2 chain): QED = 0.7621
- m24 (C3 chain): QED = 0.7919
- m28 (C4 chain): QED = 0.7518
- m17 (C5 chain): QED = 0.7498
- m22 (C6 chain): QED = 0.7377

## Trend Analysis
Let me analyze the relationship between alkyl chain length (number of carbons) and QED:

Chain Length (Carbons) | QED
----------------------|-----
2 | 0.7621
3 | 0.7919
4 | 0.7518
5 | 0.7498
6 | 0.7377

### Pattern Observation:
1. QED peaks at C3 chain (0.7919)
2. QED decreases monotonically from C3 to C6
3. The decrease from C3 to C6 is approximately linear-looking
4. The slope appears to be roughly: (0.7377 - 0.7919) / (6 - 3) = -0.0542 / 3 = -0.0181 per carbon

### Mathematical Modeling:
Given the apparent peak at C3 and subsequent decrease, this suggests a quadratic relationship. I'll fit a quadratic model to the data to capture the curvature.

Quadratic model: QED = a × (chain_length)² + b × (chain_length) + c

Data points: (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)

Sums:
n = 5
Σx = 20
Σx² = 90
Σx³ = 440
Σx⁴ = 2274
Σy = 3.7933
Σxy = 15.0823
Σx²y = 67.5065

Normal equations:
1) 5c + 20b + 90a = 3.7933
2) 20c + 90b + 440a = 15.0823
3) 90c + 440b + 2274a = 67.5065

### Solution:
Solving the system gives:
a = -0.003264
b = 0.017022
c = 0.749324

Quadratic model: 
**QED = -0.003264 × (chain_length)² + 0.017022 × (chain_length) + 0.749324**

### Model Verification:
Chain Length | Predicted QED | Actual QED | Error
-------------|---------------|------------|-------
2 | 0.7703 | 0.7621 | +0.0082
3 | 0.7710 | 0.7919 | -0.0209
4 | 0.7647 | 0.7518 | +0.0129
5 | 0.7514 | 0.7498 | +0.0016
6 | 0.7312 | 0.7377 | -0.0065

The model captures the overall downward trend after the peak near C3.

### Alternative Linear Model (C4-C6):
Since we're extrapolating beyond C6, and the trend from C4 to C6 appears roughly linear, let's fit a line to the last 3 points:

Points: (4, 0.7518), (5, 0.7498), (6, 0.7377)
Slope: m = -0.00505
Intercept: b = 0.771417
Linear model: **QED = -0.00505 × (chain_length) + 0.771417**

For C12: QED = -0.00505×12 + 0.771417 = 0.710817 ≈ 0.711

### Relationship with AlogP:
Looking at the descriptors:
- m15 (C2): alogp = 1.445, QED = 0.7621
- m24 (C3): alogp = 1.835, QED = 0.7919
- m28 (C4): alogp = 2.225, QED = 0.7518
- m17 (C5): alogp = 2.615, QED = 0.7498
- m22 (C6): alogp = 3.005, QED = 0.7377
- m05 (C12): alogp = 4.956 (extrapolated)

AlogP increases by ~0.39 per carbon. QED seems to have an inverse relationship with alogp after C3.

## Final Prediction Analysis

### Multiple Model Approaches:
1. **Quadratic fit (all points)**: Predicts QED = 0.484 for C12
2. **Linear fit (C4-C6 only)**: Predicts QED = 0.695 for C12
3. **Linear fit (all points)**: Predicts QED = 0.686 for C12
4. **Slope extrapolation from C6**: Using slope -0.00705 per carbon → 0.695

### Key Considerations:
1. We're extrapolating 6 carbons beyond the measured range (C6 to C12)
2. All molecular descriptors (alogp, rotb) worsen linearly with chain length
3. QED weighs multiple properties that all degrade with longer chains
4. The decreasing trend from C4 to C6 is consistent

### Selected Prediction:
I choose **0.69** as the predicted QED for m05 (C12 chain), based on:
- Average of linear model predictions (0.686 and 0.695)
- Conservative extrapolation given the consistent downward trend
- Recognition that very long alkyl chains (C12) typically have poor drug-like properties due to high lipophilicity and excessive rotatable bonds

### Final Budget Summary:
- Search queries: 3 budget units (found all 6 congeners)
- Measurements: 5 budget units (measured QED for m15, m24, m28, m17, m22)
- Total used: 8 budget units
- Remaining: 4 budget units

### Congeneric Series Members:
All molecules sharing the scaffold `COc1ccc(cc1)C(=O)NC` with different alkyl chain lengths:
1. m15 (C2 chain): QED = 0.7621
2. m24 (C3 chain): QED = 0.7919
3. m28 (C4 chain): QED = 0.7518
4. m17 (C5 chain): QED = 0.7498
5. m22 (C6 chain): QED = 0.7377
6. m05 (C12 chain): Target for prediction

## Strategy
1. ~~Search for molecules containing the scaffold substring~~ ✓
2. ~~Identify congeneric series members~~ ✓
3. ~~Measure QED for these molecules~~ ✓
4. Analyze trend of QED vs. alkyl chain length
5. Extrapolate to predict QED for m05 (which has 12 carbons in the alkyl chain)