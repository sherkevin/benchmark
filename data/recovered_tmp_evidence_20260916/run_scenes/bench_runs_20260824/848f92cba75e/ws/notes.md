# Analysis Notes

## Job 1: Predicting QED for m05

### Identifying the congeneric series for m05
Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has the scaffold "COc1ccc(cc1)C(=O)N" with an N-alkyl chain of 11 carbon atoms.

Other molecules in the same series (same scaffold, different chain lengths):
- m15: COc1ccc(cc1)C(=O)NCC (chain length 2)
- m24: COc1ccc(cc1)C(=O)NCCC (chain length 3) 
- m28: COc1ccc(cc1)C(=O)NCCCC (chain length 4)
- m17: COc1ccc(cc1)C(=O)NCCCCC (chain length 5)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (chain length 6)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (chain length 11)

### Measuring QED values for the series
- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

### Fitting the trend and predicting m05's QED
Using polynomial fitting (degree 2) with the measured points:
- xs: [2, 3, 4, 5, 6]
- ys: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- Query for x=11 (m05's chain length)

Predicted QED for m05: 0.5416

## Job 2: Finding a lead compound

### Criteria for lead compound:
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free descriptor)
3. Not in the same series as m05

### Screening process:
Used screen_candidates() to find molecules with PSA ≥ 40, then checked that they're not in m05's series, then measured QED.

### Testing candidates:
- m01: QED = 0.3187 (< 0.6) ❌
- m02: QED = 0.6046 (≥ 0.6) ✅, PSA = 72.24 (≥ 40) ✅, different scaffold ✅
- m03: QED = 0.3282 (< 0.6) ❌
- m06: QED = 0.4011 (< 0.6) ❌
- m14: QED = 0.3237 (< 0.6) ❌
- m10: QED = 0.3724 (< 0.6) ❌
- m12: QED = 0.4709 (< 0.6) ❌

### Lead compound selected: m02
- Meets all three criteria
- Highest QED among qualifying candidates