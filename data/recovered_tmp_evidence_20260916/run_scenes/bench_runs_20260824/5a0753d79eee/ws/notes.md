# Task Notes

## Job 1: Predict QED for `m05`

1. **Congeneric Series Identification**:
   - The SMILES string for `m05` is `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`.
   - The scaffold is `COc1ccc(cc1)C(=O)N`, and the N-alkyl chain length varies.
   - Molecules in the same series: `m15`, `m24`, `m28`, `m17`, `m22`.

2. **QED Measurements**:
   - `m15`: 0.7621 (chain length: 2)
   - `m24`: 0.7919 (chain length: 3)
   - `m28`: 0.7518 (chain length: 4)
   - `m17`: 0.7498 (chain length: 5)
   - `m22`: 0.7377 (chain length: 6)

3. **Trend Analysis**:
   - QED peaks at chain length 3 (`m24`) and decreases as the chain grows longer.
   - For `m05` (chain length: 12), the QED is predicted to be **0.68** (extrapolated downward).

## Job 2: Backup Lead Compound

1. **Constraints**:
   - Measured QED ≥ 0.6
   - `psa` ≥ 40
   - Not in the same series as `m05`.

2. **Candidates**:
   - Measured `m01`, `m03`, `m07`, `m14`, `m18`, `m19`, `m21`.
   - None met QED ≥ 0.6.
   - **No suitable backup lead found**.

## Conclusion
- Predicted QED for `m05`: **0.68**.
- No backup lead compound met all constraints.