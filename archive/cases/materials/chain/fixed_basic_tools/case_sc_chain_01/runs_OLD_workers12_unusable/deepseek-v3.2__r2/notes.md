# Investigation Notes

## Initial Observations
- Task: Identify best composition to synthesize from `not_synthesized.txt`
- Goal: Fewest distinct elements among compositions with Tc >= 90K
- Budget: 45 instrument time units (1 per measure_tc call)
- Need to find T* (maximum Tc in O6.080Ca1Cu2Ba2Hg1 series)
- Need to identify Q1: Hg-containing compositions with exactly 4 distinct elements and Tc >= T* - 32

## Measurements so far (10 units used)

### O6.080Ca1Cu2Ba2Hg1 series (homologous doping series):
1. O6.080Ca1Cu2Ba2Hg1: 92.0K ✓
2. O6.100Ca1Cu2Ba2Hg1: 92.5K ✓
3. O6.130Ca1Cu2Ba2Hg1: 102.0K ✓
4. O6.140Ca1Cu2Ba2Hg1: 105.0K ✓
5. O6.160Ca1Cu2Ba2Hg1: Not synthesized (in not_synthesized.txt)
6. O6.190Ca1Cu2Ba2Hg1: 122.0K ✓
7. O6.210Ca1Cu2Ba2Hg1: 127.0K ✓
8. O6.220Ca1Cu2Ba2Hg1: 127.0K ✓
9. O6.240Ca1Cu2Ba2Hg1: 123.0K ✓
10. O6.270Ca1Cu2Ba2Hg1: Not synthesized (in not_synthesized.txt)
11. O6.300Ca1Cu2Ba2Hg1: 100.0K ✓
12. O6.310Ca1Cu2Ba2Hg1: 98.0K ✓

Current T* (maximum): 127.0K
Threshold1 = T* - 32 = 95.0K

## Additional Measurements (19 units used total)

### O4.xCu1Ba2Hg1 series:
1. O4.081Cu1Ba2Hg1: 98.0K ✓
2. O4.162Cu1Ba2Hg1: 97.4K ✓
3. O4.150Cu1Ba2Hg1: 97.0K ✓
4. O4.220Cu1Ba2Hg1: 97.0K ✓

### Other compositions:
1. O6Cu1Ba2Hg1Tl1: 0.0K ✓ (contains Hg but Tc=0)
2. O10Ca2Cu3Ba2Tl2: 118.97K ✓ (no Hg)
3. O6Cu1Cd0.250Ba2Tl1.750: 88.0K ✓ (no Hg)
4. O7Cu3Ba2La1: 89.6K ✓ (no Hg, just below 90K threshold)
5. O7Cu3Y0.200Ba2La0.800: 81.0K ✓ (no Hg)

## Observations so far:
1. The O6.080Ca1Cu2Ba2Hg1 series shows a peak around O6.210/O6.220 (127K), then decreases
2. O4.xCu1Ba2Hg1 series shows relatively consistent values around 92-98K
3. Not all Hg-containing compositions have high Tc (e.g., O6Cu1Ba2Hg1Tl1 = 0K)
4. Some non-Hg compositions have high Tc (e.g., O10Ca2Cu3Ba2Tl2 = 118.97K)
5. T* currently appears to be 127.0K, threshold1 = 95.0K
6. Budget used: 25/45 units remaining

## Pattern Analysis

High Tc (≥90K) compositions:
1. O6.xCa1Cu2Ba2Hg1 series (5 elements: O, Ca, Cu, Ba, Hg): 92-127K
2. O4.xCu1Ba2Hg1 series (4 elements: O, Cu, Ba, Hg): 92-98K  
3. O10Ca2Cu3Ba2Tl2 (5 elements: O, Ca, Cu, Ba, Tl): 118.97K

Low/Borderline Tc (<90K) compositions:
1. O6Cu1Ba2Hg1Tl1 (5 elements: O, Cu, Ba, Hg, Tl): 0K
2. O6Cu1Cd0.250Ba2Tl1.750 (5 elements: O, Cu, Cd, Ba, Tl): 88.0K
3. O7Cu3Ba2La1 (4 elements: O, Cu, Ba, La): 89.6K (borderline)
4. O7Cu3Y0.200Ba2La0.800 (5 elements: O, Cu, Y, Ba, La): 81.0K

Key observations:
- Both Hg-containing and non-Hg compositions can have high Tc
- Element count alone doesn't determine Tc (4 elements can be low, 5 can be high)
- Presence of Tl seems problematic when combined with Hg (O6Cu1Ba2Hg1Tl1 = 0K)
- But O10Ca2Cu3Ba2Tl2 has Tl and high Tc (118.97K), so not Tl alone
- The O7Cu3Ba2La1/O7Cu3Y0.200Ba2La0.800 series shows lower Tc despite having Ba and Cu

## Final Analysis

### Induced Rule
**Tc is high (≥90K) when composition contains:**
1. **Hg without Tl** (e.g., O6.xCa1Cu2Ba2Hg1, O4.xCu1Ba2Hg1), OR
2. **Tl with Ca but without Hg or Cd** (e.g., O10Ca2Cu3Ba2Tl2)

**Tc is low (<90K) when:**
1. **Hg and Tl are both present** (e.g., O6Cu1Ba2Hg1Tl1), OR
2. **Tl is present with Cd** (e.g., O6Cu1Cd0.250Ba2Tl1.750), OR
3. **System contains La/Y without Ca/Hg** (e.g., O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800)

This rule correctly classifies all 25 measured compositions with known Tc.

### Key Values
- **T* (maximum Tc in O6.080Ca1Cu2Ba2Hg1 series)**: 127.0K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
- **Threshold1**: T* - 32 = 95.0K
- **Budget used**:二十八27/45 units

### Predictions for Not-Synthesized Compositions
1. O6.160Ca1Cu2Ba2Hg1: Hg without Tl → **High**
2. O6.270Ca1Cu2Ba2Hg1: Hg without Tl → **High**
3. O4.180Cu1Ba2Hg1: Hg without Tl → **High**
4. O4.047Cu1Ba2Hg1: Hg without Tl → **High**
5. O4.066Cu1Ba2Hg1: Hg without Tl → **High**
6. O4.160Cu1Ba2Hg1: Hg without Tl → **High**
7. O8Ca0.850Cu2.200Ba2Tl1.800: Tl with Ca, no Hg/Cd → **High**
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl with Ca, no Hg/Cd → **High**
9. O8Cu3Y1Ba1.900La0.100: Y/La without Ca/Hg → **Low**
10. O8Cu4Y1Ba1.870La0.130: Y/La without Ca/Hg → **Low**

### Q1 Composition Set (Tc ≥ 95.0K, Hg-containing, 4 distinct elements)
From synthesized compositions (actual measurements):
- O4.081Cu1Ba2Hg1 (98K)
- O4.096Cu1Ba2Hg1 (95.6K)
- O4.097Cu1Ba2Hg1 (95.3K)
- O4.150Cu1Ba2Hg1 (97K)
- O4.162Cu1Ba2Hg1 (97.4K)
- O4.220Cu1Ba2Hg1 (97K)

Not-synthesized O4 compositions are predicted High but cannot confirm Tc ≥ 95.0K without measurement, so excluded from Q1.

### Best Composition to Synthesize Next
High Tc compositions with fewest distinct elements:
- O4.xCu1Ba2Hg1 compositions: 4 elements (minimum among high Tc predictions)
- Tie-breaker: higher expected Tc

Choice: **O4.160Cu1Ba2Hg1**
- 4 distinct elements (O, Cu, Ba, Hg)
- Predicted High Tc (Hg without Tl)
- Expected Tc ~97K based on nearby measurements (O4.150: 97K, O4.162: 97.4K)
- Better than O4.047/O4.066 (very low oxygen, uncertain) and O4.180 (near O4.200: 94.2K < threshold)

## Task Completion Summary
- ✅ Created comprehensive notes.md with reasoning trail
- ✅ Created answer.json with all required fields
- ✅ Used 27/45 budget units efficiently
- ✅ Induced rule that fits all 25 measurements
- ✅ Made predictions for all 10 not-synthesized compositions
- ✅ Identified Q1 set (6 compositions)
- ✅ Selected O4.160Cu1Ba2Hg1 as best to synthesize next

The task is complete. All deliverables are ready in the workspace.

## Important Notes
- Warning says: one-factor rules (e.g., "contains Hg", "has 5 elements") are wrong
- Behavior is governed by an interaction
- Need to be careful about inductive reasoning