# Task Notes

## Overview
I need to complete two tasks:
1. Predict the QED of molecule m05 (which has never been synthesized) using trend analysis
2. Find a molecule that meets all three criteria: QED ≥ 0.6, PSA ≥ 40, and not in same series as m05

I have a shared budget of 17 units for both list_pool and measure_qed calls.

## Initial Findings from group_by_scaffold()
Found 4 scaffold groups:
1. Scaffold: "COc1ccc(cc1)C(=O)N" - m05 belongs to this group (chain_len: 11)
   Members: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)

2. Scaffold: "O=[N+]([O-])c1ccc(cc1)C(=O)N"
   Members: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)

3. Scaffold: "OCC(O)C(O)C(O)C(=O)N"
   Members: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)

4. Scaffold: "c1ccc(cc1)-c1ccc(cc1)C(=O)N"
   Members: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## QED Measurements for m05's Series
Measured QED values:
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Budget used so far: 5 measurements, budget remaining: 17

## m05 QED Prediction
Using linear fit (degree=1) with chain lengths [2,3,4,5,6] and QED values [0.7621,0.7919,0.7518,0.7498,0.7377]:
Predicted QED for m05 (chain_len=11): 0.695

Quadratic fit (degree=2) gives 0.5416, but linear seems more reasonable given the trend.

## Molecules with PSA ≥ 40 (not in m05's series)
From browsing all 9 pages (budget used: 14 total, 9 for list_pool, 5 for measurements):

Candidates with PSA ≥ 40:
1. Scaffold 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N):
   - m02: PSA=72.24
   - m06: PSA=72.24
   - m09: PSA=72.24
   - m10: PSA=72.24
   - m12: PSA=72.24
   - m29: PSA=72.24
   - m31: PSA=72.24
   - m32: PSA=72.24
   - m33: PSA=72.24

2. Scaffold 3 (OCC(O)C(O)C(O)C(=O)N):
   - m01: PSA=110.02
   - m03: PSA=110.02
   - m07: PSA=110.02
   - m14: PSA=110.02
   - m18: PSA=110.02
   - m19: PSA=110.02
   - m21: PSA=110.02
   - m25: PSA=110.02
   - m27: PSA=110.02

All these are NOT in m05's series (Scaffold 1).

## QED Measurements for Candidates
Budget: Started with 17, used all 17 (9 list_pool + 8 measurements)

Measurements:
1. m01 (Scaffold 3): QED = 0.3187 < 0.6 ✗
2. m03 (Scaffold 3): QED = 0.3282 < 0.6 ✗
3. m02 (Scaffold 2): QED = 0.6046 ≥ 0.6 ✓

## Selected Molecule for Task 2
**m02** meets all criteria:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Not in same series as m05 (Scaffold 2 vs Scaffold 1) ✓

## Final Answer
- Predicted QED for m05: 0.695
- Selected molecule: m02 with QED = 0.6046