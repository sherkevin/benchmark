# Notes: predict Tc across Gd(1-x)Pr(x)Ba2Cu3O7 series

## Task structure
- Layout: RE(1-x)Pr(x)Ba2Cu3O7. RE = rare earth, x = Pr fraction.
- `synthesized.txt` (have samples): two REBCO RE series — Er (x = 0.030…0.900) and Nd (x = 0.030…0.900) — PLUS a B/C/Sr2CuO5 series (B_x C_{1-x} Sr2 Cu1 O5).
- `targets.txt` (unsynthesized): Gd series, x = 0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900 (11 comps).
- Budget: 30 measure_tc units. Er(9) + Nd(10) = 19 REBCO samples → fits comfortably.

## Distractor handling
The B0.04C0.96O5Cu1Sr2 series is a *different compound family* (Sr-Cu-O cuprate with B/C substitution; no Ba, no Pr, no RE, O5 not O7). It shares no chemistry with the RE-Pr-Ba2Cu3O7 targets and cannot inform a Gd interpolation. Excluded from measurement to conserve instrument time. (Confirmed by inspection of formula, not measured.)

## Plan
1. Measure all Er series (9) and all Nd series (10) → 19 units.
2. From Tc(x) for Er and Nd, infer: (a) functional form of Tc(x), (b) how the form's parameters (esp. critical Pr content x_c where Tc→0, and pure-REBCO Tc at x→0) depend on RE ionic radius.
3. Gd ionic radius (1.053 Å, 8-coord) lies between Er (1.004 Å) and Nd (1.109 Å). Interpolate the Gd Tc(x) curve.
4. Predict all 11 Gd targets; report best + critical_pr.

## Measurements

### Er series (ionic radius Er3+ ≈ 1.004 Å, 8-coord — smallest of the three)
