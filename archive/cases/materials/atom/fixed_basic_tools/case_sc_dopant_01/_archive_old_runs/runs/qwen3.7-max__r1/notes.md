# Notes: Predicting Tc for Pr-doped GdBCO series

## Strategy
1. Measure both synthesized RE series (Er and Nd) to learn how Pr suppresses Tc.
2. Interpolate Tc(x) between Er and Nd using Gd's ionic radius to predict the
   unsynthesized Gd series.
3. Use piecewise linear interpolation between anchored Gd points.

## Measurements (10 of 14 instrument calls used)

### Er series: Pr(x)Er(1−x)Ba₂Cu₃O₇
| x     | Tc (K) |
|-------|--------|
| 0.030 | 93.9   |
| 0.150 | 82.9   |
| 0.300 | 61.1   |
| 0.500 | 28.6   |
| 0.800 | 0.0    |

### Nd series: Pr(x)Nd(1−x)Ba₂Cu₃O₇
| x     | Tc (K) |
|-------|--------|
| 0.030 | 85.1   |
| 0.150 | 40.7   |
| 0.300 | 5.6    |
| 0.500 | 0.0    |
| 0.900 | 0.0    |

## Key observation
Pr suppresses Tc in both series, but Nd shows **far stronger suppression** than Er:
- At x=0.300: Er retains 61.1 K, Nd drops to 5.6 K.
- SC is killed at x ≈ 0.65 for Er, but already at x ≈ 0.35–0.45 for Nd.

This correlates with the **RE ionic radius** (Shannon VIII coordination):
- Er³⁺ = 1.004 Å (smallest)
- Gd³⁺ = 1.053 Å (intermediate)
- Nd³⁺ = 1.109 Å (largest, closest to Pr³⁺ at 1.126 Å)

When the host RE size closely matches Pr, Pr sits comfortably on the RE site
and hybridizes more effectively with CuO₂ planes, causing stronger electronic
pair-breaking. Larger mismatch (Er ≪ Pr) means more lattice strain per Pr but
weaker pair-breaking efficiency.

## Model: ionic-radius interpolation

Gd sits between Er and Nd. I define the interpolation weight:

    w = (r_Gd − r_Er) / (r_Nd − r_Er) = (1.053 − 1.004) / (1.109 − 1.004)
      = 0.049 / 0.105 = 0.4667

At each Pr content x shared between Er and Nd data:

    Tc(Gd, x) = (1 − w) · Tc(Er, x) + w · Tc(Nd, x)
              = 0.5333 · Tc(Er, x) + 0.4667 · Tc(Nd, x)

**Interpolated Gd anchor points (at shared x values):**
| x     | Tc(Er) | Tc(Nd) | Tc(Gd)   |
|-------|--------|--------|----------|
| 0.030 | 93.9   | 85.1   | 89.8     |
| 0.150 | 82.9   | 40.7   | 63.2     |
| 0.300 | 61.1   | 5.6    | 35.2     |
| 0.500 | 28.6   | 0.0    | 15.3     |

For x > 0.500: only Er contributes (Nd is already 0). Using Er quadratic
(Tc = 96.76 − 92.66x − 87.34x², fit to x=0.030, 0.300, 0.500):
- Tc(Er, 0.600) = 9.72 → Tc(Gd, 0.600) = 0.5333 × 9.72 = 5.2
- Tc(Er, 0.650) ≈ 0 → Tc(Gd, 0.650) ≈ 0 → xc(Gd) ≈ 0.65

For x > 0.65: Tc = 0.

## Predictions for target compositions (piecewise linear between anchors)

Segment slopes:
- [0.030, 0.150]: (63.2−89.8)/0.120 = −221.7 K per unit x
- [0.150, 0.300]: (35.2−63.2)/0.150 = −186.7 K per unit x
- [0.300, 0.500]: (15.3−35.2)/0.200 = −99.5 K per unit x
- [0.500, 0.650]: (0−15.3)/0.150 = −102.0 K per unit x

| Target composition          | x     | Segment  | Tc (K) |
|-----------------------------|-------|----------|--------|
| O7Cu3Ba2Pr0.030Gd0.970     | 0.030 | anchor   | 89.8   |
| O7Cu3Ba2Pr0.078Gd0.922     | 0.078 | [0.03,0.15] | 89.8 − 221.7×0.048 = 79.2 |
| O7Cu3Ba2Pr0.150Gd0.850     | 0.150 | anchor   | 63.2   |
| O7Cu3Ba2Pr0.200Gd0.800     | 0.200 | [0.15,0.30] | 63.2 − 186.7×0.050 = 53.9 |
| O7Cu3Ba2Pr0.300Gd0.700     | 0.300 | anchor   | 35.2   |
| O7Cu3Ba2Pr0.400Gd0.600     | 0.400 | [0.30,0.50] | 35.2 − 99.5×0.100 = 25.3 |
| O7Cu3Ba2Pr0.500Gd0.500     | 0.500 | anchor   | 15.3   |
| O7Cu3Ba2Pr0.600Gd0.400     | 0.600 | [0.50,0.65] | 15.3 − 102.0×0.100 = 5.1 |
| O7Cu3Ba2Pr0.700Gd0.300     | 0.700 | > xc     | 0.0    |
| O7Cu3Ba2Pr0.800Gd0.200     | 0.800 | > xc     | 0.0    |
| O7Cu3Ba2Pr0.900Gd0.100     | 0.900 | > xc     | 0.0    |

## Summary quantities

**max_pr_above_80K = 0.030**
- x=0.030: Tc = 89.8 ≥ 80 ✓
- x=0.078: Tc = 79.2 < 80 ✗ (borderline — within ~1–3 K of threshold)
- All higher x: well below 80 K

**critical_pr = 0.600**
- Highest Pr content in the target list with Tc > 0.
- x=0.600: Tc = 5.1 > 0 ✓
- x=0.700: Tc = 0 ✗
- The continuous xc ≈ 0.65 (where the interpolation reaches 0).

## B-C-O-Cu-Sr compositions (distractors)
The 6 B(x)C(1−x)O₅CuSr₂ compositions in synthesized.txt are a completely
different structural family (not RE-123 cuprates). They would not inform
predictions for the Gd Pr-doping series. Correctly ignored (saved 6 calls).

## Instrument budget
- Used: 10 of 14 (5 Er + 5 Nd)
- Remaining: 4 (unused — target samples don't exist for measurement)
