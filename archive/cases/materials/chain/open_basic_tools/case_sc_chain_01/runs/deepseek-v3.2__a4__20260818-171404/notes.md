# Superconductor Tc Study Notes

## Initial Setup
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 26 total units (shared between search_db and measure_tc)
- Definition of "high": Tc >= 90 K
- Each search_db page costs 1 unit, each measure_tc costs 1 unit
- Each search_db page shows at most 4 results

## Step A: Find series peak (T*)
Goal: Find homologous doping series with same element set, varying oxygen. Need to identify composition with maximum Tc.

Seed: O6.080Ca1Cu2Ba2Hg1
Elements: O, Ca, Cu, Ba, Hg
Series should have same element set (Ca, Cu, Ba, Hg) with varying oxygen content.

First, need to search for materials with all these elements. Let's start with search for Hg-containing materials since Hg is less common.

### Search Results
1. search_db("Hg", page=1): Found 3 materials:
   - O4.047Cu1Ba2Hg1
   - O4.066Cu1Ba2Hg1  
   - O4.081Cu1Ba2Hg1
   Budget used: вещ, Remaining: 25
   Total Hg matches: 27

2. search_db("Ca", page=1-5): Found homologous series materials:
   Page 1: O10Ca2Cu3Ba2Tl2, O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1
   Page 2: O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1, O6.160Ca1Cu2Ba2Hg1
   Page 3: O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1
   Page 4: O6.240Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1
   Page 5: O6.310Ca1Cu2Ba2Hg1, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Ca0.850Cu2.200Ba2Tl1.800
   
   Total Ca matches: 15
   Budget used: 7 total (1 for Hg + 1 for CaHg + 5 for Ca searches)
   
The homologous series appears to be: O6.xxxCa1Cu2Ba2Hg1 where xxx varies from 0.080 to 0.310

### Step A Measurements (Homologous Series)
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (measurable)
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K (measurable)
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K (measurable)
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K (measurable)
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K (measurable)
7. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (measurable)
8. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (measurable)
9. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K (measurable)
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K (measurable)
12. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K (measurable)

Maximum Tc (T*) = 127.0 K (for both O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
Budget used: 17 total at this point

Observations: Tc increases with oxygen content up to a peak around O6.210-O6.220, then decreases.

### Additional Searches and Measurements
- Searched for O6.2xx patterns (4 pages): budget +4 = 21
- Searched for Hg materials pages 2-4: budget +3 = 24
- Measured O4.047Cu1Ba2Hg1: Tc = 95.0 K, budget +1 = 25
- Searched for O4.066Cu1Ba2Hg1: budget +1 = 26 (OUT OF BUDGET)

### Step B Analysis
Threshold1 = T* - 32 = 127 - 32 = 95 K
Need to find all Hg-containing materials with exactly 4 elements and Tc >= 95 K.

Found many O4.xxxCu1Ba2Hg1 materials (exactly 4 elements: O, Cu, Ba, Hg).
Measured O4.047Cu1Ba2Hg1: Tc = 95.0 K (exactly at threshold, qualifies).
Other similar materials likely have similar Tc values around 95 K.

Problem: Out of budget, cannot measure other materials.

### Step C Analysis (Rule Induction)
From measurable materials:
- O6.080Ca1Cu2Ba2Hg1 (O=6.080): 92.0 K (low)
- O6.100Ca1Cu2Ba2Hg1 (O=6.100): 92.5 K (low)
- O6.130Ca1Cu2Ba2Hg1 (O=6.130): 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1 (O=6.140): 105.0 K (high)
- O6.190Ca1Cu2Ba2Hg1 (O=6.190): 122.0 K (high)
- O6.210Ca1Cu2Ba2Hg1 (O=6.210): 127.0 K (high)
- O6.220Ca1Cu2Ba2Hg1 (O=6.220): 127.0 K (high)
- O6.240Ca1Cu2Ba2Hg1 (O=6.240): 123.0 K (high)
- O6.300Ca1Cu2Ba2Hg1 (O=6.300): 100.0 K (high)
- O6.310Ca1Cu2Ba2Hg1 (O=6.310): 98.0 K (high)
- O4.047Cu1Ba2Hg1 (O=4.047): 95.0 K (high)

Pattern: For materials with Ca (5 elements), oxygen >= 6.130 gives high Tc.
For materials without Ca (4 elements), oxygen >= 4.047 gives high Tc (based on single data point).

Unmeasurable materials found:
1. O6.160Ca1Cu2Ba2Hg1: Oxygen=6.160, has Ca → predicted high
2. O6.270Ca1Cu2Ba2Hg1: Oxygen=6.270, has Ca → predicted high

### Step D Analysis
Among unmeasurable materials predicted high:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements

Both have same number of elements (5). Need to break tie by higher expected Tc.
Based on trend: O6.140 (105K) < O6.160 < O6.190 (122K) → O6.160 likely ~115K
O6.240 (123K) < O6.270 < O6.300 (100K) → O6.270 likely ~110K
So O6.160 has higher expected Tc.

Final choice: O6.160Ca1Cu2Ba2Hg1

### Final Answer Summary
- Peak: O6.220Ca1Cu2Ba2Hg1 (Tc=127.0 K)
- Threshold1: 95.0 K
- Qualifying: Only O4.047Cu1Ba2Hg1 confirmed (Tc=95.0 K, exactly at threshold)
- Predictions: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 predicted high
- Final: O6.160Ca1Cu2Ba2Hg1 (5 elements, higher expected Tc than O6.270)
- Rule: If material contains Ca, high when oxygen >= 6.130; if material does not contain Ca, high when oxygen >= 4.0

### Budget Reflection
Used budget inefficiently:
- Too many search_db calls (could have been more targeted)
- Should have measured more O4.xxxCu1Ba2Hg1 materials early to understand the series
- Could have searched for "O4." pattern instead of browsing all Hg pages
- Budget management is key for this task