# Superconductor Tc Study Notes

## Initial Analysis
- Total budget: 34 units (shared between search_db and measure_tc)
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- "High" Tc: Tc >= 90 K
- Need to find homologous doping series (same element set, varying oxygen)

## Step A Strategy
1. First, measure the seed material's Tc to establish baseline
2. Search for homologous series members (materials with same elements: O, Ca, Cu, Ba, Hg but different oxygen counts)
3. Measure Tc for each series member to find maximum Tc (T*)

## Measurements Record
1. `O6.080Ca1Cu2Ba2Hg1`: Tc = 92.0 K (seed)
2. `O6.100Ca1Cu2Ba2Hg1`: Tc = 92.5 K
3. `O6.130Ca1Cu2Ba2Hg1`: Tc = 102.0 K
4. `O6.140Ca1Cu2Ba2Hg1`: Tc = 105.0 K
5. `O6.160Ca1Cu2Ba2Hg1`: No sample (unmeasurable)
6. `O6.190Ca1Cu2Ba2Hg1`: Tc = 122.0 K
7. `O6.210Ca1Cu2Ba2Hg1`: Tc = 127.0 K (current maximum)
8. `O6.220Ca1Cu2Ba2Hg1`: Tc = 127.0 K (tied maximum)
9. `O6.240Ca1Cu2Ba2Hg1`: Tc = 123.0 K
10. `O6.270Ca1Cu2Ba2Hg1`: No sample (unmeasurable)
11. `O6.300Ca1Cu2Ba2Hg1`: Tc = 100.0 K
12. `O6.310Ca1Cu2Ba2Hg1`: Tc = 98.0 K

## Step B Analysis
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

## Step B Analysis
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

4-element Hg materials found (O[number]Cu1Ba2Hg1):
1. `O4.047Cu1Ba2Hg1`: Unmeasurable
2. `O4.066Cu1Ba2Hg1`: Unmeasurable  
3. `O4.081Cu1Ba2Hg1`: Tc = 98.0 K (>= 95.0 K, qualifies for Q1)
4. `O4.085Cu1Ba2Hg1`: Tc = 92.0 K (< 95.0 K, doesn't qualify)
5. `O4.096Cu1Ba2Hg1`: Tc = 95.6 K (>= 95.0 K, qualifies for Q1)
6. `O4.097Cu1Ba2Hg1`: Tc = 95.3 K (>= 95.0 K, qualifies for Q1)
7. `O4.101Cu1Ba2Hg1`: Tc = 93.4 K (< 95.0 K, doesn't qualify)

Q1 so far: `O4.081Cu1Ba2Hg1`, `O4.096Cu1Ba2Hg1`, `O4.097Cu1Ba2Hg1`

## Key Discovery
`O6Cu1Ba2Hg1Tl1`: Tc = 0.0 K (low material!)
Elements: O, Cu, Ba, Hg, Tl (5 elements)

Comparison with high materials:
- High materials: Contain Ca or have specific oxygen ranges
- This low material: Contains Tl but no Ca, oxygen = 6.0

Possible rule: Materials containing Ca tend to be high, OR materials with Tl but no Ca tend to be low.

## Final Summary

### Step A - Series Peak
- Maximum Tc (T*) = 127.0 K
- Peak composition = O6.210Ca1Cu2Ba2Hg1 (also O6.220Ca1Cu2Ba2Hg1 has same Tc)

### Step B - Filter Family B
- Threshold1 = T* - 32 = 95.0 K
- Measurable 4-element Hg materials with Tc >= 95.0 K: O4.081Cu1Ba2Hg1 (98.0 K), O4.096Cu1Ba2Hg1 (95.6 K), O4.097Cu1Ba2Hg1 (95.3 K)
- Unmeasurable 4-element materials cannot be definitively assessed for Tc >= 95.0 K

### Step C - Induced Rule
- Rule: "A material is high (Tc >= 90 K) if it contains calcium (Ca) or does not contain thallium (Tl)."
- Based on: All high materials contain Ca or lack Tl; O6Cu1Ba2Hg1Tl1 (contains Tl, lacks Ca) is low.

### Step D - Final Pick
- Among unmeasurable materials predicted HIGH, fewest elements: O4.047Cu1Ba2Hg1 and O4.066Cu1Ba2Hg1 (both 4 elements)
- Tie broken by higher expected Tc (higher oxygen content): O4.066Cu1Ba2Hg1

### Budget Tracking
Starting budget: 34 units
Used: 32 units (20 measure_tc + 12 search_db)
Remaining: 2 units

## Search Results - Page 5 (Ca)
Found: `O6.310Ca1Cu2Ba2Hg1`, `O7Ca1Cu2Sr2Tl0.500Pb0.500`, `O8Ca0.850Cu2.200Ba2Tl1.800`
Note: Last page, has_more: false

## Budget Tracking
Starting budget: 34 units
Used: 10 units (5 measure_tc + 5 search_db)
Remaining: 24 units

## Search Results - Page 1 (Hg)
Found: `O4.047Cu1Ba2Hg1`, `O4.066Cu1Ba2Hg1`, `O4.081Cu1Ba2Hg1`
Note: 27 total matches, has_more: true

## Search Results - Page 1 (Ca)
Found: `O10Ca2Cu3Ba2Tl2`, `O6.080Ca1Cu2Ba2Hg1` (seed), `O6.100Ca1Cu2Ba2Hg1`
Note: 15 total matches, has_more: true

## Search Results - Page 2 (Ca)
Found: `O6.130Ca1Cu2Ba2Hg1`, `O6.140Ca1Cu2Ba2Hg1`, `O6.160Ca1Cu2Ba2Hg1`
Note: These are all homologous series members (same elements: O, Ca, Cu, Ba, Hg)

## Search Results - Page 3 (Ca)
Found: `O6.190Ca1Cu2Ba2Hg1`, `O6.210Ca1Cu2Ba2Hg1`, `O6.220Ca1Cu2Ba2Hg1`
Note: More homologous series members

## Search Results - Page 4 (Ca)
Found: `O6.240Ca1Cu2Ba2Hg1`, `O6.270Ca1Cu2Ba2Hg1`, `O6.300Ca1Cu2Ba2Hg1`
Note: More homologous series members

## Budget Tracking
Starting budget: 34 units
Used: 3 units (1 measure_tc + 2 search_db)
Remaining: 31 units