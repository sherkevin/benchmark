# Notes for QED Prediction and Lead Selection

## Series Identification

### m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
Scaffold: `COc1ccc(cc1)C(=O)N-` (methoxybenzamide) with 11-carbon N-alkyl chain

### Series 1 (m05's series - methoxybenzamide):
- m15: NCC (2C)
- m24: NCCC (3C)
- m28: NCCCC (4C)
- m17: NCCCCC (5C)
- m22: NCCCCCC (6C)
- m05: NCCCCCCCCCCC (11C) — TARGET, never synthesized

### Series 2 (sugar-like): m14, m27, m01, m19, m03, m21, m18, m07, m25 — PSA=110.02
### Series 3 (nitrobenzene): m09, m02, m12, m32, m33, m29, m31, m06, m10 — PSA=72.24
### Series 4 (biphenyl): m13, m11, m04, m30, m23, m20, m26, m08, m16 — PSA=29.1 (FAIL psa≥40)

## Job 2 Candidates: Series 2 or Series 3 only

## Measurements

### m05 series (Job 1):
- m15 (2C): QED = 0.7621
- m24 (3C): QED = 0.7919
- m28 (4C): QED = 0.7518
- m17 (5C): QED = 0.7498
- m22 (6C): QED = 0.7377

### Job 2 candidates:
- m19 (Series 2, 5C): QED = 0.3244
- m03 (Series 2, 6C): QED = 0.3282
- m21 (Series 2, 7C): QED = 0.3297
- m32 (Series 3, 5C): QED = 0.4683
- m33 (Series 3, 6C): QED = 0.4591
- m29 (Series 3, 7C): QED = 0.4446
- m09 (Series 3, 2C): QED = 0.5826

## Analysis

### Job 1: QED Prediction for m05

**Observed trend in the m05 (methoxybenzamide) series:**
| Chain length | mol_id | QED    | alogp |
|--------------|--------|--------|-------|
| 2C           | m15    | 0.7621 | 1.445 |
| 3C           | m24    | 0.7919 | 1.835 |
| 4C           | m28    | 0.7518 | 2.225 |
| 5C           | m17    | 0.7498 | 2.615 |
| 6C           | m22    | 0.7377 | 3.005 |
| 11C (target) | m05    | ?      | 4.956 |

The QED peaks at 3C (alogp ≈ 1.8), then declines. The decline accelerates:
- 3C→4C: -0.0401
- 4C→5C: -0.0020
- 5C→6C: -0.0121

**Model: Quadratic fit to the 3 most recent data points (4C, 5C, 6C)**

Setting x' = chain_length - 4:
- x'=0 (4C): y = 0.7518
- x'=1 (5C): y = 0.7498
- x'=2 (6C): y = 0.7377

Solving: a = -0.00505, b = 0.00305, c = 0.7518
→ y = -0.00505x'² + 0.00305x' + 0.7518

At m05 (11C, x'=7):
y = -0.00505(49) + 0.00305(7) + 0.7518 = -0.24745 + 0.02135 + 0.7518 = **0.526**

**Cross-check with AlogP-based quadratic:**
Fitting QED vs AlogP for the same 3 points and evaluating at alogp=4.956 gives QED ≈ 0.526 — consistent result.

**Rationale:** The quadratic captures the accelerating decline in QED as the N-alkyl chain lengthens (and alogp rises well above the optimal ~1-3 range). The m05 molecule has alogp = 4.956, far above the optimal range, justifying a substantial QED penalty.

### Job 2: Lead Selection

**Pre-filtering (free checks):**
- Series 4 (biphenyl): PSA = 29.1 < 40 → all excluded
- Series 1 (m05 series): same series as m05 → all excluded
- Remaining candidates: Series 2 (PSA=110.02) and Series 3 (PSA=72.24)

**Measurements of candidates:**
- Series 2: QEDs ~0.32-0.33 (very low, likely due to many OH groups and negative alogp)
- Series 3 (longer chains): QEDs ~0.44-0.47 (moderate)
- Series 3 (shortest chain, m09): QED = 0.5826 (highest outside m05 series)

**Best available lead: m09**
- Measured QED: 0.5826 (below 0.6 threshold, but highest available)
- PSA: 72.24 ≥ 40 ✓
- Not in m05 series ✓
- Budget exhausted (12/12 measurements used)

**Limitation:** No measured molecule outside the m05 series achieved QED ≥ 0.6 within the 12-measurement budget. m09 is the closest candidate.
