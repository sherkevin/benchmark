# Superconductor Tc Study Notes

## Initial Setup
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Total budget: 18 units (shared between measure_tc and search_db)
- Task has 4 steps (A, B, C, D)

## Budget Tracking
- Starting budget: 18 units
- Used so far: 2 units (search_db queries)
- Remaining budget: 16 units

## Step A: Find homologous series peak
Goal: Find series members with same element set (Ca, Cu, Ba, Hg) and varying oxygen content, then measure Tc to find maximum T*.

### Observations:
1. Seed: O6.080Ca1Cu2Ba2Hg1
2. Element set: O, Ca, Cu, Ba, Hg
3. Need to find other materials with same element set but different oxygen content

### Search strategy:
- Search for "CaCuBaHg" combinations
- Search for materials with all these elements

### Found homologous series members (all with pattern O[x]Ca1Cu2Ba2Hg1):
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

### Tc Measurements for Homologous Series:
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K
5. O6.160Ca1Cu2Ba2Hg1 - Unmeasurable (no sample)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K
10. O6.270Ca1Cu2Ba2Hg1 - Unmeasurable (no sample)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K

### Step A Results:
- Maximum Tc (T*) = 127.0 K
- Composition with maximum Tc: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1 with same Tc)
- Both have Tc = 127.0 K, but O6.210Ca1Cu2Ba2Hg1 appears first

### Additional Hg-containing materials found (page 2):
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1

These have exactly 4 elements (O, Cu, Ba, Hg) and could qualify for Step B.

### All Hg-containing materials found (with exactly 4 elements):
From page 1:
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1

From page 2:
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1

### Other materials discovered but not measured:
1. O10Ca2Cu3Ba2Tl2 (from Ca search page 1)
2. O7Ca1Cu2Sr2Tl0.500Pb0.500 (from Ca search page 5)
3. O8Ca0.850Cu2.200Ba2Tl1.800 (from Ca search page 5)

These might be unmeasurable, but I haven't tried to measure them due to budget constraints.

### Lessons learned:
- Used budget inefficiently by measuring all homologous series members without checking if some might be unmeasurable
- Should have been more strategic with measurements
- Could have saved budget for Steps B, C, and D
- Should have measured a diverse set of materials to induce a better rule for Step C

### Final Answer Summary:
**Step A:**
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- T* = 127.0 K
- Threshold1 = T* - 32 = 95.0 K

**Step B:**
- Found 6 materials with Hg and exactly 4 elements: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
- Could not measure their Tc due to budget constraints
- "qualifying" array is empty

**Step C:**
- Induced rule: Materials in the O[x]Ca1Cu2Ba2Hg1 homologous series with oxygen content x between 6.08 and 6.31 exhibit high Tc (>= 90 K)
- Unmeasurable materials found: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1
- Predicted both as "high" based on the rule

**Step D:**
- Among unmeasurable materials predicted as high (both have 5 elements)
- Chose O6.160Ca1Cu2Ba2Hg1 as final because:
  1. Both have 5 elements (tie)
  2. O6.160 has higher expected Tc based on parabolic trend (closer to peak)

### Budget update:
- Budget used: 18/18 units
- No budget remaining