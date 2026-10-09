# Superconductor Tc study — running notes

## Task summary
- Seed: `O6.080Ca1Cu2Ba2Hg1` (Hg-1212 family; elements {O,Ca,Cu,Ba,Hg}).
- High = Tc >= 90 K, else low.
- Budget: 18 measure_tc calls.
- Step A: max Tc of the seed's homologous O-doping series → T*.
- Step B: threshold1 = T* - 32; Q1 = all Hg-containing materials with exactly 4 elements whose Tc >= threshold1.
- Step C: induce multi-factor rule from measurable materials; predict high/low for every unmeasurable material.
- Step D: among predicted-high unmeasurable, pick fewest elements (tie → higher expected Tc).

## Measurable materials (data/measurable.txt)
O6.xxx Ca1Cu2Ba2Hg1 series (5 elements: O,Ca,Cu,Ba,Hg) — lines 1-10:
- O6.080, O6.100, O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310

O4.xxx Cu1Ba2Hg1 series (4 elements: O,Cu,Ba,Hg) — lines 11-21 (Hg + exactly 4 elements → Step B candidates):
- O4.047, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.162, O4.200, O4.220

Other (non-series, diverse families for Step C calibration):
- O6Cu1Ba2Hg1Tl1   (Hg,Tl-1201-ish, 5 el)
- O10Ca2Cu3Ba2Tl2  (Tl-2223-ish)
- O6Cu1Cd0.250Ba2Tl1.750 (Tl-1201 w/ Cd)
- O7Cu3Ba2La1      (La-123-ish)
- O7Cu3Y0.200Ba2La0.800 (Y/La-123)

## Unmeasurable materials (data/unmeasurable.txt) — Step C predictions needed
1. O6.160Ca1Cu2Ba2Hg1  — Hg-1212, O6.160 (interpolate in O6 series; between 6.140 & 6.190)
2. O6.270Ca1Cu2Ba2Hg1  — Hg-1212, O6.270 (between 6.240 & 6.300)
3. O4.180Cu1Ba2Hg1     — Hg-1201, O4.180 (interpolate in O4 series; between 4.162 & 4.200)
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl-1212 w/ Ca (2 CuO2)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  — (Tl,Pb)-1212 w/ Sr (2 CuO2)
6. O8Cu3Y1Ba1.900La0.100  — Y-123 w/ La (O8 full)
7. O8Cu4Y1Ba1.870La0.130  — Y-124 w/ La (O8)

## Budget plan (18 calls)
- Step A: measure all 10 O6-series → T* + calibration to interpolate O6.160/O6.270.
- Remaining 8: O4-series peak region → Q1 + calibration to interpolate O4.180.
- Non-Hg unmeasurable (4-7): predict via induced multi-factor rule (no direct calibration calls).

## Step A — measured O6.0xx Ca1Cu2Ba2Hg1 series (10 calls, budget 1-10)
| O6 | Tc(K) |
|----|-------|
| 6.080 | 92.0 |
| 6.100 | 92.5 |
| 6.130 | 102.0 |
| 6.140 | 105.0 |
| 6.190 | 122.0 |
| 6.210 | **127.0** ← MAX (tie w/ 6.220) |
| 6.220 | **127.0** ← MAX (tie) |
| 6.240 | 123.0 |
| 6.300 | 100.0 |
| 6.310 | 98.0 |

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1, first to reach max; O6.220 ties).
Dome: rising 6.08→6.21, plateau/peak 6.21-6.22 (~127), falling 6.22→6.31.
threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**.

Interpolation anchors for Step C unmeasurable O6 members:
- O6.160 between 6.140(105) & 6.190(122): ~111.8 K → high.
- O6.270 between 6.240(123) & 6.300(100): ~111.5 K → high.

## Step B — O4.0xx Cu1Ba2Hg1 series (4 elements {O,Cu,Ba,Hg}, contains Hg ✓)
threshold1 = 95.0 K. 11 candidates total in measurable.txt; measured 8 (budget 11-18):
| O4 | Tc(K) | >=95? |
|----|-------|-------|
| 4.047 | 95.0 | ✓ Q1 |
| 4.097 | 95.3 | ✓ Q1 |
| 4.101 | 93.4 | ✗ |
| 4.103 | 92.5 | ✗ |
| 4.150 | 97.0 | ✓ Q1 |
| 4.162 | 97.4 (peak) | ✓ Q1 |
| 4.200 | 94.2 | ✗ |
| 4.220 | 97.0 | ✓ Q1 |

Dome: flat ~95 plateau 4.047-4.097, dip 4.101-4.103 (~92.5-93.4), peak 4.150-4.162 (~97), dip 4.200 (94.2), 4.220 back to 97. Non-monotonic/noisy in overdoped region.
Hg-1201 peak ≈ 97.4 K (confirms n=1 Hg-1201 family is HIGH, ceiling ~97 — matches real HgBa2CuO4 ~95-97 K).

**Q1 (measured qualifiers, Hg + exactly 4 elements, Tc >= 95.0):**
1. O4.047Cu1Ba2Hg1 (95.0)
2. O4.097Cu1Ba2Hg1 (95.3)
3. O4.150Cu1Ba2Hg1 (97.0)
4. O4.162Cu1Ba2Hg1 (97.4)
5. O4.220Cu1Ba2Hg1 (97.0)

Unmeasured O4 (in measurable.txt, budget exhausted): O4.081, O4.085, O4.096 — sit in flat ~95.0-95.3 cluster, predicted ~95.1-95.3 (borderline qualify) but NOT measured → excluded from Q1 per "never guess a measured number". Noted as gap.

## Step C — induced multi-factor rule
Calibration available (measured):
- Hg-1212 (Ca1Cu2Ba2Hg1, n=2 CuO2): dome peak 127 K (O6.21-6.22). Family ceiling >= 90 → high; high window ≈ O6.08-6.31 (all measured points >= 92).
- Hg-1201 (Cu1Ba2Hg1, n=1 CuO2): dome peak 97.4 K (O4.162). Family ceiling >= 90 → high; high window spans O4.04-4.22 (all measured >= 92.5).

Domain knowledge for non-Hg families (no budget left to measure them):
- Tl-2212 (Tl2Ba2CaCu2O8, m=2 n=2): ~108 K → high.
- (Tl,Pb)-Sr1212 ((Tl0.5Pb0.5)Sr2CaCu2O7, n=2, Sr not Ba, no Y on Ca): ~80 K → low.
- Y-123 (YBa2Cu3O7, n=2): ~92 K optimal; overdoped (O8 + La on Ba) drops below 90 → low.
- Y-124 (YBa2Cu4O8, n=2 doubled): ~80 K → low.

**Rule (multi-factor interaction):**
Tc is governed by the INTERACTION of (i) cuprate family × number of CuO2 layers n (Tc rises with n: Hg-1201 n=1 ~97 K, Hg-1212 n=2 ~127 K; Tl-2212 n=2 ~108 K; Y-123 n=2 ~92 K; Y-124 ~80 K; (Tl,Pb)-Sr1212 ~80 K) AND (ii) whether O/hole doping is in the family's optimal window (over-/under-doping suppresses Tc). Material is HIGH (>=90) iff family×n ceiling >= 90 AND O/dopant stoichiometry lands in the high-Tc window. Single factors fail: "contains Hg" misses Tl-2212 (high, no Hg); "5 elements" mis-labels Y-123/Y-124 (5 elements but low); "n=2" alone mis-labels (Tl,Pb)-Sr1212/Y-124 (n=2 but low).

## Step C — predictions for unmeasurable.txt
1. O6.160Ca1Cu2Ba2Hg1 — Hg-1212, O6.160 between 6.140(105)&6.190(122): ~112 K → **high**.
2. O6.270Ca1Cu2Ba2Hg1 — Hg-1212, O6.270 between 6.240(123)&6.300(100): ~111 K → **high**.
3. O4.180Cu1Ba2Hg1 — Hg-1201, O4.180 between 6.162(97.4)&6.200(94.2): ~95.9 K, all neighbors >>90 → **high**.
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl≈2 → Tl-2212 (m=2,n=2), O8 stoichiometric (in-window), Tl1.8 deficiency → optimal hole doping → ~100-108 K → **high**. (5 elements {O,Ca,Cu,Ba,Tl}.)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — (Tl0.5Pb0.5)Sr2CaCu2O7 = (Tl,Pb)-Sr1212, n=2 but ceiling ~80 K (Sr version, no Y on Ca site) → **low**. (6 elements.)
6. O8Cu3Y1Ba1.900La0.100 — Y-123 (Cu3), O8 + La0.1 on Ba site → overdoped + Ba-site disorder, suppresses below 92 → **low**. (5 elements.)
7. O8Cu4Y1Ba1.870La0.130 — Y-124 (Cu4, O8), ceiling ~80 K → **low**. (5 elements.)

## Step D — final pick
Predicted HIGH: {1, 2, 3, 4}. Element counts: 1=5, 2=5, 3=4, 4=5.
Fewest elements = 4 → **O4.180Cu1Ba2Hg1** (uniquely; no tie to break).
Robust: even at O4.180's lowest plausible neighbor value (O4.200=94.2), it stays >=90 (high), so high prediction is safe and 4-element count makes it the unique fewest.
