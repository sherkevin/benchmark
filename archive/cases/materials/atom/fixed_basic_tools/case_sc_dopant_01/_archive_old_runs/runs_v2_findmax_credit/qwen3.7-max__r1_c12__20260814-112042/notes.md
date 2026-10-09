# Measurement Notes — case_sc_dopant_01

## Domain Analysis

These are O7Cu3Ba2-based cuprate superconductors with Pr doping, co-doped with Er, Gd, or Nd.
- Base: YBa2Cu3O7-type (YBCO family), Pr substitutes for the rare-earth site
- Pr is well-known to **suppress Tc** in cuprates (pair-breaking effect)
- Lower Pr content → expected higher Tc
- Three co-dopants: Er³⁺ (0.89 Å), Gd³⁺ (0.94 Å), Nd³⁺ (0.98 Å) — different ionic radii cause different lattice strain

## Strategy
1. **Phase 1 (6 credits)**: Spread across Pr concentrations and RE types to map the landscape
2. **Phase 2 (6 credits)**: Zoom into the highest-Tc region identified in Phase 1

## Phase 1 Measurements (6 credits used)

| # | Composition | Tc (K) | Notes |
|---|-------------|--------|-------|
| 1 | O7Cu3Ba2Pr0.030Er0.970 | 93.9 | **Highest!** Lowest Pr + Er |
| 2 | O7Cu3Ba2Pr0.030Nd0.970 | 85.1 | Same Pr, Nd → ~9K lower than Er |
| 3 | O7Cu3Ba2Pr0.150Gd0.850 | 67.9 | Higher Pr → lower Tc |
| 4 | O7Cu3Ba2Pr0.300Er0.700 | 61.1 | Even with Er, high Pr kills Tc |
| 5 | O7Cu3Ba2Pr0.500Gd0.500 | 0.0 | Superconductivity destroyed |
| 6 | O7Cu3Ba2Pr0.900Nd0.100 | 0.0 | Fully suppressed |

### Key findings:
- Pr strongly suppresses Tc (confirmed pair-breaking). At Pr≥0.5, Tc = 0.
- Er gives ~9K higher Tc than Nd at same Pr=0.03
- The sweet spot is **lowest Pr + Er dopant**

## Phase 2 Measurements (credits 7–12)

| # | Composition | Tc (K) | Notes |
|---|-------------|--------|-------|
| 7 | O7Cu3Ba2Pr0.030Gd0.970 | 93.1 | Gd at lowest Pr, close to Er |
| 8 | O7Cu3Ba2Pr0.076Er0.924 | 91.1 | Er at Pr=0.076, ~3K drop from Pr=0.03 |
| 9 | O7Cu3Ba2Pr0.078Gd0.922 | 84.0 | Gd at Pr=0.078 |
| 10 | O7Cu3Ba2Pr0.080Nd0.920 | 68.2 | Nd at Pr=0.08, much worse |
| 11 | O7Cu3Ba2Pr0.150Er0.850 | 82.9 | Confirms monotonic Er decline |
| 12 | O7Cu3Ba2Pr0.200Er0.800 | 76.4 | Further decline confirmed |

## Complete Data Summary (12/30 measured)

### Er series (monotonically decreasing with Pr):
- Pr=0.030 → 93.9 K
- Pr=0.076 → 91.1 K
- Pr=0.150 → 82.9 K
- Pr=0.200 → 76.4 K
- Pr=0.300 → 61.1 K

### RE comparison at Pr≈0.03:
- Er: 93.9 K > Gd: 93.1 K > Nd: 85.1 K

### RE comparison at Pr≈0.08:
- Er: 91.1 K > Gd: 84.0 K > Nd: 68.2 K

### Physics interpretation:
1. Pr is a pair-breaking dopant — monotonically suppresses Tc
2. Er³⁺ (smallest ionic radius, 0.89 Å) causes least lattice strain → highest Tc
3. Nd³⁺ (largest, 0.98 Å) causes most strain → worst Tc
4. At Pr≥0.5, superconductivity is fully destroyed (Tc=0)

## Conclusion

**O7Cu3Ba2Pr0.030Er0.970** has the highest measured Tc at **93.9 K**.
This is the combination of lowest available Pr doping (0.030) with the best-performing
rare-earth co-dopant (Er). The monotonic decrease in Tc with increasing Pr across all
RE dopants, and Er's consistent superiority, make it extremely unlikely any unmeasured
candidate could exceed this value.
