# Cross-check: parametric model vs pointwise radius-blend

Two independent methods, to bound the uncertainty:

**Method A — parametric (interpolate parameters in ionic radius):**
Fit Tc(x)=Tc0·(1−(x/xc)^n) per family, then interpolate xc,n linearly in RE3+
radius (Er 1.004, Gd 1.053, Nd 1.109; Gd at 0.467 of Er→Nd).
Er: xc=0.690, n=1.264.  Nd: xc=0.32, n=0.95.  → Gd: xc=0.517, n=1.118, Tc0=96.

**Method B — pointwise blend of the measured curves:**
Tc_Gd(x) = 0.533·Tc_Er_interp(x) + 0.467·Tc_Nd_interp(x), piecewise-linear,
clamped ≥0. (Crude: over-credits Er's long tail to Gd in the high-x region,
because it ignores that Gd has its own earlier death point.)

| x     | A: param | B: blend | Δ     | chosen | why |
|-------|----------|----------|-------|--------|-----|
| 0.030 | 92.0     | 89.8     | 2.2   | 92.0   | A respects shared Tc0≈96 ceiling; at low x all curves hug Tc0, blend under-credits this |
| 0.078 | 84.4     | 80.6     | 3.8   | 84.4   | A |
| 0.150 | 71.9     | 63.2     | 8.7   | 71.9   | A; B dragged down by Nd's 40.7 outlier-low point |
| 0.200 | 62.8     | 55.4     | 7.4   | 62.8   | A |
| 0.300 | 43.8     | 35.2     | 8.6   | 43.8   | A; B over-weights Nd already near death at 0.3 |
| 0.400 | 23.9     | 25.7     | 1.8   | 23.9   | agree |
| 0.500 | 3.5      | 15.2     | 11.7  | 3.5    | A; B's 15.2 inherits Er's longevity (Er xc=0.69) but Gd dies at xc=0.52, so x=0.5≈96% of death → nearly dead. B is physically wrong in tail |
| 0.600 | 0.0      | 10.2     | 10.2  | 0      | A; Gd xc=0.52<0.6 → dead. B's 10.2 is Er-tail artifact |
| 0.700 | 0.0      | 5.1      | 5.1   | 0      | A; same |
| 0.800 | 0.0      | 0.0      | 0     | 0      | agree (both measured 0) |
| 0.900 | 0.0      | 0.0      | 0     | 0      | agree |

## Why parametric (A) is trusted over blend (B) in the tail
A real Gd(1-x)Pr(x)Ba2Cu3O7 sample has ONE coherent suppression curve with its
OWN critical Pr fraction xc_Gd. Larger RE ion → faster Pr suppression → smaller
xc (Er 0.69 > Gd ~0.52 > Nd 0.32, monotone in radius). So Gd dies at an
intermediate x≈0.52 and is genuinely non-superconducting for x≥0.6, regardless
of Er lingering to x≈0.69. Method B's nonzero values at x=0.6–0.7 are an
artifact of dragging Er's measured tail into Gd. Method A respects per-RE death
points → trusted.

## Remaining uncertainty
- Mid-range (x=0.15–0.4): A and B differ by up to ~9K because Nd's x=0.15 point
  (40.7) is a low outlier and B weights it 0.467. A's power-law fit treats it
  as scatter and is more robust.
- The death-point xc_Gd=0.517 rests on a LINEAR-in-radius assumption (only 2 RE
  anchors). If the true radius-dependence is concave, xc_Gd could be ~0.45–0.55,
  moving the x=0.5 value in [0, ~10K]. x=0.6–0.9 = 0 is robust (xc_Gd would need
  to exceed 0.6, i.e. Gd outliving the linear trend toward Er, to change).
