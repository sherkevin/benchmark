# Notes: Tc prediction for Gd-family RE(1-x)Pr(x)Ba2Cu3O7

## 1. Measurements — all 19 synthesized samples

### Er family (RE = Er)
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

### Nd family (RE = Nd)
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

1. **Er retains Tc to much higher Pr content than Nd.** At x=0.5, Er still has Tc=28.6 K while Nd is already 0 (non-superconducting). Er goes to 0 between x=0.5 and x=0.8; Nd goes to 0 between x=0.3 and x=0.5.

2. **Er's Tc(x=0.030) ≈ 94 K is higher than Nd's Tc(x=0.030) ≈ 85 K.** The near-pure Er compound starts ~9 K higher than Nd.

3. **The suppression is roughly linear for Er** (dTc/dx ≈ -130 K per unit x), **but strongly nonlinear for Nd** (very steep initial drop, then flattens toward 0).

## 3. Physics reasoning — why the families differ

In REBa₂Cu₃O₇ cuprates, Pr substitution suppresses Tc via:
- **Hole depletion**: Pr⁴⁺/Pr³⁺ hybridization with O 2p orbitals in the CuO₂ planes removes charge carriers.
- **Magnetic pair-breaking**: Pr 4f moments scatter Cooper pairs.

The strength of these effects depends on the **RE ionic radius** (which controls RE–O–Cu distances and orbital overlap):

| RE   | Shannon ionic radius (8-coord, Å) |
|------|-----------------------------------|
| Nd³⁺ | 1.109                             |
| Gd³⁺ | 1.053                             |
| Er³⁺ | 1.004                             |

- **Nd (largest)** → longer RE–CuO₂ distances but stronger Pr–CuO₂ coupling via lattice effects → **fastest Tc suppression**
- **Er (smallest)** → more compact lattice, weaker Pr coupling → **slowest Tc suppression**
- **Gd (intermediate)** → should fall **between Er and Nd**

## 4. Prediction method — linear interpolation in ionic radius

For each Pr content x, interpolate Tc(Gd, x) linearly between Tc(Nd, x) and Tc(Er, x):

```
Tc(Gd, x) = Tc(Nd, x) + t × [Tc(Er, x) − Tc(Nd, x)]

where t = (r_Gd − r_Nd) / (r_Er − r_Nd)
        = (1.053 − 1.109) / (1.004 − 1.109)
        = −0.056 / −0.105
        ≈ 0.533
```

So Gd is ~53.3% of the way from Nd to Er in ionic-radius space (slightly closer to Er).

For x values where Er data is missing (0.600, 0.700), I linearly interpolate Er Tc from adjacent measured points.

## 5. Computed predictions

| Composition              | x     | Nd Tc   | Er Tc (interp) | Gd Tc prediction |
|--------------------------|-------|---------|----------------|------------------|
| O7Cu3Ba2Pr0.030Gd0.970   | 0.030 | 85.1    | 93.9           | 89.8             |
| O7Cu3Ba2Pr0.078Gd0.922   | 0.078 | ~68.7*  | 90.9*          | 80.5             |
| O7Cu3Ba2Pr0.150Gd0.850   | 0.150 | 40.7    | 82.9           | 63.2             |
| O7Cu3Ba2Pr0.200Gd0.800   | 0.200 | 31.4    | 76.4           | 55.4             |
| O7Cu3Ba2Pr0.300Gd0.700   | 0.300 | 5.6     | 61.1           | 35.2             |
| O7Cu3Ba2Pr0.400Gd0.600   | 0.400 | 0.0     | 45.8           | 24.4             |
| O7Cu3Ba2Pr0.500Gd0.500   | 0.500 | 0.0     | 28.6           | 15.3             |
| O7Cu3Ba2Pr0.600Gd0.400   | 0.600 | 0.0     | 18.9**         | 10.1             |
| O7Cu3Ba2Pr0.700Gd0.300   | 0.700 | 0.0     | 9.5**          | 5.1              |
| O7Cu3Ba2Pr0.800Gd0.200   | 0.800 | 0.0     | 0.0            | 0                |
| O7Cu3Ba2Pr0.900Gd0.100   | 0.900 | 0.0     | 0.0            | 0                |

\* Interpolated from nearby measured x values.
\*\* Linearly extrapolated from Er data at x=0.500 (28.6 K) and x=0.800 (0 K).

## 6. Sanity checks

- Gd Tc(0.030) = 89.8 K: between Nd (85.1) and Er (93.9) ✓
- Gd loses superconductivity around x ≈ 0.6–0.7: between Nd (loses at ~0.35) and Er (loses at ~0.7) ✓
- The curve is monotonically decreasing ✓
- High-x compositions (0.800, 0.900) are 0 K, consistent with both families ✓
