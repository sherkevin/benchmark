# Prediction for m05 and Lead Selection

## Job 1 - Predicting m05's QED

1. **Identified m05's congeneric series** by finding molecules with the same scaffold (`COc1ccc(cc1)C(=O)N`) but varying chain lengths:
   - m15: NCC (3 carbons)
   - m24: NCCC (4 carbons)
   - m28: NCCCC (5 carbons)
   - m17: NCCCCC (6 carbons)
   - m22: NCCCCCC (7 carbons)

2. **Measured QED for these molecules**:
   - m15: 0.7621
   - m24: 0.7919
   - m28: 0.7518
   - m17: 0.7498
   - m22: 0.7377

3. **Fitted a quadratic trend** to the chain length vs QED data:
   - Fit equation: QED = 0.729 + 0.0236x - 0.00326x²
   - Predicted QED for m05 (12 carbons): 0.5416

## Job 2 - Selecting Backup Lead Compound

1. **Screened for candidates** with:
   - psa ≥ 40
   - Not in m05's series

2. **Measured QED** for promising candidates until finding one with QED ≥ 0.6:
   - m01: 0.3187 (too low)
   - m02: 0.6046 (meets criteria)

3. **Selected m02** as it meets all three constraints:
   - Measured QED: 0.6046 ≥ 0.6
   - psa: 72.24 ≥ 40
   - Different scaffold from m05 (not in same series)