# Notes

## Catalog Exploration (9 pages listed, 9 budget units used)

Listed all 33 molecules across 9 pages. Identified 4 congeneric series:

### Series 1: Methoxyphenyl amides (scaffold: COc1ccc(cc1)C(=O)N-)
| mol_id | chain | MW | ALOGP | PSA | ROTB | AROM |
|--------|-------|-------|-------|-------|------|------|
| m15 | 2C | 179.22 | 1.445 | 38.33 | 3 | 1 |
| m24 | 3C | 193.25 | 1.835 | 38.33 | 4 | 1 |
| m28 | 4C | 207.27 | 2.225 | 38.33 | 5 | 1 |
| m17 | 5C | 221.30 | 2.615 | 38.33 | 6 | 1 |
| m22 | 6C | 235.33 | 3.005 | 38.33 | 7 | 1 |
| m05 | 11C | 305.46 | 4.956 | 38.33 | 12 | 1 |

### Series 2: Nitrophenyl amides (scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N-)
m09(2C), m02(3C), m12(4C), m32(5C), m33(6C), m29(7C), m31(8C), m06(9C), m10(10C)
PSA = 72.24 for all

### Series 3: Biphenyl amides (scaffold: c1ccc(cc1)-c1ccc(cc1)C(=O)N-)
m13(2C), m11(3C), m04(4C), m30(5C), m23(6C), m20(7C), m26(8C), m08(9C), m16(10C)
PSA = 29.1 for all

### Series 4: Sugar amides (scaffold: OCC(O)C(O)C(O)C(=O)N-)
m14(2C), m27(3C), m01(4C), m19(5C), m03(6C), m21(7C), m18(8C), m07(9C), m25(10C)
PSA = 110.02 for all

## Task 1: Predict QED of m05

### Measurements (3 budget units used)
- m15 (2C methoxyphenyl): QED = 0.7621
- m22 (6C methoxyphenyl): QED = 0.7377
- m09 (2C nitrophenyl): QED = 0.5826

### Trend Analysis
Linear slope: (0.7377 - 0.7621) / (6 - 2) = -0.0061 per carbon
Linear extrapolation to 11C: 0.7621 - 0.0061 * 9 = 0.7072

m05 has alogp=4.956 (near threshold of 5.0) and rotb=12 (above threshold of 10).
These values are further from optimal than m15/m22, so QED should drop somewhat
faster for the 6C-11C segment than for 2C-6C.

Applied moderate curvature correction: predicted QED(m05) ≈ 0.68

## Task 2: Lead Molecule Selection

### Constraints
1. Measured QED >= 0.6
2. PSA >= 40
3. Not in m05's congeneric series

### Candidate Analysis
- Methoxyphenyl: excluded (same series as m05, PSA=38.33 < 40)
- Biphenyl: excluded (PSA=29.1 < 40)
- Nitrophenyl: PSA=72.24 OK, not same series OK
- Sugar amides: PSA=110.02 OK, not same series OK

### Best Available
- m09 (nitrophenyl, 2C): measured QED = 0.5826
- Budget exhausted before additional candidates could be measured

## Budget Summary
- Pages listed: 9 (cost: 9)
- QED measurements: 3 successful (cost: 3)
- Total: 12/12 budget used
- Additional attempts (m02, m01, m12): blocked by exhausted budget
- m05 probe_unavailable: cost 0 (not synthesized)
