# Notes for QED Prediction Task

## Target Molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Has a methoxy-phenyl-amide scaffold with a 13-carbon alkyl chain
- QED cannot be measured directly (probe_unavailable)

## Strategy
1. Search for molecules with the scaffold substring to find congeners
2. The scaffold is `COc1ccc(cc1)C(=O)N` - the part before the alkyl chain
3. Measure QED of congeners with different chain lengths
4. Fit a trend and extrapolate to chain length 13

## Search Results
Searched with query `COc1ccc(cc1)C(=O)N` - found 6 total matches (2 pages).

### Congeners in the same series as m05:
| mol_id | SMILES | Chain | Chain Length | MW | alogP | rotb |
|--------|--------|-------|--------------|----|-------|------|
| m15 | COc1ccc(cc1)C(=O)NCC | CC | 2 | 179.22 | 1.445 | 3 |
| m24 | COc1ccc(cc1)C(=O)NCCC | CCC | 3 | 193.25 | 1.835 | 4 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | CCCC | 4 | 207.27 | 2.225 | 5 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | CCCCC | 5 | 221.30 | 2.615 | 6 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | CCCCCC | 6 | 235.33 | 3.005 | 7 |
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | CCCCCCCCCCCCC | 13 | 305.46 | 4.956 | 12 |

## QED Measurements
| mol_id | Chain Length | QED |
|--------|--------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Trend Analysis
The QED values show:
- Peak at chain length 3 (QED = 0.7919)
- Decreasing trend from n=3 to n=6
- Drop from n=3→4: -0.0401
- Drop from n=4→5: -0.0020
- Drop from n=5→6: -0.0121

The relationship is NOT linear - it peaks around n=3 then decreases with a non-uniform rate.

### Fitting an Exponential Decay Model
Using exponential fit QED = A * exp(-B*n) on data from n=3 to n=6:
- A ≈ 0.8345, B ≈ 0.0215
- At n=13: QED = 0.8345 * exp(-0.0215*13) = 0.8345 * 0.7562 ≈ 0.63

### Asymptotic Model
Using QED(n) = QED_min + (QED_peak - QED_min) * exp(-k*(n-3)):
- With QED_min ≈ 0.50-0.55, prediction ≈ 0.64-0.65
- With QED_min ≈ 0.45, prediction ≈ 0.64

## Extrapolation to m05 (chain length 13)
Given:
- m05 has alogP = 4.956 (high, penalizes QED)
- m05 has rotb = 12 (high, penalizes QED)
- The measured trend shows decreasing QED with increasing chain length
- Extrapolating ~7 carbons beyond the longest measured congener

**Predicted QED for m05: 0.62**

This accounts for:
1. The observed downward trend from the peak at n=3
2. The non-linear (asymptotic) nature of the decay
3. The high lipophilicity and rotatable bond count of m05 which would further reduce drug-likeness

## Congener IDs (same series as m05)
m15, m24, m28, m17, m22
