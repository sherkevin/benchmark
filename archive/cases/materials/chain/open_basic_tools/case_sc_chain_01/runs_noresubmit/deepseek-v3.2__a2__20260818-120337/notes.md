# Superconductor Tc Study Notes

## Initial State
- Budget: 18 units total (shared between search_db and measure_tc)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high = Tc >= 90 K, low = Tc < 90 K

## Step A: Find series peak
Homologous doping series: same element set (Ca, Cu, Ba, Hg), varying oxygen content.

Found 12 series members via search_db("Ca1Cu2Ba2Hg1"):
1. O6.080Ca1Cu2Ba2Hg1 (seed)
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.160Ca1Cu2Ba2Hg1
6. O6.190Ca1Cu2Ba2Hg1
7. O6.210Ca1Cu2Ba2Hg1
8. O6.220Ca1Cu2Ba2Hg1
9. O6.240Ca1Cu2Ba2Hg1
10. O6.270Ca1Cu2Ba2Hg1
11. O6.300Ca1Cu2Ba2Hg1
12. O6.310Ca1Cu2Ba2Hg1

Budget used so far: 11 units (all search_db)
Budget remaining: 7 units

## Measurement Results for Step A
Measured Tc values (budget exhausted):

1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1: Not measured
3. O6.130Ca1Cu2Ba2Hg1: Not measured  
4. O6.140Ca1Cu2Ba2Hg1: Not measured
5. O6.160Ca1Cu2Ba2Hg1: Not measured
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K ✓
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K ✓
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1: No sample (not synthesized) ✗
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K ✓

Maximum Tc measured: 127.0 K (O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
T* = 127.0 K

Unmeasured materials in series: O6.100, O6.130, O6.140, O6.160
But we have found the peak among measured: 127.0 K at O6.210 and O6.220

Budget: Used 18/18 units (exhausted)

## Step B Analysis
Need materials containing Hg with exactly 4 elements and Tc >= 95.0 K.
Found 3 candidates from "Hg" page 1 search: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1.
These have 4 elements (O, Cu, Ba, Hg).
However, no budget left to measure their Tc or search for more such materials.
Thus cannot determine Q1.

## Step C Analysis
Unmeasurable materials discovered (from discovery.json):
1. O6.160Ca1Cu2Ba2Hg1 - In series, between O6.080 (92 K) and O6.190 (122 K)
2. O6.270Ca1Cu2Ba2Hg1 - In series, between O6.240 (123 K) and O6.300 (100 K)
3. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Tl, Pb, Sr instead of Hg
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Contains Tl instead of Hg

Induced rule: Materials containing copper (Cu) and either mercury (Hg) or thallium (Tl) are high-Tc (>= 90 K).
Rationale: All measured Hg-cuprates are high. Tl is in same periodic table group as Hg and known to form high-Tc superconductors.

Predictions: All 4 unmeasurable materials predicted HIGH.

## Step D Analysis
Among unmeasurable predicted high, choose one with fewest elements:
- O6.160: 5 elements
- O6.270: 5 elements  
- O7...: 6 elements
- O8...: 5 elements

Tie between O6.160, O6.270, O8... (all 5 elements). Break tie by higher expected Tc:
- O6.270 estimated ~110-115 K (between O6.240 at 123 K and O6.300 at 100 K)
- O6.160 estimated ~107-112 K (between O6.080 at 92 K and O6.190 at 122 K)
- O8... unknown but likely lower than Hg-based

Choose O6.270Ca1Cu2Ba2Hg1 as final.

## Lessons Learned
1. Budget management critical: Used too many searches early
2. Should have searched directly for series pattern "Ca1Cu2Ba2Hg1" first (4 budget)
3. Should have measured series more strategically (binary search)
4. Left no budget for Step B searches/measurements
5. Need multi-factor rule for predictions as single-factor rules fail

## Final Answer Summary
- Peak: O6.220Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold1 = 95.0 K
- Q1: Empty (no measurements of 4-element Hg materials due to budget)
- Predictions: All 4 discovered unmeasurable materials predicted HIGH based on Cu+(Hg|Tl) rule
- Final pick: O6.270Ca1Cu2Ba2Hg1 (5 elements, tie-broken by higher expected Tc)
- Rule: "Materials containing copper (Cu) and either mercury (Hg) or thallium (Tl) have Tc >= 90 K"

Answer written to answer.json

## Step B: Filter family B
Materials that contain Hg and have exactly 4 elements. Need to find all such materials with Tc >= threshold1 (T* - 32).

## Step C: Extrapolate
Some materials have never been synthesized. Need to induce rule from measurable materials and predict high/low for unmeasurable ones.

## Step D: Final pick
Among unmeasurable materials predicted high, choose one with fewest elements (break ties by higher expected Tc).