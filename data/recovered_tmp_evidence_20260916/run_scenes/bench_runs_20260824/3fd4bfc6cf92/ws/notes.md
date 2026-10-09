# Notes: QED Prediction and Lead Selection

## Series Identification

Target m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxyphenyl amide scaffold with 11-carbon N-alkyl chain.

Four congeneric series identified by scaffold:

1. **Methoxyphenyl amide** (`COc1ccc(cc1)C(=O)N...`) — m05's series:
   - m15 (2C), m24 (3C), m28 (4C), m17 (5C), m22 (6C), m05 (11C, unsynthesized target)

2. **Sugar acid amide** (`OCC(O)C(O)C(O)C(=O)N...`):
   - m14, m27, m01, m19, m03, m21, m18, m07, m25

3. **Nitrophenyl amide** (`O=[N+]([O-])c1ccc(cc1)C(=O)N...`):
   - m09, m02, m12, m32, m33, m29, m31, m06, m10

4. **Biphenyl amide** (`c1ccc(cc1)-c1ccc(cc1)C(=O)N...`):
   - m13, m11, m04, m30, m23, m20, m26, m08, m16

## Job 1: Predict m05 QED

### Measurements on m05 series congeners:
| mol_id | Chain C | QED    |
|--------|---------|--------|
| m15    | 2       | 0.7621 |
| m24    | 3       | 0.7919 |
| m28    | 4       | 0.7518 |
| m17    | 5       | 0.7498 |
| m22    | 6       | 0.7377 |

### Observed trend:
QED rises 2C→3C (peak 0.7919), then declines 3C→6C. Clearly non-linear.

### Fitted model (degree-2 polynomial):
QED(x) = 0.74932 + 0.017024·x − 0.003264·x²
RMSE = 0.0119

### Extrapolation to x=11: **Predicted QED = 0.5416**

## Job 2: Lead Selection

### Constraints: QED ≥ 0.6, PSA ≥ 40, not m05 series
- screen_candidates() gave 18 with PSA ≥ 40
- Biphenyl and methoxyphenyl series all fail PSA filter
- Measured 3 nitrophenyl candidates: m09 (0.5826, fail), m02 (0.6046, pass), m12 (0.4709, fail)

### Chosen lead: **m02** — QED=0.6046, PSA=72.24, nitrophenyl series
