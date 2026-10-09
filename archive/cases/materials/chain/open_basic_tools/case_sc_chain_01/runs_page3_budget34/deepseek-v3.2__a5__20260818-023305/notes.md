# Superconductor Tc Study Notes
**Budget: 34 units total** (shared between search_db and measure_tc)
**Seed material:** O6.080Ca1Cu2Ba2Hg1

## Initial Observations
- Seed composition: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90K, otherwise low
- Need to discover homologous doping series with same element set, varying oxygen
- Need to search database using search_db queries

## Plan
1. **Step A**: Find homologous series members (same elements: O, Ca, Cu, Ba, Hg with varying oxygen stoichiometry)
2. **Step B**: Among materials containing Hg with exactly 4 elements, find those with Tc >= threshold1 (T* - 32)
3. **Step C**: Induce rule from measurable materials to predict high/low for unmeasurable ones
4. **Step D**: Choose unmeasurable material predicted high with fewest elements

## Homologous Series Members Found (Step A)
From searching "Ca":
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0K (measurable)
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5K (measurable)
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0K (measurable)
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0K (measurable)
5. O6.160Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0K (measurable)
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0K (measurable)
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0K (measurable)
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0K (measurable)
10. O6.270Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0K (measurable)
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0K (measurable)

Also found other Ca-containing materials (not part of series):
- O10Ca2Cu3Ba2Tl2
- O7Ca1Cu2Sr2Tl0.500Pb0.500
- O8Ca0.850Cu2.200Ba2Tl1.800

## Step A Results
Maximum Tc (T*) = 127.0K from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0K

## Step B Materials (Hg-containing, exactly 4 elements)
List of 4-element Hg materials (O[x]Cu1Ba2Hg1):
1. O4.047Cu1Ba2Hg1 - No sample (unmeasurable)
2. O4.066Cu1Ba2Hg1 - No sample (unmeasurable)
3. O4.081Cu1Ba2Hg1 - Tc = 98.0K (>= 95.0K, QUALIFIES)
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1 - Tc = 97.0K (>= 95.0K, QUALIFIES)
10. O4.160Cu1Ba2Hg1
11. O4.162Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1 - No sample (unmeasurable)
13. O4.200Cu1Ba2Hg1 - Tc = 94.2K (< 95.0K, DOES NOT QUALIFY)
14. O4.220Cu1Ba2Hg1

Threshold1 = 95.0K
Qualifying for Step B so far: O4.081Cu1Ba2Hg1, O4.150Cu1Ba2Hg1

## Other Measured Materials
- O6Cu1Ba2Hg1Tl1: Tc = 0.0K (LOW) - contains Hg but has Tl
- All homologous series materials (O6.080Ca1Cu2Ba2Hg1 to O6.310Ca1Cu2Ba2Hg1): HIGH (92.0K to 127.0K)

## Unmeasurable Materials Found So Far
1. O6.160Ca1Cu2Ba2Hg1 (homologous series)
2. O6.270Ca1Cu2Ba2Hg1 (homologous series)
3. O4.047Cu1Ba2Hg1 (4-element Hg)
4. O4.066Cu1Ba2Hg1 (4-element Hg)
5. O4.180Cu1Ba2Hg1 (4-element Hg)

## Final Analysis
**Step A Results:**
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0K
- Threshold1: 95.0K

**Step B Qualifying Materials (Q1):**
- O4.081Cu1Ba2Hg1 (Tc = 98.0K)
- O4.150Cu1Ba2Hg1 (Tc = 97.0K)

**Step C Rule Induction:**
From measured data:
- All materials without Tl are high Tc (≥90K)
- O6Cu1Ba2Hg1Tl1 (with Tl) is low Tc (0K)
- Rule: Materials containing thallium (Tl) have low Tc (0K), while materials without thallium have high Tc (≥90K)

**Step D Final Pick:**
Among unmeasurable materials predicted high:
- O4.047Cu1Ba2Hg1 (4 elements, lowest oxygen among unmeasurable 4-element materials)
- O4.066Cu1Ba2Hg1 (4 elements)
- O4.180Cu1Ba2Hg1 (4 elements)
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)

Choose O4.047Cu1Ba2Hg1 (fewest elements = 4, tie broken by higher expected Tc based on oxygen trend).

## Budget Tracking
Initial budget: 34 units
Used: 34 units (20 search_db + 14 measure_tc)
Remaining: 0 units