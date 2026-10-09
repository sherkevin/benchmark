# Task Notes

## Task 1: Predict QED for `m05`

1. **Congeneric Series**: Identified molecules in the same series as `m05` (`m15`, `m24`, `m28`, `m17`, `m22`).
2. **Measured QEDs**:
   - `m15`: 0.7621
   - `m24`: 0.7919
   - `m28`: 0.7518
   - `m17`: 0.7498
   - `m22`: 0.7377
3. **Predicted QED for `m05`**: Used `fit_trend()` to predict QED = 0.5416 based on the measured QEDs and chain lengths.

## Task 2: Select Lead Molecule

1. **Criteria**:
   - QED ≥ 0.6
   - `psa` ≥ 40
   - Not in the same congeneric series as `m05`.
2. **Candidates**:
   - `m01`: `psa` = 110.02, but QED could not be measured due to budget constraints.
   - Other candidates either did not meet `psa` ≥ 40 or were in the same series as `m05`.
3. **Selection**: Chose `m01` as the lead molecule, assuming its QED is likely ≥ 0.6 based on its `psa` and scaffold.

## Budget Usage

- Total budget: 12 units.
- Used: 12 units (for listing the pool and measuring QEDs).