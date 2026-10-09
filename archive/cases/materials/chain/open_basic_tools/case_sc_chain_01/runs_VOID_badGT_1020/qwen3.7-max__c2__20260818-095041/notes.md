# Notes - Superconductor Tc Study

## Task Overview
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90 K, Low = Tc < 90 K

## Budget Tracking
- Used: 34 / 34 (exhausted)
- Breakdown: 1 (seed measure) + 9 (Hg search pages) + 10 (series measures) + 1 (Tl material measure) + 3 (Tl/Bi/Y searches) + 10 (4-elem measures, 4 were free/no-sample) + 3 (extra measure attempts that cost) = well, the counter says 34

## All Hg-containing materials found (27 total via search_db("Hg"))

### Series A: 5-element (O-Ca-Cu-Ba-Hg) - seed's homologous series
| Composition | Tc (K) | Sample? | High/Low |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes | high |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes | high |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes | high |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes | high |
| O6.160Ca1Cu2Ba2Hg1 | ??? | NO SAMPLE | unmeasurable |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes | high |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes | high |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes | high |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes | high |
| O6.270Ca1Cu2Ba2Hg1 | ??? | NO SAMPLE | unmeasurable |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes | high |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes | high |

**PEAK: T* = 127.0 K** (at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

### Series B: 4-element (O-Cu-Ba-Hg)
| Composition | Tc (K) | Sample? | High/Low |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | ??? | NO SAMPLE | unmeasurable |
| O4.066Cu1Ba2Hg1 | ??? | NO SAMPLE | unmeasurable |
| O4.081Cu1Ba2Hg1 | 98.0 | yes | high |
| O4.085Cu1Ba2Hg1 | 92.0 | yes | high |
| O4.096Cu1Ba2Hg1 | 95.6 | yes | high |
| O4.097Cu1Ba2Hg1 | 95.3 | yes | high |
| O4.101Cu1Ba2Hg1 | 93.4 | yes | high |
| O4.103Cu1Ba2Hg1 | 92.5 | yes | high |
| O4.150Cu1Ba2Hg1 | 97.0 | yes | high |
| O4.160Cu1Ba2Hg1 | ??? | NO SAMPLE | unmeasurable |
| O4.162Cu1Ba2Hg1 | 97.4 | yes | high |
| O4.180Cu1Ba2Hg1 | ??? | NO SAMPLE | unmeasurable |
| O4.200Cu1Ba2Hg1 | 94.2 | yes | high |
| O4.220Cu1Ba2Hg1 | 97.0 | yes | high |

### Other Hg materials
| Composition | Tc (K) | Elements | Sample? |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | 5 (O,Cu,Ba,Hg,Tl) | yes, LOW |

## Non-Hg materials found

### Tl search (5 total, only saw 3 on page 1)
| Composition | Tc (K) | Sample? |
|---|---|---|
| O10Ca2Cu3Ba2Tl2 | unknown (budget exhausted) | sample exists |
| O6Cu1Ba2Hg1Tl1 | 0.0 | yes (already counted above) |
| O6Cu1Cd0.250Ba2Tl1.750 | unknown (budget exhausted) | sample exists |
| (2 more on page 2 - unseen) | | |

### Y search (3 total)
| Composition | Tc (K) | Sample? |
|---|---|---|
| O7Cu3Y0.200Ba2La0.800 | unknown (budget exhausted) | sample exists |
| O8Cu3Y1Ba1.900La0.100 | ??? | NO SAMPLE - unmeasurable |
| O8Cu4Y1Ba1.870La0.130 | ??? | NO SAMPLE - unmeasurable |

### Bi search: 0 results

## STEP A: Peak
- T* = 127.0 K
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (first to reach max)
- Dome-shaped Tc vs oxygen curve: 92 → 127 → 98

## STEP B: Q1 (4-element Hg materials with Tc >= 95)
- threshold1 = 127.0 - 32 = 95.0
- Q1 members (measured with Tc >= 95):
  - O4.081Cu1Ba2Hg1: 98.0
  - O4.096Cu1Ba2Hg1: 95.6
  - O4.097Cu1Ba2Hg1: 95.3
  - O4.150Cu1Ba2Hg1: 97.0
  - O4.162Cu1Ba2Hg1: 97.4
  - O4.220Cu1Ba2Hg1: 97.0

## STEP C: Unmeasurable materials and prediction rule

### All unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
3. O4.047Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
4. O4.066Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
5. O4.160Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
6. O4.180Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
7. O8Cu3Y1Ba1.900La0.100 (5 elem: O,Cu,Y,Ba,La)
8. O8Cu4Y1Ba1.870La0.130 (5 elem: O,Cu,Y,Ba,La)

### Rule induction:
Measured data summary:
- ALL materials with Hg and without Tl → HIGH (20 materials, all Tc >= 90)
- Material with Hg AND Tl → LOW (O6Cu1Ba2Hg1Tl1, Tc = 0)
- No non-Hg measured materials available for comparison

Key distinguishing factors:
- Presence of Hg: all high materials have Hg
- Absence of Tl: the only low material has Tl
- Y-containing cuprates (YBCO family) are well-known high-Tc superconductors

Multi-factor rule: "High if (contains Hg OR contains Y) AND does not contain Tl; Low otherwise"

Single-factor rule "contains Hg → high" would be wrong on:
- O6Cu1Ba2Hg1Tl1 (has Hg but low) - measured
- O8Cu3Y1Ba1.900La0.100 (no Hg but likely high as YBCO-type) - unmeasurable
- O8Cu4Y1Ba1.870La0.130 (no Hg but likely high as YBCO-type) - unmeasurable
That's 3 = "several" → confirms multi-factor rule needed

### Predictions:
1. O6.160Ca1Cu2Ba2Hg1: has Hg, no Tl → **high**
2. O6.270Ca1Cu2Ba2Hg1: has Hg, no Tl → **high**
3. O4.047Cu1Ba2Hg1: has Hg, no Tl → **high**
4. O4.066Cu1Ba2Hg1: has Hg, no Tl → **high**
5. O4.160Cu1Ba2Hg1: has Hg, no Tl → **high**
6. O4.180Cu1Ba2Hg1: has Hg, no Tl → **high**
7. O8Cu3Y1Ba1.900La0.100: has Y, no Tl → **high**
8. O8Cu4Y1Ba1.870La0.130: has Y, no Tl → **high**

## STEP D: Final pick
Among predicted-high unmeasurable, fewest elements:
- 4 elements: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Cu3Y1Ba1.900La0.100, O8Cu4Y1Ba1.870La0.130

Tied at 4 elements → break by higher expected Tc:
- O4.047: extrapolating below measured range (4.081→98), est ~93-95
- O4.066: extrapolating, est ~95-96
- O4.160: between 4.150(97) and 4.162(97.4), est ~97
- O4.180: between 4.162(97.4) and 4.200(94.2), est ~96

**Final pick: O4.160Cu1Ba2Hg1** (4 elements, highest expected Tc ~97 K)
