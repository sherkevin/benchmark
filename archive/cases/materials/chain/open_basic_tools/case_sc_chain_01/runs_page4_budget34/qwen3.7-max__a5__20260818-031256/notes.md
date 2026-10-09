# Notes - Superconductor Tc Study

## Budget Tracking
- Used: 34/34 (exhausted)
- Search pages: 10 (Hg 1-7, Cu 1-2, Tl 1-2, Ba 1, Ca 1)
- Measurements: 24 attempts (some free due to no sample)

## Step A: Series Peak (O-Ca-Cu-Ba-Hg series)
| Composition | Tc (K) | Sample? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes |
| O6.160Ca1Cu2Ba2Hg1 | - | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes |
| O6.270Ca1Cu2Ba2Hg1 | - | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes |

**T* = 127.0 K** (O6.210Ca1Cu2Ba2Hg1 — tie with O6.220, picking lower O)
**threshold1 = 127.0 - 32 = 95.0 K**

## Step B: Q1 (4-element Hg materials with Tc >= 95.0)
4-element Hg materials measured:
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | no |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | no |
| O4.103Cu1Ba2Hg1 | 92.5 | no |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | no |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

**Q1 = [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]**

Also measured: O6Cu1Ba2Hg1Tl1 → Tc = 0.0 K (LOW, 5 elements)

## Step C: Unmeasurable Materials & Rule
Confirmed unmeasurable (no sample):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
3. O4.047Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
4. O4.066Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
5. O4.160Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
6. O4.180Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb)
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl)

Uncertain (budget error, likely measurable): O10Ca2Cu3Ba2Tl2, O6Cu1Cd0.250Ba2Tl1.750

### Rule Analysis
All measurable materials:
- 20 HIGH: all contain Hg, none contain Tl
- 1 LOW: O6Cu1Ba2Hg1Tl1 (contains both Hg AND Tl)

**Rule: HIGH if contains Hg and does not contain Tl; LOW otherwise**
(Two-factor rule: single-factor "has Hg → HIGH" fails on O6Cu1Ba2Hg1Tl1)

### Predictions:
- Hg-containing, no Tl → HIGH: materials 1-6
- No Hg, has Tl → LOW: materials 7-8

## Step D: Final Pick
HIGH unmeasurable with fewest elements:
- 4 elements: O4.047, O4.066, O4.160, O4.180 (all Cu-Ba-Hg-O)
- 5 elements: O6.160, O6.270

Tie-break by expected Tc (interpolation from nearby):
- O4.160: between O4.150(97.0) and O4.162(97.4) → ~97.3 K
- O4.180: between O4.162(97.4) and O4.200(94.2) → ~95.9 K
- O4.066: below O4.081(98.0), uncertain
- O4.047: even lower O, uncertain

**Final: O4.160Cu1Ba2Hg1** (4 elements, highest expected Tc ~97.3 K)
