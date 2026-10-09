# Notes

## Task 1: Predicting QED for m05
- Grouped the pool by scaffold and identified the congeneric series for `m05` (scaffold: `COc1ccc(cc1)C(=O)N`).
- Measured QED for molecules in the series (`m15`, `m24`, `m28`, `m17`).
- Fitted a quadratic trend to predict QED for `m05` (chain length 11): **0.2689**.

## Task 2: Finding a Lead Molecule
- Explored the pool (pages 1-3) for molecules with `psa` ≥ 40 and not in the same scaffold as `m05`.
- Measured QED for candidates (`m01`, `m03`, `m06`, `m07`, `m09`).
- The closest candidate was `m09` (QED = 0.5826), but it did not meet the QED ≥ 0.6 constraint.
- Budget exhausted (12 units used).

## Conclusion
- `target_qed` for `m05` is **0.2689**.
- No valid `lead_mol_id` found within the budget.