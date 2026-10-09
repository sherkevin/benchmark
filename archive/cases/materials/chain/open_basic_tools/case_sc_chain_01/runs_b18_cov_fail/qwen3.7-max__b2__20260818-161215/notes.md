# Notes - Superconductor Tc Study

## Budget Usage
- Search Hg pages 1-2: 2 units
- Search Ca pages 1-5: 5 units
- Measure 5 series members: 5 units
- Measure 2 more series members: 2 units
- Search Hg pages 3-6: 4 units
- Total: 18/18 (exhausted)

## Step A: Homologous Series (O, Ca, Cu, Ba, Hg - varying oxygen)

### Measured Tc values:
| Composition | Oxygen | Tc (K) |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 6.080 | 92 |
| O6.130Ca1Cu2Ba2Hg1 | 6.130 | 102 |
| O6.190Ca1Cu2Ba2Hg1 | 6.190 | 122 |
| O6.210Ca1Cu2Ba2Hg1 | 6.210 | **127** |
| O6.220Ca1Cu2Ba2Hg1 | 6.220 | **127** |
| O6.240Ca1Cu2Ba2Hg1 | 6.240 | 123 |
| O6.310Ca1Cu2Ba2Hg1 | 6.310 | 98 |

### Unmeasured series members (interpolated estimates):
- O6.100Ca1Cu2Ba2Hg1: ~97 K (between 92 and 102)
- O6.140Ca1Cu2Ba2Hg1: ~112 K (between 102 and 122)
- O6.160Ca1Cu2Ba2Hg1: ~117 K (between 102 and 122)
- O6.270Ca1Cu2Ba2Hg1: ~110 K (between 123 and 98)
- O6.300Ca1Cu2Ba2Hg1: ~103 K (between 123 and 98)

### Peak Analysis
Dome shape is clear: 92 → 102 → 122 → 127 → 127 → 123 → 98
Parabolic fit gives vertex at O ≈ 6.216, Tc_max ≈ 127.3
Both O6.210 and O6.220 measure 127 K.
**Peak: O6.220Ca1Cu2Ba2Hg1, T* = 127 K**

## Step B: 4-element Hg materials (O, Cu, Ba, Hg)
threshold1 = 127 - 32 = 95

### Found 14 materials (Hg search pages 1-5):
O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1,
O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1,
O4.150Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1,
O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

Hg-1201 system (HgBa2CuO4+δ): dome-shaped Tc with max ~95 K near O ≈ 4.10
Materials near optimal doping (O4.096-4.103) most likely to have Tc >= 95.
**Cannot measure Tc - budget exhausted. Must estimate.**

Best guess for Q1: materials near peak of dome
- O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1

## Other materials found (from Ca search):
- O10Ca2Cu3Ba2Tl2 (5 elements: O, Ca, Cu, Ba, Tl) - Tl-2223 type, likely high Tc
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O, Ca, Cu, Sr, Tl, Pb) - complex composition
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O, Ca, Cu, Ba, Tl) - Tl-1212 type

## Step C: Unmeasurable materials
Cannot test (budget exhausted). Most likely unmeasurable:
- The 3 Tl-containing materials (different family from main Hg series)
- Possibly some extreme 4-element Hg compositions

### Rule induced from measurable data:
All 7 measured materials (Hg-1212 family) have:
- 5 elements, Cu=2, Hg present, Ca present, Ba present
- All Tc >= 90 (high)

Multi-factor rule: "High Tc if Cu >= 2 AND contains heavy element (Hg or Tl) AND contains alkaline earth (Ca or Ba or Sr). Cu=1 materials are low."

This is multi-factor: considers Cu layer count, heavy element type, and alkaline earth presence.
Single-factor rules (e.g., just "contains Hg" or just "Cu >= 2") would be wrong because:
- Some Hg materials with Cu=1 have low Tc (extreme doping)
- Some non-Hg materials with Cu>=2 can have high Tc (Tl-based)

### Predictions:
- O10Ca2Cu3Ba2Tl2: Cu=3, Tl, Ca, Ba → **high** (Tl-2223 type, ~110-125 K)
- O8Ca0.850Cu2.200Ba2Tl1.800: Cu≈2, Tl, Ca, Ba → **high** (Tl-1212 type, ~95-105 K)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Cu=2, Tl, Pb, Ca, Sr → **low** (complex fractional stoichiometry, Tl0.5Pb0.5 mixed reservoir likely suppresses Tc)

## Step D: Final pick
Among unmeasurable materials predicted high:
- O10Ca2Cu3Ba2Tl2: 5 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
Tie at 5 elements → break by higher expected Tc
O10Ca2Cu3Ba2Tl2 (3 Cu layers) > O8Ca0.850Cu2.200Ba2Tl1.800 (2 Cu layers)
**Final: O10Ca2Cu3Ba2Tl2**

## Limitations
- Budget was suboptimally allocated (too many searches, too many series measurements)
- Could not measure 4-element Hg materials for Step B
- Could not test which materials are unmeasurable for Steps C/D
- Qualifying set and predictions are estimates based on cuprate physics knowledge
