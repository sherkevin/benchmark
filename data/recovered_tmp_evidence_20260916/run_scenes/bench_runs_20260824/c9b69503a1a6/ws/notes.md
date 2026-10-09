# Notes on Task Completion

## Task 1: Predict QED for `m05`

1. **Grouped molecules by scaffold** to identify the congeneric series for `m05` (scaffold: `COc1ccc(cc1)C(=O)N`).
2. **Measured QED** for members of the same series (`m15`, `m24`, `m28`, `m17`, `m22`) to establish a trend.
3. **Fitted a polynomial** (degree 2) to the measured QED values vs. chain lengths:
   - Chain lengths: [2, 3, 4, 5, 6]
   - QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
4. **Predicted QED** for `m05` (chain length = 11) as **0.5416**.

## Task 2: Select a Lead Molecule

1. **Identified constraints**:
   - QED ≥ 0.6
   - `psa` ≥ 40
   - Not in the same series as `m05`.
2. **Measured QED** for candidates from other scaffolds:
   - `m02` (QED = 0.6046, `psa` = 72.24) — **satisfies all constraints**.
   - Others (`m12`, `m32`, `m33`, `m29`, `m14`, `m27`) did not meet QED or `psa` requirements.
3. **Selected `m02`** as the lead molecule.

## Final Answer

```json
{
  "target_qed": 0.5416,
  "lead_mol_id": "m02",
  "lead_qed_measured": 0.6046
}
```