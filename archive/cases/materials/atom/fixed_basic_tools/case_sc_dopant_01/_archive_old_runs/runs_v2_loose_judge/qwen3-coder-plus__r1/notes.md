# Notes on Tc Prediction for Gd-Based Superconductors

## Measured Data

### Er Series (RE = Er):
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K

### Nd Series (RE = Nd):
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K

### Other Series (Not relevant to target):
- B0.040C0.960O5Cu1Sr2: Tc = 7.0 K
- B0.050C0.950O5Cu1Sr2: Tc = 12.5 K
- B0.060C0.940O5Cu1Sr2: Tc = 21.0 K
- B0.070C0.930O5Cu1Sr2: Tc = 23.0 K

## Analysis

Looking at the data for the two RE elements (Er and Nd), I can see that Tc decreases as Pr content increases in both series. The relationship appears to follow an exponential decay pattern where Tc drops significantly as Pr content increases beyond certain thresholds.

For Er-based compounds:
- At low Pr content (0.030), Tc is ~93.9K
- At moderate Pr content (0.150), Tc is ~82.9K
- At higher Pr content (0.300), Tc is ~61.1K
- At 0.500 Pr content, Tc drops to 28.6K
- At 0.800 Pr content, Tc reaches 0.0K

For Nd-based compounds:
- At low Pr content (0.030), Tc is ~85.1K
- At moderate Pr content (0.150), Tc drops to 40.7K
- At higher Pr content (0.300), Tc drops to 5.6K
- At 0.500 Pr content, Tc reaches 0.0K

The Nd series shows a steeper decline in Tc with Pr doping compared to the Er series.

For the target Gd series, I expect a similar trend but need to determine where Gd fits between Er and Nd in terms of tolerance to Pr doping. Based on literature and typical trends in REBa2Cu3O7 systems, Gd is generally expected to have properties between Er and Nd, possibly closer to Nd due to its similar ionic radius.

## Predicted Relationship

For Gd-based compounds, I expect Tc to decrease with increasing Pr content, following a similar but potentially steeper trend than observed for Er, and possibly similar to or slightly better than Nd. Since Gd has a slightly smaller ionic radius than Nd but larger than Er, it may show intermediate behavior, but likely closer to Nd in terms of sensitivity to Pr doping.

## Predictions for Gd Series

Using the observed trends from Er and Nd series, I'll interpolate/extrapolate for Gd:

- O7Cu3Ba2Pr0.030Gd0.970: High Tc, similar to Er and Nd at this low Pr level → ~80K
- O7Cu3Ba2Pr0.078Gd0.922: Slightly lower than 0.030 → ~75K
- O7Cu3Ba2Pr0.150Gd0.850: Between Er (82.9K) and Nd (40.7K) → ~55K
- O7Cu3Ba2Pr0.200Gd0.800: Lower than 0.150 → ~45K
- O7Cu3Ba2Pr0.300Gd0.700: Between Er (61.1K) and Nd (5.6K) → ~25K
- O7Cu3Ba2Pr0.400Gd0.600: Lower than 0.300 → ~10K
- O7Cu3Ba2Pr0.500Gd0.500: Similar to Nd (0.0K) → ~0K
- O7Cu3Ba2Pr0.600Gd0.400: Likely 0K
- O7Cu3Ba2Pr0.700Gd0.300: 0K
- O7Cu3Ba2Pr0.800Gd0.200: 0K
- O7Cu3Ba2Pr0.900Gd0.100: 0K

## Critical Pr Content for Superconductivity

Based on the trend, the critical Pr content where Tc drops to 0K for Gd series is likely between 0.400 and 0.500, so the highest Pr content that's still superconducting would be around 0.400.