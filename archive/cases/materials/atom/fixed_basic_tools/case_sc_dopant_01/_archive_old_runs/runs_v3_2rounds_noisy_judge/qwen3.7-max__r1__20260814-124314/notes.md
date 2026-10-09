# Notes — Tc prediction for RE(1-x) Pr(x) Ba2 Cu3 O7

## 1. Raw measurements

### Er family (RE = Erbium, Z=68, Shannon VIII radius ≈ 1.004 Å)

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

### Nd family (RE = Neodymium, Z=60, Shannon VIII radius ≈ 1.109 Å)

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

## 2. Observed patterns

### Pr suppresses Tc in both families, but at very different rates
- **Er**: Tc falls gradually. Still 28.6 K at x=0.500. SC lost by x=0.800.
  - Critical Pr content x_c (where Tc→0) ≈ 0.6–0.7
- **Nd**: Tc falls steeply. Only 5.6 K at x=0.300. SC lost by x=0.500.
  - Critical Pr content x_c ≈ 0.32–0.40

### Nd is much more sensitive to Pr substitution than Er
- At x=0.150: Nd Tc=40.7 K vs Er Tc=82.9 K (Er retains 2× more Tc)
- At x=0.300: Nd Tc=5.6 K vs Er Tc=61.1 K (Er retains 10× more Tc)
- Initial slopes: Nd ≈ −265 K/unit-Pr, Er ≈ −46 K/unit-Pr

### Physical interpretation
The "Pr anomaly" in REBa₂Cu₃O₇ is well-known: Pr uniquely suppresses Tc via
4f–O 2p hybridization that localizes holes and possibly introduces pair-breaking.
The strength of this suppression depends on the RE: larger RE ions (Nd, radius
1.109 Å) show stronger Pr suppression; smaller RE ions (Er, 1.004 Å) resist it
better. The smaller RE likely maintains a more compact unit cell that limits
Pr–O hybridization.

## 3. Where does Gd fall?

Gd (Z=64, Shannon VIII radius ≈ 1.053 Å) sits between Nd and Er:

```
Nd (1.109 Å) ——— Gd (1.053 Å) ——————— Er (1.004 Å)
```

Fraction of the way from Nd to Er:
  α = (r_Nd − r_Gd) / (r_Nd − r_Er) = (1.109 − 1.053) / (1.109 − 1.004)
    = 0.056 / 0.105 ≈ 0.40

So Gd is **~40 % of the way from Nd toward Er** in ionic radius space.

## 4. Interpolation method

For each Pr content x, predict:

  **Tc_Gd(x) = 0.40 × Tc_Nd(x) + 0.60 × Tc_Er(x)**

Where needed, Tc_Er and Tc_Nd at non-measured x values are obtained by linear
interpolation between the nearest measured points within each family.

### Calculations for each Gd composition

| Composition              | x    | Tc_Er(x)          | Tc_Nd(x) | 0.40·Nd + 0.60·Er | Tc_Gd |
|--------------------------|------|-------------------|----------|--------------------|-------|
| O7Cu3Ba2Pr0.030Gd0.970  | 0.030| 93.9 (meas)       | 85.1 (meas) | 0.40·85.1+0.60·93.9 = 90.4 | 90.4 |
| O7Cu3Ba2Pr0.078Gd0.922  | 0.078| 90.9 (interp ¹)   | 68.9 (interp ²) | 0.40·68.9+0.60·90.9 = 82.1 | 82.1 |
| O7Cu3Ba2Pr0.150Gd0.850  | 0.150| 82.9 (meas)       | 40.7 (meas) | 0.40·40.7+0.60·82.9 = 66.0 | 66.0 |
| O7Cu3Ba2Pr0.200Gd0.800  | 0.200| 76.4 (meas)       | 31.4 (meas) | 0.40·31.4+0.60·76.4 = 58.4 | 58.4 |
| O7Cu3Ba2Pr0.300Gd0.700  | 0.300| 61.1 (meas)       | 5.6 (meas)  | 0.40·5.6+0.60·61.1 = 38.9  | 38.9 |
| O7Cu3Ba2Pr0.400Gd0.600  | 0.400| 45.8 (meas)       | 0.0 (meas)  | 0.40·0+0.60·45.8 = 27.5    | 27.5 |
| O7Cu3Ba2Pr0.500Gd0.500  | 0.500| 28.6 (meas)       | 0.0 (meas)  | 0.40·0+0.60·28.6 = 17.2    | 17.2 |
| O7Cu3Ba2Pr0.600Gd0.400  | 0.600| 19.1 (interp ³)   | 0.0 (meas)  | 0.40·0+0.60·19.1 = 11.4    | 11.4 |
| O7Cu3Ba2Pr0.700Gd0.300  | 0.700| 9.5 (interp ⁴)    | 0.0 (meas)  | 0.40·0+0.60·9.5 = 5.7      | 5.7  |
| O7Cu3Ba2Pr0.800Gd0.200  | 0.800| 0.0 (meas)        | 0.0 (meas)  | 0.0                         | 0.0  |
| O7Cu3Ba2Pr0.900Gd0.100  | 0.900| 0.0 (meas)        | 0.0 (meas)  | 0.0                         | 0.0  |

**Interpolation details:**
- ¹ Er at x=0.078: between (0.076, 91.1) and (0.150, 82.9) → 91.1 + (0.002/0.074)×(−8.2) ≈ 90.9
- ² Nd at x=0.078: between (0.030, 85.1) and (0.080, 68.2) → 85.1 + (0.048/0.050)×(−16.9) ≈ 68.9
- ³ Er at x=0.600: between (0.500, 28.6) and (0.800, 0.0) → 28.6 + (1/3)×(−28.6) = 19.1
- ⁴ Er at x=0.700: between (0.500, 28.6) and (0.800, 0.0) → 28.6 + (2/3)×(−28.6) = 9.5

## 5. Reasoning summary

Measured all 19 synthesized compositions: the Er family shows gradual Tc
suppression with Pr (Tc stays at 28.6 K at x=0.5, SC lost by x=0.8), while Nd
shows steep suppression (Tc drops to 5.6 K at x=0.3, SC lost by x=0.5). This is
consistent with the known Pr-anomaly physics: larger RE ions (Nd) are more
susceptible to Pr-induced Tc suppression. Gd's ionic radius (1.053 Å) places it
40 % of the way from Nd (1.109 Å) to Er (1.004 Å), so its Tc curve was obtained
by linear interpolation: Tc_Gd(x) = 0.40·Tc_Nd(x) + 0.60·Tc_Er(x). Gd is
predicted to lose superconductivity between x=0.7 and x=0.8.
