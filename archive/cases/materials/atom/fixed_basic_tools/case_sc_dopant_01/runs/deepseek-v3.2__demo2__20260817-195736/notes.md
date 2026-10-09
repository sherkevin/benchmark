# Notes: Predicting Tc for RE(1-x) Pr(x) Ba2 Cu3 O7 cuprates

## Materials under study
We have two synthesized rare earth (RE) families:
1. Er (Erbium) family: 9 compositions with varying Pr content
2. Nd (Neodymium) family: 10 compositions with varying Pr content

We need to predict for a third family:
- Gd (Gadolinium) family: 11 compositions with varying Pr content

## Measurements taken

### Erbium (Er) family:
1. O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
2. O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
3. O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
4. O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
5. O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K
6. O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
7. O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K

### Neodymium (Nd) family:
1. O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
2. O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
3. O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K
4. O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
5. O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
6. O7Cu3Ba2Pr0.800Nd0.200: Tc = 0.0 K

## Observations
1. Both families show Tc suppression with increasing Pr content
2. The suppression is much stronger for Nd than for Er:
   - At x=0.030: Er (93.9K) vs Nd (85.1K)
   - At x=0.076: Er (91.1K) vs Nd (68.2K) - Big difference emerging
   - At x=0.080: Nd (68.2K) [no direct Er point]
   - At x=0.150: Er (82.9K) vs Nd (40.7K) - Very different
   - At x=0.200: Er (76.4K) vs Nd (31.4K)
   - At x=0.300: Er (61.1K) vs Nd (5.6K)
   - At x=0.400: Er (45.8K) vs Nd (0K estimated)
   - At x=0.500: Er (28.6K) vs Nd (0K)
   - At x=0.600: Nd (0K)
   - At x=0.800: Both 0K
   - At x=0.900: Both 0K

3. Er shows superconductivity up to at least x=0.500, while Nd loses it between x=0.300 and x=0.500
4. The critical Pr concentration x_crit where Tc=0 appears to be:
   - For Er: between 0.500 and 0.800 (likely ~0.55-0.65)
   - For Nd: between 0.300 and 0.500 (likely ~0.35-0.45)

## Prediction methodology

Based on measurements, I observe that:
1. Pr suppresses Tc in all RE families
2. The suppression strength follows: Nd (strongest) > Er (weakest)
3. Gd (atomic number 64) is between Nd (60) and Er (68)

Using linear interpolation based on atomic number:
- Weight factor = (Gd_atomic_number - Nd_atomic_number) / (Er_atomic_number - Nd_atomic_number)
- = (64 - 60) / (68 - \60) = 4/8 = 0.5

So: Tc_Gd(x) = Tc_Nd(x) + 0.5 * [Tc_Er(x) - Tc_Nd(x)]

## Calculated predictions for overlapping x values:

1. x=0.030: Tc_Gd = 85.1 + 0.5*(93.9-85.1) = 85.1 + 4.4 = 89.5K
2. x=0.076/0.080: Need to interpolate. Using x=0.078 from Gd list:
   - Tc_Er(0.076)=91.1K, Tc_Nd(0.080)=68.2K
   - Linear interpolation in x between 0.076 and 0.080
3. x=0.150: Tc_Gd = 40.7 + 0.5*(82.9-40.7) = 40.7 + 21.1 = 61.8K
4. x=0.200: Tc_Gd = 31.4 + 0.5*(76.4-31.4) = 31.4 + 22.5 = 53.9K
5. x=0.300: Tc_Gd = 5.6 + 0.5*(61.1-5.6) = 5.6 + 27.75 = 33.35K

For other x values, I'll interpolate or extrapolate based on trends.

## Critical Pr concentration estimate:
- Nd: Tc=0 between x=0.300 and x=0.500 (~0.4)
- Er: Tc=0 between x=0.500 and x=0.800 (~0.65)
- Gd: Should be intermediate: ~0.525

Thus, Gd should lose superconductivity around x=0.5-0.6.

## Final predictions for Gd family:
1. O7Cu3Ba2Pr0.030Gd0.970: 89.5 K
2. O7Cu3Ba2Pr0.078Gd0.922: 79.2 K  
3. O7Cu3Ba2Pr0.150Gd0.850: 61.8 K
4. O7Cu3Ba2Pr0.200Gd0.800: 53.9 K
5. O7Cu3Ba2Pr0.300Gd0.700: 33.4 K
6. O7Cu3Ba2Pr0.400Gd0.600: 24.3 K
7. O7Cu3Ba2Pr0.500Gd0.500: 14.3 K
8. O7Cu3Ba2Pr0.600Gd0.400: 4.8 K
9. O7Cu3Ba2Pr0.700Gd0.300: 1.3 K
10. O7Cu3Ba2Pr0.800Gd0.200: 0.0 K
11. O7Cu3Ba2Pr0.900Gd0.100: 0.0 K

## Summary
Measured Tc for Er and Nd families across Pr concentrations, observed Nd suppresses Tc more strongly than Er. Used linear interpolation based on atomic number (Nd=60, Gd=64, Er=68) to predict Gd family behavior. Gd shows intermediate Tc suppression between Nd and Er, losing superconductivity around x=0.5-0.6.