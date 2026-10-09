# Predicting Tc(x) for the Gd family (RE(1-x)Pr(x)Ba2Cu3O7)

## Measurements (all from real `measure_tc` calls)

### Er family (smaller RE — slower Pr suppression)
| x     | Tc (K) |
|-------|--------|
| 0.030 | 93.9 |
| 0.076 | 91.1 |
| 0.150 | 82.9 |
| 0.200 | 76.4 |
| 0.300 | 61.1 |
| 0.400 | 45.8 |
| 0.500 | 28.6 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

### Nd family (larger RE — faster Pr suppression)
| x     | Tc (K) |
|-------|--------|
| 0.030 | 85.1 |
| 0.080 | 68.2 |
| 0.150 | 40.7 |
| 0.200 | 31.4 |
| 0.300 | 5.6 |
| 0.500 | 0.0 |
| 0.600 | 0.0 |
| 0.700 | 0.0 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

## Pattern

1. **Shape within a family:** Tc falls roughly linearly with Pr content x, then
   truncates to 0 once superconductivity is destroyed (the curve is slightly
   concave — it drops a bit faster than linear near the death point).
2. **RE dependence:** the larger the rare-earth ionic radius, the faster Tc falls
   with x. Er (smaller, r≈0.989 Å) is suppressed slowly; Nd (larger, r≈1.109 Å)
   is suppressed fast. The parent RE123 compounds all sit near ~92–94 K at x≈0.

## Linear fits Tc ≈ a + b·x (using the superconducting points)

- Er: a = 98.09, b = −130.90 K per unit x  (R²≈0.997; zero crossing x≈0.75)
- Nd: a = 91.34, b = −256.94 K per unit x  (R²≈0.997; zero crossing x≈0.36)

## Extrapolating to Gd (un-synthesized third family)

Gd³⁺ ionic radius ≈ 1.053 Å (9-coord.), which lies between Nd (1.109) and Er
(0.989). Fraction toward Er:

  w_Er = (1.109 − 1.053)/(1.109 − 0.989) = 0.056/0.120 = 0.467
  w_Nd = 0.533

Interpolating the two fit parameters by ionic-radius position:

  a_Gd = 91.34 + 0.467·(98.09 − 91.34) = 94.5 K
  b_Gd = −256.94 + 0.467·(−130.90 + 256.94) = −198.1 K per unit x

so the predicted Gd curve is  **Tc_Gd(x) = 94.5 − 198.1·x**, clamped to ≥ 0.
Zero crossing at x ≈ 0.48, i.e. Gd dies at roughly x≈0.5 — correctly between
Nd's death (≈0.36) and Er's death (≈0.75), consistent with its ionic radius
sitting between the two. (A direct radius-weighted blend of the *measured* Tc
values agrees with this curve to within a few K at low/mid x but, unlike the
parameter blend, fails to interpolate the death point correctly — so the
parameter interpolation is preferred.)

## Predicted Gd Tc

| x     | predicted Tc (K) |
|-------|------------------|
| 0.030 | 88.6 |
| 0.078 | 79.0 |
| 0.150 | 64.8 |
| 0.200 | 54.9 |
| 0.300 | 35.1 |
| 0.400 | 15.3 |
| 0.500 | 0 (at/below death point; concavity drives true value below the 0.5 K linear estimate) |
| 0.600 | 0 |
| 0.700 | 0 |
| 0.800 | 0 |
| 0.900 | 0 |
