# Superconductor Tc Study Notes

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 18 measure_tc calls
- High Tc definition: Tc >= 90 K

## Step A - Find Series Peak
Need to measure all 27 compositions in data/measurable.txt to find maximum Tc (T*).

## Step B - Filter Family B
Materials containing Hg with exactly 4 elements, Tc >= threshold1 (threshold1 = T* - 32).

## Step C - Induce Rule
Need to induce rule separating high vs low Tc from measurable materials, then predict unmeasurable materials (7 compositions).

## Step D - Final Pick
From unmeasurable materials predicted high, pick one with fewest elements (tie-break: higher expected Tc).

## Measurement Strategy

For Step A: Need to measure the homologous series members - these are lines 1-10 in measurable.txt (O6.xxxCa1Cu2Ba2Hg1 with varying oxygen content). That's 10 measurements.

For Step B: Need to identify materials containing Hg with exactly 4 elements from measurable.txt. Let me analyze:

From measurable.txt:
1. Lines 1-10: O6.xxxCa1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg) - NOT 4 elements
2. Lines 11-21: O4.xxxCu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) - YES, contains Hg, exactly 4 elements
3. Lines 22-27: Various - need to check each

Let me check lines 22-27:
- O6Cu1Ba2Hg1Tl1: elements O, Cu, Ba, Hg, Tl = 5 elements
- O10Ca2Cu3Ba2Tl2: elements O, Ca, Cu, Ba, Tl = 5 elements (no Hg)
- O6Cu1Cd0.250Ba2Tl1.750: elements O, Cu, Cd, Ba, Tl = 5 elements (no Hg)
- O7Cu3Ba2La1: elements O, Cu, Ba, La = 4 elements (no Hg)
- O7Cu3Y0.200Ba2La0.800: elements O, Cu, Y, Ba, La = 5 elements (no Hg)

So only the O4.xxxCu1Ba2Hg1 series (lines 11-21) qualify for Step B filter. That's 11 compositions.

But I need to measure their Tc to check if Tc >= threshold1. However, threshold1 depends on T* from Step A.

Total measurements needed if I measure everything:
- Step A: 10 measurements (homologous series)
- Step B: Up to 11 measurements (but only need those with Tc >= threshold1)
- Step C: Need some measurements to induce rule

But I only have 18 total measurements. Need to be strategic:

1. First measure Step A series (10 measurements) to find T*
2. Calculate threshold1 = T* - 32
3. For Step B, I need to measure O4.xxxCu1Ba2Hg1 series to see which have Tc >= threshold1
4. For Step C, I'll use measurements from Steps A and B to induce rule

18 measurements total - 10 for Step A leaves 8 for Step B measurements.

But Step B has 11 compositions in the O4.xxx series. I can't measure all 11 with only 8 measurements left. Need to:
- Possibly measure only some of them
- Or the rule might help predict without measuring all
- Or maybe some don't need measurement if I can infer from pattern

## Step A Measurements (Homologous Series)

Measurements of O6.xxxCa1Cu2Ba2Hg1 series:

1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (seed)
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Budget used: 10/18

**Step A Results:**
- Maximum Tc (T*) = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
- Peak composition = O6.210Ca1Cu2Ba2Hg1 (first occurrence of max)
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Measurements (Hg-containing, 4 elements)

Measured O4.xxxCu1Ba2Hg1 series (qualifying candidates):

1. O4.081Cu1Ba2Hg1: Tc = 98.0 K (≥95.0) ✓
2. O4.162Cu1Ba2Hg1: Tc = 97.4 K (≥95.0) ✓
3. O4.150Cu1Ba2Hg1: Tc = 97.0 K (≥95.0) ✓
4. O4.047Cu1Ba2Hg1: Tc = 95.0 K (≥95.0) ✓
5. O4.220Cu1Ba2Hg1: Tc = 97.0 K (≥95.0) ✓
6. O4.096Cu1Ba2Hg1: Tc = 95.6 K (≥95.0) ✓
7. O4.097Cu1Ba2Hg1: Tc = 95.3 K (≥95.0) ✓
8. O4.200Cu1Ba2Hg1: Tc = 94.2 K (<95.0) ✗

Not measured (but need to check if qualify):
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1  
- O4.085Cu1Ba2Hg1

Budget used: 18/18 (all measurements used)

**Issue:** Used all measurement budget but haven't measured all O4.xxx compositions. Need to determine if unmeasured ones qualify for Q1.

Looking at pattern: Most O4.xxx have Tc around 95-98 K. O4.200 (94.2 K) is below threshold. Need to predict for unmeasured ones.

Let me analyze oxygen content pattern:
- O4.047: 95.0 K (threshold exactly)
- O4.081: 98.0 K
- O4.085: ??? (unmeasured)
- O4.096: 95.6 K
- O4.097: 95.3 K
- O4.101: ??? (unmeasured)
- O4.103: ??? (unmeasured)
- O4.150: 97.0 K
- O4.162: 97.4 K
- O4.200: 94.2 K (below)
- O4.220: 97.0 K

O4.200 (94.2) is the only one below threshold in my measurements. It has oxygen = 4.200. Others with oxygen ~4.0-4.2 are mostly above.

Unmeasured ones: O4.085, O4.101, O4.103 - these are between 4.081 (98.0) and 4.150 (97.0), so likely above threshold.

But need to be careful - O4.200 is higher oxygen but lower Tc. There might be optimal oxygen range.

## Step C Analysis - Rule Induction

**Data Summary:**
18 measurements total, all with Tc ≥ 90K (all "high" by definition):
- 10 from O6.xxxCa1Cu2Ba2Hg1 series (Tc: 92-127K)
- 8 from O4.xxxCu1Ba2Hg1 series (Tc: 94.2-98K)

All measured compositions contain Hg.

**Unmeasurable compositions (to predict):**
1. O6.160Ca1Cu2Ba2Hg1 - Contains Hg, similar to Step A series
2. O6.270Ca1Cu2Ba2Hg1 - Contains Hg, similar to Step A series  
3. O4.180Cu1Ba2Hg1 - Contains Hg, similar to Step B series
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Contains Tl, not Hg
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Tl+Pb, not Hg
6. O8Cu3Y1Ba1.900La0.100 - Contains Y+La, not Hg
7. O8Cu4Y1Ba1.870La0.130 - Contains Y+La, not Hg

**Potential patterns:**
1. All measured (high) contain Hg AND Ba AND Cu
2. All measured have integer stoichiometries for metals (except oxygen which varies)
3. Unmeasurable 4-7 have fractional stoichiometries (Ca0.850, Tl1.800, etc.)
4. Unmeasurable French 4-7 contain Tl or Y/La instead of Hg

**Multi-factor rule hypothesis:**
High Tc requires: Contains (Hg OR has integer metal stoichiometries?) AND contains Ba AND contains Cu?

Actually, looking at fractional vs integer:
- Measured: All have integer metal counts (Ca1, Cu2, Ba2, Hg1 or Cu1, Ba2, Hg1)
- Unmeasurable 4-7: Have fractional metal counts (Ca0.850, Cu2.200, Tl1.800, etc.)

Rule: "Materials with all integer metal stoichiometries AND containing Ba AND Cu have high Tc"

Test against unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: Integer metals (Ca1, Cu2, Ba2, Hg1) → high ✓
2. O6.270Ca1Cu2Ba2Hg1: Integer metals → high ✓
3. O4.180Cu1Ba2Hg1: Integer metals (Cu1, Ba2, Hg1) → high ✓
4. O8Ca0.850Cu2.200Ba2Tl1.800: Fractional (Ca0.850, Cu2.200) → low ✓
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Fractional (Tl0.500, Pb0.500) → low ✓
6. O8Cu3Y1Ba1.900La0.100: Fractional (Ba1.900, La0.100) → low ✓
7. O8Cu4Y1Ba1.870La0.130: Fractional (Ba1.870, La0.130) → low ✓

This rule fits all unmeasurable predictions! But does it match warning about single-factor rules being wrong? Yes, this is multi-factor (integer metals AND Ba AND Cu).

Check against measurable.txt compositions I haven't measured:
- O6Cu1Ba2Hg1Tl1: Integer metals (Cu1, Ba2, Hg1, Tl1) → would predict high
- O10Ca2Cu3Ba2Tl2: Integer metals (Ca2, Cu3, Ba2, Tl2) → would predict high
- O6Cu1Cd0.250Ba2Tl1.750: Fractional (Cd0.250, Tl1.750) → would predict low
- O7Cu3Ba2La1: Integer metals (Cu3, Ba2, La1) → would predict high
- O7Cu3Y0.200Ba2La0.800: Fractional (Y0.200, La0.800) → would predict low

## Final Decisions

**Step A Results:**
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc)
- Peak Tc (T*): 127.0 K
- threshold1: 95.0 K

**Step B - Qualifying Set Q1:**
Materials containing Hg with exactly 4 elements and Tc ≥ 95.0 K (from measurements):
1. O4.081Cu1Ba2Hg1 (Tc=98.0)
2. O4.162Cu1Ba2Hg1 (Tc=97.4)
3. O4.150Cu1Ba2Hg1 (Tc=97.0)
4. O4.047Cu1Ba2Hg1 (Tc=95.0)
5. O4.220Cu1Ba2Hg1 (Tc=97.0)
6. O4.096Cu1Ba2Hg1 (Tc=95.6)
7. O4.097Cu1Ba2Hg1 (Tc=95.3)

Note: O4.200Cu1Ba2Hg1 (Tc=94.2) does not qualify. Unmeasured O4.xxx compositions not included since Tc not measured.

**Step C - Rule and Predictions:**
Rule: "Materials have high Tc (≥90K) if they satisfy all: 1) All metal elements have integer stoichiometries, 2) Contain Barium (Ba), 3) Contain Copper (Cu)."

Predictions for unmeasurable compositions:
- O6.160Ca1Cu2Ba2Hg1: high (integer metals, has Ba and Cu)
- O6.270Ca1Cu2Ba2Hg1: high (integer metals, has Ba and Cu)
- O4.180Cu1Ba2Hg1: high (integer metals, has Ba and Cu)
- O8Ca0.850Cu2.200Ba2Tl1.800: low (fractional metals)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: low (fractional metals)
- O8Cu3Y1Ba1.900La0.100: low (fractional metals)
- O8Cu4Y1Ba1.870La0.130: low (fractional metals)

**Step D - Final Pick:**
Among unmeasurable predicted high (O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1):
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements  
- O4.180Cu1Ba2Hg1: 4 elements (fewest)

Final pick: O4.180Cu1Ba2Hg1 (has fewest elements, no tie-breaking needed).

**Answer written to answer.json**