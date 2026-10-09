# Notes

## Approach

### Scaffold Grouping
Used `group_by_scaffold()` to identify 4 congeneric series:
1. **COc1ccc(cc1)C(=O)N** (methoxybenzamide): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** (nitrobenzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **OCC(O)C(O)C(O)C(=O)N** (sugar-amide): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** (biphenyl-amide): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

m05 belongs to scaffold 1 with chain_len=11.

### Task 1: Predict QED of m05

Measured all other scaffold 1 members:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |
| m05    | 11        | ???    |

Trend: QED peaks around chain 3 then declines as lipophilicity and rotatable bonds increase.

Fitted degree-2 polynomial through all 5 points:
- Coefficients: 0.74932 + 0.017024*x - 0.003264*x²
- RMSE = 0.01188
- Prediction at x=11: **0.5416**

Cross-checked with other fits:
- Linear (all 5): 0.695
- Linear (chains 3-6): 0.6508
- Degree-2 (chains 3-6): 0.9378 (unrealistic upward curve)
- Degree-3 (all 5): 2.13 (overfitting)

The degree-2 fit with all 5 points is the most physically reasonable: it captures the initial peak and subsequent decline, and the extrapolated value (~0.54) is consistent with m05's high logP (4.956) and many rotatable bonds (12).

### Task 2: Find a lead molecule

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's series (scaffold 1).

PSA filter eliminates scaffold 1 (PSA=38.33) and scaffold 4 (PSA=29.1).
Candidates: scaffold 2 (PSA=72.24) and scaffold 3 (PSA=110.02).

Measured scaffold 2 candidates:
- m09 (chain 2): QED = 0.5826 — below 0.6 ✗
- m02 (chain 3): QED = 0.6046 — **qualifies** ✓
- m12 (chain 4): QED = 0.4709 — below 0.6 ✗

Measured scaffold 3 candidates:
- m14 (chain 2): QED = 0.3237 — below 0.6 ✗
- m27 (chain 3): QED = 0.3330 — below 0.6 ✗
- m01 (chain 4): QED = 0.3187 — below 0.6 ✗
- m19 (chain 5): QED = 0.3244 — below 0.6 ✗

**Selected lead: m02** (QED=0.6046, PSA=72.24, scaffold 2 ≠ scaffold 1)

### Budget used: 12/12 measurements
