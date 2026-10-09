# Notes - Superconductor Tc Study

## Task Overview
- Budget: 26 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90 K, Low = Tc < 90 K

## Step A - Series Peak
Homologous series: O{6+x}Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
Members and Tc:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (high)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (high)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (high)
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable) → interpolated ~112 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (high)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (high) ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (high) ← PEAK (tied)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (high)
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable) → interpolated ~112 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (high)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (high)

T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1
Clear parabolic doping curve: peak near O≈6.21-6.22

## Step B - Filter Family B (Hg + exactly 4 elements)
threshold1 = 127.0 - 32 = 95.0 K

4-element Hg materials (O, Cu, Ba, Hg) found in search:
Pages 1-4 (12 materials): O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180
Page 5 (2 more): O4.200, O4.220
Total: 14 four-element Hg materials

Measured:
- O4.047Cu1Ba2Hg1: 95.0 K (high, EXACTLY at threshold → qualifies)
- O4.101Cu1Ba2Hg1: 93.4 K (high, below threshold)

Trend: Tc decreases with increasing O content. Only O4.047 qualifies for Q1.

Q1 = ["O4.047Cu1Ba2Hg1"]

## Step C - Extrapolation Rule
Rule: Multi-factor rule combining element count and oxygen stoichiometry:
- 5-element (O-Ca-Cu-Ba-Hg): parabolic Tc vs O curve, all measured members HIGH (92-127 K)
- 4-element (O-Cu-Ba-Hg): decreasing Tc vs O, measured members HIGH (93-95 K), high-O members may be LOW

Unmeasurable materials confirmed:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements): interpolated Tc ~112 K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elements): interpolated Tc ~112 K → HIGH

## Step D - Final Pick
Both predicted-high unmeasurable materials have 5 elements.
Tie-break by higher expected Tc:
- O6.160Ca1Cu2Ba2Hg1: interpolated ~111.8 K
- O6.270Ca1Cu2Ba2Hg1: interpolated ~111.5 K
Final pick: O6.160Ca1Cu2Ba2Hg1

## Budget Tracking
- Searches: 13 units (Hg pages 1-7, Ca pages 1-5, one wasted query)
- Measurements: 12 units (10 successful + 2 unmeasurable = free)
- Actually total used: 26/26

## Other materials found (non-Hg, from Ca search):
- O10Ca2Cu3Ba2Tl2 (5 elements: O,Ca,Cu,Ba,Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O,Ca,Cu,Sr,Tl,Pb)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O,Ca,Cu,Ba,Tl)
(These don't contain Hg, not relevant for Step B)

## Remaining Hg search pages (8-9) not explored:
Expected to contain: O6.220, O6.240, O6.270, O6.300, O6.310 (Ca series) + possibly 1 more unknown material (27 total - 26 accounted = 1 missing)
