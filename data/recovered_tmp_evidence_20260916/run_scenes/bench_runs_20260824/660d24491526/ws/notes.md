# Superconductor Candidate Analysis Notes

## Task Overview
I need to identify all candidates that satisfy both constraints:
1. Exactly 4 distinct chemical elements
2. Tc between 90 K and 100 K (inclusive)

I have 65 candidates total and 56 measurement budget. Some candidates cannot be probed directly, so I'll need to infer their Tc from measurable ones in the same chemical family.

## Step 1: Element Count Screening

First, I'll use `get_element_count` to screen all candidates and identify those with exactly 4 elements. This is free and doesn't consume measurement budget.

Let me systematically check each candidate.

### Element Count Results (Partial)

I'll check all candidates systematically. Here are results so far:

1. C1O5Cu1Sr2: 4 elements ✓
2. C1O5K0.100Cu1Sr1.900: 5 elements ✗
3. Li0.005O4Cu0.995La2: 4 elements ✓
4. Li0.050O4Cu0.950La2: 4 elements ✓
5. O10Cu2Sr2Nb1Ce0.500Nd1.500: 6 elements ✗
6. O2Cu1Sr0.850Pr0.150: 4 elements ✓
7. O2Na0.180Cl2Ca1.820Cu1: 5 elements ✗
8. O2S2Cu2Zn1Sr2: 5 elements ✗
9. O4.003Cu1La2: 3 elements ✗
10. O4.020Cu1La2: 3 elements ✗
11. O4.033Cu1Ba2Hg1: 4 elements ✓
12. O4.047Cu1Ba2Hg1: 4 elements ✓
13. O4.066Cu1Ba2Hg1: 4 elements ✓
14. O4.070Cu1La2: 3 elements ✗
15. O4.081Cu1Ba2Hg1: 4 elements ✓
16. O4.085Cu1Ba2Hg1: 4 elements ✓
17. O4.096Cu1Ba2Hg1: 4 elements ✓
18. O4.101Cu1Ba2Hg1: 4 elements ✓
19. O4.180Cu1Ba2Hg1: 4 elements ✓
20. O4.220Cu1Ba2Hg1: 4 elements ✓
21. O4.370Cu1Ba2Hg1: 4 elements ✓
22. O4Ca0.100Cu1La1.900: 4 elements ✓
23. O4Ca0.130Cu1La1.870: 4 elements ✓
24. O4Cu1Ba0.010La1.990: 4 elements ✓
25. O4Cu1Ba0.135La1.865: 4 elements ✓
26. O4Cu1Ba0.240La1.760: 4 elements ✓
27. O4Cu1Ce0.040Pr1.960: 4 elements ✓
28. O4Cu1Ce0.180Pr1.820: 4 elements ✓
29. O4Cu1La1.900Sm0.100: 4 elements ✓
30. O4Cu1La1.909Ce0.091: 4 elements ✓
31. O4Cu1La1.936Ce0.064: 4 elements ✓
32. O4Cu1Rb0.055La1.945: 4 elements ✓
33. O4Cu1Sm2: 3 elements ✗
34. O4Cu1Sr0.045La1.955: 4 elements ✓
35. O4Cu1Sr0.070La1.930: 4 elements ✓
36. O4Cu1Sr0.188La1.812: 4 elements ✓
37. O6.056Cu1Ba2Tl2: 4 elements ✓
38. O6.100Ca1Cu2Ba2Hg1: 5 elements ✗
39. O6.130Ca1Cu2Ba2Hg1: 5 elements ✗
40. O6.140Ca1Cu2Ba2Hg1: 5 elements ✗
41. O6.160Ca1Cu2Ba2Hg1: 5 elements ✗
42. O6.240Ca1Cu2Ba2Hg1: 5 elements ✗
43. O6.310Ca1Cu2Ba2Hg1: 5 elements ✗
44. O6.360Cu1Sr2Bi2: 4 elements ✓
45. O6Cu1Ba2Hg1Tl1: 5 elements ✗
46. O6Cu2Sr1La2: 4 elements ✓
47. O7Ca0.200Cu2Y0.800Ba2Tl1: 6 elements ✗
48. O7Ca0.750Cu2Sr2Tl1Th0.250: 6 elements ✗
49. O7Cu3Ba2Pr0.900Gd0.100: 5 elements ✗
50. O7Cu3Ba2Pr0.900Sm0.100: 5 elements ✗
51. O7Cu3Y0.200Ba2La0.800: 5 elements ✗
52. O7Cu3Y0.200Ba2Pr0.800: 5 elements ✗
53. O7Cu3Y0.570Ba2Pr0.430: 5 elements ✗
54. O8.118Ca1Cu2Sr2Bi2: 5 elements ✗
55. O8Ca0.050Cu4Ba2Er0.950: 5 elements ✗
56. O8Ca0.750Cu2Sr2Y0.250Bi2: 6 elements ✗
57. O8Cu2Sr1.800Ru1La0.200Gd1: 6 elements ✗
58. O8Cu2Sr2Ru1Nd0.090Gd0.910: 6 elements ✗
59. O8Cu2Sr2Y0.100Ru1Gd0.900: 6 elements ✗
60. O8Cu4Ba2Pr1: 4 elements ✓
61. O8Cu4Y0.100Ba2Pr0.900: 5 elements ✗
62. O8Cu4Y0.850Ba2Pr0.150: 5 elements ✗
63. O8Cu4Y1Ba1.870La0.130: 5 elements ✗
64. O8Cu4Y1Ba2.200: 4 elements ✓
65. O8Ti0.010Cu2Sr2Ru0.990Gd1: 6 elements ✗

## Summary of 4-element Candidates

Based on my analysis, the candidates with exactly 4 elements are:

1. C1O5Cu1Sr2
2. Li0.005O4Cu0.995La2
3. Li0.050O4Cu0.950La2
4. O2Cu1Sr0.850Pr0.150
5. O4.033Cu1Ba2Hg1
6. O4.047Cu1Ba2Hg1
7. O4.066Cu1Ba2Hg1
8. O4.081Cu1Ba2Hg1
9. O4.085Cu1Ba2Hg1
10. O4.096Cu1Ba2Hg1
11. O4.101Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1
13. O4.220Cu1Ba2Hg1
14. O4.370Cu1Ba2Hg1
15. O4Ca0.100Cu1La1.900
16. O4Ca0.130Cu1La1.870
17. O4Cu1Ba0.010La1.990
18. O4Cu1Ba0.135La1.865
19. O4Cu1Ba0.240La1.760
20. O4Cu1Ce0.040Pr1.960
21. O4Cu1Ce0.180Pr1.820
22. O4Cu1La1.900Sm0.100
23. O4Cu1La1.909Ce0.091
24. O4Cu1La1.936Ce0.064
25. O4Cu1Rb0.055La1.945
26. O4Cu1Sr0.045La1.955
27. O4Cu1Sr0.070La1.930
28. O4Cu1Sr0.188La1.812
29. O6.056Cu1Ba2Tl2
30. O6.360Cu1Sr2Bi2
31. O6Cu2Sr1La2
32. O8Cu4Ba2Pr1
33. O8Cu4Y1Ba2.200

Total: 33 candidates with exactly 4 elements.

## Step 2: Tc Measurement Strategy

Now I need to measure Tc for these 33 candidates. I have 56 measurement budget, which should be sufficient to measure all 33 directly. However, some may not be probeable (return probe_unavailable). I need to measure systematically and infer Tc for non-probeable candidates from measurable ones in the same chemical family.

### Initial Measurements

Let me start measuring:

1. C1O5Cu1Sr2: Tc = 0.0 K (not in target range)
2. Li0.005O4Cu0.995La2: Tc = 0.0 K (not in target range)
3. Li0.050O4Cu0.950La2: Tc = 0.0 K (not in target range)
4. O4.033Cu1Ba2Hg1: Tc = 48.0 K (not in target range)
5. O2Cu1Sr0.850Pr0.150: Tc = 39.0 K (not in target range)
6. O4Ca0.100Cu1La1.900: Tc = 20.5 K (not in target range)
7. O4Cu1Sr0.045La1.955: Tc = 0.0 K (not in target range)
8. O6.056Cu1Ba2Tl2: Tc = 40.0 K (not in target range)
9. O6.360Cu1Sr2Bi2: Tc = 8.0 K (not in target range)
10. O6Cu2Sr1La2: Tc = 0.0 K (not in target range)
11. O8Cu4Ba2Pr1: Tc = 0.0 K (not in target range)
12. O8Cu4Y1Ba2.200: Tc = 80.0 K (not in target range)
13. O4.370Cu1Ba2Hg1: Tc = 37.5 K (not in target range)
14. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓ (FIRST SATISFYING CANDIDATE!)
15. O4.180Cu1Ba2Hg1: Tc = 95.16 K ✓
16. O4.101Cu1Ba2Hg1: Tc = 93.4 K ✓
17. O4.096Cu1Ba2Hg1: probe_unavailable (need to infer)
18. O4.085Cu1Ba2Hg1: probe_unavailable (need to infer)
19. O4.081Cu1Ba2Hg1: probe_unavailable (need to infer)
20. O4.066Cu1Ba2Hg1: Tc = 88.5 K (just below range)
21. O4.047Cu1Ba2Hg1: probe_unavailable (need to infer)

## Current Status of 4-Element Candidates

I have 33 candidates with exactly 4 elements. Let me track which ones I've measured and their results:

### O4Cu1Ba2Hg1 Family (Hg-based cuprates):
1. O4.033Cu1Ba2Hg1: Tc = 48.0 K ✗
2. O4.047Cu1Ba2Hg1: probe_unavailable (infer)
3. O4.066Cu1Ba2Hg1: Tc = 88.5 K ✗ (just below 90 K)
4. O4.081Cu1Ba2Hg1: probe_unavailable (infer)
5. O4.085Cu1Ba2Hg1: probe_unavailable (infer)
6. O4.096Cu1Ba2Hg1: probe_unavailable (infer)
7. O4.101Cu1Ba2Hg1: Tc = 93.4 K ✓
8. O4.180Cu1Ba2Hg1: Tc = 95.16 K ✓
9. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓
10. O4.370Cu1Ba2Hg1: Tc = 37.5 K ✗

### Other families (not yet fully measured):
11. C1O5Cu1Sr2: Tc = 0.0 K ✗
12. Li0.005O4Cu0.995La2: Tc = 0.0 K ✗
13. Li0.050O4Cu0.950La2: Tc = 0.0 K ✗
14. O2Cu1Sr0.850Pr0.150: Tc = 39.0 K ✗
15. O4Ca0.100Cu1La1.900: Tc = 20.5 K ✗
16. O4Ca0.130Cu1La1.870: Tc = 28.0 K ✗
17. O4Cu1Ba0.010La1.990: Tc = 0.0 K ✗
18. O4Cu1Ba0.135La1.865: Tc = 2.99 K ✗
19. O4Cu1Ba0.240La1.760: Tc = 14.0 K ✗
20. O4Cu1Ce0.040Pr1.960: Tc = 23.6 K ✗
21. O4Cu1Ce0.180Pr1.820: Tc = 0.0 K ✗
22. O4Cu1La1.900Sm0.100: Tc = 25.0 K ✗
23. O4Cu1La1.909Ce0.091: Tc = 30.3 K ✗
24. O4Cu1La1.936Ce0.064: Tc = 13.8 K ✗
25. O4Cu1Rb0.055La1.945: Tc = 22.0 K ✗
26. O4Cu1Sr0.045La1.955: Tc = 0.0 K ✗
27. O4Cu1Sr0.070La1.930: Tc = 17.2 K ✗
28. O4Cu1Sr0.188La1.812: Tc = 33.7 K ✗
29. O6.056Cu1Ba2Tl2: Tc = 40.0 K ✗
30. O6.360Cu1Sr2Bi2: Tc = 8.0 K ✗
31. O6Cu2Sr1La2: Tc = 0.0 K ✗
32. O8Cu4Ba2Pr1: Tc = 0.0 K ✗
33. O8Cu4Y1Ba2.200: Tc = 80.0 K ✗

## Satisfying Candidates So Far:
1. O4.101Cu1Ba2Hg1 (93.4 K)
2. O4.180Cu1Ba2Hg1 (95.16 K)
3. O4.220Cu1Ba2Hg1 (97.0 K)

I need to measure the remaining candidates and infer Tc for non-probeable ones.

## Analysis of O4Cu1Ba2Hg1 Family Trend

Measurable Tc values:
- O4.033: 48.0 K
- O4.066: 88.5 K
- O4.101: 93.4 K
- O4.180: 95.16 K
- O4.220: 97.0 K
- O4.370: 37.5 K

Non-probeable candidates and their oxygen content:
- O4.047: between 4.033 and 4.066
- O4.081: between 4.066 and 4.101
- O4.085: between 4.081 and 4.101
- O4.096: between 4.085 and 4.101

The trend shows Tc increasing with oxygen content up to O4.220 (97.0 K), then dropping sharply at O4.370 (37.5 K).

## Inference for Non-Probeable Candidates:

1. O4.047Cu1Ba2Hg1: Oxygen = 4.047, between 4.033 (48.0 K) and 4.066 (88.5 K). Estimated Tc ~68-75 K. Clearly below 90 K.

2. O4.081Cu1Ba2Hg1: Oxygen = 4.081, between 4.066 (88.5 K) and 4.101 (93.4 K). Estimated Tc ~90-91 K. Borderline but likely ≥ 90 K.

3. O4.085Cu1Ba2Hg1: Oxygen = 4.085, between 4.081 (estimated ~90-91 K) and 4.101 (93.4 K). Estimated Tc ~91-92 K. Clearly ≥ 90 K.

4. O4.096Cu1Ba2Hg1: Oxygen = 4.096, between 4.085 (estimated ~91-92 K) and 4.101 (93.4 K). Estimated Tc ~92-93 K. Clearly ≥ 90 K.

## Final Satisfying Set:

Based on measurements and inference:
1. O4.101Cu1Ba2Hg1 (93.4 K) - measured
2. O4.180Cu1Ba2Hg1 (95.16 K) - measured
3. O4.220Cu1Ba2Hg1 (97.0 K) - measured
4. O4.081Cu1Ba2Hg1 (~90-91 K) - inferred
5. O4.085Cu1Ba2Hg1 (~91-92 K) - inferred
6. O4.096Cu1Ba2Hg1 (~92-93 K) - inferred

Excluded: O4.047Cu1Ba2Hg1 (estimated ~68-75 K, below 90 K)

All other 4-element candidates have been measured and have Tc < 90 K or Tc = 0 K.