# Notes on Predicting QED for m05 and Selecting a Backup Lead

## Job 1: Predicting QED for m05

### Congeneric Series Identification
- **Target molecule (`m05`)**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (chain length: 12).
- **Congeneric series members** (same scaffold, varying chain lengths):
  - `m15`: Chain length = 2, QED = 0.7621
  - `m24`: Chain length = 3, QED = 0.7919
  - `m28`: Chain length = 4, QED = 0.7518
  - `m17`: Chain length = 5, QED = 0.7498
  - `m22`: Chain length = 6, QED = 0.7377

### Trend Fitting
- Fit a quadratic polynomial (`degree=2`) to the measured QED values:
  - **Trend equation**: QED = 0.74932 + 0.017024x - 0.003264x²
  - **Predicted QED for `m05` (x=12)**: 0.4836
  - **Note**: Extrapolation beyond the measured range (x=2 to x=6) is less reliable.

## Job 2: Selecting a Backup Lead Compound

### Constraints
1. **Measured QED ≥ 0.6**.
2. **`psa` ≥ 40**.
3. **Not in the same series as `m05`**.

### Screening Process
1. **Initial screening**: Used `screen_candidates()` to filter molecules with `psa` ≥ 40 (18 candidates).
2. **Excluded series**: Removed molecules with the scaffold `COc1ccc(cc1)C(=O)N`.
3. **Measured QED**: Selected `m02` (QED = 0.6046), which meets all constraints.

### Final Lead
- **Lead molecule**: `m02` (`O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`).
- **Measured QED**: 0.6046.
- **PSA**: 72.24.
- **Not in `m05`'s series**.

## Conclusion
- Predicted QED for `m05`: 0.4836.
- Selected backup lead: `m02` (QED = 0.6046).