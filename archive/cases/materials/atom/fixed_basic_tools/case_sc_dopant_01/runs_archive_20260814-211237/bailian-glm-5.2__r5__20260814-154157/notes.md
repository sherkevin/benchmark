# Predicting the Tc curve of RE(1-x)Pr(x)Ba2Cu3O7 — Gd family

## Task
Predict Tc for 11 Gd-family compositions (not yet synthesized), using two
synthesized families (Er, Nd) to learn how Tc depends on Pr content x and the RE.

## Measured data (real measure_tc calls)

### Er family (RE = Er)
| x   | Tc (K) |
|-----|--------|
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
| x   | Tc (K) |
|-----|--------|
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

## Pattern analysis

Both families show Tc dropping roughly linearly with Pr content x, then
collapsing to 0 (no superconductivity) past a critical x that depends on the RE.

### Linear-regime slopes and Tc(0) intercept
- **Er**: from the first 7 nonzero points (x=0.03..0.5), linear fit
  Tc ≈ Tc0 − m·x gives Tc0 ≈ 95.0 K, m ≈ 132 K per unit x.
  - check: x=0.030 → 95.0−132*0.030 = 95.0−3.96 = 91.0 (meas 93.9, ok-ish)
  - x=0.5 → 95.0−132*0.5 = 95.0−66 = 29.0 (meas 28.6 ✓)
  - x=0.4 → 95.0−132*0.4 = 95.0−52.8 = 42.2 (meas 45.8, close)
  - Zero crossing at x = 95.0/132 ≈ 0.72; indeed Tc=0 by x=0.8 ✓
- **Nd**: Tc0 lower (~90?), slope much steeper.
  - linear fit on first 5 nonzero points (x=0.03..0.3): Tc0 ≈ 90, m ≈ 300 K/x
  - check: x=0.03 → 90−9 = 81 (meas 85.1, close); x=0.3 → 90−90 = 0 (meas 5.6, near zero) ✓
  - Zero crossing at x = 90/300 = 0.30; Tc=0 by x=0.5 ✓

### RE comparison — the rare-earth ionic radius
The key physics: Pr suppresses Tc faster for RE with LARGER ionic radius
(Nd is larger than Er). Nd (1.09 Å, 8-coord) > Gd (1.06 Å) > Er (1.00 Å).
Nd has the steepest suppression, Er the mildest. Gd sits between.

- Tc0 (pure RE-123, x→0): Er ≈ 95 K, Nd ≈ 90 K. Real literature: ErBa2Cu3O7 ≈ 92 K,
  GdBa2Cu3O7 ≈ 94 K, NdBa2Cu3O7 ≈ 92 K. So Gd's Tc0 ≈ 94 K, close to Er.
- Suppression slope m: Er ≈ 132, Nd ≈ 300. Gd between by ionic radius.
  Interpolating by ionic radius (Gd 1.06 between Er 1.00 and Nd 1.09):
  fraction = (1.06−1.00)/(1.09−1.00) = 0.06/0.09 ≈ 0.67 toward Nd.
  m_Gd ≈ 132 + 0.67*(300−132) = 132 + 0.67*168 = 132+112 = 244 K/x.
  Alternatively interpolate by slope directly: Gd closer to Nd-ish; use m_Gd ≈ 220.
- Zero crossing (x where Tc hits 0): Er ≈ 0.72, Nd ≈ 0.30. Gd between:
  x_c(Gd) ≈ 0.72 − 0.67*(0.72−0.30) = 0.72−0.28 = 0.44. Or by slope/Tc0 = 94/244 ≈ 0.39.

## Predicting Gd family

Use model: Tc(x) = max(0, Tc0_Gd − m_Gd·x), with Tc0_Gd ≈ 94, m_Gd ≈ 240.
Zero crossing x_c = 94/240 ≈ 0.39.

| x    | model Tc | predicted |
|------|----------|-----------|
| 0.030 | 94−7.2 = 86.8 | 86.8 |
| 0.078 | 94−18.7 = 75.3 | 75.3 |
| 0.150 | 94−36.0 = 58.0 | 58.0 |
| 0.200 | 94−48.0 = 46.0 | 46.0 |
| 0.300 | 94−72.0 = 22.0 | 22.0 |
| 0.400 | below x_c → ~0 | 0 |
| 0.500 | 0 | 0 |
| 0.600 | 0 | 0 |
| 0.700 | 0 | 0 |
| 0.800 | 0 | 0 |
| 0.900 | 0 | 0 |

Cross-check against the two real families at matching x:
- x=0.15: Er=82.9, Nd=40.7; Gd predicted 58.0 — sits between, ✓.
- x=0.20: Er=76.4, Nd=31.4; Gd 46.0 — between, ✓.
- x=0.30: Er=61.1, Nd=5.6; Gd 22.0 — between, ✓.
- x=0.40: Er=45.8 (still SC), Nd=0; Gd just at the edge → 0 (conservative;
  Gd zero-cross ~0.39, so at 0.40 it's essentially gone).
- x=0.50: Er=28.6, Nd=0; Gd 0.

The ordering Er > Gd > Nd at every x in the SC regime, and Gd's suppression
being intermediate by ionic radius, is physically consistent (Pr pair-breaking
strength scales with RE size). At high x all three go non-superconducting, with
Gd losing SC around x≈0.4, between Er (~0.72) and Nd (~0.30).

## Reasoning summary
Measured both synthesized families (Er, Nd) across the full Pr range. Tc falls
near-linearly with x then cuts to 0 past an RE-dependent critical x. Er (small
RE) suppresses mildly (slope ~132, dies ~x=0.72); Nd (large RE) suppresses
strongly (slope ~300, dies ~x=0.30). Gd's ionic radius lies between Er and Nd,
so its Tc0 (~94 K, near Er) and suppression slope (~240 K/x, between the two)
are interpolated; zero crossing ~x=0.39. Applied Tc=max(0, 94−240·x) per Gd point.
