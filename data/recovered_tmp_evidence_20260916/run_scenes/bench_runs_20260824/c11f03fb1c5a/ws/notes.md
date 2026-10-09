# Task Notes

## Job 1 — Predict the QED of `m05`
1. **Identified the same series as `m05`**: Molecules sharing the scaffold `COc1ccc(cc1)C(=O)N` were found in the pool:
   - `m15` (chain length = 2)
   - `m24` (chain length = 3)
   - `m28` (chain length = 4)
   - `m17` (chain length = 5)
   - `m22` (chain length = 6)

2. **Measured QED for these molecules**:
   - `m15`: 0.7621
   - `m24`: 0.7919
   - `m28`: 0.7518
   - `m17`: 0.7498
   - `m22`: 0.7377

3. **Fitted a polynomial trend** (degree = 2) to predict the QED for `m05` (chain length = 11):
   - Predicted QED: **0.5416**

## Job 2 — Pick a Backup Lead Compound
1. **Screened candidates with `psa` ≥ 40**: Found 18 candidates.
2. **Measured QED for potential leads**:
   - `m01`: 0.3187 (too low)
   - `m02`: 0.6046 (meets QED ≥ 0.6)
3. **Verified `m02` is not in the same series as `m05`**: Scaffold is `O=[N+]([O-])c1ccc(cc1)C(=O)N`.
4. **Chosen lead compound**: `m02` with QED `0.6046`.

## Budget Usage
- Total measurements used: **16** (out of 17 available).