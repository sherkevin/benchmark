# Notes

## Task Overview
- Budget: 17 units (shared between list_pool and measure_qed)
- 33 molecules in pool, 4 per page → 9 pages to catalog all

## Catalogue Summary
Four congeneric series identified (same scaffold, varying N-alkyl chain):

### 1. Methoxyphenyl amides (COc1ccc(cc1)C(=O)N-) — PSA 38.33, Arom 1
- m15 (2C), m24 (3C), m28 (4C), m17 (5C), m22 (6C), m05 (11C)

### 2. Nitrophenyl amides (O=[N+]([O-])c1ccc(cc1)C(=O)N-) — PSA 72.24, Arom 1
- m09 (2C), m02 (3C), m12 (4C), m32 (5C), m33 (6C), m29 (7C), m31 (8C), m06 (9C), m10 (10C)

### 3. Sugar amides (OCC(O)C(O)C(O)C(=O)N-) — PSA 110.02, Arom 0
- m14 (2C), m27 (3C), m01 (4C), m19 (5C), m03 (6C), m21 (7C), m18 (8C), m07 (9C), m25 (10C)

### 4. Biphenyl amides (c1ccc(cc1)-c1ccc(cc1)C(=O)N-) — PSA 29.1, Arom 2
- m13 (2C), m11 (3C), m04 (4C), m30 (5C), m23 (6C), m20 (7C), m26 (8C), m08 (9C), m16 (10C)

Plus m33 (nitrophenyl, 6C).

## Budget Usage
- Pages 1-9: 9 units
- Measurements: 8 units (m15, m24, m28, m17, m22, m09, m02, m12)
- Total: 17/17

## Task 1: Predict QED of m05

m05 (11C methoxyphenyl) has never been synthesized → measure_qed returns probe_unavailable.
Must predict from congeneric series trend.

### Measured QED values for methoxyphenyl series:
| Chain | mol_id | MW | ALogP | RotB | QED |
|-------|--------|------|-------|------|-------|
| 2C | m15 | 179.22 | 1.445 | 3 | 0.7621 |
| 3C | m24 | 193.25 | 1.835 | 4 | 0.7919 |
| 4C | m28 | 207.27 | 2.225 | 5 | 0.7518 |
| 5C | m17 | 221.30 | 2.615 | 6 | 0.7498 |
| 6C | m22 | 235.33 | 3.005 | 7 | 0.7377 |
| 11C | m05 | 305.46 | 4.956 | 12 | ? |

### Trend analysis:
- QED peaks at 3C (0.7919) then decreases
- Decreases per CH2: 3→4: -0.040, 4→5: -0.002, 5→6: -0.012
- The decrease accelerates as ALogP approaches 5 and RotB increases
- m05 has ALogP=4.956 (near Lipinski limit of 5) and RotB=12 (well above typical drug range)

### Extrapolation methods:
1. Linear fit (all 5 points): QED(11) ≈ 0.695
2. Quadratic fit (last 3 points: 4C-6C): QED(n) = -0.00505*n² + 0.04345*n + 0.6588 → QED(11) ≈ 0.526
3. Physics-based model (ADS product with calibrated K): QED(11) ≈ 0.52-0.58

The quadratic model is theoretically justified because:
- QED decrease should accelerate with increasing ALogP (quadratic penalty in ADS function)
- RotB of 12 is well above optimal range, incurring heavy penalty
- The constant second derivative model matches the observed acceleration from 4C→5C→6C

**Predicted QED for m05: 0.55**

## Task 2: Select lead molecule

### Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05's congeneric series (not methoxyphenyl scaffold)

### Candidates with PSA ≥ 40 (not methoxyphenyl):
- Nitrophenyl series (PSA 72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Sugar amide series (PSA 110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25
- (Biphenyl series all have PSA 29.1 < 40, excluded)

### Measured candidates:
- m09 (nitrophenyl, 2C): QED 0.5826 — FAILS (QED < 0.6)
- m02 (nitrophenyl, 3C): QED 0.6046, PSA 72.24 — **PASSES ALL CONSTRAINTS**
- m12 (nitrophenyl, 4C): QED 0.4709 — FAILS (QED < 0.6)

### Selected: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- Measured QED: 0.6046 (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Scaffold: nitrophenyl amide (different from m05's methoxyphenyl ✓)
