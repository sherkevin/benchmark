# Notes: Tc curve prediction for RE(1-x)Pr(x)Ba2Cu3O7 cuprates

## Setup

- System: `RE(1-x)Pr(x)Ba2Cu3O7`. Pr on the RE site suppresses Tc; the *rate* depends on the RE.
- Synthesized (measurable): Er family (9 pts) + Nd family (10 pts).
- To predict (no sample): **Gd family** (11 pts, x = 0.030…0.900).
- Notation `O7Cu3Ba2Pr0.030Er0.970` ⇒ x = Pr fraction = 0.030, RE = Er.

## Measurements (real `measure_tc` calls)

Er (Z=68):
| x     | Tc(K) |
|-------|-------|
| 0.030 | 93.9  |
| 0.076 | 91.1  |
| 0.150 | 82.9  |
| 0.200 | 76.4  |
| 0.300 | 61.1  |
| 0.400 | 45.8  |
| 0.500 | 28.6  |
| 0.800 | 0.0   |
| 0.900 | 0.0   |

Nd (Z=60):
| x     | Tc(K) |
|-------|-------|
| 0.030 | 85.1  |
| 0.080 | 68.2  |
| 0.150 | 40.7  |
| 0.200 | 31.4  |
| 0.300 | 5.6   |
| 0.500 | 0.0   |
| 0.600 | 0.0   |
| 0.700 | 0.0   |
| 0.800 | 0.0   |
| 0.900 | 0.0   |

## Pattern inferred

1. **Suppression is monotonic in x for both REs; Nd suppresses far faster than Er.**
   At x=0.15: Er=82.9 vs Nd=40.7. At x=0.30: Er=61.1 vs Nd=5.6 (Nd essentially dead).
   Er is still superconducting at x=0.5 (28.6 K); Nd is dead by x=0.5.
2. **Shape differs:** Er is concave (local slope steepens from ~−60 near x=0 to ~−172 by x=0.5);
   Nd is roughly linear with a steeper slope (~−294) and dies near x≈0.31–0.5.
3. **Both reach Tc≈0 (no longer superconducting) past a RE-dependent critical Pr content xc:**
   xc(Er)≈0.6–0.8, xc(Nd)≈0.31–0.5.

## Per-family fits (nonzero points only)

- **Er (concave):** quadratic fit Tc_Er(x) ≈ 96.3 − 75.4·x − 120·x², clamped ≥0.
  Hits 0 at x≈0.635. Max residual vs data ≈1.8 K.
- **Nd (near-linear):** linear fit Tc_Nd(x) ≈ 91.0 − 294.4·x, clamped ≥0.
  Hits 0 at x≈0.31.

## Extrapolation to Gd

Gd³⁺ ionic radius (1.053 Å) is essentially the **midpoint** of Nd³⁺ (1.109 Å) and Er³⁺
(0.994 Å): (1.109+0.994)/2 = 1.0515 ≈ 1.053. Since the suppression is controlled by the
RE ionic radius and Gd sits halfway between Nd and Er, the Gd Tc(x) curve is taken as the
midpoint of the Nd and Er curves:

  Tc_Gd(x) = ½·[ Tc_Er_fit(x) + Tc_Nd_fit(x) ], clamped ≥ 0.

Sanity checks at shared x (predicted vs "average of two measurements"):
- x=0.03: ½(93.9+85.1)=89.5 vs fit-average 88.1 ✓
- x=0.15: ½(82.9+40.7)=61.8 vs fit-average 64.6 ✓ (Nd fit over-smooths the dip at 0.15)
- x=0.30: ½(61.1+5.6)=33.4 vs fit-average 32.8 ✓
- x=0.50: ½(28.6+0)=14.3 ✓

The low-x region (where Tc is substantial) is well pinned by direct measurements; the tail
(x≥0.6) is more uncertain but values are small in absolute terms. Gd is predicted to lose
superconductivity around x≈0.6–0.65 (between Nd's ~0.31–0.5 and Er's ~0.635).

## Final Gd predictions (K)

| x     | Tc_Gd |
|-------|-------|
| 0.030 | 88.1  |
| 0.078 | 78.9  |
| 0.150 | 64.6  |
| 0.200 | 54.3  |
| 0.300 | 32.8  |
| 0.400 | 23.5  |
| 0.500 | 14.3  |
| 0.600 | 3.8   |
| 0.700 | 0.0   |
| 0.800 | 0.0   |
| 0.900 | 0.0   |
