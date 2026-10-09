# Task Notes

## Overview
I need to complete two tasks:
1. Predict the QED for m05 (target molecule) which hasn't been synthesized
2. Find a molecule that satisfies three constraints:
   - QED ≥ 0.6 (must be measured)
   - PSA ≥ 40 (from catalogue descriptor)
   - Not in the same congeneric series as m05 (different scaffold or not just N-alkyl chain length difference)

Budget: 12 units total shared between list_pool and measure_qed.

## Plan
1. First explore the pool to understand what's available
2. Use group_by_scaffold to identify congeneric series
3. Identify m05's scaffold and its series
4. Measure QED for molecules in m05's series to establish trend
5. Use fit_trend to predict m05's QED
6. Search for candidate molecules meeting Task 2 constraints
7. Measure QED for promising candidates

## Budget Management
Total: 12 units
Each list_pool page (4 entries): 1 unit
Each measure_qed: 1 unit

Budget used so far: 9 (pages 1-9)
Remaining: 3

Need to be strategic about measurements.

## Congeneric Series Analysis

Found 4 scaffold groups:

1. Scaffold: COc1ccc(cc1)C(=O)N (m05's series)
   Members: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
   
2. Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N
   Members: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)

3. Scaffold: OCC(O)C(O)C(O)C(=O)N
   Members: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)

4. Scaffold: c1ccc(cc1)-c1ccc(cc1)C(=O)N
   Members: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

For Task 2, need molecule NOT in series 1 (m05's series).

## Measurements Taken
1. m15 (chain_len=2): QED=0.7621
2. m24 (chain_len=3): QED=0.7919
3. m28 (chain_len=4): QED=0.7518

## Task 1 Prediction
Using linear fit (degree=1) with chain lengths [2,3,4] and QEDs [0.7621,0.7919,0.7518]:
Predicted QED for m05 (chain_len=11): 0.7274

## Task 2 Challenge
Budget exhausted (12/12 used). Need to find molecule with:
1. QED ≥ 0.6 (must measure)
2. PSA ≥ 40
3. Not in m05's series

Scaffold analysis:
- Scaffold 1 (m05's): PSA=38.33 < 40 (disqualified)
- Scaffold 2: PSA=72.24 ≥ 40 (qualified, not in m05's series)
- Scaffold 3: PSA=110.02 ≥ 40 (qualified, not in m05's series)
- Scaffold 4: PSA=29.1 < 40 (disqualified)

Need to measure QED for at least one molecule from scaffold 2 or 3, but no budget remaining.

## Task 2 Conclusion (based on available data)
Selected m02 as best candidate:
- PSA=72.24 ≥ 40 ✓
- Not in m05's series (scaffold 2) ✓
- Estimated QED ≥ 0.6: Based on similarity to m15 (QED=0.7621):
  - m02: mw=208.22, alogp=1.735, psa=72.24, rotb=4, arom=1
  - m15: mw=179.22, alogp=1.445, psa=38.33, rotb=3, arom=1
  - m02 has similar/higher PSA (better for QED), similar alogp, similar other descriptors
  - Likely QED similar to or higher than m15's 0.7621
  - Estimated QED: 0.77

Note: Cannot confirm with measurement due to budget exhaustion.