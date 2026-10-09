# Notes on Tc Prediction for Gd-Based Cuprates

## Data Collected

### Er-based compositions (with measured Tc):
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K

### Nd-based compositions (with measured Tc):
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
- O7Cu3Ba2Pr0.800Nd0.200: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K

### Gd-based compositions (not synthesized, need prediction):
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

## Analysis

Looking at the data, I can see that:
1. Both Er and Nd show a decreasing Tc as Pr concentration increases
2. The suppression rate seems to differ between Er and Nd
3. At high Pr concentrations (>0.5), both Er and Nd compounds show Tc ≈ 0
4. The Er-based compounds generally maintain higher Tc values at the same Pr concentrations compared to Nd-based compounds

Based on the periodic table, Gd is between Nd and Er in terms of atomic size and electronic properties. Gd has a slightly smaller ionic radius than Nd but larger than Er. This suggests that Gd-based compounds should have Tc suppression behavior somewhere between Nd and Er.

Looking at the data points:
- At low Pr concentrations (0.030-0.080), Er has higher Tc than Nd (93.9 vs 85.1 K at ~0.030 Pr)
- At moderate Pr concentrations (0.150-0.200), the difference is more pronounced (82.9 vs 40.7 K)
- At high Pr concentrations (>0.500), both show Tc = 0.0 K

I'll estimate the Gd-based Tc values to be roughly halfway between Er and Nd values at lower Pr concentrations, but closer to Er at higher concentrations since Gd is closer in ionic radius to Er than to Nd.

## Predictions

Based on the interpolation between Er and Nd behavior with Gd being closer to Er in ionic radius:
- Low Pr: Gd Tc ≈ average of Er and Nd
- Medium Pr: Gd Tc ≈ weighted towards Er (closer to Er than Nd)
- High Pr: Gd Tc → 0.0 K (like both Er and Nd)