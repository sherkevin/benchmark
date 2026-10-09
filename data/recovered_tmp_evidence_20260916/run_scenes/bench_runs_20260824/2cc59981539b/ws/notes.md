# Task notes

## Pool analysis

`m05` = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxy-phenyl amide, N-alkyl chain of 10 carbons.
Scaffold: `COc1ccc(cc1)C(=O)N<alkyl>`.

### Congeneric series of m05 (same scaffold, only N-alkyl chain length differs)
Same scaffold `COc1ccc(cc1)C(=O)N(Cn)`:
- m15: NCC        (n=2)
- m24: NCCC       (n=3)
- m28: NCCCC      (n=4)
- m17: NCCCCC     (n=5)
- m22: NCCCCCC     (n=6)
- m05: NCCCCCCCCCCC (n=10)  ← target, never synthesized (probe_unavailable)

These are the molecules whose QED trend lets us predict m05's QED.

### Other series in the pool (for Task 2 — must NOT be in m05's series)
- Nitro series `O=[N+]([O-])c1ccc(cc1)C(=O)N<alkyl>`, psa=72.24:
  m02(n=3), m06(n=8), m09(n=2), m10(n=10), m12(n=4), m29(n=7), m31(n=8), m32(n=5), m33(n=6)
- Biphenyl series `c1ccc(cc1)-c1ccc(cc1)C(=O)N<alkyl>`, psa=29.1 (EXCLUDE for Task 2, psa<40):
  m04, m08, m11, m13, m16, m20, m23, m26, m30
- Sugar/polyol series `OCC(O)C(O)C(O)C(=O)N<alkyl>`, psa=110.02:
  m01(n=4), m03(n=6), m07(n=8), m14(n=2), m18(n=7), m19(n=5), m21(n=7), m25(n=9), m27(n=3)

## Task 2 candidate pool (psa>=40, not m05 series)
Nitro series + sugar series. Need measured QED >= 0.6.

## Budget
12 total measurements shared between Task 1 (predict m05) and Task 2.

## Plan
1. Measure m05 congeneric series (m15,m24,m28,m17,m22) → predict m05 QED from chain-length trend. (5 measurements)
2. Use remaining budget (~7) to find a Task 2 lead among nitro/sugar series with QED>=0.6, psa>=40.

## Measurements

### Task 1 — m05 congeneric series (methoxy-phenyl amide, psa=38.33)
| mol_id | n (chain C) | alogp | rotb | QED measured |
|--------|------|-------|------|---------------|
| m15 | 2  | 1.445 | 3  | 0.7621 |
| m24 | 3  | 1.835 | 4  | 0.7919 (peak) |
| m28 | 4  | 2.225 | 5  | 0.7518 |
| m17 | 5  | 2.615 | 6  | 0.7498 |
| m22 | 6  | 3.005 | 7  | 0.7377 |
| m05 | 10 | 4.956 | 12 | (unsynthesized — predict) |

m05 confirmed probe_unavailable (free).

Trend: QED peaks at n=3 (0.7919), then declines as alogp drifts past ~3 and rotb grows.
From peak: −0.0401 (n3→4), −0.0020 (n4→5), −0.0121 (n5→6).
At n=6 we are 0.0542 below peak. m05 sits at n=10, alogp~4.96 (well into lipophilicity
penalty) and rotb=12 (well past the rotb desirability cutoff ~5, fully penalized).
The decline must continue past n=6.

Linear-fit estimate on n=3..6 (the post-peak decline, 4 points): slope ≈ −0.018/n,
intercept ≈ 0.846 → at n=10: 0.846 − 0.018*10 = 0.666. But QED components saturate
(rotb fully penalized by n~7-8, alogp penalty keeps growing), so the true value sits
below a pure linear extrapolation. Conservative estimate: 0.62–0.66.

**Predicted m05 QED ≈ 0.64** (midpoint of the 0.62–0.66 band; reflects continued
alogp/rotb-driven decline past n=6, but the QED floor is held up by the still-favorable
arom=1, acceptable MW=305, and single HBD/HBA amide motif).

### Task 2 — screening nitro/sugar series (psa>=40, not m05 series)
| mol_id | series | n | psa  | alogp  | QED measured | >=0.6? |
|--------|--------|---|------|--------|--------------|--------|
| m09 | nitro  | 2 | 72.24 | 1.345 | 0.5826 | no |
| m12 | nitro  | 4 | 72.24 | 2.125 | 0.4709 | no |
| m02 | nitro  | 3 | 72.24 | 1.735 | 0.6046 | **YES** |

**Lead = m02** — `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`:
- measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- nitro series ≠ m05's methoxy series ✓

Budget used: 8/12 (m15,m24,m28,m17,m22,m09,m12,m02). m05 probe free.
