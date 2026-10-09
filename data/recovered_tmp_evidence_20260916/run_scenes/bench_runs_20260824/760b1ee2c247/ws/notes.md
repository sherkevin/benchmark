# Notes: QED Prediction and Lead Selection

## Series Identification

### Target molecule m05
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Scaffold: COc1ccc(cc1)C(=O)N- (4-methoxyphenyl amide)
- N-alkyl chain length: 11 carbons

### Congeneric series for m05 (same scaffold, varying chain length)
| mol_id | Chain length | PSA |
|--------|-------------|-----|
| m15 | 2 | 38.33 |
| m24 | 3 | 38.33 |
| m28 | 4 | 38.33 |
| m17 | 5 | 38.33 |
| m22 | 6 | 38.33 |

### Other series identified
- Nitrophenyl amide: m02, m06, m09, m10, m12, m29, m31, m32, m33 (PSA 72.24)
- Sugar acid amide: m01, m03, m07, m14, m18, m19, m21, m25, m27 (PSA 110.02)
- Biphenyl amide: m04, m08, m11, m13, m16, m20, m23, m26, m30 (PSA 29.1)

## Job 1 - QED Prediction for m05

### Measured QED values
| mol_id | Chain | QED |
|--------|-------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Observed trend
QED peaks at chain 3 (0.7919) then declines. Non-linear relationship.

### Fitted model (degree-2 polynomial)
QED(x) = 0.74932 + 0.017024*x - 0.003264*x^2
RMSE: 0.01188
Prediction for m05 (x=11): 0.5416

## Job 2 - Lead Selection

### Constraints
1. QED >= 0.6 (measured)
2. PSA >= 40 (free)
3. Not in m05 series (free)

### Candidates measured
| mol_id | QED | PSA | Pass? |
|--------|------|-----|-------|
| m09 | 0.5826 | 72.24 | No |
| m12 | 0.4709 | 72.24 | No |
| m02 | 0.6046 | 72.24 | YES |

### Selected lead: m02
- QED: 0.6046 (>= 0.6)
- PSA: 72.24 (>= 40)
- Series: Nitrophenyl (not m05's series)
