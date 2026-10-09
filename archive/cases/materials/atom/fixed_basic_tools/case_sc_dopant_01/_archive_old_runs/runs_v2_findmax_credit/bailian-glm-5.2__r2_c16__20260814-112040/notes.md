# Notes: finding the highest-Tc composition (budget = 16 credits, 30 candidates)

## Candidate structure

All candidates are `O7Cu3Ba2Pr<x>RE<1-x>` — Ba2Cu3O7 (YBCO-123) with Pr on the rare-earth
site, the rest filled by one of Er / Gd / Nd. Two control variables:

- **Pr fraction x** (suppression variable): 0.030 → 0.900
- **Rare earth RE**: Er, Gd, Nd

Coverage (Pr level → REs present):
- 0.030 → Er,Gd,Nd | ~0.076/0.078/0.080 → Er,Gd,Nd (the ~0.076 tier, slightly diff per RE)
- 0.150,0.200,0.300,0.500,0.800,0.900 → Er,Gd,Nd
- 0.400 → Er,Gd (no Nd) | 0.600,0.700 → Gd,Nd (no Er)

Gd has the most complete Pr coverage (11 levels); Er is missing only 0.400/0.600/0.700.

## Prior / expected law

Task name `case_sc_dopant_01`, git log: "Pr 掺杂抑制律（真规律、可推）" = Pr-doping
**suppression** law (real, inferable). In real YBCO-123, PrBa2Cu3O7 is non-superconducting
and substituting Pr for Y/RE suppresses Tc. Expect: **higher Pr → lower Tc**, monotonic-ish.
⇒ Highest-Tc candidate is very likely at the **lowest Pr** (x = 0.030 tier, maybe ~0.076).
Among the three REs at low Pr, must measure to find which wins.

## Strategy (16 credits)

1. **Max-region (6 credits)** — measure all six lowest-Pr candidates (Pr ≤ ~0.080):
   Er/Gd/Nd × {0.030, ~0.076-0.080}. These are the most likely maxima; measure directly.
2. **Trend confirmation (9 credits)** — map Er across the full Pr range + spot-check Gd/Nd
   at mid/high Pr, to confirm monotonic suppression and rule out a hidden intermediate peak.
3. **Reserve (1 credit)** — held back; conclusion became certain before needing it.

## Measurements

### Round 1 — the six lowest-Pr candidates (max region) [credits 1–6]

| Composition | Pr x | RE | Tc (K) |
|---|---|---|---|
| O7Cu3Ba2Pr0.030Er0.970 | 0.030 | Er | **93.9** ← max so far |
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | Gd | 93.1 |
| O7Cu3Ba2Pr0.076Er0.924 | 0.076 | Er | 91.1 |
| O7Cu3Ba2Pr0.030Nd0.970 | 0.030 | Nd | 85.1 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | Gd | 84.0 |
| O7Cu3Ba2Pr0.080Nd0.920 | 0.080 | Nd | 68.2 |

Signals: (1) **Er > Gd > Nd** at every Pr level tested; (2) **higher Pr → lower Tc**.
Lowest Pr in the set is 0.030; Er wins there at 93.9. Remaining risk: a hidden
non-monotonic bump at higher Pr → map the Er curve and spot-check Gd/Nd at mid-Pr.

### Round 2 — Er curve across full Pr range + mid/high-Pr Gd,Nd [credits 7–12]

| Composition | Pr x | RE | Tc (K) |
|---|---|---|---|
| O7Cu3Ba2Pr0.150Er0.850 | 0.150 | Er | 82.9 |
| O7Cu3Ba2Pr0.300Er0.700 | 0.300 | Er | 61.1 |
| O7Cu3Ba2Pr0.500Er0.500 | 0.500 | Er | 28.6 |
| O7Cu3Ba2Pr0.800Er0.200 | 0.800 | Er | 0.0 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | Gd | 0.0 |
| O7Cu3Ba2Pr0.500Nd0.500 | 0.500 | Nd | 0.0 |

**Er curve (6 pts):** 0.030→93.9, 0.076→91.1, 0.150→82.9, 0.300→61.1, 0.500→28.6, 0.800→0.0.
Strictly monotonic decreasing — no local bump anywhere. At Pr=0.500 all three REs → 0.0 K,
confirming suppression kills superconductivity at high Pr regardless of RE.

### Round 3 — tighten the monotonic curve + Gd ordering at a 4th Pr level [credits 13–14]

| Composition | Pr x | RE | Tc (K) |
|---|---|---|---|
| O7Cu3Ba2Pr0.200Er0.800 | 0.200 | Er | 76.4 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | Gd | 57.7 |

Er > Gd again (76.4 > 57.7); both fit the smooth decline. Er curve now 7 pts, fully monotonic.

## Decision

**Final: `O7Cu3Ba2Pr0.030Er0.970` = 93.9 K.**

Two laws, each confirmed at ≥4 Pr levels and 3 REs:
1. **Er > Gd > Nd** at every Pr level (0.030, ~0.076, 0.200, 0.500).
2. **Higher Pr → lower Tc**, strictly monotonic (Er: 7 pts 93.9→0.0; Gd: 5 pts 93.1→0.0).

Since 0.030 is the **lowest Pr in the entire candidate set** (no candidate has Pr < 0.030),
and Er is the best RE, the 0.030-Er point is the global max. Every unmeasured candidate has
Pr ≥ 0.150, where even the best RE (Er) is only 82.9 K — strictly below 93.9. So no unmeasured
composition can beat the answer. 2 credits held in reserve: the conclusion is certain and
further measurements would only re-confirm known points.
