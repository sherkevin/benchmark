# Notes: Predict Tc curve of Gd(1-x)Pr(x)Ba2Cu3O7

## 1. Discovery (search_db)
- `search_db("Pr")` → 30 compositions: RE(1-x)Pr(x)Ba2Cu3O7 family for RE ∈ {Nd, Gd, Er}.
- `search_db("Gd")` → 11 Gd-series compositions (the target family, un-synthesized).
- Searched Y, Sm, Eu, Tb, Dy, Ho, Tm, Yb → all 0. Only Nd/Gd/Er exist.
- Lanthanide Z positions: **Nd=60, Gd=64, Er=68** → Gd is exactly the midpoint of Nd & Er.

## 2. Gd-series compositions to predict (x = Pr fraction)
0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900
(string form `O7Cu3Ba2Pr<x>Gd<1-x>`, 11 total)

## 3. Measured Tc (all via measure_tc; Gd returns sample_available:false)

### Nd series (Z=60) — falls fast with Pr
| x    | Tc(K) |
|------|-------|
| 0.030| 85.1  |
| 0.080| 68.2  |
| 0.150| 40.7  |
| 0.200| 31.4  |
| 0.300| 5.6   |
| 0.500| 0.0   |
| 0.700| 0.0   |
| 0.800| 0.0   |
| 0.900| 0.0   |

### Er series (Z=68) — falls slowly with Pr
| x    | Tc(K) |
|------|-------|
| 0.030| 93.9  |
| 0.076| 91.1  |
| 0.150| 82.9  |
| 0.200| 76.4  |
| 0.300| 61.1  |
| 0.400| 45.8  |
| 0.500| 28.6  |
| 0.800| 0.0   |
| 0.900| 0.0   |

Both series → 0 at high Pr (x≥0.8 for Er, x≥0.5 for Nd). Nd is suppressed faster (gap Er−Nd
grows from 8.8 K at x=0.03 to ~55 K at x=0.3, then shrinks as both →0).

## 4. Model / extrapolation to Gd
- Lanthanide contraction ⇒ ionic radius r_RE is ~linear in Z across Nd→Er.
- Tc of R-123 cuprates varies ~linearly with r_RE over a small range (only ~0.1 Å here).
- With only two bracketing series (Nd, Er), the only defensible Z-interpolation is linear.
- Gd Z=64 = midpoint (64−60)/(68−60)=0.5 ⇒ **Gd(x) = ½[Nd(x) + Er(x)]**.
- Nd(x), Er(x) at the Gd x-values obtained by **piecewise-linear interpolation** within each
  measured series (no overfitting; respects the measured knots). Values clamped ≥0.

## 5. Gd prediction table (Tc in K)
| x     | Nd(x)  | Er(x)  | Gd = avg |
|-------|--------|--------|----------|
| 0.030 | 85.1   | 93.9   | 89.5     |
| 0.078 | 68.876 | 90.878 | 79.9     |
| 0.150 | 40.7   | 82.9   | 61.8     |
| 0.200 | 31.4   | 76.4   | 53.9     |
| 0.300 | 5.6    | 61.1   | 33.4     |
| 0.400 | 2.8    | 45.8   | 24.3     |
| 0.500 | 0.0    | 28.6   | 14.3     |
| 0.600 | 0.0    | 19.07  | 9.5      |
| 0.700 | 0.0    | 9.53   | 4.8      |
| 0.800 | 0.0    | 0.0    | 0        |
| 0.900 | 0.0    | 0.0    | 0        |

Interpolation detail: Nd(0.078)=85.1+0.96·(68.2−85.1)=68.876; Er(0.078)=91.1+0.0270·(82.9−91.1)=90.878;
Nd(0.400)=5.6+0.5·(0−5.6)=2.8; Er(0.600)=28.6+(0.1/0.3)·(0−28.6)=19.07; Er(0.700)=28.6+(0.2/0.3)·(0−28.6)=9.53.

Predicted Gd curve is monotonically decreasing (physically required) and sits between the Nd
and Er curves at every x, as expected for a Z-midpoint member. Use 0 for x=0.800, 0.900
(no longer superconducting — both bracketing series already 0 there).
