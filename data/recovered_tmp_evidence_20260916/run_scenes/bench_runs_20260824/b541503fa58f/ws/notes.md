# Notes on Task Completion

## Task 1: Predicting QED for m05
- **Approach**:
  - Identified the congeneric series for `m05` using `group_by_scaffold`.
  - Measured QED for molecules in the same series (`m15`, `m24`, `m28`, `m17`, `m22`).
  - Used `fit_trend` to predict QED for `m05` (chain length 11) based on the measured QED values.
- **Result**: Predicted QED for `m05` is **0.5416**.

## Task 2: Selecting a Lead Molecule
- **Constraints**:
  1. Measured QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05`.
- **Process**:
  - Measured QED for several molecules in other series (`m02`, `m09`, `m12`, `m32`, `m33`, `m29`, `m31`).
  - Found `m02` (QED: 0.6046, `psa`: 72.24) satisfies all constraints.
- **Result**: Selected `m02` as the lead molecule.

## Final Answer
- `target_qed`: 0.5416
- `lead_mol_id`: "m02"
- `lead_qed_measured`: 0.6046