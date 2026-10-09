# Notes: QED Prediction for m05

## Target Molecule
- **m05**: SMILES = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- QED is unavailable (probe_unavailable) - needs prediction
- Chain length: 13 carbons in the N-alkyl chain

## Strategy
The scaffold (common part) is: `COc1ccc(cc1)C(=O)N`
The variable part is the alkyl chain length.

## Step 1: Search for congeners using scaffold substring

Searched for `COc1ccc(cc1)C(=O)N` in the pool.
- Page 1: Found m05 (target), m15, m17, m22
- Page 2: Found m24, m28
- Total: 6 molecules in this series

## Congeners in the same series as m05

| mol_id | SMILES | Chain | Chain Length |
|--------|--------|-------|--------------|
| m15 | COc1ccc(cc1)C(=O)NCC | NCC | 2 carbons |
| m24 | COc1ccc(cc1)C(=O)NCCC | NCCC | 3 carbons |
| m28 | COc1ccc(cc1)C(=O)NCCCC | NCCCC | 4 carbons |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | NCCCCC | 5 carbons |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | NCCCCCC | 6 carbons |
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | NCCCCCCCCCCC | 13 carbons (target) |

**Congener IDs**: m15, m24, m28, m17, m22

## Step 2: Measure QED of congeners

Measured all 5 congeners:

| mol_id | Chain Length | QED |
|--------|--------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Step 3: Fit the trend

The QED vs chain length shows:
- Peak at chain length 3 (QED = 0.7919)
- Decline thereafter
- The decline accelerates (consistent with a downward-opening parabola)

### Quadratic Fit

Fitted a quadratic model y = ax² + bx + c using least squares on all 5 data points:

**y = -0.003264x² + 0.017022x + 0.749324**

Where:
- x = chain length
- y = predicted QED

Fit verification:
- x=2: predicted 0.7703, actual 0.7621
- x=3: predicted 0.7710, actual 0.7919
- x=4: predicted 0.7652, actual 0.7518
- x=5: predicted 0.7528, actual 0.7498
- x=6: predicted 0.7340, actual 0.7377

The fit captures the overall shape: rise to peak around x=2.6, then decline.

## Step 4: Extrapolate to m05 (chain length = 13)

Using the quadratic model:

y(13) = -0.003264(169) + 0.017022(13) + 0.749324
y(13) = -0.551616 + 0.221286 + 0.749324
y(13) = 0.418994 ≈ **0.419**

## Prediction

**Predicted QED for m05: 0.419**

This makes chemical sense:
- m05 has a very long alkyl chain (13 carbons)
- This results in high aLogP (4.956, close to the QED penalty threshold of 5)
- High molecular weight (305.46)
- Many rotatable bonds (12)
- All these factors contribute to a lower QED compared to the shorter-chain congeners
