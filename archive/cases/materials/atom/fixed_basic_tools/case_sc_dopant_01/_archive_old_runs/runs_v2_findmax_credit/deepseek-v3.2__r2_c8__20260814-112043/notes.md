# Notes: Finding Highest-Tc Composition with 8 Measurement Credits

## Task Overview
- 30 candidate compositions in YBCO-like cuprate superconductors
- Format: O7Cu3Ba2Pr_xM_(1-x) where M = {Er, Gd, Nd}
- Pr concentration x varies from 0.030 to 0.900
- Have 8 measurement credits (1 credit per `measure_tc` call)
- Must find the single candidate with highest Tc

## Initial Observations
Compositions have 3 series based on co-dopant:
1. Er series: x = 0.030, 0.076, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900
2. Gd series: x = 0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900
3. Nd series: x = 0.030, 0.080, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900

The task requires strategic measurement to infer Tc trends with Pr concentration for each dopant type.

## Measurement Strategy
1. Start with low, medium, and high Pr concentrations for each dopant to understand basic trends
2. Based on initial measurements, identify promising concentration ranges
3. Focus measurements on promising ranges
4. Use remaining credits to refine search near potential maxima

## Measurement Plan
Credits: 8 total

### Phase 1: Broad Sampling (5 credits)
Measure one low, one medium, and one high Pr concentration for each dopant type to understand baseline trends.

## Measurements

### Measurement 1 (Credit 1/8)
- Composition: O7Cu3Ba2Pr0.150Er0.850
- Tc: 82.9 K

### Measurement 2 (Credit 2/8)
- Composition: O7Cu3Ba2Pr0.150Gd0.850
- Tc: 67.9 K

### Measurement 3 (Credit 3/8)
- Composition: O7Cu3Ba2Pr0.150Nd0.850
- Tc: 40.7 K

### Measurement 4 (Credit 4/8)
- Composition: O7Cu3Ba2Pr0.030Er0.970
- Tc: 93.9 K

### Measurement 5 (Credit 5/8)
- Composition: O7Cu3Ba2Pr0.900Er0.100
- Tc: 0.0 K

### Measurement 6 (Credit 6/8)
- Composition: O7Cu3Ba2Pr0.076Er0.924
- Tc:157 91.1 K

### Measurement 7 (Credit 7/8)
- Composition: O7Cu3Ba2Pr0.030Gd0.970
- Tc: 93.1 K

### Measurement 8 (Credit 8/8)
- Composition: O7Cu3Ba2Pr0.030Nd0.970
- Tc: 85.1 K

## Analysis of Results

### Tc vs Pr Concentration Trends
1. **Er series**: Tc decreases with increasing Pr concentration
   - x=0.030: 93.9 K (highest measured)
   - x=0.076: 91.1 K
   - x=0.150: 82.9 K
   - x=0.900: 0.0 K (superconductivity destroyed)

2. **Gd series**: Similar trend, lower Tc than Er at same concentration
   - x=0.030: 93.1 K
   - x=0.150: 67.9 K

3. **Nd series**: Lowest Tc overall
   - x=0.030: 85.1 K
   - x=0.150: 40.7 K

### Key Observations
- All series show decreasing Tc with increasing Pr concentration
- Er series has highest Tc at all measured concentrations
- The lowest Pr concentration (x=0.030) gives highest Tc for each series
- Among x=0.030 compositions, Er has the highest Tc (93.9 K)

## Conclusion
Based on the measured data:
1. Tc is maximized at the lowest Pr concentration (x=0.030)
2. Er series outperforms Gd and Nd series at same Pr concentration
3. The composition **O7Cu3Ba2Pr0.030Er0.970** has the highest measured Tc (93.9 K)
4. Since Tc decreases monotonically with Pr concentration, and we have measured the lowest available concentration for each series, O7Cu3Ba2Pr0.030Er0.970 is the most likely candidate with the highest Tc among all 30 compositions.