# Notes - Superconductor Tc Study

## Task Overview
- Budget: 26 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90K, Low = Tc < 90K

## Budget Usage
- Searches: 15 pages (Hg×9, Ca×5, specific×1)
- Measurements: 11 successful (cost 1 each)
- Unmeasurable checks: 5 (free - no sample)
- Total: 26/26

## Step A: Homologous Series (O,Ca,Cu,Ba,Hg - 5 elements)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured (high) |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured (high) |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured (high) |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured (high) |
| O6.160Ca1Cu2Ba2Hg1 | ??? | NO SAMPLE (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured (high) |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured (high) ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured (high) ← PEAK |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured (high) |
| O6.270Ca1Cu2Ba2Hg1 | ??? | NO SAMPLE (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured (high) |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured (high) |

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (also O6.220, both 127.0 K)
Dome shape: 92→92.5→102→105→[?]→122→127→127→123→[?]→100→98

## Step B: 4-element Hg materials (O,Cu,Ba,Hg)
threshold1 = 127.0 - 32 = 95.0 K

Measured:
- O4.047Cu1Ba2Hg1: Tc = 95.0 K → QUALIFIES (>= 95.0)

Unmeasurable:
- O4.180Cu1Ba2Hg1: no sample

Unmeasured (budget exhausted, has sample):
- O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.162, O4.200, O4.220

Unknown composition (search found them but measure_tc doesn't recognize):
- O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1

Q1 = ["O4.047Cu1Ba2Hg1"] (only confirmed qualifying member)

## Other Materials Found
- O10Ca2Cu3Ba2Tl2 (5 elements: O,Ca,Cu,Ba,Tl) - has sample, couldn't measure
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O,Ca,Cu,Sr,Tl,Pb) - NO SAMPLE
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O,Ca,Cu,Ba,Tl) - NO SAMPLE
- O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl) - has sample, couldn't measure

## Step C: Unmeasurable Materials (confirmed no sample)
1. O6.160Ca1Cu2Ba2Hg1 (5 el) - in dome between 105 and 122 → ~112 K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 el) - in dome between 123 and 100 → ~112 K → HIGH
3. O4.180Cu1Ba2Hg1 (4 el) - 4-element series → HIGH (similar to O4.047=95K)
4. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 el) - Tl-based → HIGH
5. O8Ca0.850Cu2.200Ba2Tl1.800 (5 el) - Tl-based → HIGH

Rule: "A material is high (Tc >= 90 K) if it contains Cu AND at least one of {Hg, Tl} as constituent elements."
- All measured materials satisfy this and are high (92-127 K)
- Single-factor "contains Hg → high" would miss #4 and #5 (Tl-based, no Hg) = wrong on 2

## Step D: Final Pick
Among predicted-high unmeasurable materials:
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final: O4.180Cu1Ba2Hg1** (4 elements, fewest)
