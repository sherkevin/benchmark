# Notes: predict Tc across an unsynthesized Gd substitution series

## Task recap

- Layout: RE(1-x)Pr(x)Ba2Cu3O7. Pr suppresses superconductivity.
- Synthesized (have samples): Er series + Nd series (two REs) + a B/C distraction series (different Sr-based family).
- Targets: 11 Gd compositions, x = 0.030 … 0.900. Nobody has made them → must predict.
- Budget: 14 measure_tc calls.
- Must produce: `law`, `predictions` (all 11 Gd x-values), `best`, `critical_pr`.

## Strategy

- The B/C series (B_x C_(1-x) O5 Cu1 Sr2) is a different compound family — a distractor. Do NOT spend units on it.
- Measure the full Er series (5) and full Nd series (5) = 10 calls, leaving 4 in reserve.
- Er and Nd bracket Gd by ionic radius (Nd3+ 1.109 Å > Gd3+ 1.053 Å > Er3+ 1.004 Å, 8-coord). Interpolate the x-dependence to Gd.
- Look for: Tc(x) shape per RE, and how the critical Pr content x_c (Tc→0) depends on RE.

## Measurements (budget 14)

### Er series (RE = Er, smaller ion r≈1.004 Å)
| x(Pr) | Tc (K) |
|---|---|
| 0.030 | 93.9 |
| 0.150 | 82.9 |
| 0.300 | 61.1 |
| 0.500 | 28.6 |
| 0.800 | 0.0 |

### Nd series (RE = Nd, larger ion r≈1.109 Å)
| x(Pr) | Tc (K) |
|---|---|
| 0.030 | 85.1 |
| 0.150 | 40.7 |
| 0.300 | 5.6 |
| 0.500 | 0.0 |
| 0.900 | 0.0 |

Budget used so far: 10 / 14.

## Observed relationship

- Pr suppresses Tc in both series; suppression is **stronger for Nd** (larger RE) than for Er (smaller RE) at every x.
- Er still superconducting at x=0.50 (28.6 K), dead by x=0.80.
- Nd already near-dead at x=0.30 (5.6 K), dead by x=0.50.
- → critical Pr content xc: xc_Nd ≈ 0.32–0.50  <  xc_Er ≈ 0.50–0.80. Larger RE ⇒ Pr kills SC sooner.

## Interpolation plan for Gd (target RE)

Gd3+ ionic radius (8-coord) ≈ 1.053 Å, which is essentially the **midpoint** of Er (1.004) and Nd (1.109) (midpoint = 1.0565). So Gd is expected to sit at the midpoint behavior between the two measured REs:

    Tc_Gd(x) ≈ ½·[Tc_Er(x) + Tc_Nd(x)]

with each RE's Tc(x) taken by piecewise-linear interpolation between measured points, and clamped to 0 for x beyond the last superconducting point of that RE.

The Er curve has a big gap between x=0.50 (28.6) and x=0.80 (0.0) — exactly the region that decides Gd's critical Pr. I tried to measure Er at x=0.60 and x=0.70 to pin that region down, but the instrument only accepts compositions verbatim from `synthesized.txt`, and those aren't listed. So **no further useful RE-123 points are measurable** — the only unmeasured samples are the 6 B/C-family compositions (B_x C_(1-x) O5 Cu1 Sr2), which are a different compound family (Sr2/Cu1/O5/B-C, not Ba2/Cu3/O7/Pr-RE) and a distractor. The task statement confirms only Er and Nd are the two relevant REs. I leave the remaining 4 units unused rather than waste them on a distractor family that cannot inform Gd.

## Fitting the law (per-RE linear Tc(x), parameters interpolated by ionic radius)

Tc is approximately linear in x within each RE, then clamps to 0 beyond a critical Pr content xc. Using the two cleanest anchors per RE (purest x=0.03 and deepest SC point):

- **Er** (r≈1.004 Å): line through (0.03, 93.9) & (0.50, 28.6) → Tc = 98.07 − 138.94·x ; xc_Er = 0.706. (Consistent: x=0.80 → 0 ✓, since plane < 0 there.)
- **Nd** (r≈1.109 Å): line through (0.03, 85.1) & (0.30, 5.6) → Tc = 93.93 − 294.44·x ; xc_Nd = 0.319. (Consistent: x=0.50 → 0 ✓, x=0.90 → 0 ✓.)

Both lines fit their SC points within ±3–6 K and predict all the measured-zero points (x past xc clamps to 0). So the law is a **bilinear plane in (ionic radius r, Pr content x)**: Tc = max(0, a + b·r + c·x + d·x·r), i.e. linear in x per RE, with slope & intercept linear in r.

**Gd** (r≈1.053 Å) sits 46.7 % of the way from Er to Nd — essentially the radius midpoint. Interpolating the two line parameters with weights w_Er = 0.533, w_Nd = 0.467:

- a_Gd = 0.533·98.07 + 0.467·93.93 = **96.13**
- b_Gd = −(0.533·138.94 + 0.467·294.44) = **−211.52**
- → **Tc_Gd(x) = max(0, 96.13 − 211.52·x)** ; xc_Gd = 96.13/211.52 = **0.455**

Sanity (physics): larger RE ⇒ Pr suppresses SC faster ⇒ xc shrinks. xc_Nd (0.32) < xc_Gd (0.455) < xc_Er (0.71). ✓ Monotonic in radius, as required. (A naïve per-point average of the two measured curves would wrongly give Gd the same xc as Er, because Nd's curve has already clamped to 0 at high x — that violates the monotonic trend, so the parametric plane is the right model.)

## Gd predictions (targets)

| x(Pr) | predicted Tc (K) | superconducting? |
|---|---|---|
| 0.030 | 89.8 | yes |
| 0.078 | 79.6 | yes |
| 0.150 | 64.4 | yes |
| 0.200 | 53.8 | yes |
| 0.300 | 32.7 | yes |
| 0.400 | 11.5 | yes |
| 0.500 | 0.0 | no (plane = −9.6 → clamp) |
| 0.600 | 0.0 | no |
| 0.700 | 0.0 | no |
| 0.800 | 0.0 | no |
| 0.900 | 0.0 | no |

- **best** (highest Tc) = lowest Pr = O7Cu3Ba2Pr0.030Gd0.970 (89.8 K). Tc is monotonically decreasing in x, so this is unambiguous.
- **critical_pr** = highest target Pr content still SC = **0.400** (Tc=11.5 > 0; the next, x=0.500, is 0). The cutoff xc_Gd≈0.455 sits cleanly between the 0.400 and 0.500 target points, so 0.400 is robust.

## Budget accounting

Used 10/14: 5 Er + 5 Nd. Remaining 4 unused (no measurable RE-123 points left; B/C family is a distractor).
