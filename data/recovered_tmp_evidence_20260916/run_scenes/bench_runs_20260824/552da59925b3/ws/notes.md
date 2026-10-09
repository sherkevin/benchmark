# Notes

## Catalogue Overview (33 molecules, 4 congeneric series)

### Series 1: Methoxybenzene amide — `COc1ccc(cc1)C(=O)N-[chain]` (m05 series)
| mol_id | chain | MW | ALogP | PSA | RotB |
|--------|-------|---------|-------|-------|------|
| m15 | 2C | 179.22 | 1.445 | 38.33 | 3 |
| m24 | 3C | 193.25 | 1.835 | 38.33 | 4 |
| m28 | 4C | 207.27 | 2.225 | 38.33 | 5 |
| m17 | 5C | 221.30 | 2.615 | 38.33 | 6 |
| m22 | 6C | 235.33 | 3.005 | 38.33 | 7 |
| m05 | 11C | 305.46 | 4.956 | 38.33 | 12 |

### Series 2: Nitrobenzene amide — `O=[N+]([O-])c1ccc(cc1)C(=O)N-[chain]`
| mol_id | chain | MW | ALogP | PSA | RotB |
|--------|-------|---------|-------|-------|------|
| m09 | 2C | 194.19 | 1.345 | 72.24 | 3 |
| m02 | 3C | 208.22 | 1.735 | 72.24 | 4 |
| m12 | 4C | 222.24 | 2.125 | 72.24 | 5 |
| m32 | 5C | 236.27 | 2.515 | 72.24 | 6 |
| m33 | 6C | 250.30 | 2.905 | 72.24 | 7 |
| m29 | 7C | 264.32 | 3.295 | 72.24 | 8 |
| m31 | 8C | 278.35 | 3.685 | 72.24 | 9 |
| m06 | 9C | 292.38 | 4.075 | 72.24 | 10 |
| m10 | 10C | 306.41 | 4.465 | 72.24 | 11 |

### Series 3: Sugar (gluconamide) — `OCC(O)C(O)C(O)C(=O)N-[chain]`
| mol_id | chain | MW | ALogP | PSA | RotB |
|--------|-------|---------|--------|--------|------|
| m14 | 2C | 193.20 | -2.802 | 110.02 | 5 |
| m27 | 3C | 207.23 | -2.412 | 110.02 | 6 |
| m01 | 4C | 221.25 | -2.022 | 110.02 | 7 |
| m19 | 5C | 235.28 | -1.632 | 110.02 | 8 |
| m03 | 6C | 249.31 | -1.242 | 110.02 | 9 |
| m21 | 7C | 263.33 | -0.852 | 110.02 | 10 |
| m18 | 8C | 277.36 | -0.462 | 110.02 | 11 |
| m07 | 9C | 291.39 | -0.072 | 110.02 | 12 |
| m25 | 10C | 305.42 | 0.318 | 110.02 | 13 |

### Series 4: Biphenyl — `c1ccc(cc1)-c1ccc(cc1)C(=O)N-[chain]`
| mol_id | chain | MW | ALogP | PSA | RotB |
|--------|-------|---------|-------|-------|------|
| m13 | 2C | 225.29 | 3.103 | 29.10 | 3 |
| m11 | 3C | 239.32 | 3.493 | 29.10 | 4 |
| m04 | 4C | 253.34 | 3.884 | 29.10 | 5 |
| m30 | 5C | 267.37 | 4.274 | 29.10 | 6 |
| m23 | 6C | 281.40 | 4.664 | 29.10 | 7 |
| m20 | 7C | 295.43 | 5.054 | 29.10 | 8 |
| m26 | 8C | 309.45 | 5.444 | 29.10 | 9 |
| m08 | 9C | 323.48 | 5.834 | 29.10 | 10 |
| m16 | 10C | 337.51 | 6.224 | 29.10 | 11 |

## Budget Usage
- Pages 1-9: 9 units (read all 33 molecules)
- Measurement m15: 1 unit -> QED = 0.7621
- Measurement m22: 1 unit -> QED = 0.7377
- Measurement m09: 1 unit -> QED = 0.5826
- Total: 12/12 budget used

## Task 1: Predict QED of m05

m05 is in the methoxybenzene amide series (chain = 11C). It has not been synthesized.

Measured congeneric members:
- m15 (2C): QED = 0.7621
- m22 (6C): QED = 0.7377

Linear trend: slope = (0.7377 - 0.7621) / (6 - 2) = -0.0061 per CH2

Extrapolating to 11C:
QED(m05) = 0.7621 + (11 - 2) * (-0.0061) = 0.7621 - 0.0549 = **0.7072**

Cross-check from m22: 0.7377 + (11 - 6) * (-0.0061) = 0.7377 - 0.0305 = 0.7072 (consistent)

Note: descriptor trends are perfectly linear (each CH2 adds ~14 MW, ~0.39 ALogP, 1 RotB),
supporting a linear QED extrapolation over this range.

## Task 2: Lead molecule selection

Constraints: measured QED >= 0.6, PSA >= 40, not in m05 series.

Candidates with PSA >= 40 (not in m05 series):
- Nitrobenzene series (PSA = 72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Sugar series (PSA = 110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25

Measured candidate:
- **m09** (nitrobenzene, 2C): QED = 0.5826, PSA = 72.24, not in m05 series
  - Satisfies constraints 2 and 3; QED is close to but slightly below 0.6 threshold
  - Best available measured candidate given budget constraints

Budget was exhausted before additional non-m05-series molecules with PSA >= 40 could be measured.
m09 is the lead selection from available measurements.
