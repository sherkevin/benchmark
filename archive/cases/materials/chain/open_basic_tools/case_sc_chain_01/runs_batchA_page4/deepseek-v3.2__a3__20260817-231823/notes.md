# Superconductor Tc Study Notes

## Initial Setup
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 45 units total for both search_db and measure_tc
- Goal: Find materials with high Tc (≥ 90K = "high")

## Step A: Series Peak
Need to find homologous doping series with same element set (O, Ca, Cu, Ba, Hg) but varying oxygen content.

## Measurements
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (high)

## Approach
1. Search for Hg-containing materials to find series members
2. Measure Tc of series members to find maximum Tc (T*)

## Budget Tracking
Initial budget: 45 units
Used: 14 units (1 measure seed + 1 search Hg page 1 + 1 search Ca page 1 + 1 search Ca page 2 + 1 search Ca page 3 + 1 search Ca page 4 + 8 measurements)
Remaining: 31 units

## Search Results
Page五个3 Ca-containing materials:
1. O6.220Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
2. O6.240Ca1Cu2Ba2Hg1 (Tc = 123.0 K)
3. O6.270Ca1Cu2Ba2Hg1 (No sample - unsynthesized)
4. O6.300Ca1Cu2Ba2Hg1 (Tc = 100.0 K)

Page 4 Ca-containing materials:
1. O6.310Ca1Cu2Ba2Hg1 (needs measurement)
2. O7Ca1Cu2Sr2Tl0.500Pb0.500 (different element set)
3. O8Ca0.850Cu2.200Ba2Tl1.800 (different element set)

## Search Results
Page 1 Hg-containing materials:
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Hg1

Page 1 Ca-containing materials (found homologous series):
1. O10Ca2Cu3Ba2Tl2 (different element set)
2. O6.080Ca1Cu2Ba2Hg1 (seed, Tc = 92.0 K)
3. O6.100Ca1Cu2Ba2Hg1 (Tc = 92.5 K)
4. O6.130Ca1Cu2Ba2Hg1 (Tc = 102.0 K)

Page 2 Ca-containing materials:
1. O6.140Ca1Cu2Ba2Hg1 (Tc = 105.0 K)
2. O6.160Ca1Cu2Ba2Hg1 (No sample - not synthesized)
3. O6.190Ca1Cu2Ba2Hg1 (Tc = 122.0 K)
4. O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)

## Homologous Series Identified
Same element set: O, Ca, Cu, Ba, Hg
All members found (15 total matches in Ca search):
Measured (with samples):
1. O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0 K)
2. O6.100Ca1Cu2Ba2Hg1 (Tc = 92.5 K)
3. O6.130Ca1Cu2Ba2Hg1 (Tc = 102.0 K)
4. O6.140Ca1Cu2Ba2Hg1 (Tc = 105.0 K)
5. O6.190Ca1Cu2Ba2Hg1 (Tc = 122.0 K)
6. O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
7. O6.220Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
8. O6.240Ca1Cu2Ba2Hg1 (Tc = 123.0 K)
9. O6.300Ca1Cu2Ba2Hg1 (Tc = 100.0 K)
10. O6.310Ca1Cu2Ba2Hg1 (Tc = 98.0 K)

No sample (unsynthesized):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1

Maximum Tc (T*): 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
T* = 127.0 K

## Step A Complete
Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both have Tc = 127.0 K)
Peak Tc (T*): 127.0 K

## Step B: Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Need to find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc ≥ 95.0 K

## All Hg-containing materials found (27 total):
Pages 1-3: O4.xxxCu1Ba2Hg1 compounds (4 elements: O, Cu, Ba, Hg)
Page 4: O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1 (4 elements), plus homologous series materials (5 elements)
Pages 5-7: Homologous series materials (5 elements: O, Ca, Cu, Ba, Hg)
Page 7: O6Cu1Ba2Hg1Tl1 (5 elements)

## Candidate materials with exactly 4 elements (O, Cu, Ba, Hg):
1. O4.047Cu1Ba2Hg1 - No sample (unsynthesized) - for Step C
2. O4.066Cu1Ba2Hg1 - No sample (unsynthesized) - for Step C
3. O4.081Cu1Ba2Hg1 - Tc = 98.0 K (≥ 95.0 K) - QUALIFIES for Q1
4. O4.085Cu1Ba2Hg1 - Tc = 92.0 K (< 95.0 K) - does NOT qualify
5. O4.096Cu1Ba2Hg1 - Tc = 95.6 K (≥ 95.0 K) - QUALIFIES for Q1
6. O4.097Cu1Ba2Hg1 - Tc = 95.3 K (≥ 95.0 K) - QUALIFIES for Q1
7. O4.101Cu1Ba2Hg1 - Tc = 93.4 K (< 95.0 K) - does NOT qualify
8. O4.103Cu1Ba2Hg1 - Tc = 92.5 K (< 95.0 K) - does NOT qualify
9. O4.150Cu1Ba2Hg1 - Tc = 97.0 K (≥ 95.0 K) - QUALIFIES for Q1
10. O4.160Cu1Ba2Hg1 - No sample (unsynthesized) - for Step C
11. O4.162Cu1Ba2Hg1 - Tc = 97.4 K (≥ 95.0 K) - QUALIFIES for Q1
12. O4.180Cu1Ba2Hg1 - No sample (unsynthesized) - for Step C
13. O4.200Cu1Ba2Hg1 - Tc = 94.2 K (< 95.0 K) - does NOT qualify
14. O4.220Cu1Ba2Hg1 - Tc = 97.0 K (≥ 95.0 K) - QUALIFIES for Q1

## Step B Results (Q1)
Materials with exactly 4 elements (O, Cu, Ba, Hg) and Tc ≥ 95.0 K:
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.096Cu1Ba2Hg1 (95.6 K)
3. O4.097Cu1Ba2Hg1 (95.3 K)
4. O4.150Cu1Ba2Hg1 (97.0 K)
5. O4.162Cu1Ba2Hg1 (97.4 K)
6. O4.220Cu1Ba2Hg1 (97.0 K)

## Unmeasurable materials found (for Step C):
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.160Cu1Ba2Hg1
4. O4.180Cu1Ba2Hg1
5. O6.160Ca1Cu2Ba2Hg1 (from homologous series)
6. O6.270Ca1Cu2Ba2Hg1 (from homologous series)

## Step C: Induce Rule from Measurable Materials
### Homologous Series (O, Ca, Cu, Ba, Hg):
- O6.080: 92.0 K (low)
- O6.100: 92.5 K (low)
- O6.130: 102.0 K (high)
- O6.140: 105.0 K (high)
- O6.190: 122.0 K (high)
- O6.210: 127.0 K (high)
- O6.220: 127.0 K (high)
- O6.240: 123.0 K (high)
- O6.300: 100.0 K (high)
- O6.310: 98.0 K (high)

Rule for homologous series: Oxygen ≥ 6.130 → High

### O-Cu-Ba-Hg Series (4 elements):
- O4.081: 98.0 K (high)
- O4.085: 92.0 K (low)
- O4.096: 95.6 K (high)
- O4.097: 95.3 K (high)
- O4.101: 93.4 K (low)
- O4.103: 92.5 K (low)
- O4.150: 97.0 K (high)
- O4.162: 97.4 K (high)
- O4.200: 94.2 K (low)
- O4.220: 97.0 K (high)

Induced rule: For O-Cu-Ba-Hg compounds: high if (oxygen mod 0.1 is in [0.080, 0.099] and not equal to 0.085) or in [0.140, 0.179] or equal to 0.220.

## Step D: Final Pick
Unmeasurable materials predicted high:
1. O4.160Cu1Ba2Hg1 (4 elements)
2. O6.160Ca1Cu2Ba2Hg1 (5 elements)
3. O6.270Ca1Cu2Ba2Hg1 (5 elements)

Choose one with fewest elements: O4.160Cu1Ba2Hg1 (4 elements).

## Final Answer
- Peak: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- Threshold: 95.0 K
- Q1: 6 materials with exactly 4 elements (O, Cu, Ba, Hg) and Tc ≥ 95.0 K
- Predictions: See answer.json
- Final: O4.160Cu1Ba2Hg1

## Budget Status
Used: 33 units
Remaining: 12 units

## Budget Status
Used: 21 units
Remaining: 24 units
Measuring all 13 materials would use 13 units, leaving 11 units for Steps C and D.