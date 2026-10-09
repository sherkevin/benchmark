# Notes

## Task Overview
- 33 compounds in pool across 9 pages (4 per page)
- Budget: 17 units shared between listing pages and QED measurements
- Used: 9 pages + 8 measurements = 17 total

## Compound Series Identified

### Series 1: Methoxybenzamides `COc1ccc(cc1)C(=O)N-` (m05's series)
| mol_id | Chain | MW | alogp | PSA | rotb | QED |
|--------|-------|---------|-------|-------|------|-------|
| m15 | 2C | 179.22 | 1.445 | 38.33 | 3 | 0.7621 |
| m24 | 3C | 193.25 | 1.835 | 38.33 | 4 | 0.7919 |
| m28 | 4C | 207.27 | 2.225 | 38.33 | 5 | 0.7518 |
| m17 | 5C | 221.30 | 2.615 | 38.33 | 6 | 0.7498 |
| m22 | 6C | 235.33 | 3.005 | 38.33 | 7 | 0.7377 |
| m05 | 11C | 305.46 | 4.956 | 38.33 | 12 | ??? |

### Series 2: Nitrobenzamides `O=[N+]([O-])c1ccc(cc1)C(=O)N-`
9 compounds (m09, m02, m12, m32, m33, m29, m31, m06, m10), chains 2C-10C, PSA=72.24

### Series 3: Sugar-like `OCC(O)C(O)C(O)C(=O)N-`
9 compounds (m14, m27, m01, m19, m03, m21, m18, m07, m25), chains 2C-10C, PSA=110.02

### Series 4: Biphenyls `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`
9 compounds, chains 2C-10C, PSA=29.1 (all fail PSA≥40 constraint)

## Task 1: Predict m05 QED

Linear regression on 5 data points (chain length vs QED):
- slope = -0.00909 per CH2
- intercept = 0.79502
- QED(11) = 0.79502 - 0.00909 × 11 = **0.695**

Cross-checks:
- Log-linear model: 0.698
- Sqrt model: 0.713
- Quadratic model: 0.542 (likely overfitting with only 5 points)
- All "moderate" models converge around 0.70

Final prediction: **0.695**

## Task 2: Find lead compound

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's series.

Candidates with PSA ≥ 40 (not m05 series):
- Nitrobenzamides (PSA=72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Sugar-like (PSA=110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25
- Biphenyls (PSA=29.1): ALL FAIL PSA constraint

Measured results:
- m09 (nitro, 2C): QED = 0.5826 (closest to 0.6, but below threshold)
- m14 (sugar, 2C): QED = 0.3237 (very low, due to negative logP and many HBD/HBA)
- m01 (sugar, 4C): QED = 0.3187 (similar issues)

Sugar-like compounds consistently have low QED (~0.32) due to very negative logP and many polar groups.

Nitrobenzamides have moderate QED (~0.58) limited by the nitro structural alert.

Best available candidate: **m09** with measured QED = 0.5826 and PSA = 72.24.
Note: This falls short of the 0.6 QED threshold, but it was the best measurable candidate within the 17-unit budget constraint.

## Budget Usage
- Pages: 9 (pages 1-9 to view all 33 compounds)
- Measurements: 8 (m15, m24, m28, m17, m22, m09, m14, m01)
- Free calls: m05 (probe_unavailable), m02 (budget_exhausted)
- Total: 17/17
