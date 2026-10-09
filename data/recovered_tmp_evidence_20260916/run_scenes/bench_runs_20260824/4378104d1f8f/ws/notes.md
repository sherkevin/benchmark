# Task Notes: Predicting Tc for Gd-series superconductors

## Initial Discovery
Searched database for compositions containing "Pr" and found 30 compositions in the family `RE(1-x) Pr(x) Ba2 Cu3 O7` with three rare earth (RE) types:
- Er (Erbium)
- Gd (Gadolinium) 
- Nd (Neodymium)

The compositions follow the pattern: O7Cu3Ba2Pr{x}RE{y} where x + y = 1.0

Pr concentrations found: 0.030, 0.076/0.078/0.080, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900

Gd-series compositions (need prediction):
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

## Measurements of Synthesized Compositions

### Er (Erbium) series:
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.300Er0.700: Tc =半个.1 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K

### Nd (Neodymium) series:
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
- O7Cu3Ba2Pr0.800Nd0.200: Tc = 0.0 K

### Gd (Gadolinium) series:
- O7Cu3Ba2Pr0.030Gd0.970: Not synthesized (sample_available: false)

## Complete Measurements Table

### Er (Erbium) series:
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K

### Nd (Neodymium) series:
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

### Gd (Gadolinium) series:
.### confirmed not synthesized

## Pattern Analysis
1. Both Er and Nd series show decreasing Tc with increasing Pr concentration
2. Nd shows much stronger suppression of Tc compared to Er
3. For Er: Tc drops from 93.9K at Pr=0.03 to 0K between Pr=0.50 and Pr=0.80
4. For Nd: Tc drops from 85.1K at Pr=0.03 to 0K between Pr=0.30 and Pr=0.50
5. The critical Pr concentration for superconductivity (Tc=0) is higher for Er than for Nd

## Prediction Strategy
Gd is between Er and Nd in the periodic table. Based on the trend:
- Er (atomic number 68) has moderate Pr suppression
- Gd (atomic number 64) should have suppression between Er and Nd
- Nd (atomic number 60) has strong Pr suppression

I expect Gd to have Tc suppression stronger than Er but weaker than Nd.

## Quantitative Prediction Model
Using weighted average: Tc_Gd(x) = α * Tc_Er(x) + (1-α) * Tc_Nd(x)
Where α = 0.3 (Gd is closer to Nd than Er in atomic number)

For missing Tc values, use linear interpolation/extrapolation from nearby data points.

### Final Predictions for Gd Series (corrected):

1. Pr=0.030: 
   - Tc_Er=93.9, Tc_Nd=85.1
   - Tc_Gd = 0.3*93.9 + 0.7*85.1 = 87.7 K

2. Pr=0.078:
   - Tc_Er(0.076)=91.1 ≈ 91.1 at Pr=0.078
   - Tc_Nd(0.078): linear between Pr=0.080(68.2) and Pr=0.150(40.7) = 69.0
   - Tc_Gd = 0.3*91.1 + 0.7*69.0 = 75.6 K

3. Pr=0.150:
   - Tc_Er=82.9, Tc_Nd=40.7
   - Tc_Gd = 0.3*82.9 + 0.7*40.7 = 53.4 K

4. Pr=0.200:
   - Tc_Er=76.4, Tc_Nd=31.4
   - Tc_Gd = 0.3*76.4 + 0.7*31.4 = 44.9 K

5. Pr=0.300:
   - Tc_Er=61.1, Tc_Nd=5.6
   - Tc_Gd = 0.3*61.1 + 0.7*5.6 = 22.3 K

6. Pr=0.400:
   - Tc_Er=45.8
   - Tc_Nd: linear between Pr=0.300(5.6) and Pr=0.500(0) = 2.8
   - Tc_Gd = 0.3*45.8 + 0.7*2.8 = 15.7 K

7. Pr=0.500:
   - Tc_Er=28.6, Tc_Nd=0.0
   - Tc_Gd = 0.3*28.6 = 8.6 K

8. Pr=0.600:
   - Tc_Er: linear between Pr=0.500(28.6) and Pr=0.800(0) = 19.1
   - Tc_Nd=0.0
   - Tc_Gd = 0.3*19.1 = 5.7 K

9. Pr=0.700:
   - Tc_Er: linear between Pr=0.500(28.6) and Pr=0.800(0) = 9.5
   - Tc_Nd=0.0
   - Tc_Gd = 0.3*9.5 = 2.9 K

10. Pr=0.800:
    - Tc_Er=0.0, Tc_Nd=0.0
    - Tc_Gd = 0.0 K

11. Pr=0.900:
    - Tc_Er=0.0, Tc_Nd=0.0
    - Tc_Gd = 0.0 K