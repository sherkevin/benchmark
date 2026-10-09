# Notes

## Pool Overview (33 molecules, 4 congeneric series)

All 33 molecules are amides of the form: scaffold-C(=O)N-alkyl, where only the N-alkyl chain length varies within each series.

### Series 1 (m05 series): COc1ccc(cc1)C(=O)N- (methoxyphenyl amide)
| mol_id | chain_C | MW | ALogP | PSA | rotb | arom | QED |
|--------|---------|--------|-------|-------|------|------|--------|
| m15 | 2 | 179.22 | 1.445 | 38.33 | 3 | 1 | 0.7621 (measured) |
| m24 | 3 | 193.25 | 1.835 | 38.33 | 4 | 1 | — |
| m28 | 4 | 207.27 | 2.225 | 38.33 | 5 | 1 | — |
| m17 | 5 | 221.30 | 2.615 | 38.33 | 6 | 1 | — |
| m22 | 6 | 235.33 | 3.005 | 38.33 | 7 | 1 | 0.7377 (measured) |
| m05 | 11 | 305.46 | 4.956 | 38.33 | 12 | 1 | probe_unavailable |

### Series 2: O=[N+]([O-])c1ccc(cc1)C(=O)N- (nitrophenyl amide), PSA=72.24
Members: m09(2C), m02(3C), m12(4C, QED=0.4709), m32(5C), m33(6C), m29(7C), m31(8C), m06(9C), m10(10C)

### Series 3: OCC(O)C(O)C(O)C(=O)N- (sugar acid amide), PSA=110.02, arom=0
Members: m14(2C), m27(3C), m01(4C), m19(5C), m03(6C), m21(7C), m18(8C), m07(9C), m25(10C)

### Series 4: c1ccc(cc1)-c1ccc(cc1)C(=O)N- (biphenyl amide), PSA=29.10
Members: m13(2C), m11(3C), m04(4C), m30(5C), m23(6C), m20(7C), m26(8C), m08(9C), m16(10C)

## Budget Usage
- 9 pages listed (pages 1-9) = 9 units
- 3 measurements (m15, m22, m12) = 3 units
- Total: 12/12 budget used
- Attempted m09 measurement: budget exhausted

## Task 1: Predict m05 QED
Using linear extrapolation from the m05 congeneric series:
- m15 (chain C=2): QED = 0.7621
- m22 (chain C=6): QED = 0.7377
- Slope = (0.7377 - 0.7621) / (6 - 2) = -0.0244 / 4 = -0.0061 per CH2
- m05 (chain C=11): predicted QED = 0.7621 + (-0.0061)(11-2) = 0.7621 - 0.0549 = 0.7072

Note: The actual QED may be slightly lower because QED desirability functions are nonlinear — ALogP=4.956 and rotb=12 for m05 are in ranges where desirability drops faster. But with only 2 data points, linear extrapolation is the best available approach.

## Task 2: Lead Molecule Selection
Constraints: measured QED ≥ 0.6, PSA ≥ 40, not in m05 series.

PSA ≥ 40 eliminates Series 1 (PSA=38.33) and Series 4 (PSA=29.10).
Only Series 2 (PSA=72.24) and Series 3 (PSA=110.02) remain.

Only one non-Series-1 molecule was measured: m12 (Series 2, 4C chain).
- m12: QED = 0.4709, PSA = 72.24, not in m05 series
- Satisfies constraints 2 and 3 but measured QED (0.4709) < 0.6

Budget was exhausted before additional measurements could be made. m12 is the best available candidate with a real measured QED.
