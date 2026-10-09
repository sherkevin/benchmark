# Notes

## Task 1: Predicting QED for m05

1. **Identify the scaffold and congeneric series for m05**:
   - Use `group_by_scaffold()` to find molecules sharing the same scaffold as `m05`.
   - Measure QED for these molecules to establish a trend.
   - Use `fit_trend()` to predict QED for `m05` based on the measured QED values of its congeneric series.

## Task 2: Selecting a Lead Molecule

1. **List the pool**:
   - Use `list_pool(page)` to explore the compound collection, prioritizing molecules with `psa` ≥ 40.
   - Measure QED for promising candidates to ensure QED ≥ 0.6.
   - Verify that the selected molecule is not in the same congeneric series as `m05`.

2. **Budget Management**:
   - The total budget is 17 units. Each page of `list_pool` and each `measure_qed` call consumes 1 unit.
   - Prioritize measuring QED for molecules likely to meet all constraints.

## Next Steps

- Execute the above steps systematically, recording measurements and predictions in `notes.md`.
- Update `answer.json` with the final results.