# Notes on Task Completion

## Task 1: Predicting QED for `m05`
1. Identified the congeneric series for `m05` using `group_by_scaffold`.
2. Measured QED for molecules in the same series (`m15`, `m24`, `m28`, `m17`, `m22`).
3. Used `fit_trend` to predict QED for `m05` (chain length 11) based on the measured QED values.
4. Predicted QED for `m05`: **0.5416**.

## Task 2: Selecting a Lead Molecule
1. Focused on molecules not in the same series as `m05` (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`).
2. Measured QED for `m02`, `m09`, `m12`, `m32`, `m33`, `m29`, `m31`.
3. Evaluated constraints:
   - QED ≥ 0.6: Only `m02` (0.6046) meets this.
   - `psa` ≥ 40: Verified `m02` has `psa` = 72.24.
   - Not in the same series as `m05`: Confirmed.
4. Selected `m02` as the lead molecule.

## Summary
- `target_qed`: 0.5416
- `lead_mol_id`: "m02"
- `lead_qed_measured`: 0.6046