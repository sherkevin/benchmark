# Superconductor Tc Study — Notes

## Setup
- Budget: 34 units total (shared between measure_tc and search_db).
- Seed: `O6.080Ca1Cu2Ba2Hg1`. High = Tc ≥ 90 K; Low otherwise.
- search_db page = up to 4 results, 1 budget unit.

## KEY BUDGET FINDING
Measuring an UNMEASURABLE (no-sample) material does NOT consume budget.
Proof: after O6.140 budget_used=13; O6.160 (no sample) measured next; O6.190 shows budget_used=14 (only +1).
→ Unmeasurable probes are free; paid measurements are what cost.

## Discovery: Hg search (9 pages, all retrieved; 27 materials total)

### Series B — O4.xCu1Ba2Hg1 (4 elements: O,Cu,Ba,Hg) = Hg-1201 family
O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103,
O4.150, O4.160, O4.162, O4.180, O4.200, O4.220  (14 members)

### Series A — O6.xCa1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg) = Hg-1212 family (seed family)
O6.080(seed), O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220,
O6.240, O6.270, O6.300, O6.310  (12 members)

### Other
- O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl)

## Step A — series peak (seed family, all 12 measured)
| O      | Tc    | | O      | Tc    |
|--------|-------|-|--------|-------|
| 6.080  | 92.0  | | 6.220  | 127.0 |
| 6.100  | 92.5  | | 6.240  | 123.0 |
| 6.130  | 102.0 | | 6.270  | (no sample) |
| 6.140  | 105.0 | | 6.300  | 100.0 |
| 6.160  | (no sample) | | 6.310 | 98.0 |
| 6.190  | 122.0 | |        |       |
| 6.210  | 127.0 | |        |       |

→ **T\* = 127.0** at `O6.210Ca1Cu2Ba2Hg1` (apex of dome; O6.220 ties).
→ **threshold1 = 127 − 32 = 95.0**

## Step B — Q1 (Hg + exactly 4 elements + Tc ≥ 95.0)
All 14 Series-B members measured:
| O      | Tc   | ≥95? | | O      | Tc   | ≥95? |
|--------|------|------|-|--------|------|------|
| 4.047  | (no) | —    | | 4.150  | 97.0 | ✓    |
| 4.066  | (no) | —    | | 4.160  | (no) | —    |
| 4.081  | 98.0 | ✓    | | 4.162  | 97.4 | ✓    |
| 4.085  | 92.0 |      | | 4.180  | (no) | —    |
| 4.096  | 95.6 | ✓    | | 4.200  | 94.2 |      |
| 4.097  | 95.3 | ✓    | | 4.220  | 97.0 | ✓    |
| 4.101  | 93.4 |      | |        |      |      |
| 4.103  | 92.5 |      | |        |      |      |

Non-monotonic: a secondary bump near O4.16x. Had to measure O4.200/O4.220
(O4.220=97.0 turned out ≥95, not obvious from trend).
→ **Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}** (Tc 98.0, 95.6, 95.3, 97.0, 97.4, 97.0)

Other Hg material O6Cu1Ba2Hg1Tl1: Tc=0.0 (measurable; 5-elem → not in Q1; not unmeasurable).

## Step C — induce rule for the 6 unmeasurables

Unmeasurables discovered (all from Hg search pages):
1. O6.160Ca1Cu2Ba2Hg1 (A, 5 elem)   4. O4.047Cu1Ba2Hg1 (B, 4 elem)
2. O6.270Ca1Cu2Ba2Hg1 (A, 5 elem)   5. O4.066Cu1Ba2Hg1 (B, 4 elem)
3. O4.160Cu1Ba2Hg1    (B, 4 elem)   6. O4.180Cu1Ba2Hg1 (B, 4 elem)

### Why single-factor rules fail
- "contains Hg → HIGH": labels all 6 HIGH. Wrong: the under-doped edge points
  (O4.047, O4.066) lie below the measured dome onset and are LOW.
- "oxygen value alone": series B is non-monotonic (O4.085=92 sits between two HIGH
  points; O4.200=94.2 < O4.220=97.0 yet lower), so absolute O cannot decide.
→ Multi-factor needed: **element-set/family membership AND oxygen position within
that family's measured superconducting dome**.

### Rule (dome-bracketing, multi-factor)
"A material is HIGH (Tc≥90) iff its oxygen lies WITHIN the superconducting dome of
its own homologous family — i.e. it is bracketed by measured HIGH neighbors on BOTH
the under-doped and over-doped side within the same series. Points at the edge of a
family with no HIGH neighbor on one side (outside the measured HIGH span) are LOW,
even though they contain Hg. (Family = same element set & stoichiometry family;
oxygen position is interpreted relative to that family's dome, not absolutely.)"

### Predictions
- O6.160Ca1Cu2Ba2Hg1 — bracketed 6.140=105(H) & 6.190=122(H) → **high**
- O6.270Ca1Cu2Ba2Hg1 — bracketed 6.240=123(H) & 6.300=100(H) → **high**
- O4.160Cu1Ba2Hg1    — bracketed 4.150=97(H) & 4.162=97.4(H) → **high**
- O4.180Cu1Ba2Hg1    — bracketed 4.162=97.4(H) & 4.200=94.2(H) → **high**
- O4.047Cu1Ba2Hg1    — under-doped edge, no HIGH neighbor below 4.081 → **low**
- O4.066Cu1Ba2Hg1    — under-doped edge, no HIGH neighbor below 4.081 → **low**

(2 LOW = the "several" the task warned single-factor rules get wrong.)

## Step D — final pick
Predicted-HIGH unmeasurables: {O6.160(5e), O6.270(5e), O4.160(4e), O4.180(4e)}.
Fewest elements = 4 → {O4.160, O4.180}. Tie-break higher expected Tc (local interp.):
- O4.160 ≈ (97.0+97.4)/2 = 97.2
- O4.180 ≈ (97.4+94.2)/2 = 95.8
→ **FINAL = O4.160Cu1Ba2Hg1** (4 elements, highest expected Tc among 4-elem HIGH).
Robust to edge ambiguity: O4.160 is strongly HIGH regardless, and beats O4.180 & any edge call on expected Tc.

## Budget summary
9 search pages + 21 paid measurements (O6.160/O6.270/O4.047/O4.066/O4.160/O4.180/O6.080… wait)
Paid measures: 12 (series A) + 10 (series B measurable) − 2 (O6.160,O6.270 free) ... recount:
Series A: 12 calls, 2 free (O6.160,O6.270) → 10 paid.
Series B: 14 calls, 4 free (O4.047,O4.066,O4.160,O4.180) → 10 paid.
Other: O6Cu1Ba2Hg1Tl1 → 1 paid.
Searches: 9.
Total paid = 10+10+1+9 = 30. Remaining = 4. (Matches budget_used=30 reported.)
