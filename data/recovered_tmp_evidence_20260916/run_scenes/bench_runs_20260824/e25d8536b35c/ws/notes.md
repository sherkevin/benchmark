# Notes on QED Prediction for `m05`

## Congeneric Series Identification
- **Target Molecule**: `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`)
- **Scaffold Query**: `COc1ccc(cc1)C(=O)NC`
- **Congeners Found**:
  - `m15`: `COc1ccc(cc1)C(=O)NCC` (chain length = 2)
  - `m24`: `COc1ccc(cc1)C(=O)NCCC` (chain length = 3)
  - `m28`: `COc1ccc(cc1)C(=O)NCCCC` (chain length = 4)
  - `m17`: `COc1ccc(cc1)C(=O)NCCCCC` (chain length = 5)
  - `m22`: `COc1ccc(cc1)C(=O)NCCCCCC` (chain length = 6)

## Measured QED Values
| Molecule | Chain Length | QED    |
|----------|--------------|--------|
| `m15`    | 2            | 0.7621 |
| `m24`    | 3            | 0.7919 |
| `m28`    | 4            | 0.7518 |
| `m17`    | 5            | 0.7498 |
| `m22`    | 6            | 0.7377 |

## Trend Analysis
- The QED peaks at a chain length of 3 (`m24`) and then decreases as the chain length increases.
- The trend suggests that longer chains (beyond 6) will continue to lower the QED.

## Prediction for `m05`
- **Chain Length**: 12
- **Predicted QED**: ~0.68 (extrapolated from the decreasing trend).