# Notes: QED Prediction and Lead Selection

## Catalogue Exploration (9 pages, 9 budget units)

Total 33 molecules in 4 congeneric series:

### Series A: Methoxyphenyl amide — `COc1ccc(cc1)C(=O)N-`
| mol_id | chain | MW | ALogP | PSA | RotB |
|--------|-------|-------|-------|-------|------|
| m15 | 2C | 179.22 | 1.445 | 38.33 | 3 |
| m24 | 3C | 193.25 | 1.835 | 38.33 | 4 |
| m28 | 4C | 207.27 | 2.225 | 38.33 | 5 |
| m17 | 5C | 221.30 | 2.615 | 38.33 | 6 |
| m22 | 6C | 235.33 | 3.005 | 38.33 | 7 |
| **m05** | **11C** | **305.46** | **4.956** | **38.33** | **12** |

### Series B: Sugar acid — `OCC(O)C(O)C(O)C(=O)N-` (9 members, PSA=110.02)
m14(2C), m27(3C), m01(4C), m19(5C), m03(6C), m21(7C), m18(8C), m07(9C), m25(10C)

### Series C: Nitrobenzene — `O=[N+]([O-])c1ccc(cc1)C(=O)N-` (9 members, PSA=72.24)
m09(2C), m02(3C), m12(4C), m32(5C), m33(6C), m29(7C), m31(8C), m06(9C), m10(10C)

### Series D: Biphenyl — `c1ccc(cc1)-c1ccc(cc1)C(=O)N-` (9 members, PSA=29.1)
m13(2C), m11(3C), m04(4C), m30(5C), m23(6C), m20(7C), m26(8C), m08(9C), m16(10C)

---

## Job 1: Predicting QED of m05

### Measurements (5 of 17 budget)

| mol_id | chain | QED measured |
|--------|-------|-------------|
| m15 | 2C | 0.7621 |
| m24 | 3C | 0.7919 |
| m28 | 4C | 0.7518 |
| m17 | 5C | 0.7498 |
| m22 | 6C | 0.7377 |

### Observed trend
QED peaks at 3-carbon chain (0.7919), then declines:
- 2C→3C: +0.0298
- 3C→4C: -0.0401
- 4C→5C: -0.0020
- 5C→6C: -0.0121

The decline is NOT linear. The 2C→3C rise suggests the very short chains have slightly suboptimal (low) logP, 3C is near optimal, and longer chains push logP into the penalty zone. As chain length increases, both ALogP and rotatable bonds increase linearly, but QED penalties for these are nonlinear — they accelerate.

### Model: Least-squares quadratic on all 5 points

Fitting y = ax^2 + bx + c to (chain_length, QED):

**Result: y = -0.003264x^2 + 0.017022x + 0.749324**

Verification against measured data:
- x=2: predicted 0.7703, actual 0.7621 (error +0.008)
- x=3: predicted 0.7710, actual 0.7919 (error -0.021)
- x=4: predicted 0.7652, actual 0.7518 (error +0.013)
- x=5: predicted 0.7528, actual 0.7498 (error +0.003)
- x=6: predicted 0.7340, actual 0.7377 (error -0.004)

Max residual ~0.021, sum of residuals = 0 (as expected for LS).

### Extrapolation to m05 (x=11)

y(11) = -0.003264 * 121 + 0.017022 * 11 + 0.749324
      = -0.39494 + 0.18724 + 0.74932
      = **0.5416**

**Predicted QED for m05 ~ 0.54**

Physical plausibility check: m05 has ALogP = 4.956 (high, well above optimal ~1-2) and 12 rotatable bonds (above the ~7-8 threshold). Both properties carry nonlinear QED penalties that grow with chain length. A QED of ~0.54 represents a substantial drop from the ~0.74-0.79 range of the measured congeners, consistent with these penalties.

### Alternative models considered
- Linear fit (all 5 pts): QED ~ 0.695 — ignores curvature, overestimates
- Linear fit (3C-6C only): QED ~ 0.651 — ignores acceleration of decline
- Quadratic through last 3 pts: QED ~ 0.526 — consistent with main estimate

The quadratic model on all 5 points is preferred because it uses all data and captures the nonlinear trend.

---

## Job 2: Lead Compound Selection

### Constraint filtering (free, no budget cost)

1. **PSA >= 40**: Eliminates all biphenyl series (PSA=29.1) and all m05-series (PSA=38.33)
2. **Not in m05 series**: Already eliminated above
3. **QED >= 0.6**: Requires measurement

Remaining candidates: sugar acid series (PSA=110.02) and nitrobenzene series (PSA=72.24)

### Measurements on lead candidates (3 of 17 budget)

I chose the shortest-chain nitrobenzene members (most drug-like ALogP):
| mol_id | series | PSA | QED | Passes all? |
|--------|--------|------|-------|-------------|
| m09 | nitro, 2C | 72.24 | 0.5826 | NO (QED < 0.6) |
| m02 | nitro, 3C | 72.24 | 0.6046 | **YES** |
| m12 | nitro, 4C | 72.24 | 0.4709 | NO (QED < 0.6) |

### Lead selection: **m02**

- Measured QED = 0.6046 >= 0.6 ✓
- PSA = 72.24 >= 40 ✓
- Not in m05 series (nitrobenzene series) ✓

SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC

---

## Budget Summary

- list_pool: 9 pages = 9 units
- measure_qed (m05 series): 5 measurements = 5 units
- measure_qed (lead candidates): 3 measurements = 3 units
- **Total: 17/17 budget used**
