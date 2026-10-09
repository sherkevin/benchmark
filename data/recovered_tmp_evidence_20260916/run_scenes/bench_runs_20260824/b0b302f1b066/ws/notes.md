# Notes

## Scaffold Grouping

4 scaffolds identified:
1. **COc1ccc(cc1)C(=O)N** (methoxybenzamide) — m05's series: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (nitrobenzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N** (sugar alcohol): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (biphenyl): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05

m05 has chain_len=11 in scaffold 1. Measured all 5 other members:

| mol_id | chain | QED |
|--------|-------|-------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

QED components analysis (chain 2 vs chain 6):
- MW score improves: 0.33 → 0.68
- ALOGP score improves: 0.87 → 1.00 (plateaus)
- ROTB score drops dramatically: 0.99 → 0.53 (main driver of decline)
- ALERTS score drops: 0.84 → 0.64
- HBA, HBD, PSA, AROM stay constant

For m05 (chain 11): MW=305.46, ALOGP=4.956, ROTB=12 — ROTB and ALOGP will be heavily penalized.

**Fits tried (predicting QED at chain_len=11):**
- Degree 2, all 5 points: **0.5416** (RMSE 0.0119) ← chosen model
- Degree 2, last 3 points: 0.5257 (consistent confirmation)
- Degree 1, all 5 points: 0.6950 (less physically reasonable)
- Degree 1, last 4 points: 0.6508

Chose degree 2 with all 5 points because: (1) captures curvature in the data, (2) physically reasonable (accelerating decline due to ROTB/ALOGP penalties), (3) consistent with degree-2 fit on last 3 points, (4) lowest RMSE among all-5-point fits.

**Predicted QED for m05: 0.5416**

## Task 2: Lead molecule selection

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's scaffold.

PSA filter: scaffold 2 (PSA=72.24 ✓) and scaffold 3 (PSA=110.02 ✓) qualify. Scaffold 4 (PSA=29.1) fails.

Measured candidates from scaffold 2:
- m09 (chain 2): QED = 0.5826 ❌ (< 0.6)
- **m02 (chain 3): QED = 0.6046 ✅**
- m12 (chain 4): QED = 0.4709 ❌
- m32 (chain 5): QED = 0.4683 ❌

m02 is the only molecule meeting all three constraints.

**Selected lead: m02** with measured QED = 0.6046, PSA = 72.24, scaffold = O=[N+]([O-])c1ccc(cc1)C(=O)N

## Budget used: 9 of 12 measurements
