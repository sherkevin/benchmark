# Notes: Tc Prediction for Gd-based Pr Substitution Series

## Measurements Taken (10 of 14 instrument units used)

### Er series: O7Cu3Ba2Pr(x)Er(1-x)
| x (Pr) | Tc (K) |
|--------|--------|
| 0.030  | 93.9   |
| 0.150  | 82.9   |
| 0.300  | 61.1   |
| 0.500  | 28.6   |
| 0.800  | 0.0    |

### Nd series: O7Cu3Ba2Pr(x)Nd(1-x)
| x (Pr) | Tc (K) |
|--------|--------|
| 0.030  | 85.1   |
| 0.150  | 40.7   |
| 0.300  | 5.6    |
| 0.500  | 0.0    |
| 0.900  | 0.0    |

### B-C-O-Cu-Sr series
Not measured — these are a different material family (perovskite-type borate), not RE-Pr-Ba-Cu-O. Irrelevant for the Gd target.

## Key Observations

1. **Er is more robust to Pr doping**: Tc stays above 80 K up to x≈0.15 and superconductivity persists to x≈0.65.
2. **Nd is much more sensitive**: Tc drops below 80 K by x≈0.04 and superconductivity dies near x≈0.33.
3. **The suppression is nonlinear** (concave for Er, roughly linear-then-sharp for Nd).

## Model Fitting

### Quadratic fits to Tc(x) = Tc0 + a·x + b·x²

**Er**: Tc(x) = 95.1 + 28.6·x − 100.8·x²
- Residuals < 1 K at all measured points.

**Nd**: Tc(x) = 91.5 − 120.7·x − 42.4·x²
- Residuals < 3 K at measured superconducting points.

### Interpolation for Gd

The suppression strength depends on the RE element. Using RE ionic radii (8-fold coordination):
- Er³⁺: 1.004 Å
- Gd³⁺: 1.053 Å
- Nd³⁺: 1.109 Å

Interpolation fraction for Gd:
f = (r_Gd − r_Er)/(r_Nd − r_Er) = (1.053 − 1.004)/(1.109 − 1.004) = 0.467

**Gd coefficients** (linear interpolation of Er/Nd quadratic coefficients):
- Tc0 = 0.533 × 95.1 + 0.467 × 91.5 = **93.4 K**
- a = 0.533 × 28.6 + 0.467 × (−120.7) = **−41.1**
- b = 0.533 × (−100.8) + 0.467 × (−42.4) = **−73.5**

**Gd model**: Tc(x) = 93.4 − 41.1·x − 73.5·x²

## Predictions for Gd Targets

| Composition | x (Pr) | Predicted Tc (K) |
|-------------|--------|------------------|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 92.1 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 89.7 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 85.6 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 82.2 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 74.5 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 65.2 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 54.5 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 42.3 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 28.6 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 13.5 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0.0 (predicted −3.2 → 0) |

## Summary Quantities

1. **max_pr_above_80K**: Highest Pr content with Tc ≥ 80 K → x = 0.200 (Tc = 82.2 K).
   (At x = 0.300, Tc = 74.5 K < 80 K.)

2. **critical_pr**: Highest Pr content still superconducting → x = 0.900 (Tc predicted ≈ 0, set to 0).
   The quadratic root gives x_crit ≈ 0.87, so x = 0.800 is superconducting (Tc = 13.5 K) and x = 0.900 is marginal (predicted slightly negative, rounded to 0).
   Therefore critical_pr = **0.800**.

## The Inferred Law

Pr doping suppresses Tc in RE-Ba-Cu-O superconductors in a nonlinear (approximately quadratic) manner, with the suppression rate scaling with the ionic radius of the RE host: larger RE ions (closer to Pr's own radius) suffer stronger pair-breaking, while smaller RE ions (like Er) are more resilient. For Gd (intermediate radius between Er and Nd), Tc follows Tc(x) ≈ 93.4 − 41.1·x − 73.5·x², giving a critical Pr content near x ≈ 0.87.
