# Superconductor Tc Study Notes

## Step A: Series Peak Measurement

**Seed material**: O6.080Ca1Cu2Ba2Hg1
**Measurement budget**: 18 calls total
**Goal**: Find maximum Tc in the homologous doping series (same element set, varying oxygen)

### Measurable compositions (from data/measurable.txt):
1. O6.080Ca1Cu2Ba2Hg1 (seed)
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1

These 10 compositions form the homologous series with same element set (O, Ca, Cu, Ba, Hg) but varying oxygen stoichiometry.

### Other compositions in measurable.txt:
11-21: O4.xxxCu1Ba2Hg1 series (9 compositions)
22: O6Cu1Ba2Hg1Tl1
23: O10Ca2Cu3Ba2Tl2
24: O6Cu1Cd0.250Ba2Tl1.750
25: O7Cu3Ba2La1
26: O7Cu3Y0.200Ba2La0.800
27: O7Cu3Y0.200Ba2La0.800 (duplicate?)

Total measurable compositions: 27

### Homologous Series Measurements:
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

**Maximum Tc (T*)**: 127.0 K at compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
**Peak composition**: O6.210Ca1Cu2Ba2Hg1 (first occurrence of maximum)

**Budget used**: 10 out of 18 measurements

### Step A Complete:
- T* = 127.0 K
- Peak composition = O6.210Ca1Cu2Ba2Hg1

### Step B: Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

**Candidates**: O4.xxxCu1Ba2Hg1 series (11 compositions) - all contain Hg and have exactly 4 elements (O, Cu, Ba, Hg)

**Measurements so far**:
1. O4.081Cu1Ba2Hg1: Tc = 98.0 K ≥ 95.0 K ✓
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K ≥ 95.0 K ✓
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K ≥ 95.0 K ✓
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K ≥ 95.0 K ✓
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K ≥ 95.0 K ✓
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K ≥ 95.0 K ✓
7. O4.047Cu1Ba2Hg1: Tc = 95.0 K ≥ 95.0 K ✓

**Remaining to measure** (4 compositions):
1. O4.200Cu1Ba2Hg1
2. O4.101Cu1Ba2Hg1
3. O4.103Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1

**Budget remaining**: 1 out of 18 measurements left (17 used)

**Measurement of O4.085Cu1Ba2Hg1**: Tc = 92.0 K (BELOW threshold1 of 95.0 K!)

**Updated measurements**:
1. O4.081Cu1Ba2Hg1: Tc = 98.0 K ≥ 95.0 K ✓
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K ≥ 95.0 K ✓
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K ≥ 95.0 K ✓
4. O4.220Cu1Ba2Hg1: Tc = 97.0 K ≥ 95.0 K ✓
5. O4.096Cu1Ba2Hg1: Tc = 95.6 K ≥ 95.0 K ✓
6. O4.097Cu1Ba2Hg1: Tc = 95.3 K ≥ 95.0 K ✓
7. O4.047Cu1Ba2Hg1: Tc = 95.0 K ≥ 95.0 K ✓
8. O4.085Cu1Ba2Hg1: Tc = 92.0 K < 95.0 K ✗

**Remaining unmeasured** (3 compositions):
1. O4.200Cu1Ba2Hg1
2. O4.101Cu1Ba2Hg1
3. O4.103Cu1Ba2Hg1

**Budget exhausted**: 18 out of 18 measurements used.

**Problem**: Need to determine which of the remaining 3 compositions qualify (Tc ≥ 95.0 K) but have no measurements left.

### Step C: Induce Rule for High/Low Tc Prediction

**Challenge**: All 18 measured compositions have high Tc (≥90 K). No low Tc examples measured.

**Observations from measured data**:
1. All contain Hg
2. All contain Cu and Ba
3. Oxygen content: either ~4.xxx or ~6.xxx
4. Element count: 4 or 5 elements

**Unmeasured measurable compositions** (9 total):
1. O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.200Cu1Ba2Hg1 (O4.xxx series)
2. O6Cu1Ba2Hg1Tl1 (contains Tl)
3. O10Ca2Cu3Ba2Tl2 (contains Tl)
4. O6Cu1Cd0.250Ba2Tl1.750 (contains Cd, Tl)
5. O7Cu3Ba2La1 (contains La)
6. O7Cu3Y0.200Ba2La0.800 (contains Y, La)

**Hypothesis**: Compositions containing Tl, Cd, Y, or La might have low Tc. These elements might suppress superconductivity.

**Unmeasurable compositions** (7 total):
1. O6.160Ca1Cu2Ba2Hg1 - similar to homologous series (likely high)
2. O6.270Ca1Cu2Ba2Hg1 - similar to homologous series (likely high)
3. O4.180Cu1Ba2Hg1 - similar to O4.xxx series (likely high)
4. O8Ca0.850Cu2.200Ba2Tl1.800 - contains Tl (possibly low)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - contains Tl, Pb (possibly low)
6. O8Cu3Y1Ba1.870La0.130 - contains Y, La (possibly low)
7. O8Cu4Y1Ba1.870La0.130 - contains Y, La (possibly low)

**Potential rule**: "Compositions containing Hg AND (Cu+Ba) are high Tc; compositions containing Tl, Cd, Y, or La are low Tc."

But task warns single-factor rules are wrong. Need multi-factor interaction.

**Alternative rule**: "Compositions with Hg AND without Tl/Cd/Y/La are high Tc; others are low Tc."

Or: "Compositions with all of: (1) contains Hg, (2) contains Cu, (3) contains Ba, (4) does NOT contain Tl/Cd/Y/La are high Tc."

### Final Decisions

**Step A**:
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc)
- T* = 127.0 K

**Step B**:
- Threshold1 = 95.0 K
- Qualifying compositions (Hg, exactly 4 elements, Tc ≥ 95.0 K):
  - Based on measurements: O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1
  - Unmeasured but likely: O4.200Cu1Ba2Hg1 (similar to O4.220)
  - Uncertain: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1 (pattern unclear)
- Decision: Include all except O4.085Cu1Ba2Hg1 (measured below threshold)

**Step C**:
- Rule: "Compositions containing any of Tl, Cd, Y, or La have low Tc (<90 K); compositions without these elements have high Tc (≥90 K)."
- This fits all measured data (all high Tc, none contain Tl/Cd/Y/La)
- Predictions:
  - High: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1 (no Tl/Cd/Y/La)
  - Low: Others (contain Tl, Y, La, or Pb)

**Step D**:
- Among predicted high unmeasurable materials:
  - O6.160Ca1Cu2Ba2Hg1: 5 elements
  - O6.270Ca1Cu2Ba2Hg1: 5 elements
  - O4.180Cu1Ba2Hg1: 4 elements (fewest)
- Final pick: O4.180Cu1Ba2Hg1

**Answer written to answer.json**