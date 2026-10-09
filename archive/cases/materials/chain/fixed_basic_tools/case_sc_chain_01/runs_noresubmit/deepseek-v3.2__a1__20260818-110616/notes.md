# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- High Tc: Tc >= 90 K
- Measurement budget: 18 `measure_tc` calls
- Step A: Find maximum Tc in homologous doping series (same element set, varying oxygen)
- Step B: Find materials with Hg and exactly 4 elements with Tc >= threshold1 (T* - 32)
- Step C: Induce rule from measurable materials, predict high/low for unmeasurable materials
- Step D: Pick unmeasurable material with fewest elements predicted high (tie-break: higher expected Tc)

## Step A Results
Measured Tc values for homologous doping series:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (maximum Tc)
**Peak composition**: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both 127.0 K)
Need to check which one to report as peak - likely the first occurrence with maximum value.

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Budget used: 10 measurements remaining: 8

## Step B Measurements and Analysis
Measured Tc values for Hg-containing 4-element materials:
1. O4.081Cu1Ba2Hg1: 98.0 K ✓ (qualifies, >= 95.0)
2. O4.162Cu1Ba2Hg1: 97.4 K ✓
3. O4.150Cu1Ba2Hg1: 97.0 K ✓
4. O4.220Cu1Ba2Hg1: 97.0 K ✓
5. O4.096Cu1Ba2Hg1: 95.6 K ✓
6. O4.097Cu1Ba2Hg1: 95.3 K ✓

Remaining unmeasured (5):
7. O4.047Cu1Ba2Hg1
8. O4.200Cu1Ba2Hg1
9. O4.101Cu1Ba2Hg1
10. O4.103Cu1Ba2Hg1
11. O4.085Cu1Ba2Hg1

Budget used: 16 measurements, remaining: 2

Pattern so far: All measured values are close to threshold (95.3-98.0 K). Unmeasured ones have oxygen values in similar range (0.047 to 0.220). Likely all qualify, but I can't measure all due to budget.

For Step C rule induction, I need to measure other materials to understand factors affecting high/low Tc. Only 2 measurements left.

## Complete Measurement Data

**Step A - Homologous series (all high Tc >= 90 K):**
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K (T*, peak)
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

**Step B - Hg-containing 4-element materials measured (all high Tc >= 90 K):**
1. O4.081Cu1Ba2Hg1: 98.0 K
2. O4.162Cu1Ba2Hg1: 97.4 K
3. O4.150Cu1Ba2Hg1: 97.0 K
4. O4.220Cu1Ba2Hg1: 97.0 K
5. O4.096Cu1Ba2Hg1: 95.6 K
6. O4.097Cu1Ba2Hg1: 95.3 K

**Other measurable materials measured:**
1. O7Cu3Ba2La1: 89.6 K (LOW, < 90 K)
2. O7Cu3Y0.200Ba2La0.800: 81.0 K (LOW, < 90 K)

**Unmeasured materials:**
- Step B remaining (5): O4.047Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.085Cu1Ba2Hg1
- Other measurable (4): O6Cu1Ba2Hg1Tl1, O10Ca2Cu3Ba2Tl2, O6Cu1Cd0.250Ba2Tl1.750

**Budget:** All 18 measurements used.

**Step C Rule Hypothesis:**

Based on measured data:
- All 16 materials with Hg have HIGH Tc (>= 90 K)
- All 2 materials without Hg but with La/Y have LOW Tc (< 90 K)
- No materials measured with Tl

Rule hypothesis: "A material has HIGH Tc (>= 90 K) if it contains Hg or Tl (group 12 elements). A material has LOW Tc (< 90 K) if it contains rare earth elements (La, Y) and does not contain Hg or Tl."

This explains:
- Hg materials: HIGH (Hg present)
- La/Y materials without Hg: LOW (rare earths present, no group 12)
- Tl materials without rare earths: predicted HIGH (Tl substitutes Hg)
- Tl materials with rare earths: not in dataset

Potential issues:
- What about materials with neither group 12 nor rare earths? Not in dataset.
- What about materials with both Tl and rare earths? Not in dataset.

For unmeasurable predictions:
1. O6.160Ca1Cu2Ba2Hg1: HIGH (contains Hg)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (contains Hg)
3. O4.180Cu1Ba2Hg1: HIGH (contains Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800: HIGH (contains Tl, no rare earths)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: HIGH (contains Tl, no rare earths)
6. O8Cu3Y1Ba1.900La0.100: LOW (contains Y/La, no Hg/Tl)
7. O8Cu4Y1Ba1.870La0.130: LOW (contains Y/La, no Hg/Tl)

This rule would be wrong if #4 or #5 are actually low Tc. But I have no data on Tl materials.

Alternative rule: "HIGH if contains Hg, LOW if contains La/Y without Hg, unknown for Tl-only."
But need predictions for all unmeasurables.

**Step D Analysis:**
Among unmeasurables predicted HIGH: #1-5. Need one with FEWEST elements.
- #1: 5 elements (O, Ca, Cu, Ba, Hg)
- #2: 5 elements
- #3: 4 elements (O, Cu, Ba, Hg) 
- #4: 5 elements (O, Ca, Cu, Ba, Tl)
- #5: 6 elements (O, Ca, Cu, Sr, Tl, Pb)

#3 has fewest (4 elements). Tie-break: higher expected Tc. Among 4-element Hg materials measured, Tc range: 95.3-98.0 K. O4.180 likely similar.

But #1 and #2 have higher Tc potential (homologous series: up to 127 K). But they have 5 elements vs 4.

Step D says: "among the unmeasurable materials you predicted HIGH in step C, return the one with the FEWEST elements (break ties by higher expected Tc)."

So #3 has fewest (4 elements). If tie with other 4-element materials? Only #3 has 4 elements among HIGH predictions.

**Final Decisions:**

**Step A:** 
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence of maximum Tc 127.0 K)
- T* = 127.0 K
- threshold1 = 127.0 - 32 = 95.0 K

**Step B:**
- Qualifying materials: Only the 6 measured Hg-containing 4-element materials with Tc >= 95.0 K
- Unmeasured materials (O4.047, O4.200, O4.101, O4.103, O4.085) not included because Tc unknown
- Budget constraint prevented measuring all 11 materials

**Step C:**
- Rule: "High Tc (>= 90 K) if material contains Hg or Tl (group 12 elements). Low Tc (< 90 K) if material contains rare earth elements (La, Y) and does not contain Hg or Tl."
- Predictions as shown in answer.json

**Step D:**
- Among unmeasurable materials predicted HIGH: O4.180Cu1Ba2Hg1 has fewest elements (4)
- Other HIGH predictions have 5 or 6 elements
- Final pick: O4.180Cu1Ba2Hg1

**Measurement Strategy Reflection:**
- Used 10 measurements for Step A (necessary)
- Used 6 measurements for Step B (partial)
- Used 2 measurements for Step C rule induction
- Total: 18/18 measurements used
- Ideally would have measured all Step B materials, but budget insufficient
- Alternative strategy: Measure fewer from homologous series to free budget for Step B?