# Notes for Task

## Task 1: Predict QED for `m05`

1. **Objective**: Predict the QED of `m05` (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`), which has never been synthesized.
2. **Approach**:
   - Used `group_by_scaffold()` to identify molecules sharing the same scaffold as `m05`.
   - Measured the QED of scaffold-mates (`m15`, `m24`, `m28`, `m17`, `m22`) to establish a trend.
   - Used `fit_trend()` to predict the QED of `m05` as **0.5416**.

## Task 2: Select a Lead Molecule

1. **Objective**: Find a molecule with:
   - QED ≥ 0.6
   - `psa` ≥ 40
   - Not in the same congeneric series as `m05`.
2. **Approach**:
   - Explored the pool using `list_pool()` (pages 1-7).
   - Identified `m01` as a candidate with `psa` = 110.02 (≥ 40) and not in the same series as `m05`.
   - Budget was exhausted before measuring QED for `m01`, so it could not be validated.

## Budget Management

- Total budget: 12 units (shared between `list_pool` and `measure_qed`).
- Used 5 units to measure QED for scaffold-mates of `m05`.
- Used 7 units to explore the pool, leaving no budget to measure QED for `m01`.