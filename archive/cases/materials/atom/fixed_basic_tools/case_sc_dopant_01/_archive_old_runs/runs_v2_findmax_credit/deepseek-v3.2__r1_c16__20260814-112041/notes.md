# Measurement Strategy Notes

## Initial Analysis
- 30 candidate compositions: O7Cu3Ba2PrxX1-x where X ∈ {Er, Gd, Nd}
- Pr concentration (x) varies: 0.030, 0.076/0.078/0.080, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900
- Budget: 16 measurement credits (1 credit per measurement)
- Need to find highest Tc composition

## Strategy Development
Given the constraints:
1. Need to sample across Pr concentrations to understand trend
2. Need to compare different rare earth elements (Er, Gd, Nd)
3. Should focus measurements where highest Tc is likely

Hypothesis:
- Very low Pr concentrations might have higher Tc (Pr typically suppresses superconductivity)
- Different rare earths may have different optimal concentrations
- Need to explore both concentration space and element space efficiently

Plan:
1. Initial broad sampling across low, medium, and high Pr concentrations for one element
2. Identify trend direction
3. Then sample other elements at promising concentrations
4. Refine search around promising region

## Measurements Log

### Initial Measurements (Round 1) - 3 credits used
Will measure 3 samples across Pr concentration range for Nd (Neodymium) system:

1. `O7Cu3Ba2Pr0.030Nd0.970` → Tc = 85.1 K
2. `O7Cu3Ba2Pr0.300Nd0.700` → Tc = 5.6 K  
3. `O7Cu3Ba2Pr0.900Nd0.100` → Tc = 0.0 K

**Observations:**
- Clear trend: Tc decreases sharply with increasing Pr concentration
- At very low Pr (0.030), Tc is relatively high (85.1 K)
- At moderate Pr (0.300), Tc is very low (5.6 K)
- At high Pr (0.900), superconductivity is completely suppressed (0 K)

**Implications:**
- Highest Tc likely at lowest Pr concentrations
- Need to explore low Pr region more finely
- Also need to compare different rare earth elements at low Pr

### Next Measurements (Round 2) - Focus on low Pr region - 2 credits used
Plan: Measure other elements (Er, Gd) at low Pr concentration (0.030) to compare:

4. `O7Cu3Ba2Pr0.030Er0.970` → Tc = 93.9 K  
5. `O7Cu3Ba2Pr0.030Gd0.970` → Tc = 93.1 K

**Observations:**
- Er system has highest Tc so far (93.9 K)
- Gd system is close (93.1 K)
- Nd system is lowest (85.1 K)
- Clear ranking at low Pr: Er > Gd > Nd

**Current hypothesis:** Highest Tc likely with Er at low Pr concentration.

**Remaining questions:**
1. Is Pr=0.030 the optimal concentration, or could even lower Pr be better?
2. Should we explore slightly higher Pr concentrations for Er to confirm trend?
3. Should we check other elements at other low concentrations?

**Credits used:** 5/16 remaining: 11

### Next Measurements (Round 3) - Exploring concentration dependence for Er - 2 credits used
Plan: Measure Er at a few more concentrations to map the trend:

6. `O7Cu3Ba2Pr0.076Er0.924` → Tc = 91.1 K
7. `O7Cu3Ba2Pr0.150Er0.850` → Tc = 82.9 K

**Observations:**
- Tc decreases with increasing Pr concentration for Er:
  - Pr=0.030: 93.9 K
  - Pr=0.076: 91.1 K (small drop)
  - Pr=0.150: 82.9 K (significant drop)
- Trend confirms Pr suppresses Tc
- Highest Tc remains at lowest Pr concentration (0.030)

**Credits used:** 7/16 remaining: 9

### Analysis so far:
1. Er gives highest Tc at low Pr concentrations
2. Trend shows Tc decreases monotonically with increasing Pr
3. Need to verify if Gd follows similar trend at slightly higher concentrations
4. Should check if Er at Pr=0.030 is indeed the maximum

### Next Measurements (Round 4) - Confirming trends across elements - 2 credits used
Plan: Measure Gd and Nd at slightly higher Pr concentrations to confirm trends:

8. `O7Cu3Ba2Pr0.078Gd0.922` → Tc = 84.0 K  
9. `O7Cu3Ba2Pr0.080Nd0.920` → Tc = 68.2 K

**Observations:**
- Both Gd and Nd show decreasing Tc with increasing Pr:
  - Gd: Pr=0.030 → 93.1 K, Pr=0.078 → 84.0 K
  - Nd: Pr=0.030 → 85.1 K, Pr=0.080 → 68.2 K
- Trend is consistent across all three elements
- Er remains the highest at each comparable concentration

**Credits used:** 9/16 remaining: 7

### Current understanding:
1. Tc decreases monotonically with increasing Pr concentration for all elements
2. Er gives highest Tc at each concentration level
3. Lowest Pr concentration (0.030) gives highest Tc for each element
4. Er at Pr=0.030 (93.9 K) is current best candidate

**Remaining credits:** 7
**Key questions:**
1. Is there any chance a slightly different Pr concentration could give even higher Tc?
2. Should we check intermediate points between 0.030 and 0.076 for Er?
3. Should we verify the trend continues to even lower Tc at higher Pr?

### Next Measurements (Round 5) - Confirming Er trend at higher Pr - 2 credits used
Plan: Measure Er at higher Pr concentrations to confirm monotonic decrease:

10. `O7Cu3Ba2Pr0.200Er0.800` → Tc = 76.4 K
11. `O7Cu3Ba2Pr0.400Er0.600` → Tc = 45.8 K

**Observations:**
- Trend continues: Tc decreases with increasing Pr
- Sequence: 93.9 K (0.030) → 91.1 K (0.076) → 82.9 K (0.150) → 76.4 K (0.200) → 45.8 K (0.400)
- Clear monotonic decrease

**Credits used:** 11/16 remaining: 5

### Strategic considerations with 5 credits left:
We have strong evidence that:
1. Tc decreases monotonically with Pr concentration
2. Er gives highest Tc among elements
3. Lowest Pr concentration (0.030) gives highest Tc

**Possible uses of remaining credits:**
1. **Exploration strategy**: Check if any other element at Pr=0.030 could beat Er (but Gd at 93.1 K already measured, Nd at 85.1 K)
2. **Refinement strategy**: Check Er at Pr=0.500 to see if trend continues to even lower values
3. **Verification strategy**: Check Gd at Pr=0.150 to compare with Er trend
4. **Edge case strategy**: Check if extremely high Pr (0.900) for Er gives 0 K like Nd did

### Next Measurements (Round 6) - Comprehensive trend confirmation - 3 credits used
Plan: Confirm trends across elements and concentrations:

12. `O7Cu3Ba2Pr0.150Gd0.850` → Tc = 67.9 K  
13. `O7Cu3Ba2Pr0.500Er0.500` → Tc = 28.6 K
14. `O7Cu3Ba2Pr0.800Er0.200` → Tc = 0.0 K

**Observations:**
- Gd at Pr=0.150 (67.9 K) is lower than Er at same concentration (82.9 K)
- Er at Pr=0.500 (28.6 K) continues decreasing trend
- Er at Pr=0.800 (0.0 K) shows superconductivity completely suppressed at high Pr
- Trend is consistent and monotonic

**Credits used:** 14/16 remaining: 2

### Final strategic decisions with 2 credits:
We have overwhelming evidence:
1. Tc decreases monotonically with Pr concentration for all elements
2. Er gives highest Tc at every comparable concentration
3. Lowest Pr concentration (0.030) gives highest Tc
4. Current best: `O7Cu3Ba2Pr0.030Er0.970` with Tc = 93.9 K

**Options for final 2 credits:**
1. Check Er at Pr=0.900 to confirm 0 K (redundant since Pr=0.800 already 0 K)
2. Check Nd at Pr=0.150 to complete pattern (less valuable since Nd consistently lower than Er)
3. Check Gd at Pr=0.200 to confirm trend (less valuable)
4. **Best use**: Check if there's any anomaly - measure Er at Pr=0.300 (we have Nd at 5.6 K at this concentration)
5. Check Er at Pr=0.600 to complete Er dataset

### Final Measurements (Round 7) - Last 2 credits used
Plan: Test consistency at moderate Pr concentration and complete dataset:

15. `O7Cu3Ba2Pr0.300Er0.700` → Tc = 61.1 K  
16. `O7Cu3Ba2Pr0.600Gd0.400` → Tc = 0.0 K

**Observations:**
- Er at Pr=0.300 (61.1 K) is much higher than Nd at same concentration (5.6 K)
- Shows element-dependent sensitivity to Pr doping
- Gd at Pr=0.600 gives 0.0 K, consistent with complete suppression at high Pr

**All credits used:** 16/16

## Final Analysis and Conclusion

### Summary of measurements:
1. **Er system** (highest performer):
   - Pr=0.030: 93.9 K ✓ (highest overall)
   - Pr=0.076: 91.1 K
   - Pr=0.150: 82.9 K  
   - Pr=0.200: 76.4 K
   - Pr=0.300: 61.1 K
   - Pr=0.400: 45.8 K
   - Pr=0.500: 28.6 K
   - Pr=0.800: 0.0 K

2. **Gd system** (second highest):
   - Pr=0.030: 93.1 K
   - Pr=0.078: 84.0 K
   - Pr=0.150: 67.9 K
   - Pr=0.600: 0.0 K

3. **Nd system** (lowest):
   - Pr=0.030: 85.1 K
   - Pr=0.080: 68.2 K
   - Pr=0.300: 5.6 K
   - Pr=0.900: 0.0 K

### Key findings:
1. **Monotonic decrease**: Tc decreases with increasing Pr concentration for all elements
2. **Element ranking**: Er > Gd > Nd at comparable concentrations
3. **Optimal concentration**: Lowest Pr (0.030) gives highest Tc for each element
4. **Best candidate**: `O7Cu3Ba2Pr0.030Er0.970` with Tc = 93.9 K

### Reasoning for final answer:
- Systematic sampling revealed clear trends
- Er consistently outperformed other elements
- Lowest Pr concentration gave highest Tc
- No evidence that any other combination could exceed 93.9 K
- Used credits efficiently: sampled across concentrations and elements to build confidence

**Final answer**: `O7Cu3Ba2Pr0.030Er0.970`