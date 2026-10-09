# Notes

## Task Overview
- Budget: 18 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90K, Low = Tc < 90K

## Budget Tracking
- Used: 18/18 (exhausted)
- Note: Unmeasurable measurements (O6.160, O6.270) did NOT cost budget

## Step A: Homologous Series (O_x Ca1 Cu2 Ba2 Hg1)
All series members found via Ca search (5 pages, 15 results total):
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured (seed) |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured ← PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | — | UNMEASURABLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | ? | not measured (budget) |

**Peak: T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1** (first to reach max)

## Step B: threshold1 = 127.0 - 32 = 95.0
Need Hg materials with exactly 4 elements and Tc >= 95.0.
4-element Hg materials found (O, Cu, Ba, Hg) from Hg search pages 1-4:
- O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
(12 of 27 Hg matches seen; 5 more pages remain unseen)
**Could not measure any — budget exhausted.**

## Step C: Unmeasurable Materials
Known unmeasurable: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
Rule induced: All measurable 5-element series members are HIGH (Tc >= 90).
Materials with Ca (5 elements) → high; materials without Ca (4 elements) → likely low.
Warning: single-factor rules are wrong on several.

Expected Tc for unmeasurable series members (linear interpolation):
- O6.160: between O6.140(105) and O6.190(122) → ~111.8 K → HIGH
- O6.270: between O6.240(123) and O6.300(100) → ~111.5 K → HIGH

## Step D: Final Pick
Among unmeasurable predicted HIGH, fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~111.8
- O6.270Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~111.5
Tie-break (higher expected Tc): **O6.160Ca1Cu2Ba2Hg1**

## Attempted additional measurements
- Tried O4.047Cu1Ba2Hg1 → "instrument time exhausted" (even for potentially unmeasurable materials)
- Note: Earlier, unmeasurable attempts (O6.160, O6.270) did not consume budget, but now budget is fully exhausted.

## Limitations
- Could not measure any 4-element Hg materials for Step B (Q1 is empty due to lack of data)
- Only 2 unmeasurable materials confirmed (O6.160, O6.270)
- 15 more Hg search pages unseen (27 total Hg matches, only 12 seen)
- O6.310Ca1Cu2Ba2Hg1 status unknown (not measured)

## Final Answer
Written to answer.json
