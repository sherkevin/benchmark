# Notes - Superconductor Tc Study

## Budget: 26/26 used (0 remaining)

## Seed: O6.080Ca1Cu2Ba2Hg1 → Tc = 92.0 K

## Step A: Homologous Series OxCa1Cu2Ba2Hg1 (12 members)
| Composition | Tc (K) | Sample? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes |
| O6.160Ca1Cu2Ba2Hg1 | -- | NO (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes |
| O6.270Ca1Cu2Ba2Hg1 | -- | NO (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes |

**Peak: O6.210Ca1Cu2Ba2Hg1, T* = 127.0 K**

## All Hg materials found (27 total via search_db)
- 12 × OxCa1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg) - the series above
- 14 × OxCu1Ba2Hg1 (4 elements: O,Cu,Ba,Hg):
  O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
- 1 × O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl)

## Step B: 4-element Hg materials with Tc >= 95 (threshold = 127-32 = 95)
Measured 3 of 14:
- O4.047Cu1Ba2Hg1: 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 ✓
Other 11 untested (budget exhausted). Based on trend (Tc ~95-97 across O range), likely all >= 95.

## Step C: Unmeasurable materials
Confirmed: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1
- O6.160: interpolated ~112 K → high (between O6.140=105 and O6.190=122)
- O6.270: interpolated ~112 K → high (between O6.240=123 and O6.300=100)

Note: O4.066Cu1Ba2Hg1 gave "unknown composition" error - unclear if unmeasurable
Note: Warning says single-factor rules are wrong on several - may be missing unmeasurable materials

## Step D: Final pick
Both unmeasurable materials have 5 elements and are predicted high.
Tie-break by expected Tc: very close (~112 K for both). O6.160 slightly higher.
→ O6.160Ca1Cu2Ba2Hg1
