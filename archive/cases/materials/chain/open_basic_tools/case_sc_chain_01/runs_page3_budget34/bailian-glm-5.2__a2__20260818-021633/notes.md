# Superconductor Tc study — notes

## Task summary
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg)
- Budget: 34 units total (measure_tc & search_db each cost 1; **unmeasurable measure_tc calls are FREE**)
- High = Tc >= 90 K, Low = Tc < 90 K

## KEY DISCOVERY
**Unmeasurable `measure_tc` calls do NOT cost budget.** Only successful (sample_available=true) measurements and search pages increment budget_used.

## Hg search (9 pages, 27 materials, budget 1→9)
- 4-elem family B (O-Cu1Ba2Hg1, vary O): O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220 (14)
- 5-elem seed series (O-Ca1Cu2Ba2Hg1, vary O): O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310 (12)
- 5-elem w/ Tl: O6Cu1Ba2Hg1Tl1 (1)

## Step A — seed series dome (budget 9→19; 12 calls, 2 free)
Sorted by O:
| O | Tc | Status |
|---|---|---|
| 6.080 | 92.0 | ✓ |
| 6.100 | 92.5 | ✓ |
| 6.130 | 102.0 | ✓ |
| 6.140 | 105.0 | ✓ |
| 6.160 | ? | UNMEAS |
| 6.190 | 122.0 | ✓ |
| 6.210 | 127.0 | ✓ PEAK |
| 6.220 | 127.0 | ✓ PEAK |
| 6.240 | 123.0 | ✓ |
| 6.270 | ? | UNMEAS |
| 6.300 | 100.0 | ✓ |
| 6.310 | 98.0 | ✓ |

Peak among measured = 127.0 at O6.210 & O6.220.
**T* = 127.0**. **threshold1 = 127 - 32 = 95.0**

## Step B — family B (O4-Cu1Ba2Hg1, 4 elem, Hg✓, Tc>=95.0) (budget 19→29; 14 calls, 3 free)
Sorted by O:
| O | Tc | >=95? | Status |
|---|---|---|---|
| 4.047 | ? | (low) | UNMEAS |
| 4.066 | ? | (low) | UNMEAS |
| 4.081 | 98.0 | ✓ | measured |
| 4.085 | 92.0 | ✗ | measured |
| 4.096 | 95.6 | ✓ | measured |
| 4.097 | 95.3 | ✓ | measured |
| 4.101 | 93.4 | ✗ | measured |
| 4.103 | 92.5 | ✗ | measured |
| 4.150 | 97.0 | ✓ | measured |
| 4.160 | ? | (high,~97.3) | UNMEAS |
| 4.162 | 97.4 | ✓ | measured |
| 4.180 | ? | (high,~97.1) | UNMEAS |
| 4.200 | 94.2 | ✗ | measured |
| 4.220 | 97.0 | ✓ | measured |

Measured Q1 (Tc>=95.0): O4.081, O4.096, O4.097, O4.150, O4.162, O4.220.

## O6Cu1Ba2Hg1Tl1 — measured Tc=0.0 (budget 29→30). 5 elem, not in B, not unmeasurable.

## Step C — interpolation rule for unmeasurables
RULE: Predict Tc by linear interpolation between the two nearest measured neighbors within the SAME homologous series (same non-O stoichiometry). Then classify high (>=90) vs low (<90). Respects the dome (non-monotonic), avoiding the single-factor trap of "higher O → higher Tc".

### Predictions (all 6 unmeasurables discovered via Hg search):
- O4.047Cu1Ba2Hg1: below lowest measured (O4.081→98.0). Extrapolate/edge → low. ~<90. → LOW
- O4.066Cu1Ba2Hg1: below lowest measured O4.081→98.0. Edge → low. → LOW
- O4.160Cu1Ba2Hg1: interp O4.150(97.0)–O4.162(97.4): 97.33 → HIGH
- O4.180Cu1Ba2Hg1: interp O4.162(97.4)–O4.200(94.2): 97.1 → HIGH
- O6.160Ca1Cu2Ba2Hg1: interp O6.140(105)–O6.190(122): 113.4 → HIGH
- O6.270Ca1Cu2Ba2Hg1: interp O6.240(123)–O6.300(100): 111.5 → HIGH

### Sanity: interpolation rule fits all measured points (they ARE the data). No single-factor error.

## Step B — Q1 (with predictions, threshold1=95.0):
Measured >=95.0: O4.081(98.0), O4.096(95.6), O4.097(95.3), O4.150(97.0), O4.162(97.4), O4.220(97.0)
Predicted >=95.0 (unmeasurable): O4.160(~97.3), O4.180(~97.1)
Predicted <95.0 (unmeasurable, excluded): O4.047(low), O4.066(low)
**Q1 = {O4.081, O4.096, O4.097, O4.150, O4.160, O4.162, O4.180, O4.220}**

## Step D — among predicted-HIGH unmeasurables, fewest elements (tie→higher exp Tc)
Predicted-high unmeasurables:
- 5-elem: O6.160Ca1Cu2Ba2Hg1 (~113.4), O6.270Ca1Cu2Ba2Hg1 (~111.5)
- 4-elem: O4.160Cu1Ba2Hg1 (~97.3), O4.180Cu1Ba2Hg1 (~97.1)
Fewest elements = 4. Among 4-elem highs: O4.160 (~97.3) > O4.180 (~97.1).
**FINAL = O4.160Cu1Ba2Hg1**

## Budget: 30 used, 4 remaining (unused — sufficient; no need to spend more).
