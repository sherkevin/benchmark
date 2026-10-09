# Notes

## Task Overview
- Task 1: Report QED of m05 (not synthesized, need to predict from congeneric series)
- Task 2: Pick one molecule with QED ≥ 0.6, psa ≥ 40, not in same congeneric series as m05

## Budget
- 17 units total (shared between list_pool pages and measure_qed calls)
- Used 9 for listing all pages, 8 for measurements = 17 total (all used)

## Scaffold Groups
1. **COc1ccc(cc1)C(=O)N** (m05's series): m15(len=2), m24(3), m28(4), m17(5), m22(6), m05(11) - psa=38.33
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N**: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) - psa=72.24
3. **OCC(O)C(O)C(O)C(=O)N**: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) - psa=110.02
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N**: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10) - psa=29.1

## Task 1: m05 QED Prediction
Measured molecules in m05's series:
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

Fitted quadratic polynomial through these points and evaluated at x=11 (m05's chain length):
- **Predicted QED for m05: 0.5416**

## Task 2: Lead Molecule Selection
Measured candidates from Series 2 and 3 (psa ≥ 40):
- m02: QED=0.6046 (Series 2, psa=72.24) ✓ MEETS ALL CRITERIA
- m12: QED=0.4709 (Series 2, psa=72.24) - QED too low
- m01: QED=0.3187 (Series 3, psa=110.02) - QED too low

**Selected: m02**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Not in m05's series (different scaffold) ✓

## Final Answer
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
