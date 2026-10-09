# Notes - Superconductor Tc Study

## Task Overview
- Budget: 18 units total (shared between search_db and measure_tc)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90K, Low = Tc < 90K

## Budget Tracking
Used: 18/18 (exhausted)
- Searches: 9 units (5 Ca pages + 3 Hg pages + 1 Tl page)
- Measurements: 9 units (9 successful; failed measurements cost 0)

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)
Found 12 series members via Ca search:
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92 | measured, HIGH |
| O6.100Ca1Cu2Ba2Hg1 | ? | not measured |
| O6.130Ca1Cu2Ba2Hg1 | ? | not measured |
| O6.140Ca1Cu2Ba2Hg1 | ? | not measured |
| O6.160Ca1Cu2Ba2Hg1 | - | UNMEASURABLE (no sample) |
| O6.190Ca1Cu2Ba2Hg1 | 122 | measured, HIGH |
| O6.210Ca1Cu2Ba2Hg1 | 127 | measured, HIGH |
| O6.220Ca1Cu2Ba2Hg1 | 127 | measured, HIGH |
| O6.240Ca1Cu2Ba2Hg1 | 123 | measured, HIGH |
| O6.270Ca1Cu2Ba2Hg1 | - | UNMEASURABLE (no sample) |
| O6.300Ca1Cu2Ba2Hg1 | 100 | measured, HIGH |
| O6.310Ca1Cu2Ba2Hg1 | 98 | measured, HIGH |

**Peak: T* = 127 K at O6.220Ca1Cu2Ba2Hg1** (tied with O6.210)
Classic parabolic doping behavior: Tc rises from 92K to 127K then falls to 98K.

## Step B: Family B (Hg + exactly 4 elements)
threshold1 = 127 - 32 = 95 K

4-element Hg materials (O-Cu-Ba-Hg) from Hg search pages 1-3:
| Composition | Tc (K) | Status |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95 | measured, HIGH (= threshold) |
| O4.066Cu1Ba2Hg1 | - | ERROR: "unknown composition" |
| O4.081Cu1Ba2Hg1 | 98 | measured, HIGH |
| O4.085Cu1Ba2Hg1 | ? | not measured (likely >= 95) |
| O4.096Cu1Ba2Hg1 | ? | not measured (likely >= 95) |
| O4.097Cu1Ba2Hg1 | ? | not measured (likely >= 95) |
| O4.101Cu1Ba2Hg1 | ? | not measured (likely >= 95) |
| O4.103Cu1Ba2Hg1 | ? | not measured (likely >= 95) |
| O4.150Cu1Ba2Hg1 | ? | not measured (borderline ~95) |

Q1 (confirmed >= 95): O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
Likely Q1 (predicted >= 95 based on parabolic trend): all of the above except possibly O4.150

## Step C: Unmeasurable Materials & Rule

### Confirmed unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 - "no sample exists"
2. O6.270Ca1Cu2Ba2Hg1 - "no sample exists"

### Rule induced:
All measurable materials in these cuprate families have Tc >= 90K (all HIGH).
The Tc follows a parabolic dependence on oxygen content within each homologous series.
Multi-factor rule: A material is HIGH if it contains Hg and has <=5 elements in a cuprate 
structure (Cu-O planes + alkaline earth). Within a known series, unmeasured members at 
intermediate oxygen positions follow the parabolic trend and are HIGH.
Single-factor rules fail because: (a) "has Hg" alone misses that element count matters, 
(b) "element count" alone misses the importance of Hg as charge reservoir.

### Predictions:
- O6.160Ca1Cu2Ba2Hg1: HIGH (interpolated ~114K, between O6.080@92K and O6.190@122K, parabolic fit gives ~121K)
- O6.270Ca1Cu2Ba2Hg1: HIGH (interpolated ~112K, between O6.240@123K and O6.300@100K, steeper decline on overdoped side)

## Step D: Final Pick
Among predicted-HIGH unmeasurables:
- O6.160Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~114-121K
- O6.270Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~112K

Both have 5 elements (tied). Tiebreaker = higher expected Tc.
O6.160 is on the underdoped side (gentler slope), O6.270 on overdoped side (steeper drop).
O6.160 has higher expected Tc.

**Final pick: O6.160Ca1Cu2Ba2Hg1**

## Other materials discovered (not measured, not confirmed unmeasurable):
- O10Ca2Cu3Ba2Tl2 (6 elements, no Hg)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements, no Hg, no Ba)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements, no Hg)
- O6Cu1Ba2Hg1Tl1 (5 elements, has Hg + Tl)
- O6Cu1Cd0.250Ba2Tl1.750 (5 elements, no Hg, has Cd)
