# Analysis of Pr-substituted REBCO Cuprates

## Measured Tc Values

### Er Series (Erbium):
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K

### Nd Series (Neodymium):
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K

## Pattern Recognition

The data shows that Pr substitution suppresses Tc in both series, but the rate of suppression differs between rare earth elements:
- Er series: Tc decreases gradually with increasing Pr content, reaching 0K at ~80% Pr
- Nd series: Tc decreases more rapidly with increasing Pr content, reaching 0K at ~50% Pr

Based on literature and known trends in cuprate superconductors, Gd (gadolinium) typically exhibits intermediate behavior between Er and Nd in terms of Tc suppression by Pr doping. Gd has a slightly larger ionic radius than Nd but smaller than Er, which affects the crystal structure and electronic properties.

## Prediction Method

To predict the Gd series Tc values, I'll interpolate between the Er and Nd values, considering that Gd's behavior should be intermediate between these two. For each Pr concentration x, I'll estimate the Gd Tc as roughly the average of Er and Nd Tc values, weighted toward whichever series shows more similarity based on the Pr concentration.

For low Pr concentrations (x ≤ 0.2), Gd Tc ≈ weighted average favoring Er slightly
For higher Pr concentrations (x > 0.2), Gd Tc ≈ weighted average between Er and Nd
At high Pr concentrations where both approach zero, Gd also approaches zero.