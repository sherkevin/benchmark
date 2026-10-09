# Notes on Predicting QED for `m05` and Selecting a Backup Lead

## Job 1: Predicting QED for `m05`
1. **Identified congeneric series for `m05`**:
   - Scaffold: `COc1ccc(cc1)C(=O)N`.
   - Molecules in the same series: `m15`, `m24`, `m28`, `m17`, `m22`.
2. **Measured QED for the series**:
   - `m15` (chain length = 2): QED = 0.7621.
   - `m24` (chain length = 3): QED = 0.7919.
   - `m28` (chain length = 4): QED = 0.7518.
   - `m17` (chain length = 5): QED = 0.7498.
   - `m22` (chain length = 6): QED = 0.7377.
3. **Fitted a quadratic trend**:
   - Extrapolated to `m05` (chain length = 11): QED = **0.5416**.

## Job 2: Selecting a Backup Lead
1. **Filtered candidates with `psa ≥ 40`**: 18 molecules passed.
2. **Excluded molecules in the same series as `m05`**.
3. **Measured QED for potential leads**:
   - `m02`: QED = 0.6046 (meets QED ≥ 0.6).
4. **Selected `m02` as the backup lead**.
   - `psa` = 72.24 ≥ 40.
   - Not in the same series as `m05`.

## Summary
- Predicted QED for `m05`: **0.5416**.
- Backup lead: `m02` (QED = 0.6046).