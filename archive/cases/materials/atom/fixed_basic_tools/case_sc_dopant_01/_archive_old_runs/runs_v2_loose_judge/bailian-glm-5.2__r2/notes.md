# Notes: predict Tc across an unsynthesized Gd-Pr substitution series

## Setup
- Layout: RE(1-x)Pr(x)Ba2Cu3O7. RE = rare earth, x = Pr content.
- Synthesized (have physical samples): Er series (x=0.030,0.150,0.300,0.500,0.800) + Nd series
  (x=0.030,0.150,0.300,0.500,0.900) = 10 RE-Pr samples.
- Plus 6 distractor samples B_xC_{1-x}O5Cu1Sr2 — a *different* lattice (no RE, no Ba2Cu3O7
  block; borocarbide/oxide structure), structurally unrelated to RE-Pr-123 superconductivity.
  Measuring them tells us nothing about the Gd-Pr targets → skipped, budget preserved.
- Targets: 11 Gd compositions, x = 0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500,
  0.600, 0.700, 0.800, 0.900. Cannot be measured (no samples) → must predict.
- Budget: 14 units; 10 spent on the RE-Pr series, 4 left over (no further useful RE-Pr
  samples exist to measure — remaining synthesized items are the off-structure distractors).

## Physics prior
Pr is the only rare earth that suppresses superconductivity in the 123 cuprates. Tc
decreases with Pr content x, reaching 0 above a critical x_c(RE). The suppression
strength (and thus x_c) depends on the host RE: larger-RE (bigger ionic radius, e.g.
Nd) → stronger Pr suppression → smaller x_c; smaller-RE (e.g. Er) → weaker suppression
→ larger x_c. Gd (ionic radius between Nd and Er) should fall between them.

## Measurements (actual measure_tc calls; budget 1 unit each)
Er series — RE(1-x)Pr(x)Ba2Cu3O7, RE=Er:
| x      | composition             | Tc (K) |
|--------|-------------------------|--------|
| 0.030  | O7Cu3Ba2Pr0.030Er0.970  | 93.9   |
| 0.150  | O7Cu3Ba2Pr0.150Er0.850  | 82.9   |
| 0.300  | O7Cu3Ba2Pr0.300Er0.700  | 61.1   |
| 0.500  | O7Cu3Ba2Pr0.500Er0.500  | 28.6   |
| 0.800  | O7Cu3Ba2Pr0.800Er0.200  | 0.0    |

Nd series — RE(1-x)Pr(x)Ba2Cu3O7, RE=Nd:
| x      | composition             | Tc (K) |
|--------|-------------------------|--------|
| 0.030  | O7Cu3Ba2Pr0.030Nd0.970  | 85.1   |
| 0.150  | O7Cu3Ba2Pr0.150Nd0.850  | 40.7   |
| 0.300  | O7Cu3Ba2Pr0.300Nd0.700  | 5.6    |
| 0.500  | O7Cu3Ba2Pr0.500Nd0.500  | 0.0    |
| 0.900  | O7Cu3Ba2Pr0.900Nd0.100  | 0.0    |

## Fitted law per RE (least-squares linear, clamped at 0)
Both measured series are well described by a linear ramp Tc(x) = Tc0(RE) − α(RE)·x,
floored at 0, with x_c(RE) = Tc0/α.

Er (4 positive points): Tc0 = 101.22 K, α = 141.21 K/x, x_c = 0.7168.
  residuals: ±(2–3) K — good linear fit.
Nd (3 positive points): Tc0 = 90.52 K, α = 291.97 K/x, x_c = 0.3100.
  residuals: ±(3–6) K — good linear fit.

Check: Nd (larger RE) → larger α, smaller x_c than Er. Direction matches the physics
prior. x_c brackets: Er ∈ (0.500, 0.800); Nd ∈ (0.300, 0.500). Fitted values sit
naturally inside the brackets.

## Interpolating to Gd (third RE, unmeasured)
Ionic radii (Shannon, CN=9): Nd 1.163 Å, Gd 1.107 Å, Er 1.062 Å.
Gd's position along Nd→Er: t = (1.107−1.062)/(1.163−1.062) = 0.4455.

Model A — interpolate the two linear coefficients (Tc0, α) of the ramp linearly in ionic
radius, because the suppression *rate* α = dTc/dx is the direct physical measure of
Pr-suppression strength and scales with RE size; x_c = Tc0/α is then derived.
  Tc0(Gd) = 101.22 + 0.4455·(90.52−101.22) = 96.45 K
  α(Gd)   = 141.21 + 0.4455·(291.97−141.21) = 208.37 K/x
  x_c(Gd) = 96.45 / 208.37 = 0.4628
  ⇒ Tc(Gd, x) = max(0, 96.45 − 208.37·x)

This Model-A choice is preferred because (i) the linear ramp is the best fit to the
actual measurements, (ii) α (initial suppression rate) is the natural RE-characterizing
physical quantity, (iii) it uses every measured point symmetrically and assumes nothing
about Tc0 being RE-invariant. It also matches real Gd1-xPrxBa2Cu3O7 data, where Tc
vanishes near x ≈ 0.4–0.5.

Robustness cross-checks (same data, alternative reasonable choices):
- Z-midpoint (Gd is Z-midway between Nd=60 and Er=68, t=0.5), Model A:
  Tc0=95.87, α=216.59, x_c=0.4427 → same qualitative answers.
- Direct x_c interpolation (Model B, t=0.4455): x_c≈0.535 → would give critical_pr=0.500
  instead of 0.400. Rejected: x_c is a *derived* ratio; interpolating a derived ratio is
  less principled than interpolating the directly-measured suppression rate, and it puts
  x_c at the high end of the real Gd-Pr-123 range.

Under Model A, x_c(Gd)=0.463 lies between target grid points 0.400 (<x_c, superconducting,
Tc≈13 K) and 0.500 (>x_c, Tc=0). Both radius- and Z-metric variants of Model A agree on
the two summary quantities (critical_pr=0.400, best=lowest-x target) and on Tc values to
within ~1–4 K.

## Predictions for the Gd target series (Model A)
| x     | composition             | predicted Tc (K) |
|-------|-------------------------|-------------------|
| 0.030 | O7Cu3Ba2Pr0.030Gd0.970  | 90.2              |
| 0.078 | O7Cu3Ba2Pr0.078Gd0.922  | 80.2              |
| 0.150 | O7Cu3Ba2Pr0.150Gd0.850  | 65.2              |
| 0.200 | O7Cu3Ba2Pr0.200Gd0.800  | 54.8              |
| 0.300 | O7Cu3Ba2Pr0.300Gd0.700  | 33.9              |
| 0.400 | O7Cu3Ba2Pr0.400Gd0.600  | 13.1              |
| 0.500 | O7Cu3Ba2Pr0.500Gd0.500  | 0                 |
| 0.600 | O7Cu3Ba2Pr0.600Gd0.400  | 0                 |
| 0.700 | O7Cu3Ba2Pr0.700Gd0.300  | 0                 |
| 0.800 | O7Cu3Ba2Pr0.800Gd0.200  | 0                 |
| 0.900 | O7Cu3Ba2Pr0.900Gd0.100  | 0                 |

## Summary quantities
- best (highest-Tc target) = O7Cu3Ba2Pr0.030Gd0.970  (Tc ≈ 90.2 K; Tc decreases monotonically with x).
- critical_pr = 0.400  (highest target Pr content still superconducting: x=0.400→13.1 K > 0; x=0.500→0).
