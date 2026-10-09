# Task Notes

## Job 1: Predict the QED of `m05`

### Congeneric Series Identification
- **Target Molecule**: `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`)
- **Scaffold**: `COc1ccc(cc1)C(=O)N`
- **Congeneric Series Members**: `m15`, `m24`, `m17`, `m28`, `m22`

### QED Measurements
| mol_id | Chain Length (carbons) | QED     |
|--------|------------------------|---------|
| m15    | 2                      | 0.7621  |
| m24    | 3                      | 0.7919  |
| m17    | 5                      | 0.7498  |
| m28    | 4                      | 0.7518  |
| m22    | 6                      | 0.7377  |

### Trend Fitting
- Fitted a quadratic polynomial to the QED vs. chain length data.
- Predicted QED for `m05` (chain length: 12 carbons): **0.4836**.

## Job 2: Pick a Backup Lead Compound

### Screening Process
1. **Filtered for `psa ≥ 40`**: 18 candidates.
2. **Excluded `m05`'s series**: No overlap, so all 18 candidates remained.
3. **Measured QED** of candidates with high `psa` values:
   - None of the measured candidates met the QED ≥ 0.6 requirement.
   - Exhausted the measurement budget (12/12 used).

### Conclusion
No suitable backup lead compound was found within the measurement budget.