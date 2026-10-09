# Notes

## Series Identification

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxyphenyl carboxamide with C11 chain.

### Series 1 (methoxyphenyl, same scaffold as m05): `COc1ccc(cc1)C(=O)N-`
- m15: NCC (C2), m24: NCCC (C3), m28: NCCCC (C4), m17: NCCCCC (C5), m22: NCCCCCC (C6)
- **m05: NCCCCCCCCCCC (C11) — target, never synthesized**

### Series 2 (sugar acid amide): `OCC(O)C(O)C(O)C(=O)N-` — PSA=110.02
m14, m27, m01, m19, m03, m21, m18, m07, m25

### Series 3 (nitrophenyl amide): `O=[N+]([O-])c1ccc(cc1)C(=O)N-` — PSA=72.24
m09, m02, m12, m32, m33, m29, m31, m06, m10

### Series 4 (biphenyl amide): `c1ccc(cc1)-c1ccc(cc1)C(=O)N-` — PSA=29.1
m13, m11, m04, m30, m23, m20, m26, m08, m16
**PSA=29.1 < 40 → ALL excluded from Job 2**

---

## Measurements (12/12 used)

### Series 1 (m05's congeneric series):
| mol_id | Chain | QED    |
|--------|-------|--------|
| m15    | C2    | 0.7621 |
| m24    | C3    | 0.7919 |
| m28    | C4    | 0.7518 |
| m17    | C5    | 0.7498 |
| m22    | C6    | 0.7377 |

### Job 2 candidates:
| mol_id | Series | QED    | PSA    | QED≥0.6? |
|--------|--------|--------|--------|----------|
| m09    | 3 (C2) | 0.5826 | 72.24  | NO       |
| m02    | 3 (C3) | 0.6046 | 72.24  | **YES**  |
| m12    | 3 (C4) | 0.4709 | 72.24  | NO       |
| m32    | 3 (C5) | 0.4683 | 72.24  | NO       |
| m14    | 2 (C2) | 0.3237 | 110.02 | NO       |
| m27    | 2 (C3) | 0.3330 | 110.02 | NO       |
| m01    | 2 (C4) | 0.3187 | 110.02 | NO       |

---

## Job 1: Predicting QED of m05

### Observed Trend
QED peaks at C3 (0.7919), then declines: C3→C4 (-0.040), C4→C5 (-0.002), C5→C6 (-0.012).
The decline accelerates from C4 onward.

### Model: Quadratic fit to all 5 data points
Fitting y = ax² + bx + c where x = chain length, y = QED:

Normal equations solved:
- a = -0.003264
- b = 0.017022
- c = 0.749324

**y = -0.003264x² + 0.017022x + 0.749324**

Peak at x = -b/(2a) = 2.61 (close to observed peak at C3).
Fit quality: max residual 0.021 at C3, others < 0.01.

**Extrapolation to C11:**
y(11) = -0.003264(121) + 0.017022(11) + 0.749324
     = -0.39494 + 0.18724 + 0.74932
     = **0.542**

### Cross-check: Quadratic fit to last 3 points (C4, C5, C6)
y = -0.00505x² + 0.04345x + 0.6588
y(11) = -0.611 + 0.478 + 0.659 = **0.526**

### Cross-check: ln(QED) quadratic fit
ln(QED) = -0.00425x² + 0.02203x - 0.28812
ln(QED)(11) = -0.560 → QED = **0.571**

### Final Prediction
All three nonlinear models give 0.53–0.57. Best estimate: **target_qed ≈ 0.55**

Physical rationale: At C11, logP=4.96 (high penalty) and rotb=12 (penalty) both depress QED well below the measured range. The accelerating decline is consistent with Gaussian desirability penalties that grow quadratically with distance from optimal.

---

## Job 2: Lead Selection

**Constraints:**
1. measured QED ≥ 0.6 ✓
2. PSA ≥ 40 ✓
3. not in m05's series ✓

Series 4 excluded (PSA=29.1 < 40).
Series 2 all have QED < 0.34 — far below 0.6 (high PSA and negative logP hurt QED).
Series 3: only **m02** (C3, nitrophenyl) passes with QED = 0.6046.

**Lead: m02**
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- Measured QED: 0.6046
- PSA: 72.24 ≥ 40 ✓
- Not in m05's series (Series 3) ✓
