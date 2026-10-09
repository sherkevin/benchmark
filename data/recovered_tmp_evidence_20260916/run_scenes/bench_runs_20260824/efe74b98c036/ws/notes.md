# Task Notes

## Task 1: Predict QED for `m05`
- Identified the congeneric series for `m05` using `group_by_scaffold`.
- Measured QED for members `m15`, `m24`, `m28`, and `m17`.
- Predicted QED for `m05` (chain length: 11) using `fit_trend`:
  - **Predicted QED**: 0.2689

## Task 2: Find a Suitable Lead Molecule
- Explored the pool using `list_pool` (pages 1-3).
- Measured QED for candidates with `psa` ≥ 40 (`m01`, `m03`, `m06`, `m07`, `m09`).
- No molecule met all three criteria (QED ≥ 0.6, `psa` ≥ 40, not in the same series as `m05`) within the budget.

## Budget Usage
- Total budget: 12 units
- Used: 12 units (4 for `measure_qed`, 3 for `list_pool`)

## Conclusion
- Updated `answer.json` with `target_qed` for `m05`.
- No suitable lead molecule found for Task 2.