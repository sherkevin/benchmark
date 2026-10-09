# Materials Discovery Experiment Notes

## Initial Analysis

**Candidate Composition Pattern:**
All compositions follow: `O7Cu3Ba2PrxRy` where:
- `x` = Pr concentration (0.030 to 0.900)
- `R` = Rare earth element (Er, Gd, Nd)
- `y` = R concentration = 1 - x

**Structure:**
- 30 total candidates
- 3 rare earth elements × 10 Pr concentrations each
- Pr concentrations: 0.030, 0.076/0.078/0.080, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900

**Constraints:**
- 12 measurement credits available (1 credit per measurement)
- Need to find single highest-Tc composition
- Must strategically sample to infer pattern

## Strategy

I'll use a systematic approach:
1. Initial broad sampling across Pr concentrations to understand trend
2. Focus on promising Pr concentration range
3. Compare across rare earth elements
4. Refine within optimal range

**Hypothesis:** Tc likely follows a dome-shaped curve vs Pr concentration (common in doped superconductors), with optimal doping level somewhere in middle range.

## Measurement Plan

### Phase 1: Broad Sampling (4 credits)
Start with samples across the full Pr range to identify trend:

1. Low Pr: 0.030 (baseline)
2. Medium-low: 0.150  
3. Medium-high: 0.500
4. High: 0.900

For each Pr level, pick one rare earth (Gd seems like a good representative).

### Phase 1 Results (4 credits used)
Measurements completed:
1. O7Cu3Ba2Pr0.030Gd0.970: Tc = 93.1 K
2. O7Cu3Ba2Pr0.150Gd0.850: Tc = 67.9 K  
3. O7Cu3Ba2Pr0.500Gd0.500: Tc =的阿 0.0 K
4. O7Cu3Ba2Pr0.900Gd0.100: Tc = 0.0 K

### Phase 2 Measurements (all 12 credits used)
5. O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K  ← **Current highest**
6. O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
7. O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K
8. O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
9. O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
10. O7Cu3Ba2Pr0.078Gd0.922: Tc = 84.0 K
11. O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2 K
12. O7Cu3Ba2Pr0.200Gd0.800: Tc = 57.7 K

**Total: 12 credits used, 0 credits remaining**

**Key Observations:**
1. Highest Tc at lowest Pr concentration (0.030)
2. Er gives highest Tc (93.9 K) among rare earths at Pr=0.030
3. Tc decreases monotonically with increasing Pr:
   - Pr=0.030 (Er): 93.9 K
   - Pr=0.076 (Er): 91.1 K  
   - Pr=0.150 (Er): 82.9 K
   - Pr=0.150 (Gd): 67.9 K
4. Superconductivity disappears at Pr ≥ 0.500
5. No dome shape observed - Tc highest at minimum Pr

## Final Analysis

**Clear Pattern Observed:**
1. **Tc decreases monotonically with increasing Pr concentration**
   - Pr=0.030 (Er): 93.9 K (highest)
   - Pr=0.076 (Er): 91.1 K (-2.8 K)
   - Pr=0.078 (Gd): 84.0 K
   - Pr=0.080 (Nd): 68.2 K  
   - Pr=0.150 (Er): 82.9 K (-11.0 K from Pr=0.030)
   - Pr=0.150 (Gd): 67.9 K
   - Pr=0.200 (Er): 76.4 K (-17.5 K from Pr=0.030)
   - Pr=0.200 (Gd): 57.7 K
   - Pr≥0.500: Tc = 0 K (no superconductivity)

2. **Er consistently gives highest Tc at each Pr concentration**
   - Pr=0.030: Er (93.9) > Gd (93.1) > Nd (85.1)
   - Pr≈0.076-0.080: Er (91.1) > Gd (84.0) > Nd (68.2)
   - Pr=0.150: Er (82.9) > Gd (67.9)
   - Pr=0.200: Er (76.4) > Gd (57.7)

3. **No optimal doping dome** - Tc is highest at minimum Pr (0.030)

## Conclusion

The composition with **highest Tc is O7Cu3Ba2Pr0.030Er0.970** at 93.9 K.

**Reasoning:**
- Measured systematic trend: Tc decreases with increasing Pr
- Er gives highest Tc among rare earth elements at all Pr levels tested
- Lowest Pr concentration (0.030) gives highest Tc
- Extrapolation suggests even lower Pr might give higher Tc, but 0.030 is minimum available
- No evidence of dome-shaped optimal doping curve