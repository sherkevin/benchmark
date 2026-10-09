# Notes for Medicinal Chemistry Task

## Task Overview
1. Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - this molecule has never been synthesized
2. Find one molecule that satisfies:
   - QED ≥ 0.6
   - PSA ≥ 40
   - Not in same congeneric series as m05 (different scaffold or same scaffold but same chain length)

## Initial Observations
- Total of 33 molecules in the pool
- Budget: 12 QED measurements total for both tasks
- m05 has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (12-carbon alkyl chain)
- Need to use `group_by_scaffold()` to understand congeneric series
- Need to use `fit_trend()` to predict m05 QED from its congeneric series
- For Task 2, need to measure QED of potential candidates to verify QED ≥ 0.6

## Scaffold Groups Identified
1. Scaffold "COc1ccc(cc1)C(=O)N" (m05's series): 
   - m15(2): QED = 0.7621
   - m24(3): QED = 0.7919
   - m28(4): QED = 0.7518
   - m17(5): QED = 0.7498
   - m22(6): QED = 0.7377
   - m05(11): needs prediction
2. Scaffold "O=[N+]([O-])c1ccc(cc1)C(=O)N": m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. Scaffold "OCC(O)C(O)C(O)C(=O)N": m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. Scaffold "c1ccc(cc1)-c1ccc(cc1)C(=O)N": m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Measurements Used: 5/12 budget remaining

## Task 1 Analysis
- Measured QED for m15(2)=0.7621, m24(3)=0.7919, m28(4)=0.7518, m17(5)=0.7498, m22(6)=0.7377
- Chain lengths: [2, 3, 4, 5, 6], QEDs: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- Predicting m05 (chain length 11):
  - Degree 1 (linear): 0.695
  - Degree 2 (quadratic): 0.5416
  - Degree 3 (cubic): 2.1323 (invalid, >1)
- Degree 2 prediction (0.5416) seems most reasonable given downward trend

## Task 2 Strategy
Need molecule with:
1. QED ≥ 0.6
2. PSA ≥ 40
3. Different scaffold from m05

PSA analysis:
- Scaffold 1 (m05 series): PSA = 38.33 < 40 ❌
- Scaffold 2 (nitro): PSA = 72.24 ≥ 40 ✅
- Scaffold 3 (polyol): PSA = 110.02 ≥ 40 ✅
- Scaffold 4 (biphenyl): PSA = 29.1 < 40 ❌

So candidates must be from Scaffold 2 or 3.

## Measurements Taken (12/12 budget used)
1. m15: 0.7621 (scaffold 1)
2. m24: 0.7919 (scaffold 1)  
3. m28: 0.7518 (scaffold 1)
4. m17: 0.7498 (scaffold 1)
5. m22: 0.7377 (scaffold 1)
6. m09: 0.5826 (scaffold 2)
7. m02: 0.6046 (scaffold 2) ✅ QED ≥ 0.6, PSA ≥ 40, different scaffold
8. m12: 0.4709 (scaffold 2)
9. m14: 0.3237 (scaffold 3)
10. m27: 0.333 (scaffold 3)
11. m32: 0.4683 (scaffold 2)
12. m33: 0.4591 (scaffold 2)

## Task 2 Candidate
m02 satisfies all criteria:
- QED = 0.6046 ≥ 0.6 ✅
- PSA = 72.24 ≥ 40 ✅  
- Different scaffold from m05 (scaffold 2 vs scaffold 1) ✅

No other measured candidates have QED ≥ 0.6.

## Final Decisions
### Task 1: m05 QED Prediction
Using quadratic fit (degree 2) with chain lengths [2,3,4,5,6] and QEDs [0.7621,0.7919,0.7518,0.7498,0.7377], predicted QED for chain length 11 is 0.5416.

### Task 2: Lead Molecule Selection
Selected m02 because:
1. Measured QED = 0.6046 ≥ 0.6
2. PSA = 72.24 ≥ 40
3. Different scaffold from m05 (nitro series vs methoxy series)

All 12 measurement budget units used.