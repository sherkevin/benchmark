# Superconductor Tc Study Notes
Date: 2026-08-18

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, otherwise low
- Measurement budget: 18 `measure_tc` calls
- Need to complete 4 steps: A, B, C, D

## Step A - Series Peak
Goal: Measure homologous doping series members in `data/measurable.txt` (same element set, varying oxygen) and find composition with MAXIMUM Tc (T*).

The seed belongs to a homologous doping series with same element set, varying oxygen. From `data/measurable.txt`, I need to identify which compositions have the same element set as the seed (O6.080Ca1Cu2Ba2Hg1).

Looking at the seed: O6.080Ca1Cu2Ba2Hg1
Elements: O, Ca, Cu, Ba, Hg

From measurable.txt, compositions with same element set:
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

Also note: There are unmeasurable compositions in `data/unmeasurable.txt` with same element set:
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1

These are unmeasurable (no physical sample) per step C instructions.

I need to measure Tc for all 10 measurable compositions in this series to find maximum Tc.

Measurement budget: 18 calls total for entire task.

## Step A Results
Measured Tc values for homologous series (O-Ca-Cu-Ba-Hg with varying oxygen):

1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

Maximum Tc (T*) = 127.0 K
Composition with maximum Tc: O6.210Ca1Cu2Ba2Hg1 (tied with O6.220Ca1Cu2Ba2Hg1 at 127.0 K)
Need to check tie-breaking rules. The task says "find the composition with the MAXIMUM Tc". If there's a tie, I should probably take the first one in the list or check oxygen content. Since O6.210 comes before O6.220 in the list, I'll use O6.210Ca1Cu2Ba2Hg1.

Budget used: 10/18

## Step B - Filter Family B
Goal: threshold1 = T* - 32 = 127.0 - 32 = ‚Öõ95.0 K
Among materials that contain Hg and have exactly 4 elements, report ALL whose Tc >= threshold1 (95.0 K). Call this set Q1.

First, I need to identify all materials in `data/measurable.txt` that:
1. Contain Hg
2. Have exactly 4 elements

Then measure their Tc values to see which ones are >= 95.0 K.

From `data/measurable.txt`, compositions that contain Hg and have exactly 4 elements:
1. O4.081Cu1Ba2Hg1
2. O4.162Cu1Ba2Hg1
3. O4.150Cu1Ba2Hg1
4. O4.220Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.047Cu1Ba2Hg1
8. O4.200Cu1Ba2Hg1
9. O4.101Cu1Ba2Hg1
10. O4.103Cu1Ba2Hg1
11. O4.085Cu1Ba2Hg1

That's 11 compositions. I have 8 measurements left (10 used, 18 total budget). I need to measure all 11 to determine Q1, but I only have 8 measurements left. This is a problem.

Wait, I need to re-check. I've used 10 measurements for Step A. I have 8 left for Steps B, C, and D. But Step C doesn't require measurements (unmeasurable materials). Step D only requires selecting from predictions made in Step C. So I need to use my 8 remaining measurements for Step B.

But I have 11 compositions to measure for Step B. I need to be strategic. Maybe some will obviously be below threshold or I can sample some to infer pattern.

Let me measure a few to see the range.

## Step B Measurements (so far)
Threshold: 95.0 K
Measured:
1. O4.081Cu1Ba2Hg1: 98.0 K ✓ QUALIFIES
2. O4.162Cu1Ba2Hg1: 97.4 K ✓ QUALIFIES
3. O4.150Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES
4. O4.220Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES

All 4 measured so far qualify (Tc >= 95.0 K).
Budget used: 14/18 (4 left)

Remaining compositions to measure:
1. O4.096Cu1Ba2Hg1
2. O4.097Cu1Ba2Hg1
3. O4.047Cu1Ba2Hg1
4. O4.200Cu1Ba2Hg1
5. O4.101Cu1Ba2Hg1
6. O4.103Cu1Ba2Hg1
7. O4.085Cu1Ba2Hg1

I have 4 measurements left but 7 compositions remaining. I need to either:
1. Find a pattern to infer which might be below threshold without measuring all
2. Or accept that I can't measure all and need to make educated guesses

Looking at the pattern: All have similar compositions (O4.xxxCu1Ba2Hg1). The oxygen stoichiometry varies. From the 4 I measured:
- O4.081: 98.0 K
- O4.162: 97.4 K
- O4.150: 97.0 K
- O4.220: 97.0 K

The Tc seems relatively stable around 97-98 K for these O4.xxxCu1Ba2Hg1 compounds. All are well above 95.0 K threshold.

Let me check if there's a trend with oxygen content:
- O4.081 (low oxygen): 98.0 K (highest)
- O4.150 (medium): 97.0 K
- O4.162 (medium): 97.4 K
- O4.220 (higher): 97.0 K

No clear decreasing trend with oxygen content. All are above threshold.

Given that all 4 measured are above threshold and the compositions are very similar, it's likely most or all of the remaining 7 are also above threshold. But I need to be certain for the answer.

Let me measure 4 more strategically to cover different oxygen values:

Measured 4 more:
5. O4.096Cu1Ba2Hg1: 95.6 K ✓ QUALIFIES
6. O4.047Cu1Ba2Hg1: 95.0 K ✓ QUALIFIES (exactly at threshold)
7. O4.200Cu1Ba2Hg1: 94.2 K ✗ DOES NOT QUALIFY
8. O4.101Cu1Ba2Hg1: 93.4 K ✗ DOES NOT QUALIFY

Interesting! O4.200Cu1Ba2Hg1 (94.2 K) and O4.101Cu1Ba2Hg1 (93.4 K) are below threshold.
O4.096Cu1Ba2Hg1 (95.6 K) and O4.047Cu1Ba2Hg1 (95.0 K) are at or above threshold.

Budget used: 18/18 (all measurements used)

I still have 3 compositions unmeasured:
1. O4.097Cu1Ba2Hg1
2. O4.103Cu1Ba2Hg1
3. O4.085Cu1Ba2Hg1

I need to determine if these qualify without measuring them. Looking at the pattern:

Measured compositions and their Tc:
- O4.081: 98.0 K (✓)
- O4.162: 97.4 K (✓)
- O4.150: 97.0 K (✓)
- O4.220: 97.0 K (✓)
- O4.096: 95.6 K (✓)
- O4.047: 95.0 K (✓)
- O4.200: 94.2 K (✗)
- O4.101: 93.4 K (✗)

Pattern analysis:
1. O4.200 (high oxygen: 4.200) is below threshold (94.2 K)
2. O4.101 (medium oxygen: 4.101) is below threshold (93.4 K)
3. O4.047 (low oxygen: 4.047) is at threshold (95.0 K)
4. O4.096 (low oxygen: 4.096) is above threshold (95.6 K)
5. O4.081 (low oxygen: 4.081) is above threshold (98.0 K)
6. O4.150 (medium oxygen: 4.150) is above threshold (97.0 K)
7. O4.162 (medium oxygen: 4.162) is above threshold (97.4 K)
8. O4.220 (high oxygen: 4.220) is above threshold (97.0 K)

No clear pattern with oxygen content alone. O4.200 (high oxygen) is below, but O4.220 (also high oxygen) is above.

Let me look at the unmeasured ones:
- O4.097Cu1Ba2Hg1 (oxygen: 4.097) - similar to O4.096 (95.6 K ✓) and O4.101 (93.4 K ✗)
- O4.103Cu1Ba2Hg1 (oxygen: 4.103) - similar to O4.101 (93.4 K ✗)
- O4.085Cu1Ba2Hg1 (oxygen: 4.085) - between O4.081 (98.0 K ✓) and O4.096 (95.6 K ✓)

Based on proximity:
- O4.097 (4.097) is very close to O4.096 (4.096, 95.6 K ✓) and O4.101 (4.101, 93.4 K ✗). Might be borderline.
- O4.103 (4.103) is close to O4.101 (4.101, 93.4 K ✗). Likely below threshold.
- O4.085 (4.085) is between O4.081 (4.081, 98.0 K ✓) and O4.096 (4.096, 95.6 K ✓). Likely above threshold.

But I need to be certain. The task requires reporting ALL whose Tc >= threshold1. Without measurements, I can't be certain.

Wait, I've used all 18 measurements. I can't measure the remaining 3. I need to either:
1. Use the pattern to infer
2. Or acknowledge I can't measure them all

Given the instructions: "Use the `measure_tc` tool for every Tc value you cite — never guess a measured number." This suggests I should only report measured values. But I need to report Q1 which includes ALL qualifying compositions. If I haven't measured some, I can't know if they qualify.

However, looking back at the task description: "Measurement budget: 18 `measure_tc` calls." It seems the budget is designed to be sufficient for the task. Maybe I don't need to measure ALL compositions in Step B? Or maybe I need to be more strategic?

Let me re-examine. I measured 10 for Step A, 8 for Step B. That's 18 total. But there are 11 compositions for Step B. I can't measure all 11 with only 8 measurements left.

Perhaps I misunderstood Step B. Let me re-read: "Among materials that contains Hg and has exactly 4 elements, report ALL whose Tc >= threshold1." It doesn't say I need to measure ALL such materials. It says report ALL whose Tc >= threshold1. To know which ones qualify, I need to know their Tc values. But I only have 8 measurements.

Maybe the expectation is that I should use some measurements to establish a pattern, then infer for others? Or maybe I should prioritize measuring all for Step B and skip some in Step A? But Step A requires finding the maximum Tc in the series, which requires measuring all in the series.

Actually, looking at the homologous series in Step A: there are 10 compositions. I measured all 10. That's correct.
For Step B: there are 11 compositions that contain Hg and have exactly 4 elements. I can only measure 8 of them.

I think the solution is: I need to measure a representative sample, establish that most are above threshold, and report the ones I measured that qualify. The ones I didn't measure, I can't report because I don't know their Tc.

But the instruction says "report ALL whose Tc >= threshold1". If I don't measure some, I can't know if they qualify.

Let me check if there's another interpretation. Maybe "materials that contains Hg and has exactly 4 elements" refers to ALL materials in the dataset, not just measurable.txt? But step B comes after step A which uses measurable.txt. Likely step B also uses measurable.txt.

Actually, let me check if there are any materials in unmeasurable.txt that contain Hg and have exactly 4 elements. From unmeasurable.txt:
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements, contains Hg
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements, contains Hg
3. O4.180Cu1Ba2Hg1 - 4 elements: O, Cu, Ba, Hg ✓ contains Hg and exactly 4 elements!
4. O8Ca0.850Cu2.200Ba2Tl1.800 - 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - 6 elements
6. O8Cu3Y1Ba1.900La0.100 - 5 elements
7. O8Cu4Y1Ba1.870La0.130 - 5 elements

So O4.180Cu1Ba2Hg1 in unmeasurable.txt contains Hg and has exactly 4 elements. But it's unmeasurable (no sample). So for Step B, I should only consider measurable materials.

Given the budget constraint, I think I need to:
1. Report the 6 compositions I measured that qualify (O4.081, O4.162, O4.150, O4.220, O4.096, O4.047)
2. For the 3 unmeasured ones (O4.097, O4.103, O4.085), I cannot include them in Q1 because I don't know their Tc values.

This seems like the only logical approach given the budget.

## Step B Final Results
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Measured compositions containing Hg with exactly 4 elements:
1. O4.081Cu1Ba2Hg1: 98.0 K ✓ QUALIFIES
2. O4.162Cu1Ba2Hg1: 97.4 K ✓ QUALIFIES
3. O4.150Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES
4. O4.220Cu1Ba2Hg1: 97.0 K ✓ QUALIFIES
5. O4.096Cu1Ba2Hg1: 95.6 K ✓ QUALIFIES
6. O4.047Cu1Ba2Hg1: 95.0 K ✓ QUALIFIES (exactly at threshold)
7. O4.200Cu1Ba2Hg1: 94.2 K ✗ DOES NOT QUALIFY
8. O4.101Cu1Ba2Hg1: 93.4 K ✗ DOES NOT QUALIFY

Unmeasured compositions (cannot determine):
1. O4.097Cu1Ba2Hg1
2. O4.103Cu1Ba2Hg1
3. O4.085Cu1Ba2Hg1

Q1 (qualifying set) = {
  "O4.081Cu1Ba2Hg1",
  "O4.162Cu1Ba2Hg1",
  "O4.150Cu1Ba2Hg1",
  "O4.220Cu1Ba2Hg1",
  "O4.096Cu1Ba2Hg1",
  "O4.047Cu1Ba2Hg1"
}

## Step C - Extrapolate (no measurement possible)
Goal: Induce a rule that separates high (Tc >= 90 K) from low Tc using materials I CAN measure, then predict high/low for every unmeasurable material.

Unmeasurable materials (from `data/unmeasurable.txt`):
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1
4. O8Ca0.850Cu2.200Ba2Tl1.800
5. O7Ca1Cu2Sr2Tl0.500Pb0.500
6. O8Cu3Y1Ba1.900La0.100
7. O8Cu4Y1Ba1.870La0.130

I need to induce a rule based on measurable materials. I have measured:
1. Homologous series (O-Ca-Cu-Ba-Hg): 10 measurements
2. Hg-containing 4-element compounds: 8 measurements

Total measured: 18 compositions.

I need to analyze patterns in these 18 measurements to induce a rule that predicts high/low Tc (>=90 K or <90 K).

First, let me list all measured compositions with their Tc values and classify as high/low:

### All Measured Compositions (18 total):

#### Homologous Series (O-Ca-Cu-Ba-Hg):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K → HIGH (>=90)
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K → HIGH
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K → HIGH
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K → HIGH
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K → HIGH
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K → HIGH
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K → HIGH
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K → HIGH
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K → HIGH
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K → HIGH

#### Hg-containing 4-element compounds (O-Cu-Ba-Hg):
11. O4.081Cu1Ba2Hg1: 98.0 K → HIGH
12. O4.162Cu1Ba2Hg1: 97.4 K → HIGH
13. O4.150Cu1Ba2Hg1:157 97.0 K → HIGH
14. O4.220Cu1Ba2Hg1: 97.0 K → HIGH
15. O4.096Cu1Ba2Hg1: 95.6 K → HIGH
16. O4.047Cu1Ba2Hg1: 95.0 K → HIGH
17. O4.200Cu1Ba2Hg1: 94.2 K → HIGH (94.2 >= 90)
18. O4.101Cu1Ba2Hg1: 93.4 K → HIGH

Interesting! ALL 18 measured compositions are HIGH (Tc >= 90 K). There are NO measured LOW Tc compositions.

This presents a problem: How can I induce a rule to separate high from low if I have no low examples?

Wait, let me check the measurable.txt file again. There are 26 compositions total in measurable.txt. I measured 18. There are 8 compositions I didn't measure:
1. O6Cu1Ba2Hg1Tl1
2. O10Ca2Cu3Ba2Tl2
3. O6Cu1Cd0.250Ba2Tl1.750
4. O7Cu3Ba2La1
5. O7Cu3Y0.200Ba2La0.800
6. O4.097Cu1Ba2Hg1
7. O4.103Cu1Ba2Hg1
8. O4.085Cu1Ba2Hg1

I could measure some of these to get low Tc examples, but I've used all 18 measurements. I can't measure anymore.

But wait, the task says "the materials in `data/unmeasurable.txt` have never been synthesized — there is no physical sample to put in the instrument. `measure_tc` will tell you 'no sample exists' for them."

So for Step C, I need to induce a rule using materials I CAN measure. But I only have high Tc examples. How can I learn what makes a material low Tc?

Perhaps some of the unmeasured compositions in measurable.txt would be low Tc, but I can't measure them now.

Or perhaps the rule is based on compositional features that I can deduce from the high Tc examples. Maybe the absence of certain elements or combinations leads to low Tc.

Let me examine the unmeasurable materials and see if I can find patterns:

Unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 - Same as homologous series (O-Ca-Cu-Ba-Hg)
2. O6.270Ca1Cu2Ba2Hg1 - Same as homologous series (O-Ca-Cu-Ba-Hg)
3. O4.180Cu1Ba2Hg1 - Similar to O4.xxxCu1Ba2Hg1 series
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Contains Tl, not Hg
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Sr, Tl, Pb
6. O8Cu3Y1Ba1.900La0.100 - Contains Y, La
7. O8Cu4Y1Ba1.870La0.130 - Contains Y, La

From the homologous series I measured, oxygen content varies from 6.08 to 6.31. All are high Tc. The unmeasurable O6.160 and O6.270 are within this range (6.16, 6.27). Likely also high Tc.

From the O4.xxxCu1Ba2Hg1 series, oxygen varies from 4.047 to 4.220. All are high Tc. O4.180 is within this range. Likely also high Tc.

But I need a rule that might predict LOW Tc for some unmeasurable materials. The warning says: "single-factor rules (e.g. 'contains Hg' or 'has 5 elements') are wrong on several of them — the governing rule is an interaction of multiple factors."

So I need to find a multi-factor rule. But with only high Tc examples, this is challenging.

Maybe I should look at other compositions in measurable.txt that I didn't measure. Their element combinations might give clues.

Let me analyze the element combinations in all measurable compositions:

### All Compositions in measurable.txt (26 total):

#### Group 1: O-Ca-Cu-Ba-Hg series (10) - ALL MEASURED, ALL HIGH
1. O6.080Ca1Cu2Ba2Hg1
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1

#### Group 2: O-Cu-Ba-Hg series (11) - 8 MEASURED (ALL HIGH), 3 UNMEASURED
11. O4.081Cu1Ba2Hg1 ✓ HIGH
12. O4.162Cu1Ba2Hg1 ✓ HIGH
13. O4.150Cu1Ba2Hg1 ✓ HIGH
14. O4.220Cu1Ba2Hg1 ✓ HIGH
15. O4.096Cu1Ba2Hg1 ✓ HIGH
16. O4.097Cu1Ba2Hg1 ✗ UNMEASURED
17. O4.047Cu1Ba2Hg1 ✓ HIGH
18. O4.200Cu1Ba2Hg1 ✓ HIGH
19. O4.101Cu1Ba2Hg1 ✓ HIGH
20. O4.103Cu1Ba2Hg1 ✗ UNMEASURED
21. O4.085Cu1Ba2Hg1 ✗ UNMEASURED

#### Group 3: Other compositions (5) - ALL UNMEASURED
22. O6Cu1Ba2Hg1Tl1 (O, Cu, Ba, Hg, Tl) - 5 elements, contains Hg and Tl
23. O10Ca2Cu3Ba2Tl2 (O, Ca, Cu, Ba, Tl) - 5 elements, contains Tl (no Hg)
24. O6Cu1Cd0.250Ba2Tl1.750 (O, Cu, Cd, Ba, Tl) - 5 elements, contains Cd and Tl (no Hg)
25. O7Cu3Ba2La1 (O, Cu, Ba, La) - 4 elements, contains La (no Hg)
26. O7Cu3Y0.200Ba2La0.800 (O, Cu, Y, Ba, La) - 5 elements, contains Y and La (no Hg)

#### Group 4: Unmeasurable compositions (7) - CANNOT BE MEASURED
From unmeasurable.txt:
1. O6.160Ca1Cu2Ba2Hg1 (O, Ca, Cu, Ba, Hg) - Same as Group 1
2. O6.270Ca1Cu2Ba2Hg1 (O, Ca, Cu, Ba, Hg) - Same as Group 1
3. O4.180Cu1Ba2Hg1 (O, Cu, Ba, Hg) - Same as Group 2
4. O8Ca0.850Cu2.200Ba2Tl1.800 (O, Ca, Cu, Ba, Tl) - Contains Tl, similar to #23 but different stoichiometry
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (O, Ca, Cu, Sr, Tl, Pb) - Contains Sr, Tl, Pb
6. O8Cu3Y1Ba1.900La0.100 (O, Cu, Y, Ba, La) - Contains Y, La
7. O8Cu4Y1Ba1.870La0.130 (O, Cu, Y, Ba, La) - Contains Y, La

Now I need to induce a rule. The warning says single-factor rules are wrong. So I need multi-factor rule.

Looking at what I've measured: ALL contain Hg. All measured compositions contain Hg. But the warning says "contains Hg" is a single-factor rule that's wrong on several.

Also, all measured compositions contain Ba. All contain Cu. All contain O.

The unmeasured compositions in Group 3 don't all contain Hg. Some contain Tl, La, Y, Cd, Sr, Pb.

Perhaps the rule involves specific element combinations. For example:
- Hg + Ba + Cu = high Tc
- But Tl instead of Hg might change things
- La or Y instead of Hg might change things

Without low Tc examples, it's hard to induce a rule. But maybe I can test the `measure_tc` tool on unmeasurable materials to see if it gives an error, confirming they're unmeasurable. Actually, the task says `measure_tc` will tell me "no sample exists" for them.

Let me test one unmeasurable material to confirm: