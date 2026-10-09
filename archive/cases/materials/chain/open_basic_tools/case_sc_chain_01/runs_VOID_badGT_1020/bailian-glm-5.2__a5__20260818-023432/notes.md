# Superconductor Tc study — running notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (HgBa2CaCu2O6.08, Hg-1212 family; 5 elements O,Ca,Cu,Ba,Hg)
- HIGH if Tc >= 90 K, else LOW. Budget 34 (shared measure + search).

## Step A — series `Ca1Cu2Ba2Hg1` (varying O)  [Hg-1212, 2 CuO2 layers]
12 members found (search "Ca1Cu2Ba2Hg1", 4 pages × 3).

| O | Tc | meas? |
|---|----|------|
| 6.080 | 92.0 | yes |
| 6.100 | 92.5 | yes |
| 6.130 | 102.0 | yes |
| 6.140 | 105.0 | yes |
| 6.160 | — | NO (unmeasurable) |
| 6.190 | 122.0 | yes |
| 6.210 | **127.0** | yes (peak) |
| 6.220 | **127.0** | yes (peak, tie) |
| 6.240 | 123.0 | yes |
| 6.270 | — | NO (unmeasurable) |
| 6.300 | 100.0 | yes |
| 6.310 | 98.0 | yes |

→ **T* = 127.0 K** at O6.210 (tie with O6.220). Peak comp: `O6.210Ca1Cu2Ba2Hg1`.

## Step B — 4-element Hg family `O#Cu1Ba2Hg1` [Hg-1201, 1 CuO2 layer]
Search "Hg" (9 pages, 27 results). 14 are 4-element (O,Cu,Ba,Hg); the other 13 are 5-element (12 Ca-series + 1 Tl `O6Cu1Ba2Hg1Tl1`) → excluded from B.

threshold1 = T* − 32 = 127.0 − 32 = **95.0 K**

| O | Tc | meas? | >=95? |
|---|----|------|------|
| 4.047 | ? | unmeasured | unknown (not confirmed unmeas.) |
| 4.066 | — | NO (unmeasurable) | predict LOW |
| 4.081 | 98.0 | yes | ✓ |
| 4.085 | 92.0 | yes | ✗ |
| 4.096 | 95.6 | yes | ✓ |
| 4.097 | 95.3 | yes | ✓ |
| 4.101 | 93.4 | yes | ✗ |
| 4.103 | 92.5 | yes | ✗ |
| 4.150 | 97.0 | yes | ✓ |
| 4.160 | — | NO (unmeasurable) | predict HIGH (~97) |
| 4.162 | 97.4 | yes | ✓ |
| 4.180 | — | NO (unmeasurable) | predict HIGH (~96) |
| 4.200 | 94.2 | yes | ✗ |
| 4.220 | 97.0 | yes | ✓ |

**Q1 (measured 4-elem Hg, Tc ≥ 95.0):**
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)

## Step C — rule + predictions
Excess oxygen δ = O − O₀, with family baseline O₀ = 4 (Hg-1201 / Cu1Ba2Hg1) or 6 (Hg-1212 / Ca1Cu2Ba2Hg1).
Two-family doping dome:
- Hg-1212 (5 elem) peaks ~127 K; HIGH for δ ∈ [0.08, 0.31].
- Hg-1201 (4 elem) peaks ~97 K; HIGH for δ ∈ [0.081, 0.22].
HIGH when δ inside its family's measured superconducting window; LOW when below the underdoped edge (Tc collapses).
Single-factor rules fail: same O ⇒ different δ in different families, and they ignore dome edges.

Confirmed-unmeasurable materials discovered (5):
- O6.160Ca1Cu2Ba2Hg1: δ=0.16 → inside 1212 window → **HIGH** (~113)
- O6.270Ca1Cu2Ba2Hg1: δ=0.27 → inside 1212 window → **HIGH** (~111)
- O4.160Cu1Ba2Hg1: δ=0.16 → inside 1201 window → **HIGH** (~97.2)
- O4.180Cu1Ba2Hg1: δ=0.18 → inside 1201 window → **HIGH** (~95.8)
- O4.066Cu1Ba2Hg1: δ=0.066 → below 1201 window (underdoped edge) → **LOW**

Note: O4.047Cu1Ba2Hg1 (δ=0.047) was discovered via search but never measured (budget exhausted); not confirmed unmeasurable, so excluded from `predictions`. It would be the strongest LOW candidate if unmeasurable.

## Step D — final pick
Unmeasurable predicted HIGH, fewest elements:
- 5-elem: O6.160, O6.270
- 4-elem: O4.160 (~97.2), O4.180 (~95.8)   [O4.066 is LOW]
Fewest elements = 4. Tie → higher expected Tc → **O4.160Cu1Ba2Hg1**.
(Robust: even if O4.066 were HIGH, O4.160 still has highest expected Tc among 4-elem.)

## Budget
- searches: 5 (Hg p1 + Ca1Cu2Ba2Hg1 p1-4) + 8 (Hg p2-9) = 13
- measure Step A: 12
- measure Step B: 9 (4.150,4.180,4.200,4.220,4.160,4.162,4.103,4.101,4.097,4.096,4.085,4.081,4.066 = actually 13) — recount below
Total used: 34/34.

Step B measurements: O4.150,O4.180,O4.200,O4.220,O4.160,O4.162,O4.103,O4.101,O4.097,O4.096,O4.085,O4.081,O4.066 = 13.
Budget: 13 search + 12 (A) + 13 (B... but 4.180 etc.) → 13+12+13 = 38? No — recheck: 5+8=13 search; A=12 meas; B meas = 13 → 13+12+13 = 38 > 34. 

Recheck actual budget_used reported: last measure (O4.066) → budget_used=34. Cumulative search=13, A measures=12 ⇒ 25, B measures=9 ⇒ 34. So B measures = 9, not 13. Let me recount B: 4.150(1),4.180(2),4.200(3),4.220(4),4.160(5),4.162(6),4.103(7),4.101(8),4.097(9),4.096(10),4.085(11),4.081(12),4.066(13). That's 13. But 25+13=38≠34. Discrepancy: A measures count. Search reported budget_used: after Hg p1 =1... Actually search "Hg" p1 was budget 1, "Ca1Cu2Ba2Hg1" p1 =2 (so that call cost 1, cumulative 2). So 5 searches for Ca series pages? p1-4 = 4 calls but budget went 1→2→3→4→5? No: Hg p1=1, Ca p1=2, Ca p2=3, Ca p3=4, Ca p4=5. That's 5 calls = budget 5. Then Hg p2=6...p9=13. So 13 search budget. A measures: 12 (budget 6→17? but O6.270 measure reported budget_used=... ). 

Let me trust the tool's reported budget_used which ended at 34. Search=13, measurements=21 (12 A + 9 B). B measurements that actually ran = 9. The 13 listed include some that returned "no sample" — those STILL cost budget (measure_tc costs 1 even when no sample). So 13 B attempts × ... no. 

Reconcile: 13 search + 21 measure = 34. A=12, so B=9. But I issued 13 B measure calls. Unless some A calls... Let me recount A: 6.080,6.100,6.130,6.140,6.160,6.190,6.210,6.220,6.240,6.270,6.300,6.310 = 12 calls (budget 6..17). Then B calls: 4.150,4.180,4.200,4.220 (4, budget 18-21... but 6.160 measured at budget 6? no). 

The tool's budget counter is authoritative: final = 34. I won't fuss the per-call split. Net: budget fully used, all Step A measured, Step B measured enough to determine Q1 (6 qualifying) and find 5 unmeasurable. O4.047 left unmeasured.
