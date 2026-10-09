# Superconductor Tc Study Notes

## Initial Analysis
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Total budget: 34 units (shared between search_db and measure_tc)
- Search_db costs 1 unit per page (max 4 results per page)
- Measure_tc costs 1 unit per measurement

## Step A - Find homologous doping series
Goal: Find series members (same element set, varying oxygen) and identify composition with maximum Tc (T*).

Seed composition breakdown:
- O: 6.080 (oxygen varies in the series)
- Ca: 1
- Cu: 2
- Ba: 2
- Hg: 1

Elements: O, Ca, Cu, Ba, Hg (5 elements total)

Found series members (Ca, Cu, Ba, Hg with varying oxygen):
1. O6.080Ca1Cu2Ba2Hg1 (seed) - Tc = 92.0 K (measurable)
2. O6.100Ca1Cu2Ba2Hg1 - Tc = 92.5 K (measurable)
3. O6.130Ca1Cu2Ba2Hg1 - Tc = 102.0 K (measurable)
4. O6.140Ca1Cu2Ba2Hg1 - Tc = 105.0 K (measurable)
5. O6.160Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - Tc = 122.0 K (measurable)
7. O6.210Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable)
8. O6.220Ca1Cu2Ba2Hg1 - Tc = 127.0 K (measurable)
9. O6.240Ca1Cu2Ba2Hg1 - Tc = 123.0 K (measurable)
10. O6.270Ca1Cu2Ba2Hg1 - No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - Tc = 100.0 K (measurable)
12. O6.310Ca1Cu2Ba2Hg1 - Tc = 98.0 K (measurable)

Maximum Tc (T*) = 127.0 K (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Current budget used: 34 units (22 search + 12 measurements)
Remaining budget: 0 units

Note: Unmeasurable materials found:
1. O6.160Ca1Cu2Ba2Hg1 (from series)
2. O6.270Ca1Cu2Ba2Hg1 (from series)
3. O4.047Cu1Ba2Hg1 (4-element material)
4. O4.066Cu1Ba2Hg1 (4-element material)
5. Other O4.xxxCu1Ba2Hg1 materials are likely also unmeasurable (not checked due to budget)

Measurable materials (all from series, all have Tc >= 90 K = high):
1. O6.080Ca1Cu2Ba2Hg1 - 92.0 K (high)
2. O6.100Ca1Cu2Ba2Hg1 - 92.5 K (high)
3. O6.130Ca1Cu2Ba2Hg1 - 102.0 K (high)
4. O6.140Ca1Cu2Ba2Hg1 - 105.0 K (high)
5. O6.190Ca1Cu2Ba2Hg1 - 122.0 K (high)
6. O6.210Ca1Cu2Ba2Hg1 - 127.0 K (high)
7. O6.220Ca1Cu2Ba2Hg1 - 127.0 K (high)
8. O6.240Ca1Cu2Ba2Hg1 - 123.0 K (high)
9. O6.300Ca1Cu2Ba2Hg1 - 100.0 K (high)
10. O6.310Ca1Cu2Ba2Hg1 - 98.0 K (high)

## Step B - Filter family B
Goal: Find materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc >= 95.0 K

Problem: No budget left to measure 4-element materials. However, based on pattern:
- All measurable materials in the series have Tc >= 90 K
- The 4-element materials (O4.xxxCu1Ba2Hg1) are likely unmeasurable (2 checked were unmeasurable)
- If they were measurable, we could check Tc, but we can't due to budget

## Step C - Extrapolate
Goal: Induce rule from measurable materials to predict high/low for unmeasurable ones.
All measurable materials have Tc >= 90 K (high). Need to find a rule that explains this.
Warning: Single-factor rules are wrong on several of them.

Materials discovered: 30 unique materials
Measured: 10 materials (all from series, all high Tc)
Unmeasurable confirmed: 4 materials (O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1)
Other O4.xxxCu1Ba2Hg1 materials likely also unmeasurable

Rule induction: Looking at measurable materials:
- All contain Ca, Cu, Ba, Hg, O
- Oxygen content: 6.080 to 6.310
- Tc values show parabolic trend with oxygen content
- Peak Tc at O6.210-O6.220 (127.0 K)

For unmeasurable series members O6.160 and O6.270:
- Oxygen content within range of measurable materials
- Likely high Tc (>= 90 K) based on trend

For 4-element materials O4.xxxCu1Ba2Hg1:
- No Ca
- Lower oxygen content (~4.xxx)
- No comparable measurable materials
- Hard to predict

Rule induced: "Materials containing all four elements Ca, Cu, Ba, and Hg with oxygen content between 6.0 and 6.4 have high Tc (>= 90 K). Materials missing any of these four elements or with oxygen content outside this range have low Tc (< 90 K)."
This is a multi-factor rule (requires presence of 4 specific elements AND oxygen content in a specific range).

## Step D - Final pick
Goal: Among unmeasurable materials predicted high, choose one with fewest elements (break ties by higher expected Tc).

Unmeasurable materials predicted high (candidates):
1. O6.160Ca1Cu2Ba2Hg1 (5 elements) - predicted high
2. O6.270Ca1Cu2Ba2Hg1 (5 elements) - predicted high

Both have 5 elements. Need to break tie by higher expected Tc.
Based on Tc trend: O6.140 (105.0 K) < O6.160 (estimated ~113-115 K) < O6.190 (122.0 K)
O6.240 (123.0 K) > O6.270 (estimated ~111-112 K) > O6.300 (100.0 K)
O6.160 likely has slightly higher Tc than O6.270.

Final pick: O6.160Ca1Cu2Ba2Hg1

## Answer Summary
- Peak composition: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold1 = 95.0 K
- Qualifying materials: None (no budget to measure 4-element materials)
- Predictions: All O4.xxxCu1Ba2Hg1 predicted low (missing Ca), series members O6.160 and O6.270 predicted high
- Final pick: O6.160Ca1Cu2Ba2Hg1
- Rule: Materials containing all four elements Ca, Cu, Ba, and Hg have high Tc

## Step B - Filter family B
Goal: Among materials that contain Hg and have exactly 4 elements, report all with Tc >= threshold1 (T* - 32)

## Step C - Extrapolate
Goal: Induce rule from measurable materials to predict high/low for unmeasurable ones
- Warning: Single-factor rules are wrong on several of them

## Step D - Final pick
Goal: Among unmeasurable materials predicted high, choose one with fewest elements (break ties by higher expected Tc)