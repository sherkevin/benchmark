# Superconductor Tc Study — Notes

## Task overview
- Budget: 18 units total, shared between measure_tc and search_db
- Seed material: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg = 5 elements)
  - Wait: O, Ca, Cu, Ba, Hg = 5 elements
- High = Tc >= 90K, Low = Tc < 90K
- 4 steps: A (series peak), B (filter family B), C (extrapolate), D (final pick)

## Step A — series peak
- Seed belongs to homologous doping series (same element set, varying oxygen)
- Element set of seed: {O, Ca, Cu, Ba, Hg}
- Varying oxygen means O coefficient changes, rest same
- Need to find series members and measure to find MAX Tc = T*

## Step B — filter family B
- threshold1 = T* - 32
- Family B: contains Hg AND exactly 4 elements
- Report ALL with Tc >= threshold1 (set Q1)

## Step C — extrapolate
- Some materials never synthesized — measure_tc returns "no sample exists"
- Induce rule from measurable, predict high/low for every unmeasurable
- Warning: single-factor rules wrong on several

## Step D — final pick
- Among unmeasurable predicted high, choose FEWEST elements (tie-break higher expected Tc)

## Budget tracking
- Start: 18
- Search "Hg" pages 1-9: used 9
- Remaining: 9 (for all measurements across Steps A-D)

## All Hg materials found (27 total)
### Seed series (Step A): O6.xxxCa1Cu2Ba2Hg1 — 5 elements {O,Ca,Cu,Ba,Hg}, 12 members
O6.080(seed), O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310
### Family B candidates (4 elements {O,Cu,Ba,Hg}): O4.xxxCu1Ba2Hg1 — 14 members
O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
### 5-element Tl variant
O6Cu1Ba2Hg1Tl1 (elements O,Cu,Ba,Hg,Tl = 5) — NOT family B

## ALL MEASUREMENT DATA (budget 18/18 exhausted)

### Step A — seed series O6.xxxCa1Cu2Ba2Hg1 (Hg-1212, 2 CuO2 layers)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measurable (underdoped) |
| O6.160Ca1Cu2Hg1 | null | UNMEASURABLE (no sample) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measurable |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** | measurable ← PEAK T* |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measurable (overdoped) |
| O6.270Ca1Cu2Ba2Hg1 | null | UNMEASURABLE (no sample) |

**Peak T* = 127 K at O6.220Ca1Cu2Ba2Hg1.** (Bell curve: 92→122→127→123; peak at O6.220.)
Note: O6.200 returned "unknown composition" — not a real DB entry (real neighbor is O6.210).
Note: Series is NOT all-synthesized — O6.160 and O6.270 are unmeasurable.

### Step B — family B O4.xxxCu1Ba2Hg1 (Hg-1201, 1 CuO2 layer, exactly 4 elements {O,Cu,Ba,Hg})
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.103Cu1Ba2Hg1 | 92.5 | no |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.162Cu1Ba2Hg1 | **97.4** | YES (peak) |
| O4.200Cu1Ba2Hg1 | 94.2 | no |
| O4.180Cu1Ba2Hg1 | null | UNMEASURABLE |

threshold1 = T* - 32 = 127 - 32 = **95 K**
Hg-1201 peak ~97K. **Q1 = {O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1}** (both >= 95K).
(O4.160 was "unknown composition" — not a real DB entry; peak at O4.162=97.4.)

### Additional data point for rule induction
| Composition | Tc (K) | Notes |
|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | 5 elements (O,Cu,Ba,Hg,Tl), NO Ca, O6 round → p=0, non-superconducting |

## Step C — unmeasurable materials & rule induction
Unmeasurable found via search_db:
1. O6.160Ca1Cu2Ba2Hg1 (series, 2 Cu layers, optimal-adjacent O6.160)
2. O6.270Ca1Cu2Ba2Hg1 (series, 2 Cu layers, overdoped O6.270)
3. O4.180Cu1Ba2Hg1 (family B, 1 Cu layer, optimal-adjacent O4.180)

### Multi-factor rule (per warning: single-factor rules fail)
Combined physics + measured-curve logic:
1. **# CuO2 layers sets peak ceiling**: 1 layer (Hg-1201, O4 no-Ca) → peak ~97K → HIGH only near optimal O (4.15-4.17). 2 layers (Hg-1212, O6.Ca) → peak ~127K → HIGH over wide O range (~6.13-6.27+).
2. **Optimal-doping window**: underdoped far from peak → LOW even with many layers. (O6.080=92 just barely high; O4.103=92.5 high by def>=90.)
3. **Structural completeness / Ca presence**: O6Cu1Ba2Hg1Tl1 has NO Ca → p=0 → totally non-superconducting (Tc=0), LOW.
4. **HIGH vs LOW threshold (>=90K)** for predictions: need BOTH (a) enough layers (1+ near-peak O, or 2) AND (b) O doping within the superconducting dome.

### Predictions
- **O6.160Ca1Cu2Ba2Hg1** (2 layers, O6.160 just below measured-peak O6.220; O6.190=122 already high; O6.160 is between O6.080=92 and O6.190=122, within dome) → **HIGH** (expected ~120-125K, on rising side).
- **O6.270Ca1Cu2Ba2Hg1** (2 layers, O6.270 — just past measured O6.240=123; still within dome on gentle overdoped side, 2-layer peak~127) → **HIGH** (expected ~120K, dome is broad for 2 layers).
- **O4.180Cu1Ba2Hg1** (1 layer, O4.180 — between O4.162=97.4 peak and O4.200=94.2, still within 1-layer dome near peak) → **HIGH** (expected ~96-97K, near peak, >90).

## Step D — final pick
Predicted HIGH unmeasurables: O6.160 (5 elements), O6.270 (5 elements), O4.180 (4 elements).
FEWEST elements = **O4.180Cu1Ba2Hg1** (4 elements). → FINAL answer.
(Tie-break by higher expected Tc not needed: 4 elements is unique minimum.)

## Double-check: single-factor rule failure (sanity)
- A pure "#elements" rule would call O6Cu1Ba2Hg1Tl1 (5 elem, but it's measurable & 0K) — not in unmeasurable set, N/A, but shows structure matters.
- A pure "#Cu layers" rule: O6.080 (2 layers) is only 92K (barely HIGH), O4.103 (1 layer) is 92.5 (HIGH) — so 1-layer can be HIGH near peak too. Confirms multi-factor needed; my predictions respect O-window + layers.
