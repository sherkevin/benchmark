# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc >= 90 K, otherwise low
- Measurement budget: 34 calls to measure_tc

## Step A - Series Peak
Need to measure all compositions in measurable.txt and find the one with maximum Tc.
The seed belongs to a homologous doping series (same element set, varying oxygen).

Measured compositions and Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.180Cu1Ba2Hg1: No sample available
- O4.047Cu1Ba2Hg1: No sample available
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O6Cu1Ba2Hg1Tl1: 0.0 K
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O7Cu3Ba2La1: 89.6 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K

Peak composition: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 both have Tc = 127.0 K
So T* = 127.0 K

## Step B - Filter family B
Materials containing Hg and having exactly 4 elements with Tc >= T* - 32 = 127.0 - 32 = 95.0 K:
- O4.081Cu1Ba2Hg1: 98.0 K >= 95.0 K ✓
- O4.162Cu1Ba2Hg1: 97.4 K >= 95.0 K ✓
- O4.150Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓
- O4.220Cu1Ba2Hg1: 97.0 K >= 95.0 K ✓
- O4.096Cu1Ba2Hg1: 95.6 K >= 95.0 K ✓
- O4.097Cu1Ba2Hg1: 95.3 K >= 95.0 K ✓
- O4.200Cu1Ba2Hg1: 94.2 K < 95.0 K (doesn't qualify)
- O4.101Cu1Ba2Hg1: 93.4 K < 95.0 K (doesn't qualify)
- O4.103Cu1Ba2Hg1: 92.5 K < 95.0 K (doesn't qualify)
- O4.085Cu1Ba2Hg1: 92.0 K < 95.0 K (doesn't qualify)

Therefore Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}
Threshold = 95.0

## Step C - Extrapolate Rule
Based on the measured materials, I've identified the following pattern:
- High Tc materials: Hg-based without Cd, La, or Y impurities; also Ca-Cu-Ba-Tl systems
- Low Tc materials: Those containing Cd, La, or Y; also problematic Hg-Tl system

Rule: Materials with Hg AND NOT containing Cd, La, or Y are high Tc. Materials with Ca, Cu, Ba, Tl (but no Cd, La, Y, Hg) can also be high Tc. Materials containing Cd, La, or Y are low Tc regardless of other elements.

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Has Hg, no Cd/La/Y → high
2. O6.270Ca1Cu2Ba2Hg1: Has Hg, no Cd/La/Y → high  
3. O4.180Cu1Ba2Hg1: Has Hg, no Cd/La/Y → high
4. O4.047Cu1Ba2Hg1: Has Hg, no Cd/La/Y → high
5. O4.066Cu1Ba2Hg1: Has Hg, no Cd/La/Y → high
6. O4.160Cu1Ba2Hg1: Has Hg, no Cd/La/Y → high
7. O8Ca0.850Cu2.200Ba2Tl1.800: No Hg, no Cd/La/Y, has Ca/Cu/Ba/Tl → high
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: Different system (Sr, Pb instead of Ba, Hg) → low
9. O8Cu3Y1Ba1.900La0.100: Has Y and La → low
10. O8Cu4Y1Ba1.870La0.130: Has Y and La → low

## Step D - Final Pick
Among the high-predicted unmeasurable materials, find those with fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

The materials with 4 elements are tied for fewest. To break the tie by expected Tc:
- O4.160Cu1Ba2Hg1: Should be close to O4.162Cu1Ba2Hg1 (97.4K) → estimated ~97.4K
- O4.180Cu1Ba2Hg1: Should be close to O4.162Cu1Ba2Hg1 (97.4K) and O4.220Cu1Ba2Hg1 (97.0K) → estimated ~97.2K
- O4.066Cu1Ba2Hg1: Close to O4.081Cu1Ba2Hg1 (98.0K) → estimated ~98.0K
- O4.047Cu1Ba2Hg1: Further from high Tc compositions → estimated lower

Wait, O4.066 is closest to O4.081 which had 98.0K, so it should have the highest Tc among these.

Actually, let me recalculate more carefully:
- O4.066 vs O4.081: difference of 0.015
- O4.160 vs O4.162: difference of 0.002
- O4.180 vs O4.162: difference of 0.018

Since O4.162 had 97.4K, O4.160 should be very close to 97.4K.
Since O4.081 had 98.0K, O4.066 should be very close to 98.0K.

So O4.066Cu1Ba2Hg1 would have the highest expected Tc, followed by O4.160Cu1Ba2Hg1.

Therefore, the final pick is O4.066Cu1Ba2Hg1.