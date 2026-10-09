# Superconductor Tc Study Notes

## Step A: Series Peak Analysis

Seed material: O6.080Ca1Cu2Ba2Hg1
Need to measure series members in measurable.txt to find composition with MAXIMUM Tc (T*).

Looking at measurable.txt, I see:
1. Lines 1-10 appear to be a homologous doping series with the same element set (O, Ca, Cu, Ba, Hg) but varying oxygen content
2. Lines 11-21 appear to be another series with O, Cu, Ba, Hg (4 elements)
3. Lines 22-27 are various other compositions

The seed belongs to a homologous doping series (same element set, varying oxygen). Based on the instructions, I need to measure the series members in data/measurable.txt that belong to the same series as the seed (O6.080Ca1Cu2Ba2Hg1).

Looking at the data, the series appears to be lines 1-10:
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

I need to measure Tc for all 10 of these to find the maximum.

## Step A Measurements Results:

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

Maximum Tc (T*) = 127.0 K (for both O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Need to pick one as the peak composition. According to instructions: "find the composition with the MAXIMUM Tc". Both have same Tc = 127.0 K. I'll need to check if there's a tie-breaking rule. Looking at TASK.md, it doesn't specify tie-breaking for Step A, but for Step D it says "break ties by higher expected Tc". Since both have same measured Tc, I'll pick O6.210Ca1Cu2Ba2Hg1 (first occurrence).

So:
- Peak composition: O6.210Ca1Cu2Ba2Hg1
- T* = 127.0 K
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Budget used: 10 measurements remaining: 8

## Step B: Filter Family B (contains Hg and has exactly 4 elements)

Candidates from measurable.txt (lines 11-21):
11. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓ (>= 95.0 K)
12. O4.162Cu1Ba2Hg1: Tc = 97.4 K ✓
13. O4.150Cu1Ba2Hg1: Tc = 97.0 K ✓
14. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓
15. O4.096Cu1Ba2Hg1: Tc = 95.6 K ✓
16. O4.097Cu1Ba2Hg1: Tc = 95.3 K ✓
17. O4.047Cu1Ba2Hg1: Tc = 95.0 K ✓

Remaining to measure (but only 1 measurement left in budget):
18. O4.200Cu1Ba2Hg1
19. O4.101Cu1Ba2Hg1  
20. O4.103Cu1Ba2Hg1
21. O4.085Cu1Ba2Hg1

Problem: Need to measure all 11 compositions to know which qualify, but only have 1 measurement left. Budget miscalculation: I've used 17 measurements total (10 for Step A + 7 for Step B), 1 remaining out of 18.

Need to reconsider approach. Looking at the data, I see a pattern in the O4.xCu1Ba2Hg1 series - all seem to have Tc around 95-98 K based on what I've measured so far. The threshold is 95.0 K, and all 7 I measured were >= 95.0 K. It's possible ALL of them qualify.

But I need to be sure. Let me check if there are any other compositions in measurable.txt that contain Hg and have exactly 4 elements besides these 11. Looking at lines 22-27:
22. O6Cu1Ba2Hg1Tl1 - contains Hg but has 5 elements (O, Cu, Ba, Hg, Tl)
23. O10Ca2Cu3Ba2Tl2 - no Hg
24. O6Cu1Cd0.250Ba2Tl1.750 - no Hg (has Cd)
25. O7Cu3Ba2La1 - no Hg
26. O7Cu3Y0.200Ba2La0.800 - no Hg

So only the 11 compositions (lines 11-21) are candidates.

Given my limited budget, I should use my last measurement strategically. All 7 I've measured so far qualified, suggesting the remaining 4 might also qualify. But I need to check at least one to confirm the pattern holds.

Used last measurement on O4.200Cu1Ba2Hg1: Tc = 94.2 K (BELOW threshold of 95.0 K)

So not all compositions in the series qualify. Pattern seems to be: lower oxygen stoichiometry gives higher Tc.

Unmeasured compositions (oxygen values):
- O4.101Cu1Ba2Hg1 (oxygen = 4.101) - between O4.096 (95.6 K) and O4.081 (98.0 K)
- O4.103Cu1Ba2Hg1 (oxygen = 4.103) - similar  
- O4.085Cu1Ba2Hg1 (oxygen = 4.085) - between O4.081 (98.0 K) and O4.096 (95.6 K)

Based on the trend (higher oxygen → lower Tc), and since O4.200 (4.200) gave 94.2 K while O4.096 (4.096) gave 95.6 K, these three with oxygen between 4.085-4.103 should have Tc between 95.6-98.0 K, all above threshold.

But I can't be 100% certain without measuring. However, I've used all 18 measurements, so I must make educated predictions.

## Step B Summary:
Threshold1 = 127.0 - 32 = 95.0 K

Qualifying compositions (Tc >= 95.0 K):
1. O4.081Cu1Ba2Hg1 (98.0 K) ✓
2. O4.162Cu1Ba2Hg1 (97.4 K) ✓
3. O4.150Cu1Ba2Hg1 (97.0 K) ✓
4. O4.220Cu1Ba2Hg1 (97.0 K) ✓
5. O4.096Cu1Ba2Hg1 (95.6 K) ✓
6. O4.097Cu1Ba2Hg1 (95.3 K) ✓
7. O4.047Cu1Ba2Hg1 (95.0 K) ✓

Predicted to qualify (based on oxygen trend):
8. O4.101Cu1Ba2Hg1 (estimated ~96-97 K)
9. O4.103Cu1Ba2Hg1 (estimated ~96-97 K)  
10. O4.085Cu1Ba2Hg1 (estimated ~97 K)

Does not qualify:
11. O4.200Cu1Ba2Hg1 (94.2 K) ✗

So Q1 (set of qualifying compositions) should include all except O4.200Cu1Ba2Hg1.

## Step C: Induce Rule for High/Low Tc Prediction

Challenge: I've measured 18 compositions, ALL with Tc >= 90 K (high). No low Tc examples in my measurements.

Measured compositions pattern:
- All contain Hg (mercury)
- All are either O-Ca-Cu-Ba-Hg (5 elements) or O-Cu-Ba-Hg (4 elements)
- All have Tc >= 90 K

Unmeasurable materials (7 compositions):
1. O6.160Ca1Cu2Ba2Hg1 - contains Hg, 5 elements
2. O6.270Ca1Cu2Ba2Hg1 - contains Hg, 5 elements  
3. O4.180Cu1Ba2Hg1 - contains Hg, 4 elements
4. O8Ca0.850Cu2.200Ba2Tl1.800 - no Hg (Tl instead), 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - no Hg (Tl, Pb), 6 elements
6. O8Cu3Y1Ba1.900La0.100 - no Hg, 5 elements
7. O8Cu4Y1Ba1.870La0.130 - no Hg, 5 elements

The warning says single-factor rules like "contains Hg" are wrong on several. So Hg alone doesn't determine high/low.

Need to consider interaction of factors. Looking at oxygen stoichiometry patterns:

For O-Ca-Cu-Ba-Hg series (Step A):
- O6.080: 92.0 K
- O6.100: 92.5 K
- O6.130: 102.0 K
- O6.140:176.0 K
- O6.190: 122.0 K
- O6.210: 127.0 K (peak)
- O6.220: 127.0 K
- O6.240: 123.0 K
- O6.300: 100.0 K
- O6.310: 98.0 K

Pattern: Tc increases with oxygen up to ~6.22, then decreases.

For O-Cu-Ba-Hg series (Step B):
- O4.047: 95.0 K
- O4.081: 98.0 K
- O4.085: ? (unmeasured, estimated high)
- O4.096: 95.6 K
- O4.097: 95.3 K
- O4.101: ? (unmeasured, estimated high)
- O4.103: ? (unmeasured, estimated high)
- O4.150: 97.0 K
- O4.162: 97.4 K
- O4.180: ? (unmeasurable, needs prediction)
- O4.200: 94.2 K
- O4.220: 97.0 K

Pattern: Seems optimal around O4.08-O4.16 range, decreases at higher oxygen (O4.200: 94.2 K).

Potential rule: Materials with Hg AND optimal oxygen stoichiometry for their series are high Tc. But materials without Hg might also be high Tc if they meet other criteria.

Actually, I realize I haven't measured any compositions without Hg. All my measurements are biased toward Hg-containing materials. The unmeasurable materials without Hg (4-7) could be either high or low Tc.

Let me look at the measurable compositions I didn't measure (lines 22-27):
22. O6Cu1Ba2Hg1Tl1 - contains Hg AND Tl
23. O10Ca2Cu3Ba2Tl2 - no Hg, has Tl
24. O6Cu1Cd0.250Ba2Tl1.750 - no Hg, has Cd, Tl
25. O7Cu3Ba2La1 - no Hg
26. O7Cu3Y0.200Ba2La0.800 - no Hg

These might have low Tc, but I can't measure them now (budget exhausted).

Need to induce rule from limited data. Perhaps: Materials containing both Hg AND (Ca OR in optimal oxygen range) are high Tc. Or: Materials without Hg but with certain other elements (Tl? La? Y?) might be low Tc.