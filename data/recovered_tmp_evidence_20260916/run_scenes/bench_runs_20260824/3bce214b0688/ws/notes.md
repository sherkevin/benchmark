# Notes: QED Prediction and Lead Selection

## Congeneric Series Identification

### m05's Series (Series A: methoxybenzamides, scaffold `COc1ccc(cc1)C(=O)N-`)
- m15: chain length 2 (NCC)
- m24: chain length 3 (NCCC)
- m28: chain length 4 (NCCCC)
- m17: chain length 5 (NCCCCC)
- m22: chain length 6 (NCCCCCC)
- **m05**: chain length 11 (NCCCCCCCCCCC) — target, never synthesized

### Other series identified:
- **Series B** (sugar-amides, `OCC(O)C(O)C(O)C(=O)N-`): m14, m27, m01, m19, m03, m21, m18, m07, m25 (chains 2–10)
- **Series C** (nitrobenzamides, `O=[N+]([O-])c1ccc(cc1)C(=O)N-`): m09, m02, m12, m32, m33, m29, m31, m06, m10 (chains 2–10)
- **Series D** (biphenyl-amides, `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`): m13, m11, m04, m30, m23, m20, m26, m08, m16 (chains 2–10)

## Job 1: QED Prediction for m05

### Measured QEDs for Series A:
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

### Trend observed:
QED peaks at chain length 3 (0.7919) then declines monotonically through chains 4–6. The relationship is non-linear (concave down), so a quadratic fit is appropriate.

### Quadratic fit (degree=2):
QED(chain) = 0.74932 + 0.017024 × chain − 0.003264 × chain²
- RMSE on fitted points: 0.01188

### Linear fit (degree=1) for comparison:
QED(chain) = 0.79502 − 0.00909 × chain
- RMSE on fitted points: 0.01308

The quadratic fit is slightly better (lower RMSE) and captures the curvature. At chain length 11:
- **Quadratic prediction: 0.5416**
- Linear prediction: 0.695 (overestimates because it misses the curvature/peak)

I use the quadratic prediction: **target_qed = 0.5416**

## Job 2: Lead Compound Selection

### Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in Series A (m05's series)

### Pre-filtering by PSA ≥ 40 (free):
- Series A (PSA 38.33): excluded (both PSA < 40 AND same series as m05)
- Series B (PSA 110.02): passes PSA ✓
- Series C (PSA 72.24): passes PSA ✓
- Series D (PSA 29.1): excluded (PSA < 40)

### Measurements of candidates:
| mol_id | Series | QED    | Pass? |
|--------|--------|--------|-------|
| m09    | C      | 0.5826 | No (QED < 0.6) |
| m12    | C      | 0.4709 | No |
| m32    | C      | 0.4683 | No |
| m33    | C      | 0.4591 | No |
| m02    | C      | **0.6046** | **YES** |
| m14    | B      | 0.3237 | No |
| m01    | B      | 0.3187 | No |

### Selected lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- QED measured: 0.6046 (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Series: C (not Series A ✓)
- Chain length: 3
