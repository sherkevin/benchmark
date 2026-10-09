# Superconductor Tc Study Notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`; High = Tc >= 90 K; Budget 34 shared.
- KEY INSIGHT: measure_tc on an UNMEASURABLE material (no sample) does NOT consume budget.
  Verified in batches: when N calls include k unmeasurables, budget increments by N-k.

## Step A — homologous Ca series (Ca1Cu2Ba2Hg1 + varying O)
All 12 members found via search_db("Ca1Cu2Ba2Hg1") pages 1-3:
| O6.080 | 92.0  |
| O6.100 | 92.5  |
| O6.130 | 102.0 |
| O6.140 | 105.0 |
| O6.160 | UNMEASURABLE |
| O6.190 | 122.0 |
| O6.210 | 127.0 |  <-- T* (tie with 6.220)
| O6.220 | 127.0 |  <-- T* (tie)
| O6.240 | 123.0 |
| O6.270 | UNMEASURABLE |
| O6.300 | 100.0 |
| O6.310 | 98.0  |

T* = 127.0 K (peak at O6.210, first max). threshold1 = 127 - 32 = 95.0.

## All Hg materials (27) via search_db("Hg") pages 1-7
Family B (contains Hg & exactly 4 elements) = O4.xxxCu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg):
  O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103,
  O4.150, O4.160, O4.162, O4.180, O4.200, O4.220  (14 total)
Excluded (5 elements): O6.xxxCa1Cu2Ba2Hg1 (Ca added), O6Cu1Ba2Hg1Tl1 (Tl added).

## Step B — Family B measured Tc (budget tracking: unmeasurables free)
| O4.047 | UNMEASURABLE |
| O4.066 | UNMEASURABLE |
| O4.081 | 98.0  | >=95 Q1
| O4.085 | 92.0  |
| O4.096 | 95.6  | >=95 Q1
| O4.097 | 95.3  | >=95 Q1
| O4.101 | 93.4  |
| O4.103 | 92.5  |
| O4.150 | 97.0  | >=95 Q1
| O4.160 | UNMEASURABLE |
| O4.162 | 97.4  | >=95 Q1
| O4.180 | UNMEASURABLE |
| O4.200 | 94.2  |
| O4.220 | 97.0  | >=95 Q1

Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}  (measured Tc >= 95).

## Step C — rule induction & predictions
Measured HIGH (Tc>=90): all family B (92-98) + all family Ca (92-127).
Measured LOW: O6Cu1Ba2Hg1Tl1 (Tc=0). O/metal = 6/(1+2+1+1)=6/5=1.20 (overdoped).

Compute O / (sum of non-O atoms) = O/metal ratio:
Family B (metals = Cu1Ba2Hg1 = 4): measured HIGH ratios 1.020-1.055.
Family Ca (metals = Ca1Cu2Ba2Hg1 = 6): measured HIGH ratios 1.013-1.052.
LOW Tl material: ratio 1.20 (way above).
=> HIGH window ≈ [1.013, 1.055]; outside (underdoped < ~1.013 OR overdoped > ~1.06) => LOW.

Unmeasurable predictions:
- O4.047Cu1Ba2Hg1: 4.047/4 = 1.0118  < 1.013 (underdoped edge) => LOW
- O4.066Cu1Ba2Hg1: 4.066/4 = 1.0165  in window => HIGH
- O4.160Cu1Ba2Hg1: 4.160/4 = 1.040   in window => HIGH
- O4.180Cu1Ba2Hg1: 4.180/4 = 1.045   in window => HIGH
- O6.160Ca1Cu2Ba2Hg1: 6.160/6 = 1.0267 in window => HIGH
- O6.270Ca1Cu2Ba2Hg1: 6.270/6 = 1.045  in window => HIGH

Single-factor "all family-B/Ca cuprates are HIGH" would be WRONG for O4.047 (underdoped
edge) — confirming the multi-factor O/metal doping-window rule is needed.

## Step D — final pick
Predicted-HIGH unmeasurables: O6.160(5e), O6.270(5e), O4.066(4e), O4.160(4e), O4.180(4e).
Fewest elements = 4: {O4.066, O4.160, O4.180}.
Expected Tc (interpolate within family B):
- O4.066: extrapolated low end ~92-93 (near underdoped edge)
- O4.160: between O4.150(97.0)&O4.162(97.4) => ~97.3
- O4.180: between O4.162(97.4)&O4.200(94.2) => ~95.9
Highest expected Tc => O4.160Cu1Ba2Hg1.

## Budget
Final: used 31/34 (search 20 + measure 11 synthesized). 3 remaining (unused).
