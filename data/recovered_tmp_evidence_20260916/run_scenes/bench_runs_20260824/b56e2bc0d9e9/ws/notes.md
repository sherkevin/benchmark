# Notes

## Task Overview
- Predict QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC, 11-carbon N-alkyl chain)
- Pick one lead molecule with QED ≥ 0.6, PSA ≥ 40, not in m05's congeneric series

## Congeneric Series Identification

### Series A — Scaffold: COc1ccc(cc1)C(=O)N- (m05's series)
| mol_id | chain | mw | alogp | psa | rotb | arom |
|--------|-------|--------|-------|-------|------|------|
| m15 | 2C | 179.22 | 1.445 | 38.33 | 3 | 1 |
| m24 | 3C | 193.25 | 1.835 | 38.33 | 4 | 1 |
| m28 | 4C | 207.27 | 2.225 | 38.33 | 5 | 1 |
| m17 | 5C | 221.30 | 2.615 | 38.33 | 6 | 1 |
| m22 | 6C | 235.33 | 3.005 | 38.33 | 7 | 1 |
| m05 | 11C | 305.46 | 4.956 | 38.33 | 12 | 1 |

### Series B — Scaffold: OCC(O)C(O)C(O)C(=O)N- (PSA=110.02)
### Series C — Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N- (PSA=72.24)
### Series D — Scaffold: c1ccc(cc1)-c1ccc(cc1)C(=O)N- (PSA=29.1)

## Measurements (12/12 used)

### Series A (5 measurements):
- m15 (2C): QED = 0.7621
- m24 (3C): QED = 0.7919
- m28 (4C): QED = 0.7518
- m17 (5C): QED = 0.7498
- m22 (6C): QED = 0.7377

### Series C (5 measurements):
- m09 (2C): QED = 0.5826
- m02 (3C): QED = 0.6046
- m12 (4C): QED = 0.4709
- m32 (5C): QED = 0.4683
- m33 (6C): QED = 0.4591

### Series B (2 measurements):
- m14 (2C): QED = 0.3237
- m27 (3C): QED = 0.333

## Task 1: Predicting m05 QED

### Observed Trend (Series A)
Chain length vs QED shows a gradual decline:
- 2C: 0.7621
- 3C: 0.7919 (slight bump — possibly anomalous)
- 4C: 0.7518
- 5C: 0.7498
- 6C: 0.7377

Step-by-step changes:
- 2→3: +0.030
- 3→4: −0.040
- 4→5: −0.002
- 5→6: −0.012

### Extrapolation Methods

**Linear regression (all 5 points):**
- slope = −0.00909 per carbon
- intercept = 0.795
- QED(11) = 0.795 − 0.00909 × 11 = **0.695**

**Linear regression (excluding 3C outlier):**
- slope = −0.00564 per carbon
- QED(11) ≈ **0.712**

**Rate from 4C–6C:**
- Rate: −0.00705 per carbon
- QED(11) = 0.7377 − 5 × 0.00705 = **0.702**

**Quadratic fit (excluding 3C):**
- Accounts for nonlinear QED penalties at high alogP (4.96) and rotb (12)
- QED(11) ≈ **0.670**

### Final Prediction
Linear regression on all 5 points gives 0.695. Accounting for expected nonlinear
penalties at alogP=4.956 and rotb=12 (both entering steep penalty zones of QED
desirability functions), I adjust slightly downward.

**Predicted QED for m05: 0.69**

## Task 2: Lead Selection

### Constraint Checklist
1. QED ≥ 0.6
2. PSA ≥ 40
3. Not in Series A

### PSA Screening
- Series A: PSA = 38.33 → FAILS (< 40)
- Series B: PSA = 110.02 → passes
- Series C: PSA = 72.24 → passes
- Series D: PSA = 29.1 → FAILS (< 40)

### QED Screening (candidates from Series B and C)

**Series B** (PSA=110): QED too low due to very high PSA
- m14: QED = 0.3237 → FAILS
- m27: QED = 0.333 → FAILS

**Series C** (PSA=72):
- m09: QED = 0.5826 → FAILS (< 0.6)
- m02: QED = 0.6046 → PASSES ALL CONSTRAINTS ✓
- m12: QED = 0.4709 → FAILS
- m32: QED = 0.4683 → FAILS
- m33: QED = 0.4591 → FAILS

### Selected Lead: **m02**
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- Measured QED: 0.6046
- PSA: 72.24 ≥ 40 ✓
- Series C (different scaffold from m05) ✓
- QED 0.6046 ≥ 0.6 ✓
