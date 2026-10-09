# Materials Discovery Notes

## Step 1: Initial exploration
First, I need to discover what compositions exist in the database related to RE(1-x) Pr(x) Ba2 Cu3 O7 cuprates.

**Search results for "Ba":**
Found 30 compositions of the form O7Cu3Ba2Pr[x]RE[1-x] where RE = Er, Gd, Nd.

The compositions show Pr concentrations (x values): 0.030, 0.076/0.078/0.080, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900

**Gd-series compositions (11 total, not synthesized):**
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

## Step 2: Measuring synthesized samples
I measured Tc for Er and Nd compositions to understand the suppression pattern.

**Er-series Tc measurements:**
- x=0.030: Tc = 93.9 K
- x=0.076: Tc = 91.1 K
- x=0.150: Tc = 82.9 K
- x=0.200: Tc = 76.4 K
- x=0.300: Tc = 61.1 K
- x=0.400: Tc = 45.8 K
- x=0.500: Tc = 28.6 K
- x=0.800: Tc = 0.0 K (not superconducting)
- x=0.900: Tc = E.0 K (not superconducting)

**Nd-series Tc measurements:**
- x=0.030: Tc = 85.1 K
- x=0.080: Tc = 68.2 K
- x=0.150: Tc = 40.7 K
- x=0.200: Tc = 31.4 K
- x=0.300: Tc = 5.6 K
- x=0.500: Tc = 0.0 K (not superconducting)
- x=0.600: Tc = 0.0 K (not superconducting)
- x=0.700: Tc = 0.0 K (not superconducting)
- x=0.800: Tc = 0.0 K (not superconducting)
- x=0.900: Tc = 0.0 K (not superconducting)

## Step 3: Analyzing patterns
Observations:
1. Er has higher Tc than Nd at all Pr concentrations
2. Tc decreases with increasing Pr concentration (x) for both RE elements
3. The suppression appears to be approximately linear or slightly curved
4. Er superconductivity persists to higher Pr concentrations (x=0.500 still has Tc=28.6K) compared to Nd (x=0.500 has Tc=0K)

## Step 4: Quantitative analysis

**Data summary:**
Pr(x) | Er Tc (K) | Nd Tc (K)
------|-----------|----------
0.030 | 93.9      | 85.1
0.076 | 91.1      | -
0.078 | -         | -
0.080 | -         | 68.2
0.150 | 82.9      | 40.7
0.200 | 76.4      | 31.4
0.300 | 61.1      | 5.6
0.400 | 45.8      | -
0.500 | 28.6      | 0.0
0.600 | -         | 0.0
0.700 | -         | 0.0
0.800 | 0.0       | 0.0
0.900 | 0.0       | 0.0

**Interpolation approach:**
Since Gd (atomic number 64) is between Nd (60) and Er (68), I'll estimate Gd Tc as a weighted average:
- Gd is 4 atomic numbers above Nd and 4 below Er (equal distance)
- So Gd Tc ≈ (Er Tc + Nd Tc) / 2 when both exist at same x
- When Nd Tc is 0, Gd Tc should be lower than the average

**Estimating missing Nd values by linear interpolation:**
- Nd at x=0.076: between 85.1 (x=0.030) and 40.7 (x=0.150): ~68.5 K
- Nd at x=0.078: similar to x=0.080 (68.2 K)
- Nd at x=0.400: between 5.6 (x=0.300) and 0.0 (x=0.500): ~2.8 K

**Gd predictions:**
1. x=0.030: Gd ≈ (93.9 + 85.1)/2 = 89.5 K
2. x=0.078: Er at x=0.076 = 91.1, Nd at x=0.078 ≈ 68.2, Gd ≈ (91.1 + 68.2)/2 = 79.65 K
3. x=0.150: Gd ≈ (82.9 + 40.7)/2 = 61.8 K
4. x=0.200: Gd ≈ (76.4 + 31.4)/2 = 53.9 K
5. x=0.300: Gd ≈ (61.1 + 5.6)/2 = 33.35 K
6. x=0.400: Er = 45.8, Nd ≈ 2.8, Gd ≈ (45.8 + 2.8)/2 = 24.3 K
7. x=0.500: Er = 28.6, Nd = 0.0, but since Nd is already 0, Gd might be 0.0 K or very low (~5-10 K)
8. x=0.600: Even Er is 0K at higher x, so Gd = 0.0 K
9. x=0.700: Gd = 0.0 K
10. x=0.800: Gd = 0.0 K
11. x=0.900: Gd = 0.0 K

However, looking at the trend, Gd might lose superconductivity between x=0.400 and x=0.500. Let me check if my estimates make sense.

## Step 5: Final predictions for Gd-series compositions

**Model:** Linear Tc(x) = a - b*x, with parameters interpolated between Er and Nd:
- Er: Tc ≈ 97.8 - 130*x (from data fitting)
- Nd: Tc ≈ 93.2 - 270*x (from data fitting)
- Gd: Tc ≈ 95.5 - 200*x (average parameters)

**Critical x where Tc=0:**
- For Gd: x_critical = 95.5/200 = 0.4775
- So compositions with x ≥ 0.4775 should have Tc = 0 K

**Final Gd predictions:**
1. O7Cu3Ba2Pr0.030Gd0.970: x=0.030 → Tc = 95.5 - 200*0.030 = 89.5 ≈ 89.5 K
2. O7Cu3Ba2Pr0.078Gd0.922: x=0.078 → Tc = 95.5 - 200*0.078 = 79.9 ≈ 80.0 K
3. O7Cu3Ba2Pr0.150Gd0.850: x=0.150 → Tc = 95.5 - 200*0.150 = 65.5 ≈ 65.5 K
4. O7Cu3Ba2Pr0.200Gd0.800: x=0.200 → Tc = 95.5 - 200*0.200 = 55.5 ≈ 55.5 K
5. O7Cu3Ba2Pr0.300Gd0.700: x=0.300 → Tc = 95.5 - 200*0.300 = 35.5 ≈ 35.5 K
6. O7Cu3Ba2Pr0.400Gd0.600: x=0.400 → Tc = 95.5 - 200*0.400 = 15.5 ≈ 15.5 K
7. O7Cu3Ba2Pr0.500Gd0.500: x=0.500 > 0.4775 → Tc = 0.0 K
8. O7Cu3Ba2Pr0.600Gd0.400: x=0.600 > 0.4775 → Tc = 0.0 K
9. O7Cu3Ba2Pr0.700Gd0.300: x=0.700 > 0.4775 → Tc = 0.0 K
10. O7Cu3Ba2Pr0.800Gd0.200: x=0.800 > 0.4775 → Tc = 0.0 K
11. O7Cu3Ba2Pr0.900Gd0.100: x=0.900 > 0.4775 → Tc = 0.0 K

These predictions follow the observed trend that heavier rare earths (Er) maintain superconductivity to higher Pr concentrations than lighter ones (Nd), with Gd in between.