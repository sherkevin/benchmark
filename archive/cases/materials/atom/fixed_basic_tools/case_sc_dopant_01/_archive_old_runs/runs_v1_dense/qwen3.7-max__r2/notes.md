# Notes — Tc prediction for Gd-Pr substitution series

## Measurements (19 of 30 budget used)

### Er series: Er(1-x)Pr(x)Ba2Cu3O7

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

### Nd series: Nd(1-x)Pr(x)Ba2Cu3O7

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

The 6 B(x)C(1-x)O5Cu1Sr2 compositions are a different structure entirely and were not measured.

## Key observations

1. **Both series show monotonic Tc suppression** with increasing Pr content x.
2. **Er resists Pr suppression much better than Nd:** Er maintains Tc ≈ 29 K at x = 0.5, while Nd is already non-superconducting by x = 0.5 (Tc = 0 at x = 0.5, last superconducting at x = 0.3 with Tc = 5.6 K).
3. **Linear model fits well** for both series:
   - Er: Tc(x) = 102.0 − 141.5·x (RMS error ≈ 2.5 K)
   - Nd: Tc(x) = 91.0 − 294.4·x (RMS error ≈ 3.2 K)
4. **Critical Pr content** (where Tc → 0):
   - Er: x_c ≈ 102.0/141.5 = 0.72 (consistent with Tc = 28.6 at x = 0.5 and Tc = 0 at x = 0.8)
   - Nd: x_c ≈ 91.0/294.4 = 0.31 (consistent with Tc = 5.6 at x = 0.3 and Tc = 0 at x = 0.5)

## Interpolation to Gd

**Gd sits between Nd and Er** in the lanthanide series. Using Shannon ionic radii (8-fold coordination):
- Nd³⁺: 1.109 Å
- Gd³⁺: 1.053 Å
- Er³⁺: 1.004 Å

Interpolation fraction: f = (R_Nd − R_Gd)/(R_Nd − R_Er) = (1.109 − 1.053)/(1.109 − 1.004) = 0.533

So Gd is 53.3% of the way from Nd to Er (slightly closer to Er).

**Interpolated Gd parameters:**
- Tc0_Gd = (1 − 0.533) × 91.0 + 0.533 × 102.0 = 42.5 + 54.4 = **96.8 K**
- slope_Gd = (1 − 0.533) × (−294.4) + 0.533 × (−141.5) = −137.5 − 75.4 = **−212.9 K per unit x**
- x_c_Gd = 96.8 / 212.9 = **0.455**

**Gd model:** Tc_Gd(x) = max(0, 96.8 − 212.9·x)

## Final predictions

| Composition                     | x     | Tc (K) |
|---------------------------------|-------|--------|
| O7Cu3Ba2Pr0.030Gd0.970         | 0.030 | 90.4   |
| O7Cu3Ba2Pr0.078Gd0.922         | 0.078 | 80.2   |
| O7Cu3Ba2Pr0.150Gd0.850         | 0.150 | 64.9   |
| O7Cu3Ba2Pr0.200Gd0.800         | 0.200 | 54.2   |
| O7Cu3Ba2Pr0.300Gd0.700         | 0.300 | 32.9   |
| O7Cu3Ba2Pr0.400Gd0.600         | 0.400 | 11.6   |
| O7Cu3Ba2Pr0.500Gd0.500         | 0.500 | 0      |
| O7Cu3Ba2Pr0.600Gd0.400         | 0.600 | 0      |
| O7Cu3Ba2Pr0.700Gd0.300         | 0.700 | 0      |
| O7Cu3Ba2Pr0.800Gd0.200         | 0.800 | 0      |
| O7Cu3Ba2Pr0.900Gd0.100         | 0.900 | 0      |

**Best target:** O7Cu3Ba2Pr0.030Gd0.970 (Tc ≈ 90.4 K — lowest Pr doping gives highest Tc)
**Critical Pr:** 0.400 (highest Pr content in the target series with Tc > 0)

## Budget: 19 measurements used, 11 remaining
