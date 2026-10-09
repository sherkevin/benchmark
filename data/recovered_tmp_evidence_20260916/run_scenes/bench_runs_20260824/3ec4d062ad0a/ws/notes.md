# Notes on Predicting Tc for Gd Series

## Discovered Compositions

### Gd Series (Unsynthsized)
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

### Nd Series (Synthesized)
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K

### Er Series (Synthesized)
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K

## Trend Analysis

The Tc suppression with increasing Pr content (`x`) follows a similar pattern for both the `Nd` and `Er` series:
1. **Low Pr content (x ≤ 0.15)**: Tc is high but decreases rapidly with small increases in `x`.
2. **Intermediate Pr content (0.15 < x ≤ 0.30)**: Tc drops significantly but remains measurable.
3. **High Pr content (x > 0.30)**: Tc approaches or reaches 0 K, indicating loss of superconductivity.

The `Gd` series is expected to follow a similar trend, but the exact Tc values may differ based on the rare earth element (`Gd`). Given that `Gd` is intermediate between `Nd` and `Er` in terms of ionic radius and electronic properties, we can interpolate the Tc values for the `Gd` series.

## Prediction Strategy
1. For low Pr content (`x ≤ 0.15`), predict Tc as the average of `Nd` and `Er` series values.
2. For intermediate Pr content (`0.15 < x ≤ 0.30`), predict a linear interpolation between the `Nd` and `Er` series.
3. For high Pr content (`x > 0.30`), predict Tc = 0 K if the trend suggests loss of superconductivity.

Next steps: Calculate predicted Tc values for the `Gd` series based on this analysis.