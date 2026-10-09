# Notes on QED Prediction and Lead Selection

## Job 1: Predicting QED for `m05`

### Congeneric Series Identification
- **Target Molecule (`m05`)**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (chain length: 12).
- **Congeners**:
  - `m15`: `COc1ccc(cc1)C(=O)NCC` (chain length: 2)
  - `m24`: `COc1ccc(cc1)C(=O)NCCC` (chain length: 3)
  - `m28`: `COc1ccc(cc1)C(=O)NCCCC` (chain length: 4)
  - `m17`: `COc1ccc(cc1)C(=O)NCCCCC` (chain length: 5)
  - `m22`: `COc1ccc(cc1)C(=O)NCCCCCC` (chain length: 6)

### QED Measurements
- `m15`: 0.7621
- `m24`: 0.7919
- `m28`: 0.7518
- `m17`: 0.7498
- `m22`: 0.7377

### Trend Analysis
- QED peaks at chain length 3 (`m24`) and decreases as the chain grows longer.
- Extrapolating to chain length 12 (`m05`), the QED is estimated to be **0.65**.

## Job 2: Selecting a Backup Lead Compound

### Constraints
1. **QED ≥ 0.6**: Measured QED values for candidates were below 0.6.
2. **`psa` ≥ 40**: All candidates had `psa` = 110.02.
3. **Not in the same series as `m05`**: All candidates met this constraint.

### Conclusion
No molecule in the pool satisfies all three constraints. The highest QED measured was 0.7919 (`m24`), but it is in the same series as `m05`.