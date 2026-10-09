# Notes

## Budget Usage
- Pages listed: 9 (budget 1-9)
- Measurements: 3 (budget 10-12)
- Total: 12/12

## Congeneric Series Identification

### m05's Series (scaffold: COc1ccc(cc1)C(=O)N-, methoxybenzene amide)
| mol_id | N-alkyl | #C | MW | ALogP | PSA | RotB | Arom |
|--------|---------|----|------|-------|------|------|------|
| m15 | NCC | 2 | 179.22 | 1.445 | 38.33 | 3 | 1 |
| m24 | NCCC | 3 | 193.25 | 1.835 | 38.33 | 4 | 1 |
| m28 | NCCCC | 4 | 207.27 | 2.225 | 38.33 | 5 | 1 |
| m17 | NCCCCC | 5 | 221.30 | 2.615 | 38.33 | 6 | 1 |
| m22 | NCCCCCC | 6 | 235.33 | 3.005 | 38.33 | 7 | 1 |
| m05 | NCCCCCCCCCCC | 11 | 305.46 | 4.956 | 38.33 | 12 | 1 |

### Other Series
1. **Nitro series** (O=[N+]([O-])c1ccc(cc1)C(=O)N-): m09, m02, m12, m32, m33, m29, m31, m06, m10 — PSA=72.24
2. **Sugar series** (OCC(O)C(O)C(O)C(=O)N-): m14, m27, m01, m19, m03, m21, m18, m07, m25 — PSA=110.02
3. **Biphenyl series** (c1ccc(cc1)-c1ccc(cc1)C(=O)N-): m13, m11, m04, m30, m23, m20, m26, m08, m16 — PSA=29.1

## Task 1: Predict m05 QED

### Measured QEDs in m05's series:
- m15 (2C): QED = 0.7621
- m22 (6C): QED = 0.7377

### Linear extrapolation:
- Slope = (0.7377 - 0.7621) / (6 - 2) = -0.0061 per CH2
- m05 (11C): QED = 0.7621 + (11 - 2) * (-0.0061) = 0.7621 - 0.0549 = **0.7072**

### Sanity check:
- ALogP increases linearly (~0.39 per CH2), driving QED down as lipophilicity grows
- PSA, aromatic rings constant across series
- RotB increases, further penalizing longer chains
- Linear extrapolation is the best available model with 2 data points

## Task 2: Lead Candidate

### Constraints: QED >= 0.6, PSA >= 40, not in m05's series

### Candidates measured:
- m09 (nitro series, NCC): QED = 0.5826, PSA = 72.24, not in m05 series
  - QED slightly below 0.6 threshold

### Analysis:
- Biphenyl series (PSA=29.1) and m05 series (PSA=38.33) both fail PSA >= 40
- Nitro and sugar series have PSA >= 40
- m09 was the only non-m05-series molecule measured; QED=0.5826
- Budget exhausted before additional candidates could be measured
- m09 is the best available candidate from measured data

### Decision:
Report m09 as lead (closest to satisfying all constraints among measured molecules).
