# Notes - Superconductor Tc Study

## Budget Usage
- Used 18/18 budget units (8 searches + 10 measurements)
- Exhausted budget before completing Steps B-D fully

## Step A: Series Peak

Seed: O6.080Ca1Cu2Ba2Hg1, Tc = 92.0 K

Homologous series (O-Ca-Cu-Ba-Hg, varying oxygen):
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | -- | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | -- | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

T* = 127 K at O6.220Ca1Cu2Ba2Hg1 (tied with O6.210)
Dome shape: rises from 92 at O6.080, peaks at 127 near O6.21-6.22, falls to 98 at O6.310

## Step B: Filter Family B (Hg + exactly 4 elements)

threshold1 = 127 - 32 = 95

4-element Hg materials found (Hg search pages 1-3, all O-Cu-Ba-Hg):
Page 1: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
Page 2: O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
Page 3: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1

Hg search had 27 total matches (9 pages), seen 3 pages.
12 are series members (5 elements). Remaining 6 on pages 4-9 unknown.
Could not measure any 4-element materials (budget exhausted). Q1 left empty.

## Step C: Unmeasurable Materials (from discovery.json)

Found 4 unmeasurable (out of 7 total in database, discovered 24 of 35 total):
1. O6.160Ca1Cu2Ba2Hg1 (5 elements, series) - between O6.140(105) and O6.190(122), expected ~113 K - HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elements, series) - between O6.240(123) and O6.300(100), expected ~111 K - HIGH
3. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O,Ca,Cu,Sr,Tl,Pb) - no Hg, no Ba, has Sr, complex 6-element - LOW
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O,Ca,Cu,Ba,Tl) - has Ba, Tl-based cuprate - HIGH

Multi-factor rule: High if (contains Hg) OR (contains Ba AND <=5 elements AND Cu>=2)
- Single-factor rules fail: "all 5-element = high" wrong for some; "Hg presence" alone wrong for some

## Step D: Final Pick

Among unmeasurables predicted high:
- O6.160Ca1Cu2Ba2Hg1 (5 elements, expected Tc ~113)
- O6.270Ca1Cu2Ba2Hg1 (5 elements, expected Tc ~111)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements, expected Tc ~100)

All have 5 elements (tied). Higher expected Tc -> O6.160Ca1Cu2Ba2Hg1
