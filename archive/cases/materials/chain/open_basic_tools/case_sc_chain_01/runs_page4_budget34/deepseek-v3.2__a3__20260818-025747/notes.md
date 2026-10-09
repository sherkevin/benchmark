# Superconductor Tc Study Notes

## Initial Setup
- Budget: 34 units total for both search_db and measure_tc
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- High Tc defined as >= 90 K
- Need to complete 4 steps (A, B, C, D)
- Budget used:73 units (9 search_db queries)
- Budget remaining: 25 units

## Step A - Series Peak
Seed: `O6.080Ca1Cu2Ba2Hg1`
Goal: Find homologous doping series (same element set, varying oxygen). Measure series members to find maximum Tc (T*).

### Hg-containing materials search results:
Total of 27 Hg-containing materials found across 7 pages.

### Homologous series identified:
Materials with composition Ca1Cu2Ba2Hg1 with varying oxygen:
1. O6.080Ca1Cu2Ba2Hg1 (seed)
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.160Ca1Cu2Ba2Hg1
6. O6.190Ca1Cu2Ba2Hg1
7. O6.210Ca1Cu2Ba2Hg1
8. O6.220Ca1Cu2Ba2Hg1
9. O6.240Ca1Cu2Ba2Hg1
10. O6.270Ca1Cu2Ba2Hg1
11. O6.300Ca1Cu2Ba2Hg1
12. O6.310Ca1Cu2Ba2Hg1

Total of 12 materials in the homologous series.

### Tc Measurements for homologous series:
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

### Step A Results:
- Maximum Tc (T*) = 127.0 K
- Composition with maximum Tc: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1 - both 127.0 K)
- I will use O6.210Ca1Cu2Ba2Hg1 as the peak composition
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

### Step B - Filter Family B:
Need to find all materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc >= 95.0 K

From Hg search results, 14 materials have exactly 4 elements (O_xCu1Ba2Hg1):
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1
10. O4.160Cu1Ba2Hg1
11. O4.162Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1
13. O4.200Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1

### Step B Measurements for 4-element materials (threshold1 = 95.0 K):
1. O4.047Cu1Ba2Hg1: Unmeasurable (no sample)
2. O4.066Cu1Ba2Hg1: Unmeasurable (no sample)
3. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓ QUALIFIES
4. O4.085Cu1Ba2Hg1: Tc = 92.0 K ✗
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K ✓ QUALIFIES
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K ✓ QUALIFIES
7. O4.101Cu1Ba2Hg1: Tc = 93.4 K ✗
8. O4.103Cu1Ba2Hg1: Tc = 92.5 K ✗
9. O4.150Cu1Ba2Hg1: Tc = 97.0 K ✓ QUALIFIES
10. O4.160Cu1Ba2Hg1: Unmeasurable (no sample)
11. O4.162Cu1Ba2Hg1: Tc = 97.4 K ✓ QUALIFIES
12. O4.180Cu1Ba2Hg1: Unmeasurable (no sample)
13. O4.200Cu1Ba2Hg1: Tc = 94.2 K ✗
14. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓ QUALIFIES

### Q1 (qualifying materials with Tc >= 95.0 K):
1. O4.081Cu1Ba2Hg1
2. O4.096Cu1Ba2Hg1
3. O4.097Cu1Ba2Hg1
4. O4.150Cu1Ba2Hg1
5. O4.162Cu1Ba2Hg1
6. O4.220Cu1Ba2Hg1

### Additional measurement:
O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (LOW - Tc < 90 K)

### Summary of measurements:
- Total measured: 21 materials
- All materials with composition O_xCu1Ba2Hg1 or O_xCa1Cu2Ba2Hg1 have Tc >= 90 K (HIGH)
- O6Cu1Ba2Hg1Tl1 has Tc = 0.0 K (LOW) - contains Tl
- Unmeasurable materials (6): O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1

### Step C - Rule Induction:
Need to induce a rule from measurable materials to predict high/low for unmeasurable ones.
Warning: "single-factor rules are wrong on several of them."

Observations:
1. All O_xCu1Ba2Hg1 materials (4 elements) have Tc >= 90 K (HIGH)
2. All O_xCa1Cu2Ba2Hg1 materials (5 elements) have Tc >= 90 K (HIGH)  
3. O6Cu1Ba2Hg1Tl1 (contains Tl) has Tc = 0.0 K (LOW)
4. Pattern: Materials containing Tl seem to have low Tc?

But warning says single-factor rules are wrong. Maybe need to consider oxygen content or other factors.

### Step C - Predictions:
Unmeasurable materials (6):
1. O6.160Ca1Cu2Ba2Hg1 - predicted HIGH (similar to other Ca-containing materials)
2. O6.270Ca1Cu2Ba2Hg1 - predicted HIGH (in decreasing Tc region but likely > 90 K)
3. O4.047Cu1Ba2Hg1 - predicted HIGH (similar to other O_xCu1Ba2Hg1 materials)
4. O4.066Cu1Ba2Hg1 - predicted HIGH (similar to other O_xCu1Ba2Hg1 materials)
5. O4.160Cu1Ba2Hg1 - predicted HIGH (similar to other O_xCu1Ba2Hg1 materials)
6. O4.180Cu1Ba2Hg1 - predicted HIGH (similar to other O_xCu1Ba2Hg1 materials)

Rule: Materials containing Hg but not containing Tl have high Tc (>= 90 K). Materials containing Tl have low Tc (< 90 K).

### Step D - Final Pick:
Among unmeasurable materials predicted HIGH, choose the one with FEWEST elements (break ties by higher expected Tc).

Unmeasurable materials predicted HIGH (all 6):
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O4.180Cu1Ba2Hg1: 4 elements

Materials with fewest elements (4): O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1

Need to break ties by higher expected Tc. Based on oxygen stoichiometry trend for O_xCu1Ba2Hg1 materials, higher oxygen generally correlates with higher Tc (with some variation). Among these, O4.180Cu1Ba2Hg1 has highest oxygen (4.180), so expected highest Tc.

Final pick: O4.180Cu1Ba2Hg1

### New Discovery:
Searched for Tl-containing materials (5 total):
1. O10Ca2Cu3Ba2Tl2 - measured: Tc = 118.97 K (HIGH)
2. O6Cu1Ba2Hg1Tl1 - measured: Tc = 0.0 K (LOW)
3. O6Cu1Cd0.250Ba2Tl1.750 - not measured
4. O7Ca1Cu2Sr2Tl0.500Pb0.500 - not measured
5. O8Ca0.850Cu2.200Ba2Tl1.800 - not measured

Rule update: Simple "contains Tl" rule is wrong. O10Ca2Cu3Ba2Tl2 has high Tc despite containing Tl.

Observations:
- O6Cu1Ba2Hg1Tl1 contains Hg AND Tl, has low Tc
- O10Ca2Cu3Ba2Tl2 contains Tl but no Hg, has high Tc
- All materials with Hg (without Tl) have high Tc
- Maybe combination of Hg AND Tl causes low Tc?

But need more data. Have only 1 budget unit left.

### Final Measurements and Rule:
Used last budget unit to measure O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (LOW)

Summary of Tl-containing materials:
1. O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (LOW) - contains Hg and Tl, no Ca
2. O6Cu1Cd0.250Ba2Tl1.750: Tc =344980 K (LOW) - contains Tl and Cd, no Ca
3. O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (HIGH) - contains Tl and Ca

Refined rule: Materials without Tl have high Tc. Materials with Tl have high Tc only if they also contain Ca; otherwise low Tc.

All unmeasurable materials discovered (via Hg search) don't contain Tl, so predicted HIGH.

### Final Answer Summary:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- Threshold: 95.0 K
- Q1: 6 qualifying materials with 4 elements and Tc >= 95.0 K
- Predictions: All 6 unmeasurable materials predicted HIGH
- Final pick: O4.180Cu1Ba2Hg1 (fewest elements among predicted HIGH, highest oxygen among ties)

Budget used: 34 units (11 searches + 23 measurements)
Budget remaining: 0 units