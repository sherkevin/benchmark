# Notes for Task Completion

## Task 1: Predict QED for `m05`
1. Grouped molecules by scaffold and identified `m05`'s congeneric series:
   - Scaffold: `COc1ccc(cc1)C(=O)N`
   - Members: `m15`, `m24`, `m28`, `m17`, `m22`, `m05`.
2. Measured QED for `m15`, `m24`, `m28`, and `m17` (4 measurements used).
3. Fitted a quadratic trend to the measured QED values vs. chain length.
4. Predicted `m05`'s QED as **0.2689**.

## Task 2: Find a Lead Molecule
1. Identified scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` (not shared with `m05`).
2. Measured QED for `m02` and `m09` (2 measurements used).
3. Selected `m02` as it satisfies all constraints:
   - QED = 0.6046 ≥ 0.6.
   - `psa` = 72.24 ≥ 40.
   - Not in the same series as `m05`.

## Summary
- Total measurements used: 6 out of 12.
- Results saved in `answer.json`.