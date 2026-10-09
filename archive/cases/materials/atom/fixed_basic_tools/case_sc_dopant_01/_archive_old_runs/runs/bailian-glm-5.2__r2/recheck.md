# Self-review / recheck of the two knife-edge deliverables

## critical_pr — recheck (the swing question: is x_c(Gd) > or < 0.500?)

x_c(Gd) = 0.533·x_c(Er) + 0.467·x_c(Nd)   [radius interpolation, f=0.467]

Pinning x_c from the data point CLOSEST to each RE's cutoff (most constraining):
- Er: 28.6 K @ x=0.500 is the highest-x nonzero Er point. Fit power-law Tc=T0·(1-x/x_c)^p:
  for p in [0.7, 1.0] (the plausible Er shape range), x_c(Er) solves to [0.606, 0.710].
  Central (p=0.84): x_c(Er)=0.656.  -> weights the 0.500 point, the strongest constraint.
- Nd: 5.6 K @ x=0.300 is the highest-x nonzero Nd point. Gives x_c(Nd)=0.340 (tight;
  5.6 K so close to 0 pins x_c near 0.34 across plausible p).

=> x_c(Gd) = 0.533·0.656 + 0.467·0.340 = 0.509.   > 0.500.

So Gd(0.500) is just INSIDE the cutoff (Tc ~ 1-2 K > 0), Gd(0.600) past it (=0).
=> critical_pr = 0.500.

The alternative (critical_pr=0.400) requires x_c(Er) <= 0.60, which only arises if you
weight the x=0.30 Er point (poorly fit by any power law) over the x=0.50 point. The
x=0.50 point is closer to the cutoff and more constraining; trusting it gives 0.500.

Verdict: critical_pr = 0.500. Knife-edge acknowledged (x_c(Gd)=0.509 +/- ~0.02; if GT
x_c(Gd) <= 0.500 then critical_pr would be 0.400).

## max_pr_above_80K — recheck (is Gd(0.078) above or below 80?)

Best (parameter-free) estimate: model-free radius blend at x=0.078, using linear
interpolation of MEASURED Er and Nd between their measured x=0.030 and x=0.150 points
(pure interpolation, no extrapolation, no curve-fit assumptions):
  Er(0.078) = 93.9 + 0.40*(82.9-93.9) = 89.50   [0.40 = (0.078-0.030)/0.120]
  Nd(0.078) = 85.1 + 0.40*(40.7-85.1) = 67.34
  Gd(0.078) = 89.50 + 0.467*(67.34-89.50) = 79.15 K.   < 80.

Curvature correction: Er is concave-down (real Er(0.078) slightly ABOVE its chord 89.50,
raising Gd); Nd is concave-up (real Nd(0.078) BELOW its chord 67.34, lowering Gd).
Net effect ~= (1-f)*dEr + f*dNd, weights similar -> near-cancels; estimated +0.0 to -0.1 K.
=> Gd(0.078) ~= 79.1-79.2 K, robustly (if narrowly) below 80.

Independent cross-check via parameter-interp power-law (Tc=95.2*(1-x/0.509)^1.064):
Gd(0.078) = 79.8 K. Also < 80. Two methods agree.

80 K crossing at x ~= 0.075, which falls BETWEEN target 0.030 (>80) and 0.078 (<80).
=> max_pr_above_80K = 0.030.   Robust.

## Tail predictions (x=0.400, 0.500) — method comparison
  x=0.400:  model-free A (unreliable, smears Nd cutoff) ~24; u-space ~20; param-interp ~18.5.
            Reliable methods cluster ~18.5-20. Use 19.0 (central).  Uncertainty +/- 4 K.
  x=0.500:  param-interp 1.3; u-space 2.0; power-law-B 0.8.  Cluster 0.8-2.0. Use 1.3.
            Uncertainty +/- 1.5 K. Knife-edge: this is >0 (=> critical_pr=0.500).

## Final predictions (low x: model-free A, measured-anchored; tail: param-interp/u-space)
  0.030: 89.8   0.078: 79.2   0.150: 63.2   0.200: 53.8   0.300: 35.2
  0.400: 19.0   0.500: 1.3    0.600: 0.0    0.700: 0.0    0.800: 0.0   0.900: 0.0

  max_pr_above_80K = 0.030
  critical_pr      = 0.500
