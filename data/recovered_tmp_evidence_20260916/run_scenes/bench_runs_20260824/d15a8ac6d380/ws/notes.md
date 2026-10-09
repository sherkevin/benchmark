# Notes for Medicinal Chemistry Task

## Task 1: Predict QED for m05

### Data Collected:
- m05 scaffold: `COc1ccc(cc1)C(=O)N` with chain length 11
- Congeneric series members and their measured QED:
  1. m15 (chain_len: 2): QED = 0.7621
  2. m24 (chain_len: 3): QED = 0.7919
  3. m28 (chain_len: 4): QED = 0.7518
  4. m17 (chain_len: 5): QED = 0.7498
  5. m22 (chain_len: 6): QED = 0.7377

### Trend Analysis:
- Linear fit (degree 1): predicts QED = 0.695 for chain length 11
- Quadratic fit (degree 2): predicts QED = 0.5416 for chain length 11
- The linear fit has RMSE = 0.01308, quadratic has RMSE = 0.01188
- Based on the decreasing trend (0.7621 → 0.7919 → 0.7518 → 0.7498 → 0.7377), the linear prediction of 0.695 seems reasonable.

### Selected prediction for m05: 0.695

## Task 2: Find molecule meeting all constraints

### Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in same congeneric series as m05

### Candidate found: m02
- QED measured: 0.6046 ✓ (≥ 0.6)
- psa: 72.24 ✓ (≥ 40)
- Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` ✓ (different from m05's scaffold)
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Descriptors: mw=208.22, alogp=1.735, psa=72.24, rotb=4, arom=1

### Budget tracking:
- Pages viewed: 4 (pages 1-4) = 4 units
- Measurements: 7 (m15, m24, m28, m17, m22, m09, m02) = 7 units
- Additional page view (verification): 1 unit
- Total used: 12 units
- Remaining: 0 units