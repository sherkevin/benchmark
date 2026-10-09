# Notes on Predicting QED for `m05`

## Search Strategy
1. Searched for molecules containing the scaffold substring `COc1ccc(cc1)C(=O)N`.
2. Found 6 molecules in the same series as `m05`:
   - `m15`, `m24`, `m28`, `m17`, `m22`, `m05`.

## Measured QED Values
| Molecule | Chain Length | QED    |
|----------|--------------|--------|
| `m15`    | 2            | 0.7621 |
| `m24`    | 3            | 0.7919 |
| `m28`    | 4            | 0.7518 |
| `m17`    | 5            | 0.7498 |
| `m22`    | 6            | 0.7377 |

## Trend Analysis
- QED peaks at chain length 3 (0.7919).
- After chain length 3, QED decreases as the chain length increases.

## Prediction for `m05`
- `m05` has a chain length of 12.
- Given the decreasing trend, the QED is predicted to be **0.65**.

## Budget Usage
- Total budget: 12 units.
- Used: 7 units (2 for search, 5 for measurements).
- Remaining: 5 units.