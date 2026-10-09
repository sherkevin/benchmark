# Notes: QED Prediction and Lead Selection

## Job 1: Predict QED of m05

### Identifying the Congeneric Series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — scaffold is **methoxyphenyl amide** (`COc1ccc(cc1)C(=O)N–`), with an 11-carbon N-alkyl chain.

Pool molecules sharing this scaffold (differing only in chain length):

| mol_id | Chain length | MW     | ALogP  | PSA    | RotB |
|--------|-------------|--------|--------|--------|------|
| m15    | 2C          | 179.22 | 1.445  | 38.33  | 3    |
| m24    | 3C          | 193.25 | 1.835  | 38.33  | 4    |
| m28    | 4C          | 207.27 | 2.225  | 38.33  | 5    |
| m17    | 5C          | 221.30 | 2.615  | 38.33  | 6    |
| m22    | 6C          | 235.33 | 3.005  | 38.33  | 7    |
| **m05**| **11C**     | 305.46 | 4.956  | 38.33  | 12   |

**Congener IDs: m15, m17, m22, m24, m28**

### Measured QED Values (5 measurements)

| mol_id | Chain | QED    |
|--------|-------|--------|
| m15    | 2C    | 0.7621 |
| m24    | 3C    | 0.7919 |
| m28    | 4C    | 0.7518 |
| m17    | 5C    | 0.7498 |
| m22    | 6C    | 0.7377 |

### Trend Analysis

The QED shows a mild peak at 3C (0.7919) then gradually declines:
- 3C to 4C: -0.0401
- 4C to 5C: -0.0020
- 5C to 6C: -0.0121

The decline is gentle but should accelerate beyond the measured range because:
- ALogP goes from 3.0 (6C) to 5.0 (11C), entering the steep penalty region of the QED ALogP desirability function
- RotB goes from 7 (6C) to 12 (11C), entering the steep penalty region for rotatable bonds

### Model: Quadratic Fit of ln(QED)

Since QED is a geometric mean of desirability scores, modeling ln(QED) is more physically appropriate. Fitting a quadratic to all 5 points:

x = chain_length, centered at x' = x - 4:

**ln(QED) = -0.004271 * x'^2 - 0.01196 * x' - 0.26798**

Fit quality (predicted vs actual QED):
- 2C: 0.770 vs 0.762 (err +0.008)
- 3C: 0.771 vs 0.792 (err -0.021)
- 4C: 0.765 vs 0.752 (err +0.013)
- 5C: 0.753 vs 0.750 (err +0.003)
- 6C: 0.734 vs 0.738 (err -0.004)

### Extrapolation to m05 (chain = 11, x' = 7)

ln(QED) = -0.004271 * 49 - 0.01196 * 7 - 0.26798
= -0.2093 - 0.0837 - 0.2680
= -0.5610

**QED(m05) = exp(-0.561) = 0.57**

### Cross-Validation with Other Series

For additional calibration, I measured the nitrophenyl series and one biphenyl molecule:

| mol_id | Series        | Chain | QED    | ALogP |
|--------|--------------|-------|--------|-------|
| m09    | nitrophenyl  | 2C    | 0.5826 | 1.345 |
| m02    | nitrophenyl  | 3C    | 0.6046 | 1.735 |
| m12    | nitrophenyl  | 4C    | 0.4709 | 2.125 |
| m33    | nitrophenyl  | 6C    | 0.4591 | 2.905 |
| m29    | nitrophenyl  | 7C    | 0.4446 | 3.295 |
| m06    | nitrophenyl  | 9C    | 0.4011 | 4.075 |
| m20    | biphenyl     | 7C    | 0.6780 | 5.054 |

The biphenyl m20 at ALogP=5.05 (very close to m05's 4.96) has QED=0.678. Adjusting for m05's higher RotB (12 vs 8) and slightly better PSA (38.33 vs 29.1), the biphenyl comparison independently estimates QED(m05) in the range 0.56-0.63, consistent with the quadratic prediction of 0.57.

### Predicted QED for m05: **0.57**

---

## Job 2: Lead Compound Selection

### Pre-filtering (free checks before spending measurements)

**Constraint 2: PSA >= 40** eliminates molecules with PSA < 40 (m04, m05, m08, m11, m13, m15, m16, m17, m20, m22, m23, m24, m26, m28, m30)

**Constraint 3: Not in m05's series** eliminates m15, m17, m22, m24, m28

Remaining candidates: nitrophenyl series (m09, m02, m12, m32, m33, m29, m31, m06, m10) and sugar-alcohol series (m14, m27, m01, m19, m03, m07, m21, m18, m25)

### Measurement Strategy

Prioritized short-chain candidates with best property profiles:

1. **m09** (nitrophenyl, 2C): QED = 0.5826 (< 0.6, fails)
2. **m02** (nitrophenyl, 3C): QED = 0.6046 (>= 0.6, passes all constraints)

### Lead Selection: **m02**

| Constraint | Requirement | m02 Value | Pass? |
|-----------|-------------|-----------|-------|
| QED       | >= 0.6      | 0.6046    | Yes   |
| PSA       | >= 40       | 72.24     | Yes   |
| Series    | not m05     | nitrophenyl | Yes |

---

## Measurement Budget Summary

| #  | mol_id | Purpose | QED    |
|----|--------|---------|--------|
| 1  | m15    | m05 series (2C) | 0.7621 |
| 2  | m24    | m05 series (3C) | 0.7919 |
| 3  | m28    | m05 series (4C) | 0.7518 |
| 4  | m17    | m05 series (5C) | 0.7498 |
| 5  | m22    | m05 series (6C) | 0.7377 |
| 6  | m09    | Job 2 candidate | 0.5826 |
| 7  | m02    | Job 2 lead      | 0.6046 |
| 8  | m29    | Calibration (nitro 7C) | 0.4446 |
| 9  | m06    | Calibration (nitro 9C) | 0.4011 |
| 10 | m12    | Calibration (nitro 4C) | 0.4709 |
| 11 | m33    | Calibration (nitro 6C) | 0.4591 |
| 12 | m20    | Calibration (biphenyl 7C) | 0.6780 |

Total: 12/12 measurements used.
