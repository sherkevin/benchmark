# Notes

## Overview
- 33 molecules in 4 congeneric series (scaffold + N-alkyl chain length variation)
- Budget: 17 units total (9 pages listed + 8 measurements = 17 used)

## Congeneric Series Identified

### 1. Methoxybenzamide: COc1ccc(cc1)C(=O)N- (m05's series)
| mol_id | chain | MW | logP | psa | rotb | QED (measured) |
|--------|-------|--------|-------|-------|------|----------------|
| m15 | 2C | 179.22 | 1.445 | 38.33 | 3 | 0.7621 |
| m24 | 3C | 193.25 | 1.835 | 38.33 | 4 | 0.7919 |
| m28 | 4C | 207.27 | 2.225 | 38.33 | 5 | — |
| m17 | 5C | 221.30 | 2.615 | 38.33 | 6 | 0.7498 |
| m22 | 6C | 235.33 | 3.005 | 38.33 | 7 | 0.7377 |
| **m05** | **11C** | **305.46** | **4.956** | **38.33** | **12** | **? (target)** |

### 2. Nitrobenzamide: O=[N+]([O-])c1ccc(cc1)C(=O)N-
| mol_id | chain | QED (measured) | psa |
|--------|-------|----------------|-------|
| m09 | 2C | 0.5826 | 72.24 |
| m02 | 3C | 0.6046 | 72.24 |
| m12 | 4C | 0.4709 | 72.24 |
| m32 | 5C | 0.4683 | 72.24 |
| m33 | 6C | — | 72.24 |
| m29 | 7C | — | 72.24 |
| m31 | 8C | — | 72.24 |
| m06 | 9C | — | 72.24 |
| m10 | 10C | — | 72.24 |

### 3. Biphenyl amide: c1ccc(cc1)-c1ccc(cc1)C(=O)N-
All have psa=29.1 (< 40), so excluded from Task 2.

### 4. Gluconamide: OCC(O)C(O)C(O)C(=O)N-
All have psa=110.02 (≥ 40), but very negative logP; not measured due to budget.

## Task 1: Predict QED of m05

Linear regression on chain length vs QED for methoxybenzamide (4 measured points):
- Data: (2, 0.7621), (3, 0.7919), (5, 0.7498), (6, 0.7377)
- x̄ = 4, ȳ = 0.760375
- slope = Σ(xi-x̄)(yi-ȳ) / Σ(xi-x̄)² = -0.0909 / 10 = -0.00909
- intercept = ȳ - slope·x̄ = 0.760375 + 0.03636 = 0.79674
- Prediction at chain=11: 0.79674 - 0.00909×11 = 0.79674 - 0.100 = **0.697**

Cross-check with logP-based regression:
- Data (logP, QED): (1.445, 0.7621), (1.835, 0.7919), (2.615, 0.7498), (3.005, 0.7377)
- Predicted QED at logP=4.956: ~0.697 (consistent)

**Prediction: m05 QED ≈ 0.70**

## Task 2: Lead Selection

Constraints: QED ≥ 0.6, psa ≥ 40, not in m05's series.

Candidates with psa ≥ 40, not in methoxybenzamide series:
- Nitrobenzamide (psa=72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Gluconamide (psa=110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25

Measured results:
- m09: QED=0.5826 → FAILS (QED < 0.6)
- **m02: QED=0.6046 → PASSES** (QED ≥ 0.6 ✓, psa=72.24 ≥ 40 ✓, nitrobenzamide ≠ methoxybenzamide ✓)
- m12: QED=0.4709 → FAILS (QED < 0.6)
- m32: QED=0.4683 → FAILS (QED < 0.6)

**Selected lead: m02** (SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC)
- Measured QED: 0.6046
- PSA: 72.24
- Series: nitrobenzamide (different from m05's methoxybenzamide)
