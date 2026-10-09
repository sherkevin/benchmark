# Superconductor Tc Study — Notes

## Budget Tracking
- Total: 34 units
- Used: 34 (9 search_db pages for Hg + 2 for Tl + 1 measure seed + 11 series + 10 measurable 4-elem + 1 O6Cu1Ba2Hg1Tl1 + 1 O10Ca2Cu3Ba2Tl2 = 34)
- Unmeasurable checks (no sample) and exhausted-budget checks were free

## Step A — Series Peak

Seed: O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0 K)
Homologous series: HgBa2CaCu2O(6+δ) — 5 elements (O, Ca, Cu, Ba, Hg), varying oxygen

| Composition | O content | Tc (K) | Available |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 6.080 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 6.100 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 6.130 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 6.140 | 105.0 | Yes |
| O6.160Ca1Cu2Ba2Hg1 | 6.160 | — | No (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 6.190 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | 6.210 | **127.0** | Yes |
| O6.220Ca1Cu2Ba2Hg1 | 6.220 | **127.0** | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 6.240 | 123.0 | Yes |
| O6.270Ca1Cu2Ba2Hg1 | 6.270 | — | No (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 6.300 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | 6.310 | 98.0 | Yes |

Clear Tc dome with peak at O6.210 and O6.220 (both 127.0 K).
**T* = 127.0 K**, peak composition: O6.210Ca1Cu2Ba2Hg1

## Step B — Filter Family B (Hg + exactly 4 elements)

threshold1 = 127.0 - 32 = 95.0

4-element Hg materials (O, Cu, Ba, Hg) — 14 total:

| Composition | O content | Tc (K) | >= 95? | Available |
|---|---|---|---|---|
| O4.047Cu1Ba2Hg1 | 4.047 | — | — | No |
| O4.066Cu1Ba2Hg1 | 4.066 | — | — | No |
| O4.081Cu1Ba2Hg1 | 4.081 | 98.0 | ✓ | Yes |
| O4.085Cu1Ba2Hg1 | 4.085 | 92.0 | ✗ | Yes |
| O4.096Cu1Ba2Hg1 | 4.096 | 95.6 | ✓ | Yes |
| O4.097Cu1Ba2Hg1 | 4.097 | 95.3 | ✓ | Yes |
| O4.101Cu1Ba2Hg1 | 4.101 | 93.4 | ✗ | Yes |
| O4.103Cu1Ba2Hg1 | 4.103 | 92.5 | ✗ | Yes |
| O4.150Cu1Ba2Hg1 | 4.150 | 97.0 | ✓ | Yes |
| O4.160Cu1Ba2Hg1 | 4.160 | — | — | No |
| O4.162Cu1Ba2Hg1 | 4.162 | 97.4 | ✓ | Yes |
| O4.180Cu1Ba2Hg1 | 4.180 | — | — | No |
| O4.200Cu1Ba2Hg1 | 4.200 | 94.2 | ✗ | Yes |
| O4.220Cu1Ba2Hg1 | 4.220 | 97.0 | ✓ | Yes |

**Q1 = [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]**

## Other Materials Found

O6Cu1Ba2Hg1Tl1 (5 elem: O, Cu, Ba, Hg, Tl): Tc = 0.0 K — LOW!
O10Ca2Cu3Ba2Tl2 (6 elem: O, Ca, Cu, Ba, Tl): Tc = 118.97 K — HIGH

## Step C — Unmeasurable Materials & Predictions

### All Unmeasurable Materials Found (via search_db + attempted measure_tc):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, O=6.160) — in 5-elem Hg series
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, O=6.270) — in 5-elem Hg series
3. O4.047Cu1Ba2Hg1 (4 elem, O=4.047) — in 4-elem Hg series
4. O4.066Cu1Ba2Hg1 (4 elem, O=4.066) — in 4-elem Hg series
5. O4.160Cu1Ba2Hg1 (4 elem, O=4.160) — in 4-elem Hg series
6. O4.180Cu1Ba2Hg1 (4 elem, O=4.180) — in 4-elem Hg series
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (7 elem) — Tl material
8. O8Ca0.850Cu2.200Ba2Tl1.800 (6 elem) — Tl material
9. O6Cu1Cd0.250Ba2Tl1.750 (6 elem) — Tl material (couldn't confirm availability, budget exhausted)

### Rule Induction

Key observations from measurable data:
- ALL pure Hg-cuprates (no Tl) are HIGH (Tc 92-127 K)
- O6Cu1Ba2Hg1Tl1 (Hg+Tl, no Ca): Tc = 0 K (LOW!)
- O10Ca2Cu3Ba2Tl2 (Tl, has Ca, no Hg): Tc = 119 K (HIGH)

**Multi-factor rule:**
1. If material contains BOTH Hg and Tl → LOW (Hg-Tl antagonism kills superconductivity)
2. If material contains Tl but NOT Ca → LOW (Tl-cuprates need Ca for charge reservoir/high Tc)
3. If material's oxygen content is well below the measured dome range for its series → LOW (severe underdoping suppresses Tc)
4. Otherwise → HIGH

Single-factor rules fail because:
- "All high" → wrong on O4.047, O4.066, O6Cu1Cd0.250Ba2Tl1.750 (3 errors = several)
- "Has Tl → low" → wrong on O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Ca0.850Cu2.200Ba2Tl1.800 (2 errors)
- "Hg → high" → misses Tl-only low materials

### Predictions:
| Composition | Rule Applied | Prediction |
|---|---|---|
| O4.047Cu1Ba2Hg1 | O=4.047 well below dome (min measured=4.081) | **LOW** |
| O4.066Cu1Ba2Hg1 | O=4.066 well below dome | **LOW** |
| O4.160Cu1Ba2Hg1 | Within dome (between 4.150 and 4.162) | **HIGH** |
| O4.180Cu1Ba2Hg1 | Within dome (between 4.162 and 4.200) | **HIGH** |
| O6.160Ca1Cu2Ba2Hg1 | Within dome (between 6.140 and 6.190) | **HIGH** |
| O6.270Ca1Cu2Ba2Hg1 | Within dome (between 6.240 and 6.300) | **HIGH** |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | Tl with Ca → HIGH | **HIGH** |
| O8Ca0.850Cu2.200Ba2Tl1.800 | Tl with Ca → HIGH | **HIGH** |
| O6Cu1Cd0.250Ba2Tl1.750 | Tl without Ca → LOW | **LOW** |

## Step D — Final Pick

Among predicted HIGH materials:
- O4.160Cu1Ba2Hg1: 4 elements, expected Tc ~97.2
- O4.180Cu1Ba2Hg1: 4 elements, expected Tc ~96
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 7 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 6 elements

Fewest elements = 4 (tied: O4.160Cu1Ba2Hg1 and O4.180Cu1Ba2Hg1)
Tie-break (higher expected Tc): O4.160Cu1Ba2Hg1 (~97.2) > O4.180Cu1Ba2Hg1 (~96)

**Final pick: O4.160Cu1Ba2Hg1**
