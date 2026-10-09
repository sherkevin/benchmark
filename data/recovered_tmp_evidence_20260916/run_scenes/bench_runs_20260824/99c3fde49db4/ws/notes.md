# Notes

## Task 1: Predicting QED for `m05`
1. Identified the congeneric series for `m05` using `group_by_scaffold`.
2. Measured QED for `m15`, `m24`, `m28`, `m17`, and `m22` (chain lengths 2 to 6).
3. Fitted a polynomial trend to predict QED for `m05` (chain length 11) using `fit_trend`.
4. Predicted QED for `m05`: **0.5416**.

## Task 2: Finding a Lead Molecule
1. Filtered molecules with `psa` ≥ 40 from `data/pool.tsv`.
2. Excluded molecules in the same congeneric series as `m05`.
3. Measured QED for `m01` (0.3187) and `m02` (0.6046).
4. Selected `m02` as the lead molecule:
   - `mol_id`: **m02**
   - `psa`: 72.24
   - Measured QED: **0.6046**

## Budget Usage
Total measurements used: **7** out of 12.