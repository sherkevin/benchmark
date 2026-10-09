# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: High Tc >= 90 K, otherwise Low
- Measurement budget: 24 measure_tc calls
- Must use measure_tc tool for every Tc value cited

## Step A: Series Peak
Find composition with maximum Tc among homologous doping series (same element set, varying oxygen) in measurable.txt.

The seed O6.080Ca1Cu2Ba2Hg1 suggests we should look for compositions with the same element set (O, Ca, Cu, Ba, Hg) but varying oxygen stoichiometry.

Looking at measurable.txt:
- Lines 1-10: O6.xxxCa1Cu2Ba2Hg1 (varying oxygen) - this is the series
- Lines 11-21: O4.xxxCu1Ba2Hg1 (different element set, no Ca)
- Lines 22-27: Various other compositions

So the series is lines 1-10 in measurable.txt.

## Step B: Filter Family B
Find all materials that contain Hg and have exactly 4 elements with Tc >= threshold1 (T* - 32).

## Step C: Extrapolate
Predict high/low for unmeasurable materials in data/unmeasurable.txt based on rule induced from measurable materials.

## Step D: Final Pick
Among unmeasurable materials predicted high in step C, return the one with fewest elements (break ties by higher expected Tc).

## Step A Results
Measured Tc values for O6.xxxCa1Cu2Ba2Hg1 series:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAXIMUM)
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K (tie for maximum)
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (maximum Tc in series)
**Peak composition**: O6.210Ca1Cu2Ba2Hg1 (first occurrence of maximum)

**threshold1 = T* - 32 = 127.0 - 32 = 95.0 K**

## Step B Results
Materials with Hg and exactly 4 elements (O4.xxxCu1Ba2Hg1 series):
1. O4.081Cu1Ba2Hg1: 98.0 K (>= 95.0 K)
2. O4.162Cu1Ba2Hg1: 97.4 K (>= 95.0 K)
3. O4.150Cu1Ba2Hg1: 97.0 K (>= 95.0 K)
4. O4.220Cu1Ba2Hg1: 97.0 K (>= 95.0 K)
5. O4.096Cu1Ba2Hg1: 95.6 K (>= 95.0 K)
6. O4.097Cu1Ba2Hg1: 95.3 K (>= 95.0 K)
7. O4.047Cu1Ba2Hg1: 95.0 K (= 95.0 K)
8. O4.200Cu1Ba2Hg1: 94.2 K (< 95.0 K)
9. O4.101Cu1Ba2Hg1: 93.4 K (< 95.0 K)
10. O4.103Cu1Ba2Hg1: 92.5 K (< 95.0 K)
11. O4.085Cu1Ba2Hg1: 92.0 K (< 95.0 K)

**Set Q1** (Tc >= threshold1 = 95.0 K):
1. O4.081Cu1Ba2Hg1
2. O4.162Cu1Ba2Hg1
3. O4.150Cu1Ba2Hg1
4. O4.220Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.047Cu1Ba2Hg1

## All Measurement Results (24/24 budget used)

**O6.xxxCa1Cu2Ba2Hg1 series (all HIGH: Tc >= 90 K):**
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAXIMUM)
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

**O4.xxxCu1Ba2Hg1 series (all HIGH: Tc >= 90 K):**
1. O4.081Cu1Ba2Hg1: 98.0 K
2. O4.162Cu1Ba2Hg1: 97.4 K
3. O4.150Cu1Ba2Hg1: 97.0 K
4. O4.220Cu1Ba2Hg1: 97.0 K
5. O4.096Cu1Ba2Hg1: 95.6 K
6. O4.097Cu1Ba2Hg1: 95.3 K
7. O4.047Cu1Ba2Hg1: 95.0 K
8. O4.200Cu1Ba2Hg1: 94.2 K
9. O4.101Cu1Ba2Hg1: 93.4 K
10. O4.103Cu1Ba2Hg1: 92.5 K
11. O4.085Cu1Ba2Hg1: 92.0 K

**Other measurable materials:**
22. O6Cu1Ba2Hg1Tl1: 0.0 K (LOW: < 90 K)
23. O10Ca2Cu3Ba2Tl2: 118.97 K (HIGH: >= 90 K)
24. O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (LOW: < 90 K)
25. O7Cu3Ba2La1: Not measured
26. O7Cu3Y0.200Ba2La0.800: Not measured

**Note:** Materials 25-26 were not measured due to budget constraints.

## Step C: Inducing a Rule
From the data:
- Materials with Hg but no Tl: All HIGH (O6.xxxCa1Cu2Ba2Hg1 and O4.xxxCu1Ba2Hg1 series)
- Materials with Tl: Mixed results
  - O6Cu1Ba2Hg1Tl1: 0.0 K (LOW) - contains both Hg and Tl
  - O10Ca2Cu3Ba2Tl2: 118.97 K (HIGH) - contains Tl but no Hg
  - O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (LOW) - contains Tl but no Hg (has Cd instead)
  
Possible rule factors:
1. Contains Hg
2. Contains Tl
3. Contains both Hg AND Tl (interaction)
4. Contains Cd
5. Number of elements
6. Oxygen stoichiometry

The warning says: "single-factor rules (e.g. 'contains Hg' or 'has 5 elements') are wrong on several of them — the governing rule is an interaction of multiple factors."

Looking at the data:
- O6Cu1Ba2Hg1Tl1: Contains both Hg AND Tl → LOW (0.0 K)
- O10Ca2Cu3Ba2Tl2: Contains Tl but NOT Hg → HIGH (118.97 K)
- O6Cu1Cd0.250Ba2Tl1.750: Contains Tl but NOT Hg, has Cd → LOW (88.0 K)

Potential rule: A material is LOW if it contains both Hg AND Tl, OR if it contains Tl AND Cd.

But we need to check against all measurable materials. The O6.xxxCa1Cu2Ba2Hg1 series all contain Hg but no Tl → all HIGH. The O4.xxxCu1Ba2Hg1 series all contain Hg but no Tl → all HIGH.

Let me check element counts:
- O6Cu1Ba2Hg1Tl1: 5 elements (O, Cu, Ba, Hg, Tl) → LOW
- O10Ca2Cu3Ba2Tl2: 5 elements (O, Ca, Cu, Ba, Tl) → HIGH  
- O6Cu1Cd0.250Ba2Tl1.750: 5 elements (O, Cu, Cd, Ba, Tl) → LOW
- O6.xxxCa1Cu2Ba2Hg1 series: 5 elements (O, Ca, Cu, Ba, Hg) → all HIGH
- O4.xxxCu1Ba2Hg1 series: 4 elements (O, Cu, Ba, Hg) → all HIGH

So element count alone doesn't determine it.

Let me analyze oxygen stoichiometry:
- High Tc materials: O6.xxx (6.xxx), O4.xxx (4.xxx), O10
- Low Tc materials: O6, O6

But O6Cu1Ba2Hg1Tl1 has O6 (no decimal) while O6Cu1Cd0.250Ba2Tl1.750 has O6 (no decimal). The HIGH O6.xxx series has decimal oxygen values.

Check O10Ca2Cu3Ba2Tl2: O10 (no decimal) but HIGH.

So oxygen decimal doesn't seem consistent.

Looking more carefully at the interaction: The LOW materials both have Tl AND either Hg or Cd. The HIGH material with Tl (O10Ca2Cu3Ba2Tl2) has neither Hg nor Cd.

Potential rule: A material is LOW if it contains Tl AND (Hg OR Cd). Otherwise HIGH.

Let me test this rule against all measurable:
1. O6.xxxCa1Cu2Ba2Hg1: Contains Hg, no Tl → HIGH ✓
2. O4.xxxCu1Ba2Hg1: Contains Hg, no Tl → HIGH ✓
3. O6Cu1Ba2Hg1Tl1: Contains Hg AND Tl → LOW ✓
4. O10Ca2Cu3Ba2Tl2: Contains Tl, but NO Hg or Cd → HIGH ✓
5. O6Cu1Cd0.250Ba2Tl1.750: Contains Tl AND Cd → LOW ✓

This rule works for all measured materials!

## Step C: Predictions for Unmeasurable Materials

**Rule induced from measurable materials:** A material has LOW Tc (< 90 K) if it contains Thallium (Tl) AND (Mercury (Hg) OR Cadmium (Cd)). Otherwise HIGH (>= 90 K).

**Applying rule to unmeasurable materials:**
1. O6.160Ca1Cu2Ba2Hg1: Contains Hg, no Tl → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Contains Hg, no Tl → HIGH
3. O4.180Cu1Ba2Hg1: Contains Hg, no Tl → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Tl, but NO Hg or Cd → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Tl, but NO Hg or Cd → HIGH
6. O8Cu3Y1Ba1.900La0.100: No Tl → HIGH
7. O8Cu4Y1Ba1.870La0.130: No Tl → HIGH

**All unmeasurable materials predicted as HIGH.**

## Step D: Final Pick
Among unmeasurable materials predicted HIGH, select the one with FEWEST elements (break ties by higher expected Tc).

Let me count elements for each:
1. O6.160Ca1Cu2Ba2Hg1: O, Ca, Cu, Ba, Hg → 5 elements
2. O6.270Ca1Cu2Ba2Hg1: O, Ca, Cu, Ba, Hg → 5 elements
3. O4.180Cu1Ba2Hg1: O, Cu, Ba, Hg → 4 elements
4. O8Ca0.850Cu2.200Ba2Tl1.800: O, Ca, Cu, Ba, Tl → 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: O, Ca, Cu, Sr, Tl, Pb → 6 elements
6. O8Cu3Y1Ba1.900La0.100: O, Cu, Y, Ba, La → 5 elements
7. O8Cu4Y1Ba1.870La0.130: O, Cu, Y, Ba, La → 5 elements

**Materials with fewest elements:**
- O4.180Cu1Ba2Hg1: 4 elements
- All others: 5 or 6 elements

So **O4.180Cu1Ba2Hg1** has the fewest elements (4).

For tie-breaking (if multiple with same element count), we would choose by higher expected Tc. But here there's only one with 4 elements.

## Summary
**Step A Complete:** Peak composition O6.210Ca1Cu2Ba2Hg1 with T* = 127.0 K
**Step B Complete:** 7 materials qualify for Q1 (Tc >= 95.0 K)
**Step C Complete:** Rule induced: "A material has LOW Tc (< 90 K) if it contains Thallium (Tl) AND (Mercury (Hg) OR Cadmium (Cd)). Otherwise HIGH (>= 90 K)."
**Step D Complete:** Final pick O4.180Cu1Ba2Hg1 (fewest elements among HIGH predictions)

**Answer.json created with all required fields.**

**Budget:** 24/24 measurements used.

## Plan
1. ~~Measure all 10 compositions in the O6.xxxCa1Cu2Ba2Hg1 series to find T*~~ DONE
2. ~~Calculate threshold1 = T* - 32~~ DONE
3. ~~Identify all materials in measurable.txt that contain Hg and have exactly 4 elements~~ DONE
4. ~~Measure Tc for those materials to find which have Tc >= threshold1 (set Q1)~~ DONE
5. ~~Analyze all measurable materials to induce a rule separating high/low Tc~~ DONE
6. ~~Apply rule to predict unmeasurable materials~~ DONE
7. ~~Select final pick from high predictions with fewest elements~~ DONE
8. ~~Create answer.json~~ DONE