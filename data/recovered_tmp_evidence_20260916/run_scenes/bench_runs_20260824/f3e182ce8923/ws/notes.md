# Notes for Task Completion

## Task 1: Predict QED for `m05`

1. **Scaffold Grouping**: Identified `m05`'s congeneric series (scaffold `COc1ccc(cc1)C(=O)N`).
2. **Measurements**: Measured QED for `m15` (0.7621), `m24` (0.7919), and `m28` (0.7518).
3. **Prediction**: Used linear fit to predict QED for `m05` (chain_len: 11) as **0.7274**.

## Task 2: Find a Molecule Meeting All Constraints

1. **Constraints**:
   - QED ≥ 0.6 (measured)
   - PSA ≥ 40
   - Not in `m05`'s congeneric series
2. **Measurements**:
   - Measured QED for several candidates (`m09`, `m14`, `m13`, etc.).
   - Best QED: `m13` (0.854), but PSA (29.1) is too low.
   - Best PSA: `m09` (72.24), but QED (0.5826) is just below 0.6.
3. **Conclusion**: No molecule satisfied all three constraints based on our measurements.

## Final Answer

- `target_qed`: 0.7274 (predicted for `m05`)
- `lead_mol_id`: `m13` (best QED, but PSA is too low)
- `lead_qed_measured`: 0.854