# Notes on Task Completion

## Task 1: Predicting QED for m05
- **m05** (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so its QED cannot be measured directly.
- Identified molecules in the same congeneric series:
  - **m15** (SMILES: `COc1ccc(cc1)C(=O)NCC`, QED: **0.7621**)
  - **m17** (SMILES: `COc1ccc(cc1)C(=O)NCCCCC`, QED: **0.7498**)
- Trend: Longer alkyl chains reduce QED. Predicted QED for **m05**: **0.6**.

## Task 2: Selecting a Lead Molecule
- **m02** (SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`) meets all constraints:
  1. Measured QED: **0.6046** (≥ 0.6)
  2. `psa`: **72.24** (≥ 40)
  3. Not in the same series as `m05`.

## Budget Usage
- Total budget: **12 units**
- Used: **8 units** (4 for listing pool, 4 for measuring QEDs).