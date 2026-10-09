# Task Notes

## Job 1: Predict QED for `m05`

1. **Identified Congeneric Series**:
   - Molecules sharing the scaffold `COc1ccc(cc1)C(=O)N`:
     - `m15` (chain length: 2, QED: 0.7621)
     - `m24` (chain length: 3, QED: 0.7919)
     - `m28` (chain length: 4, QED: 0.7518)
     - `m17` (chain length: 5, QED: 0.7498)
     - `m22` (chain length: 6, QED: 0.7377).

2. **Trend Fitting**:
   - Used `fit_trend` with `degree=2` to fit a polynomial trend to the measured QED values.
   - Extrapolated QED for `m05` (chain length: 11): **0.5416**.

## Job 2: Pick a Backup Lead Compound

1. **Screened Candidates**:
   - Used `screen_candidates()` to list molecules with `psa ≥ 40`.
   - Excluded molecules in the same series as `m05`.

2. **Measured QED**:
   - Measured QED for `m02`: **0.6046** (meets `QED ≥ 0.6`).
   - Verified `m02` is not in the same series as `m05`.

3. **Final Lead**:
   - Selected `m02` as the backup lead compound.