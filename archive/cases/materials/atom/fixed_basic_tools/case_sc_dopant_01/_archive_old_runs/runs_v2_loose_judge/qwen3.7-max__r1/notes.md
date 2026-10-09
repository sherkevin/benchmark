# Notes: Predicting Tc for the Gd(1-x)Pr(x)Ba2Cu3O7 series

## 1. Measurements (10 of 14 budget used)

### Er series — Er(1-x)Pr(x)Ba2Cu3O7
| x (Pr) | Tc (K) |
|--------|--------|
| 0.030  | 93.9   |
| 0.150  | 82.9   |
| 0.300  | 61.1   |
| 0.500  | 28.6   |
| 0.800  | 0.0    |

### Nd series — Nd(1-x)Pr(x)Ba2Cu3O7
| x (Pr) | Tc (K) |
|--------|--------|
| 0.030  | 85.1   |
| 0.150  | 40.7   |
| 0.300  | 5.6    |
| 0.500  | 0.0    |
| 0.900  | 0.0    |

### Key observations
- Both RE hosts show monotonic Tc suppression with increasing Pr content.
- Er series: gradual suppression, Tc > 0 up to x ≈ 0.55–0.60.
- Nd series: steep suppression, Tc > 0 only up to x ≈ 0.34.
- Nd suppresses ~2× faster than Er despite having a larger ionic radius.

The 6 remaining synthesized samples (B-C-O-Cu-Sr series) are a completely different material system and are irrelevant to the Gd-REBCO prediction.

## 2. Interpolation strategy

### Physical basis
All compositions share the RE(1-x)Pr(x)Ba2Cu3O7 structure. The RE-site ionic radius varies smoothly across the lanthanide series, and structural parameters of REBCO (lattice constants, bond lengths) scale approximately linearly with RE ionic radius (Vegard's law). For Gd — which sits between Er and Nd in the lanthanide series — linear interpolation by ionic radius is the simplest physically defensible approach.

### Shannon ionic radii (CN = 8, appropriate for the RE site in REBCO)
- Er³⁺: 1.004 Å
- Gd³⁺: 1.053 Å
- Nd³⁺: 1.109 Å

### Interpolation weight
```
w = (r_Gd − r_Er) / (r_Nd − r_Er)
  = (1.053 − 1.004) / (1.109 − 1.004)
  = 0.049 / 0.105
  = 0.467
```

So Tc_Gd(x) = (1 − 0.467) × Tc_Er(x) + 0.467 × Tc_Nd(x)
            = **0.533 × Tc_Er(x) + 0.467 × Tc_Nd(x)**

Gd is approximately midway between Er and Nd, slightly closer to Nd.

## 3. Interpolating within-series values

For target compositions where x doesn't match a measured point, I use piecewise-linear interpolation within each RE series.

### Er series — interpolated values needed:
| x     | Between measured points                | Tc_Er(x) |
|-------|----------------------------------------|----------|
| 0.078 | (0.030, 93.9) → (0.150, 82.9), f=0.40 | 89.5     |
| 0.200 | (0.150, 82.9) → (0.300, 61.1), f=0.33 | 75.6     |
| 0.400 | (0.300, 61.1) → (0.500, 28.6), f=0.50 | 44.9     |
| 0.600 | (0.500, 28.6) → (0.800, 0.0), f=0.33  | 19.1     |
| 0.700 | (0.500, 28.6) → (0.800, 0.0), f=0.67  | 9.5      |

### Nd series — interpolated values needed:
| x     | Between measured points                | Tc_Nd(x) |
|-------|----------------------------------------|----------|
| 0.078 | (0.030, 85.1) → (0.150, 40.7), f=0.40 | 67.3     |
| 0.200 | (0.150, 40.7) → (0.300, 5.6), f=0.33  | 29.0     |
| 0.400 | (0.300, 5.6) → (0.500, 0.0), f=0.50   | 2.8      |
| 0.600 | (0.500, 0.0) → (0.900, 0.0)            | 0.0      |
| 0.700 | (0.500, 0.0) → (0.900, 0.0)            | 0.0      |

## 4. Final predictions for Gd series

Tc_Gd(x) = 0.533 × Tc_Er(x) + 0.467 × Tc_Nd(x)

| Target composition              | x (Pr) | Tc_Er | Tc_Nd | Tc_Gd (K) |
|---------------------------------|--------|-------|-------|-----------|
| O7Cu3Ba2Pr0.030Gd0.970         | 0.030  | 93.9  | 85.1  | 89.8      |
| O7Cu3Ba2Pr0.078Gd0.922         | 0.078  | 89.5  | 67.3  | 79.1      |
| O7Cu3Ba2Pr0.150Gd0.850         | 0.150  | 82.9  | 40.7  | 63.2      |
| O7Cu3Ba2Pr0.200Gd0.800         | 0.200  | 75.6  | 29.0  | 53.8      |
| O7Cu3Ba2Pr0.300Gd0.700         | 0.300  | 61.1  | 5.6   | 35.2      |
| O7Cu3Ba2Pr0.400Gd0.600         | 0.400  | 44.9  | 2.8   | 25.2      |
| O7Cu3Ba2Pr0.500Gd0.500         | 0.500  | 28.6  | 0.0   | 15.2      |
| O7Cu3Ba2Pr0.600Gd0.400         | 0.600  | 19.1  | 0.0   | 10.2      |
| O7Cu3Ba2Pr0.700Gd0.300         | 0.700  | 9.5   | 0.0   | 5.1       |
| O7Cu3Ba2Pr0.800Gd0.200         | 0.800  | 0.0   | 0.0   | 0         |
| O7Cu3Ba2Pr0.900Gd0.100         | 0.900  | 0.0   | 0.0   | 0         |

## 5. Summary quantities

**Best target** (highest Tc):
- `O7Cu3Ba2Pr0.030Gd0.970` with Tc ≈ 89.8 K

**Critical Pr content**:
- Tc > 0 for all x ≤ 0.700 (Tc = 5.1 K)
- Tc = 0 for x ≥ 0.800
- Theoretical critical x where Tc → 0: ~0.75
- **Highest Pr content in the target series still superconducting: 0.700** (Tc = 5.1 K)

## 6. The inferred law

In RE(1-x)Pr(x)Ba2Cu3O7, Pr doping monotonically suppresses Tc toward zero. Both the undoped Tc₀ and the suppression strength depend on the host RE element. For RE ions with intermediate ionic radius (such as Gd, r = 1.053 Å, between Er at 1.004 and Nd at 1.109), Tc at each Pr content is well approximated by linear interpolation (by ionic radius) between the Er and Nd reference series. Gd shows Tc₀ ≈ 90 K and remains superconducting up to Pr content x ≈ 0.75.

## 7. Model uncertainty

The choice of interpolation parameter (ionic radius vs. de Gennes factor vs. magnetic moment) affects the exact predictions. The ionic-radius approach was chosen because lattice parameters in REBCO scale linearly with RE ionic radius, and Pr suppression involves Pr-O hybridization that depends on local bond geometry. However, the Nd series shows anomalously strong suppression relative to Er (faster than expected from ionic radius alone), suggesting additional mechanisms (e.g., magnetic pair-breaking) may contribute. If those mechanisms dominate, the critical Pr content for Gd could shift.
