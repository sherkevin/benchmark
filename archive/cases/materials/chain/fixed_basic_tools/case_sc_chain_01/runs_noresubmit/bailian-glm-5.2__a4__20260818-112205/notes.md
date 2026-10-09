# Superconductor Tc study — notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (Hg-1212 family: Hg, Ba, Ca, Cu, O).
- "high" = Tc >= 90 K; "low" = Tc < 90 K.
- Budget: 18 measure_tc calls total. Step C/D need 0 (unmeasurable → "no sample").
- All 18 budget → Step A (10) + Step B (8 of 11).

## Step A — Hg-1212 doping series (O6.XXX Ca1Cu2Ba2Hg1), 10 members
| O | Tc | O | Tc |
|---|---|---|---|
| 6.080 | 92.0 | 6.220 | 127.0 |
| 6.100 | 92.5 | 6.240 | 123.0 |
| 6.130 | 102.0 | 6.300 | 100.0 |
| 6.140 | 105.0 | 6.310 | 98.0 |
| 6.190 | 122.0 | | |
| 6.210 | 127.0 | | |

→ **T\* = 127.0** K (plateau peak: O6.210 and O6.220 both = 127.0). Peak comp = O6.210Ca1Cu2Ba2Hg1.
→ **threshold1 = T\* − 32 = 95.0** K.

## Step B — family B (contains Hg AND exactly 4 elements) = Hg-1201 (O4.XXX Cu1Ba2Hg1), 11 members
threshold1 = 95.0. Measured 8 (budget forced skipping 3: O4.085, O4.097, O4.162).
| O | Tc | ≥95? |
|---|---|---|
| 4.047 | 95.0 | ✓ |
| 4.081 | 98.0 | ✓ |
| 4.085 | [skipped] ~97 (interp 4.081=98.0→4.096=95.6) ✓* |
| 4.096 | 95.6 | ✓ |
| 4.097 | [skipped] ~95.2 (interp 4.096=95.6→4.101=93.4) ✓* borderline |
| 4.101 | 93.4 | ✗ |
| 4.103 | 92.5 | ✗ |
| 4.150 | 97.0 | ✓ |
| 4.162 | [skipped] ~96.3 (interp 4.150=97.0→4.200=94.2) ✓* |
| 4.200 | 94.2 | ✗ |
| 4.220 | 97.0 | ✓ |

\* 3 members inferred by interpolation (not directly measured — budget exhausted).
Dome is noisy/non-monotonic (97.0→94.2→97.0 across 4.150/4.200/4.220), so interp is approximate;
O4.097 is the genuinely borderline case (~95.2, just above 95.0). All 3 lean qualifying → included in Q1.

**Q1 (Tc ≥ 95.0) = 8 members:** O4.047, O4.081, O4.085, O4.096, O4.097, O4.150, O4.162, O4.220 (all Cu1Ba2Hg1).
Non-qualifying: O4.101 (93.4), O4.103 (92.5), O4.200 (94.2).

## Step C — induce rule, predict unmeasurable.txt (no measurements possible)
Unmeasurables (7):
1. O6.160Ca1Cu2Ba2Hg1 — Hg-1212 series (between 6.140=105 & 6.190=122 → ~113K). HIGH.
2. O6.270Ca1Cu2Ba2Hg1 — Hg-1212 series (between 6.240=123 & 6.300=100 → ~110K). HIGH.
3. O4.180Cu1Ba2Hg1 — Hg-1201 series (between 4.150=97 & 4.200=94.2 → ~96K). HIGH.
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl-2212 (Ba2 Ca Cu2 Tl2 ~110K). HIGH.
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — (Tl,Pb)Sr2CaCu2O7, Sr-based 1212 (<90K). LOW.
6. O8Cu3Y1Ba1.900La0.100 — Y-123 (YBa2Cu3O7 ~92K, La-sub). HIGH.
7. O8Cu4Y1Ba1.870La0.130 — Y-124 (YBa2Cu4O8 ~80K). LOW.

Rule search. Single factors fail:
- "contains Hg": misses #4 (Tl-2212, high) & #6 (Y-123, high) which have no Hg.
- "has 5 elements": #3 has 4 elem (high); #5 has 6 elem (low) — not clean.
- "contains Ba": #7 has Ba but is LOW (Y-124). Not clean alone.

**Induced 2-factor rule (interaction):**
**HIGH iff [alkaline earth is Ba (not Sr)] AND [Cu stoichiometry ≤ 3] ; LOW otherwise.**
- Ba-presence excludes the Sr-based (Tl/Pb)-1212 (#5, low).
- Cu≤3 excludes the 4-Cu Y-124 phase (#7, low) while keeping Y-123 (3 Cu, high).
- All Hg-Ba cuprates (1,2,3) and Tl-Ba-2212 (4, Cu2.2) and Y-Ba-123 (6, Cu3) satisfy both → HIGH.

Validation vs measured data: all Hg-1212 (Ba, Cu2) and Hg-1201 (Ba, Cu1) → HIGH, consistent with all-measured-high. Clean separation of all 7 unmeasurables. Physically motivated (Ba-cuprates = high-Tc families; Sr-1212 & Y-124 are the known lower-Tc phases).

Predictions: 1=high, 2=high, 3=high, 4=high, 5=low, 6=high, 7=low.

## Step D — final pick (fewest elements among predicted-high, tie→higher expected Tc)
Predicted HIGH: 1(5 elem), 2(5), 3(4 elem), 4(5), 6(5).
Fewest elements = #3 `O4.180Cu1Ba2Hg1` (4 elements, Hg-1201, exp. ~96K). Unique minimum → **final = O4.180Cu1Ba2Hg1**.

## Budget audit
18/18 used: 10 (Step A) + 8 (Step B). 0 reserved for C/D (correct — unmeasurable). No leftover to resolve the 3 skipped Step-B members or to measure the 5 "other" measurables for rule validation.
