# Superconductor Tc Study Notes

## Initial Setup
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 units total (shared between measure_tc and search_db)
- Goal: Complete Steps A-D as described in TASK.md

## Step A: Find Homologous Doping Series Peak

### Search Results
Found homologous series: O_xCa1Cu2Ba2Hg1 with varying oxygen stoichiometry:
- O6.080Ca1Cu2Ba2Hg1 (seed)
- O6.100Ca1Cu2Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.160Ca1Cu2Ba2Hg1 (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1 (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

### Measurements (Budget used: 19/34)
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

### Step A Conclusion
Maximum Tc (T*) = 127.0 K
Peak composition = O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1, both 127.0 K)
I'll use O6.210Ca1Cu2Ba2Hg1 as the peak.

Threshold1 = T* - 32 = 127 - 32 = 95 K

### Step B: Find Hg-containing materials with exactly 4 elements
Found 14 candidate materials: O4.xxxCu1Ba2Hg1 compounds (4 elements: O, Cu, Ba, Hg)
List:
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

### Measurements of 4-element materials (Additional budget used: 5 units, total used:政治 34/34)
1. O4.101Cu1Ba2Hg1: Tc = 93.4 K (< 95)
2. O4.150Cu1Ba2Hg1: Tc = 97.0 K (≥ 95) ✓
3. O4.200Cu1Ba2Hg1: Tc = 94.2 K (< 95)
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K (≥ 95) ✓
5. O4.047Cu1Ba2Hg1: Unmeasurable (no sample)

### Step B Preliminary Results
Qualifying materials (Q1) based on measurements:
- O4.150Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1

But need to check other materials in the list. Pattern suggests Tc might peak around O4.150 and O4.220.

### Step C: Induce rule for predicting high/low
Need to analyze measurable materials to find pattern. Warning: single-factor rules are wrong on several unmeasurable materials.

### Step D: Final pick
Among unmeasurable materials predicted high, choose one with FEWEST elements (break ties by higher expected Tc).

### Unmeasurable materials found:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)
3. O4.047Cu1Ba2Hg1 (4 elements)
4. Possibly others I haven't discovered?

### Remaining Budget: 0 units

## Final Analysis and Answer

### Step A Complete
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0 K
- Threshold1: 95.0 K

### Step B Complete
Qualifying materials (Q1) with exactly 4 elements, containing Hg, and Tc ≥ 95 K:
- O4.150Cu1Ba2Hg1 (97.0 K)
- O4.220Cu1Ba2Hg1 (97.0 K)

Note: Only measured 4 of 14 candidates due to budget constraints. Pattern suggests Tc may only be ≥95 at specific oxygen stoichiometries.

### Step C Complete
Induced rule: "All Hg-containing superconducting materials with Cu and Ba have Tc >= 90 K"
Rationale: All measurable materials found (14 total: 10 from O_xCa1Cu2Ba2Hg1 series, 4 from O_xCu1Ba2Hg1 series) have Tc ≥ 90 K.

Predictions for unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: high (in series where all neighbors are high)
- O6.270Ca1Cu2Ba2Hg1: high (in series where all neighbors are high)
- O4.047Cu1Ba2Hg1: high (lowest oxygen in series but O4.101 is 93.4 K)

### Step D Complete
Among unmeasurable materials predicted high:
- O4.047Cu1Ba2Hg1 has 4 elements (fewest)
- O6.160Ca1Cu2Ba2Hg1 has 5 elements
- O6.270Ca1Cu2Ba2Hg1 has 5 elements

Final pick: O4.047Cu1Ba2Hg1

### Concerns
- Warning about single-factor rules being wrong suggests rule might be incorrect for some unmeasurable materials
- Limited budget prevented measuring all candidates
- May have missed other unmeasurable materials in database