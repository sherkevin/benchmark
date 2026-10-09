# Materials Science Analysis Notes

## Task Overview
Predict Tc for Gd-based compositions (targets.txt) using measurements from Er-based and Nd-based compositions (synthesized.txt). All compositions follow RE(1-x)Pr(x)Ba2Cu3O7 format.

## Initial Observations
1. Synthesized samples: RE = Er (Erbium) and Nd (Neodymium) with varying Pr content (x)
2. Target samples: RE = Gd (Gadolinium) with varying Pr content (x)
3. Additional B-C-O compositions at end of synthesized.txt - these appear to be a different system

## Strategy
1. Measure key compositions from Er and Nd series to understand Tc vs. Pr content relationship for each RE
2. Infer how Tc depends on both x (Pr content) and RE element identity
3. Extrapolate to Gd series using the inferred relationships
4. Identify best composition (highest Tc) and critical Pr content (highest x with Tc > 0)

## Budget: 30 instrument time units available

## Measurements Plan
Start with systematic measurements across Er and Nd series to characterize the Tc(x) relationship for each RE.

## Measurements Taken (29/30 budget used)

### Erbium (Er) series:
1. O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K (x=0.030)
2. O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K (x=0.076)  
3. O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K (x=0.150)
4. O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K (x=0.200)
5. O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K (x=0.300)
6. O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K (x=0.400)
7. O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K (x=0.500)
8. O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K (x=0.800)
9. O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K (x=0.900)

### Neodymium (Nd) series:
1. O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K (x=0.030)
2. O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2 K (x=0.080)
3. O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K (x=0.150)
4. O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K (x=0.200)
5. O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K (x=0.300)
6. O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K (x=0.500)
7. O7Cu3Ba2Pr0.600Nd0.400: Tc = 0.0 K (x=0.600)
8. O7Cu3Ba2Pr0.700Nd0.300: Tc = 0.0 K (x=0.700)
9. O7Cu3Ba2Pr0.800Nd0.200: Tc = 0.0 K (x=0.800)

### B-C-O system (different chemistry):
1. B0.040C0.960O5Cu1Sr2: Tc = 7.0 K (B=0.040)
2. B0.050C0.950O5Cu1Sr2: Tc = 12.5 K (B=0.050)
3. B0.060C0.940O5Cu1Sr2: Tc = 21.0 K (B=0.060)
4. B0.070C0.930O5Cu1Sr2: Tc = 23.0 K (B=0.070)
5. B0.080C0.920O5Cu1Sr2: Tc = 25.0 K (B=0.080)
6. B0.090C0.910O5Cu1Sr2: Tc = 26.5 K (B=0.090)

## Analysis of Relationships

### For RE(1-x)Pr(x)Ba2Cu3O7 systems:
1. **Er series**: Tc decreases approximately linearly with x. Extrapolation suggests Tc = 0 around x ≈ 0.7-0.8
2. **Nd series**: Tc decreases more steeply with x. Tc = 0 for x ≥ 0.500
3. **Comparison**: At same Pr content x, Er compounds have higher Tc than Nd compounds
4. **RE effect**: The difference in Tc between RE elements increases with Pr content x

### For B-C-O system (BxCyO5Cu1Sr2):
1. Tc increases with B content (decreasing C)
2. Different behavior than RE-Pr-Ba-Cu-O system

## Quantitative Analysis for Gd Prediction

### Data Table (x, Tc_Nd, Tc_Er):
x     | Tc_Nd (K) | Tc_Er (K)
0.030 | 85.1      | 93.9
0.076/0.080 | 68.2 | 91.1  
0.150 | 40.7      | 82.9
0.200 | 31.4      | 76.4
0.300 | 5.6       | 61.1
0.400 | (est. ~2.8) | 45.8
0.500 | 0.0       | 28.6
0.600 | 0.0       | (est. ~14.3)
0.700 | 0.0       | (est. ~3.6)
0.800 | 0.0       | 0.0
0.900 | 0.0       | 0.0

### Interpolation Strategy:
Gd atomic number = 64, between Nd (60) and Er (68).
Use linear interpolation in atomic number space:
Tc_Gd(x) = Tc_Nd(x) + (Tc_Er(x) - Tc_Nd(x)) * (64-60)/(68-60)
         = Tc_Nd(x) + (Tc_Er(x) - Tc_Nd(x)) * 0.5

### Predictions for Gd series:
1. Need to estimate Tc_Nd and Tc_Er at each target x value
2. For x where only one RE has data, estimate the other using trends
3. Apply interpolation formula

### Prediction Table for Gd Targets:

x     | Tc_Nd (K) estimate | Tc_Er (K) estimate | Tc_Gd predicted (K)
------|-------------------|-------------------|-------------------
0.030 | 85.1 (measured)   | 93.9 (measured)   | (85.1+93.9)/2 = 89.5
0.078 | 68.2 (x=0.080)    | 91.1 (x=0.076)    | (68.2+91.1)/2 = 79.7
0.150 | 40.7 (measured)   | 82.9 (measured)   | (40.7+82.9)/2 = 61.8
0.200 | 31.4 (measured)   | 76.4 (measured)   | (31.4+76.4)/2 = 53.9
0.300 | 5.6 (measured)    | 61.1 (measured)   | (5.6+61.1)/2 = 33.4
0.400 | 2.8 (interp: 5.6→0) | 45.8 (measured) | (2.8+45.8)/2 = 24.3
0.500 | 0.0 (measured)    | 28.6 (measured)   | (0.0+28.6)/2 = 14.3
0.600 | 0.0 (measured)    | 19.1 (interp: 28.6→0) | (0.0+19.1)/2 = 9.6
0.700 | 0.0 (measured)    | 9.5 (interp: 28.6→0) | (0.0+9.5)/2 = 4.8
0.800 | 0.0 (measured)    | 0.0 (measured)    | 0.0
0.900 | 0.0 (measured)    | 0.0 (measured)    | 0.0

### Key Results:
1. **Best composition**: x=0.030 (lowest Pr content) with predicted Tc = 89.5 K
2. **Critical Pr content**: Highest x with Tc > 0 is x=0.700 (Tc=4.8K > 0). At x=0.800, Tc=0.
   So critical_pr = 0.700

### Inferred Relationship (Law):
Tc decreases with increasing Pr content x, and the rate of decrease depends on the rare-earth element. Gd compounds show intermediate behavior between Nd (rapid suppression) and Er (gradual suppression), with superconductivity persisting up to x ≈ 0.700.

## Conclusion
Using measurements of Er and Nd compounds with varying Pr content, I inferred that Tc suppression by Pr doping is RE-dependent. By interpolating between Nd and Er data for each Pr content x, I predicted Tc for Gd compounds. The best composition is the one with lowest Pr content (x=0.030, Tc=89.5K), and superconductivity persists up to x=0.700 in the Gd series.

Budget usage: 29/30 units used. All relevant synthesized compositions were measured to establish the relationship between Tc, Pr content, and RE identity.