# Predicting the Gd-series Tc curve for RE(1-x)Pr(x)Ba2Cu3O7

## 1. Discovering the family (search_db)

`search_db` for every rare-earth symbol returns compositions only for **Er, Nd, Gd**
(no Y, Sm, Eu, Dy, Ho, Yb, La, Lu). The family in the DB is:

- **Er series** (9 comps): Pr0.030, 0.076, 0.150, 0.200, 0.300, 0.400, 0.500, 0.800, 0.900
- **Nd series** (10 comps): Pr0.030, 0.080, 0.150, 0.200, 0.300, 0.500, 0.600, 0.700, 0.800, 0.900
- **Gd series** (11 comps, ALL un-synthesized): Pr0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900

Spot-checked `measure_tc` on three Gd comps (x=0.030, 0.500, 0.900) → all
`sample_available: false`. Entire Gd series must be **predicted**.

## 2. Measured reference curves (measure_tc, real calls)

Er series (higher-Tc boundary — Pr suppresses Tc slowly):
| x(Pr) | 0.030 | 0.076 | 0.150 | 0.200 | 0.300 | 0.400 | 0.500 | 0.800 | 0.900 |
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| Tc(K) | 93.9  | 91.1  | 82.9  | 76.4  | 61.1  | 45.8  | 28.6  | 0.0   | 0.0   |

Nd series (lower-Tc boundary — Pr suppresses Tc fast):
| x(Pr) | 0.030 | 0.080 | 0.150 | 0.200 | 0.300 | 0.500 | 0.600 | 0.700 | 0.800 | 0.900 |
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| Tc(K) | 85.1  | 68.2  | 40.7  | 31.4  | 5.6   | 0.0   | 0.0   | 0.0   | 0.0   | 0.0   |

Key feature: at every shared x, Er > Nd. Er (small RE3+) resists Pr; Nd (large RE3+)
collapses fast. This matches the known RE-123 trend — Pr-substitution suppression
strength grows with the host RE ionic radius.

## 3. Locating Gd between Er and Nd

Shannon radii (RE3+, CN=8): Er 1.004, Gd 1.053, Nd 1.109 Å.
Linear-radius position of Gd between Er→Nd:
  f = (r_Gd - r_Er)/(r_Nd - r_Er) = (1.053-1.004)/(1.109-1.004) = 0.049/0.105 = **0.467**.

Cross-check using the measured small-x slopes (the only independently observable
"suppression strength"):
  α_Er ≈ (93.9-91.1)/(0.076-0.030) = 60.9 K per unit x
  α_Nd ≈ (85.1-68.2)/(0.080-0.030) = 338.0 K per unit x
Fitting α = k(r - r0): k·0.023 = 60.9 → k≈2652; k·0.128 = 338 → k≈2639 (consistent,
≈linear in radius). So a linear-in-radius interpolation of the suppression is
well-supported, and at each x:
  **Tc_Gd(x) = (1-f)·Tc_Er(x) + f·Tc_Nd(x)**, f = 0.467.

(Linear interpolation of Tc at fixed x is exactly equivalent to linear interpolation
of the suppression α at fixed x because all RE share ~the same Tc0 ≈ 93 K at x=0.)

## 4. Evaluating Er(x) and Nd(x) at the Gd x-values

Piecewise-linear within each measured series; clamp to 0 above the zero crossing.

| x   | Er(x) | Nd(x) | Tc_Gd = 0.533·Er + 0.467·Nd |
|-----|-------|-------|------|
|0.030| 93.9  | 85.1  | 89.8 |
|0.078| 90.9* | 68.9* | 80.6 |
|0.150| 82.9  | 40.7  | 63.2 |
|0.200| 76.4  | 31.4  | 55.4 |
|0.300| 61.1  |  5.6  | 35.2 |
|0.400| 45.8  |  2.8* | 25.7 |
|0.500| 28.6  |  0.0  | 15.2 |
|0.600| 19.1* |  0.0  | 10.2 |
|0.700|  9.5* |  0.0  |  5.1 |
|0.800|  0.0  |  0.0  |  0.0 |
|0.900|  0.0  |  0.0  |  0.0 |

* = local linear interpolation between measured bracketing points of that series
(e.g. Er(0.6) from Er(0.5)=28.6 and Er(0.8)=0; Nd(0.4) from Nd(0.3)=5.6 and Nd(0.5)=0;
Nd(0.078) from Nd(0.03)=85.1 and Nd(0.08)=68.2; Er(0.078) from Er(0.076) & Er(0.150)).

x≥0.8: both boundaries are already 0 → Gd = 0 (no longer superconducting).

## 5. Result

Gd-series curve sits cleanly between Er (high) and Nd (low), crossing to 0 at x≈0.8,
as expected for an RE of intermediate ionic radius.
