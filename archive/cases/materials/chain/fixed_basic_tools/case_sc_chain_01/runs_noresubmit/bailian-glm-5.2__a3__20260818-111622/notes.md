# Superconductor Tc study — working notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elts: O,Ca,Cu,Ba,Hg). HIGH = Tc>=90 K. Budget 18.

## Step A — series peak (Ca1Cu2Ba2Hg1, vary O)  → T*
| O | Tc | |
|---|---|---|
| 6.080 | 92.0 | |
| 6.100 | 92.5 | (18th meas, lower-bound check) |
| 6.140 | 105.0 | |
| 6.190 | 122.0 | |
| 6.220 | **127.0** | ← PEAK |
| 6.240 | 123.0 | |
| 6.310 | 98.0 | |
Unmeasured series members: 6.130, 6.210, 6.300. Curve rises 92→127 @6.22, falls to 98 @6.31.
**T* = 127.0 K at O6.220Ca1Cu2Ba2Hg1.**

## Step B — family B (contains Hg AND exactly 4 elts) → Q1, threshold1 = T*-32 = 95.0
Candidates = O4.0xx Cu1Ba2Hg1 (elts O,Cu,Ba,Hg = 4). All 11 measured:
| comp | Tc | ≥95? |
|---|---|---|
| O4.047 | 95.0 | ✓ (boundary, inclusive) |
| O4.081 | 98.0 | ✓ |
| O4.085 | 92.0 | ✗ |
| O4.096 | 95.6 | ✓ |
| O4.097 | 95.3 | ✓ |
| O4.101 | 93.4 | ✗ |
| O4.103 | 92.5 | ✗ |
| O4.150 | 97.0 | ✓ |
| O4.162 | 97.4 | ✓ |
| O4.200 | 94.2 | ✗ |
| O4.220 | 97.0 | ✓ |
**Q1 = {O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220} (7 members).**
**threshold1 = 95.0.**

## Step C — rule + predictions for unmeasurable.txt (no sample → induce, predict)
Measured evidence: ALL Hg materials (Hg-1201 Cu1: 92-98; Hg-1212 Cu2: 92-127) are HIGH.
No measured LOW example and no measured Tl example (budget exhausted before lines 22-26).
→ Rule grounded in measured Hg-1212/Hg-1201 data + cuprate chemistry.

**Rule (multi-factor interaction):** HIGH iff BOTH (a) a heavy charge-reservoir/pairing
cation from {Hg, Tl, Pb} is present AND (b) the proper Ba-based cuprate block-layer
architecture (Ba + Ca/Y spacer + CuO2 planes) at favorable O doping. The pairing cation
alone is not sufficient: a Tl/Pb layer on an Sr-based (not Ba) block, or wrong O doping,
drops Tc below 90; and the Y-Ba-La-Cu family with NO Hg/Tl/Pb layer is LOW.
⇒ single-factor "contains Hg/Tl" is WRONG on #5 (has Tl/Pb but Sr-only → LOW).

Interpolations (Hg series, solid):
- #1 O6.160Ca1Cu2Ba2Hg1: between 6.140(105) & 6.190(122) → ~113 → HIGH
- #2 O6.270Ca1Cu2Ba2Hg1: between 6.240(123) & 6.310(98) → ~112 → HIGH
- #3 O4.180Cu1Ba2Hg1: between 4.150(97.0) & 4.200(94.2) → ~95.6 → HIGH
Domain (Tl/Y cuprates):
- #4 O8Ca0.850Cu2.200Ba2Tl1.800: Tl-2212 (Tl~2, Ba2, Cu2.2, Ca) → ~108 → HIGH
- #5 O7Ca1Cu2Sr2Tl0.500Pb0.500: (Tl,Pb)-1212 on Sr (no Ba) → ~80-85 → LOW
- #6 O8Cu3Y1Ba1.900La0.100: YBCO-123 type, no Hg/Tl/Pb, O8 overdoped → <90 → LOW
- #7 O8Cu4Y1Ba1.870La0.130: Y-124 (Cu4) type, no Hg/Tl/Pb → ~80 → LOW

Predictions: HIGH={#1,#2,#3,#4}; LOW={#5,#6,#7}.

## Step D — among predicted-HIGH unmeasurables, fewest elements (tie→higher Tc)
- #1 O6.160Ca1Cu2Ba2Hg1: 5 elts (O,Ca,Cu,Ba,Hg)
- #2 O6.270Ca1Cu2Ba2Hg1: 5 elts
- #3 O4.180Cu1Ba2Hg1: **4 elts** (O,Cu,Ba,Hg) ← FEWEST
- #4 O8Ca0.850Cu2.200Ba2Tl1.800: 5 elts
**Final = O4.180Cu1Ba2Hg1.**

## Budget log (18/18)
1 O6.080, 2 O6.140, 3 O6.190, 4 O6.240, 5 O6.310,
6 O4.047, 7 O4.097, 8 O4.150, 9 O4.200, 10 O4.220,
11 O6.220(=peak 127), 12 O4.081, 13 O4.085, 14 O4.096, 15 O4.101, 16 O4.103, 17 O4.162,
18 O6.100.
Note: did not get to measure non-Hg materials (lines 22-26); rule for Tl/Y families
relies on cuprate domain knowledge anchored by measured Hg data.
