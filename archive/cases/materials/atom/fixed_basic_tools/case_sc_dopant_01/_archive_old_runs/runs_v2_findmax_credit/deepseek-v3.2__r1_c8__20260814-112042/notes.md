# Notes: Finding Highest-Tc Composition with Measurement Budget

## Task Overview
- Budget: 8 measurement credits (1 credit per measurement)
- Candidates: 30 compositions with Pr doping in O7Cu3Ba2Pr_xRE_(1-x) superconductors
- Goal: Find the composition with highest critical temperature (Tc)
- Strategy: Need to intelligently sample to infer pattern, then choose best candidate

## Candidate Analysis
Pattern observed in candidates.txt:
- Base formula: O7Cu3Ba2Pr_xRE_(1-x)
- Rare earth elements (RE): Er (Erbium), Gd (Gadolinium), Nd (Neodymium)
- Pr concentrations: 0.030, 0.076/0.078/0.080, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900
- Need to understand Tc dependence on both Pr concentration and RE element

## Initial Strategy
1. Measure low, medium, and high Pr concentrations across different RE elements
2. Look for patterns: optimal Pr concentration? Best RE element?
3. Use remaining credits to explore promising region more densely

## Measurements Plan
Credits remaining: 8

## Measurements Log

### Round 1: Low Pr concentration (x=0.030) baseline
1. **O7Cu3Ba2Pr0.030Er0.970**: Tc = 93.9 K
2. **O7Cu3Ba2Pr0.030Gd0.970**: Tc = 93.1 K  
3. **O7Cu3Ba2Pr0.030Nd0.970**: Tc = 85.1 K

**Observations from Round 1:**
- Er and Gd compositions have similar Tc (~93-94 K)
- Nd composition has significantly lower Tc (85.1 K)
- At low Pr concentration, Er and Gd seem better than Nd

**Credits remaining: 5**

### Round 2: Medium Pr concentration (x=0.150)
4. **O7Cu3Ba2Pr0.150Er0.850**: Tc = 82.9 K
5. **O7Cu3Ba2Pr0.150Gd0.850**: Tc = 67.9 K
6. **O7Cu3Ba2Pr0.150Nd0.850**: Tc = 40.7 K

**Observations from Round 2:**
- Tc drops significantly with increased Pr concentration
- From x=0.030 to x=0.150:
  - Er: 93.9K → 82.9K (Δ = -11.0K)
  - Gd: 93.1K → 67.9K (Δ = -25.2K)
  - Nd: 85.1K → 40.7K (Δ = -44.4K)
- Nd shows the strongest suppression of Tc with Pr doping
- Er shows the mildest suppression

**Hypothesis:** Pr doping suppresses Tc, and this suppression gets stronger as Pr concentration increases. The highest Tc might be at the lowest Pr concentration (x=0.030).

**Credits remaining: 2**

### Round 3: High Pr concentration (x=0.800)
7. **O7Cu3Ba2Pr0.800Er0.200**: Tc = 0.0 K

**Observations from Round 3:**
- At very high Pr concentration (x=0.800), Tc drops to 0 K
- Confirms that Pr doping strongly suppresses superconductivity
- The suppression appears monotonic: higher Pr → lower Tc

**Credits remaining: 1**

## Pattern Analysis
Clear trend observed:
1. Lowest Pr concentration (x=0.030) gives highest Tc
2. Increasing Pr concentration decreases Tc
3. At high Pr (x=0.800), superconductivity is completely suppressed (Tc = 0 K)
4. Among RE elements at low Pr: Er (93.9K) > Gd (93.1K) > Nd (85.1K)

**Hypothesis confirmed:** The highest Tc should be at the lowest Pr concentration. Among low-Pr compositions, Er gives the highest Tc.

**Remaining consideration:** There are compositions with slightly higher Pr (x=0.076-0.080) that I haven't measured. Given the downward trend, these likely have lower Tc than x=0.030 compositions.

### Final Measurement
8. **O7Cu3Ba2Pr0.076Er0.924**: Tc = 91.1 K

**Final confirmation:** Intermediate Pr concentration (x=0.076) gives lower Tc (91.1K) than lowest Pr (x=0.030, 93.9K). Trend holds.

## Final Decision
All measurements show:
1. Tc decreases monotonically with increasing Pr concentration
2. Lowest measured Tc at highest Pr concentration (0 K at x=0.800)
3. Highest measured Tc at lowest Pr concentration (93.9K at x=0.030 for Er)
4. Intermediate Pr concentration (x=0.076) gives intermediate Tc (91.1K)

Therefore, the composition with highest Tc among all candidates is **O7Cu3Ba2Pr0.030Er0.970** with Tc = 93.9 K.

**All credits used: 8/8**