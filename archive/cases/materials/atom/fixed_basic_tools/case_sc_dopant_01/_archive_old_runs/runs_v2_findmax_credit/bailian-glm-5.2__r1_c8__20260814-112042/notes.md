# Notes: highest-Tc composition under measurement budget

## Candidate structure analysis

All 30 candidates share the host formula **O7Cu3Ba2Pr{p}(RE){1-p}** — a YBCO-family
cuprate (Ba2Cu3O7 backbone) where the rare-earth (Y) site is split between **Pr**
(fraction p) and a rare-earth dopant **RE ∈ {Er, Gd, Nd}** (fraction 1−p).

Pr fractions present: 0.030, 0.076, 0.078, 0.080, 0.150, 0.200, 0.300, 0.400,
0.500, 0.600, 0.700, 0.800, 0.900 (Pr+RE = 1.0 in every row).

## Underlying physics (prior)

This is the **Pr-substitution suppression law** for R-123 cuprates:
- Pure R-123 (Pr = 0) superconducts: Y-123 ≈ 92 K, Nd-123 ≈ 93–96 K,
  Gd-123 ≈ 93 K, Er-123 ≈ 91 K. Nd/Gd tend slightly above Y; Er slightly below.
- Pr-123 is **non-superconducting** (4f magnetism breaks the pairing).
- Substituting Pr onto the RE site **monotonically suppresses Tc** toward ~0 as
  Pr fraction → 1.

So the predicted ranking: **lowest Pr fraction → highest Tc**, and at matched
low Pr, the RE with the highest base Tc (likely Nd ≳ Gd ≳ Er) wins.

## Budget plan (8 credits)

The three candidates at the lowest Pr fraction (p = 0.030) are the top suspects:
- O7Cu3Ba2Pr0.030Er0.970
- O7Cu3Ba2Pr0.030Gd0.970
- O7Cu3Ba2Pr0.030Nd0.970

Plan:
1. Measure the three p=0.030 variants (3 credits) — find the best RE.
2. Measure the second tier for each RE (p≈0.076–0.080) to confirm monotonic
   suppression (3 credits).
3. Spend the last 2 credits to confirm the trend at high Pr (suppression check)
   and/or break any tie at the top.

## Measurement log

| # | Composition | Tc (K) | Credit |
|---|---|---|---|
| 1 | O7Cu3Ba2Pr0.030Er0.970 | **93.9** | 1 |
| 2 | O7Cu3Ba2Pr0.030Gd0.970 | 93.1 | 2 |
| 3 | O7Cu3Ba2Pr0.030Nd0.970 | 85.1 | 3 |
| 4 | O7Cu3Ba2Pr0.076Er0.924 | 91.1 | 4 |
| 5 | O7Cu3Ba2Pr0.078Gd0.922 | 84.0 | 5 |
| 6 | O7Cu3Ba2Pr0.080Nd0.920 | 68.2 | 6 |
| 7 | O7Cu3Ba2Pr0.200Er0.800 | 76.4 | 7 |
| 8 | O7Cu3Ba2Pr0.800Er0.200 | 0.0 | 8 |

## Results by rare-earth

| Pr (p) | Er | Gd | Nd |
|---|---|---|---|
| 0.030 | **93.9** | 93.1 | 85.1 |
| ~0.078 | 91.1 | 84.0 | 68.2 |
| 0.200 | 76.4 | — | — |
| 0.800 | 0.0 | — | — |

## Interpretation

- **At the lowest Pr (0.030), Er (93.9) > Gd (93.1) > Nd (85.1).** Er wins the
  top tier.
- **Suppression is monotonic and steepest for Nd, flattest for Er**:
  - Er: 93.9 → 91.1 → 76.4 → 0.0 (Δ−2.8 / +0.046 Pr near top, then steep)
  - Gd: 93.1 → 84.0 (Δ−9.1 / +0.048 Pr)
  - Nd: 85.1 → 68.2 (Δ−16.9 / +0.050 Pr)
- Since Er has both the highest baseline Tc *and* the flattest suppression slope,
  Er at the minimum available Pr fraction (0.030) is the global maximum. No
  other candidate can beat 93.9 K:
  - Higher-Pr Er variants are already measured lower (91.1, 76.4, 0.0).
  - Gd/Nd start lower and suppress faster, so at any Pr ≥ 0.030 they are below Er.

## Answer

**O7Cu3Ba2Pr0.030Er0.970 — 93.9 K** is the highest-Tc composition.
