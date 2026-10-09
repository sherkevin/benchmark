# Notes: Predicting Tc for Gd-series cuprates

## Database Discovery

Searched the materials database and found:
- **Gd series**: 11 compositions (O7Cu3Ba2Pr(x)Gd(1-x), x = 0.03 to 0.90) - NOT synthesized
- **Er series**: 9 compositions - synthesized, measured
- **Nd series**: 10 compositions - synthesized, measured

## Measured Tc Data

### Er series (Pr fraction x → Tc in K)
| x | RE fraction | Tc (K) |
|---|-------------|--------|
| 0.030 | Er0.970 | 93.9 |
| 0.076 | Er0.924 | 91.1 |
| 0.150 | Er0.850 | 82.9 |
| 0.200 | Er0.800 | 76.4 |
| 0.300 | Er0.700 | 61.1 |
| 0.400 | Er0.600 | 45.8 |
| 0.500 | Er0.500 | 28.6 |
| 0.800 | Er0.200 | 0.0 |
| 0.900 | Er0.100 | 0.0 |

### Nd series (Pr fraction x → Tc in K)
| x | RE fraction | Tc (K) |
|---|-------------|--------|
| 0.030 | Nd0.970 | 85.1 |
| 0.080 | Nd0.920 | 68.2 |
| 0.150 | Nd0.850 | 40.7 |
| 0.200 | Nd0.800 | 31.4 |
| 0.300 | Nd0.700 | 5.6 |
| 0.500 | Nd0.400 | 0.0 |
| 0.600 | Nd0.300 | 0.0 |
| 0.700 | Nd0.200 | 0.0 |
| 0.800 | Nd0.100 | 0.0 |
| 0.900 | Nd0.100 | 0.0 |

## Pattern Analysis

1. **Er series**: Gradual Tc suppression. Critical Pr concentration xc ≈ 0.6 (Tc→0)
2. **Nd series**: Rapid Tc suppression. Critical Pr concentration xc ≈ 0.35-0.4 (Tc→0)

The difference arises from different RE electronic/magnetic properties affecting:
- Baseline Tc (pure RE compound)
- Pr suppression rate (pair-breaking strength)

## Gd Prediction Reasoning

Gd in cuprate superconductors is known to have:
- High baseline Tc (~92-93K for pure GdBa2Cu3O7)
- Strong magnetic pair-breaking from Gd 4f moments
- Suppression rate comparable to or faster than Nd

Using Nd as the closest analog (both have strong magnetic moments), but accounting for:
- Slightly higher baseline Tc for Gd vs Nd (pure Gd ~93K vs pure Nd ~87K)
- Similar or slightly faster suppression due to Gd's magnetic moment

### Predicted Tc for Gd series

| Composition | Pr (x) | Predicted Tc (K) | Rationale |
|-------------|--------|------------------|-----------|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 88.0 | High baseline, small suppression |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 62.0 | Steep drop, similar to Nd at x=0.08 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 32.0 | Following Nd trend (40.7K) but faster suppression |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 18.0 | Nd has 31.4K; Gd suppresses faster |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 2.0 | Near critical point; Nd has 5.6K |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 0.0 | Beyond critical xc |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 0.0 | Well beyond critical xc |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 0.0 | No superconductivity |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 0.0 | No superconductivity |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0.0 | No superconductivity |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0.0 | No superconductivity |

## Conclusion

The Gd series Tc curve follows a steep suppression pattern similar to Nd but with:
- Slightly higher starting Tc (~88K vs ~85K at x=0.03)
- Critical Pr concentration xc ≈ 0.35 (superconductivity destroyed for x ≥ 0.40)
