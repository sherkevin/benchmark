# Materials Discovery Task: Predicting Tc for Gd-substituted Cuprates

## Task Overview
Predict the critical temperature (Tc) curve for the Gd series of `RE(1-x)Pr(x)Ba2Cu3O7` cuprates, which have not been synthesized yet. Use synthesized Er and Nd series data to infer the pattern.

## Database Exploration

### Searching for Relevant Compositions
Using `search_db("Pr")`, found 30 compositions following the pattern `O7Cu3Ba2Pr<x>RE<1-x>` where RE ∈ {Er, Gd, Nd}.

**Gd compositions (11 total, not synthesized):**
1. O7Cu3Ba2Pr0.030Gd0.970
2. O7Cu3Ba2Pr0.078Gd0.922  
3. O7Cu3Ba2Pr0.150Gd0.850
4. O7Cu3Ba2Pr0.200Gd0.800
5. O7Cu3Ba2Pr0.300Gd0.700
6. O7Cu3Ba2Pr0.400Gd0.600
7. O7Cu3Ba2Pr0.500Gd0.500
8. O7Cu3Ba2Pr0.600Gd0.400
9. O7Cu3Ba2Pr0.700Gd0.300
10. O7Cu3Ba2Pr0.800Gd0.200
11. O7Cu3Ba2Pr0.900Gd0.100

### Measured Tc Values

**Er (Erbium) series (synthesized):**
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K  
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K

**Nd (Neodymium) series (synthesized):**
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
- O7Cu3Ba2Pr0.600Nd0.400: Tc = 0.0 K
- O7Cu3Ba2Pr0.700Nd0.300: Tc = 0.0 K
- O7Cu3Ba2Pr0.800Nd0.200: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K

**Gd (Gadolinium) series verification:**
- O7Cu3Ba2Pr0.030Gd0.970: sample_available = false (not synthesized)

## Pattern Analysis

### Key Observations:
1. **Suppression strength**: Nd shows stronger Tc suppression with Pr doping compared to Er
2. **Critical Pr concentration**: 
   - Nd loses superconductivity at Pr ≈ 0.500
   - Er loses superconductivity at Pr ≈ 0.800
3. **Atomic number correlation**: Gd (Z=64) is between Nd (Z=60) and Er (Z=68)
4. **Baseline Tc**: Extrapolating to Pr=0, both Nd and Er have T0 ≈ 95K

### Quantitative Comparison:
At Pr=0.030:
- Er: 93.9 K, Nd: 85.1 K → Ratio Nd/Er = 0.907

At Pr=0.150:
- Er: 82.9 K, Nd: 40.7 K → Ratio Nd/Er = 0.491

At Pr=0.300:
- Er: 61.1 K, Nd: 5.6 K → Ratio Nd/Er = 0.092

The ratio decreases rapidly with Pr concentration, indicating Nd is much more sensitive to Pr doping.

## Prediction Methodology

### Approach 1: Linear Interpolation by Atomic Number
Since Gd (Z=64) is midway between Nd (Z=60) and Er (Z=68), use weighted average:
Tc_Gd(x) = 0.5 × Tc_Er(x) + 0.5 × Tc_Nd(x)

### Approach 2: Critical Concentration Interpolation
Interpolate the critical Pr concentration where Tc=0:
- x_crit_Nd ≈ 0.500
- x_crit_Er ≈ 0.800  
- x_crit_Gd (midpoint) ≈ 0.650

Use quadratic decay: Tc(x) = T0 × [1 - (x/x_crit)^2] for x < x_crit

### Chosen Method
I will use **Linear Interpolation by Atomic Number** (Approach 1) because:
1. It uses all available data points directly
2. It doesn't assume a specific functional form for Tc(x)
3. It naturally handles cases where one series has Tc=0

## Predictions for Gd Series

For each Gd composition, predicted Tc = 0.5 × Tc_Er + 0.5 × Tc_Nd

Where Tc_Er and Tc_Nd are the measured values at the same Pr concentration.

For Pr concentrations where I don't have direct measurements:
- Use nearest available measurements
- For Pr=0.078, use Er at 0.076 and Nd at 0.080
- For Pr=0.400, interpolate between Er at 0.300 and 0.500
- For Pr where Tc=0 for both Er and Nd, Tc_Gd=0

## Final Predictions

### Prediction Method Details
For each Pr concentration x:
1. Find Tc_Er(x) and Tc_Nd(x) from measurements
2. For missing values, use linear interpolation between nearest measurements
3. Calculate Tc_Gd(x) = 0.5 × Tc_Er(x) + 0.5 × Tc_Nd(x)
   - Weight 0.5 comes from atomic number position: Gd(64) is midway between Nd(60) and Er(68)

### Gd Series Predictions
1. O7Cu3Ba2Pr0.030Gd0.970: Tc = 0.5×93.9 + 0.5×85.1 = 89.5 K
2. O7Cu3Ba2Pr0.078Gd0.922: Tc = 0.5×91.1 + 0.5×68.2 = 79.65 K (using Er at 0.076 and Nd at 0.080)
3. O7Cu3Ba2Pr0.150Gd0.850: Tc = 0.5×82.9 + 0.5×40.7 = 61.8 K
4. O7Cu3Ba2Pr0.200Gd0.800: Tc = 0.5×76.4 + 0.5×31.4 = 53.9 K
5. O7Cu3Ba2Pr0.300Gd0.700: Tc = 0.5×61.1 + 0.5×5.6 = 33.35 K
6. O7Cu3Ba2Pr0.400Gd0.600: Tc = 0.5×45.8 + 0.5×2.8 = 24.3 K (Nd at 0.400 estimated as 2.8K between 5.6 at 0.300 and 0.0 at 0.500)
7. O7Cu3Ba2Pr0.500Gd0.500: Tc = 0.5×28.6 + 0.5×0.0 = 14.3 K
8. O7Cu3Ba2Pr0.600Gd0.400: Tc = 0.5×22.9 + 0.5×0.0 = 11.45 K (Er at 0.600 estimated as 22.9K between 45.8 at 0.400 and 0.0 at 0.800)
9. O7Cu3Ba2Pr0.700Gd0.300: Tc = 0.5×9.53 + 0.5×0.0 = 4.77 K (Er at 0.700 estimated as 9.53K between 28.6 at 0.500 and 0.0 at 0.800)
10. O7Cu3Ba2Pr0.800Gd0.200: Tc = 0.5×0.0 + 0.5×0.0 = 0.0 K
11. O7Cu3Ba2Pr0.900Gd0.100: Tc = 0.5×0.0 + 0.5×0.0 = 0.0 K

### Key Insights
1. Gd series shows intermediate suppression between Nd and Er
2. Superconductivity persists up to Pr ~0.700-0.800 for Gd
3. The predicted curve smoothly connects the more strongly suppressed Nd series with the more robust Er series
4. All predictions are grounded in actual measurements from synthesized samples