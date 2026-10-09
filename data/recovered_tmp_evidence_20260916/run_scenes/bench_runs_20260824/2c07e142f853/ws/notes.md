# Notes

## Task Overview
- Task 1: Predict QED of m05 (never synthesized, must predict from congeneric series trend)
- Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, NOT in m05's congeneric series
- Budget: 12 measurements total

## Molecule Analysis

### Congeneric Series Identification
m05 has scaffold `COc1ccc(cc1)C(=O)N-` (methoxy-phenyl with amide).

Molecules in m05's series (same scaffold, different N-alkyl chain lengths):
- m15: NCC (2C)
- m24: NCCC (3C)
- m28: NCCCC (4C)
- m17: NCCCCC (5C)
- m22: NCCCCCC (6C)
- m05: NCCCCCCCCCCC (11C) - target

Other series (NOT same as m05):
- Nitro-phenyl series (O=[N+]([O-])c1ccc(cc1)C(=O)N-): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar-like series (OCC(O)C(O)C(O)C(=O)N-): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Biphenyl series (c1ccc(cc1)-c1ccc(cc1)C(=O)N-): m04, m08, m11, m13, m16, m20, m23, m26, m30

### Measurements Taken (12 total)

**m05 congeneric series (for Task 1 prediction):**
1. m15 (2C): QED = 0.7621
2. m24 (3C): QED = 0.7919
3. m28 (4C): QED = 0.7518
4. m17 (5C): QED = 0.7498
5. m22 (6C): QED = 0.7377

**Task 2 candidates:**
6. m01 (sugar, psa=110.02): QED = 0.3187 - FAILS (QED < 0.6)
7. m02 (nitro, psa=72.24): QED = 0.6046 - **QUALIFIES!**
8. m09 (nitro, psa=72.24): QED = 0.5826 - FAILS (QED < 0.6)
9. m12 (nitro, psa=72.24): QED = 0.4709 - FAILS
10. m32 (nitro, psa=72.24): QED = 0.4683 - FAILS
11. m33 (nitro, psa=72.24): QED = 0.4591 - FAILS
12. m29 (nitro, psa=72.24): QED = 0.4446 - FAILS

## Task 1: QED Prediction for m05

Trend in m05 series:
- Peak QED at 3C (m24): 0.7919
- Decrease from 3C to 6C: 0.7919 → 0.7377 = -0.0542 over 3 carbons
- Rate: ~0.018 per carbon

m05 has 11 carbons. Extrapolating from 3C:
- 11C - 3C = 8 carbons difference
- Estimated drop: 8 × 0.018 = 0.144
- Predicted QED: 0.7919 - 0.144 = 0.648

Alternative: from 6C:
- 11C - 6C = 5 carbons difference
- Estimated drop: 5 × 0.018 = 0.09
- Predicted QED: 0.7377 - 0.09 = 0.648

Considering m05's properties (mw=305.46, alogp=4.956, rotb=12) are significantly worse than m22 (mw=235.33, alogp=3.005, rotb=7), the QED penalty could be more severe.

**Final prediction for m05 QED: 0.60**

## Task 2: Lead Molecule Selection

**m02 qualifies:**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Nitro-phenyl series, NOT same as m05's methoxy-phenyl series ✓

**Answer:**
- target_qed: 0.60 (predicted for m05)
- lead_mol_id: m02
- lead_qed_measured: 0.6046
