# Notes

## Task 1: Predicting QED for m05

1. **Grouped molecules by scaffold** to identify the congeneric series for `m05`.
2. **Measured QED** for molecules in the same scaffold (`m15`, `m24`, `m28`, `m17`, `m22`).
3. **Fitted a polynomial trend** using chain lengths as `xs` and measured QEDs as `ys`.
4. **Predicted QED** for `m05` (chain length 11) as `0.5416`.

## Task 2: Selecting a Lead Molecule

1. **Filtered molecules** based on:
   - Measured QED ≥ 0.6 (`m02`, `m04`).
   - `psa` ≥ 40 (from `data/pool.tsv`).
   - Not in the same congeneric series as `m05`.
2. **Selected `m02`** as the lead molecule:
   - Measured QED: `0.6046`.
   - `psa`: `72.24`.
   - Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (different from `m05`).