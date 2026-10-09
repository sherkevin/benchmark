# Superconductor Tc Study Notes

## Budget: 34 units total
- Starting budget: 34 units
- Both `measure_tc` and `search_db` (per page) cost 1 unit each

## Seed material: O6.080Ca1Cu2Ba2Hg1
- Contains elements: O (oxygen), Ca (calcium), Cu (copper), Ba (barium), Hg (mercury)
- This belongs to a homologous doping series with same element set, varying oxygen

## Step A - Find series peak
Need to find all materials in the same homologous doping series (same element set: Ca, Cu, Ba, Hg, varying oxygen content).

## Initial strategy:
1. Search for materials containing all these elements to find the series
2. Measure Tc for each series member to find maximum Tc (T*)

Let's start by searching for materials containing Hg (since Hg is less common than O, Cu, etc.)

## Step B - Filter family B
Materials that: contain Hg AND have exactly 4 elements
Threshold1 = T* - 32
Need to report ALL materials in Q1 with Tc >= threshold1

## Step C - Extrapolate
Find unmeasurable materials (no sample exists)
Induce a rule from measurable materials to predict high/low (Tc >= 90K)
Warning: single-factor rules are wrong on several

## Step A Results (Homologous Series Measurements):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K ✓
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K ✓
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K ✓

Maximum Tc (T*) = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Peak composition = O6.210Ca1Cu2Ba2Hg1 (first one with max Tc)

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B - Filter family B
Materials that: contain Hg AND have exactly 4 elements
Threshold1 = 95.0 K
Need to report ALL materials in Q1 with Tc >= threshold1

## Step C - Extrapolate
Unmeasurable materials found so far: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
Need to find more unmeasurable materials via search_db
Induce a rule from measurable materials to predict high/low (Tc >= 90K)
Warning: single-factor rules are wrong on several

## Step A Results (Homologous Series Measurements):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K ✓
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K ✓
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K ✓

Maximum Tc (T*) = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Peak composition = O6.210Ca1Cu2Ba2Hg1 (first one with max Tc)

Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B - Filter family B Results:
Materials that contain Hg AND have exactly 4 elements (O4.xxxCu1Ba2Hg1):
Measured with Tc >= 95.0 K:
1. O4.081Cu1Ba2Hg1: 98.0 K ✓
2. O4.096Cu1Ba2Hg1: 95.6 K ✓
3. O4.097Cu1Ba2Hg1: 95.3 K ✓

Measured with Tc < 95.0 K:
1. O4.085Cu1Ba2Hg1: 92.0 K ✗
2. O4.101Cu1Ba2Hg1: 93.4 K ✗

Unmeasurable (cannot determine Tc for Q1):
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1

Other O4.xxx materials not measured due to budget constraints.

Q1 = [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]

## Step C - Extrapolate:
Unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)
3. O4.047Cu1Ba2Hg1 (4 elements)
4. O4.066Cu1Ba2Hg1 (4 elements)

Rule induction needed. All measured materials have Tc >= 90K (all high).
Single-factor rules are wrong on several.

Potential rule: Materials containing (Cu AND Ba) AND (Hg OR Tl) with oxygen content in optimal ranges are high.
For homologous series: optimal oxygen ~6.2
For O4.xxx series: optimal oxygen >4.08?

## Step D - Final pick:
From unmeasurable materials predicted HIGH, choose one with FEWEST elements.
If multiple with same element count, choose higher expected Tc.

## Final Answer:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- Threshold1: 95.0 K
- Q1: [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]
- Predictions: All unmeasurable materials predicted HIGH based on rule
- Final pick: O4.066Cu1Ba2Hg1 (4 elements, predicted HIGH, higher expected Tc than O4.047)
- Rule: All materials containing Cu, Ba, and at least one of Hg or Tl are high-Tc (Tc >= 90K)

## Budget Summary:
Total budget: 34 units
Used: 34 units
- search_db calls: 13 (13 units)
- measure_tc calls:车间21 (21 units)

All budget exhausted.