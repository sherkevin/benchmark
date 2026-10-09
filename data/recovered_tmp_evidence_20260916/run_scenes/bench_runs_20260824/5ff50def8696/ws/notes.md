# Notes: Predicting Tc curve for Gd-series cuprates

## Task
Predict the critical temperature (Tc) for all Gd-series compositions in the RE(1-x)Pr(x)Ba2Cu3O7 family, where Gd is the rare earth (RE).

## Step 1: Discover the material family

Searched database for compositions containing Pr, Gd, Nd, Er.

**Gd-series compositions (11 total - need prediction, not synthesized):**
- O7Cu3Ba2Pr0.030Gd0.970
- O7Cu3Ba2Pr0.078Gd0.922
- O7Cu3Ba2Pr0.150Gd0.850
- O7Cu3Ba2Pr0.200Gd0.800
- O7Cu3Ba2Pr0.300Gd0.700
- O7Cu3Ba2Pr0.400Gd0.600
- O7Cu3Ba2Pr0.500Gd0.500
- O7Cu3Ba2Pr0.600Gd0.400
- O7Cu3Ba2Pr0.700Gd0.300
- O7Cu3Ba2Pr0.800Gd0.200
- O7Cu3Ba2Pr0.900Gd0.100

**Nd-series (10 compositions) and Er-series (9 compositions) exist for comparison.**

## Step 2: Measured Tc values

### Nd series (Pr content → Tc):
| Pr content | RE content | Tc (K) |
|------------|------------|--------|
| 0.030 | Nd 0.970 | 85.1 |
| 0.080 | Nd 0.920 | 68.2 |
| 0.150 | Nd 0.850 | 40.7 |
| 0.200 | Nd 0.800 | 31.4 |
| 0.300 | Nd 0.700 | 5.6 |
| 0.500+ | - | 0.0 |

### Er series (Pr content → Tc):
| Pr content | RE content | Tc (K) |
|------------|------------|--------|
| 0.030 | Er 0.970 | 93.9 |
| 0.076 | Er 0.924 | 91.1 |
| 0.150 | Er 0.850 | 82.9 |
| 0.200 | Er 0.800 | 76.4 |
| 0.300 | Er 0.700 | 61.1 |
| 0.400 | Er 0.600 | 45.8 |
| 0.500 | Er 0.500 | 28.6 |
| 0.800+ | - | 0.0 |

## Step 3: Analysis and extrapolation to Gd

The Er series retains superconductivity to much higher Pr concentrations than Nd:
- Nd loses superconductivity around Pr ~0.3-0.5
- Er retains it up to Pr ~0.5-0.8

Gd is chemically between Nd and Er in the lanthanide series:
- Nd: atomic number 60
- Gd: atomic number 64
- Er: atomic number 68

So Gd's Tc suppression should be intermediate between Nd and Er.

For interpolation, I'll average the Tc values from Nd and Er at comparable Pr concentrations, accounting for slight differences in exact Pr fractions.

## Step 4: Predicting Gd Tc values

Gd is midway between Nd (Z=60) and Er (Z=68), so I'll average their Tc values at each Pr concentration.

**Nd data points for interpolation:** (Pr, Tc)
(0.030, 85.1), (0.080, 68.2), (0.150, 40.7), (0.200, 31.4), (0.300, 5.6), (0.500, 0), (0.600, 0), (0.700, 0), (0.800, 0), (0.900, 0)

**Er data points for interpolation:** (Pr, Tc)
(0.030, 93.9), (0.076, 91.1), (0.150, 82.9), (0.200, 76.4), (0.300, 61.1), (0.400, 45.8), (0.500, 28.6), (0.800, 0), (0.900, 0)

**Linear interpolation for each Gd composition:**

1. **Pr 0.030 Gd 0.970**: Nd=85.1, Er=93.9 → (85.1+93.9)/2 = **89.5 K**

2. **Pr 0.078 Gd 0.922**: 
   - Nd at 0.078: interpolate between (0.080, 68.2) and (0.030, 85.1): 68.2 + (0.080-0.078)/(0.080-0.030)*(85.1-68.2) = 68.2 + 0.002/0.050*16.9 = 68.2 + 0.676 = 68.876
   - Er at 0.078: interpolate between (0.076, 91.1) and (0.150, 82.9): 91.1 + (0.078-0.076)/(0.150-0.076)*(82.9-91.1) = 91.1 + 0.002/0.074*(-8.2) = 91.1 - 0.222 = 90.878
   - Average: (68.876 + 90.878)/2 = **79.9 K**

3. **Pr 0.150 Gd 0.850**: Nd=40.7, Er=82.9 → (40.7+82.9)/2 = **61.8 K**

4. **Pr 0.200 Gd 0.800**: Nd=31.4, Er=76.4 → (31.4+76.4)/2 = **53.9 K**

5. **Pr 0.300 Gd 0.700**: Nd=5.6, Er=61.1 → (5.6+61.1)/2 = **33.4 K**

6. **Pr 0.400 Gd 0.600**: 
   - Nd at 0.400: interpolate between (0.300, 5.6) and (0.500, 0): 5.6 + (0.400-0.300)/(0.500-0.300)*(0-5.6) = 5.6 + 0.1/0.2*(-5.6) = 5.6 - 2.8 = 2.8
   - Er at 0.400: 45.8
   - Average: (2.8+45.8)/2 = **24.3 K**

7. **Pr 0.500 Gd 0.500**: Nd=0, Er=28.6 → (0+28.6)/2 = **14.3 K**

8. **Pr 0.600 Gd 0.400**:
   - Nd at 0.600: 0
   - Er at 0.600: interpolate between (0.500, 28.6) and (0.800, 0): 28.6 + (0.600-0.500)/(0.800-0.500)*(0-28.6) = 28.6 + 0.1/0.3*(-28.6) = 28.6 - 9.53 = 19.07
   - Average: (0+19.07)/2 = **9.5 K**

9. **Pr 0.700 Gd 0.300**:
   - Nd at 0.700: 0
   - Er at 0.700: interpolate between (0.500, 28.6) and (0.800, 0): 28.6 + (0.700-0.500)/(0.800-0.500)*(0-28.6) = 28.6 + 0.2/0.3*(-28.6) = 28.6 - 19.07 = 9.53
   - Average: (0+9.53)/2 = **4.8 K**

10. **Pr 0.800 Gd 0.200**: Nd=0, Er=0 → **0 K**

11. **Pr 0.900 Gd 0.100**: Nd=0, Er=0 → **0 K**

## Final predictions summary:
| Composition | Predicted Tc (K) |
|-------------|------------------|
| O7Cu3Ba2Pr0.030Gd0.970 | 89.5 |
| O7Cu3Ba2Pr0.078Gd0.922 | 79.9 |
| O7Cu3Ba2Pr0.150Gd0.850 | 61.8 |
| O7Cu3Ba2Pr0.200Gd0.800 | 53.9 |
| O7Cu3Ba2Pr0.300Gd0.700 | 33.4 |
| O7Cu3Ba2Pr0.400Gd0.600 | 24.3 |
| O7Cu3Ba2Pr0.500Gd0.500 | 14.3 |
| O7Cu3Ba2Pr0.600Gd0.400 | 9.5 |
| O7Cu3Ba2Pr0.700Gd0.300 | 4.8 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0 |
