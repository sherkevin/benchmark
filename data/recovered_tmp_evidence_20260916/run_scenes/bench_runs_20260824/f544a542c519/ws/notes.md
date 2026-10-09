# Materials Discovery Notes

## Task
Predict Tc for all Gd-series compositions: RE(1-x)Pr(x)Ba2Cu3O7 where RE=Gd.
The Gd series has NOT been synthesized, so I must predict from other RE series.

## Step 1: Discover compositions in the database

### search_db results:
- **Gd series** (11 compositions): Pr = 0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900
- **Nd series** (10 compositions): Pr = 0.030, 0.080, 0.150, 0.200, 0.300, 0.500, 0.600, 0.700, 0.800, 0.900
- **Er series** (9 compositions): Pr = 0.030, 0.076, 0.150, 0.200, 0.300, 0.400, 0.500, 0.800, 0.900
- No La, Sm, Eu, Dy, Ho, or Y series found.

Confirmed Gd series is unsynthesized (measure_tc returned sample_available: false).

## Step 2: Measure synthesized series

### Nd series (measured Tc in K):
| Pr content | Tc (K) |
|-----------|--------|
| 0.030 | 85.1 |
| 0.080 | 68.2 |
| 0.150 | 40.7 |
| 0.200 | 31.4 |
| 0.300 | 5.6 |
| 0.500 | 0.0 |
| 0.600 | 0.0 |
| 0.700 | 0.0 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

Nd Tc drops very steeply. Tc reaches 0 by Pr ~ 0.32 (extrapolated from slope).

### Er series (measured Tc in K):
| Pr content | Tc (K) |
|-----------|--------|
| 0.030 | 93.9 |
| 0.076 | 91.1 |
| 0.150 | 82.9 |
| 0.200 | 76.4 |
| 0.300 | 61.1 |
| 0.400 | 45.8 |
| 0.500 | 28.6 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

Er Tc drops much more gradually. Slope ~ -155 K/unit Pr from Pr=0.200 to 0.500.
Tc reaches 0 by Pr ~ 0.65-0.68 (extrapolated).

## Step 3: Infer the pattern

### Key observations:
1. Pr suppresses Tc in all RE series, but the rate depends strongly on the RE.
2. Nd (lighter RE, larger ionic radius ~1.109 A): Very fast suppression, Tc to 0 by Pr ~ 0.32.
3. Er (heavier RE, smaller ionic radius ~1.004 A): Slow suppression, Tc to 0 by Pr ~ 0.65-0.68.
4. The suppression rate correlates with RE ionic radius: larger RE means faster Pr suppression.
   This is consistent with Pr-Ba site mixing being more favorable for larger RE ions.

### Gd position:
- Gd (Z=64) is exactly midway between Nd (Z=60) and Er (Z=68) in atomic number.
- Gd ionic radius ~1.053 A is between Nd and Er.
- Therefore, Gd Tc suppression curve should be intermediate between Nd and Er.

## Step 4: Predict Gd series

### Method: Midpoint interpolation
For each Pr content x, estimate Tc_Gd(x) = (Tc_Nd(x) + Tc_Er(x)) / 2, using piecewise linear
interpolation of measured Nd and Er data to obtain values at the Gd Pr contents.

### Detailed calculations:

**Pr=0.030:** Tc_Nd=85.1, Tc_Er=93.9 => Tc_Gd = (85.1+93.9)/2 = **89.5**

**Pr=0.078:**
- Nd at 0.078: interp (0.030,85.1) to (0.080,68.2) = 85.1 - 338*0.048 = 68.9
- Er at 0.078: interp (0.076,91.1) to (0.150,82.9) = 91.1 - 110.8*0.002 = 90.9
- Tc_Gd = (68.9+90.9)/2 = **79.9**

**Pr=0.150:** Tc_Nd=40.7, Tc_Er=82.9 => Tc_Gd = (40.7+82.9)/2 = **61.8**

**Pr=0.200:** Tc_Nd=31.4, Tc_Er=76.4 => Tc_Gd = (31.4+76.4)/2 = **53.9**

**Pr=0.300:** Tc_Nd=5.6, Tc_Er=61.1 => Tc_Gd = (5.6+61.1)/2 = **33.4**

**Pr=0.400:**
- Nd at 0.400: interp (0.300,5.6) to (0.500,0) = 5.6 - 28*0.1 = 2.8
- Er at 0.400: 45.8 (measured)
- Tc_Gd = (2.8+45.8)/2 = **24.3**

**Pr=0.500:** Tc_Nd=0, Tc_Er=28.6 => Tc_Gd = (0+28.6)/2 = **14.3**

**Pr=0.600:**
- Nd at 0.600: 0 (beyond Nd critical Pr)
- Er at 0.600: interp (0.500,28.6) to (0.800,0) = 28.6 - 95.3*0.1 = 19.1
- Tc_Gd = (0+19.1)/2 = **9.5**

**Pr=0.700:**
- Nd at 0.700: 0
- Er at 0.700: interp (0.500,28.6) to (0.800,0) = 28.6 - 95.3*0.2 = 9.5
- Tc_Gd = (0+9.5)/2 = **4.8**

**Pr=0.800:** Tc_Nd=0, Tc_Er=0 => Tc_Gd = **0**

**Pr=0.900:** Tc_Nd=0, Tc_Er=0 => Tc_Gd = **0**

## Summary
The Gd series is predicted to have Tc values intermediate between Nd and Er, decreasing
monotonically from ~89.5 K at Pr=0.030 to 0 at Pr>=0.800.
