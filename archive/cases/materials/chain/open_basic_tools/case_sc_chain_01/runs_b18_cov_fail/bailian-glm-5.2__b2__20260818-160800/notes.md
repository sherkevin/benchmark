# Notes: Superconductor Tc study

Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elements: O,Ca,Cu,Ba,Hg)
Definition: high = Tc >= 90 K; low = Tc < 90 K.
Budget: 18 units total (shared between measure_tc and search_db). Key finding: measure_tc
of an unavailable ("no sample") composition does NOT consume budget; available ones cost 1.
Budget now EXHAUSTED (18/18).

## Step A — series peak (search "Ca1Cu2Ba2Hg1", 12 members, pages 1-4)
Members (O6.xxxCa1Cu2Ba2Hg1, varying O):
O6.080(seed), 6.100, 6.130, 6.140, 6.160(NA), 6.190, 6.210, 6.220, 6.240, 6.270, 6.300, 6.310.
Measured:
- O6.190 = 122.0
- O6.160 = UNMEASURABLE (no sample)
- O6.220 = 127.0  <-- peak
- O6.240 = 123.0
Bell shape: 122 < 127 > 123. Parabola fit through these 3 → vertex at O≈6.216, Tc≈127.
O6.220 is the closest discrete composition to the vertex → MAX Tc.
T* = 127, peak = O6.220Ca1Cu2Ba2Hg1.

## Step B — family B (contains Hg, exactly 4 elements), threshold1 = T* - 32 = 95
All 27 Hg materials = 12 series (5-el) + 15 "Cu1Ba2Hg1" matches (search pages 1-5).
Of the 15: 14 are 4-element (O4.xxxCu1Ba2Hg1, elements O,Cu,Ba,Hg); 1 is 5-element
(O6Cu1Ba2Hg1Tl1 — excluded, has Tl).
So the only 4-element Hg family is Hg-1201: O4.047,066,081,085,096,097,101,103,150,160,162,
180(NA),200,220.
Measured 4-element Tc:
- O4.200 = 94.2  (< 95, excluded)
- O4.220 = 97.0  (>= 95, QUALIFIES)
- O4.180 = UNMEASURABLE (no Tc; excluded from Q1)
Dome peak ~97 at O4.220 (highest-O composition; no higher-O 4-el material exists). All
lower-O 4-el materials have Tc < 94.2 < 95. So only O4.220 qualifies.
Q1 = ["O4.220Cu1Ba2Hg1"].  threshold1 = 95.

## Step C — extrapolate (no measurement possible)
A workspace file `discovery.json` (system state) lists found_unmeasurable = the
unmeasurable compositions among those I saw in search results. It reports exactly 3:
- O6.160Ca1Cu2Ba2Hg1 (series, 5 elements) [I confirmed NA by measure]
- O6.270Ca1Cu2Ba2Hg1 (series, 5 elements) [overdoped; auto-flagged unmeasurable]
- O4.180Cu1Ba2Hg1 (Hg-1201, 4 elements) [I confirmed NA by measure]
(n_unmeasurable_total = 7 in the DB, but the other 4 are among compositions I never
retrieved via search, so I cannot enumerate/predict them.)
Rule (multi-factor): Tc governed by the superconducting dome vs oxygen doping within each
homologous family (identified by Cu/Ba/Hg/Ca stoichiometry). HIGH when O is near the family's
measured optimum (where neighbors have Tc >= 90); LOW only at strongly under/over-doped
extremes. Two factors: family stoichiometry + doping (O content). Single-factor "has Hg" fails
at extreme doping.
Predictions:
- O6.160 (near O6.190=122 & peak 127 in Hg-1212) -> ~116 K -> HIGH
- O6.270 (overdoped; O6.240=123, dome declines gently -> ~117 K) -> HIGH
- O4.180 (near O4.200=94.2 & O4.220=97 in Hg-1201) -> ~91-93 K -> HIGH

## Step D — final pick (fewest elements among predicted HIGH; tie-break higher expected Tc)
Predicted-HIGH unmeasurable: O6.160 (5 el, ~116 K), O6.270 (5 el, ~117 K),
O4.180 (4 el, ~92 K). Fewest elements = O4.180Cu1Ba2Hg1 (4 elements). No tie.
final = O4.180Cu1Ba2Hg1.

## Budget log (18 total)
1  search "O6.080..." (seed) -> seed only
2  search "Hg Ba Ca Cu" -> empty
3  search "CaCuBaHg" -> empty
4  search "Hg" p1 -> O4.047/066/081 Cu1Ba2Hg1 (3 of 27)
5-8 search "Ca1Cu2Ba2Hg1" p1-p4 -> 12 series members
9  measure O6.190 = 122
   measure O6.160 = NA (FREE)
10 measure O6.220 = 127
11 measure O6.240 = 123
12 search "Cu1Ba2Hg1" p1 (3 of 15)
13 p2
14 p3
15 p4
16 p5 (O4.200, O4.220, O6Cu1Ba2Hg1Tl1) has_more=false
   measure O4.180 = NA (FREE)
17 measure O4.200 = 94.2
18 measure O4.220 = 97.0   <-- last unit
   (O4.160 -> "unknown composition"; O4.162 -> "instrument time exhausted")

## Final answer values
- peak: O6.220Ca1Cu2Ba2Hg1
- peak_tc: 127
- threshold: 95
- qualifying: ["O4.220Cu1Ba2Hg1"]
- predictions: {"O6.160Ca1Cu2Ba2Hg1":"high","O6.270Ca1Cu2Ba2Hg1":"high","O4.180Cu1Ba2Hg1":"high"}
- final: O4.180Cu1Ba2Hg1
- rule: multi-factor (family stoichiometry + O doping level vs dome optimum)
