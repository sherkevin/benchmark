# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls (ALL USED)
- High Tc: >= 90 K, Low Tc: < 90 K

## All Measurements (18 total)

### Series 2: O4.xxxCu1Ba2Hg1 (4 elements: O,Cu,Ba,Hg) — 11 calls
- O4.081Cu1Ba2Hg1: 98.0 K (HIGH)
- O4.162Cu1Ba2Hg1: 97.4 K (HIGH)
- O4.150Cu1Ba2Hg1: 97.0 K (HIGH)
- O4.220Cu1Ba2Hg1: 97.0 K (HIGH)
- O4.096Cu1Ba2Hg1: 95.6 K (HIGH)
- O4.097Cu1Ba2Hg1: 95.3 K (HIGH)
- O4.047Cu1Ba2Hg1: 95.0 K (HIGH)
- O4.200Cu1Ba2Hg1: 94.2 K (HIGH)
- O4.101Cu1Ba2Hg1: 93.4 K (HIGH)
- O4.103Cu1Ba2Hg1: 92.5 K (HIGH)
- O4.085Cu1Ba2Hg1: 92.0 K (HIGH)

### Other compositions — 5 calls
- O6Cu1Ba2Hg1Tl1: 0.0 K (LOW)
- O10Ca2Cu3Ba2Tl2: 118.97 K (HIGH)
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (LOW)
- O7Cu3Ba2La1: 89.6 K (LOW)
- O7Cu3Y0.200Ba2La0.800: 81.0 K (LOW)

### Series 1: O6.xxxCa1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg) — 2 calls
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (HIGH)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (HIGH)

## Step A: Peak
T* = 123.0 K at O6.240Ca1Cu2Ba2Hg1

## Step B: Filter family B
threshold1 = 123.0 - 32 = 91.0 K
4-element Hg-containing measurable materials: Series 2 (all 11)
All have Tc >= 91.0 (range 92.0–98.0), so ALL qualify.
Q1 = all 11 Series 2 compositions

## Step C: Rule induction

HIGH materials (measured):
- O4.xxxCu1Ba2Hg1: Hg=yes, Tl=no, Ca=no → HIGH
- O6.xxxCa1Cu2Ba2Hg1: Hg=yes, Tl=no, Ca=yes → HIGH  
- O10Ca2Cu3Ba2Tl2: Hg=no, Tl=yes, Ca=yes → HIGH

LOW materials (measured):
- O6Cu1Ba2Hg1Tl1: Hg=yes, Tl=yes, Ca=no → LOW (has both Hg AND Tl)
- O6Cu1Cd0.250Ba2Tl1.750: Hg=no, Tl=yes, Ca=no → LOW (has Tl but no Ca)
- O7Cu3Ba2La1: Hg=no, Tl=no, Ca=no → LOW
- O7Cu3Y0.200Ba2La0.800: Hg=no, Tl=no, Ca=no → LOW

**Rule: HIGH if (contains Hg AND NOT Tl) OR (contains Ca AND Tl)**

Verification:
- "contains Hg" alone would be WRONG: O6Cu1Ba2Hg1Tl1 has Hg but is LOW
- "contains Tl" alone would be WRONG: O6Cu1Cd0.250Ba2Tl1.750 has Tl but is LOW
- The interaction is key: Hg works only without Tl; Tl works only with Ca

### Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: Hg=yes, Tl=no → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Hg=yes, Tl=no → HIGH
3. O4.180Cu1Ba2Hg1: Hg=yes, Tl=no → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ca=yes, Tl=yes, Hg=no → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Ca=yes, Tl=yes, Hg=no → HIGH
6. O8Cu3Y1Ba1.900La0.100: no Hg, no Tl, no Ca → LOW
7. O8Cu4Y1Ba1.870La0.130: no Hg, no Tl, no Ca → LOW

## Step D: Final pick
Predicted HIGH unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, fewest among predicted HIGH)
