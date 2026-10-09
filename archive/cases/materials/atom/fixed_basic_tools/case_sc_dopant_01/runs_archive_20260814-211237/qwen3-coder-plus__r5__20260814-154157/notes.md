# Notes on Tc Prediction for Gd-Based Cuprates

## Measurements Taken

I measured various compositions to understand the relationship between Pr doping and Tc for different RE elements:

### Er-based compositions:
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K

### Nd-based compositions:
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.400Nd0.600: Not available in synthesized.txt
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
- O7Cu3Ba2Pr0.600Nd0.400: Tc = 0.0 K
- O7Cu3Ba2Pr0.700Nd0.300: Tc = 0.0 K
- O7Cu3Ba2Pr0.800Nd0.200: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K

## Observed Pattern

From the measurements, I can see that:
1. Both Er and Nd compositions show decreasing Tc with increasing Pr content
2. The Nd-based compositions show a more rapid decrease in Tc compared to Er-based compositions
3. At Pr0.500, the Nd-based composition has Tc = 0.0 K (no superconductivity), while Er-based still has Tc = 28.6 K
4. At Pr0.800, both Nd and Er-based compositions have Tc = 0.0 K
5. The starting Tc values are different: Er-based starts higher (93.9 K) than Nd-based (85.1 K)
6. The Tc suppression follows a sigmoidal-like curve for both RE elements, but with different sensitivities to Pr doping

## Predicting Gd-based behavior

Based on the periodic table, Gd is between Nd and Er in atomic size and properties. Gd has a slightly smaller ionic radius than Er but larger than Nd. Therefore, I expect the Gd-based compositions to show behavior between that of Er and Nd.

Since Er shows more gradual Tc suppression with Pr doping and Nd shows more rapid suppression, I predict Gd will have intermediate behavior - closer to Er but perhaps slightly more sensitive to Pr doping than Er.

## Final Predictions

Based on the observed trends, I estimate the Tc values for Gd-based compositions by interpolating between the Er and Nd data, with Gd showing intermediate sensitivity to Pr doping:

For low Pr content (0.030), Gd should be between Er (93.9K) and Nd (85.1K), but closer to Er: ~90.0K
For moderate Pr content, Gd should show intermediate behavior between Er and Nd values.
For high Pr content (≥0.600), Gd likely loses superconductivity like both Er and Nd.