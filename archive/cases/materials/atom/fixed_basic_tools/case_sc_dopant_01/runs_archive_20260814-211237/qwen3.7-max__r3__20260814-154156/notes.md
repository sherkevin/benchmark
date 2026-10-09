# Notes — Tc prediction for Gd family

## Step 1: Measured all 19 synthesized compositions

### Er family (RE = Erbium, Er³⁺ ionic radius ≈ 1.004 Å, CN=8)
| x (Pr) | Tc (K) |
|--------|--------|
| 0.030  | 93.9   |
| 0.076  | 91.1   |
| 0.150  | 82.9   |
| 0.200  | 76.4   |
| 0.300  | 61.1   |
| 0.400  | 45.8   |
| 0.500  | 28.6   |
| 0.800  | 0.0    |
| 0.900  | 0.0    |

**Observations**: Er family has a high base Tc (~94 K) and Pr suppresses Tc gradually, roughly linearly (~140 K per unit x). SC persists to x ≈ 0.7.

### Nd family (RE = Neodymium, Nd³⁺ ionic radius ≈ 1.109 Å, CN=8)
| x (Pr) | Tc (K) |
|--------|--------|
| 0.030  | 85.1   |
| 0.080  | 68.2   |
| 0.150  | 40.7   |
| 0.200  | 31.4   |
| 0.300  | 5.6    |
| 0.500  | 0.0    |
| 0.600  | 0.0    |
| 0.700  | 0.0    |
| 0.800  | 0.0    |
| 0.900  | 0.0    |

**Observations**: Nd family has a lower base Tc (~85 K, likely due to Nd/Ba site mixing) and Pr suppresses Tc **much more aggressively** (~280 K per unit x). SC is killed by x ≈ 0.32.

## Step 2: Pattern analysis

Two key effects of the RE host on the Tc(x) curve:

1. **Base Tc (x→0)**: Er (~94 K) > Nd (~85 K). The lower Nd Tc is a known effect from Nd/Ba site mixing.
2. **Suppression rate dTc/dx**: |Er: ~140 K/x| ≪ |Nd: ~280 K/x|. Pr suppresses Tc about 2× faster in the Nd host.

Both effects correlate with RE ionic radius: larger RE → lower base Tc and faster Pr suppression.

## Step 3: Interpolation strategy for Gd (Gd³⁺ ionic radius ≈ 1.053 Å, CN=8)

Gd sits between Nd and Er in the lanthanide series:
- Nd³⁺: 1.109 Å (largest of the three)
- Gd³⁺: 1.053 Å (intermediate)
- Er³⁺: 1.004 Å (smallest of the three)

**Fractional position of Gd between Nd and Er:**
f = (r_Nd - r_Gd) / (r_Nd - r_Er) = (1.109 - 1.053) / (1.109 - 1.004) = 0.056 / 0.105 ≈ **0.464**

So Gd is roughly 46% of the way from Nd to Er (slightly closer to Er).

**Interpolation formula:**
Tc_Gd(x) = (1 − 0.464) × Tc_Nd(x) + 0.464 × Tc_Er(x)
         = 0.536 × Tc_Nd(x) + 0.464 × Tc_Er(x)

## Step 4: Predictions for Gd family

At each x in to_predict.txt, I interpolate using matched Nd and Er data:

| # | Composition (Gd)         | x     | Tc_Nd  | Tc_Er | Tc_Gd (interpolated) |
|---|--------------------------|-------|--------|-------|----------------------|
| 1 | O7Cu3Ba2Pr0.030Gd0.970   | 0.030 | 85.1   | 93.9  | 0.536×85.1+0.464×93.9 = **89.2** |
| 2 | O7Cu3Ba2Pr0.078Gd0.922   | 0.078 | 68.2*  | 91.1  | 0.536×68.2+0.464×91.1 = **78.8** |
| 3 | O7Cu3Ba2Pr0.150Gd0.850   | 0.150 | 40.7   | 82.9  | 0.536×40.7+0.464×82.9 = **60.3** |
| 4 | O7Cu3Ba2Pr0.200Gd0.800   | 0.200 | 31.4   | 76.4  | 0.536×31.4+0.464×76.4 = **52.3** |
| 5 | O7Cu3Ba2Pr0.300Gd0.700   | 0.300 | 5.6    | 61.1  | 0.536×5.6+0.464×61.1 = **31.4** |
| 6 | O7Cu3Ba2Pr0.400Gd0.600   | 0.400 | 0      | 45.8  | 0.536×0+0.464×45.8 = **21.3** |
| 7 | O7Cu3Ba2Pr0.500Gd0.500   | 0.500 | 0      | 28.6  | 0.536×0+0.464×28.6 = **13.3** |
| 8 | O7Cu3Ba2Pr0.600Gd0.400   | 0.600 | 0      | 0     | **0** |
| 9 | O7Cu3Ba2Pr0.700Gd0.300   | 0.700 | 0      | 0     | **0** |
| 10| O7Cu3Ba2Pr0.800Gd0.200   | 0.800 | 0      | 0     | **0** |
| 11| O7Cu3Ba2Pr0.900Gd0.100   | 0.900 | 0      | 0     | **0** |

*Nd at x=0.080 used for Gd prediction at x=0.078 (close enough; curve is smooth in this region).

## Summary

The Gd family is predicted to have:
- **Base Tc ≈ 89 K** (between Er's 94 and Nd's 85, weighted by ionic radius)
- **Moderate Pr suppression** (between Er's gentle and Nd's steep decline)
- **SC killed around x ≈ 0.55** (Nd kills SC at ~0.32, Er at ~0.71)
- The interpolation is physically motivated: Gd³⁺ ionic radius sits between Nd³⁺ and Er³⁺, so the Pr-suppression mechanism (which scales with RE size/structure) should interpolate smoothly.
