# Task Progress Notes

## Step A - Series Peak
Need to measure the series members in data/measurable.txt to find the composition with maximum Tc.

Measurements completed:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAX)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAX)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1)
Maximum Tc (T*): 127.0 K

## Step B - Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Materials containing Hg with exactly 4 elements:
- O4.081Cu1Ba2Hg1: Tc = 98.0 K (>= 95.0) ✓
- O4.162Cu1Ba2Hg1: Tc = 97.4 K (>= 95.0) ✓
- O4.150Cu1Ba2Hg1: Tc = 97.0 K (>= 95.0) ✓
- O4.220Cu1Ba2Hg1: Tc = 97.0 K (>= 95.0) ✓
- O4.096Cu1Ba2Hg1: Tc = 95.6 K (>= 95.0) ✓
- O4.097Cu1Ba2Hg1: Tc = 95.3 K (>= 95.0) ✓
- O4.047Cu1Ba2Hg1: Tc = 95.0 K (>= 95.0) ✓
- O4.200Cu1Ba2Hg1: Tc = 94.2 K (< 95.0) ✗

Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1]

## Step C - Rule Induction and Prediction
Used all measurement budget. Based on measured materials, I'll predict for unmeasurable materials:
Rule: High Tc materials typically have specific oxygen stoichiometry ranges (around 6-7 for Ca-Ba-Cu-Hg systems, around 4-5 for Cu-Ba-Hg systems) combined with heavy metals like Hg, Tl, Pb, etc. The balance between oxygen content and metal ratios appears crucial.

Unmeasurable materials predictions:
- O6.160Ca1Cu2Ba2Hg1: HIGH (similar to successful Ca1Cu2Ba2Hg1 series, oxygen content near peak)
- O6.270Ca1Cu2Ba2Hg1: LOW (oxygen content further from optimal range)
- O4.180Cu1Ba2Hg1: HIGH (similar to successful Cu1Ba2Hg1 series, oxygen content in good range)
- O8Ca0.850Cu2.200Ba2Tl1.800: HIGH (contains Tl, high oxygen content)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: HIGH (contains Tl and Pb, high oxygen content)
- O8Cu3Y1Ba1.900La0.100: HIGH (complex structure with high oxygen content)
- O8Cu4Y1Ba1.870La0.130: HIGH (complex structure with high oxygen content)

## Step D - Final Pick
From high predictions in Step C, finding the one with fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements  
- O4.180Cu1Ba2Hg1: 4 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 6 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements
- O8Cu3Y1Ba1.900La0.100: 5 elements
- O8Cu4Y1Ba1.870La0.130: 5 elements

The one with fewest elements among high predictions is O4.180Cu1Ba2Hg1 (4 elements).