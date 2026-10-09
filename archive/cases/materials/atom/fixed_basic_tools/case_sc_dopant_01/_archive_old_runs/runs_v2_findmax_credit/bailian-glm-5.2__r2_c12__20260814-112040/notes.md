# Notes — find highest-Tc composition under a 12-credit budget

## Candidate structure
All 30 candidates are `O7Cu3Ba2Pr(x)R(1−x)`: a YBCO-7 (YBa₂Cu₃O₇) cuprate backbone
where the Y-site is split between **Pr** (praseodymium, fraction x) and a partner
rare earth **R ∈ {Er, Gd, Nd}** (fraction 1−x). The two fractions always sum to 1.

## Underlying physics (the deducible rule)
This is the classic **Pr-substituted YBCO** system. Pr replacing Y in YBCO is the
well-known "Pr anomaly": it **monotonically and strongly suppresses Tc** as Pr
content x rises (Tc → 0 around x ≈ 0.4–0.6 in real Pr-YBCO). The partner rare
earths Er/Gd/Nd each form superconducting RBCO with Tc ≈ 88–95 K on their own.

⇒ Dominant rule: **lower Pr fraction → higher Tc**. The global maximum should be
at the **lowest Pr concentration (x = 0.030)**. The open question is *which*
partner rare earth (Er / Gd / Nd) gives the highest Tc at x = 0.030.

## Pr concentrations present (per rare earth)
- Er: 0.030, 0.076, 0.150, 0.200, 0.300, 0.400, 0.500, 0.800, 0.900
- Gd: 0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900
- Nd: 0.030, 0.080, 0.150, 0.200, 0.300, 0.500, 0.600, 0.700, 0.800, 0.900

Min Pr = 0.030 for all three rare earths → the three candidates at x=0.030 are the
prime suspects for the global maximum.

## Measurement plan (12 credits)
1. **Round 1 (3 cr):** measure the x=0.030 trio (Er, Gd, Nd) → identifies the
   leading rare earth at min Pr and their relative ranking.
2. **Round 2 (3 cr):** measure the next-Pr point for each rare earth
   (0.076 Er, 0.078 Gd, 0.080 Nd) → confirms the downward Pr-suppression slope.
3. **Round 3 (6 cr):** characterize the leader's suppression curve at a few
   mid/high Pr points to confirm monotonic decrease (rules out a non-monotonic
   bump that could beat x=0.030), and cross-check the runner-up rare earth's
   slope to rule out a curve crossing. Then commit to the x=0.030 leader.

## Measurements

### Round 1 — x=0.030 trio (lowest Pr = prime suspects) — 3 cr
| composition | Pr x | R | Tc (K) |
|---|---|---|---|
| O7Cu3Ba2Pr0.030Er0.970 | 0.030 | Er | **93.9** ← leader |
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | Gd | 93.1 ← close runner-up |
| O7Cu3Ba2Pr0.030Nd0.970 | 0.030 | Nd | 85.1 ← well behind |

Pr-suppression holds at min Pr. Er leads, Gd near-tie, Nd far behind.

### Round 2 — next-Pr points (confirm downward slope) — 3 cr
| composition | Pr x | R | Tc (K) | Δ from x=0.030 |
|---|---|---|---|---|
| O7Cu3Ba2Pr0.076Er0.924 | 0.076 | Er | 91.1 | −2.8 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | Gd | 84.0 | −9.1 |
| O7Cu3Ba2Pr0.150Er0.850 | 0.150 | Er | 82.9 | −11.0 (vs 0.030) |

- Er curve: 93.9 → 91.1 → 82.9, monotonic decreasing.
- Gd curve: 93.1 → 84.0, monotonic decreasing, **steeper** than Er ⇒ the Er–Gd gap
  *widens* as Pr rises. Gd will never cross over Er.
- Nd: still only the 0.030 point (85.1).

## Round 3 — confirm monotonicity at mid/high Pr (6 cr) — 3 cr
| composition | Pr x | R | Tc (K) |
|---|---|---|---|
| O7Cu3Ba2Pr0.300Er0.700 | 0.300 | Er | 61.1 |
| O7Cu3Ba2Pr0.500Er0.500 | 0.500 | Er | 28.6 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | Gd | 39.6 |

Er keeps falling steeply (93.9→91.1→82.9→61.1→28.6). No bump. Gd @0.300 = 39.6,
far below Er's curve everywhere.

## Round 3 (cont.) — last 3 cr: Gd high, Nd curve — 3 cr
| composition | Pr x | R | Tc (K) |
|---|---|---|---|
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | Gd | 0.0 (non-superconducting) |
| O7Cu3Ba2Pr0.150Nd0.850 | 0.150 | Nd | 40.7 |
| O7Cu3Ba2Pr0.500Nd0.500 | 0.500 | Nd | 0.0 (non-superconducting) |

## Full curves (Tc vs Pr x)
- **Er:** 0.030→93.9, 0.076→91.1, 0.150→82.9, 0.300→61.1, 0.500→28.6  (strictly ↓)
- **Gd:** 0.030→93.1, 0.078→84.0, 0.300→39.6, 0.500→0.0  (strictly ↓, hits 0 by 0.5)
- **Nd:** 0.030→85.1, 0.150→40.7, 0.500→0.0  (strictly ↓, hits 0 by 0.5)

## Conclusion
Every rare earth's Tc **decreases strictly monotonically** with Pr fraction x — the
Pr-suppression rule holds with no non-monotonic bumps anywhere. The lowest-Pr point
(x = 0.030) is the maximum for each rare earth, and Er wins that head-to-head
(93.9 > Gd 93.1 > Nd 85.1).

Every *unmeasured* candidate sits at higher Pr than a point already measured on its
own monotonic-decreasing curve (e.g. Er@0.200/0.400/0.800/0.900 are all > 0.150 where
Er is already 82.9 and falling; Gd@0.200/0.600/0.700/0.800/0.900 likewise) ⇒ none
can exceed Er@0.030 = 93.9 K.

**Answer: O7Cu3Ba2Pr0.030Er0.970, Tc = 93.9 K.**

## Credit ledger
Start: 12. Used: 12. Remaining: 0.
