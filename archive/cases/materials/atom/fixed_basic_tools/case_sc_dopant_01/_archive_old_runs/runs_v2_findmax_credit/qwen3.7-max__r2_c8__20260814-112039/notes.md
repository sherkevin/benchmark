# Notes — qwen3.7-max r2, 8-credit run

## Candidates overview
30 compositions: O₇Cu₃Ba₂Pr_x RE_(1-x), RE ∈ {Er, Gd, Nd}, x from 0.030 to 0.900.
Pr is a known Tc-suppressor in cuprates → expect lower Pr → higher Tc.

## Strategy
1. Measure across Pr-doping range to confirm suppression trend
2. Compare RE elements at same Pr level
3. Focus remaining budget on the best region

## Round 1: Establish trend (4 measurements, 4 credits used)
| Composition | Pr (x) | RE | Tc (K) |
|---|---|---|---|
| O7Cu3Ba2Pr0.030Er0.970 | 0.030 | Er | **93.9** |
| O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | Gd | 93.1 |
| O7Cu3Ba2Pr0.030Nd0.970 | 0.030 | Nd | 85.1 |
| O7Cu3Ba2Pr0.900Er0.100 | 0.900 | Er | 0.0 |

### Observations
- Pr suppression confirmed: x=0.900 → Tc=0
- At x=0.030: Er (93.9) > Gd (93.1) >> Nd (85.1)
- Nd surprisingly low — possibly different suppression curve or structural effect
- Er x=0.030 is the LOWEST Pr Er candidate and currently the best

## Round 2: Confirm trend at higher Pr (4 credits, all used)
| Composition | Pr (x) | RE | Tc (K) |
|---|---|---|---|
| O7Cu3Ba2Pr0.076Er0.924 | 0.076 | Er | 91.1 |
| O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | Gd | 84.0 |
| O7Cu3Ba2Pr0.150Er0.850 | 0.150 | Er | 82.9 |
| O7Cu3Ba2Pr0.200Nd0.800 | 0.200 | Nd | 31.4 |

## All 8 measurements compiled

| # | Composition | Pr (x) | RE | Tc (K) |
|---|---|---|---|---|
| 1 | O7Cu3Ba2Pr0.030Er0.970 | 0.030 | Er | **93.9** |
| 2 | O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | Gd | 93.1 |
| 3 | O7Cu3Ba2Pr0.030Nd0.970 | 0.030 | Nd | 85.1 |
| 4 | O7Cu3Ba2Pr0.076Er0.924 | 0.076 | Er | 91.1 |
| 5 | O7Cu3Ba2Pr0.078Gd0.922 | 0.078 | Gd | 84.0 |
| 6 | O7Cu3Ba2Pr0.150Er0.850 | 0.150 | Er | 82.9 |
| 7 | O7Cu3Ba2Pr0.200Nd0.800 | 0.200 | Nd | 31.4 |
| 8 | O7Cu3Ba2Pr0.900Er0.100 | 0.900 | Er | 0.0 |

## Pattern analysis

**Er series** (most resilient to Pr doping):
- x=0.030 → 93.9 K
- x=0.076 → 91.1 K (drop of 2.8 K for Δx=0.046)
- x=0.150 → 82.9 K (further drop)
- x=0.900 → 0.0 K (fully suppressed)
Monotonic decrease — classic Pr suppression in cuprates.

**Gd series**:
- x=0.030 → 93.1 K
- x=0.078 → 84.0 K (drop of 9.1 K for Δx≈0.048 — much steeper than Er!)
Gd starts close to Er at low Pr but falls off faster.

**Nd series**:
- x=0.030 → 85.1 K (already lower)
- x=0.200 → 31.4 K (steep suppression)
Nd is worst of the three — lower baseline + faster suppression.

**RE ranking for Tc resilience**: Er > Gd >> Nd

## Final decision
**O7Cu3Ba2Pr0.030Er0.970** with Tc = 93.9 K.

This is the lowest-Pr Er candidate. The monotonic Er-series trend means all higher-Pr Er candidates will have lower Tc. Gd at x=0.030 gives 93.1 (0.8 K lower). Nd is far behind. No unmeasured candidate can plausibly exceed 93.9 K.
