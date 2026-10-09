# Notes

## Task Overview
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low: < 90 K
- Budget used: 34/34 (exhausted)

## Step A: Homologous Series (Ca1Cu2Ba2Hg1, varying O) — 5 elements: O,Ca,Cu,Ba,Hg
All 12 members found via search "Ca1Cu2Ba2Hg1" (6 pages):
| Composition           | Tc (K)  | Status         |
|-----------------------|---------|----------------|
| O6.080Ca1Cu2Ba2Hg1   | 92.0    | Measured       |
| O6.100Ca1Cu2Ba2Hg1   | 92.5    | Measured       |
| O6.130Ca1Cu2Ba2Hg1   | 102.0   | Measured       |
| O6.140Ca1Cu2Ba2Hg1   | 105.0   | Measured       |
| O6.160Ca1Cu2Ba2Hg1   | ???     | UNSYNTHESIZED  |
| O6.190Ca1Cu2Ba2Hg1   | 122.0   | Measured       |
| O6.210Ca1Cu2Ba2Hg1   | 127.0   | Measured (PEAK)|
| O6.220Ca1Cu2Ba2Hg1   | 127.0   | Measured (PEAK)|
| O6.240Ca1Cu2Ba2Hg1   | 123.0   | Measured       |
| O6.270Ca1Cu2Ba2Hg1   | ???     | UNSYNTHESIZED  |
| O6.300Ca1Cu2Ba2Hg1   | 100.0   | Measured       |
| O6.310Ca1Cu2Ba2Hg1   | 98.0    | Measured       |

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (tied with O6.220)
Dome shape: Tc rises from 92→127 then falls to 98.
O6.160 interpolated ~110-115 K (between 105 and 122)
O6.270 interpolated ~112 K (between 123 and 100)

## Step B: threshold1 = 127 - 32 = 95.0 K
4-element Hg materials (Cu1Ba2Hg1 family, O-Cu-Ba-Hg): 15 search results, 14 are 4-element + 1 is 5-element (Tl)
| Composition           | Tc (K)  | >= 95? | Status         |
|-----------------------|---------|--------|----------------|
| O4.047Cu1Ba2Hg1      | ???     | —      | UNSYNTHESIZED  |
| O4.066Cu1Ba2Hg1      | ???     | —      | UNSYNTHESIZED  |
| O4.081Cu1Ba2Hg1      | 98.0    | YES    | Measured       |
| O4.085Cu1Ba2Hg1      | 92.0    | no     | Measured       |
| O4.096Cu1Ba2Hg1      | 95.6    | YES    | Measured       |
| O4.097Cu1Ba2Hg1      | 95.3    | YES    | Measured       |
| O4.101Cu1Ba2Hg1      | 93.4    | no     | Measured       |
| O4.103Cu1Ba2Hg1      | 92.5    | no     | Measured       |
| O4.150Cu1Ba2Hg1      | 97.0    | YES    | Measured       |
| O4.160Cu1Ba2Hg1      | ???     | —      | UNSYNTHESIZED  |
| O4.162Cu1Ba2Hg1      | ???     | ???    | BUDGET EXHAUSTED (has sample) |
| O4.180Cu1Ba2Hg1      | ???     | —      | UNSYNTHESIZED  |
| O4.200Cu1Ba2Hg1      | ???     | ???    | BUDGET EXHAUSTED (has sample) |
| O4.220Cu1Ba2Hg1      | ???     | ???    | BUDGET EXHAUSTED (has sample) |

5-element Tl material: O6Cu1Ba2Hg1Tl1 — also budget exhausted (has sample)

**Q1 (qualifying, Tc >= 95):** O4.081Cu1Ba2Hg1 (98.0), O4.096Cu1Ba2Hg1 (95.6), O4.097Cu1Ba2Hg1 (95.3), O4.150Cu1Ba2Hg1 (97.0)

## Step C: Unmeasurable materials (no sample exists)
6 unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements) — interpolated ~110-115 K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elements) — interpolated ~112 K → HIGH
3. O4.047Cu1Ba2Hg1 (4 elements) — far below measured range → LOW
4. O4.066Cu1Ba2Hg1 (4 elements) — below measured range → LOW
5. O4.160Cu1Ba2Hg1 (4 elements) — just above O4.150(97K) → HIGH
6. O4.180Cu1Ba2Hg1 (4 elements) — above measured range → LOW

**Rule (multi-factor):** Materials with Ca (Hg-1212, 5 elements, 2 CuO2 layers) always have HIGH Tc (all measured >90K, range 92-127K). For 4-element Cu-Ba-Hg (Hg-1201, 1 CuO2 layer), Tc is near 90K threshold; oxygen must be in/near optimal doping range (4.08-4.16). Outside this range, Tc drops below 90K.
- Single-factor "all HIGH" would be wrong on O4.047, O4.066, O4.180 (3 errors = "several")

## Step D: Final pick
Predicted HIGH unmeasurable: O6.160 (5 elem), O6.270 (5 elem), O4.160 (4 elem)
Fewest elements: O4.160Cu1Ba2Hg1 (4 elements)
**Final: O4.160Cu1Ba2Hg1**

## Budget Tracking
- search_db: 1 (Hg p1) + 1 (seed) + 6 (Ca-series) + 1 (Hg p2) + 1 (CuBaHg p1) + 7 (CuBaHg p2-8) = 17 pages = 17 units
- measure_tc (successful): 10 (Ca-series) + 7 (CuBaHg) = 17 units
- measure_tc (failed/free): 2 (Ca-series unsynth) + 2 (CuBaHg unsynth before budget) = 0 units
- Total: 17 + 17 = 34 ✓
