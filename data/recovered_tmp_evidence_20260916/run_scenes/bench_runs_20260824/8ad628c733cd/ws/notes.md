# Notes

## Catalogue Organization (33 molecules, 4 scaffolds)

### Scaffold 1: Methoxybenzamides — `COc1ccc(cc1)C(=O)N-` (congeneric series of m05)
| mol_id | chain | MW | ALogP | PSA | RotB | Arom | QED (measured) |
|--------|-------|---------|--------|-------|------|------|-----------------|
| m15 | 2C | 179.22 | 1.445 | 38.33 | 3 | 1 | 0.7621 |
| m24 | 3C | 193.25 | 1.835 | 38.33 | 4 | 1 | — |
| m28 | 4C | 207.27 | 2.225 | 38.33 | 5 | 1 | 0.7518 |
| m17 | 5C | 221.30 | 2.615 | 38.33 | 6 | 1 | — |
| m22 | 6C | 235.33 | 3.005 | 38.33 | 7 | 1 | 0.7377 |
| m05 | 11C | 305.46 | 4.956 | 38.33 | 12 | 1 | **unsynthesized** |

### Scaffold 2: Nitrobenzamides — `O=[N+]([O-])c1ccc(cc1)C(=O)N-`
m09(2C), m02(3C), m12(4C), m32(5C), m33(6C), m29(7C), m31(8C), m06(9C), m10(10C)
PSA = 72.24 for all.

### Scaffold 3: Sugar amides — `OCC(O)C(O)C(O)C(=O)N-`
m14(2C), m27(3C), m01(4C), m19(5C), m03(6C), m21(7C), m18(8C), m07(9C), m25(10C)
PSA = 110.02 for all.

### Scaffold 4: Biphenyl amides — `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`
m13(2C), m11(3C), m04(4C), m30(5C), m23(6C), m20(7C), m26(8C), m08(9C), m16(10C)
PSA = 29.10 for all.

## Budget usage
- 9 pages listed (all 33 molecules seen) = 9 units
- 8 QED measurements = 8 units
- Total: 17/17 exhausted

## Task 1: QED prediction for m05

### Measured methoxybenzamides
- m15 (2C): QED = 0.7621
- m28 (4C): QED = 0.7518
- m22 (6C): QED = 0.7377

### Trend analysis
QED decreases with chain length. Rate of decrease accelerates:
- 2C→4C: ΔQED = -0.0103 (rate = -0.00515/C)
- 4C→6C: ΔQED = -0.0141 (rate = -0.00705/C)

### Linear extrapolation
QED(nC) = 0.7749 - 0.0061 × nC
QED(11) = 0.7749 - 0.0671 = **0.708**

### Quadratic extrapolation (preferred — captures QED's nonlinear alogp penalty)
QED(nC) = 0.7686 - 0.0023 × nC - 0.000475 × nC²
QED(11) = 0.7686 - 0.0253 - 0.0575 = **0.686**

### Final prediction: ~0.69 (between linear and quadratic, slight rounding)

## Task 2: Lead molecule selection

### Constraints
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in methoxybenzamide series (same scaffold as m05)

### Candidates with psa ≥ 40 and NOT in m05's series
- Nitrobenzamides (psa=72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Sugar amides (psa=110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25

### QED measurements of candidates
- m09 (nitro, 2C): QED = 0.5826 — highest among non-methoxybenzamide with psa ≥ 40
- m12 (nitro, 4C): QED = 0.4709
- m32 (nitro, 5C): QED = 0.4683
- m27 (sugar, 3C): QED = 0.3330
- m01 (sugar, 4C): QED = 0.3187

### Assessment
No measured candidate reaches QED ≥ 0.6 among molecules with psa ≥ 40 and not in m05's series.
- Methoxybenzamides have QED ~0.74-0.76 but psa=38.33 < 40 (fail constraint 2)
- Biphenyl amides have psa=29.10 < 40 (fail constraint 2)
- Nitrobenzamides peak at QED=0.5826 (m09) — below 0.6
- Sugar amides peak at QED ~0.33 — well below 0.6

Best available: **m09** with measured QED = 0.5826, psa = 72.24, not in m05's series.

## Final Answers
- Task 1 (target_qed): 0.69 (predicted from congeneric series extrapolation)
- Task 2 (lead_mol_id): m09
- Task 2 (lead_qed_measured): 0.5826
