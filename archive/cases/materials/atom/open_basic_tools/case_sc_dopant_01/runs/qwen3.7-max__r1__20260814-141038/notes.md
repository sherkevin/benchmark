# Notes: Predicting Tc of Gd-series RE(1-x)Pr(x)Ba2Cu3O7 cuprates

## Discovery

Searched the database with `search_db` for all rare-earth elements. Found three
RE families in the `RE(1-x)Pr(x)Ba2Cu3O7` structure:

- **Nd** (Z=60): 10 compositions (Pr = 0.030 – 0.900)
- **Gd** (Z=64): 11 compositions (Pr = 0.030 – 0.900) — NOT synthesized
- **Er** (Z=68): 9 compositions (Pr = 0.030 – 0.900)

Total 30 compositions in the `O7Cu3Ba2...` family.

## Measured Data

### Nd series (synthesized — all measured)

| Pr content (x) | Tc (K) |
|---|---|
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

### Er series (synthesized — all measured)

| Pr content (x) | Tc (K) |
|---|---|
| 0.030 | 93.9 |
| 0.076 | 91.1 |
| 0.150 | 82.9 |
| 0.200 | 76.4 |
| 0.300 | 61.1 |
| 0.400 | 45.8 |
| 0.500 | 28.6 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

### Gd series — confirmed NOT synthesized

`measure_tc(O7Cu3Ba2Pr0.030Gd0.970)` → `sample_available: false`

## Pattern Analysis

### Key observation: Tc suppression rate depends on the RE

Pr substitution suppresses Tc much faster for Nd than for Er:

- **Nd**: Tc drops steeply — reaches 0 K by x ≈ 0.32 (extrapolated from linear fit of first 5 points: Tc ≈ 89.4 − 282.4·x)
- **Er**: Tc drops slowly — reaches 0 K by x ≈ 0.65 (extrapolated: Tc ≈ 97.0 − 103.0·x)

### Physical reasoning

The suppression rate correlates with the RE ionic radius and 4f-orbital extent.
Nd³⁺ (ionic radius ~1.109 Å in VIII-coordination) has more spatially extended 4f
orbitals than Er³⁺ (~1.004 Å), leading to stronger hybridization between Pr-4f
and CuO₂-plane electrons. Gd³⁺ (~1.053 Å) sits almost exactly midway between
Nd and Er (fraction = (1.109−1.053)/(1.109−1.004) = 0.533 ≈ 0.5).

### Prediction method: linear interpolation in atomic number

Since Gd (Z=64) sits at the midpoint between Nd (Z=60) and Er (Z=68), and
Gd³⁺ ionic radius is nearly the midpoint of Nd³⁺ and Er³⁺, the Gd Tc at each
Pr content x is predicted as:

    Tc_Gd(x) ≈ [Tc_Nd(x) + Tc_Er(x)] / 2

For Pr contents where Nd and Er have slightly different x values (x ≈ 0.078),
I first linearly interpolated Tc_Nd and Tc_Er to x = 0.078, then averaged.

### Detailed predictions

**x = 0.030**: Tc_Nd = 85.1, Tc_Er = 93.9 → Gd = (85.1+93.9)/2 = **89.5 K**

**x = 0.078**: Nd and Er don't have exact x=0.078 points. Interpolated:
- Tc_Nd(0.078): between (0.030, 85.1) and (0.080, 68.2) → 85.1 + (0.048/0.050)×(68.2−85.1) = 68.9
- Tc_Er(0.078): between (0.030, 93.9) and (0.076, 91.1) → 93.9 + (0.048/0.046)×(91.1−93.9) = 91.0
- Gd = (68.9+91.0)/2 = **79.9 K**

**x = 0.150**: Tc_Nd = 40.7, Tc_Er = 82.9 → Gd = (40.7+82.9)/2 = **61.8 K**

**x = 0.200**: Tc_Nd = 31.4, Tc_Er = 76.4 → Gd = (31.4+76.4)/2 = **53.9 K**

**x = 0.300**: Tc_Nd = 5.6, Tc_Er = 61.1 → Gd = (5.6+61.1)/2 = **33.4 K**

**x = 0.400**: Tc_Nd = 0.0 (extrapolated; Nd is already non-SC), Tc_Er = 45.8
→ Gd = (0+45.8)/2 = **22.9 K**

**x = 0.500**: Tc_Nd = 0.0, Tc_Er = 28.6 → Gd = (0+28.6)/2 = **14.3 K**

**x ≥ 0.600**: Gd is near or past its critical Pr concentration (~0.48–0.57,
interpolated between Nd's ~0.32 and Er's ~0.65). → **0 K** (non-superconducting)

## Consistency check

The predicted initial suppression rate for Gd:
- dTc/dx|_Gd ≈ (89.5 − 33.4) / (0.300 − 0.030) = 207.8 K per unit Pr
- dTc/dx|_Nd ≈ (85.1 − 5.6) / (0.300 − 0.030) = 294.4 K per unit Pr
- dTc/dx|_Er ≈ (93.9 − 61.1) / (0.300 − 0.030) = 121.5 K per unit Pr
- Average of Nd and Er slopes: (294.4+121.5)/2 = 208.0 ≈ 207.8 ✓

The Gd suppression rate is exactly the average of Nd and Er — confirming the
linear interpolation is self-consistent.
