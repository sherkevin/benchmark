# Task Analysis Notes

## Initial Observations
- Two lists: synthesized.txt (30 compositions we have samples of) and not_synthesized.txt (10 compositions not yet made)
- Some compositions appear in both lists (duplicates): O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1
- According to task: compositions in not_synthesized.txt have no samples, so measure_tc cannot return a value for them
- Have 45 measurement units available (1 per measure_tc call)
- Confirmed: duplicates in not_synthesized.txt have no samples (measure_tc returns null)

## Homologous Series Measurements (O6.xxxCa1Cu2Ba2Hg1)
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: No sample (in not_synthesized.txt)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: No sample (in not_synthesized.txt)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Current maximum Tc (T*) in series: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Threshold1 = T* - 32 = 127 - 32 = 95 K

## Other Measurements
13. O4.081Cu1Ba2Hg1: Tc = 98.0 K
14. O4.162Cu1Ba2Hg1: Tc = 97.4 K
15. O4.150Cu1Ba2Hg1: Tc = 97.0 K
16. O4.220Cu1Ba2Hg1: Tc = 97.0 K
17. O4.096Cu1Ba2Hg1: Tc = 95.6 K
18. O4.097Cu1Ba2Hg1: Tc = 95.3 K
19. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (low)
20. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (high)
21. O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (low)
22. O7Cu3Ba2La1: Tc = 89.6 K (low)
23. O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K (low)

## Observations So Far
- O4.xxxCu1Ba2Hg1 series: All Tc values around 95-98 K (high). Oxygen content (O4.047 to O4.220) doesn't significantly affect high/low classification.
- O6.xxxCa1Cu2Ba2Hg1 series: Tc varies with oxygen content, peaks around O6.210-O6.220 (127K). Lower oxygen (O6.080: 92K, O6.100: 92.5K) are borderline high.
- Tl-containing compositions show mixed behavior: 
  - O6Cu1Ba2Hg1Tl1 = 0K (low) - contains Tl without Ca
  - O10Ca2Cu3Ba2Tl2 = 118.97K (high) - contains Tl with Ca
- Cd-containing (O6Cu1Cd0.250Ba2Tl1.750): 88K (low) - contains Cd and Tl without Ca
- La-containing: 89.6K and 81.0K (both low)
- Threshold for high/low: Tc >= 90K
- Budget used: 27 measurements, 18 remaining

## Qualifying Set Q1 Analysis
T* = 127K, threshold1 = 95K
Compositions containing Hg with exactly 4 distinct elements (O, Cu, Ba, Hg):
- O4.081Cu1Ba2Hg1: 98.0K (>=95K) ✓
- O4.162Cu1Ba2Hg1: 97.4K (>=95K) ✓
- O4.150Cu1Ba2Hg1: 97.0K (>=95K) ✓
- O4.220Cu1Ba2Hg1: 97.0K (>=95K) ✓
- O4.096Cu1Ba2Hg1: 95.6K (>=95K) ✓
- O4.097Cu1Ba2Hg1: 95.3K (>=95K) ✓
- O4.200Cu1Ba2Hg1: 94.2K (<95K) ✗
- O4.101Cu1Ba2Hg1: 93.4K (<95K) ✗
- O4.103Cu1Ba2Hg1: 92.5K (<95K) ✗
- O4.085Cu1Ba2Hg1: 92.0K (<95K) ✗

Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]

## Pattern Analysis for Rule Induction
Key observations:
1. All Cu-Ba-Hg-O compositions (4 elements) have high Tc (≥90K)
2. Ca-Cu-Ba-Hg-O compositions (5 elements) mostly high, optimal at O~6.2
3. Tl without Ca gives low Tc (0K for O6Cu1Ba2Hg1Tl1)
4. Tl with Ca can give high Tc (118.97K for O10Ca2Cu3Ba2Tl2)
5. Cd with Tl gives low Tc (88K)
6. La gives low Tc (89.6K, 81.0K)
7. Y with La gives even lower Tc (81.0K)

Potential interaction: Ca may counteract negative effects of Tl on Tc.

## Predictions for not_synthesized.txt
1. O6.160Ca1Cu2Ba2Hg1: Similar to O6.140 (105K) and O6.190 (122K) → high
2. O6.270Ca1Cu2Ba2Hg1: In O6.xxx series beyond peak, likely 110-120K → high
3. O4.180Cu1Ba2Hg1: All O4.xxxCu1Ba2Hg1 have high Tc → high
4. O4.047Cu1Ba2Hg1: All O4.xxxCu1Ba2Hg1 have high Tc → high
5. O4.066Cu1Ba2Hg1: All O4.xxxCu1Ba2Hg1 have high Tc → high
6. O4.160Cu1Ba2Hg1: All O4.xxxCu1Ba2Hg1 have high Tc → high
7. O8Ca0.850Cu2.200Ba2Tl1.800: Contains Ca with Tl (like O10Ca2Cu3Ba2Tl2) → high
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Contains Ca with Tl, Sr similar to Ba, Pb unknown but Ca-Tl combination suggests high → high
9. O8Cu3Y1Ba1.900La0.100: Contains Y and La (like O7Cu3Y0.200Ba2La0.800 with low Tc) → low
10. O8Cu4Y1Ba1.870La0.130: Similar to above → low

## Final Selection
Best composition to synthesize: fewest distinct elements among those with high predicted Tc.
- High predicted compositions with 4 elements: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- Among these, select one with highest expected Tc. Based on O4.xxx series measurements, Tc ranges 92-98K. O4.160Cu1Ba2Hg1 is chosen as it's similar to O4.150 (97.0K) and O4.162 (97.4K).

## Rule Formulation
"Tc is high (≥90K) for compositions containing Cu and Ba along with either Hg alone or Tl with Ca; Tc is low for compositions containing La or Tl without Ca. Oxygen content modulates Tc within high-Tc families but does not change the high/low classification."

## Final Answer Summary
- Peak Tc in series: O6.220Ca1Cu2Ba2Hg1 with Tc = 127.0K
- Threshold1 = 95K (T* - 32)
- Qualifying set Q1: 6 compositions with Hg, 4 distinct elements, Tc ≥ 95K
- Predictions: All O4.xxxCu1Ba2Hg1 and Ca-containing Tl compositions predicted high; La/Y-containing compositions predicted low
- Final selection: O4.160Cu1Ba2Hg1 (4 distinct elements, predicted high Tc, similar to measured O4.150 and O4.162 with Tc ~97K)

## Measurement Budget
- Total budget: 45 measurements
- Used: 27 measurements
- Remaining: 18 measurements (unused)

## Task Requirements
1. Find T* = maximum Tc in homologous doping series of O6.080Ca1Cu2Ba2Hg1 (same element set, varying oxygen content)
2. Calculate threshold1 = T* - 32
3. Find qualifying set Q1: compositions containing Hg with exactly 4 distinct elements and Tc >= threshold1
4. Induce rule for Tc prediction (interaction-based, not simple one-factor rules)
5. Predict high/low (Tc >= 90K) for all compositions in not_synthesized.txt
6. Select best composition to synthesize: fewest distinct elements among those with high Tc, ties broken by higher expected Tc

## Strategy
1. First measure Tc for homologous series of O6.080Ca1Cu2Ba2Hg1 to find T*
2. Measure other compositions to understand Tc patterns
3. Induce predictive rule
4. Apply rule to predict not_synthesized compositions
5. Make final selection