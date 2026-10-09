# Measurement Notes — case_sc_dopant_01

## Composition Space Analysis
- All candidates: O7Cu3Ba2Pr_x RE_(1-x)
- Pr doping levels (x): 0.030, 0.076-080, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900
- Rare earth elements: Er, Gd, Nd
- Physics prior: Pr doping in cuprates typically suppresses Tc → lower Pr = higher Tc expected
- Strategy: sample broadly at low/mid/high Pr, then zoom in

## Measurements

### Round 1 (3 credits): Broad sampling at low Pr across all 3 rare earths
| # | Composition | Tc (K) |
|---|-------------|--------|
| 1 | O7Cu3Ba2Pr0.030Er0.970 | 93.9 |
| 2 | O7Cu3Ba2Pr0.030Gd0.970 | 93.1 |
| 3 | O7Cu3Ba2Pr0.030Nd0.970 | 85.1 |

**Observation**: Er > Gd >> Nd at lowest Pr. Er leads with 93.9 K.

### Round 2 (3 credits): Map Tc decay with increasing Pr for Er and Gd
| # | Composition | Tc (K) |
|---|-------------|--------|
| 4 | O7Cu3Ba2Pr0.150Er0.850 | 82.9 |
| 5 | O7Cu3Ba2Pr0.300Er0.700 | 61.1 |
| 6 | O7Cu3Ba2Pr0.150Gd0.850 | 67.9 |

**Observation**: Tc drops monotonically with Pr. Er decays slower than Gd (-11 vs -25 K at Pr=0.15).
Er Pr=0.030 remains the leader at 93.9 K.

### Round 3 (2 credits): Confirm the monotonic trend at Pr=0.076 and Pr=0.500
| # | Composition | Tc (K) |
|---|-------------|--------|
| 7 | O7Cu3Ba2Pr0.076Er0.924 | 91.1 |
| 8 | O7Cu3Ba2Pr0.500Er0.500 | 28.6 |

**Observation**: Er at Pr=0.076 gives 91.1 K — fits perfectly between Pr=0.030 (93.9) and Pr=0.150 (82.9). Er at Pr=0.500 drops to 28.6 K. Monotonic decay fully confirmed.

---

## Summary

### All measurements (8/8 credits used)

| # | Composition | Pr (x) | RE | Tc (K) |
|---|-------------|--------|-----|--------|
| 1 | O7Cu3Ba2Pr0.030Er0.970 | 0.030 | Er | **93.9** |
| 2 | O7Cu3Ba2Pr0.030Gd0.970 | 0.030 | Gd | 93.1 |
| 3 | O7Cu3Ba2Pr0.030Nd0.970 | 0.030 | Nd | 85.1 |
| 4 | O7Cu3Ba2Pr0.076Er0.924 | 0.076 | Er | 91.1 |
| 5 | O7Cu3Ba2Pr0.150Er0.850 | 0.150 | Er | 82.9 |
| 6 | O7Cu3Ba2Pr0.150Gd0.850 | 0.150 | Gd | 67.9 |
| 7 | O7Cu3Ba2Pr0.300Er0.700 | 0.300 | Er | 61.1 |
| 8 | O7Cu3Ba2Pr0.500Er0.500 | 0.500 | Er | 28.6 |

### Patterns discovered
1. **Monotonic Tc decay with Pr doping**: Tc decreases steadily as Pr content increases. This is consistent with known physics — Pr doping in cuprates suppresses superconductivity.
2. **Rare earth ranking**: Er > Gd >> Nd at the same Pr level (e.g., at Pr=0.030: Er=93.9, Gd=93.1, Nd=85.1; at Pr=0.150: Er=82.9, Gd=67.9).
3. **Er decays slowest**: Er maintains higher Tc across all Pr levels compared to Gd.

### Final answer
**O7Cu3Ba2Pr0.030Er0.970** with Tc = **93.9 K**

This is the lowest-Pr, Er-doped composition — the point in composition space with minimal Tc-suppressing Pr doping and the most favorable rare earth element. The monotonic decay pattern strongly implies that no unmeasured composition can exceed this value, as all other candidates have higher Pr content.
