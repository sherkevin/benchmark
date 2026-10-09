# Notes - Superconductor Tc Study

## Task Overview
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90 K, Low = Tc < 90 K

## Step A: Find homologous series peak
- Seed has elements: O, Ca, Cu, Ba, Hg (5 elements)
- Series = same element set, varying oxygen
- Need to find all series members and measure Tc

## Step A Results
Series: O{varying}Ca1Cu2Ba2Hg1
- O6.080: 92.0 K
- O6.100: 92.5 K
- O6.130: 102.0 K
- O6.140: 105.0 K
- O6.160: NO SAMPLE (unmeasurable)
- O6.190: 122.0 K
- O6.210: 127.0 K ← PEAK
- O6.220: 127.0 K ← ALSO PEAK
- O6.240: 123.0 K
- O6.270: NO SAMPLE (unmeasurable)
- O6.300: 100.0 K
- O6.310: 98.0 K

T* = 127.0 (at O6.210Ca1Cu2Ba2Hg1)
threshold1 = 127.0 - 32 = 95.0

## Step B: 4-element Hg materials
Need: Hg + exactly 4 elements, Tc >= 95
Found so far (from Hg search):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1

## Other Ca materials found:
- O10Ca2Cu3Ba2Tl2
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Ca0.850Cu2.200Ba2Tl1.800

## Step B Results: 4-element Hg materials (O, Cu, Ba, Hg)
Measured:
- O4.047Cu1Ba2Hg1: NO SAMPLE (unmeasurable)
- O4.066Cu1Ba2Hg1: NO SAMPLE (unmeasurable)
- O4.081Cu1Ba2Hg1: 98.0 K ✓ (>= 95)
- O4.085Cu1Ba2Hg1: 92.0 K ✗
- O4.096Cu1Ba2Hg1: 95.6 K ✓ (>= 95)
- O4.097Cu1Ba2Hg1: 95.3 K ✓ (>= 95)
- O4.101Cu1Ba2Hg1: 93.4 K ✗
- O4.103Cu1Ba2Hg1: 92.5 K ✗
- O4.150Cu1Ba2Hg1: 97.0 K ✓ (>= 95)
- O4.160Cu1Ba2Hg1: NO SAMPLE (unmeasurable)
- O4.162Cu1Ba2Hg1: 97.4 K ✓ (>= 95)
- O4.180Cu1Ba2Hg1: NO SAMPLE (unmeasurable)
- O4.200Cu1Ba2Hg1: measurable (budget exhausted before measurement)
- O4.220Cu1Ba2Hg1: measurable (budget exhausted before measurement)

Q1 = [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1]

## Step C: Unmeasurable materials and predictions
Unmeasurable discovered:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem) → HIGH (between O6.140=105 and O6.190=122, ~113 K)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem) → HIGH (between O6.240=123 and O6.300=100, ~112 K)
3. O4.047Cu1Ba2Hg1 (4 elem) → LOW (far below measurable O range)
4. O4.066Cu1Ba2Hg1 (4 elem) → LOW (below measurable O range)
5. O4.160Cu1Ba2Hg1 (4 elem) → HIGH (between O4.150=97 and O4.162=97.4)
6. O4.180Cu1Ba2Hg1 (4 elem) → HIGH (near O4.162=97.4, extrapolated ~98)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem) → LOW (no Hg)
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem) → LOW (no Hg)

Rule: "Hg-containing materials with oxygen stoichiometry within/near the measured superconducting range are HIGH (Tc >= 90 K). Materials without Hg, or Hg-materials with extreme oxygen deficiency (well below the lowest measured composition in their series), are LOW (Tc < 90 K)."

## Step D: Final pick
Predicted HIGH unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1 (5 elements, ~113 K)
- O6.270Ca1Cu2Ba2Hg1 (5 elements, ~112 K)
- O4.160Cu1Ba2Hg1 (4 elements, ~97.2 K)
- O4.180Cu1Ba2Hg1 (4 elements, ~98.0 K)

Fewest elements = 4: O4.160 and O4.180
Higher expected Tc: O4.180Cu1Ba2Hg1 (~98.0)

Final: O4.180Cu1Ba2Hg1

## Other materials found (not unmeasurable, but unmeasured due to budget):
- O10Ca2Cu3Ba2Tl2: measurable (returned "instrument time exhausted")
- O4.200Cu1Ba2Hg1: measurable (returned "instrument time exhausted")
- O4.220Cu1Ba2Hg1: measurable (returned "instrument time exhausted")

## Budget: 34/34 (exhausted)
## Note: Hg search pages 9-14 not explored (budget ran out). There may be ~12 more materials.
