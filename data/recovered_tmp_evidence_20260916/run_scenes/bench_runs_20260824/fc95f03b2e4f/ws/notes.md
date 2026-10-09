# Notes — Full Reasoning

## 1. Series Identification (from SMILES)

**m05** SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
Scaffold: methoxybenzamide `COc1ccc(cc1)C(=O)N–` with an 11-carbon N-alkyl chain.

### Series A — Methoxybenzamide (same scaffold as m05): `COc1ccc(cc1)C(=O)N–`
| mol_id | N-alkyl | chain_len | MW | ALOGP | PSA |
|--------|---------|-----------|---------|-------|-------|
| m15 | CC | 2 | 179.22 | 1.445 | 38.33 |
| m24 | CCC | 3 | 193.25 | 1.835 | 38.33 |
| m28 | CCCC | 4 | 207.27 | 2.225 | 38.33 |
| m17 | CCCCC | 5 | 221.30 | 2.615 | 38.33 |
| m22 | CCCCCC | 6 | 235.33 | 3.005 | 38.33 |
| **m05** | **CCCCCCCCCCC** | **11** | **305.46** | **4.956** | **38.33** |

### Series B — Gluconamide: `OCC(O)C(O)C(O)C(=O)N–` (PSA=110.02)
m14, m27, m01, m19, m03, m21, m18, m07, m25

### Series C — Nitrobenzamide: `O=[N+]([O-])c1ccc(cc1)C(=O)N–` (PSA=72.24)
m09, m02, m12, m32, m33, m29, m31, m06, m10

### Series D — Biphenyl: `c1ccc(cc1)-c1ccc(cc1)C(=O)N–` (PSA=29.1)
m13, m11, m04, m30, m23, m20, m26, m08, m16

## 2. Job 1 — Predicting QED of m05

### Measurements (Series A):
| mol_id | chain | QED (measured) |
|--------|-------|----------------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Observed trend:
- QED peaks at chain 3 (0.7919), then declines with increasing chain length
- Decline accelerates: Δ(3→4) = −0.040, Δ(4→5) = −0.002, Δ(5→6) = −0.012
- Physically: increasing ALOGP (4.956 for m05) and rotatable bonds (12) penalize QED

### Fitted model — quadratic (all 5 points):
**QED(x) = −0.003263·x² + 0.017014·x + 0.749338** where x = N-alkyl chain length

Fit quality:
| chain | predicted | actual | residual |
|-------|-----------|--------|----------|
| 2 | 0.770 | 0.762 | +0.008 |
| 3 | 0.771 | 0.792 | −0.021 |
| 4 | 0.765 | 0.752 | +0.013 |
| 5 | 0.753 | 0.750 | +0.003 |
| 6 | 0.734 | 0.738 | −0.004 |

### Extrapolation to chain 11:
QED(11) = −0.003263×121 + 0.017014×11 + 0.749338
= −0.3948 + 0.1872 + 0.7493 = **0.542**

Cross-check: quadratic through chains 4–6 only gives QED(11) = 0.526. The two models agree on ~0.54.

**Predicted QED for m05: 0.54**

## 3. Job 2 — Lead Selection

### Filtering (free checks first):
- **Series D**: PSA = 29.1 < 40 → all excluded
- **Series A**: same series as m05 → all excluded
- **Series B** (PSA=110.02) and **Series C** (PSA=72.24): pass PSA filter

### Measurements for lead candidates:
| mol_id | series | chain | QED | ≥0.6? |
|--------|--------|-------|-------|-------|
| m09 | C | 2 | 0.5826 | ✗ |
| m02 | C | 3 | **0.6046** | **✓** |
| m12 | C | 4 | 0.4709 | ✗ |
| m32 | C | 5 | 0.4683 | ✗ |
| m33 | C | 6 | 0.4591 | ✗ |
| m03 | B | 6 | 0.3282 | ✗ |
| m01 | B | 4 | 0.3187 | ✗ |

Series C: QED peaks at chain 3 (m02, 0.6046), then declines. Only m02 meets the QED ≥ 0.6 threshold.
Series B: Very low QED due to high PSA (110) and negative ALOGP.

### Selected lead: **m02**
- QED measured: 0.6046 (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Series C (nitrobenzamide), NOT Series A ✓

## 4. Budget used
12/12 measurements: m15, m24, m28, m17, m22, m09, m12, m32, m02, m33, m03, m01
