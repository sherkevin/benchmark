# Notes: Predicting Tc of Gd(1-x)Pr(x)Ba2Cu3O7

## Database Discovery
Searched for RE elements in the database. Found three series:
- **Nd** series: 10 compositions (synthesized)
- **Er** series: 9 compositions (synthesized)
- **Gd** series: 11 compositions (NOT synthesized — confirmed sample_available: false)

No Sm, Eu, Y, Dy, Ho, Tm, or La series found.

## Measured Tc Data

### Nd series (RE=Nd, ionic radius 1.109 Å):
| x (Pr) | Tc (K) |
|---------|--------|
| 0.030   | 85.1   |
| 0.080   | 68.2   |
| 0.150   | 40.7   |
| 0.200   | 31.4   |
| 0.300   | 5.6    |
| 0.500   | 0.0    |
| 0.600   | 0.0    |
| 0.700   | 0.0    |
| 0.800   | 0.0    |
| 0.900   | 0.0    |

Nd Tc drops rapidly, reaching 0 around x ≈ 0.32.

### Er series (RE=Er, ionic radius 1.004 Å):
| x (Pr) | Tc (K) |
|---------|--------|
| 0.030   | 93.9   |
| 0.076   | 91.1   |
| 0.150   | 82.9   |
| 0.200   | 76.4   |
| 0.300   | 61.1   |
| 0.400   | 45.8   |
| 0.500   | 28.6   |
| 0.800   | 0.0    |
| 0.900   | 0.0    |

Er Tc drops much more slowly, reaching 0 around x ≈ 0.68.

## Physical Reasoning

The Pr suppression rate depends on the RE ionic radius. Larger RE ions → stronger Pr-O hybridization → faster Tc suppression. Gd³⁺ (ionic radius 1.053 Å) falls between Nd³⁺ (1.109 Å) and Er³⁺ (1.004 Å).

Interpolation weight: f = (r_Gd - r_Er)/(r_Nd - r_Er) = (1.053 - 1.004)/(1.109 - 1.004) = 0.467

So Gd is ~47% of the way from Er to Nd in ionic radius space.

## Model Fitting

Fitted quadratic models Tc(x) = a + bx + cx²:
- Nd: Tc = 95.7 - 359.8x + 198.0x²
- Er: Tc = 96.8 - 92.7x - 87.3x²

Interpolated coefficients for Gd (using f=0.467):
- a_Gd = 0.533×96.8 + 0.467×95.7 = 96.3
- b_Gd = 0.533×(-92.7) + 0.467×(-359.8) = -217.4
- c_Gd = 0.533×(-87.3) + 0.467×198.0 = 46.0

**Gd model: Tc(x) = max(0, 96.3 - 217.4x + 46.0x²)**

This gives Tc=0 at x ≈ 0.495 (between Nd's ~0.32 and Er's ~0.68).

## Predictions for Gd series

| Composition | x | Tc (K) |
|---|---|---|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 89.8 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 79.6 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 64.6 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 54.6 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 35.2 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 16.7 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 0 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 0 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 0 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0 |
