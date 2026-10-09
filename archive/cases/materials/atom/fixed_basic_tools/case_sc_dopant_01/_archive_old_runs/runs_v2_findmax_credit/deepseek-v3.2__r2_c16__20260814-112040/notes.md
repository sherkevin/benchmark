# Notes: Searching for Highest-Tc Composition

## Task Overview
- 30 candidate compositions in data/candidates.txt
- 16 measurement credits available (each `measure_tc` costs 1 credit)
- Must find composition with highest Tc (critical temperature)

## Pattern Analysis
Compositions follow pattern: O7Cu3Ba2Pr_xR_y where:
- R ∈ {Er, Gd, Nd} (rare earth elements)
- x varies from 0.030 to 0.900 (Pr concentration)
- y = 1 - x (complementary rare earth concentration)

System appears to study effect of Pr doping on superconductivity in cuprate materials.

## Materials Science Background
- Pr doping typically suppresses superconductivity in high-Tc cuprates
- Different rare earth elements (Er, Gd, Nd) may have different effects
- Need to find optimal doping level that maximizes Tc
- Likely non-linear relationship: Tc may peak at some intermediate doping level

## Measurement Strategy
1. Initial sampling across different Pr concentrations to understand trend
2. Focus on promising regions based on initial measurements
3. Compare across different rare earth elements at same doping levels
4. Refine search toward maximum

## Measurement Results (15 credits used)

### Low Pr concentration (0.030):
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K ✓ (best so far)
- O7Cu3Ba2Pr0.030Gd0.970: Tc = 93.1 K
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K

### Slightly higher Pr (0.076-0.080):
- O7Cu3Ba2Pr0.076Er0.924: Tc = 91.1 K (lower than at 0.030)
- O7Cu3Ba2Pr0.078Gd0.922: Tc = 84.0 K (lower than at 0.030)
- O7Cu3Ba2Pr0.080Nd0.920: Tc = 68.2 K (lower than at 0.030)

### Medium Pr concentration (0.150):
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.150Gd0.850: Tc = 67.9 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K

### Higher Pr concentrations:
- O7Cu3Ba2Pr0.200Er0.800: Tc = 76.4 K
- O7Cu3Ba2Pr0.200Gd0.800: Tc = 57.7 K
- O7Cu3Ba2Pr0.300Er0.700: Tc = 61.1 K
- O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K (superconductivity suppressed)
- O7Cu3Ba2Pr0.900Gd0.100: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K

## Pattern Observed
1. **Pr doping suppresses Tc**: Higher Pr concentration consistently lowers Tc
2. **Er gives highest Tc**: Among {Er, Gd, Nd}, Er consistently yields highest Tc at each doping level
3. **Monotonic decrease**: Tc decreases monotonically with increasing Pr concentration
4. **Best at lowest Pr**: Highest Tc occurs at minimum Pr concentration (0.030)
5. **Complete suppression**: At Pr=0.900, superconductivity is completely suppressed (Tc=0 K)

## Decision Process
- Started with systematic sampling across doping levels and rare earth elements
- Identified clear trend: Tc decreases with increasing Pr
- Confirmed Er gives highest Tc among rare earths
- Used 15 credits to measure key points, saving 1 credit
- No need to measure more: pattern is clear, best candidate identified

## Final Selection
**O7Cu3Ba2Pr0.030Er0.970** with Tc = 93.9 K is the highest-Tc composition based on:
1. Lowest available Pr concentration (0.030)
2. Er consistently outperforms Gd and Nd
3. All higher Pr concentrations show lower Tc
4. No indication of non-monotonic behavior (Tc peak) at intermediate doping
5. Remaining 1 credit unused but not needed