# Materials Analysis Notes
## Task: Predict Tc for Gd-based cuprates RE(1-x)Pr(x)Ba2Cu3O7

### Materials Overview
**Synthesized families:**
1. Erbium (Er) family: 9 compositions with Pr concentration x from 0.03 to 0.9
2. Neodymium (Nd) family: chatting with Pr concentration x from 0.03 to 0.9

**To predict:**
Gadolinium (Gd) family: 11 compositions with Pr concentration x from 0.03 to 0.9

### Measurement Strategy
1. First measure low, medium, and high Pr concentrations for both Er and Nd families to understand:
   - How Tc decreases with increasing Pr concentration
   - How different RE elements affect the Tc vs Pr relationship
2. Look for systematic patterns that can be extrapolated to Gd family

### Initial Measurements
**Erbium (Er) family measurements:**
1. O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
2. O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
3. O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
4. O7Cu3Ba2Pr0.800Er0.200: Tc = 0.0 K (no superconductivity)
5. O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K (no superconductivity)

**Neodymium (Nd) family measurements:**
1. O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
2. O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2 K
3. O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
4. O7Cu3Ba2Pr0.200Nd0.800: Tc = 31.4 K
5. O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K (no superconductivity)
6. O7Cu3Ba2Pr0.700Nd0.300: Tc = 0.0 K (no superconductivity)

**Additional Erbium (Er) measurements:**
1. O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K
2. O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
3. O7Cu3Ba2Pr0.400Er0.600: Tc = 45.8 K

**Additional Neodymium (Nd) measurement:**
1. O7Cu3Ba2Pr0.600Nd0.400: Tc = 0.0 K (no superconductivity)

### Pattern Analysis
From the measurements:
1. **Er family**: 
   - Higher initial Tc (~94K at x=0.03)
   - More gradual decline with Pr doping
   - Superconductivity persists to higher Pr concentrations (~x=0.5 still has Tc=28.6K)
   - Tc = 0 for x ≥ 0.8

2. **Nd family**:
   - Lower initial Tc (~85K at x=0.03) 
   - Sharper decline with Pr doping
   - Superconductivity disappears earlier (x=0.5 already Tc=0)
   - At x=0.15, Tc is already down to 40.7K vs Er's 82.9K at same x

The data suggests that different rare earth elements have different sensitivity to Pr doping. Pr is known to suppress superconductivity in cuprates, and this suppression effect depends on the specific RE element.

### Quantitative Analysis
**Er data points (x, Tc):**
- x=0.030: Tc=93.9K
- x=0.076: Tc=91.1K  
- x=0.150: Tc=82.9K
- x=0.300: Tc=61.1K
- x=0.400: Tc=45.8K
- x=0.500: Tc=28.6K
- x=0.800: Tc=0K
- x=0.900: Tc=0K

**Nd data points (x, Tc):**
- x=0.030: Tc=85.1K
- x=0.080: Tc=68.2K
- x=0.150: Tc=40.7K
- x=0.200: Tc=31.4K
- x=0.300: Tc=5.6K
- x=0.500: Tc=0K
- x=0.600: Tc=0K
- x=0.700: Tc=0K

**Updated Er data points:**
- x=0.200: Tc=76.4K (new measurement)

### Physics Context
Pr doping suppresses superconductivity through magnetic pair-breaking. The effectiveness depends on RE ionic radius:
- Nd³⁺: 1.109 Å (largest)
- Gd³⁺: 1.078 Å (intermediate)
- Er³⁺: 1.062 Å (smallest)

Smaller ionic radius (Er) creates more structural strain but may be more resistant to Pr suppression. Larger ionic radius (Nd) systems are more easily disrupted by Pr.

### Prediction Methodology
Since Gd is chemically between Nd and Er in the periodic table, I'll predict Gd Tc at each Pr concentration x as the average of Nd and Er Tc values:

**Gd_Tc(x) = (Nd_Tc(x) + Er_Tc(x)) / 2**

Where Nd_Tc(x) and Er_Tc(x) are interpolated/extrapolated from measured data.

### Interpolation/Extrapolation
For missing data points, I'll use linear interpolation between nearest measured points.

**Nd estimates:**
- x=0.078: Very close to x=0.080 (Tc=68.2K) → ~68.0K
- x=0.400: Between x=0.300 (5.6K) and x=0.500 (0K) → ~2.8K
- x=0.800: Already 0K (superconductivity gone by x=0.500)
- x=0.900: 0K

**Er estimates:**
- x=0.078: Very close to x=0.076 (91.1K) → ~91.0K  
- x=0.600: Between x=0.500 (28.6K) and x=0.800 (0K) → ~14.3K
- x=0.700: Between x=0.500 (28.6K) and x=0.800 (0K) → ~9.5K
- x=0.900: Already measured as 0K

### Calculation Table
| x | Nd_Tc (K) | Er_Tc (K) | Gd_Tc predicted (K) | Notes |
|---|-----------|-----------|---------------------|-------|
| 0.030 | 85.1 | 93.9 | (85.1+93.9)/2 = 89.5 | Direct measurements |
| 0.078 | 68.0 | 91.0 | (68.0+91.0)/2 = 79.5 | Interpolated |
| 0.150 | 40.7 | 82.9 | (40.7+82.9)/2 = 61.8 | Direct measurements |
| 0.200 | 31.4 | 76.4 | (31.4+76.4)/2 = 53.9 | Direct measurements |
| 0.300 | 5.6 | 61.1 | (5.6+61.1)/2 = 33.4 | Direct measurements |
| 0.400 | 2.8 | 45.8 | (2.8+45.8)/2 = 24.3 | Nd interpolated, Er direct |
| 0.500 | 0.0 | 28.6 | (0.0+28.6)/2 = 14.3 | Direct measurements |
| 0.600 | 0.0 | 14.3 | (0.0+14.3)/2 = 7.2 | Nd direct, Er interpolated |
| 0.700 | 0.0 | 9.5 | (0.0+9.5)/2 = 4.8 | Nd direct, Er interpolated |
| 0.800 | 0.0 | 0.0 | (0.0+0.0)/2 = 0.0 | Direct measurements |
| 0.900 | 0.0 | 0.0 | (0.0+0.0)/2 = 0.0 | Direct measurements |

### Summary of Measurements Made
**Total measurements: 15**
- Er family: 8 measurements (x=0.030, 0.076, 0.150, 0.200, 0.300, 0.400, 0.500, 0.800, 0.900 = 9 actually)
- Nd family: 7 measurements (x=0.030, 0.080, 0.150, 0.200, 0.300, 0.500, 0.600, 0.700 = 8 actually)

Actually counting: I measured 9 Er samples and 8 Nd samples = 17 total measurements.

### Final Assessment
The measurements capture the key trends:
1. Initial Tc values for both families
2. Intermediate points to characterize curve shape
3. Critical concentrations where superconductivity disappears

The interpolation model (average of Nd and Er) provides reasonable predictions for Gd family given its intermediate chemical position between Nd and Er.