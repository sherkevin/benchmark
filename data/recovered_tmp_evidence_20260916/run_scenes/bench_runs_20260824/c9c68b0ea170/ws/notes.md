# Notes on QED Prediction Task

## Target Molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- This is a methoxy-benzamide with a long N-alkyl chain (C13 = 13 carbons in the chain)
- QED is unavailable (probe_unavailable) - must predict

## Strategy
1. Find the scaffold substring to search for congeners
2. The scaffold is: `COc1ccc(cc1)C(=O)N` (methoxy-benzamide core)
3. Search for molecules with this scaffold but different chain lengths

## Step 1: Search for congeners using scaffold

Searched with query `COc1ccc(cc1)C(=O)N` - found 6 total matches across 2 pages.

### Molecules Found (same series):

| mol_id | SMILES | Chain Length | Descriptors |
|--------|--------|--------------|-------------|
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | C13 | mw=305.46, alogp=4.956, psa=38.33, rotb=12, arom=1 |
| m15 | COc1ccc(cc1)C(=O)NCC | C2 | mw=179.22, alogp=1.445, psa=38.33, rotb=3, arom=1 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | C5 | mw=221.3, alogp=2.615, psa=38.33, rotb=6, arom=1 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | C6 | mw=235.33, alogp=3.005, psa=38.33, rotb=7, arom=1 |
| m24 | COc1ccc(cc1)C(=O)NCCC | C3 | mw=193.25, alogp=1.835, psa=38.33, rotb=4, arom=1 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | C4 | mw=207.27, alogp=2.225, psa=38.33, rotb=5, arom=1 |

**Congeners in pool (excluding target m05)**: m15, m17, m22, m24, m28

## Step 2: Measure QED of congeners

Measured all 5 congeners:

| mol_id | Chain Length | QED |
|--------|--------------|-----|
| m15 | C2 | 0.7621 |
| m24 | C3 | 0.7919 |
| m28 | C4 | 0.7518 |
| m17 | C5 | 0.7498 |
| m22 | C6 | 0.7377 |

## Step 3: Analyze the trend

The QED vs chain length relationship shows:
- **C2 → C3**: QED increases (0.7621 → 0.7919), peaking at C3
- **C3 → C6**: QED decreases steadily (0.7919 → 0.7377)

This is a non-linear relationship with a maximum around C3, then declining.

### Trend Analysis:
- Peak QED at C3 (m24): 0.7919
- Decline from C3 to C4: -0.0401
- Decline from C4 to C5: -0.0020
- Decline from C5 to C6: -0.0121

The decline from C3 onwards is not perfectly linear but shows a general downward trend.

### Extrapolation to C13 (m05):

Since we're extrapolating from C6 to C13 (7 carbons beyond our data), and the task warns against assuming linearity:

1. **Linear extrapolation from C4-C6 tail**: 
   - Average decline: ~0.007 per carbon
   - Prediction: 0.7377 - 7×0.007 ≈ 0.69

2. **Considering non-linear decay**: 
   - The QED likely approaches an asymptote as chain length increases
   - Long alkyl chains increase lipophilicity (alogp), which reduces QED
   - For C13 (very long chain), QED would be significantly lower than C6

3. **Fitted trend**: The data from C3-C6 suggests a decaying pattern. Given:
   - C3: 0.7919, C4: 0.7518, C5: 0.7498, C6: 0.7377
   - The decline accelerates slightly from C5 to C6
   - For C13, extrapolating with a modest decay rate suggests QED around 0.60-0.65

### Final Prediction:

Based on the observed trend (peak at C3, then declining), and extrapolating to C13:
- The QED will continue to decrease as chain length increases beyond C6
- A reasonable estimate accounting for non-linearity: **QED ≈ 0.62**

This reflects:
- The clear downward trend from C3 to C6
- The expectation that very long chains (C13) significantly reduce drug-likeness
- Not simply quoting the nearest measured value (C6 = 0.7377)
- Not assuming a simple linear relationship

## Budget Used
- Search: 2 units (2 pages)
- Measurements: 5 units (5 congeners)
- Total: 7/12 units

## Congener IDs for answer.json
["m15", "m17", "m22", "m24", "m28"]
