# Superconductor Tc Study Notes

## Task Overview
- Total budget: 45 units (shared between measure_tc and search_db)
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- "High" Tc: Tc >= 90 K
- Need to complete 4 steps (A-D)

## Step A: Series Peak
Find homologous doping series (same element set, varying oxygen). Need to find composition with maximum Tc.

## Step A Results:
- Found homologous series: O6.xxxCa1Cu2Ba2Hg1 materials with varying oxygen content
- Measured Tc values:
  1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
  2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
  3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
  4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
  5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
  6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
  7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
  8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
  9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
  10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
  11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
  12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

- Maximum Tc (T*): 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc)
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Results:
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- Measured Tc for 4-element Hg-containing materials (O4.xxxCu1Ba2Hg1 series):
  1. O4.047Cu1Ba2Hg1: No sample (unmeasurable)
  2. O4.066Cu1Ba2Hg1: No sample (unmeasurable)
  3. O4.081Cu1Ba2Hg1: Tc = 98.0 K (≥ 95.0) ✓
  4. O4.085Cu1Ba2Hg1: Tc = 92.0 K (< 95.0)
  5. O4.096Cu1Ba2Hg1: Tc = 95.6 K (≥ 95.0) ✓
  6. O4.097Cu1Ba2Hg1: Tc = 95.3 K (≥ 95.0) ✓
  7. O4.101Cu1Ba2Hg1: Tc = 93.4 K (< 95.0)
  8. O4.103Cu1Ba2Hg1: Tc = 92.5 K (< 95.0)
  9. O4.150Cu1Ba2Hg1: Tc = 97.0 K (≥ 95.0) ✓
  10. O4.160Cu1Ba2Hg1: No sample (unmeasurable)
  11. O4.162Cu1Ba2Hg1: Tc = 97.4 K (≥ 95.0) ✓
  12. O4.180Cu1Ba2Hg1: No sample (unmeasurable)
  13. O4.200Cu1Ba2Hg1: Tc = 94.2 K (< 95.0)
  14. O4.220Cu1Ba2Hg1: Tc = 97.0 K (≥ 95.0) ✓

- Qualifying materials (Q1) with Tc ≥ 95.0 K:
  1. O4.081Cu1Ba2Hg1
  2. O4.096Cu1Ba2Hg1
  3. O4.097Cu1Ba2Hg1
  4. O4.150Cu1Ba2Hg1
  5. O4.162Cu1Ba2Hg1
  6. O4.220Cu1Ba2Hg1

## Additional measurements for rule induction:
1. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (high)
2. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (low)
3. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (low)

## Unmeasurable materials found (for Step C):
1. O6.160Ca1Cu2Ba2Hg1 (from Step A)
2. O6.270Ca1Cu2Ba2Hg1 (from Step A)
3. O4.047Cu1Ba2Hg1 (from Step B)
4. O4.066Cu1Ba2Hg1 (from Step B)
5. O4.160Cu1Ba2Hg1 (from Step B)
6. O4.180Cu1Ba2Hg1 (from Step B)

## Budget Tracking
- Starting budget: 45 units
- Operations used: 45 (25 measure_tc + 20 search_db)
- Remaining: 0 units

## Step C: Rule Induction
Need to induce a rule from measurable materials to predict high/low for unmeasurable ones.

### Data analysis:
All materials containing only O, Cu, Ba, Hg (4-element series) are HIGH (≥ 90 K)
All materials containing O, Ca, Cu, Ba, Hg (5-element series) are HIGH (≥ 90 K)
O10Ca2Cu3Ba2Tl2 (contains Tl) is HIGH (118.97 K)
O6Cu1Ba2Hg1Tl1 (contains Tl) is LOW (0.0 K)
O6Cu1Cd0.250Ba2Tl1.750 (contains Tl and Cd) is LOW (88.0 K)

Pattern: Materials containing Tl seem to have lower Tc, BUT O10Ca2Cu3Ba2Tl2 is high despite having Tl. So Tl alone isn't the determining factor.

Look at oxygen content:
- High materials: O4.xxx, O6.xxx, O10 (various)
- Low materials: O6Cu1Ba2Hg1Tl1 (O6), O6Cu1Cd0.250Ba2Tl1.750 (O6)

Look at element combinations:
All HIGH materials have either:
- Only O, Cu, Ba, Hg (no Tl)
- O, Ca, Cu, Ba, Hg (no Tl)
- O10Ca2Cu3Ba2Tl2 (has Tl but also has high oxygen O10 and different stoichiometry)

All LOW materials have Tl AND specific patterns.

Potential rule: Materials containing Tl AND having exactly 1 Cu atom AND having Ba2Hg1 pattern are LOW. Or: Materials with Tl AND without Ca tend to be LOW.

Actually, looking at O10Ca2Cu3Ba2Tl2: has Ca, has 3 Cu atoms (not 1)
O6Cu1Ba2Hg1Tl1: no Ca, has 1 Cu atom
O6Cu1Cd0.250Ba2Tl1.750: no Ca, has 1 Cu atom, has Cd

Rule hypothesis: Materials containing Tl AND (no Ca OR Cu count = 1) are LOW. Materials with Tl AND Ca AND Cu count > 1 are HIGH.

Test against data:
- O10Ca2Cu3Ba2Tl2: has Tl, has Ca, Cu count = 3 (>1) → HIGH ✓
- O6Cu1Ba2Hg1Tl1: has Tl, no Ca, Cu count = 1 → LOW ✓
- O6Cu1Cd0.250Ba2Tl1.750: has Tl, no Ca, Cu count = 1 → LOW ✓

For unmeasurable materials (none contain Tl based on compositions seen), need different rule.

Actually, looking at unmeasurable materials: all are from the two series I found (O6.xxxCa1Cu2Ba2Hg1 and O4.xxxCu1Ba2Hg1). All measurable ones from these series are HIGH. So perhaps all unmeasurable ones in these series are also HIGH? But the warning says single-factor rules are wrong on several unmeasurable ones.

Maybe some unmeasurable materials are LOW due to oxygen content being outside optimal range? Let's check oxygen values:
- Unmeasurable in Ca series: O6.160 and O6.270
- Measurable HIGH in Ca series: O6.080 to O6.310 (range)
O6.160 is within range (between O6.140 and O6.190 which are HIGH)
O6.270 is within range (between O6.240 and O6.300 which are HIGH)

So oxygen content alone doesn't explain.

Maybe the rule involves multiple factors. Let me think...

## Final Conclusions:

### Induced Rule:
A material is HIGH if:
1. It contains Hg AND (contains Ca OR has oxygen content ≥ O4.081) AND does not contain Tl
OR
2. It contains Tl AND contains Ca AND has Cu count > 1
Otherwise LOW.

### Predictions for all unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1: contains Hg, contains Ca, no Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1: contains Hg, contains Ca, no Tl → HIGH
3. O4.047Cu1Ba2Hg1: contains Hg, no Ca, oxygen = O4.047 (< O4.081), no Tl → LOW
4. O4.066Cu1Ba2Hg1: contains Hg, no Ca, oxygen = O4.066 (< O4.081), no Tl → LOW
5. O4.160Cu1Ba2Hg1: contains Hg, no Ca, oxygen = O4.160 (≥ O4.081), no Tl → HIGH
6. O4.180Cu1Ba2Hg1: contains Hg, no Ca, oxygen = O4.180 (≥ O4.081), no Tl → HIGH
7. O7Ca1Cu2Sr2Tl0.500Pb0.500: contains Tl, contains Ca, Cu count = 2 (>1) → HIGH
8. O8Ca0.850Cu2.200Ba2Tl1.800: contains Tl, contains Ca, Cu count = 2.200 (>1) → HIGH

### Step D Final Selection:
Among unmeasurable materials predicted HIGH:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)
- O4.160Cu1Ba2Hg1 (4 elements)
- O4.180Cu1Ba2Hg1 (4 elements)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements)

Fewest elements: O4.160Cu1Ba2Hg1 and O4.180Cu1Ba2Hg1 (4 elements each)
Tie break: O4.160Cu1Ba2Hg1 expected to have slightly higher Tc based on proximity to O4.162 (97.4 K) vs O4.180 between O4.162 (97.4 K) and O4.200 (94.2 K).

Final selection: O4.160Cu1Ba2Hg1

## Final Answer Written to answer.json