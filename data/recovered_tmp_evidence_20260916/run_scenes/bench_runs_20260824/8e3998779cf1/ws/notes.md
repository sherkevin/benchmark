# Notes for QED Prediction and Lead Selection

## Initial Analysis

Target molecule m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

Looking at the SMILES patterns, I can identify different scaffolds:

1. **OCC(O)C(O)C(O)C(=O)NCC...** - Polyol scaffold (m01, m03, m07, m14, m18, m19, m21, m25, m27)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)NCC...** - Nitrobenzamide scaffold (m02, m06, m09, m10, m12, m29, m31, m32, m33)
3. **c1ccc(cc1)-c1ccc(cc1)C(=O)NCC...** - Biphenyl scaffold (m04, m08, m11, m13, m16, m17, m20, m23, m26, m30)
4. **COc1ccc(cc1)C(=O)NCC...** - Methoxybenzamide scaffold (m05, m15, m17, m22, m24, m28)

Target m05 has scaffold: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
This matches the methoxybenzamide scaffold: `COc1ccc(cc1)C(=O)NCC...`

Looking at other methoxybenzamide molecules:
- m15: `COc1ccc(cc1)C(=O)NCC` (chain length: NCC - 2 carbons)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain length: NCCCCC - 5 carbons) 
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain length: NCCCCCC - 6 carbons)
- m24: `COc1ccc(cc1)C(=O)NCCC` (chain length: NCCC - 3 carbons)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (chain length: NCCCC - 4 carbons)

Target m05 has chain: `NCCCCCCCCCCC` (12 carbons)

So the congeneric series for m05 includes molecules with the same scaffold (`COc1ccc(cc1)C(=O)NCC...`) but different alkyl chain lengths.

## Congeneric Series Identification

**Target m05 series (methoxybenzamide scaffold):**
Website
- m15: `COc1ccc(cc1)C(=O)NCC` (2 carbons) - QED: 0.7621
- m24: `COc1ccc(cc1)C(=O)NCCC` (3 carbons) - QED: 0.7919
- m28: `COc1ccc(cc1)C(=O)NCCCC` (4 carbons) - QED: 0.7518
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (5 carbons) - QED: 0.7498
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6 carbons) - QED: 0.7377
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (12 carbons) - target to predict

**Chain length to QED data:**
- 2 carbons: 0.7621
- 3 carbons: 0.7919
- 4 carbons: 0.7518
- 5 carbons: 0.7498
- 6 carbons: 0.7377

## Other Scaffolds for Lead Selection:

1. **Polyol scaffold**: `OCC(O)C(O)C(O)C(=O)NCC...`
   - m01, m03, m07, m14, m18, m19, m21, m25, m27
   - PSA values: All have PSA = 110.02 (meets PSA ≥ 40 constraint)

2. **Nitrobenzamide scaffold**: `O=[N+]([O-])c1ccc(cc1)C(=O)NCC...`
   - m02, m06, m09, m10, m12, m29, m31, m32, m33
   - PSA values: All have PSA = 72.24 (meets PSA ≥ 40 constraint)

3. **Biphenyl scaffold**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCC...`
   - m04, m08, m11, m13, m16, m20, m23, m26, m30
   - PSA values: All have PSA = 29.1 (does NOT meet PSA ≥ 40 constraint)

## Measurements Summary

**Used measurements: 8/12**
1. m15 (methoxybenzamide): 0.7621
2. m24 (methoxybenzamide): 0.7919
3. m28 (methoxybenzamide): 0.7518
4. m17 (methoxybenzamide): 0.7498
5. m22 (methoxybenzamide): 0.7377
6. m01 (polyol): 0.3187
7. m03 (polyol): 0.3282
8. m02 (nitrobenzamide): 0.6046

**Lead Candidate Found: m02**
- QED: 0.6046 ≥ 0.6 ✓
- PSA: 72.24 ≥ 40 ✓  
- Scaffold: Nitrobenzamide (not methoxybenzamide) ✓

## Trend Analysis for m05 Prediction

Chain length vs QED data for methoxybenzamide series:
- 2C: 0.7621
- 3C: 0.7919 (peak)
- 4C: 0.7518
- 5C: 0.7498
- 6C: 0.7377

Pattern: QED peaks at 3 carbons, then decreases with longer chains.

ALOGP values show linear increase with chain length:
- 2C: 1.445
- 3C: 1.835
- 4C: 2.225
- 5C: 2.615
- 6C: 3.005
- 12C (m05): 4.956

ROTB (rotatable bonds) also increases linearly:
- 2C: 3
- 3C: 4
- 4C: 5
- 5C: 6
- 6C: 7
- 12C: 12

**Trend Analysis:**
1. QED shows a clear decreasing trend after the peak at 3 carbons
2. Both ALOGP (lipophilicity) and ROTB (flexibility) increase with chain length
3. These factors contribute to decreasing QED
4. The rate of QED decrease appears to be slowing but still significant

**Extrapolation to m05 (12 carbons):**
- From 3C to 6C: QED decreased by 0.0542 over 3 carbons
- Average decrease: ~0.018 per carbon
- From 6C to 12C: 6 more carbons
- Linear extrapolation: 0.7377 - (6 × 0.018) = 0.6297

However, considering:
1. The decrease rate slows after the initial drop
2. ALOGP-based prediction: ~0.647
3. Pattern suggests QED continues decreasing but may be flattening

**Final Prediction: QED = 0.645 for m05**

This considers:
- Clear downward trend from measured data
- Significant increase in ALOGP for m05 (4.956 vs max 3.005 in measured)
- Increase in rotatable bonds (12 vs max 7 in measured)
- Conservative estimate given slowing rate of decrease