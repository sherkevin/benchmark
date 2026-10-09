# Notes for Medicinal Chemistry Task

## Task Overview
1. Need to predict QED for m05 (which has never been synthesized)
2. Need to find one molecule that satisfies all three constraints:
   - Measured QED ≥ 0.6
   - psa ≥ 40
   - Not in same congeneric series as m05 (different scaffold or same scaffold but with different modifications beyond N-alkyl chain length)

## Scaffold Groups Identified
1. Scaffold "COc1ccc(cc1)C(=O)N" (m05's group): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. Scaffold "O=[N+]([O-])c1ccc(cc1)C(=O)N": m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. Scaffold "OCC(O)C(O)C(O)C(=O)N": m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. Scaffold "c1ccc(cc1)-c1ccc(cc1)C(=O)N": m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Strategy for Task 1
- Need to predict QED for m05 (chain_len=11) using its congeneric series
- Should measure several molecules from m05's series to establish trend
- Use fit_trend to predict m05 QED at chain_len=11

## Strategy for Task 2
- Need to find molecule with psa ≥ 40 (already known from data)
- Must have measured QED ≥ 0.6
- Cannot be in same series as m05 (scaffold 1)
- Best to focus on scaffolds 2 and 3 which have higher psa values
- Scaffold 2 molecules all have psa=72.24
- Scaffold 3 molecules all have psa=110.02
- Scaffold 4 molecules have psa=29.1 (too low)

## Measurement Budget
- Total: 12 measurements
- Need to allocate wisely between Task 1 and Task 2

## Measurements Taken (5/12 budget used)
1. m15 (chain_len=2): QED=0.7621
2. m24 (chain_len=3): QED=0.7919
3. m28 (chain_len=4): QED=0.7518
4. m17 (chain_len=5): QED=0.7498
5. m22 (chain_len=6): QED=0.7377

## Observations
- QED seems to decrease slightly with increasing chain length
- Need to predict m05 (chain_len=11) using fit_trend

## Task 1 Prediction Results
- Quadratic fit (degree=2): predicted QED for m05 = 0.5416
- Linear fit (degree=1): predicted QED for m05 = 0.695

## Additional Measurements for Task 2 (7 more, total 12/12 used)
6. m02 (scaffold 2, chain_len=3): QED=0.6046 ✓ satisfies QED≥0.6, psa=72.24≥40, not in m05 series
7. m12 (scaffold 2, chain_len=4): QED=0.4709 ✗ too low
8. m32 (scaffold 2, chain_len=5): QED=0.4683 ✗ too low
9. m14 (scaffold 3, chain_len=2): QED=0.3237 ✗ too low
10. m27 (scaffold 3, chain_len=3): QED=0.3330 ✗ too low
11. m01 (scaffold 3, chain_len=4): QED=0.3187 ✗ too low
12. m33 (scaffold 2, chain_len=6): QED=0.4591 ✗ too low

## Final Decision
Task 1: Using quadratic fit (degree=2) which has lower RMSE (0.01188 vs 0.01308), predicted QED for m05 = 0.5416
Task 2: Best candidate is m02 with QED=0.6046

## Summary
- Used all 12 measurement budget
- Task 1: Predicted m05 QED using fit_trend on 5 measured molecules from same series
- Task 2: Found m02 satisfies all constraints: QED=0.6046≥0.6, psa=72.24≥40, not in m05 series
- Scaffold 3 molecules had very low QED due to poor ALOGP and HBD scores
- Scaffold的两个系列具有不同结构属性2 had moderate QED, scaffold 3 had low QED due to high polarity