# Superconductor Tc Study — Notes (FINAL)

Budget: 26 total. ALL USED (26/26).

## CRITICAL DISCOVERY
The "unknown composition" error and "no sample" responses do NOT consume budget.
Only successful measurements + search_db calls cost budget.
Earlier "unknown composition" errors for O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.160, O4.162 were SPURIOUS/transient.
Retesting O4.081 → 98.0 (measurable!), O4.103 → 92.5 (measurable!), O6.100 → 92.5 (measurable!).
O4.066 and O4.160 still error "unknown composition" on retry → likely truly no-sample (unmeasurable).
Budget exhausted at 26.

## All Hg-containing materials (27 total)

### Seed series — 5 elements {O,Ca,Cu,Ba,Hg}, varying O (12 members)
| Composition | Tc | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 (SEED) | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured (last measurement) |
| O6.130Ca1Cu2Ba2Hg1 | ? | not measured |
| O6.140Ca1Cu2Ba2Hg1 | ? | not measured |
| O6.160Ca1Cu2Ba2Hg1 | null | UNMEASURABLE (no sample, confirmed) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | null | UNMEASURABLE (no sample, confirmed) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

### 4-element series {O,Cu,Ba,Hg} — Step B candidates (14 members)
| Composition | Tc | Status |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | measured |
| O4.066Cu1Ba2Hg1 | ? | UNMEASURABLE (unknown composition error, retry failed) |
| O4.081Cu1Ba2Hg1 | 98.0 | measured |
| O4.085Cu1Ba2Hg1 | ? | not retried; originally errored — UNCERTAIN, treat as unmeasurable |
| O4.096Cu1Ba2Hg1 | ? | UNCERTAIN — treat as unmeasurable |
| O4.097Cu1Ba2Hg1 | ? | UNCERTAIN — treat as unmeasurable |
| O4.101Cu1Ba2Hg1 | ? | UNCERTAIN — treat as unmeasurable |
| O4.103Cu1Ba2Hg1 | 92.5 | measured |
| O4.150Cu1Ba2Hg1 | 97.0 | measured |
| O4.160Cu1Ba2Hg1 | ? | UNMEASURABLE (unknown composition error, retry failed) |
| O4.162Cu1Ba2Hg1 | ? | UNCERTAIN — treat as unmeasurable |
| O4.180Cu1Ba2Hg1 | null | UNMEASURABLE (no sample, confirmed) |
| O4.200Cu1Ba2Hg1 | 94.2 | measured |
| O4.220Cu1Ba2Hg1 | 97.0 | measured |

### 5-element with Tl (1 member)
- O6Cu1Ba2Hg1Tl1 — Tc 0.0, measured (synthesized) — 5 elements, excluded from Step B (needs exactly 4)

## Step A: Peak
Measured Tc vs O (seed series): 92(6.080), 92.5(6.100), [6.130,6.140 unmeasured], 122(6.190), 127(6.210), 127(6.220), 123(6.240), 100(6.300), 98(6.310).
**T* = 127.0** at peak **O6.220Ca1Cu2Ba2Hg1** (6.210 also 127; pick 6.220 as it appears later in series, both maximal).

## Step B: threshold1 = 127 - 32 = 95.0
4-element Hg materials (exactly 4 elements = {O,Cu,Ba,Hg} series).
Measured Tc >= 95:
- O4.047: 95.0 >= 95 ✓
- O4.081: 98.0 >= 95 ✓
- O4.103: 92.5 < 95 ✗
- O4.150: 97.0 >= 95 ✓
- O4.200: 94.2 < 95 ✗
- O4.220: 97.0 >= 95 ✓
**Q1 (qualifying, measured with Tc>=95) = [O4.047, O4.081, O4.150, O4.220]**

## Step C: Unmeasurable materials (no sample / cannot measure)
Confirmed unmeasurable (no sample): O6.160, O6.270, O4.180
Unknown composition (persistent error → no sample): O4.066, O4.160
Uncertain (errored once, not retried due to budget): O4.085, O4.096, O4.097, O4.101, O4.162
Also unmeasured (could measure but didn't due to budget): O6.130, O6.140 — but these COULD be measured (sample exists), so NOT "unmeasurable" per definition. They're just unmeasured. Definition of unmeasurable = measure_tc returns "no sample exists". So exclude O6.130, O6.140 from predictions.

Truly unmeasurable (no sample exists):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem)
3. O4.180Cu1Ba2Hg1 (4 elem)
4. O4.066Cu1Ba2Hg1 (4 elem) — persistent error
5. O4.160Cu1Ba2Hg1 (4 elem) — persistent error
Plus uncertain (treat as unmeasurable since errored): O4.085, O4.096, O4.097, O4.101, O4.162

## Rule induction
4-element series measured Tc vs O: 95(4.047), 98(4.081), 92.5(4.103), 97(4.150), 94.2(4.200), 97(4.220).
Shape: rises 95→98 (4.047→4.081), dips 92.5 (4.103), back up 97 (4.150), ~94-97 (4.200-4.220).
Flat-ish high plateau ~92-98 across O4.05-4.22, all >= 90 (high). Min measured = 92.5 (still high).
Unmeasurable 4-element (O4.066, 4.085, 4.096, 4.097, 4.101, 4.160, 4.162, 4.180) all within O4.066-4.180 → within plateau → predict HIGH.
Seed series unmeasurable:
- O6.160: between 6.100(92.5) and 6.190(122). Rising part of dome. ~110-120 → HIGH.
- O6.270: between 6.240(123) and 6.300(100). ~112 → HIGH.
ALL unmeasurable predicted HIGH.

## Step D: Among predicted high, fewest elements (tie-break higher expected Tc).
5-element: O6.160, O6.270
4-element: O4.066, O4.085, O4.096, O4.097, O4.101, O4.160, O4.162, O4.180
Fewest = 4 elements. Among 4-element predicted high, pick highest expected Tc.
Measured 4-element peak ~98 at O4.081; 97 at 4.150/4.220. Unmeasurable 4-element closest to peak:
- O4.085 (near 4.081=98) → expected ~98
- O4.096, O4.097 (near 4.081-4.103) → expected ~95-98
- O4.160, O4.162 (near 4.150=97) → expected ~97
- O4.180 (near 4.200=94.2, 4.150=97) → expected ~96
- O4.066 (near 4.047=95) → expected ~96
Highest expected Tc among 4-element unmeasurable: O4.085 (~98, adjacent to measured 4.081=98).
**Final = O4.085Cu1Ba2Hg1** (4 elements, expected Tc ~98, highest among 4-element unmeasurable).

## Budget mechanics (CONFIRMED)
- Successful measure_tc: +1 budget. search_db: +1 budget.
- No-sample (null) responses and "unknown composition" errors: NO budget consumed (free).
- BUT: once budget_used == 26, ALL measure_tc calls return "instrument time exhausted" (even no-sample ones are blocked).
- Lesson: should have tested ALL uncertain materials (free) BEFORE exhausting budget on successful measurements.
Budget trail: 9 search (Hg pages) + 1 (O4.160 search) + 16 successful measurements = 26.
budget_used progression: searches 1-9 → 10(6.080)→11(6.220)→12(6.310)→13(6.190)→14(6.210)→15(6.240)→16(6.300)→17(4.047)→18(4.150)→19(4.220)→20(4.200)→21(re-search p4)→22(Tl)→23(search 4.160)→24(4.081)→25(4.103)→26(6.100). ✓
Free along the way: 6.160 no-sample, 6.270 no-sample, 4.180 no-sample, 4.066 err, 4.160 err.

## Unmeasurable classification (FINAL)
Strict definition (explicit "no sample exists" null response): O6.160, O6.270, O4.180 (3 confirmed).
"unknown composition" error (no Tc returned → treated as unmeasurable since no Tc obtainable):
  O4.066 (errored twice incl. dedicated retry → likely truly no sample)
  O4.160 (errored twice incl. dedicated retry → likely truly no sample)
  O4.085, O4.096, O4.097, O4.101, O4.162 (errored once; not retried — but no Tc obtained, treat as unmeasurable for predictions)
Note: O4.081 originally errored but succeeded on retry (98.0) → errors CAN be transient. So the single-error ones are uncertain. Including them in predictions per "ALL unmeasurable you discovered" instruction.

## FINAL ANSWER
- peak: O6.220Ca1Cu2Ba2Hg1, T*=127.0
- threshold1 = 95.0
- qualifying (Q1, measured 4-element with Tc>=95): O4.047, O4.081, O4.150, O4.220
- predictions: all 10 unmeasurable = high
- final (Step D): O4.085Cu1Ba2Hg1 (4 elements, expected Tc ~98 adjacent to measured peak O4.081=98)
- rule: series-specific Tc(O) inverted-parabola dome; single-factor rules fail.
