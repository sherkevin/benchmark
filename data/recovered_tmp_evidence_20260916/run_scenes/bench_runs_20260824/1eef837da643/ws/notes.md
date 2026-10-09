# Notes on Predicting Tc for Gd-series Compositions

## Discovery Phase

Using search_db, I found 30 compositions in the RE(1-x)Pr(x)Ba2Cu3O7 family:

### Erbium (Er) Series (9 compositions):
- O7Cu3Ba2Pr0.030Er0.970
- O7Cu3Ba2Pr0.076Er0.924  
- O7Cu3Ba2Pr0.150Er0.850
- O7Cu3Ba2Pr0.200Er0.800
- O7Cu3Ba2Pr0.300Er0.700
- O7Cu3Ba2Pr0.400Er0.600
- O7Cu3Ba2Pr0.500Er0.500
- O7Cu3Ba2Pr0.800Er0.200
- O7Cu3Ba2Pr0.900Er0.100

### Gadolinium (Gd) Series (11 compositions):
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

### Neodymium (Nd) Series (10 compositions):
- O7Cu3Ba2Pr0.030Nd0.970
- O7Cu3Ba2Pr0.080Nd0.920
- O7Cu3Ba2Pr0.150Nd0.850
- O7Cu3Ba2Pr0.200Nd0.800
- O7Cu3Ba2Pr0.300Nd0.700
- O7Cu3Ba2Pr0.500Nd0.500
- O7Cu3Ba2Pr0.600Nd0.400
- O7Cu3Ba2Pr0.700Nd0.300
- O7Cu3Ba2Pr0.800Nd0.200
- O7Cu3Ba2Pr0.900Nd0.100

According to the task, the Gd series has not been synthesized yet, so I need to measure Tc for Er and Nd series to understand the pattern, then predict Tc for Gd series.

## Measurements

### Er Series Tc Measurements:
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K  
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K (no superconductivity)
- O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K (no superconductivity)

### Nd Series Tc Measurements:
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K (no superconductivity)
- O7Cu3Ba2Pr0.600Nd0.400: Tc = 0.0 K (no superconductivity)
- O7Cu3Ba2Pr0.700Nd0.300: Tc = 0.0 K (no superconductivity)
- O7Cu3Ba2Pr0.800Nd0.200: Tc = 0.0 K (no superconductivity)
- O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K (no superconductivity)

### Gd Series (Not Synthesized):
- All Gd compositions return `sample_available: false` when measured
- Confirmed with O7Cu3Ba2Pr0.030Gd0.970: `sample_available: false`

## Pattern Analysis

### Key Observations:
1. **Tc decreases with increasing Pr concentration** for both Er and Nd series
2. **Er series has higher Tc at all Pr concentrations** compared to Nd series
3. **Superconductivity persists to higher Pr concentrations in Er series**:
   - Er: superconductivity up to x=0.5 (Tc=28.6 K at x=0.5)
   - Nd: superconductivity only up to x=0.3 (Tc=5.6 K at x=0.3, 0 K at x=0.5)

### Interpolation Strategy:
Gd (atomic number 64) is between Er (68) and Nd (60) in the lanthanide series. Given that:
- Er (higher atomic number) has higher Tc
- Nd (lower atomic number) has lower Tc
- Gd should have Tc values intermediate between Er and Nd

I will predict Gd Tc by linear interpolation between Er and Nd values at each Pr concentration. For Pr concentrations where both Er and Nd have superconductivity, I'll interpolate. For concentrations where only Er has superconductivity, I'll extrapolate from the trend. For concentrations where neither has superconductivity, Gd will also have Tc=0.

## Predictions for Gd Series

Based on interpolation between Er and Nd measurements:

1. **O7Cu3Ba2Pr0.030Gd0.970**: (93.9 + 85.1)/2 = **89.5 K**
2. **O7Cu3Ba2Pr0.078Gd0.922**: (91.1 + 68.2)/2 = **79.65 K** (using Er at 0.076 and Nd at 0.080)
3. **O7Cu3Ba2Pr0.150Gd0.850**: (82.9 + 40.7)/2 = **61.8 K**
4. **O7Cu3Ba2Pr0.200Gd0.800**: (76.4 + 31.4)/2 = **53.9 K**
5. **O7Cu3Ba2Pr0.300Gd0.700**: (61.1 + 5.6)/2 = **33.35 K**
6. **O7Cu3Ba2Pr0.400Gd0.600**: Er=45.8 K, Nd=0 K → **22.9 K** (estimated as half of Er value)
7. **O7Cu3Ba2Pr0.500Gd0.500**: Er=28.6 K, Nd=0 K → **14.3 K** (estimated as half of Er value)
8. **O7Cu3Ba2Pr0.600Gd0.400**: Both series show 0 K at nearby concentrations → **0 K**
9. **O7Cu3Ba2Pr0.700Gd0.300**: Both series show 0 K at nearby concentrations → **0 K**
10. **O7Cu3Ba2Pr0.800Gd0.200**: Both series show 0 K → **0 K**
11. **O7Cu3Ba2Pr0.900Gd0.100**: Both series show 0 K → **0 K**

**Note**: For x ≥ 0.6, both Er and Nd series show no superconductivity (Tc=0), so Gd should also have Tc=0.

**Validation**: 
- No Er composition exists at x=0.600 or x=0.700 in the database
- Er at x=0.800 has Tc=0 K
- Nd at x=0.600 and x=0.700 has Tc=0 K
- Given the downward trend with increasing Pr concentration, predicting Tc=0 K for Gd at x≥0.6 is reasonable

## Final Predictions
All 11 Gd-series compositions have been predicted based on interpolation between measured Er and Nd Tc values, with Tc=0 for compositions where both parent series lose superconductivity.