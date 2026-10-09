# Superconductor Tc study — running notes

## Seed & definitions
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- HIGH = Tc >= 90 K; LOW = Tc < 90 K
- Budget: 34 measure_tc calls (used 25; 4 "no sample" calls did not consume budget)

## Step A — series peak (Hg-1212 family: O + Ca1Cu2Ba2Hg1, 5 elements)
| O | Tc | note |
|---|----|------|
| 6.080 | 92.0 | HIGH |
| 6.100 | 92.5 | HIGH |
| 6.130 | 102.0 | HIGH |
| 6.140 | 105.0 | HIGH |
| 6.160 | — | NO SAMPLE (also in unmeasurable) |
| 6.190 | 122.0 | HIGH |
| 6.210 | **127.0** | HIGH ← peak (tied) |
| 6.220 | **127.0** | HIGH ← peak (tied) |
| 6.240 | 123.0 | HIGH |
| 6.270 | — | NO SAMPLE (also in unmeasurable) |
| 6.300 | 100.0 | HIGH |
| 6.310 | 98.0 | HIGH |

Dome peaks at O6.210/6.220 (both 127.0). **T* = 127.0**. Peak composition (first occurrence of max) = `O6.210Ca1Cu2Ba2Hg1`.
**threshold1 = T* − 32 = 95.0 K.**

## Step B — filter family B (contains Hg AND exactly 4 elements → B-family = O4.xxxCu1Ba2Hg1)
Elements = {O, Cu, Ba, Hg} = 4 ✓, contains Hg ✓. (Series excluded: 5 elements. O6Cu1Ba2Hg1Tl1 excluded: 5 elements.)
| O | Tc | ≥95? |
|---|----|------|
| 4.047 | — | NO SAMPLE |
| 4.066 | — | (not in measurable.txt; unmeasurable) |
| 4.081 | 98.0 | ✓ Q1 |
| 4.085 | 92.0 | ✗ |
| 4.096 | 95.6 | ✓ Q1 |
| 4.097 | 95.3 | ✓ Q1 |
| 4.101 | 93.4 | ✗ |
| 4.103 | 92.5 | ✗ |
| 4.150 | 97.0 | ✓ Q1 |
| 4.160 | — | (unmeasurable) |
| 4.162 | 97.4 | ✓ Q1 |
| 4.180 | — | NO SAMPLE |
| 4.200 | 94.2 | ✗ |
| 4.220 | 97.0 | ✓ Q1 |

**Q1** (measured Tc ≥ 95.0, Hg + 4 elements) = {O4.081, O4.162, O4.150, O4.220, O4.096, O4.097}. (Unmeasurable members O4.047/066/160/180 have no measured Tc → cannot be cited; handled in Step C.)

## Step C — auxiliary measurements to induce the rule (5 "other" materials)
| Composition | Elements (n) | Hg? Tl? Ca/Y? | Tc | class |
|---|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 5 (O,Cu,Ba,Hg,Tl) | Hg+Tl, no Ca | **0.0** | LOW |
| O10Ca2Cu3Ba2Tl2 | 5 (O,Ca,Cu,Ba,Tl) | Tl2, Ca | **118.97** | HIGH |
| O6Cu1Cd0.250Ba2Tl1.750 | 5 (O,Cu,Cd,Ba,Tl) | Tl+Cd, no Ca | **88.0** | LOW |
| O7Cu3Ba2La1 | 4 (O,Cu,Ba,La) | no Hg/Tl, La on Y-site | **89.6** | LOW |
| O7Cu3Y0.200Ba2La0.800 | 5 (O,Cu,Y,Ba,La) | no Hg/Tl, La on Y-site | **81.0** | LOW |

### Induced rule (multi-factor interaction)
HIGH requires a cuprate whose charge-reservoir + spacer chemistry match a high-Tc family at near-optimal oxygen:
- **(i) clean Hg-only reservoir** — Hg-1201 (HgBa2CuO4+x, measured HIGH across O≈4.08–4.22) and Hg-1212 (HgBa2CaCu2O6+x, measured HIGH across O≈6.08–6.31); OR
- **(ii) double-Tl reservoir with Ca spacer** — Tl-22xx family (Tl2Ba2Can−1CunO…, e.g. O10Ca2Cu3Ba2Tl2 → 119 K).

LOW when any of:
- **mixed/disordered reservoir** (Hg+Tl → 0 K; Tl+Cd → 88 K; Tl+Pb single-cation) — kills superconductivity in the n=1 2201-type;
- **La on the small rare-earth (Y) site** — La-123 type, destabilizes structure (81–90 K);
- **oxygen outside the high-Tc window** (under- or over-doped: below O4.081 in Hg-1201, O8 over-oxygenation of 123);
- **intrinsically low-Tc phase** (Y-124 / Cu4 type ~80 K, or no reservoir cation at all).

Single-factor checks that FAIL (confirming multi-factor need): "contains Hg" fails on O6Cu1Ba2Hg1Tl1 (Hg but 0 K); "has Ca" fails on B-family (HIGH, no Ca); "has 4 elements" fails on O7Cu3Ba2La1 (4 elem, LOW); "contains Tl" fails on O6Cu1Cd0.25Ba2Tl1.75 (Tl, LOW).

### Predictions for the 10 unmeasurable materials
1. O6.160Ca1Cu2Ba2Hg1 → Hg-1212, O∈[6.08,6.31] window → **HIGH** (~112, interp 105↔122)
2. O6.270Ca1Cu2Ba2Hg1 → Hg-1212, O∈window → **HIGH** (~115, interp 123↔100)
3. O4.180Cu1Ba2Hg1 → Hg-1201, O∈[4.08,4.22] window → **HIGH** (~96, interp 97.4↔94.2)
4. O4.047Cu1Ba2Hg1 → Hg-1201, O BELOW window (underdoped) → **LOW** (~80)
5. O4.066Cu1Ba2Hg1 → Hg-1201, O below window (underdoped) → **LOW** (~85)
6. O4.160Cu1Ba2Hg1 → Hg-1201, O∈window → **HIGH** (~97.3, interp 97.0↔97.4)
7. O8Ca0.850Cu2.200Ba2Tl1.800 → Tl1.8≈Tl2 + Ca (Tl-22xx, n≈2) → **HIGH** (~100–108)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 → mixed Tl+Pb single reservoir + Sr → **LOW** (~60–80)
9. O8Cu3Y1Ba1.900La0.100 → no Hg/Tl reservoir; O8 over-doped + Ba-site La → **LOW** (~85–88)
10. O8Cu4Y1Ba1.870La0.130 → Cu4 Y-124-type (intrinsically ~80 K) + La → **LOW**

## Step D — final pick
Predicted HIGH unmeasurables: #1, #2, #3, #6, #7.
Element counts: #1,#2,#7 = 5 elements; #3 (O4.180) = 4 elements; #6 (O4.160) = 4 elements.
Fewest elements = 4 → tie between O4.180 (~96 K) and O4.160 (~97.3 K).
Tie-break higher expected Tc → **O4.160Cu1Ba2Hg1** (~97.3 > ~96).

## Budget
25 measure_tc calls used (all materials with samples); 9 unused. No further measurable materials remain; unmeasurable ones return "no sample" without consuming budget.
