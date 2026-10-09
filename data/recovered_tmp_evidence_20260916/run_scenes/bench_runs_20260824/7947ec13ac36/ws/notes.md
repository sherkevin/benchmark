# Notes on Task Completion

## Task 1: Predict QED for `m05`
1. Identified the congeneric series for `m05` using `group_by_scaffold`.
2. Measured QED for molecules in the same series (`m15`, `m24`, `m28`, `m17`, `m22`).
3. Used `fit_trend` to predict QED for `m05` (chain length 11) based on measured QED values.
4. Predicted QED for `m05`: **0.5416**.

## Task 2: Find Lead Molecule
1. Searched for molecules with `psa` ≥ 40 and not in the same series as `m05`.
2. Measured QED for promising candidates (`m01`, `m03`, `m06`, `m07`, `m09`, `m10`, `m12`, `m14`).
3. The closest candidate is `m09` (QED: 0.5826, `psa`: 72.24), but it does not fully meet the QED ≥ 0.6 requirement.
4. Conclusion: No molecule in the pool meets all three criteria within the given budget.

## Budget Usage
- Total budget: 17 units.
- Used: 17 units (5 for Task 1, 12 for Task 2).