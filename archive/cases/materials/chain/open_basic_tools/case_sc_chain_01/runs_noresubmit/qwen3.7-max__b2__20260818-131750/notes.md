# Notes - Superconductor Tc Study

## Task Overview
- Budget: 18 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low: < 90 K

## Step A: Find series peak
- Seed elements: O, Ca, Cu, Ba, Hg (5 elements)
- Series: same element set, varying oxygen
- Need to search for materials with these elements

## Progress

### Budget Usage: 18/18 (exhausted)
- Searches: 11 (6 Hg pages + 5 Ca pages)
- Measurements: 9 attempts (7 successful, 2 no-sample - free)

### Discovered Materials

**5-element series (O-Ca-Cu-Ba-Hg) - 12 members:**
O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310

**4-element Hg materials (O-Cu-Ba-Hg) - 14 found:**
O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220

**Non-Hg materials from Ca search:**
O10Ca2Cu3Ba2Tl2, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Ca0.850Cu2.200Ba2Tl1.800

### Step A: Series Measurements
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | ? | not tried |
| O6.140Ca1Cu2Ba2Hg1 | ? | not tried |
| O6.160Ca1Cu2Ba2Hg1 | - | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | - | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | ? | not tried |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

Peak: T* = 127.0 K at O6.220Ca1Cu2Ba2Hg1 (and O6.210)
Dome shape: 92 → 92.5 → ... → 122 → 127 → 127 → 123 → ... → 98

### Step B: threshold1 = 127 - 32 = 95
4-element Hg materials: O4.xxxCu1Ba2Hg1
Could not measure any due to budget exhaustion.
Q1 = unknown

### Step C: Unmeasurable confirmed
1. O6.160Ca1Cu2Ba2Hg1 - predicted HIGH (between 92.5 and 122 in dome)
2. O6.270Ca1Cu2Ba2Hg1 - predicted HIGH (between 123 and 98 in dome)

### Step D: Both predicted high, both 5 elements
Tie-break: higher expected Tc
- O6.160: ~113 K (left side of dome)
- O6.270: ~111 K (right side of dome)
Final: O6.160Ca1Cu2Ba2Hg1

### Analysis for Rule (Step C)
The Tc dome for the 5-element series:
- Oxygen 6.080: Tc 92.0 (underdoped)
- Oxygen 6.100: Tc 92.5
- Oxygen 6.190: Tc 122.0
- Oxygen 6.210: Tc 127.0 (optimal)
- Oxygen 6.220: Tc 127.0 (optimal)
- Oxygen 6.240: Tc 123.0
- Oxygen 6.310: Tc 98.0 (overdoped)

All measured 5-element materials are high (>=90). The dome peaks at oxygen ~6.210-6.220.
Unmeasurable O6.160 (between 92.5 and 122) and O6.270 (between 123 and 98) should both be high.

Multi-factor rule: High if (has Ca AND Cu>=2 AND oxygen in range 6.0-6.5). 
The number of Cu layers and presence of Ca are critical for high Tc in cuprates.
Materials without Ca or with fewer Cu atoms tend to have lower Tc_max.

### Key Issue
Could not measure any 4-element Hg materials (budget exhausted).
Could not search Hg pages 7-9 (9 more materials unseen).
Q1 likely empty based on available data.
