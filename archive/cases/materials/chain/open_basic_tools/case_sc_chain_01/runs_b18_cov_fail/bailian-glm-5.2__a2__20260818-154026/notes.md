# Superconductor Tc study — running notes

## Setup
- Budget: 18 units shared by `measure_tc` (sample exists) and `search_db` (1/page).
- KEY finding: `measure_tc` on a composition with NO sample returns `sample_available: false`
  and does NOT consume budget (only sample-having measurements cost 1). Confirmed: after 5
  sample measurements (budget=10), `measure_tc(O6.270...)` returned no-sample with NO budget
  field, and the subsequent `search_db("Hg")` reported budget_used=11 (not 12) → 270 was free.
- Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elements: O,Ca,Cu,Ba,Hg; Hg-1212-type).
- High defined as Tc >= 90 K.

## Step A — 5-element homologous series (vary O6.x), `Ca1Cu2Ba2Hg1`
Search `Ca1Cu2Ba2Hg1` → 12 members (pages 1-4), oxygen O6.080 → O6.310:
  O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220,
  O6.240, O6.270, O6.300, O6.310

Measured (sampled across range):
| composition        | Tc    | note |
|--------------------|-------|------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0  | low end |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | **peak T* = 123.0** |
| O6.270Ca1Cu2Ba2Hg1 | —     | NO SAMPLE (unmeasurable) |
| O6.310Ca1Cu2Ba2Hg1 | 98.0  | declining |

Bell curve: 92→105→122→123(peak)→(270 unmeas)→98. Flattening 122→123 then drop to 98 ⇒
peak at O6.240. T* = 123.0. (O6.220/O6.210 unmeasured but the +1 net 190→240 and drop to
98 at 310 make 240 the measured maximum.)

## Step B — 4-element Hg materials (exactly 4 elements)
threshold1 = T* - 32 = 123 - 32 = **91.0**.

Search `Cu1Ba2Hg1` → 15 matches (pages 1-5). 14 are the 4-element `O4.xCu1Ba2Hg1`
family (Hg-1201-type); the 15th is `O6Cu1Ba2Hg1Tl1` (5 elements: O,Cu,Ba,Hg,Tl) → EXCLUDED.
The 12 five-element `Ca1Cu2Ba2Hg1` are also EXCLUDED (5 elements).

4-element members (O: 4.047→4.220):
  O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103,
  O4.150, O4.160, O4.162, O4.180, O4.200, O4.220  (all `Cu1Ba2Hg1`)

Measured (budget ran out after 2):
| composition        | Tc   | vs 91 |
|--------------------|------|-------|
| O4.047Cu1Ba2Hg1    | 95.0 | >= 91 ✓ (Q1) |
| O4.220Cu1Ba2Hg1    | 97.0 | >= 91 ✓ (Q1) |

Both measured extremes (lowest & highest oxygen) are comfortably above threshold 91.
Family is a smooth homologous series; the 5-element analog stayed >= 92 across its whole
measured range. ⇒ infer ALL 14 four-element members have Tc >= 91 → Q1 = all 14.

## Step C — unmeasurable materials
Confirmed unmeasurable (measure_tc returned "no sample"): only
  O6.270Ca1Cu2Ba2Hg1  (5 elements; in the 5-element series).
(Budget exhausted before probing more 4-element members; the 2 probed both had samples.)

Rule induced from measurable materials (TWO factors — family/structure + oxygen doping;
a single-factor oxygen rule is wrong cross-family):
  "Tc is high (>=90K) iff the oxygen content lies within the superconducting dome of its
   homologous family; the dome and the Tc ceiling are set by the family (element set /
   structure). 5-element Hg-1212 (Ca1Cu2Ba2Hg1, O6.x) ceilings ~123K with a measured dome
   O6.080(92)–O6.310(98), all >=90. 4-element Hg-1201 (Cu1Ba2Hg1, O4.x) ceilings lower
   (~95–98K). Oxygen alone is NOT a single-factor predictor across families."

Prediction for O6.270: lies inside the 5-element dome, between peak O6.240(123) and
O6.310(98). Linear interpolation: 123 - (30/70)*(123-98) = 123 - 10.7 ≈ 112K → **high**.

## Step D — final pick
Unmeasurable predicted-high set = {O6.270Ca1Cu2Ba2Hg1} (only confirmed unmeasurable).
Fewest elements: only one candidate → **final = O6.270Ca1Cu2Ba2Hg1** (5 elements;
expected Tc ≈ 112K). Tie-break (higher expected Tc) moot with one candidate.

## Limitations
- Budget exhausted (18/18) after 11 searches + 7 sample-measurements.
- Could not probe the remaining 12 four-element members to confirm which are unmeasurable;
  Q1 membership for the unmeasured 12 is inferred from family consistency (both measured
  extremes 95/97 >> 91), and they are assumed measurable (no evidence of a gap).
