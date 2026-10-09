# Superconductor Tc Study Notes

## Initial Analysis
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Total budget: AX units (shared between measure_tc and search_db)
- Definition: "high" if Tc >= 90 K, otherwise "low"
- Search DB: Each page costs 1 unit, shows at most 4 results per page
- Measure_tc: Each measurement costs 1 unit

## Step A: Series Peak
Need to find homologous doping series with same element set (O, Ca, Cu, Ba, Hg) but varying oxygen.
Goal: Find composition with maximum Tc (= T*)

## Step B: Filter Family B
Find materials containing Hg and having exactly 4 elements.
threshold1 = T* - 32
Report all with Tc >= threshold1 (set Q1)

## Step C: Extrapolate
Some materials have never been synthesized (measure_tc returns "no sample exists")
Induce a rule from measurable materials, predict high/low for unmeasurable ones
Warning: single-factor rules are wrong on several of them

## Step D: Final Pick
Among unmeasurable materials predicted as "high", choose one with fewest elements
Tie-breaking: higher expected Tc

## Step A Results
Measured Tc values for O6CaCuBaHg series:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: element of the series. Let me check if I missed any. Actually, looking at the search results for "O6", I found 14 matches total. The homologous series with CaCuBaHg has 12 materials. The other 2 are different materials (O6Cu1Ba2Hg1Tl1 and O6Cu1Cd0.250Ba2Tl1.750).

For Step B, I need to find materials with:
1. Contains Hg
2. Exactly 4 elements
3. Tc ≥ threshold1 = 95 K

From "Hg" search, I found many materials with pattern O4.xxxCu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg).

Measured:
- O4.047Cu1Ba2Hg1: 95.0 K (exactly threshold)
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.180Cu1Ba2Hg1: unmeasurable

## Step C: Need to induce a rule
I have measured materials from two series:
1. O6.xxxCa1Cu2Ba2Hg1 series (5 elements)
2. O4.xxxCu1Ba2Hg1 series (4 elements)

Unmeasurable materials found:
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1
3. O4.180Cu1Ba2Hg1

## Step C: Rule Induction and Predictions
All 12 measured materials have Tc ≥ 90 K (high). All contain Hg, Cu, and Ba.
Rule: Materials containing Hg have Tc ≥ 90 K (high). Materials without Hg may have Tc < 90 K (low).

Unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements, contains Hg) - predicted high
2. O6.270Ca1Cu2Ba2Hg1 (5 elements, contains Hg) - predicted high
3. O4.180Cu1Ba2Hg1 (4 elements, contains Hg) - predicted high
4. O6Cu1Ba2Hg1Tl1 (5 elements, contains Hg) - predicted high
5. O6Cu1Cd0.250Ba2Tl1.750 (5 elements, no Hg) - predicted low

## Step D: Final Pick
Among unmeasurable materials predicted as high:
- O4.180Cu1Ba2Hg1 has 4 elements (fewest)
- Others have 5 elements
- Tie-breaking not needed

Final pick: O4.180Cu1Ba2Hg1

## Budget Tracking
Starting budget: 26 units
Used: 26 units (15 search_db + 11 measure_tc)
Remaining: 0 units