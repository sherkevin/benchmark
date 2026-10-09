# Medicinal Chemistry Task Notes

## Task Overview

I need to complete two tasks:
1. Predict the QED of molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - never been synthesized
2. Find one molecule with: 
   - Measured QED ≥ 0.6
   - PSA ≥ 40
   - Not in the same congeneric series as m05 (different scaffold or not just N-alkyl chain length variation)

I have 12 total measurements budget shared between both tasks.

## Initial Analysis

From the data file:
- m05 has SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- It has PSA = 38.33, which is below 40
- I need to identify its scaffold and N-alkyl chain

## Scaffold Analysis Results

From `group_by_scaffold()`:
1. Series 1 (m05's series): Scaffold "COc1ccc(cc1)C(=O)N" 
   - Members: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
   - All have PSA = 38.33 (from data file)

2. Series 2: Scaffold "O=[N+]([O-])c1ccc(cc1)C(=O)N"
   - Members: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
   - All have PSA = 72.24 (from data file) - meets PSA ≥ 40 requirement

3. Series 3: Scaffold "OCC(O)C(O)C(O)C(=O)N"
   - Members: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
   - All have PSA = 110.02 (from data file) - meets PSA ≥ 40 requirement

4. Series拨Scaffold "c1ccc(cc1)-c1ccc(cc1)C(=O)N"
   - Members: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)
   - All have PSA = 29.1 (from data file) - does NOT meet PSA ≥ 40 requirement

## Measurements So Far

Task 1 measurements (m05 series):
1. m15 (chain_len: 2): QED = 0.7621
2. m24 (chain_len: 3): QED = 0.7919  
3. m28 (chain_len: 4): QED = 0.7518
4. m17 (chain_len: 5): QED = 0.7498
5. m22 (chain_len: 6): QED = 0.7377

Pattern: QED seems to decrease with increasing chain length beyond 3 carbons.

m05 prediction using fit_trend (degree 2):
- Chain lengths: [2, 3, 4, 5, 6]
- QEDs: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- Predicted QED for m05 (chain_len: 11): 0.5416

Task 2 measurement:
6. m02 (Series 2, nitrobenzene scaffold): QED = 0.6046 ✓

## Analysis of m02 for Task 2:
- Scaffold: "O=[N+]([O-])c1ccc(cc1)C(=O)N" (different from m05's scaffold)
- PSA = 72.24 ≥ 40 ✓
- QED = 0.6046 ≥ 0.6 ✓
- Not in same series as m05 (different scaffold) ✓

m02 satisfies all requirements for Task 2!

Remaining budget: 6 measurements (12 total - 6 used)

## Final Results

### Task 1: QED prediction for m05
- Used 5 measurements from m05's series (m15, m24, m28, m17, m22)
- Applied polynomial fit (degree 2) to predict QED for chain length 11
- **Predicted QED: 0.5416**

### Task 2: Find molecule meeting all criteria
- Criteria 1: Measured QED ≥ 0.6 ✓
- Criteria 2: PSA ≥ 40 ✓  
- Criteria 3: Not in same series as m05 ✓
- Selected molecule: **m02**
- Measured QED: **0.6046**
- PSA: 72.24 (from data file)
- Scaffold: "O=[N+]([O-])c1ccc(cc1)C(=O)N" (different from m05's scaffold)

### Budget Usage
- Total measurements used: 6 out of 12 budget
- Efficient use of resources