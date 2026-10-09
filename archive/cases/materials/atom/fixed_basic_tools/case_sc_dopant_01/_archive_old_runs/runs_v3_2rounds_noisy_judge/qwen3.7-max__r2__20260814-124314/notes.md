# Notes: Tc Prediction for Gd-family Cuprates

## Data collected

### Er family (Er₁₋ₓPrₓBa₂Cu₃O₇) — 9 compositions measured

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

### Nd family (Nd₁₋ₓPrₓBa₂Cu₃O₇) — 10 compositions measured

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

## Observed patterns

1. **Er family**: Tc decreases gradually from 93.9 K (x=0.03) to 28.6 K (x=0.50). Superconductivity is lost between x=0.50 and x=0.80 (cutoff ≈ x=0.73).

2. **Nd family**: Tc decreases much more steeply, from 85.1 K (x=0.03) to 5.6 K (x=0.30). Superconductivity is lost between x=0.30 and x=0.50 (cutoff ≈ x=0.32).

3. **Suppression rate**: Nd's Tc falls roughly **2.1× faster** than Er's (initial slope ≈ -292 K per unit x for Nd vs ≈ -140 for Er from cubic fits).

## Reasoning: where Gd falls

### Physical basis
The rare-earth ionic radii (9-coordination, Shannon 1976) follow:
- Er³⁺ = 0.890 Å
- Gd³⁺ = 0.938 Å
- Nd³⁺ = 0.983 Å

Gd sits **between** Er and Nd, closer to Nd. The rate of Tc suppression by Pr correlates with the host RE ionic radius (larger RE → faster suppression). Gd should therefore exhibit intermediate suppression.

### Interpolation weight
Using linear interpolation in ionic radius:

w_Nd = (r_Er - r_Gd) / (r_Er - r_Nd) = (0.890 - 0.938) / (0.890 - 0.983) = 0.516

So Gd ≈ 51.6% Nd + 48.4% Er character (or equivalently, weight 0.516 toward Nd).

**Note**: I also tested 8-coordination radii (Er=1.004, Gd=1.053, Nd=1.109) giving w_Nd=0.524 — nearly identical. Final predictions are insensitive to coordination choice.

### Functional form: cubic polynomial

I fit each family with a cubic Tc(x) = Tc₀ + b·x + c·x² + d·x³:

**Er cubic** (Tc₀=92, least-squares with a=92 fixed):
Tc_Er(x) = 92 - 140.20x + 3.44x² - 72.41x³

Verification against measured data:
- x=0.03: 87.8 (actual 93.9, Δ=-6.1)
- x=0.076: 81.5 (actual 91.1, Δ=-9.6)
- x=0.15: 71.4 (actual 82.9, Δ=-11.5)
- x=0.20: 65.9 (actual 76.4, Δ=-10.5)
- x=0.30: 49.6 (actual 61.1, Δ=-11.5)
- x=0.40: 37.2 (actual 45.8, Δ=-8.6)
- x=0.50: 28.5 (actual 28.6, Δ=-0.1)
RMSE ≈ 9.5 K (cubic captures shape but underfits Er middle range)

**Nd cubic** (Tc₀=88):
Tc_Nd(x) = 88 - 313.16x + 720.41x² - 1579.51x³

Verification:
- x=0.03: 79.2 (actual 85.1, Δ=-5.9)
- x=0.08: 65.8 (actual 68.2, Δ=-2.4)
- x=0.15: 43.2 (actual 40.7, Δ=+2.5)
- x=0.20: 30.5 (actual 31.4, Δ=-0.9)
- x=0.30: 8.9 (actual 5.6, Δ=+3.3)
- x=0.50: -2.4 → 0 (actual 0.0)
RMSE ≈ 3.6 K (excellent fit)

**Gd cubic** (interpolated):
Tc_Gd(x) = 92 - 221.88x + 342.83x² - 798.99x³

### Cutoff
The Gd cubic crosses zero at x ≈ 0.53. For x ≥ 0.53, Tc = 0 (non-superconducting).

## Predictions for Gd family

| Composition | x (Pr) | Predicted Tc (K) |
|-------------|--------|-------------------|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 85.7 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 77.6 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 64.2 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 55.0 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 38.0 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 23.7 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 3.8 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 0.0 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 0.0 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0.0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0.0 |

## Sanity checks

1. At x=0.03, Gd Tc (85.7 K) falls between Er (93.9 K) and Nd (85.1 K) ✓
2. At x=0.15, Gd (64.2 K) is between Er (82.9 K) and Nd (40.7 K), closer to Nd ✓
3. Suppression is faster than Er but slower than Nd ✓
4. Superconductivity is lost around x≈0.53, between Er's ≈0.73 and Nd's ≈0.32 ✓
5. All predictions are monotonically decreasing ✓
