# Task Notes

## Task 1: Predict QED for `m05`
1. Identified the congeneric series for `m05` using `group_by_scaffold`.
2. Measured QED for molecules in the same scaffold (`m15`, `m24`, `m28`, `m17`, `m22`).
3. Used `fit_trend` to predict QED for `m05` (chain length: 11) based on the measured QED values.
   - Predicted QED: **0.5416**.

## Task 2: Find a molecule satisfying all constraints
1. Filtered molecules with `psa` ≥ 40.
2. Excluded molecules in the same congeneric series as `m05`.
3. Measured QED for candidates (`m01`, `m02`, `m03`, `m06`).
4. Selected `m02` as it satisfies all constraints:
   - Measured QED: **0.6046** (≥ 0.6).
   - `psa`: 72.24 (≥ 40).
   - Not in the same series as `m05`.

## Budget Usage
- Total measurements used: **7** out of 12.