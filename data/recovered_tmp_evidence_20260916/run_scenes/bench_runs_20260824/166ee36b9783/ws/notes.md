# Medicinal Chemistry Analysis Notes

## Budget Usage
- Pages read: 7 (pages 1-7 of 9), cost = 7
- Measurements made: 5 (m15, m28, m17, m22, m09), cost = 5
- Total: 12/12 budget used

## Congeneric Series Identified

### m05 series (methoxybenzamide: COc1ccc(cc1)C(=O)N-)
| mol_id | N-alkyl | alogp | MW | QED (measured) |
|--------|---------|-------|------|----------------|
| m15 | CC (2C) | 1.445 | 179.22 | 0.7621 |
| m24 | CCC (3C) | 1.835 | 193.25 | - |
| m28 | CCCC (4C) | 2.225 | 207.27 | 0.7518 |
| m17 | CCCCC (5C) | 2.615 | 221.30 | 0.7498 |
| m22 | CCCCCC (6C) | 3.005 | 235.33 | 0.7377 |
| m05 | CCCCCCCCCCC (11C) | 4.956 | 305.46 | probe_unavailable |

### Other series
- Nitrobenzamide: m09, m02, m12, m06, m10 (psa=72.24)
- Sugar-derived: m14, m27, m01, m19, m03, m21, m18, m07, m25 (psa=110.02)
- Biphenyl: m13, m11, m04, m23, m20, m26, m08, m16 (psa=29.1)

## Task 1: Predict m05 QED

Linear regression (chain length vs QED):
- slope = -0.00564, intercept = 0.77432
- At 11C: 0.77432 - 0.00564*11 = 0.712

Quadratic regression (chain length vs QED):
- y = 0.76327 + 0.000977x - 0.000843x^2
- At 11C: 0.672

The QED decreases with chain length and the rate accelerates due to compounding logP/rotb penalties. Best estimate averages linear and quadratic: ~0.69

## Task 2: Lead Candidate

Constraints: measured QED >= 0.6, psa >= 40, not in m05 series.

Only non-m05 measurement: m09 (nitro series, psa=72.24) -> QED=0.5826.
Budget exhausted before measuring sugar-series candidates.

Best available: m09 with measured QED = 0.5826
