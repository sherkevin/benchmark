# Notes for QED Prediction Task

## Initial Analysis

Target molecule m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

Key structural features:
- Methoxybenzene scaffold (COc1ccc(cc1))
- Amide linkage (C(=O)N)
- Long alkyl chain (12 carbons)

Looking for molecules with same scaffold pattern. Let me examine SMILES patterns:

1. Series with `COc1ccc(cc1)C(=O)N` - methoxybenzene-amide scaffold:
   - m15: `COc1ccc(cc1)C(=O)NCC` (chain length: 2 carbons)
   - m24: `COc1ccc(cc1)C(=O)NCCC` (chain length: 3 carbons)  
   - m28: `COc1ccc(cc1)C(=O)NCCCC` (chain length: R = C4H9, so 4 carbons)
   - m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain length: 5 carbons)
   - m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain length: 6 carbons)
   - m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (chain length: 12 carbons)

This appears to be a congeneric series varying only in N-alkyl chain length.

## Identifying Series Members

From visual inspection of SMILES patterns, I can identify these series:

1. **Methoxybenzene series (m05's series)**: m15, m24, m28, m17, m22, m05
   - All have `COc1ccc(cc1)C(=O)N` scaffold
   - Differ only in alkyl chain length

2. **Nitrophenol series**: m02, m09, m12, m32, m33, m29, m06, m31, m10
   - All have `O=[N+]([O-])c1ccc(cc1)C(=O)N` scaffold
   - Contain nitro group

3. **Diol series**: m14, m27, m01, m19, m03, m21, m18, m07, m25
   - All have `OCC(O)C(O)C(O)C(=O)N` scaffold
   - Contain multiple hydroxyl groups

4. **Biphenyl series**: m13, m11, m04, m30, m23, m20, m08, m26, m16
   - All have `c1ccc(cc1)-c1ccc(cc1)C(=O)N` scaffold
   - Contain biphenyl core

## m05's Congeneric Series Members

Confirmed series members (all share `COc1ccc(cc1)C(=O)N` scaffold):
.### Chain length vs QED measurements:
- m15: chain length 2 carbons → QED = 0.7621
- m24: chain length 3 carbons → QED = 0.7919  
- m28: chain length 4 carbons → QED = 0.7518
- m17: chain length 5 carbons → QED = 0.7498
- m22: chain length 6 carbons → QED = 0.7377
- m05: chain length 12 carbons → TARGET (need to predict)

### Trend Analysis:
Chain lengths: [2, 3, 4, 5, 6]
QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

Observations:
1. QED peaks at chain length 3 (0.7919)
2. After chain length 3, QED decreases monotonically
3. The trend appears to be decreasing after the peak

Note: m05 has chain length 12, while the measured pool molecules have chain lengths 2-6. This means we need to extrapolate beyond the measured range.

## Extrapolation Models for m05 (chain length 12):

### Model 1: Linear fit to all points
Slope = (0.7377 - 0.7621) / (6 - 2) = -0.0244 / 4 = -0.0061 per carbon
From length 6 to 12: 6 × -0.0061 = -0.0366
Prediction: 0.7377 - 0.0366 = 0.7011

### Model 2: Linear fit to last 3 points (lengths 4-6)
Points: (4, 0.7518), (5, 0.7498), (6, 0.7377)
Average slope = [(0.7498-0.7518) + (0.7377-0.7498)] / 2 = [-0.0020 + -0.0121] / 2 = -0.00705 per carbon
From length 6 to 12: 6 × -0.00705 = -0.0423
Prediction: 0.7377 - 0.0423 = 0.6954

### Model 3: Average decrease rate from lengths 3-6
Total decrease 3→6: 0.7919 - 0.7377 = 0.0542 over 3 carbons = -0.01807 per carbon
From length 6 to 12: 6 × 0.01807 = 0.1084 decrease
Prediction: 0.7377 - 0.1084 = 0.6293

### Model 4: Conservative estimate assuming stabilization
If we assume the rate of decrease slows further, QED might stabilize around 0.72-0.73 for longer chains.

Given the data shows decreasing trend, prediction should be below 0.7377. Reasonable range: 0.68-0.72.

## Planning Measurements

I have 12 measurement budget total for both jobs. Need to:
1. Measure QED for m05's series members (m15, m24, m28, m17, m22) - DONE (5 measurements used)
2. Extrapolate to predict m05's QED at chain length 12
3. Find lead compound meeting all constraints:
   - psa ≥ 40 (check pool.tsv first)
   - Not in m05's series (check SMILES first) 
   - Measured QED ≥ 0.6

Remaining budget: 7 measurements.

## Measurements Taken:

### m05's series (5 measurements):
- m15: QED = 0.7621
- m24: QED = 0.7919  
- m28: QED = 0.7518
- m17: QED = 0.7498
- m22: QED = 0.7377

### Lead candidate screening (7 measurements):
1. m14 (diol): QED = 0.3237 ✗
2. m27 (diol): QED = 0.333 ✗
3. m02 (nitrophenol): QED = 0.6046 ✓ MEETS ALL CONSTRAINTS
4. m09 (nitrophenol): QED = 0.5826 ✗
5. m12 (nitrophenol): QED = 0.4709 ✗
6. m07 (diol): QED = 0.3233 ✗
7. m10 (nitrophenol): QED = 0.3724 ✗

## Final Decisions:

### m05 QED Prediction:
Based on trend analysis showing decreasing QED with increasing chain length after length 3, and extrapolating to chain length 12, predicted QED = 0.69 (mid-range of 0.68-0.70 estimate).

### Lead Compound Selection:
m02 meets all constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in m05's series (nitrophenol scaffold vs methoxybenzene scaffold) ✓

### Congener IDs for m05's series:
m15, m24, m28, m17, m22 (all share `COc1ccc(cc1)C(=O)N` scaffold)