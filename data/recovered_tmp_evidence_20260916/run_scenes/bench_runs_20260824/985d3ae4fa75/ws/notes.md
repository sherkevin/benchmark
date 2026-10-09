# Notes on Predicting QED for `m05` and Selecting a Backup Lead

## Job 1: Predicting QED for `m05`

### Congeneric Series Identification
- Molecules in the same series as `m05` (scaffold: `COc1ccc(cc1)C(=O)N`) were identified by their SMILES:
  - `m15`: 2-carbon chain, QED = 0.7621
  - `m17`: 5-carbon chain, QED = 0.7498
  - `m22`: 6-carbon chain, QED = 0.7377

### Trend Fitting
- A degree-2 polynomial was fitted to the QED values of `m15`, `m17`, and `m22`.
- The predicted QED for `m05` (12-carbon chain) is **0.5811**.

## Job 2: Selecting a Backup Lead Compound

### Criteria
1. **QED ≥ 0.6**: Measured via `measure_qed`.
2. **`psa` ≥ 40**: Filtered via `screen_candidates`.
3. **Not in `m05` series**: Verified by scaffold comparison.

### Selected Lead
- **`m02`**:
  - QED = 0.6046 (≥ 0.6)
  - `psa` = 72.24 (≥ 40)
  - Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (not in `m05` series)