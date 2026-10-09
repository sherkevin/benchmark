# Superconductor Tc study — notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: Ca, Cu, Ba, Hg → 4 elements; Hg-1212-like, 2 CuO2 layers)
- high = Tc >= 90 K; low = Tc < 90 K
- Budget: 18 measure_tc calls

## Series identification (Step A)
Seed's homologous doping series = same element set {Ca,Cu,Ba,Hg}, varying O:
- O6.080, O6.100, O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310 (all Ca1Cu2Ba2Hg1) — 10 members

Other measurable series: O4.xxxCu1Ba2Hg1 (Cu,Ba,Hg; 3 elements; Hg-1201-like, 1 CuO2 layer) — 11 members
Plus: O6Cu1Ba2Hg1Tl1, O10Ca2Cu3Ba2Tl2, O6Cu1Cd0.250Ba2Tl1.750, O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800

## Step B candidates (contains Hg AND exactly 4 elements)
- 10 Ca1Cu2Ba2Hg1 series (4 elem each, has Hg) — measured in Step A
- O6Cu1Ba2Hg1Tl1 (Cu,Ba,Hg,Tl = 4 elem, has Hg) — need to measure
- O4.xxxCu1Ba2Hg1 has only 3 elements (no Ca) → excluded

## Unmeasurable (Step C/D) — 7 materials
1. O6.160Ca1Cu2Ba2Hg1 — Ca,Cu,Ba,Hg (4 elem, Hg); seed series, between O6.140 & O6.190
2. O6.270Ca1Cu2Ba2Hg1 — Ca,Cu,Ba,Hg (4 elem, Hg); seed series, between O6.240 & O6.300
3. O4.180Cu1Ba2Hg1 — Cu,Ba,Hg (3 elem, Hg); O4 Cu1Ba2Hg1 series
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Ca,Cu,Ba,Tl (4 elem, NO Hg)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — Ca,Cu,Sr,Tl,Pb (5 elem, NO Hg)
6. O8Cu3Y1Ba1.900La0.100 — Cu,Y,Ba,La (4 elem, NO Hg)
7. O8Cu4Y1Ba1.870La0.130 — Cu,Y,Ba,La (4 elem, NO Hg)

## Budget plan
- Step A: 10 calls (series)
- Step B: 1 call (O6Cu1Ba2Hg1Tl1)
- Step C: 7 calls remaining for rule induction

## Measurements (17 used / 18)

### Step A — Hg-1212 series (Ca1Cu2Ba2Hg1, O6.xxx) — all HIGH, peak plateau ~6.21–6.22
| O | Tc | high? |
|---|----|------|
| 6.080 | 92.0 | yes |
| 6.100 | 92.5 | yes |
| 6.130 | 102.0 | yes |
| 6.140 | 105.0 | yes |
| 6.190 | 122.0 | yes |
| 6.210 | **127.0** | yes ← MAX (tie with 6.220) |
| 6.220 | 127.0 | yes ← MAX |
| 6.240 | 123.0 | yes |
| 6.300 | 100.0 | yes |
| 6.310 | 98.0 | yes |

→ T* = 127.0 (peak = O6.210Ca1Cu2Ba2Hg1, first at the max; 6.220 ties)
→ threshold1 = 127 − 32 = **95.0**

### Step B — Hg & exactly-4-elements candidates, Tc ≥ 95
- All 10 Hg-1212 series members (4 elem each: Ca,Cu,Ba,Hg). ≥95 → 8 qualify (excl. 6.080@92, 6.100@92.5).
- O6Cu1Ba2Hg1Tl1 (Cu,Ba,Hg,Tl = 4 elem, has Hg) → Tc = **0.0** → excluded.
- O4.xxxCu1Ba2Hg1 only 3 elements → not in family B.

Q1 (8): O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310 (all Ca1Cu2Ba2Hg1).

### Step C — rule-induction measurements (high/low contrast)
| composition | features | Tc | H/L |
|---|---|---|---|
| O4.162Cu1Ba2Hg1 | Hg, Cu1, 3el | 97.4 | HIGH |
| O4.200Cu1Ba2Hg1 | Hg, Cu1, 3el | 94.2 | HIGH |
| O10Ca2Cu3Ba2Tl2 | Tl2, Cu3, no Hg, 4el | 118.97 | HIGH |
| O6Cu1Ba2Hg1Tl1 | Hg+Tl mixed, Cu1, 4el | 0.0 | LOW |
| O6Cu1Cd0.250Ba2Tl1.750 | Tl1.75(+Cd), Cu1, no Hg, 4el | 88.0 | LOW |
| O7Cu3Ba2La1 | no Hg/Tl, Cu3, 4el | 89.6 | LOW |
| O7Cu3Y0.200Ba2La0.800 | no Hg/Tl, Cu3, 4el | 81.0 | LOW |

### Rule induced (multi-factor)
HIGH iff exactly one of {Hg, Tl} is present as the charge-reservoir metal
(not both → mixed Hg/Tl gives 0; not neither → no reservoir gives <90),
interacting with Cu (CuO2-layer) count:
- Hg reservoir → HIGH for any Cu ≥ 1 (Hg-1201 Cu1 high; Hg-1212 Cu2 high)
- Tl reservoir → needs Cu ≥ 2 (Tl-2201 Cu1 = 88 LOW; Tl-2223 Cu3 = 119 HIGH)
Formally: HIGH iff (Hg∧¬Tl) ∨ (Tl∧¬Hg∧Cu≥2).

Verify on all 17 measured: Hg-1212 ✓, Hg-1201 ✓, Tl-2223 ✓(high), mixed Hg/Tl ✓(low), Tl-2201 ✓(low), YBCO-La×2 ✓(low). Perfect separation.

Cross-check vs hint "single-factor 'contains Hg' wrong on several unmeasurables":
that requires ≥2 non-Hg unmeasurables to be HIGH → D and E must be HIGH. ✓ supports Tl+Cu≥2 branch.

### Step C/D — predictions for unmeasurable
A. O6.160Ca1Cu2Ba2Hg1  — Hg,¬Tl → **high** (interp ~114)
B. O6.270Ca1Cu2Ba2Hg1  — Hg,¬Tl → **high** (~111)
C. O4.180Cu1Ba2Hg1     — Hg,¬Tl,Cu1 → **high** (~96, between 97.4 & 94.2)
D. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl,¬Hg,Cu2.2≥2 → **high** (Tl-2212-like ~118)
E. O7Ca1Cu2Sr2Tl0.500Pb0.500  — Tl0.5,¬Hg,Cu2≥2 → **high** ((Tl,Pb)-1212)
F. O8Cu3Y1Ba1.900La0.100 — no Hg/Tl → **low** (Y-123 overdoped, O8)
G. O8Cu4Y1Ba1.870La0.130 — no Hg/Tl → **low** (Y-124-like ~80)

### Step D — final
HIGH predicted = {A,B,C,D,E}. Element counts:
- A,B: 4 (Ca,Cu,Ba,Hg); C: 3 (Cu,Ba,Hg); D: 4 (Ca,Cu,Ba,Tl); E: 5 (Ca,Cu,Sr,Tl,Pb).
Fewest elements = **C (3)**, unique → no tie-break needed.
**Final = O4.180Cu1Ba2Hg1** (robust: C is uniquely 3-elem and unambiguously high; D/E ambiguity doesn't affect it.)

### Budget
17/18 used. 1 spare saved (no measurable Tl-Cu2 sample exists to disambiguate; remaining unmeasured are Hg-1201 series members that only re-confirm high).
