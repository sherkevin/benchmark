# Notes: Predicting Tc curve of Gd(1-x)Pr(x)Ba2Cu3O7

## Step 1: Discover family members
Searched for Gd, Pr, Er, Nd, and other RE elements. Database contains 3 RE families:
- Er series (9 compositions): x = 0.030, 0.076, 0.150, 0.200, 0.300, 0.400, 0.500, 0.800, 0.900
- Nd series (10 compositions): x = 0.030, 0.080, 0.150, 0.200, 0.300, 0.500, 0.600, 0.700, 0.800, 0.900
- Gd series (11 compositions): x = 0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900

## Step 2: Measure synthesized series

### Er series:
x=0.030→93.9K, x=0.076→91.1K, x=0.150→82.9K, x=0.200→76.4K, x=0.300→61.1K, x=0.400→45.8K, x=0.500→28.6K, x=0.800→0K, x=0.900→0K

### Nd series:
x=0.030→85.1K, x=0.080→68.2K, x=0.150→40.7K, x=0.200→31.4K, x=0.300→5.6K, x=0.500→0K, x=0.600→0K, x=0.700→0K, x=0.800→0K, x=0.900→0K

### Gd series: confirmed not synthesized (sample_available: false)

## Step 3: Pattern analysis
Nd shows much faster Tc suppression than Er. Gd (Z=64) is midway between Nd (Z=60) and Er (Z=68).

## Step 4: Prediction — Gd_Tc = (Er_Tc + Nd_Tc) / 2 at each x
Interpolating Er/Nd where needed:
- x=0.078: Er≈90.9, Nd≈68.9, Gd≈79.9
- x=0.400: Er=45.8, Nd≈0, Gd≈22.9
- x=0.600: Er≈19.1, Nd=0, Gd≈9.5
- x=0.700: Er≈9.5, Nd=0, Gd≈4.8
