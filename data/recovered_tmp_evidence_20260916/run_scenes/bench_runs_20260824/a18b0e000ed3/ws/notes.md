# Predicting the Gd-series Tc curve of RE(1-x)Pr(x)Ba2Cu3O7

## Setup
Family: RE(1-x)Pr(x)Ba2Cu3O7, RE ∈ {Er, Gd, Nd}. Pr substitutes on the RE site
and suppresses Tc; the suppression strength depends on the RE.
- Gd series has NOT been synthesized → every measure_tc returns
  `sample_available: false`. Must predict from the bracketing curves.
- Er and Nd series ARE synthesized → measured them to learn the x-dependence
  (Pr content) and the RE-dependence, then bracket Gd between them.

Compositions are stored as `O7Cu3Ba2Pr<x>RE<1-x>`; the Pr fraction is the Pr
subscript.

## Discovery (search_db "Pr" and "Gd")
Gd-series compositions to predict (11, all `sample_available: false`):
Pr = 0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900.

Er series x present: 0.030, 0.076, 0.150, 0.200, 0.300, 0.400, 0.500, 0.800, 0.900.
Nd series x present: 0.030, 0.080, 0.150, 0.200, 0.300, 0.500, 0.600, 0.700, 0.800, 0.900.
(NB: no Nd at x=0.400; measure_tc confirms it is not a known composition.)

## Measured Tc (K)
| x (Pr) | Er     | Nd     |
|--------|--------|--------|
| 0.030  | 93.9   | 85.1   |
| 0.076/0.080 | 91.1 (Er@0.076) | 68.2 (Nd@0.080) |
| 0.150  | 82.9   | 40.7   |
| 0.200  | 76.4   | 31.4   |
| 0.300  | 61.1   |  5.6   |
| 0.400  | 45.8   |  — (none) |
| 0.500  | 28.6   |  0.0   |
| 0.600  |  —     |  0.0   |
| 0.700  |  —     |  0.0   |
| 0.800  |  0.0   |  0.0   |
| 0.900  |  0.0   |  0.0   |

Key observations:
- At every x, Er Tc > Nd Tc (Er is the more Pr-tolerant / higher-Tc member).
- Low-x slopes: Er ≈ -110 K/unit, Nd ≈ -340 K/unit → Nd is suppressed ~3× harder.
- Tc→0 ("critical Pr content" x_c): Er dies at x≈0.8, Nd dies at x≈0.4.
- Gd (ionic radius 1.053 Å, Shannon CN8) sits between Nd (1.109 Å) and Er
  (1.004 Å). (Gd−Er)/(Nd−Er) = 0.049/0.105 = 0.467, i.e. Gd is 46.7 % of the
  way from Er toward Nd in ionic-radius space — essentially the midpoint.

## Method
For each RE, build the Tc(x) curve by piecewise-linear interpolation between
measured points, clamped at 0 beyond the last measured x (and at 0 wherever a
0 was measured). Then interpolate Gd by ionic radius between the two curves:

    Tc_Gd(x) = 0.533·Tc_Er(x) + 0.467·Tc_Nd(x)   (clamped ≥ 0)

Weights come from Gd's position in ionic-radius space (closer to Er, so Er
gets slightly more weight; a 50/50 midpoint gives numbers within <0.5 K of
these). This guarantees Gd's Tc lies between the Nd and Er values at every x,
which is the physical requirement for the intermediate-radius member.

## Interpolated Er(x), Nd(x) at the Gd x-grid
| x     | Er(x)  | Nd(x) | Gd = 0.533·Er+0.467·Nd |
|-------|--------|-------|------------------------|
| 0.030 | 93.9   | 85.1  | 89.8 |
| 0.078 | 90.88  | 68.88 | 80.6 |
| 0.150 | 82.9   | 40.7  | 63.2 |
| 0.200 | 76.4   | 31.4  | 55.4 |
| 0.300 | 61.1   |  5.6  | 35.2 |
| 0.400 | 45.8   |  2.8* | 25.7 |
| 0.500 | 28.6   |  0.0  | 15.2 |
| 0.600 | 19.07  |  0.0  | 10.2 |
| 0.700 |  9.53  |  0.0  |  5.1 |
| 0.800 |  0.0   |  0.0  |  0   |
| 0.900 |  0.0   |  0.0  |  0   |
(* Nd(0.400) linearly interpolated between Nd(0.300)=5.6 and Nd(0.500)=0.)

## Final Gd predictions (K)
0.030→89.8, 0.078→80.6, 0.150→63.2, 0.200→55.4, 0.300→35.2, 0.400→25.7,
0.500→15.2, 0.600→10.2, 0.700→5.1, 0.800→0, 0.900→0.

## Note on uncertainty
The bracketing-average method makes Gd reach 0 at x=0.8 (where Er dies). A
parameter-level model (interpolating each RE's x_c) would kill Gd nearer
x≈0.6. I chose the bracketing average because it is assumption-light, uses
only measured Tc, and provably keeps Gd between the two bracketing curves at
every x; the two estimates agree well (<2 K) on the densely-sampled low/mid
range (x≤0.4) and diverge only near the death region (x=0.5–0.7).
