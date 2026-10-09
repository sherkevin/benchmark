# Superconductor Tc Study - Notes

## Budget Tracking
Total budget: 18 units (shared between search_db and measure_tc)
Used: 18 (9 searches + 9 measurements attempted, but 1 was no-sample so 8 paid measurements + 1 no-sample = 9 measurement attempts... wait, the no-sample one didn't cost? Let me check.)

Actually looking at the results, O6.160 didn't increment budget. So:
- Searches: 9 (Hg pages 1-6, Ca pages 1-5)
- Measurements: 8 budget units (O6.080, O6.100, O6.220, O6.300, O6.190, O6.240, O4.081, and the no-sample O6.160 didn't cost)
Wait, actually the budget went from 12 to 12 after O6.160 (no sample). Let me recount:
Budget 1: measure O6.080 → 92.0
Budget 2: search Hg page 1
Budget 3: search Hg page 2 + search Ca page 1 (both at budget 3-4)
Budget 4: search Ca page 1
Budget 5: search Ca page 2 + search Hg page 3 (budget 5-6)
Budget 6: search Hg page 3
Budget 7: search Ca page 3
Budget 8: search Ca page 4
Budget 9: search Ca page 5
Budget 10: measure O6.100 → 92.5
Budget 11: measure O6.220 → 127.0
Budget 12: measure O6.300 → 100.0
O6.160: no sample, no cost (stayed at 12)
Budget 13: measure O6.190 → 122.0
Budget 14: measure O6.240 → 123.0
Budget 15: search Hg page 4
Budget 16: measure O4.081 → 98.0
Budget 17: search Hg page 5
Budget 18: search Hg page 6

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)

### Series Members Found:
1. O6.080Ca1Cu2Ba2Hg1 → Tc = 92.0 K (measured)
2. O6.100Ca1Cu2Ba2Hg1 → Tc = 92.5 K (measured)
3. O6.130Ca1Cu2Ba2Hg1 → not measured
4. O6.140Ca1Cu2Ba2Hg1 → not measured
5. O6.160Ca1Cu2Ba2Hg1 → NO SAMPLE (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 → Tc = 122.0 K (measured)
7. O6.210Ca1Cu2Ba2Hg1 → not measured
8. O6.220Ca1Cu2Ba2Hg1 → Tc = 127.0 K (measured) ← PEAK
9. O6.240Ca1Cu2Ba2Hg1 → Tc = 123.0 K (measured)
10. O6.270Ca1Cu2Ba2Hg1 → not measured
11. O6.300Ca1Cu2Ba2Hg1 → Tc = 100.0 K (measured)
12. O6.310Ca1Cu2Ba2Hg1 → not measured

### Tc vs Oxygen trend:
O6.080: 92.0 → O6.100: 92.5 → ... → O6.190: 122.0 → O6.220: 127.0 → O6.240: 123.0 → ... → O6.300: 100.0

Peak: O6.220Ca1Cu2Ba2Hg1 with T* = 127.0 K

## Step B: 4-element Hg materials (O, Cu, Ba, Hg)

threshold1 = T* - 32 = 127 - 32 = 95

### 4-element Hg materials found:
Page 1: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
Page 2: O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
Page 3: O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1
Page 4: O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
Page 5: O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

Only measured: O4.081Cu1Ba2Hg1 → Tc = 98.0 K (≥ 95, qualifies)

Q1 = ["O4.081Cu1Ba2Hg1"]

## Step C: Unmeasurable Materials

Confirmed unmeasurable:
- O6.160Ca1Cu2Ba2Hg1 (tried measuring, got "no sample exists")

There might be more unmeasurable materials among those I found but didn't try to measure, but I've exhausted my budget.

## Rule Induction

From measured data:
- 5-element series (HgBa2CaCu2O6+δ): All measured members have Tc ≥ 90 K (range 92-127 K)
- 4-element series (HgBa2CuO4+δ): O4.081 has Tc = 98 K

Multi-factor rule: A cuprate superconductor is "high" (Tc ≥ 90 K) if:
- It has 2+ CuO2 planes (5+ elements including Ca) → always high regardless of doping
- It has 1 CuO2 plane (4 elements, no Ca) → high only near optimal doping (δ ≈ 0.06-0.12)

## Step D: Final Pick

Among unmeasurable materials predicted high, pick fewest elements:
- O6.160Ca1Cu2Ba2Hg1 (5 elements, predicted high ~112 K)

Final = O6.160Ca1Cu2Ba2Hg1

## Non-Hg materials found (from Ca search):
- O10Ca2Cu3Ba2Tl2 (5 elements: O, Ca, Cu, Ba, Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O, Ca, Cu, Sr, Tl, Pb)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O, Ca, Cu, Ba, Tl)

## Hg search pages 7-9 not explored (budget exhausted)
There are 9 more Hg-containing materials I haven't seen.
