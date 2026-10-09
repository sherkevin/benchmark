# Reasoning for QED Prediction and Lead Selection

## Job 1: Predicting QED for m05

### Congeneric Series Identification
- **m05's SMILES**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (chain length: 12)
- **Congeneric series members**:
  - `m15`: Chain length 2, QED: 0.7621
  - `m24`: Chain length 3, QED: 0.7919
  - `m28`: Chain length 4, QED: 0.7518
  - `m17`: Chain length 5, QED: 0.7498
  - `m22`: Chain length 6, QED: 0.7377

### Trend Analysis
- QED peaks at chain length 3 (`m24`) and decreases as the chain grows longer.
- For `m05` (chain length 12), the QED is extrapolated to **0.65** based on the downward trend.

## Job 2: Backup Lead Compound

### Constraints
1. **QED ≥ 0.6**: Must measure.
2. **`psa` ≥ 40**: From pool data.
3. **Not in m05's series**: From SMILES comparison.

### Attempted Candidates
- Measured `m01`, `m03`, `m07`, `m14`, `m18`, `m19`, `m21` — all had QED < 0.6.
- Exhausted measurement budget (12/12 used).

### Conclusion
No suitable backup lead compound found within the given constraints.