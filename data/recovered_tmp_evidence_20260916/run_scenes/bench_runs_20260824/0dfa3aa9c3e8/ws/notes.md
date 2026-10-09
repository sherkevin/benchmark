# Notes for Task Completion

## Task 1: Predict QED for `m05`

- `m05` has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`.
- It belongs to a congeneric series with varying N-alkyl chain lengths.
- Measured QED for congeners:
  - `m15`: 0.7621
  - `m24`: 0.7919
  - `m17`: 0.7498
  - `m28`: 0.7518
  - `m22`: 0.7377
- **Predicted QED for `m05`: 0.75** (based on trend).

## Task 2: Select a Lead Molecule

- Constraints:
  1. QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05`.
- Measured QED for `psa` ≥ 40 candidates:
  - `m01`: 0.3187
  - `m03`: 0.3282
  - `m07`: 0.3233
  - `m14`: 0.3237
  - `m18`: 0.3282
  - `m19`: 0.3244
  - `m21`: 0.3297
- **No molecule met all constraints.**