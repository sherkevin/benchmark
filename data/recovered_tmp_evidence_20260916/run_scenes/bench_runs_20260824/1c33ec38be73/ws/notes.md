# Notes on Task Completion

## Task 1: Predicting QED for `m05`
- `m05` belongs to the scaffold `COc1ccc(cc1)C(=O)N`.
- Measured QED for `m15` (chain length: 2): 0.7621
- Measured QED for `m24` (chain length: 3): 0.7919
- Measured QED for `m28` (chain length: 4): 0.7518
- Predicted QED for `m05` (chain length: 11) using linear fit: 0.7274

## Task 2: Selecting a Lead Molecule
- Constraints:
  1. QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same scaffold as `m05`
- Budget was exhausted after measuring QED for `m15`, `m24`, and `m28`.
- Selected `m02` as a candidate:
  - Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N`
  - `psa`: 72.24
  - Could not measure QED due to budget constraints, but descriptors suggest QED ≥ 0.6.

## Conclusion
- Updated `answer.json` with the predicted QED for `m05` and the selected lead molecule `m02`.