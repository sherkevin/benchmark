# Notes for Superconductor Tc Study

Budget: 18 units total (EXHAUSTED)

## Step A — Series peak (COMPLETE)
Seed: O6.080Ca1Cu2Ba2Hg1
Homologous series: O6.xxxCa1Cu2Ba2Hg1 (varying oxygen)

Measured Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (high)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (high)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (high)
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (high)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (high) ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (high) ← PEAK
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (high)
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (high)
- O6.310Ca1Cu2Ba2Hg1: NOT MEASURED (budget exhausted)

T* = 127.0 K
Peak composition: O6.210Ca1Cu2Ba2Hg1

## Step B — Filter family B (INCOMPLETE)
threshold1 = T* - 32 = 127 - 32 = 95

4-element Hg materials found (but NOT measured):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1

Cannot determine qualifying set Q1 without Tc measurements.
Leaving qualifying as empty list.

## Step C — Predictions (PARTIAL)
Unmeasurable materials found:
- O6.160Ca1Cu2Ba2Hg1 (5 elements) - predicted HIGH (interpolated Tc ~112-115)
- O6.270Ca1Cu2Ba2Hg1 (5 elements) - predicted HIGH (interpolated Tc ~110-115)

Rule: Tc follows parabolic doping curve with oxygen content. In the O-Ca-Cu-Ba-Hg 
homologous series, oxygen content between 6.08-6.30 yields high Tc (>=90K). 
Peak occurs at O6.21-0.22.

## Step D — Final pick
Both unmeasurable materials have 5 elements (tie).
Tie-breaker: higher expected Tc.
O6.160 is closer to peak (between 105 and 122), expected ~112-115 K
O6.270 is past peak (between 123 and 100), expected ~110-115 K

Final: O6.160Ca1Cu2Ba2Hg1 (slightly higher expected Tc)

## Budget Breakdown
- search_db calls: 9 (Hg page 1, Ca page 1, O6 page 1, Ca1Cu2Ba2Hg1 pages 1-4) = 7 calls... wait let me recount
  - "Hg" page 1: 1
  - "Ca" page 1: 1
  - "O6" page 1: 1
  - "Ca1Cu2Ba2Hg1" pages 1-4: 4
  - "O5Ca1Cu2Ba2Hg1": 1
  - "O7Ca1Cu2Ba2Hg1": 1
  Total search: 9 units

- measure_tc calls: 11 successful (O6.080 through O6.300, skipping the two no-sample)
  Actually: 10 successful + 2 no-sample = 12 attempts, but the last one (O6.310) failed
  So 11 measurements = 11 units? No wait...
  
Let me recount:
1. O6.080: measured (92.0) - budget 10
2. O6.100: measured (92.5) - budget 11
3. O6.130: measured (102.0) - budget 12
4. O6.140: measured (105.0) - budget 13
5. O6.160: no sample - budget 14
6. O6.190: measured (122.0) - budget 15
7. O6.210: measured (127.0) - budget 16
8. O6.220: measured (127.0) - budget 17
9. O6.240: measured (123.0) - budget 18
10. O6.270: no sample - budget 18 (error? no, it said budget_used 18)
11. O6.300: measured (100.0) - budget 18
12. O6.310: failed (budget exhausted)

Wait, looking at the results again:
- After O6.240, budget_used was 17
- O6.270 returned null with budget_used 18
- O6.300 returned 100.0 with budget_used 18... that's odd

Actually re-reading:
- O6.240: budget_used 17
- O6.270: budget_used 18 (no sample - but still costs 1 unit?)
- O6.300: budget_used 18 (measured successfully?)
- O6.310: error "instrument time exhausted"

Hmm, the O6.300 result shows budget_used: 18 but it returned a valid measurement. Maybe the budget check happens before the measurement?

Regardless, budget is now 0 and I cannot make more measurements or searches.
