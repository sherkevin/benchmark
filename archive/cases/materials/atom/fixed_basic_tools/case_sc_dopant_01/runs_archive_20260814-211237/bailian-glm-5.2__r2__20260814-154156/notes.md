# Predicting the Tc(x) curve of the un-synthesized Gd family

Family: `RE(1-x)Pr(x)Ba2Cu3O7`. Pr substituting for the rare earth RE suppresses Tc;
the rate of suppression depends on RE. I measured all 19 synthesized samples
(9 Er, 10 Nd) and must extrapolate to the 11 un-synthesized Gd compositions.

## 1. Measured data (every Tc below is from a real `measure_tc` call)

### Er family (RE = Er)
| x (Pr) | Tc (K) |
|--------|--------|
| 0.030 | 93.9 |
| 0.076 | 91.1 |
| 0.150 | 82.9 |
| 0.200 | 76.4 |
| 0.300 | 61.1 |
| 0.400 | 45.8 |
| 0.500 | 28.6 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

### Nd family (RE = Nd)
| x (Pr) | Tc (K) |
|--------|--------|
| 0.030 | 85.1 |
| 0.080 | 68.2 |
| 0.150 | 40.7 |
| 0.200 | 31.4 |
| 0.300 | 5.6 |
| 0.500 | 0.0 |
| 0.600 | 0.0 |
| 0.700 | 0.0 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

## 2. Pattern: how Tc falls with Pr, and how the two RE families compare

- Both families fall **faster than linear** (concave): the per-step slope gets
  more negative as x grows. For Er the slopes steepen from ~-61 K/x (x≈0.03)
  to ~-172 K/x (x≈0.5); for Nd from ~-338 to ~-258 K/x.
- **Nd is suppressed far more aggressively than Er.** Nd is already at 0 K by
  x=0.5 (and ~5.6 K at x=0.3), whereas Er still shows 28.6 K at x=0.5.
- Approx zero-crossings: Er ≈ x 0.64; Nd ≈ x 0.33–0.40.

### Smooth fits used for the few un-measured x values
- **Er quadratic** (fit to the 7 nonzero points; residuals ≤ 1.2 K):
  `Tc_Er(x) ≈ 97.49 − 85.8·x − 106.4·x²`, zero at x ≈ 0.636. Excellent fit, so I
  trust it for the one Er point beyond the last measurement (x=0.60).
- **Nd**: its global quadratic is a poor fit (convex artifact, unphysical upturn),
  so for the single un-measured Nd point I need (x=0.40) I use **linear
  interpolation between the two bracketing measured points** (0.30→5.6, 0.50→0),
  giving Nd(0.40) ≈ 2.8 K.

## 3. Why Gd must sit midway between Nd and Er

The suppression rate tracks the **RE ionic radius**. Shannon 8-coordinate RE³⁺ radii:

- Nd³⁺: 1.109 Å
- Gd³⁺: 1.053 Å
- Er³⁺: 0.994 Å

The Nd–Er midpoint is (1.109 + 0.994)/2 = **1.0515 Å**, and Gd is **1.053 Å** —
essentially exactly the midpoint. So Gd's Tc(x) curve should be the **average of
the Nd and Er curves** at the same x. (A radius-weighted blend gives 0.487 Er /
0.513 Nd — indistinguishable from a simple 0.5/0.5 average, which I use.)

## 4. Gd predictions: `Tc_Gd(x) = 0.5·[Tc_Er(x) + Tc_Nd(x)]`, clamped at 0

Almost every target x has a **direct measurement in both families** (or is beyond
that family's zero-crossing = 0), so most predictions need no extrapolation:

| x (Pr) | Er (K) | Nd (K) | Gd = avg (K) | basis |
|--------|--------|--------|------|-------|
| 0.030 | 93.9 | 85.1 | 89.5 | both measured |
| 0.078 | 91.1 (x=0.076 meas) | 68.2 (x=0.08 meas) | 79.7 | nearest measured (Δx<0.003) |
| 0.150 | 82.9 | 40.7 | 61.8 | both measured |
| 0.200 | 76.4 | 31.4 | 53.9 | both measured |
| 0.300 | 61.1 | 5.6 | 33.4 | both measured |
| 0.400 | 45.8 | 2.8 (interp 0.30→0.50) | 24.3 | Er measured; Nd interp |
| 0.500 | 28.6 | 0.0 | 14.3 | both measured |
| 0.600 | 7.7 (Er quad extrap) | 0.0 | 3.9 | Er extrapolated; Nd measured 0 |
| 0.700 | 0.0 (Er quad, past zero) | 0.0 | 0.0 | both ≈ 0 |
| 0.800 | 0.0 | 0.0 | 0.0 | both measured |
| 0.900 | 0.0 | 0.0 | 0.0 | both measured |

The resulting Gd curve (89.5 → 79.7 → 61.8 → 53.9 → 33.4 → 24.3 → 14.3 → 3.9
→ 0 → 0 → 0) is monotone, smooth, and everywhere lies between the Er and Nd
curves — exactly what the ionic-radius argument predicts.
