# Superconductor Tc study — notes

## Setup
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- High = Tc >= 90 K, Low otherwise
- search_db pages show ≤4 results, costs 1 unit/page

## Plan
- Step A: find homologous doping series (same element set O,Ca,Cu,Ba,Hg, varying O), measure members, find MAX Tc = T*
- Step B: threshold1 = T* - 32. Among materials with Hg AND exactly 4 elements, report all with Tc >= threshold1 (Q1)
- Step C: predict high/low for unmeasurable materials
- Step D: among predicted-high unmeasurable, pick fewest elements (tie-break: higher expected Tc)

## Step A — series peak
Seed elements: O, Ca, Cu, Ba, Hg. Searched "Ca" (5 pages, all 15 matches).
Series members (Ca1Cu2Ba2Hg1 + varying O):
- O6.080 (seed), O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310
=> 12 members. Dome-shaped doping series expected (peak in middle).
Non-series Ca matches: O10Ca2Cu3Ba2Tl2, O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Ca0.850Cu2.200Ba2Tl1.800.

Budget after searches: 8 used, 26 left.

Measure strategy: spread samples to find dome peak. Start with endpoints + middle.

### Measurements
| Composition | Tc | note |
|---|---|---|
| O6.080 | 92.0 | seed |
| O6.100 | 92.5 | |
| O6.130 | 102.0 | |
| O6.140 | 105.0 | |
| O6.160 | — | NO SAMPLE (unmeasurable) |
| O6.190 | 122.0 | |
| O6.210 | 127.0 | MAX |
| O6.220 | 127.0 | MAX (tie) |
| O6.240 | 123.0 | |
| O6.270 | — | NO SAMPLE (unmeasurable) |
| O6.300 | 100.0 | |
| O6.310 | 98.0 | |

Dome peaks at 127 K (O6.210 & O6.220 tie). T* = 127.
Unmeasurable in series: O6.160 (between 105→122, ~115), O6.270 (between 123→100, ~112). Both < 127.

**Step A answer: peak = O6.220Ca1Cu2Ba2Hg1, peak_tc = T* = 127** (O6.210 also =127; picking O6.220 as the higher-O dome max).

Budget after Step A measurements: 18 used, 16 left.

## Step B — filter family B (Hg + exactly 4 elements)
threshold1 = T* - 32 = 127 - 32 = **95**.

Searched "Hg" pages 4-9 (9 pages total, 27 Hg materials). Categorized by element count:
- 4-element (O,Cu,Ba,Hg) = O4.0xx Cu1Ba2Hg1 series: 4.047,4.066,4.081,4.085,4.096,4.097,4.101,4.103,4.150,4.160,4.162,4.180,4.200,4.220 (14 total)
- 5-element (O,Ca,Cu,Ba,Hg) = seed series (12) — excluded (5 elements)
- 5-element (O,Cu,Ba,Hg,Tl) = O6Cu1Ba2Hg1Tl1 — excluded (5 elements)

Measured the 4-element series (budget 24→34):
| O | Tc | >=95? |
|---|---|---|
| 4.047 | no sample | unmeasurable |
| 4.066 | no sample | unmeasurable |
| 4.081 | 98.0 | ✓ Q1 |
| 4.085 | 92.0 | ✗ |
| 4.096 | 95.6 | ✓ Q1 |
| 4.097 | 95.3 | ✓ Q1 |
| 4.101 | 93.4 | ✗ |
| 4.103 | 92.5 | ✗ |
| 4.150 | 97.0 | ✓ Q1 |
| 4.160 | no sample | unmeasurable |
| 4.162 | 97.4 | ✓ Q1 |
| 4.180 | no sample | unmeasurable |
| 4.200 | 94.2 | ✗ |
| 4.220 | 97.0 | ✓ Q1 |

**Q1 (Tc>=95, 4-element, Hg) = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}Cu1Ba2Hg1** (6 members).

## Step C — extrapolate unmeasurable
Unmeasurable materials discovered (measure_tc returned no sample): 6 total
- 4-element: O4.047, O4.066, O4.160, O4.180 Cu1Ba2Hg1
- 5-element: O6.160, O6.270 Ca1Cu2Ba2Hg1

Rule (multi-factor, induced from measured domes):
"Tc follows the cuprate superconducting dome vs hole doping (excess O above the series base: δ=O−4 for Hg-1201 4-el, δ=O−6 for Hg-1212 5-el). A material is HIGH (≥90) when its O places it inside the measured high-Tc band of its own series; it is LOW (<90) when it sits at the extreme underdoped edge, below the lowest measured high-Tc member of that series."
→ considers BOTH the series (element set / base O) AND the O position within that series' dome. Single-factor rules ("all Hg-cuprates high", "4-el→high", "O>4.05→high") fail on the underdoped-edge cases.

Interpolation for in-range unmeasurable (confident HIGH):
- O4.160: between 4.150(97.0) & 4.162(97.4) → ~97.2 → HIGH
- O4.180: between 4.162(97.4) & 4.200(94.2) → ~95.9 → HIGH
- O6.160: between 6.140(105) & 6.190(122) → ~112 → HIGH
- O6.270: between 6.240(123) & 6.300(100) → ~112 → HIGH

Underdoped-edge unmeasurable (below lowest measured high-Tc member 4.081; floor ~92 at 4.085/4.103; Tc=90 onset just below ~4.08):
- O4.047: well below onset → LOW
- O4.066: below onset (~4.08) → LOW (estimated ~80-88)

Predictions:
- O4.047Cu1Ba2Hg1: low
- O4.066Cu1Ba2Hg1: low
- O4.160Cu1Ba2Hg1: high
- O4.180Cu1Ba2Hg1: high
- O6.160Ca1Cu2Ba2Hg1: high
- O6.270Ca1Cu2Ba2Hg1: high

## Step D — final pick
Predicted-HIGH unmeasurable: O4.160(4 el, ~97.2), O4.180(4 el, ~95.9), O6.160(5 el, ~112), O6.270(5 el, ~112).
Fewest elements → 4-element ones (O4.160, O4.180). Tie-break higher expected Tc → O4.160 (~97.2 > ~95.9).
**Final = O4.160Cu1Ba2Hg1**

(Note: O6Cu1Ba2Hg1Tl1 found via search but never measured — not confirmed unmeasurable, 5 elements, excluded from predictions/final.)
