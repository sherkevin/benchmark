# Notes for Superconductor Candidate Analysis

## Initial Understanding
- Need to find the best unexplored superconductor candidate from data/unmeasurable.txt
- "Best" = fewest elements among those whose Tc is high (Tc >= 90 K)
- Have a measurement budget of 45 get_property calls
- Materials in unmeasurable.txt have never been probed, so I need to infer their properties from measurable materials
- Need to identify the qualifying set Q1: materials containing Hg with exactly 4 elements and Tc >= threshold1 (where threshold1 = T* - 32)
- T* is the maximum Tc within the homologous doping series of the seed material O6.080Ca1Cu2Ba2Hg1

## Step 1: Measure the Tc values for the seed material's homologous doping series
First, I'll measure the Tc values for all materials in the series starting with O6.080Ca1Cu2Ba2Hg1 (those with the same elements but different oxygen content).

## Measurements for O6.080Ca1Cu2Ba2Hg1 series:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: Tc = null (unmeasurable, as expected)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: Tc = null (unmeasurable, as expected)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

## Analysis:
- The maximum Tc (T*) in the series is 127.0 K (found in O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
- Therefore, threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- The unmeasurable materials in this series are O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1

## Step 2: Identify materials for Q1 (containing Hg, exactly 4 elements, Tc >= 95.0 K)
Materials containing Hg from measurable list:
- O4.081Cu1Ba2Hg1: Tc = 98.0 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.162Cu1Ba2Hg1: Tc = 97.4 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.150Cu1Ba2Hg1: Tc = 97.0 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.220Cu1Ba2Hg1: Tc = 97.0 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.096Cu1Ba2Hg1: Tc = 95.6 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.097Cu1Ba2Hg1: Tc = 95.3 K (4 elements: Cu, Ba, Hg, O) ✓
- O4.180Cu1Ba2Hg1: Tc = null (unmeasurable) 
- O4.047Cu1Ba2Hg1: Tc = null (unmeasurable)
- O4.200Cu1Ba2Hg1: Tc = 94.2 K (4 elements: Cu, Ba, Hg, O) - below threshold
- O4.101Cu1Ba2Hg1: Tc = 93.4 K (4 elements: Cu, Ba, Hg, O) - below threshold
- O4.103Cu1Ba2Hg1: Tc = 92.5 K (4 elements: Cu, Ba, Hg, O) - below threshold
- O4.085Cu1Ba2Hg1: Tc = 92.0 K (4 elements: Cu, Ba, Hg, O) - below threshold
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (5 elements: Cu, Ba, Hg, Tl, O) - has 5 elements

## Q1 candidates (containing Hg, exactly 4 elements, Tc >= 95.0 K):
- O4.081Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1

## Step 3: Determine a rule for predicting Tc values
Looking at the data patterns:
- In the O6.xCa1Cu2Ba2Hg1 series, Tc varies with oxygen content, peaking around O6.210-O6.220 (127.0 K)
- In the O4.xCu1Ba2Hg1 series, Tc is consistently high (around 92-98 K)

Let me analyze the oxygen content pattern in the O6.xCa1Cu2Ba2Hg1 series:
- Low oxygen: O6.080 (92.0 K), O6.100 (92.5 K)
- Medium oxygen: O6.130 (102.0 K), O6.140 (105.0 K)
- High oxygen: O6.190 (122.0 K), O6.210 (127.0 K), O6.220 (127.0 K), O6.240 (123.0 K)
- Then decreasing: O6.270 (interpolated ~120-125 K), O6.300 (100.0 K), O6.310 (98.0 K)

For the unmeasurable materials in this series:
- O6.160Ca1Cu2Ba2Hg1: Based on the pattern, likely around 115-120 K (interpolated between O6.140 at 105.0 K and O6.190 at 122.0 K) - HIGH
- O6.270Ca1Cu2Ba2Hg1: Based on the pattern, likely around 120-125 K (interpolated between O6.240 at 123.0 K and O6.300 at 100.0 K) - HIGH

For the O4.xCu1Ba2Hg1 series:
- Unmeasurable: O4.180Cu1Ba2Hg1 and O4.047Cu1Ba2Hg1
- These likely have high Tc values similar to other materials in the series (around 94-98 K) - HIGH

Additional measurements of other materials:
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (5 elements) - contains Hg but 5 elements, low Tc
- O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (5 elements) - no Hg
- O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (5 elements) - no Hg
- O7Cu3Ba2La1: Tc = 89.6 K (5 elements) - no Hg
- O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K (5 elements) - no Hg

## Step 4: Predict Tc for unmeasurable materials
Based on the patterns observed:

Materials with 4 elements containing Hg (these have highest chance of being high Tc):
- O6.160Ca1Cu2Ba2Hg1: High (interpolated ~115-120 K)
- O6.270Ca1Cu2Ba2Hg1: High (interpolated ~120-125 K)
- O4.180Cu1Ba2Hg1: High (similar to other O4.xCu1Ba2Hg1 series ~94-98 K)
- O4.047Cu1Ba2Hg1: High (similar to other O4.xCu1Ba2Hg1 series ~94-98 K)
- O4.066Cu1Ba2Hg1: High (similar to other O4.xCu1Ba2Hg1 series ~94-98 K)
- O4.160Cu1Ba2Hg1: High (similar to other O4.xCu1Ba2Hg1 series ~94-98 K)

Materials with 5+ elements (not optimal for "fewest elements"):
- O8Ca0.850Cu2.200Ba2Tl1.800: 5+ elements - not optimal
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 5+ elements - not optimal
- O8Cu3Y1Ba1.900La0.100: 5+ elements - not optimal
- O8Cu4Y1Ba1.870La0.130: 5+ elements - not optimal

## Rule Induction:
The rule appears to be: Materials containing Hg with 4 elements tend to have high Tc values, particularly those in the O4.xCu1Ba2Hg1 series and O6.xCa1Cu2Ba2Hg1 series. The Tc values are influenced by the oxygen content, with an optimal range depending on the specific composition.

## Final Answer Determination:
"Best" = fewest elements among those whose Tc is high (Tc >= 90 K)

The materials with the fewest elements (4) that are Hg-containing and predicted to have high Tc are:
- O6.160Ca1Cu2Ba2Hg1: High Tc (predicted ~115-120 K)
- O6.270Ca1Cu2Ba2Hg1: High Tc (predicted ~120-125 K)
- O4.180Cu1Ba2Hg1: High Tc (predicted ~94-98 K)
- O4.047Cu1Ba2Hg1: High Tc (predicted ~94-98 K)
- O4.066Cu1Ba2Hg1: High Tc (predicted ~94-98 K)
- O4.160Cu1Ba2Hg1: High Tc (predicted ~94-98 K)

Among these, O6.270Ca1Cu2Ba2Hg1 is predicted to have the highest Tc (~120-125 K), followed by O6.160Ca1Cu2Ba2Hg1 (~115-120 K). Since they all have the same number of elements (4), the tie-breaker is higher expected Tc, making O6.270Ca1Cu2Ba2Hg1 the best candidate.