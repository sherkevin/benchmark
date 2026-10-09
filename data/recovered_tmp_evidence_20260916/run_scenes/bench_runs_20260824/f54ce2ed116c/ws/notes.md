# Notes

## Scaffold Grouping (free tool)
4 scaffold groups identified:
- **Scaffold A** (`COc1ccc(cc1)C(=O)N`): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
- **Scaffold B** (`O=[N+]([O-])c1ccc(cc1)C(=O)N`): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- **Scaffold C** (`OCC(O)C(O)C(O)C(=O)N`): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- **Scaffold D** (`c1ccc(cc1)-c1ccc(cc1)C(=O)N`): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

m05 is in Scaffold A with chain_len=11.

## Budget Usage
- list_pool pages: 3 (pages 1-3) → saw m01-m12 descriptors
- measure_qed calls: 9 (m15, m24, m28, m17, m22, m09, m02, m01, m12)
- Total: 12/12

## Task 1: Predicting m05 QED

### Scaffold A QED measurements:
| mol_id | chain_len | QED |
|--------|-----------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Trend fitting (extrapolating to chain_len=11):
- **Degree 1 (linear)**: QED ≈ 0.695, RMSE = 0.01308
- **Degree 2 (quadratic)**: QED ≈ 0.5416, RMSE = 0.01188
- Degree 3: 2.13 (diverges — overfitting)
- Degree 4: -12.10 (diverges — overfitting)

### Decision: Use quadratic fit (0.5416)
- Better RMSE than linear (0.01188 vs 0.01308)
- Physically motivated: QED penalties compound non-linearly with longer chains
  - m05 has logP=4.956 (very high), rotb=12 (high), both strongly penalized by QED
  - Each additional CH2 increases logP linearly, but QED penalty is non-linear
- The quadratic captures the accelerating decline better than linear

**Predicted m05 QED: ~0.54**

## Task 2: Lead Selection

### Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. Not in scaffold A (m05's series)

### PSA by scaffold (from catalogue):
- Scaffold B: psa = 72.24 ✓
- Scaffold C: psa = 110.02 ✓
- Scaffold D: psa = 29.1 ✗ (too low)
- Scaffold A: psa = 38.33 (excluded anyway)

### Measurements of non-A candidates:
| mol_id | scaffold | chain_len | QED | psa | Qualifies? |
|--------|----------|-----------|-----|-----|------------|
| m09 | B | 2 | 0.5826 | 72.24 | ✗ (QED < 0.6) |
| m02 | B | 3 | 0.6046 | 72.24 | ✓ |
| m01 | C | 4 | 0.3187 | 110.02 | ✗ (QED < 0.6) |
| m12 | B | 4 | 0.4709 | 72.24 | ✗ (QED < 0.6) |

### Selected lead: **m02**
- QED = 0.6046 (measured)
- PSA = 72.24 ≥ 40 ✓
- Scaffold B ≠ Scaffold A ✓
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
