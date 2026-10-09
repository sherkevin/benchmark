# Superconductor Tc Study — Notes

## Budget
Total 18 units, shared by both tools. **Key insight: failed measure_tc probes on
unmeasurable materials do NOT consume budget** — budget only incremented on
successful searches and on successful measurements.

Final budget used: 18/18.

## Step A — series `O_x Ca1 Cu2 Ba2 Hg1` (oxygen-doping dome, 12 members)
Measured Tc:
| O | 0.080 | 0.100 | 0.130 | 0.140 | 0.160 | 0.190 | 0.210 | 0.220 | 0.240 | 0.270 | 0.300 | 0.310 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Tc | 92.0 | 92.5 | 102 | 105 | **no sample** | 122 | **127** | **127** | 123 | — | — | 98 |

Dome shape: rises 92→105, gap at 0.160, resumes 122, peak **127** at O6.210 & O6.220,
declines 123→98. Peak T* = 127 K → report O6.210Ca1Cu2Ba2Hg1 (max; ties with O6.220).

## Step B — threshold1 = T* − 32 = 127 − 32 = 95 K
4-element Hg family `O_x Cu1 Ba2 Hg1` (O,Cu,Ba,Hg). Probed 3 candidates:
- O4.047 → 95.0 ✓ (≥95)  [budget 17]
- O4.066 → ERROR "unknown composition; discover it via search_db first"
  (NOT the "no sample exists" signal that defines Step-C unmeasurable materials;
   distinct error schema with `error` key vs O6.160's `sample_available:false`.
   Composition appears in search_db results but measure_tc won't recognize it.
   Excluded from Q1 (Tc unknown) and from predictions (not an unmeasurable sample).)
- O4.081 → 98.0 ✓ (≥95)  [budget 18]
Q1 = {O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1}
(Caveat: Cu1Ba2Hg1 has 15 matches total; only page 1 (3) was retrieved —
 remaining 12 unsearched 4-element Hg-1201 members may exist but no budget left.)

## Step C — unmeasurable material(s) discovered via search_db
Only one unmeasurable material was discovered: `O6.160Ca1Cu2Ba2Hg1`
(measure_tc returned "no sample exists", budget did NOT increase).

**Rule:** Cuprate oxygen-doping series follow a single-peaked dome (parabola-like)
in Tc vs oxygen content. Predict an unsynthesized member's Tc by **monotonic
interpolation** along its branch of the dome:
- If the point sits between two measured neighbors, predict Tc between them.
- If it lies inside the [low‑O, peak] rising branch or [peak, high‑O] falling branch,
  bound it by the nearer measured neighbor and the peak.

For O6.160: neighbors O6.140 (105) and O6.190 (122), both on the rising branch.
Interpolated Tc ≈ 110–115 K → ≥ 90 → **high**. (Also below peak 127, consistent
with being on the rising side.)

## Step D — final pick
Predicted-high unmeasurable set = {O6.160Ca1Cu2Ba2Hg1}.
Fewest elements → 5 elements. Only candidate → final = O6.160Ca1Cu2Ba2Hg1.
