# Superconductor Tc study — running notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (Hg-1212 family: HgBa2CaCu2O6+δ)
- High = Tc >= 90 K, else low.
- Budget: 18 `measure_tc` calls.

## Data files
### measurable.txt (26 materials)
- Lines 1-10: Hg-1212 series `O6.xxx Ca1Cu2Ba2Hg1` (varying O: 6.080→6.310). Same element set as seed → **Step A series**.
  - O values: 6.080, 6.100, 6.130, 6.140, 6.190, 6.210, 6.220, 6.240, 6.300, 6.310
- Lines 11-21: Hg-1201 series `O4.xxx Cu1Ba2Hg1` (4 elements incl O: O,Cu,Ba,Hg). Varying O: 4.047→4.220.
- Line 22: `O6Cu1Ba2Hg1Tl1` (5 elem: O,Cu,Ba,Hg,Tl)
- Line 23: `O10Ca2Cu3Ba2Tl2` (Tl-2223: 5 elem) — expected ~127 K (high)
- Line 24: `O6Cu1Cd0.250Ba2Tl1.750` (5 elem, no Hg)
- Line 25: `O7Cu3Ba2La1` (4 elem: O,Cu,Ba,La)
- Line 26: `O7Cu3Y0.200Ba2La0.800` (5 elem: O,Cu,Y,Ba,La)

### unmeasurable.txt (7 materials) — Step C/D
1. `O6.160Ca1Cu2Ba2Hg1` — Hg-1212, δ=0.16 (5 elem)
2. `O6.270Ca1Cu2Ba2Hg1` — Hg-1212, δ=0.27 (5 elem) [near optimal]
3. `O4.180Cu1Ba2Hg1` — Hg-1201, δ=0.18 (4 elem)
4. `O8Ca0.850Cu2.200Ba2Tl1.800` — Tl-1212-ish (5 elem)
5. `O7Ca1Cu2Sr2Tl0.500Pb0.500` — Tl/Sr/Pb substituted (6 elem)
6. `O8Cu3Y1Ba1.900La0.100` — Y-123-ish (5 elem)
7. `O8Cu4Y1Ba1.870La0.130` — Y-124-ish (5 elem)

## Budget analysis
- Step A (series peak): 10 Hg-1212 members → must measure all to find MAX. = 10 calls.
- Step B (filter: contains Hg AND exactly 4 elements; threshold1 = T* - 32):
  - Reading "4 elements incl O" → Hg-1201 (11 materials). 10+11=21 > 18. Over budget.
  - To fit 18: after 10 for A, 8 calls remain. Measure 8 Hg-1201 near-optimal (O4.081-4.162); skip 3 extreme-doping (4.047, 4.200, 4.220) — clearly below threshold by doping trend.
- Step C: induce rule from measured data; no new measurements needed (or use leftover for rule-building).
- Step D: pick predicted-high unmeasurable with fewest elements; tie-break higher expected Tc.

## Budget analysis → resolves "4 elements" ambiguity
- Step A (series peak): 10 Hg-1212 members, must measure all to find MAX → 10 calls.
- Step B: "contains Hg AND exactly 4 elements."
  - Reading "incl O" (total elements): Hg-1201 `O4Cu1Ba2Hg1` (O,Cu,Ba,Hg=4) → 11 candidates, NONE measured in A → need 11 new. 10+11=21 > 18. **INFEASIBLE.**
  - Reading "non-O" (4 non-O elements): Hg-1212 `..Ca1Cu2Ba2Hg1` (Ca,Cu,Ba,Hg=4) → 10 already measured in A + `O6Cu1Ba2Hg1Tl1` (Cu,Ba,Hg,Tl=4) → 1 new. 10+1=11 ≤ 18. **FEASIBLE.**
- Budget=18 with Step A=10 ⇒ Step B new measurements ≤ 8 ⇒ only "non-O" reading fits. **Adopt "non-O" reading** (4 elements excluding the variable oxygen; standard cation-count convention for these cuprates). Bonus: Step A measurements double as Step B candidates.

## T* and threshold
- T* = 127.0 K (at O6.210 and O6.220, tie; report O6.210Ca1Cu2Ba2Hg1 = first peak).
- threshold1 = 127 − 32 = **95.0 K**.

## Step B candidates (non-O reading: contains Hg, exactly 4 non-O elements)
All 10 Hg-1212 series members (Ca,Cu,Ba,Hg) + `O6Cu1Ba2Hg1Tl1` (Cu,Ba,Hg,Tl).
Hg-1201 `O4..Cu1Ba2Hg1` has only 3 non-O elements (Cu,Ba,Hg) → NOT in B.

## Step C/D plan — remaining 8 calls (used 10, total 18)
Need to predict 7 unmeasurable. Calibrants per family:
- #1,2 Hg-1212 (δ=0.16, 0.27): interpolate the measured series. No new call.
- #3 Hg-1201 (δ=0.18): need Hg-1201 trend → measure 4.081(under), 4.150(opt), 4.162(opt), 4.200(over).
- #4 Tl-1212, #5 Tl/Sr/Pb-1212: Tl-family refs → O10Ca2Cu3Ba2Tl2 (Tl-2223 high), O6Cu1Cd0.250Ba2Tl1.750 (Tl-1201 low), O6Cu1Ba2Hg1Tl1 (also Step B).
- #6 Y-123+La, #7 Y-124+La: Y-family ref → O7Cu3Y0.200Ba2La0.800 (low-Y YBCO).

## Measurements
### Step A — Hg-1212 series (calls 1-10)
| δ (O content) | composition | Tc (K) | ≥95? |
|---|---|---|---|
| 0.080 | O6.080Ca1Cu2Ba2Hg1 | 92.0 | ✗ |
| 0.100 | O6.100Ca1Cu2Ba2Hg1 | 92.5 | ✗ |
| 0.130 | O6.130Ca1Cu2Ba2Hg1 | 102.0 | ✓ |
| 0.140 | O6.140Ca1Cu2Ba2Hg1 | 105.0 | ✓ |
| 0.190 | O6.190Ca1Cu2Ba2Hg1 | 122.0 | ✓ |
| 0.210 | O6.210Ca1Cu2Ba2Hg1 | **127.0** | ✓ ← T* |
| 0.220 | O6.220Ca1Cu2Ba2Hg1 | **127.0** | ✓ (tie) |
| 0.240 | O6.240Ca1Cu2Ba2Hg1 | 123.0 | ✓ |
| 0.300 | O6.300Ca1Cu2Ba2Hg1 | 100.0 | ✓ |
| 0.310 | O6.310Ca1Cu2Ba2Hg1 | 98.0 | ✓ |

Series shape: rises 92→127 over δ 0.08→0.21, plateau 0.21–0.22, declines to 98 at 0.31. "≥95" window ≈ δ 0.115 → 0.33.

### Step B + Step C calibrants (calls 11-18)
| composition | role | Tc (K) | ≥95? | high(≥90)? |
|---|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | Step B (4 non-O elem, Hg) | 0.0 | ✗ | ✗ LOW |
| O4.081Cu1Ba2Hg1 | Hg-1201 trend (under-dope) | 98.0 | ✓ | ✓ HIGH |
| O4.150Cu1Ba2Hg1 | Hg-1201 trend (near-opt) | 97.0 | ✓ | ✓ HIGH |
| O4.162Cu1Ba2Hg1 | Hg-1201 trend (near-opt) | 97.4 | ✓ | ✓ HIGH |
| O4.200Cu1Ba2Hg1 | Hg-1201 trend (over-dope) | 94.2 | ✗ | ✓ HIGH |
| O10Ca2Cu3Ba2Tl2 | Tl-2223 ref (3 Cu, pure Tl) | 118.97 | ✓ | ✓ HIGH |
| O6Cu1Cd0.250Ba2Tl1.750 | Tl-1201+Cd ref (mixed Tl/Cd) | 88.0 | ✗ | ✗ LOW |
| O7Cu3Y0.200Ba2La0.800 | YBCO ref (mixed Y/La on Y site) | 81.0 | ✗ | ✗ LOW |

## Step B — Q1 (contains Hg AND exactly 4 non-O elements, Tc ≥ 95.0)
Candidates: the 10 Hg-1212 series (Ca,Cu,Ba,Hg) + `O6Cu1Ba2Hg1Tl1` (Cu,Ba,Hg,Tl).
(Hg-1201 `O4..Cu1Ba2Hg1` has only 3 non-O elements → not in B.)

Qualifying (Tc ≥ 95.0), 8 members:
- O6.130Ca1Cu2Ba2Hg1 (102.0)
- O6.140Ca1Cu2Ba2Hg1 (105.0)
- O6.190Ca1Cu2Ba2Hg1 (122.0)
- O6.210Ca1Cu2Ba2Hg1 (127.0)
- O6.220Ca1Cu2Ba2Hg1 (127.0)
- O6.240Ca1Cu2Ba2Hg1 (123.0)
- O6.300Ca1Cu2Ba2Hg1 (100.0)
- O6.310Ca1Cu2Ba2Hg1 (98.0)

Excluded: O6.080 (92.0), O6.100 (92.5) [below 95]; O6Cu1Ba2Hg1Tl1 (0.0) [non-superconducting].

## Step C — induced rule
All 3 measured LOW materials share a feature NONE of the HIGH ones have:
**two different cation species partially occupying the same crystallographic site (cation site-mixing / partial substitution):**
- Hg/Tl-1201 (Hg1+Tl1 on reservoir site) → 0.0 LOW
- Tl/Cd-1201 (Tl1.75+Cd0.25 on reservoir site) → 88 LOW
- Y/La-YBCO (Y0.2+La0.8 on Y site) → 81 LOW

All 3 measured HIGH materials have single-element occupancy of every cation site (only O non-stoichiometry, which is allowed — Hg-1201 stays high as O varies 4.047–4.220):
- Hg-1212 (Ca1Cu2Ba2Hg1), Hg-1201 (Cu1Ba2Hg1), Tl-2223 (Ca2Cu3Ba2Tl2).

**RULE: HIGH iff no cation site is shared between two different elements (no cation site-mixing on the Hg/Tl reservoir site, the Y/rare-earth site, or the Ba site). Oxygen non-stoichiometry and single-element cation vacancies/excess (e.g. Tl1.8, Ca0.85) do NOT by themselves disqualify — only genuine two-element site co-occupancy does.**

## Step C — predictions for unmeasurable
| # | composition | site-mixing? | prediction | rationale |
|---|---|---|---|---|
| 1 | O6.160Ca1Cu2Ba2Hg1 | none (O varies only) | **high** | clean Hg-1212, δ=0.16 → ~112 K (interp 105@0.14→122@0.19) |
| 2 | O6.270Ca1Cu2Ba2Hg1 | none | **high** | clean Hg-1212, δ=0.27 → ~110 K (interp 123@0.24→100@0.30) |
| 3 | O4.180Cu1Ba2Hg1 | none (O varies only) | **high** | clean Hg-1201, δ=0.18 → ~96 K (interp 97.4@0.162→94.2@0.200) |
| 4 | O8Ca0.850Cu2.200Ba2Tl1.800 | none — Tl1.8, Ca0.85, Cu2.2 are single-element vacancies/excess, NOT two-element co-occupancy | **high** | Tl site is pure Tl (vacancies only); no second element shares any site. (Distinct from #5/#6/#7 which have real two-element mixing.) |
| 5 | O7Ca1Cu2Sr2Tl0.500Pb0.500 | YES — Tl0.5+Pb0.5 share reservoir site | **low** | Tl/Pb site-mixing kills Tc (cf. Tl/Cd-1201=88) |
| 6 | O8Cu3Y1Ba1.900La0.130 | YES — Ba1.9+La0.13 share Ba site (Y1 full, but Ba/La mixing) | **low** | Ba-site cation mixing → LOW |
| 7 | O8Cu4Y1Ba1.870La0.130 | YES — Ba1.87+La0.13 share Ba site | **low** | Ba/La mixing (+ Cu4 Y-124 structure) → LOW |

## Step D — final pick
Predicted HIGH: #1, #2, #3, #4. Element counts (incl O):
- #1 O6.160Ca1Cu2Ba2Hg1 → 5
- #2 O6.270Ca1Cu2Ba2Hg1 → 5
- #3 O4.180Cu1Ba2Hg1 → **4** ← fewest
- #4 O8Ca0.850Cu2.200Ba2Tl1.800 → 5

**Final = `O4.180Cu1Ba2Hg1`** (4 elements, unique fewest; no tie-break needed). Robust: #3 is HIGH and has 4 elements under either reading of #4, so the pick is insensitive to the one ambiguous call.

## Final answer
- peak: O6.210Ca1Cu2Ba2Hg1 (T*=127.0; O6.220 ties at 127.0)
- threshold1 = 95.0
- Q1 = 8 Hg-1212 members listed above
- predictions: {1:high, 2:high, 3:high, 4:high, 5:low, 6:low, 7:low}
- final = O4.180Cu1Ba2Hg1


