# Superconductor Tc study — notes

Seed: `O6.080Ca1Cu2Ba2Hg1`. High = Tc >= 90 K. Budget = 18 measure_tc calls.

## Step A — series peak (O6 doping series: O,Ca,Cu,Ba,Hg; 10 members)
| composition | Tc |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** (peak) |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** (peak) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

- T* = 127.0 (dome peaks at O6.210/O6.220). All 10 members are HIGH (>=90).
- This is the Hg-1212 family (Ca1Cu2Ba2Hg ≈ HgBa2CaCu2 with 2 CuO2 layers). Optimal Tc ~127 K matches literature Hg-1212.
- threshold1 = T* - 32 = 127 - 32 = **95**.

## Step B — Q1: 4-element Hg materials with Tc >= 95
Candidates (contain Hg, exactly 4 elements = O,Cu,Ba,Hg): the O4.xxxCu1Ba2Hg1 series (Hg-1201, 1 CuO2 layer), 11 members:
O4.047, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.162, O4.200, O4.220.
(O6 series has 5 elements; O6Cu1Ba2Hg1Tl1 has 5; others lack Hg.)
Hg-1201 typical optimal Tc ~94-97 K — right at the 95 threshold. Must measure.

Budget after Step A: 8 left. Measuring O4 subset strategically + Tl/YBaLa for Step C rule.

## Step B results (O4 / Hg-1201 series, 4 elements w/ Hg; threshold1=95)
| composition | Tc | >=95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO (just below) |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
Unmeasured O4 members: O4.047, O4.085, O4.097, O4.101, O4.103 — NOT cited in Q1 (not measured).
**Q1 = {O4.081, O4.096, O4.150, O4.162, O4.220}Cu1Ba2Hg1** (all >=95).

## Step C — family reference data (measured anchors)
- **Hg-1212** (O6 Ca1Cu2Ba2Hg1, n=2): peak 127 K — HIGH family.
- **Hg-1201** (O4 Cu1Ba2Hg1, n=1): 94.2–98.0 K — HIGH family (near threshold).
- **Tl-1223** (`O10Ca2Cu3Ba2Tl2`, n=3, Ca2/Cu3/Ba2/Tl2): **118.97 K** — HIGH family.
- **YBCO-123 (low-Y/La-sub)** (`O7Cu3Y0.200Ba2La0.800`, n=3, Y0.2/La0.8): **81 K** — LOW.

## Step C — induced RULE (multi-factor interaction)
Anchors measured: Hg-1201/1212 HIGH (94–127), Tl-2223 (double-Tl, Ba-based, n=3) = 118.97 HIGH,
La-dominant YBCO-123 (Y0.2/La0.8) = 81 LOW.
A material is HIGH iff it is in a high-Tc cuprate family AND has the high-Tc structural/occupancy
variant. Concretely HIGH requires BOTH:
 (i) canonical Ba-based charge reservoir at near-full occupancy — Hg-Ba-Ca-Cu (1201 n=1 /
     1212 n=2) with near-optimal O, OR double-Tl-layer Tl-Ba-Ca-Cu 22xx, OR Y-predominant
     YBCO-123 (Y dominant over La on earth site, Cu3/123-structure); AND
 (ii) NOT a low-Tc structural variant.
LOW results from failing ANY factor: La-dominant earth site (measured Y0.2/La0.8 = 81K),
Sr-substituted / single-Tl-layer 1212 charge reservoir (Tl0.5/Pb0.5/Sr, ~80K), or YBCO-124
structure (Cu4, ~80K).
Why multi-factor: "contains Tl" fails on #5 (Tl but Sr single-layer -> low); "is YBCO / has
5 elements" fails on #7 (124-structure -> low); "Cu3" fails on measured La-dominant YBCO.
These single-factor rules are wrong on several of the 7 (the #5 and #7 traps).

## Step C — predictions for all 7 unmeasurable
1. `O6.160Ca1Cu2Ba2Hg1` — Hg-1212, O between 6.140(105)&6.190(122) -> **high** (~113).
2. `O6.270Ca1Cu2Ba2Hg1` — Hg-1212, O between 6.240(123)&6.300(100) -> **high** (~112).
3. `O4.180Cu1Ba2Hg1` — Hg-1201, O between 4.162(97.4)&4.200(94.2) -> **high** (~96; linear ~95.9).
4. `O8Ca0.850Cu2.200Ba2Tl1.800` — double-Tl (Tl1.8~2) Ba2 Tl-2212 n=2 -> **high** (~108; Tl-22xx anchor 119).
5. `O7Ca1Cu2Sr2Tl0.500Pb0.500` — Sr (not Ba) + single charge-reservoir layer (Tl0.5+Pb0.5=1) =
   single-Tl-layer (Tl,Pb)-1212/Sr, low-Tc variant -> **low** (~80). TRAP for "contains Tl".
6. `O8Cu3Y1Ba1.900La0.130` — Y1 predominant earth, Cu3/123-structure, Ba~2 -> **high** (canonical Y-123 ~92).
7. `O8Cu4Y1Ba1.870La0.130` — Cu4 = Y-124 structure (~80K) -> **low**. TRAP for "5-elem/YBCO".

## Step D — final pick
Predicted HIGH: {1,2,3,4,6}. Element counts:
 1=5, 2=5, 3=4 (O,Cu,Ba,Hg), 4=5 (O,Ca,Cu,Ba,Tl), 6=5 (O,Cu,Y,Ba,La).
FEWEST elements = 4 -> **#3 `O4.180Cu1Ba2Hg1`** (expected Tc ~96 K). Tie-break N/A (unique 4-elem).
Note: final is robust — #3 (4-elem Hg-1201, ~96K) is the only 4-element HIGH regardless of #5's status.
