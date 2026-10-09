# Superconductor Tc Study Notes

## Starting Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Total budget: 34 units (shared between search_db and measure_tc)
- High Tc: Tc >= 90 K
- Task has 4 steps (A, B, C, D)

## Budget Tracking
Initial budget: 34 units
Used: 34 units (search_db: 16 units, measure_tc: 18 units)
Remaining: 0 units

## Final Results
**Step A - Series Peak:**
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0 K
- Alternative peak: O6.220Ca1Cu2Ba2Hg1 (also 127.0 K)

**Step B - Filter Family B:**
- Threshold1 = T* - 32 = 127 - 32 = 95.0 K
- Qualifying materials (Q1): O4.081Cu1Ba2Hg1 (98.0 K), O4.096Cu1Ba2Hg1 (95.6 K), O4.097Cu1Ba2Hg1 (95.3 K), O4.150Cu1Ba2Hg1 (97.0 K)

**Step C - Extrapolate:**
- Unmeasurable materials discovered: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- Rule induced: Tc is high if (material contains Ca) OR (for Hg-containing 4-element materials, the last two digits of oxygen content do not end with 5, 01, or 03)
- All unmeasurable materials predicted HIGH

**Step D - Final Pick:**
- Among unmeasurable materials predicted HIGH: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1 (all have 4 elements)
- Tie broken by higher expected Tc: O4.160Cu1Ba2Hg1 chosen (similar to O4.150Cu1Ba2Hg1 with Tc = 97.0 K)

## Step A: Series Peak Identification
Goal: Find homologous doping series members (same element set: O, Ca, Cu, Ba, Hg) with varying oxygen content.
Need to measure Tc of series members to find maximum Tc (T*).

Approach:
1. Search for materials containing all elements (Ca, Cu, Ba, Hg) to find series members
2. Measure Tc of each found material
3. Identify composition with maximum Tc

## Step B: Filter Family B
Goal: Among materials that contain Hg and have exactly 4 elements, report all with Tc >= threshold1 (T* - 32).

## Step C: Extrapolate
Goal: Induce rule from measurable materials to predict high/low for unmeasurable ones.

## Step D: Final Pick
Goal: Among unmeasurable materials predicted high, choose one with fewest elements (break ties by higher expected Tc).

## Actions
1. Start by searching for series members
2. Track budget carefully

## Search Results

### Hg-containing materials
Page 1 results:
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1

Page 2 results:
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1

Note: 27 total matches, has_more: true

### Ca-containing materials
Page 1 results:
- O10Ca2Cu3Ba2Tl2
- O6.080Ca1Cu2Ba2Hg1 (seed)

Note: 15 total matches, has_more: true

### Cu-containing materials
Page 1 results:
- O10Ca2Cu3Ba2Tl2
- O4.047Cu1Ba2Hg1

Note: 35 total matches, has_more: true

### Ba-containing materials
Page 1 results:
- O10Ca2Cu3Ba2Tl2
- O4.047Cu1Ba2Hg1

Note: 34 total matches, has_more: true

### Ca1-containing materials (homologous series members)
Page 1 results:
- O6.080Ca1Cu2Ba2Hg1 (seed)
- O6.100Ca1Cu2Ba2Hg1

Page apropos results:
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1

Page 3 results:
- O6.160Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1

Page 4 results:
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1

Page 5 results:
- O6.240Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1

Page 6 results:
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

Page 7 results:
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (not part of series)

Note: 13 total matches, has_more: false

## Homologous Series Members (Ca1Cu2Ba2Hg1 with varying oxygen)
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1 - No sample exists (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1 - No sample exists (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K

Total: 12 members

### Ca and Hg combined search
Page 1 results: No materials contain both Ca and Hg (search query issue)

## Observations
1. Found homologous series member: O6.100Ca1Cu2Ba2Hg1
2. There are 13 total matches for "Ca1", meaning there are likely more homologous series members