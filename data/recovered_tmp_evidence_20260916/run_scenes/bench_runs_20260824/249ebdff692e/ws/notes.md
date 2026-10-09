# Notes for QED Prediction and Lead Compound Selection

## Job 1: Predict the QED of m05

### Identifying Congeners of m05
- Molecule m05 has SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Its scaffold is: COc1ccc(cc1)C(=O)N
- Its N-alkyl chain length is: 11

Other molecules in the same series (same scaffold, different chain lengths):
- m15: COc1ccc(cc1)C(=O)NCC (chain length 2)
- m24: COc1ccc(cc1)C(=O)NCCC (chain length 3)
- m28: COc1ccc(cc1)C(=O)NCCCC (chain length 4)
- m17: COc1ccc(cc1)C(=O)NCCCCC (chain length 5)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (chain length 6)

### Measuring QED Values of Congeners
- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

### Fitting Trend and Extrapolating
Using quadratic fit (degree=2) with the measured points:
- xs = [2, 3, 4, 5, 6]
- ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- x_query = 11 (for m05)

Result: Predicted QED for m05 = 0.5416

## Job 2: Selecting a Lead Compound

### Screening Candidates with PSA ≥ 40
Used screen_candidates() to get molecules with PSA ≥ 40.

### Filtering Out Molecules in Same Series as m05
Eliminated molecules with scaffold COc1ccc(cc1)C(=O)N (same as m05).

### Testing Remaining Candidates for QED ≥ 0.6
Tested various candidates that passed PSA screen and were not in m05's series:

- m01: Different scaffold, PSA=110.02 ✓, but QED=0.3187 ✗ (< 0.6)
- m02: Different scaffold, PSA=72.24 ✓, QED=0.6046 ✓ → MEETS ALL CRITERIA
- m03: Different scaffold, PSA=110.02 ✓, but QED=0.3282 ✗ (< 0.6)
- m06: Different scaffold, PSA=72.24 ✓, but QED=0.4011 ✗ (< 0.6)
- m10: Different scaffold, PSA=72.24 ✓, but QED=0.3724 ✗ (< 0.6)

### Final Lead Compound
Selected m02 because it meets all three criteria:
1. Measured QED ≥ 0.6: 0.6046 ✓
2. PSA ≥ 40: 72.24 ✓
3. Not in same series as m05: Different scaffold ✓