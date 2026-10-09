# Notes for Superconductor Tc Study

## Initial Setup
- Budget: 26 units total for both `measure_tc` and `search_db`
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- High Tc: >= 90 K, otherwise low
- Task has 4 steps (A-D)

## Budget Tracking
- Starting: 26 units
- Used so far: 26 units
- Remaining: 0 units

## Step A: Series Peak
Found homologous doping series (same element set: O, Ca, Cu, Ba, Hg) with varying oxygen:

Materials in series (12 total) with Tc measurements:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K (measurable)
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K (measurable)
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K (measurable)
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K (measurable)
5. O6.160Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K (measurable)
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable) - CURRENT MAXIMUM
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable) - TIE FOR MAXIMUM
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K (measurable)
10. O6.270Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K (measurable)
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K (measurable)

Maximum Tc (T*) = 127.0 K
Composition with maximum Tc: O6.210Ca1Cu2Ba2Hg1 (first occurrence)

## Step B: Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need to find materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Tc >= 95.0 K

Found 14 materials with exactly 4 elements from Hg search:
1. O4.047Cu1Ba2Hg1 - Tc = 95.0 K (MEASURED, QUALIFIES)
2. O4.066Cu1Ba2Hg1 - Tc unknown
3. O4.081Cu1Ba2Hg1 - Tc unknown
4. O4.085Cu1Ba2Hg1 - Tc unknown
5. O4.096Cu1Ba2Hg1 - Tc unknown
6. O4.097Cu1Ba2Hg1 - Tc unknown
7. O4.101Cu1Ba2Hg1 - Tc unknown
8. O4.103Cu1Ba2Hg1 - Tc unknown
9. O4.150Cu1Ba2Hg1 - Tc unknown
10. O4.160Cu1Ba2Hg1 - Tc unknown
11. O4.162Cu1Ba2Hg1 - Tc unknown
12. O4.180Cu1Ba2Hg1 - Tc unknown
13. O4.200Cu1Ba2Hg1 - Tc unknown
14. O4.220Cu1Ba2Hg1 - Tc unknown

PROBLEM: Out of budget, cannot measure remaining materials.
Need to make educated guesses or find patterns.

## Step C: Extrapolate
Induced rule from measurable materials:
- For O6.xxxCa1Cu2Ba2Hg1 compositions: Tc is high if oxygen content >= 6.130
- For O4.xxxCu1Ba2Hg1 compositions: Tc is high if oxygen content >= 4.047

Based on this rule, predictions for unmeasurable materials found:
1. O6.160Ca1Cu2Ba2Hg1 - oxygen 6.160 >= 6.130, so predicted HIGH
2. O6.270Ca1Cu2Ba2Hg1 - oxygen 6.270 >= 6.130, so predicted HIGH

## Step D: Final Pick
Among unmeasurable materials predicted as high:
.Both have 5 elements (tie)
.O6.160 expected Tc: between O6.140 (105.0 K) and O6.190 (122.0 K) ≈ 113-115 K
.O6.270 expected Tc: between O6.240 (123.0 K) and O6.300 (100.0 K) ≈ 111-112 K
.O6.160 has higher expected Tc, so selected as final.

## Final Answer Summary
- Peak: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- Threshold: 95.0 K
- Qualifying: Only O4.047Cu1Ba2Hg1 measured (Tc = 95.0 K exactly)
- Predictions: O6.160Ca1Cu2Ba2Hg1 (high), O6.270Ca1Cu2Ba2Hg1 (high)
- Final pick: O6.160Ca1Cu2Ba2Hg1
- Rule: "Tc is high if: for O6.xxxCa1Cu2Ba2Hg1 compositions, oxygen content >= 6.130; for O4.xxxCu1Ba2Hg1 compositions, oxygen content >= 4.047"

Note: Budget constraints prevented complete measurement of all Hg-containing 4-element materials for Step B.