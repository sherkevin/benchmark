# QED Prediction for m05

## Target Molecule
- m05: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Has C11 alkyl chain (11 carbons)
- Not synthesized, cannot be measured directly

## Search Strategy
1. Searched for scaffold substring `COc1ccc(cc1)C(=O)N` (methoxybenzamide core)
2. Found 6 total matches across 2 pages (budget used: 2 units)
3. All molecules share same scaffold with varying N-alkyl chain lengths

## Congeneric Series Members
Found 6 molecules in the series:
1. m15: `COc1ccc(cc1)C(=O)NCC` - C2 chain
2. m24: `COc1ccc(cc1)C(=O)NCCC` - C3 chain  
3. m28: `COc1ccc(cc1)C(=O)NCCCC` - C4 chain
4. m17: `COc1ccc(cc1)C(=O)NCCCCC` - C5 chain
5. m22: `COc1ccc(cc1)C(=O)NCCCCCC` - C6 chain
6. m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - C11 chain (target)

## QED Measurements
Measured QED for all synthesized molecules (budget used: 5 more units, total 7 used):
- m15 (C2): QED = 0.7621
- m24 (C3): QED = 0.7919
- m28 (C4): QED = 0.7518
- m17 (C5): QED = 0.7498
- m22 (C6): QED = 0.7377
- m05 (C11): probe_unavailable (as expected)

## Trend Analysis
Chain length vs QED:
- C2: 0.7621
- C3: 0.7919 (maximum)
- C4: 0.7518
- C5: 0.7498
- C6: 0.7377

Observations:
1. QED increases from C2 to C3 (optimal around C3)
2. QED decreases monotonically from C3 to C6
3. Trend suggests parabolic relationship with maximum around C3

## Extrapolation to C11
The target has C11 chain length, which is 5 carbons longer than the longest measured (C6).
Given the decreasing trend from C3 to C6, we expect QED to continue decreasing for C11.

Possible approaches:
1. Linear extrapolation from C3-C6 trend
2. Quadratic fit to capture curvature
3. Consider molecular properties: longer chains increase lipophilicity (alogp) and rotatable bonds (rotb), which may negatively impact QED

The measured alogp values increase with chain length:
- C2: 1.445
- C3: 1.835
- C4: 2.225
- C5: 2.615
- C6: 3.005
- C11: estimated ~4.956 (from search results)

Rotatable bonds also increase linearly with chain length.

## Prediction Method
Given the clear decreasing trend after C3 and the substantial increase in chain length to C11, I expect QED to continue decreasing. The rate of decrease appears to be slowing slightly (differences: C3→C4: -0.0401, C4→C5: -0.0020, C5→C6: -0.0121).

### Calculations:

1. **Linear regression on C3-C6 data (post-peak):**
   - Points: (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)
   - Regression: y = -0.01646x + 0.83187
   - For x=11: QED = -0.01646*11 + 0.83187 = 0.65081

2. **Average decrease per carbon from C3 to C6:**
   - Total decrease: 0.7919 - 0.7377 = 0.0542
   - Per carbon: 0.0542 / 3 = 0.01807
   - From C6 to C11 (5 carbons): 0.7377 - (0.01807 * 5) = 0.64735

3. **AlogP relationship:**
   - AlogP increases ~0.39 per carbon
   - C6 alogp=3.005, C11 alogp≈4.956 (confirmed)
   - QED vs alogp shows peak at alogp=1.835 (C3), then decreases
   - Higher alogp (4.956) suggests lower QED

4. **Molecular property considerations:**
   - C11 has 11 rotatable bonds (vs 6 for C6)
   - Higher MW (305 vs 235)
   - Higher lipophilicity
   - All factors generally negative for QED beyond optimal ranges

Given these analyses, predicted QED for m05 (C11) is approximately **0.65-0.68**.

## Final Prediction
Considering:
1. Linear regression from C3-C6: 0.6508
2. Average decrease per carbon: 0.6474
3. Continuation of C5→C6 rate: 0.6772
4. Molecular properties: C11 has excessive lipophilicity (alogp~4.96), many rotatable bonds (12), high MW (305) - all beyond optimal drug-like ranges
5. The trend shows QED decreasing with chain length beyond C3

I predict **QED = 0.66** for m05. This value:
- Is significantly lower than C6 value (0.7377)
- Reflects negative impact of very long alkyl chain
- Is consistent with extrapolated trends
- Respects QED bounds (0-1)

## Budget Usage
- Search_pool: 2 pages = 2 units
- Measure_qed: 5 measurements = 5 units  
- Total used: 7 of 12 units
- Remaining: 5 units unused