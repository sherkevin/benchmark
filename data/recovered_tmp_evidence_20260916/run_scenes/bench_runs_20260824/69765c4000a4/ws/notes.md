# Notes: QED Prediction and Lead Selection

## Job 1 — Predicting QED of m05

### Target molecule
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzamide)
- N-alkyl chain length: **11 carbons**
- Never synthesized → must extrapolate from measured congeners

### Congeneric series (same scaffold, pool members)
| mol_id | chain_len | QED (measured) |
|--------|-----------|----------------|
| m15    | 2         | 0.7621         |
| m24    | 3         | 0.7919         |
| m28    | 4         | 0.7518         |
| m17    | 5         | 0.7498         |
| m22    | 6         | 0.7377         |

### Trend observed
QED rises from chain 2→3 (0.762→0.792), then steadily declines through chains 4–6 (0.752→0.750→0.738). The non-monotonic behavior suggests a peak near chain 3 with decline thereafter — consistent with increasing lipophilicity penalizing QED for longer chains.

### Fitted model (degree-2 polynomial, all 5 points)
```
QED(chain) = 0.74932 + 0.017024 × chain − 0.003264 × chain²
```
- RMSE on fitted points: 0.01188 (good fit)
- The negative quadratic coefficient captures the rise-then-fall pattern

### Extrapolation to m05 (chain=11)
```
QED(11) = 0.74932 + 0.017024 × 11 − 0.003264 × 121
        = 0.74932 + 0.18726 − 0.39494
        ≈ 0.5416
```

### Comparison with other fits
- Linear (all 5 pts): 0.695 — ignores curvature, likely too optimistic
- Quadratic (chain 3–6 only): 0.938 — upward artifact from subset, physically unreasonable
- Cubic (all 5 pts): 2.13 — clearly wrong (>1), overfitting

**Predicted QED for m05: 0.5416**

---

## Job 2 — Lead compound selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free check)
3. Not in m05's series (free check)

### Pre-filtering
- m05's series has PSA = 38.33 < 40, so NO series member passes the PSA screen
- 18 molecules pass PSA ≥ 40: sugar series (PSA=110.02) and nitrobenzamide series (PSA=72.24)

### Measurements of candidates
| mol_id | series         | PSA    | QED (measured) | Pass? |
|--------|----------------|--------|----------------|-------|
| m09    | nitrobenzamide | 72.24  | 0.5826         | No (QED < 0.6) |
| m02    | nitrobenzamide | 72.24  | **0.6046**     | **YES** |
| m12    | nitrobenzamide | 72.24  | 0.4709         | No |
| m14    | sugar          | 110.02 | 0.3237         | No |
| m32    | nitrobenzamide | 72.24  | 0.4683         | No |
| m27    | sugar          | 110.02 | 0.3330         | No |

### Selected lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Measured QED: **0.6046**
- PSA: 72.24 ≥ 40 ✓
- Series: nitrobenzamide (NOT m05's methoxybenzamide series) ✓
- All three constraints satisfied

### Budget used
- 5 measurements for m05 series (m15, m24, m28, m17, m22)
- 6 measurements for lead candidates (m09, m02, m12, m14, m32, m27)
- Total: 11 of 12 measurements used
