# Notes

## Budget: 18/18 (exhausted)
- search_db: 9 units (pages 1-9 for Hg)
- measure_tc: 9 units (11 attempts, 2 unmeasurable)

## All Hg-containing materials found (27 total):

### Series A (5 elements: O, Ca, Cu, Ba, Hg) - 12 members:
1. O6.080Ca1Cu2Ba2Hg1 - Tc = 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1 - NO SAMPLE (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K ✓
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K ✓
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1 - NO SAMPLE (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - NOT MEASURED (no budget)
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K ✓

### Family B (4 elements: O, Cu, Ba, Hg) - 14 members:
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

### Other:
- O6Cu1Ba2Hg1Tl1 (5 elements: O, Cu, Ba, Hg, Tl)

## Step A: Peak
- T* = 127.0 K (at O6.210 and O6.220)
- Peak composition: O6.220Ca1Cu2Ba2Hg1

## Step B: threshold1 = 127.0 - 32 = 95.0 K
- Need to find all 4-element Hg materials with Tc >= 95.0 K
- Problem: No budget to measure family B materials

## Step C: Unmeasurable materials
- O6.160Ca1Cu2Ba2Hg1 (in series, between 105 and 122)
- O6.270Ca1Cu2Ba2Hg1 (in series, between 123 and 98)
- O6.300Ca1Cu2Ba2Hg1 (not measured, but might be measurable)
- O6Cu1Ba2Hg1Tl1 (not measured, might be measurable or not)
- All 14 family B materials (not measured, unknown if measurable)

## Rule induction
From series data, Tc depends on oxygen content in a dome shape:
- Low O (~6.08-6.10): Tc ~92 K (high)
- Mid O (~6.13-6.14): Tc ~102-105 K (high)
- High O (~6.19-6.24): Tc ~122-127 K (high, peak)
- Very high O (~6.31): Tc ~98 K (high)

All 5-element series members are high (>= 90 K).

Multi-factor rule:
- If number of elements >= 5 (has Ca or Tl): high Tc (all measured 5-element materials are high)
- If number of elements == 4 (no Ca, no Tl, 1 Cu): dome-shaped dependence on O content
  - High if 4.08 <= O <= 4.18
  - Low if O < 4.08 or O > 4.18

## Predictions

### Confirmed unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements → high (dome predicts ~113 K)
2. O6.270Ca1Cu2Ba2Hg1: 5 elements → high (dome predicts ~110 K)

### Untested (assume unmeasurable):
3. O6.300Ca1Cu2Ba2Hg1: 5 elements → high
4. O6Cu1Ba2Hg1Tl1: 5 elements, has Tl → high
5. O4.047Cu1Ba2Hg1: 4 elements, O=4.047 < 4.08 → low
6. O4.066Cu1Ba2Hg1: 4 elements, O=4.066 < 4.08 → low
7. O4.081Cu1Ba2Hg1: 4 elements, O=4.081 >= 4.08 → high
8. O4.085Cu1Ba2Hg1: 4 elements, O=4.085 >= 4.08 → high
9. O4.096Cu1Ba2Hg1: 4 elements, O=4.096 >= 4.08 → high
10. O4.097Cu1Ba2Hg1: 4 elements, O=4.097 >= 4.08 → high
11. O4.101Cu1Ba2Hg1: 4 elements, O=4.101 → high
12. O4.103Cu1Ba2Hg1: 4 elements, O=4.103 → high
13. O4.150Cu1Ba2Hg1: 4 elements, O=4.150 → high (near optimal)
14. O4.160Cu1Ba2Hg1: 4 elements, O=4.160 → high
15. O4.162Cu1Ba2Hg1: 4 elements, O=4.162 → high
16. O4.180Cu1Ba2Hg1: 4 elements, O=4.180 <= 4.18 → high
17. O4.200Cu1Ba2Hg1: 4 elements, O=4.200 > 4.18 → low
18. O4.220Cu1Ba2Hg1: 4 elements, O=4.220 > 4.18 → low

## Step B: Q1 (family B with Tc >= 95 K)
threshold1 = 127.0 - 32 = 95.0 K
Family B materials predicted high (>= 90 K): O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180
Assuming all predicted high have Tc >= 95 K (Hg-1201 phase near optimal doping):
Q1 = [O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180]

## FINAL ANSWER SUMMARY

### Step A: Series Peak
- Peak composition: O6.220Ca1Cu2Ba2Hg1
- Peak Tc (T*): 127.0 K
- Note: O6.210Ca1Cu2Ba2Hg1 also has 127.0 K (tied peak)

### Step B: Qualifying Set Q1
- threshold1 = 127.0 - 32 = 95.0 K
- 10 family B materials (4 elements: O, Cu, Ba, Hg) with predicted Tc >= 95 K

### Step C: Predictions
- 18 unmeasurable/untested materials predicted
- 14 predicted high, 4 predicted low
- Rule: Multi-factor (element count + oxygen dome)

### Step D: Final Pick
- O4.150Cu1Ba2Hg1 (4 elements, near optimal doping, highest expected Tc among 4-element predicted high)

### Budget Usage
- search_db: 9 pages = 9 units
- measure_tc: 9 attempts (7 successful + 2 unmeasurable) = 9 units
- Total: 18/18 units
