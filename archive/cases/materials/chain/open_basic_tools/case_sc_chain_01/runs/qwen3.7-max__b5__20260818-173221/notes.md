# Notes - Superconductor Tc Study

## Task Overview
- Budget: 26 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90 K, Low = Tc < 90 K

## Step A - Series Peak
- Seed has elements: O, Ca, Cu, Ba, Hg (5 elements)
- Homologous series = same element set, varying oxygen
- Need to find all series members and measure Tc

## Step B - Filter Family B
- threshold1 = T* - 32
- Materials containing Hg with exactly 4 elements
- Report all with Tc >= threshold1

## Step C - Extrapolate
- Find unmeasurable materials
- Induce rule from measurable ones
- Predict high/low for unmeasurable

## Step D - Final Pick
- Among predicted "high" unmeasurable, pick fewest elements (tie-break: higher expected Tc)

---

## Actions Log

### Searches performed (budget: 15 units on searches)
- Hg search: 9 pages = 27 materials found
- Seed search: 1 page = 1 material
- Y search: 1 page = 3 materials  
- La search: 2 pages = 4 materials
- Tl search: 2 pages = 5 materials

### All Hg-containing materials found (27 total):

**4-element (O, Cu, Ba, Hg) - 14 materials:**
O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1,
O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1,
O4.150Cu1Ba2Hg1 (measured: 97 K), O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1,
O4.180Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

**5-element series (O, Ca, Cu, Ba, Hg) - 12 materials:**
| Composition | Tc (K) | Available? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes |
| O6.160Ca1Cu2Ba2Hg1 | N/A | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes |
| O6.270Ca1Cu2Ba2Hg1 | N/A | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes |

**Other Hg material:**
O6Cu1Ba2Hg1Tl1 (5 elements: O, Cu, Ba, Hg, Tl)

### Non-Hg materials found:
- O7Cu3Y0.200Ba2La0.800 (5 elem: O,Cu,Y,Ba,La)
- O8Cu3Y1Ba1.900La0.100 (5 elem: O,Cu,Y,Ba,La)
- O8Cu4Y1Ba1.870La0.130 (5 elem: O,Cu,Y,Ba,La)
- O7Cu3Ba2La1 (4 elem: O,Cu,Ba,La)
- O10Ca2Cu3Ba2Tl2 (5 elem: O,Ca,Cu,Ba,Tl)
- O6Cu1Cd0.250Ba2Tl1.750 (5 elem: O,Cu,Cd,Ba,Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl)

---

## Step A: Series Peak
- Peak: O6.210Ca1Cu2Ba2Hg1 (first to reach max)
- T* = 127.0 K
- Both O6.210 and O6.220 have 127.0 K (plateau)

## Step B: Filter Family B
- threshold1 = 127.0 - 32 = 95.0 K
- 4-element Hg materials: O-Cu-Ba-Hg family
- Only measured O4.150Cu1Ba2Hg1 = 97 K (qualifies: 97 >= 95)
- Cannot measure remaining 13 (budget exhausted)

## Step C: Unmeasurable materials found
- O6.160Ca1Cu2Ba2Hg1 (between 105 and 122 K → predicted high)
- O6.270Ca1Cu2Ba2Hg1 (between 123 and 100 K → predicted high)

## Step D: Final Pick
- Both have 5 elements → tie on element count
- O6.160: interpolated ~112 K, O6.270: interpolated ~112 K
- Pick O6.160Ca1Cu2Ba2Hg1 (slightly higher expected Tc by interpolation)

