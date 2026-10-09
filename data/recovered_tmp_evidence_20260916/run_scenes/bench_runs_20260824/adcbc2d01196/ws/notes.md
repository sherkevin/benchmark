# Task: Predict Tc of Gd1-xPrxBa2Cu3O7 series (not yet synthesized)

## Database discovery
`search_db` shows exactly 30 compositions across 3 rare-earth series (no Y/Sm/Eu/Yb/Dy/Ho):
- **Er series** (synthesized, measured): x = 0.030, 0.076, 0.150, 0.200, 0.300, 0.400, 0.500, 0.800, 0.900
- **Nd series** (synthesized, measured): x = 0.030, 0.080, 0.150, 0.200, 0.300, 0.500, 0.600, 0.700, 0.800, 0.900
- **Gd series** (NOT synthesized — confirmed `sample_available: false` for 0.030 and 0.500): 11 compositions to predict at x = 0.030, 0.078, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900

## Measured Tc (K) — all from real measure_tc calls
### Er series (RE radius r=1.004 Å)
| x    | Tc   |
|------|------|
| 0.030| 93.9 |
| 0.076| 91.1 |
| 0.150| 82.9 |
| 0.200| 76.4 |
| 0.300| 61.1 |
| 0.400| 45.8 |
| 0.500| 28.6 |
| 0.800|  0.0 |
| 0.900|  0.0 |
Linear fit (nonzero pts): Tc ≈ -141.4·x + 102.0, zero-crossing ≈ x0.72.

### Nd series (r=1.109 Å)
| x    | Tc   |
|------|------|
| 0.030| 85.1 |
| 0.080| 68.2 |
| 0.150| 40.7 |
| 0.200| 31.4 |
| 0.300|  5.6 |
| 0.500|  0.0 |
| 0.600|  0.0 |
| 0.700|  0.0 |
| 0.800|  0.0 |
| 0.900|  0.0 |
Linear fit (nonzero pts): Tc ≈ -294.4·x + 91.0, zero-crossing ≈ x0.31.

## Physical pattern & extrapolation method
- Pr substitution suppresses Tc (Pr-4f / Cu-O-2p hybridization destroys superconductivity; pure Pr member Tc=0).
- Suppression rate scales with RE ionic radius: Nd (largest) suppresses fastest (~-294 K/x), Er (smallest) slowest (~-141 K/x). Gd (r=1.053 Å) is intermediate.
- Gd fractional position Nd→Er by ionic radius: f = (r_Gd-r_Nd)/(r_Er-r_Nd) = (1.053-1.109)/(1.004-1.109) = 0.5333. So Gd ≈ 0.4667·Nd + 0.5333·Er (slightly more Er-like).
- Method: at each x, take Er Tc and Nd Tc **clipped to ≥0** (using measured values; linear-interpolate the boundary curve between measured points where the needed x wasn't measured), then interpolate in ionic radius. Clip-then-mix respects that a dead RE (Tc=0, measured) is genuinely 0, so Gd tracks the still-superconducting boundary scaled by radius weight.

## Gd predictions (K)
| x    | Er used | Nd used | Gd = 0.4667·Nd + 0.5333·Er |
|------|---------|---------|---------------------------|
| 0.030| 93.9 (m)| 85.1 (m)| 89.8 |
| 0.078| 91.1 (m, x=0.076) | 68.2 (m, x=0.080) | 80.4 |
| 0.150| 82.9 (m)| 40.7 (m)| 63.2 |
| 0.200| 76.4 (m)| 31.4 (m)| 55.4 |
| 0.300| 61.1 (m)|  5.6 (m)| 35.2 |
| 0.400| 45.8 (m)|  2.8 (interp 0.3→0.5) | 25.7 |
| 0.500| 28.6 (m)|  0.0 (m)| 15.3 |
| 0.600| 19.1 (interp 0.5→0.8) | 0.0 | 10.2 |
| 0.700|  9.5 (interp 0.5→0.8) | 0.0 |  5.1 |
| 0.800|  0.0 (m)|  0.0 (m)|  0.0 |
| 0.900|  0.0 (m)|  0.0 (m)|  0.0 |

m = measured directly; interp = linear interpolation between measured bracketing points of that boundary series.

## Sanity checks
- Monotonic decreasing in x ✓
- At every shared x, Nd < Gd < Er ✓ (correct physical ordering by ionic radius)
- Smooth curve, zero-crossing ~x0.75 (between Nd's ~0.32 and Er's ~0.70... Gd dies slightly later than Er because once Nd=0 Gd just tracks scaled Er; Er's measured tail 0.5→0.8 drives Gd to 0 by x=0.8) ✓
