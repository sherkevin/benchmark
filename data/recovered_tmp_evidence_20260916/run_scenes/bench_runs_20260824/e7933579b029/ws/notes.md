# Notes

## Task Overview
- Task 1: Report QED of m05 (cannot be directly measured, need to predict from congeneric series trend)
- Task 2: Pick one molecule with QED >= 0.6, psa >= 40, not in same congeneric series as m05
- Budget: 12 measurements total

## Scaffold Groups
1. **Scaffold COc1ccc... (m05's series)**: m15, m24, m28, m17, m22, m05 (chain_len=11) - psa=38.33
2. **Scaffold O=[N+]... (nitro)**: m09, m02, m12, m32, m33, m29, m31, m06, m10 - psa=72.24
3. **Scaffold OCC(O)... (sugar-like)**: m14, m27, m01, m19, m03, m21, m18, m07, m25 - psa=110.02
4. **Scaffold c1ccc... biphenyl**: m13, m11, m04, m30, m23, m20, m26, m08, m16 - psa=29.1

For Task 2, candidates must be from scaffold 2 or 3 (psa >= 40 rules out scaffolds 1 and 4).

## Task 1: Predicting m05 QED
Measured m05's congeneric series:
- m15 (chain=2): QED=0.7621
- m24 (chain=3): QED=0.7919
- m28 (chain=4): QED=0.7518
- m17 (chain=5): QED=0.7498
- m22 (chain=6): QED=0.7377

Fit polynomial (degree 2) to predict at chain_len=11:
- **Predicted QED for m05: 0.5416**

## Task 2: Finding a lead molecule
Measured candidates from scaffold 2 and 3:
- m09 (scaffold 2): QED=0.5826 - fails QED >= 0.6
- **m02 (scaffold 2): QED=0.6046** - PASSes all criteria!
  - QED=0.6046 >= 0.6 ✓
  - psa=72.24 >= 40 ✓
  - Not in m05's series (scaffold 2, not scaffold 1) ✓
- m14 (scaffold 3): QED=0.3237 - fails QED >= 0.6
- m27 (scaffold 3): QED=0.333 - fails QED >= 0.6

**Selected lead molecule: m02** with measured QED=0.6046

## Summary
- Task 1: m05 predicted QED = 0.5416
- Task 2: m02 selected, measured QED = 0.6046

Measurements used: 9/12
