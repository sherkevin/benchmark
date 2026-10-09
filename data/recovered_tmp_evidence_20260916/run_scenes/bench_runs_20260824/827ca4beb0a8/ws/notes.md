# Notes for Task Completion

## Task 1: Predict QED for `m05`
- Identified the congeneric series for `m05` using `group_by_scaffold`.
- Measured QED for molecules in the series (`m15`, `m24`, `m28`, `m17`, `m22`).
- Used `fit_trend` to predict `m05`'s QED based on chain length (11): **0.5416**.

## Task 2: Find a Lead Molecule
- Listed the pool and filtered molecules with `psa` ≥ 40 and not in `m05`'s series.
- Measured QED for candidates (`m01`, `m02`, `m03`).
- Selected `m02` as it met all constraints:
  - QED: 0.6046 (≥ 0.6)
  - `psa`: 72.24 (≥ 40)
  - Not in `m05`'s series.

## Budget Used
- Total budget: 12 units
- Used: 9 units (5 for Task 1, 4 for Task 2).