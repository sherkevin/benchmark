# Notes for QED Prediction Task

## Target Molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Has never been synthesized, so QED must be predicted
- Chain length: 13 carbons after the amide nitrogen

## Search Strategy
Searched for the scaffold `COc1ccc(cc1)C(=O)N` to find all congeners in the same series.
- Page 1: Found m05 (target), m15, m17, m22
- Page 2: Found m24, m28
- Total: 6 matches (5 measurable congeners + 1 target)

## Congeners Found (same series as m05)
| mol_id | SMILES | Chain Length | QED (measured) |
|--------|--------|-------------|----------------|
| m15 | COc1ccc(cc1)C(=O)NCC | 2 | 0.7621 |
| m24 | COc1ccc(cc1)C(=O)NCCC | 3 | 0.7919 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | 4 | 0.7518 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | 5 | 0.7498 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | 6 | 0.7377 |

## Observed Trend
The QED values show a non-linear pattern:
- Peaks at chain length 3 (QED = 0.7919)
- Then declines as chain length increases
- Chain 2→3: increase (+0.0298)
- Chain 3→4: decrease (-0.0401)
- Chain 4→5: slight decrease (-0.0020)
- Chain 5→6: decrease (-0.0121)

## Curve Fitting
Fit a quadratic model to the declining portion (chains 4, 5, 6) since m05 (chain 13) is well beyond the peak:

Model: QED = -0.00505 × (chain_length)² + 0.04345 × (chain_length) + 0.6588

This model exactly fits the three data points at chains 4, 5, 6.

## Extrapolation to Chain Length 13
QED(13) = -0.00505 × 169 + 0.04345 × 13 + 0.6588
        = -0.85345 + 0.56485 + 0.6588
        = 0.3702

## Prediction
**Predicted QED for m05: 0.37** (rounded to 2 decimal places)

Rationale: The long C13 alkyl chain significantly increases lipophilicity and molecular weight, which are penalized in the QED calculation. The extrapolated value of ~0.37 reflects this substantial decrease from the shorter-chain congeners.

## Budget Usage
- search_pool page 1: 1 unit
- search_pool page 2: 1 unit
- measure_qed m15: 1 unit
- measure_qed m24: 1 unit
- measure_qed m28: 1 unit
- measure_qed m17: 1 unit
- measure_qed m22: 1 unit
- **Total: 7 units used out of 12**
