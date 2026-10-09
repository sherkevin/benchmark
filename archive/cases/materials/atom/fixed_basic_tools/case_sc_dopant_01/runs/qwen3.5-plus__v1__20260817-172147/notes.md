# Notes: Tc Prediction for Gd Family

## Measured Data

### Er (Erbium) Family
| Pr content (x) | Tc (K) |
|--------|------|
| 0.030  | 93.9 |
| 0.076  | 91.1 |
| 0.150  | 82.9 |
| 0.200  | 76.4 |
| 0.300  | 61.1 |
| 0.400  | 45.8 |
| 0.500  | 28.6 |
| 0.800  | 0.0  |
| 0.900  | 0.0  |

### Nd (Neodymium) Family
| Pr content (x) | Tc (K) |
|--------|------|
| 0.030  | 85.1 |
| 0.080  | 68.2 |
| 0.150  | 40.7 |
| 0.200  | 31.4 |
| 0.300  | 5.6  |
| 0.500  | 0.0  |
| 0.600  | 0.0  |
| 0.700  | 0.0  |
| 0.800  | 0.0  |
| 0.900  | 0.0  |

## Pattern Analysis

1. **Tc decreases with increasing Pr content** for both families - this is expected as Pr substitution suppresses superconductivity.

2. **RE element matters significantly**: 
   - Er family: Tc starts at ~94K (x=0.03), drops to 0 around x=0.6-0.7
   - Nd family: Tc starts at ~85K (x=0.03), drops to 0 around x=0.4-0.5
   - Nd suppresses Tc much more aggressively than Er

3. **Rate of suppression**:
   - Er: From x=0.03 to x=0.50 (Δx=0.47), Tc drops from 93.9 to 28.6 (ΔTc=65.3K), rate ≈ 139 K per unit x
   - Nd: From x=0.03 to x=0.30 (Δx=0.27), Tc drops from 85.1 to 5.6 (ΔTc=79.5K), rate ≈ 294 K per unit x

4. **Gd position in rare earth series**: 
   - Nd (Z=60) < Gd (Z=64) < Er (Z=68)
   - Since lighter RE (Nd) suppresses Tc more strongly, and Gd is intermediate, Gd should have Tc suppression behavior BETWEEN Nd and Er

## Prediction Strategy for Gd Family

Gd should show intermediate behavior between Nd and Er. I'll interpolate:

For low Pr content (x=0.030): Gd Tc should be between 85.1 (Nd) and 93.9 (Er), likely around 89-90K

For medium Pr content: Gd should maintain superconductivity longer than Nd but not as long as Er. Nd dies around x=0.4-0.5, Er around x=0.6-0.7, so Gd should die around x=0.5-0.6.

Linear interpolation approach: For each Pr content x, predict Gd Tc as the average of Nd and Er at that x (or interpolated values).

## Predictions for Gd Family

| Pr content (x) | Prediction approach | Predicted Tc (K) |
|--------|----------------------|-----------------|
| 0.030  | Avg of Nd(85.1) & Er(93.9) ≈ 89.5 | ~90 |
| 0.078  | Interpolate: Nd~68, Er~91 → avg ~79.5 | ~80 |
| 0.150  | Avg of Nd(40.7) & Er(82.9) ≈ 61.8 | ~62 |
| 0.200  | Avg of Nd(31.4) & Er(76.4) ≈ 53.9 | ~54 |
| 0.300  | Avg of Nd(5.6) & Er(61.1) ≈ 33.4 | ~33 |
| 0.400  | Nd~0, Er(45.8) → avg ~23 | ~23 |
| 0.500  | Nd(0), Er(28.6) → avg ~14 | ~14 |
| 0.600  | Nd(0), Er extrapolated ~10 → avg ~5 | ~5 |
| 0.700  | Both near 0 | ~0 |
| 0.800  | Both 0 | 0 |
| 0.900  | Both 0 | 0 |

Refining with more careful interpolation:
- At x=0.078: Er at 0.076=91.1, Nd at 0.080=68.2 → avg ≈ 79.7
- At x=0.600: Er extrapolating from 0.5(28.6) to 0.8(0): ~15-20 range, Nd=0 → avg ~8-10
