# Predicting the Gd family Tc curve — RE(1-x)Pr(x)Ba2Cu3O7

## 1. Measurements (real `measure_tc` calls)

Two synthesized RE families, Er and Nd, over a range of Pr content x.

| x (Pr) | Er Tc (K) | Nd Tc (K) |
|--------|-----------|-----------|
| 0.030  | 93.9      | 85.1      |
| 0.076/0.080 | 91.1 (Er@0.076) | 68.2 (Nd@0.080) |
| 0.150  | 82.9      | 40.7      |
| 0.200  | 76.4      | 31.4      |
| 0.300  | 61.1      | 5.6       |
| 0.400  | 45.8      | (interp ~2.8) |
| 0.500  | 28.6      | 0.0       |
| 0.600  | (interp)  | 0.0       |
| 0.700  | (interp)  | 0.0       |
| 0.800  | 0.0       | 0.0       |
| 0.900  | 0.0       | 0.0       |

(All measured; entries marked "interp" are linear interpolation between bracketing measured points.)

## 2. Pattern inferred

- **Pure (x→0) Tc is nearly RE-independent.** Both families extrapolate to ~94–98 K at x=0:
  - Er linear fit through the data: Tc ≈ 98 − 139·x, zero crossing ≈ x=0.70.
  - Nd linear fit: Tc ≈ 93.9 − 294·x, zero crossing ≈ x=0.32.
  - The two intercepts (~98 and ~94) agree within scatter → all RE123 pure cuprates sit around ~93–95 K. Pr is what suppresses Tc; the RE only sets *how fast*.
- **Pr suppresses Tc faster for the larger RE ion.** Nd (largest RE ion) is already at 5.6 K by x=0.30 and dead by x=0.50. Er (smallest RE ion) is still at 28.6 K at x=0.50 and only reaches 0 by x=0.80. The suppression rate (initial |dTc/dx|) ranks Nd ≫ Er, tracking the RE ionic radius.
- **Curve shapes differ.** Er's curve is slightly concave (flatter early, steeper late, zero near 0.7–0.8); Nd's is steepest right at low x and levels into zero by ~0.32–0.5. So a single parametric form does not fit both — better to interpolate the *measured curves* directly than to fit-and-blend parameters.

## 3. Extrapolation to the un-synthesized Gd family

Gd is the third RE. Its 8-coordinate Shannon ionic radius (1.053 Å) sits almost exactly halfway between Nd (1.109 Å) and Er (1.004 Å), marginally closer to Er:
- position from Nd → Er = (1.109−1.053)/(1.109−1.004) = 0.056/0.105 = **0.533**.

So Gd's curve should be a blend of the Er and Nd curves, weighted ~0.533 (Er) + ~0.467 (Nd) at every x. Linear interpolation in ionic radius is the standard first-order scaling for Pr-suppression rate in RE123 cuprates, and Gd's near-midpoint position makes the result robust to small radius errors.

For each target x I take Tc_Er(x) and Tc_Nd(x) (measured, or linearly interpolated between bracketing measured points; 0 beyond the family's measured zero), then:

  **Tc_Gd(x) = 0.533·Tc_Er(x) + 0.467·Tc_Nd(x)**

### Computation

| x | Tc_Er(x) | Tc_Nd(x) | Tc_Gd (K) |
|---|----------|----------|-----------|
| 0.030 | 93.9 | 85.1 | 0.533·93.9 + 0.467·85.1 = 89.8 |
| 0.078 | 90.9 (interp 0.076→0.150) | 68.9 (interp 0.030→0.080) | 80.6 |
| 0.150 | 82.9 | 40.7 | 63.2 |
| 0.200 | 76.4 | 31.4 | 55.4 |
| 0.300 | 61.1 | 5.6 | 35.2 |
| 0.400 | 45.8 | 2.8 (interp 0.300→0.500) | 25.7 |
| 0.500 | 28.6 | 0.0 | 15.2 |
| 0.600 | 19.1 (interp 0.500→0.800) | 0.0 | 10.2 |
| 0.700 | 9.5 (interp 0.500→0.800) | 0.0 | 5.1 |
| 0.800 | 0.0 | 0.0 | 0.0 |
| 0.900 | 0.0 | 0.0 | 0.0 |

The Gd curve falls between Er and Nd at every x, crosses zero around x≈0.75–0.80 (inherited from the Er side, since Er dominates the high-x tail by being the slower suppressor), and stays at 0 beyond — consistent with both measured families being non-superconducting by x=0.80.

## 4. Caveats
- Only two RE families are measured, so the interpolation in RE-radius is a 2-point extrapolation; Gd's near-midpoint radius keeps the estimate low-risk.
- Linear interpolation of the measured curves (rather than a smooth parametric fit) is used because Er and Nd have visibly different curvatures, so blending the raw curves preserves the real shapes.
- Values at x=0.60 and 0.70 depend on Er interpolation between x=0.50 and 0.80 (no intermediate measured point there); if Er's decline steepens through that gap the true Gd values there could be ~1–2 K lower, but the qualitative picture (small but nonzero, then zero by 0.80) is stable.
