# Notes: QED Prediction and Lead Selection

## Series Identification

### m05 series (scaffold: COc1ccc(cc1)C(=O)N — methoxyphenyl amide)
- **m05** (target, unsynthesized): `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — 11 carbon N-alkyl chain
- Pool members (same scaffold, varying chain length):
  - m15: 2C chain
  - m24: 3C chain
  - m28: 4C chain
  - m17: 5C chain
  - m22: 6C chain

### Other series in the pool:
- **Sugar series** (OCC(O)C(O)C(O)C(=O)N): m01, m03, m07, m14, m18, m19, m21, m25, m27
- **Nitrophenyl series** (O=[N+]([O-])c1ccc(cc1)C(=O)N): m02, m06, m09, m10, m12, m29, m31, m32, m33
- **Biphenyl series** (c1ccc(cc1)-c1ccc(cc1)C(=O)N): m04, m08, m11, m13, m16, m20, m23, m26, m30

## Job 1: QED Prediction for m05

### Measurements (5 of 12 budget used):
| mol_id | Chain length (C) | QED |
|--------|------------------|--------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Observed Trend:
QED peaks at chain length 3 (0.7919) then declines through 4, 5, 6. The curve is concave down.

### Fitted Model (degree-2 polynomial):
- y = 0.74932 + 0.017024*x − 0.003264*x²
- RMSE on training points: 0.01188
- Linear fit RMSE was higher (0.01308), confirming nonlinearity

### Extrapolation to m05 (chain length = 11):
- Predicted QED = **0.5416**
- This is a significant extrapolation (from max observed chain=6 to chain=11), so uncertainty is high
- The declining quadratic reflects growing lipophilicity penalty as chain lengthens

## Job 2: Lead Compound Selection

### Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free check)
3. Not in m05's series (free check)

### Pre-filtering (free):
- Used `screen_candidates()` — 18 molecules pass PSA ≥ 40
- Excluded m05-series members (none passed PSA screen anyway, since psa=38.33 < 40)
- Biphenyl series all have PSA=29.1, so excluded
- Remaining: sugar series (9 molecules) and nitrophenyl series (9 molecules)

### Strategic measurement reasoning:
- **Sugar series**: Very high PSA (110), very negative logP (-2.8 to +0.3), no aromatic rings, many HBD/HBA → expected low QED
- **Nitrophenyl series**: Moderate PSA (72.24), moderate logP, 1 aromatic ring, fewer rotb → better QED prospects, especially for short chains

### Measurements for lead candidates (3 more used, 8 total of 12):
| mol_id | Series | QED | Pass? |
|--------|--------|------|-------|
| m09 | Nitro, 2C | 0.5826 | QED < 0.6 ✗ |
| m02 | Nitro, 3C | 0.6046 | ✓ ALL constraints |
| m12 | Nitro, 4C | 0.4709 | QED < 0.6 ✗ |

### Selected Lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Measured QED: 0.6046 (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Series: Nitrophenyl (not m05's series ✓)
- Shortest nitrophenyl chain with passing QED; longer chains showed declining QED

## Budget Summary
- Total measurements used: 8 of 12
- 5 for m05 series (Job 1)
- 3 for lead candidates (Job 2)
- 4 remaining (unused)
