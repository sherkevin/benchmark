# Notes: Predicting Tc of Gd(1-x)Pr(x)Ba2Cu3O7

## Database Discovery

Searched `search_db` for rare earth elements. Found three RE series in `RE(1-x)Pr(x)Ba2Cu3O7`:
- **Gd series** (11 compositions) — NOT synthesized, target for prediction
- **Nd series** (11 compositions, one invalid: Pr0.400Nd0.600) — synthesized, measurable
- **Er series** (9 compositions) — synthesized, measurable

No Y, La, Sm, Eu, Dy, or other RE variants exist in the database.

## Measured Data

### Nd series: Nd(1-x)Pr(x)Ba2Cu3O7

| Pr (x) | Tc (K) |
|---------|--------|
| 0.030   | 85.1   |
| 0.080   | 68.2   |
| 0.150   | 40.7   |
| 0.200   | 31.4   |
| 0.300   | 5.6    |
| 0.400   | (invalid composition) |
| 0.500   | 0.0    |
| 0.600   | 0.0    |
| 0.700   | 0.0    |
| 0.800   | 0.0    |
| 0.900   | 0.0    |

**Key observation**: Nd series Tc drops sharply and reaches 0 at x ≈ 0.33–0.50.

### Er series: Er(1-x)Pr(x)Ba2Cu3O7

| Pr (x) | Tc (K) |
|---------|--------|
| 0.030   | 93.9   |
| 0.076   | 91.1   |
| 0.150   | 82.9   |
| 0.200   | 76.4   |
| 0.300   | 61.1   |
| 0.400   | 45.8   |
| 0.500   | 28.6   |
| 0.800   | 0.0    |
| 0.900   | 0.0    |

**Key observation**: Er series Tc declines much more gradually; superconductivity persists to higher Pr content.

### Gd series: confirmed not synthesized
`measure_tc("O7Cu3Ba2Pr0.030Gd0.970")` → `sample_available: false`

## Pattern Analysis

### Physics context
Pr substitution in REBa2Cu3O7 suppresses Tc via hole localization and pair-breaking. The suppression rate depends on the host RE ion — specifically its ionic radius and electronic structure. Larger RE ions (like Nd) show faster Tc suppression; smaller RE ions (like Er) show slower suppression.

### Ionic radii (VIII coordination, Shannon 1976)
- Nd³⁺: 1.109 Å
- Gd³⁺: 1.053 Å
- Er³⁺: 0.890 Å

Gd sits between Nd and Er, closer to the midpoint (fraction α = 0.522 from Nd toward Er).

### Linear model fits

**Nd series** (using 5 nonzero points, x = 0.03–0.30):
- Linear fit: Tc(x) = 95.3 − 295.2x
- Critical Pr fraction (Tc→0): x_c ≈ 0.323

**Er series** (using 7 nonzero points, x = 0.03–0.50):
- Linear fit: Tc(x) = 103.2 − 152.5x
- Critical Pr fraction (Tc→0): x_c ≈ 0.677

### Gd interpolation

Position parameter α = (1.109 − 1.053) / (1.109 − 0.890) = 0.522

Interpolated Gd model:
- Tc₀ = 0.478 × 95.3 + 0.522 × 103.2 = **99.4 K**
- slope = 0.478 × (−295.2) + 0.522 × (−152.5) = **−220.8 K/x**
- x_c = 99.4 / 220.8 ≈ **0.450**

Model: **Tc(x) = max(0, 99.4 − 220.8·x)**

## Predictions for Gd series

| Composition | Pr (x) | Predicted Tc (K) |
|---|---|---|
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | 92.8 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | 82.2 |
| O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | 66.3 |
| O7Cu3Ba2Pr0.200Gd0.800 | 0.200 | 55.2 |
| O7Cu3Ba2Pr0.300Gd0.700 | 0.300 | 33.2 |
| O7Cu3Ba2Pr0.400Gd0.600 | 0.400 | 11.1 |
| O7Cu3Ba2Pr0.500Gd0.500 | 0.500 | 0 |
| O7Cu3Ba2Pr0.600Gd0.400 | 0.600 | 0 |
| O7Cu3Ba2Pr0.700Gd0.300 | 0.700 | 0 |
| O7Cu3Ba2Pr0.800Gd0.200 | 0.800 | 0 |
| O7Cu3Ba2Pr0.900Gd0.100 | 0.900 | 0 |

**Sanity check**: These predictions correctly fall between the Nd and Er curves at every Pr fraction, and the critical Pr content (~0.45) is between Nd (0.32) and Er (0.68), consistent with Gd's intermediate ionic radius.
