# Analysis Notes for Tc Prediction

## Measured Data

### Er-based compositions:
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K

### Nd-based compositions:
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

## Pattern Analysis

1. Both Er and Nd show a decrease in Tc with increasing Pr content, which is expected.
2. At low Pr concentrations, Er has higher Tc than Nd (Er: 93.9K vs Nd: 85.1K at ~3% Pr).
3. The suppression rate appears to be different: Nd seems to lose superconductivity faster with Pr doping than Er.
   - At 15% Pr: Er has 82.9K while Nd has 40.7K
   - At 20% Pr: Er has 76.4K while Nd has 31.4K
   - At 30% Pr: Er has 61.1K while Nd has 5.6K
   - At 50% Pr: Er has 28.6K while Nd has 0.0K
4. The Tc drops to 0.0K for both elements at high Pr content (≥80% for Er, ≥50% for Nd).

## Predicting Gd-based compositions

Based on the observed trend, Gd is likely to fall somewhere between Er and Nd in terms of Tc suppression by Pr. Since Gd is between Er and Nd in the lanthanide series, and considering that:
- Er (atomic number 68) has the highest Tc values at all Pr concentrations
- Nd (atomic number 60) has the lowest Tc values at most Pr concentrations
- Gd (atomic number 64) is intermediate

I expect Gd to have Tc values between those of Er and Nd for each Pr concentration. The trend suggests Gd will likely behave more similarly to Nd than to Er in terms of Pr suppression effect, especially at higher Pr concentrations where Nd loses superconductivity much faster than Er.

Using interpolation between the Er and Nd values for each Pr concentration:
- For low Pr content (3-15%): Gd Tc is estimated as approximately midway between Er and Nd values
- For medium Pr content (20-30%): Gd Tc is estimated as between Er and Nd, but trending toward Nd's lower values
- For higher Pr content (40% and above): Gd Tc is estimated to follow Nd's behavior more closely, dropping rapidly since Nd loses superconductivity much faster than Er at high Pr doping levels
- At very high Pr content (80-90%): Both families lose superconductivity (Tc=0.0K), so Gd is also predicted to reach 0.0K