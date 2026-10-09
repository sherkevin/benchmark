# Notes

## Task 1: Predicting QED for m05

1. **Grouped molecules by scaffold** using `group_by_scaffold()` to identify congeneric series.
2. Found that `m05` belongs to the scaffold `COc1ccc(cc1)C(=O)N` with chain length 11.
3. Measured QED for other molecules in the same series (`m15`, `m24`, `m28`, `m17`, `m22`).
4. Used `fit_trend()` to predict QED for `m05` (chain length 11) based on the measured QED values:
   - `xs`: [2, 3, 4, 5, 6] (chain lengths)
   - `ys`: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377] (measured QEDs)
   - Predicted QED for `m05`: **0.5416**.

## Task 2: Selecting a Lead Molecule

1. **Constraints**:
   - QED ≥ 0.6
   - `psa` ≥ 40
   - Not in the same congeneric series as `m05`.
2. Measured QED for molecules from other scaffolds:
   - `m02`: QED = 0.6046, `psa` = 72.24 (satisfies all constraints).
   - Other molecules either had QED < 0.6 or `psa` < 40.
3. Selected `m02` as the lead molecule.

## Measurements Used

- Total measurements: 12 (budget exhausted).
- Measurements for Task 1: 5
- Measurements for Task 2: 7