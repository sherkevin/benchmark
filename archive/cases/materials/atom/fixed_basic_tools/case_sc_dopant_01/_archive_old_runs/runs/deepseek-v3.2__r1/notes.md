# Notes: Predicting Tc for Gd-based superconductors

## Initial Analysis

### Data Structure

**Synthesized compositions (RE = Er, Nd):**
- RE(1-x)Pr(x)Ba2Cu3O7 format
- Two RE elements: Er (Erbium) and Nd (Neodymium)
- Pr (Praseodymium) doping levels:
  - For Er: x = 0.030, 0.150, 0.300, 0.500, 0.800
  - For Nd: x = 0.030, 0.150, 0.300, 0.500, 0.900

**Target compositions (RE = Gd):**
- Gd(1-x)Pr(x)Ba2Cu3O7
- Pr doping levels: x = 0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900

**Instrument constraints:**
- 14 units of instrument time available (1 unit per `measure_tc` call)
- 11 synthesized compositions available to measure
- Need to measure enough to establish relationship between Pr content and Tc for different RE elements

**Additional synthesized compositions:**
- BxC(1-x)O5Cu1Sr2 series (x = 0.040 to 0.090) - appears unrelated to REBa2Cu3O7 system

## Strategy

1. First measure the synthesized compositions to understand Tc vs Pr content relationship
2. Establish how Tc changes with Pr doping for each RE element (Er and Nd)
3. Compare Er and Nd behaviors to infer relationship for Gd
4. Use measurements to predict Tc for Gd series
5. Calculate summary quantities: `max_pr_above_80K` and `critical_pr`

Given 14 measurement units and 11 synthesized compositions, I have 3 additional measurements available if needed.

## Measurements

### Er-based compositions:
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K  
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K

### Nd-based compositions:
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K

**Budget used:** 14/14 units remaining (all used)

## All Measurements

### REBa2Cu3O7 system:
**Er-based:**
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K  
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K

**Nd-based:**
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.300Nd0.700: Tc = 5.6 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K

### BxC(1-x)O5Cu1Sr2 system (unrelated):
- B0.040C0.960O5Cu1Sr2: Tc = 7.0 K
- B0.050C0.950O5Cu1Sr2: Tc = 12.5 K
- B0.060C0.940O5Cu1Sr2: Tc = 21.0 K
- B0.070C0.930O5Cu1Sr2: Tc = 23.0 K

## Data Analysis

Looking at the REBa2Cu3O7 data:

**Er series (Er(1-x)Pr(x)Ba2Cu3O7):**
- Tc decreases monotonically with increasing Pr content
- Superconductivity disappears between x=0.500 (Tc=28.6 K) and x=0.800 (Tc=0 K)
- Critical Pr content for Er: between 0.500 and 0.800

**Nd series (Nd(1-x)Pr(x)Ba2Cu3O7):**
- Tc decreases more rapidly with Pr content compared to Er
- Superconductivity disappears between x=0.300 (Tc=5.6 K) and x=0.500 (Tc=0 K)
- Critical Pr content for Nd: between 0.300 and 0.500

**Comparison:**
- At low Pr doping (x=0.030), Er has higher Tc (93.9 K) than Nd (85.1 K)
- Nd system is more sensitive to Pr doping (faster Tc suppression)
- The B-doped system appears unrelated to REBa2Cu3O7

## Quantitative Analysis

**Model: Linear suppression Tc(x) = T0*(1 - x/x_c)**

For Er:
- Assuming x_c ≈ 0.65 (between 0.5 and 0.8)
- T0 = 93.9/(1 - 0.03/0.65) = 93.9/0.954 ≈ 98.4 K
- Predicted vs actual:
  - x=0.15: 98.4*(1-0.15/0.65)=75.7 K (actual 82.9 K)
  - x=0.30: 98.4*(1-0.30/0.65)=53.0 K (actual 61.1 K)
  - x=0.50: 98.4*(1-0.50/0.65)=22.7 K (actual 28.6 K)

For Nd:
- Assuming x_c ≈ 0.4 (between 0.3 and 0.5)
- T0 = 85.1/(1 - 0.03/0.4) = 85.1/0.925 ≈ 92.0 K
- Predicted vs actual:
  - x=0.15: 92.0*(1-0.15/0.4)=57.5 K (actual 40.7 K)
  - x=0.30: 92.0*(1-0.30/0.4)=23.0 K (actual 5.6 K)

The linear model fits Er reasonably well but not Nd well.

**Alternative: Quadratic suppression Tc(x) = T0*(1 - (x/x_c)^2)**

For Er (x_c=0.65, T0=93.9/(1-(0.03/0.65)^2)=93.9/0.998=94.1 K):
- x=0.15: 94.1*(1-(0.15/0.65)^2)=94.1*(1-0.053)=89.1 K (actual 82.9)
- x=0.30: 94.1*(1-(0.30/0.65)^2)=94.1*(1-0.213)=74.1 K (actual 61.1)
- x=0.50: 94.1*(1-(0.50/0.65)^2)=94.1*(1-0.591)=38.5 K (actual 28.6)

For Nd (x_c=0.4, T0=85.1/(1-(0.03/0.4)^2)=85.1/0.994=85.6 K):
- x=0.15: 85.6*(1-(0.15/0.4)^2)=85.6*(1-0.141)=73.5 K (actual 40.7)
- x=0.30: 85.6*(1-(0.30/0.4)^2)=85.6*(1-0.563)=37.4 K (actual 5.6)

Neither model fits perfectly. The actual suppression seems steeper than quadratic for Nd.

**Interpolation for Gd:**
Gd is between Er (atomic number 68) and Nd (60), with atomic number 64.
Position: (64-60)/(68-60) = 4/8 = 0.5

Interpolating parameters:
- x_c(Gd) = 0.4 + 0.5*(0.65-0.4) = 0.4 + 0.125 = 0.525
- T0(Gd) = 92.0 + 0.5*(98.4-92.0) = 92.0 + 3.2 = 95.2 K

But this assumes linear interpolation of T0 and x_c.

## Final Predictions for Gd Series

**Model:** Linear suppression Tc(x) = T0 * (1 - x/x_c)
- T0 = 89.5 K (interpolated from Er=93.9 K and Nd=85.1 K at x=0.03)
- x_c = 0.525 (interpolated from Er≈0.65 and Nd≈0.4)

**Predicted Tc values:**
1. O7Cu3Ba2Pr0.030Gd0.970: 89.5*(1-0.03/0.525) = 84.4 K
2. O7Cu3Ba2Pr0.078Gd0.922: 89.5*(1-0.078/0.525) = 76.2 K
3. O7Cu3Ba2Pr0.150Gd0.850: 89.5*(1-0.15/0.525) = 63.9 K
4. O7Cu3Ba2Pr0.200Gd0.800: 89.5*(1-0.20/0.525) = 55.4 K
5. O7Cu3Ba2Pr0.300Gd0.700: 89.5*(1-0.30/0.525) = 38.4 K
6. O7Cu3Ba2Pr0.400Gd0.600: 89.5*(1-0.40/0.525) = 21.3 K
7. O7Cu3Ba2Pr0.500Gd0.500: 89.5*(1-0.50/0.525) = 4.3 K
8. O7Cu3Ba2Pr0.600Gd0.400: 0 K (x > x_c)
9. O7Cu3Ba2Pr0.700Gd0.300: 0 K
10. O7Cu3Ba2Pr0.800Gd0.200: 0 K
11. O7Cu3Ba2Pr0.900Gd0.100: 0 K

**Summary quantities:**
- max_pr_above_80K: 0.03 (only x=0.03 has Tc ≥ 80 K)
- critical_pr: 0.50 (highest x with Tc > 0)

**Inferred relationship (law):** Pr doping linearly suppresses Tc in REBa2Cu3O7 superconductors, with the suppression rate depending on the rare-earth element. Heavier rare earths (like Er) show weaker suppression than lighter ones (like Nd), with Gd exhibiting intermediate behavior.