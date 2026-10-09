# Notes

## Congeneric Series Identification

Target m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxybenzamide scaffold with 11-carbon N-alkyl chain.

### Series 1 — Methoxybenzamide (same scaffold as m05): `COc1ccc(cc1)C(=O)N-`
- m15: NCC (2C), mw=179.22, alogp=1.445, psa=38.33
- m24: NCCC (3C), mw=193.25, alogp=1.835, psa=38.33
- m28: NCCCC (4C), mw=207.27, alogp=2.225, psa=38.33
- m17: NCCCCC (5C), mw=221.30, alogp=2.615, psa=38.33
- m22: NCCCCCC (6C), mw=235.33, alogp=3.005, psa=38.33
- m05: NCCCCCCCCCCC (11C) — TARGET, never synthesized

### Series 2 — Sugar amide: `OCC(O)C(O)C(O)C(=O)N-`
m01(4C), m03(6C), m07(9C), m14(2C), m18(8C), m19(5C), m21(7C), m25(10C), m27(3C)

### Series 3 — Nitrobenzamide: `O=[N+]([O-])c1ccc(cc1)C(=O)N-`
m02(3C), m06(9C), m09(2C), m10(10C), m12(4C), m29(7C), m31(8C), m32(5C), m33(6C)

### Series 4 — Biphenyl amide: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`
m04(4C), m08(9C), m11(3C), m13(2C), m16(10C), m20(7C), m23(6C), m26(8C), m30(5C)

## Plan
- Job 1: Measure all 5 members of Series 1 (m15, m24, m28, m17, m22) → 5 measurements
- Job 2: Need QED ≥ 0.6, psa ≥ 40, not in Series 1
  - Series 4 has psa=29.1 → excluded (fails psa ≥ 40)
  - Candidates: Series 2 (psa=110.02) and Series 3 (psa=72.24)
  - Remaining budget: 7 measurements

## Measurements — Job 1 (Series 1, methoxybenzamide)

| mol_id | chain | alogp | QED (measured) |
|--------|-------|-------|-----------------|
| m15 | 2C | 1.445 | 0.7621 |
| m24 | 3C | 1.835 | 0.7919 |
| m28 | 4C | 2.225 | 0.7518 |
| m17 | 5C | 2.615 | 0.7498 |
| m22 | 6C | 3.005 | 0.7377 |

### Trend Analysis
QED peaks at 3C (0.7919) then declines. Deltas per CH2:
- 2→3: +0.0298
- 3→4: -0.0401
- 4→5: -0.0020
- 5→6: -0.0121

The decline accelerates as chain grows (except for the anomalous 4→5 step). This makes physical sense: each additional CH2 raises logP by ~0.39, pushing the molecule further from the QED logP sweet spot (~1.5-2.5).

### Fitted Model: Quadratic regression on all 5 points
y = -0.003264x² + 0.017022x + 0.749324

Check against data:
- x=2: 0.770 (actual 0.762)
- x=3: 0.771 (actual 0.792)
- x=4: 0.765 (actual 0.752)
- x=5: 0.753 (actual 0.750)
- x=6: 0.734 (actual 0.738)

### Extrapolation to m05 (x=11)
QED(11) = -0.003264(121) + 0.017022(11) + 0.749324
= -0.3949 + 0.1872 + 0.7493
= 0.5416

**Predicted QED for m05: ~0.54**

Physical justification: At 11C, logP≈4.96 and RotB=12, both in penalty regions of QED desirability functions. The quadratic captures the accelerating penalty as logP moves from ~2 (measured region) toward ~5 (target).

## Measurements — Job 2 (Lead selection)

Measured candidates (all psa ≥ 40, not in Series 1):

| mol_id | series | chain | psa | alogp | QED |
|--------|--------|-------|-----|-------|-----|
| m09 | nitro | 2C | 72.24 | 1.345 | 0.5826 |
| m02 | nitro | 3C | 72.24 | 1.735 | **0.6046** ✓ |
| m12 | nitro | 4C | 72.24 | 2.125 | 0.4709 |
| m32 | nitro | 5C | 72.24 | 2.515 | 0.4683 |
| m03 | sugar | 6C | 110.02 | -1.242 | 0.3282 |
| m07 | sugar | 9C | 110.02 | -0.072 | 0.3233 |
| m33 | nitro | 6C | 72.24 | 2.905 | 0.4591 |

Sugar amides (Series 2) have very low QED (~0.33) due to extreme PSA (110) and negative alogp.
Nitrobenzamides (Series 3) peak at 3C (m02, QED=0.6046).

### Lead Selection: m02
- Measured QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Series 3 (nitrobenzamide), not in Series 1 ✓

All three constraints satisfied.
