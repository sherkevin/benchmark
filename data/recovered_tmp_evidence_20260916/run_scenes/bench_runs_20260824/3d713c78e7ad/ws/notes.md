# Notes on Task Completion

## Task 1: Predicting QED for `m05`
- Identified the congeneric series for `m05` using `group_by_scaffold`:
  - Scaffold: `COc1ccc(cc1)C(=O)N`
  - Members: `m15` (chain_len: 2), `m24` (chain_len: 3), `m28` (chain_len: 4), `m17` (chain_len: 5), `m22` (chain_len: 6), `m05` (chain_len: 11)
- Measured QED for available molecules in the series:
  - `m15`: 0.7621
  - `m24`: 0.7919
  - `m28`: 0.7518
- Used `fit_trend` to predict QED for `m05` (chain_len: 11) based on the measured data:
  - Predicted QED: -0.0229 (This value is unrealistic; likely due to insufficient data points or high variability in QED for longer chains.)

## Task 2: Selecting a Lead Molecule
- **Constraints**:
  1. QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05`
- Identified potential candidates:
  - `m02`: `psa` = 72.24, not in `m05`'s series
  - `m06`: `psa` = 72.24, not in `m05`'s series
  - `m09`: `psa` = 72.24, not in `m05`'s series
- Measured QED for `m02` (budget exhausted): Could not measure due to budget constraints.
- Selected `m02` as a promising candidate based on `psa` and scaffold difference, but unable to confirm QED due to budget.

## Budget Usage
- Total budget: 12 units
- Used:
  - `list_pool`: 9 units (pages 1-9)
  - `measure_qed`: 3 units (`m15`, `m24`, `m28`)
- Budget exhausted before completing all measurements.

## Final Answer
Given the budget constraints, the predicted QED for `m05` is unrealistic (-0.0229), and we could not confirm QED for potential lead candidates. Adjusting the approach may be necessary for future tasks.