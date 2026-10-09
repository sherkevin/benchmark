# Notes for Task Completion

## Task 1: Predict QED for `m05`
- **Objective**: Predict the QED of `m05` (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`), which has never been synthesized.
- **Approach**:
  1. Identify the congeneric series of `m05` using `group_by_scaffold`.
  2. Measure QED for molecules in the same series (same scaffold, varying N-alkyl chain length).
  3. Fit a polynomial trend to the measured QED values and predict QED for `m05`.

## Task 2: Identify a Lead Molecule
- **Objective**: Find a molecule with:
  1. Measured QED ≥ 0.6.
  2. `psa` ≥ 40.
  3. Not in the same congeneric series as `m05`.
- **Approach**:
  1. Use `list_pool` to explore the catalogue.
  2. Filter candidates based on `psa` ≥ 40.
  3. Measure QED for promising candidates.
  4. Ensure the selected molecule is not in the same series as `m05`.
