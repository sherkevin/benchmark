# Notes for Task Completion

## Task 1: Predicting QED for `m05`

1. **Identify the scaffold and congeneric series for `m05`:**
   - `m05` has the SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`.
   - The scaffold is `COc1ccc(cc1)C(=O)N`, and the N-alkyl chain length is 12.

2. **Group molecules by scaffold to find congeneric series:**
   - Used `group_by_scaffold()` to identify molecules with the same scaffold as `m05`.
   - Found molecules `m15`, `m24`, `m28`, `m17`, and `m22` in the same series.

3. **Measure QED for molecules in the same congeneric series:**
   - Measured QED for `m15` (0.7621), `m24` (0.7919), `m28` (0.7518), `m17` (0.7498), and `m22` (0.7377).
   - Used `fit_trend()` to predict QED for `m05` based on the measured values.
   - Predicted QED for `m05`: **0.5416**.

## Task 2: Selecting a Lead Molecule

1. **Filter molecules based on constraints:**
   - QED ≥ 0.6
   - `psa` ≥ 40
   - Not in the same congeneric series as `m05`.

2. **Measure QED for potential candidates:**
   - Measured QED for `m02` (0.6).
   - `m02` has `psa` = 72.24 and is not in the same series as `m05`.

3. **Select the best candidate:**
   - **Lead molecule:** `m02`
   - **Measured QED:** 0.6

## Final Answer

```json
{
  "target_qed": 0.5416,
  "lead_mol_id": "m02",
  "lead_qed_measured": 0.6
}
```