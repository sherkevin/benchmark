# Superconductor Tc Study Notes

## Starting Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 26 units total (shared between measure_tc and search_db)
- Definition: high Tc >= 90 K, low Tc < 90 K
- Budget: All 26 units used (no remaining budget)

## Step A: Series Peak Analysis
Goal: Find and measure homologous doping series members (same element set, varying oxygen) to find composition with maximum Tc (T*).

Found 12 potential series members (same element set: O, Ca, Cu, Ba, Hg):
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc: 92.0 K (high)
2. O6.100Ca1Cu2Ba2Hg1 - Tc: 92.5 K (high)
3. O6.130Ca1Cu2Ba2Hg1 - Tc: 102.0 K (high)
4. O6.140Ca1Cu2Ba2Hg1 - Tc: 105.0 K (high)
5. O6.160Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc: 122.0 K (high)
7. O6.210Ca1Cu2Ba2Hg1 - Tc: 127.0 K (high) - MAXIMUM (T*)
8. O6.220Ca1Cu2Ba2Hg1 - Tc: 127.0 K (high) - Also maximum
9. O6.240Ca1Cu2Ba2Hg1 - Tc: 123.0 K (high)
10. O6.270Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - Tc: 100.0 K (high)
12. O6.310Ca1Cu2Ba2Hg1 - Tc: 98.0 K (high)

Maximum Tc (T*) = 127.0 K
Threshold1 = T* - 32 = 95.0 K

## Step B: Filter Family B
Goal: Among materials containing Hg and having exactly 4 elements, report all with Tc >= threshold1 (T* - 32)

Found 14 materials with exactly 4 elements (Hg-containing):
1. O4.047Cu1Ba2Hg1 - Measured: Tc = 95.0 K (qualifies)
2. O4.066Cu1Ba2Hg1 - Unknown (error when measuring)
3. O4.081Cu1Ba2Hg1 - Measured: Tc = 98.0 K (qualifies)
4. O4.085Cu1Ba2Hg1 - Not measured (no budget)
5. O4.096Cu1Ba2Hg1 - Not measured (no budget)
6. O4.097Cu1Ba2Hg1 - Not measured (no budget)
7. O4.101Cu1Ba2Hg1 - Not measured (no budget)
8. O4.103Cu1Ba2Hg1 - Not measured (no budget)
9. O4.150Cu1Ba2Hg1 - Not measured (no budget)
10. O4.160Cu1Ba2Hg1 - Not measured (no budget)
11. O4.162Cu1Ba2Hg1 - Not measured (no budget)
12. O4.180Cu1Ba2Hg1 - Not measured (no budget)
13. O4.200Cu1Ba2Hg1 - Not measured (no budget)
14. O4.220Cu1Ba2Hg1 - Not measured (no budget)

Only measured 2 due to budget constraints. Both qualify (Tc >= 95.0 K).

## Step C: Extrapolate
Goal: Induce rule from measurable materials to predict high/low for unmeasurable ones
Warning: Single-factor rules are wrong on several materials

Unmeasurable materials found:
1. O6.160Ca1Cu2Ba2Hg1 (no sample)
2. O6.270Ca1Cu2Ba2Hg1 (no sample)

Other 4-element materials might also be unmeasurable, but couldn't test due to budget.

From measurable data:
- All measured materials have Tc >= 90 K (high)
- Tc seems to depend on oxygen content and element combination
- Trend in homologous series: Tc increases with oxygen up to optimum, then decreases

Potential rule: "Materials containing Hg, Cu, and Ba with optimized oxygen stoichiometry have high Tc (>= 90 K)"

## Step D: Final Pick
Goal: Among unmeasurable materials predicted high, choose one with fewest elements (break ties by higher expected Tc)

Unmeasurable materials with predictions:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements) - Predicted: high (based on trend)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements) - Predicted: high (based on trend)

Both have 5 elements. Need to choose based on higher expected Tc.

## Final Answer Summary
- Peak: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold: 95.0 K
- Qualifying materials: O4.047Cu1Ba2Hg1 and O4.081Cu1Ba2Hg1
- Predictions: O6.160Ca1Cu2Ba2Hg1 (high), O6.270Ca1Cu2Ba2Hg1 (high)
- Final pick: O6.160Ca1Cu2Ba2Hg1 (expected higher Tc than O6.270)
- Rule: Materials containing Hg, Cu, and Ba with oxygen content between 4.0 and 6.3 have Tc >= 90 K

## Reflection
The task was challenging due to budget constraints. Could not fully explore all materials or test all predictions. The answer represents the best possible outcome given the limited resources.