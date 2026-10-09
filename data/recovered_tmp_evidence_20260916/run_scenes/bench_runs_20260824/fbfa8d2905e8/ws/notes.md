# QED Prediction for m05

## Target Molecule Analysis
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Structure: Methoxybenzene ring -> amide -> C11 alkyl chain
- Key parts:
  - Scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzene-amide)
  - Variable alkyl chain: `CCCCCCCCCCC` (11 carbons)
- The target has never been synthesized, so we need to predict its QED by studying congeners (same scaffold, different alkyl chain lengths)

## Search Results
Searching for scaffold `COc1ccc(cc1)C(=O)N` returned 6 molecules:
1. m05: Target molecule - C11 chain (cannot be measured)
2. m15: `COc1ccc(cc1)C(=O)NCC` - C2 chain (ethyl)
3. m17: `COc1ccc(cc1)C(=O)NCCCCC` - C5 chain (pentyl)
4. m22: `COc1ccc(cc1)C(=O)NCCCCCC` - C6 chain (hexyl)
5. m24: `COc1ccc(cc1)C(=O)NCCC` - C3 chain (propyl)
6. m28: `COc1ccc(cc1)C(=O)NCCCC` - C4 chain (butyl)

All share the same scaffold `COc1ccc(cc1)C(=O)N` with different alkyl chain lengths attached to the nitrogen.

## QED Measurements
Measured QED values for congeners:
1. m15 (C2): QED = 0.7621
2. m17 (C5): QED = 0.7498  
3. m22 (C6): QED = 0.7377
4. m24 (C3): QED = 0.7919
5. m28 (C4): QED = 0.7518

Data points (chain length, QED):
- C2: 0.7621
- C3: 0.7919 (maximum)
- C4: 0.7518
- C5: 0.7498
- C6: 0.7377

## Trend Analysis
The data shows:
1. QED peaks at C3 chain (0.7919)
2. QED decreases with chain length beyond C3
3. The decrease from C3 to C6 is: 0.7919 → 0.7377 (Δ = -0.0542 over 3 carbons)
4. Individual decreases: C3→C4: -0.0401, C4→C5: -0.0020, C5→C6: -0.0121
5. The rate of decrease appears to be slowing after initial drop from C3 to C4

Descriptor trends (perfectly linear with chain length):
- Each additional carbon adds: ~14.05 MW, ~0.39 ALogP, +1 rotatable bond
- PSA (38.33) and aromatic rings (1) are constant
- For m05 (C11): MW=305.46, ALogP=4.956, rotb=12 (matches linear extrapolation from C6)

## Extrapolation Approaches
For extrapolation from C6 (measured) to C11 (target):

1. **Linear extrapolation from C3-C6**: Slope = -0.01807 per carbon
   Prediction: 0.7377 - (5 × 0.01807) = 0.64735

2. **Using recent trend (C5-C6)**: Drop of 0.0121 per carbon
   Prediction: 0.7377 - (5 × 0.0121) = 0.6772

3. **Using average C4-C6 trend**: Average drop = (0.7518 - 0.7377)/2 = 0.00705 per carbon
   Prediction: 0.7377 - (5 × 0.00705) = 0.70245

4. **Considering slowing rate**: The drop from C4→C5 was only 0.0020
   If this minimal rate continued: 0.7377 - (5 × 0.0020) = 0.7277

## Prediction Reasoning
Given that:
- QED decreases with increasing chain length beyond C3
- The rate of decrease appears to slow (largest drop from C3→C4, smaller drops thereafter)
- C11 is significantly longer than any measured congener (5 carbons beyond C6)
- QED is bounded between 0 and 1, and very long chains likely continue to decrease QED

A reasonable prediction should be:
- Lower than any measured value (< 0.7377)
- But not as low as linear extrapolation from C3-C6 suggests (0.64735), since rate appears to be slowing
- Probably in the range 0.67-0.70

**Selected prediction: 0.685** (midpoint between conservative 0.702 and more aggressive 0.677 estimates)

## Search Strategy
1. Search for molecules with scaffold substring `COc1ccc(cc1)C(=O)N` to find congeners ✓ (found 6 total)
2. Analyze returned molecules to identify alkyl chain lengths ✓ (C2, C3, C4, C5, C6, C11)
3. Measure QED for found congeners within budget ✓ (measured 5 congeners, used 5 measurement units + 2 search units = 7/12 budget)
4. Fit trend of QED vs. chain length ✓ (identified peak at C3, decreasing trend beyond)
5. Extrapolate to C11 chain (target m05) ✓ (predicted QED = 0.685)

## Final Answer Summary
- **Predicted QED for m05 (C11 chain): 0.685**
- **Congeners in same series:** m15 (C2), m24 (C3), m28 (C4), m17 (C5), m22 (C6)
- All QED values obtained from actual `measure_qed` calls
- Budget usage: 7/12 units (2 search pages + 5 measurements)