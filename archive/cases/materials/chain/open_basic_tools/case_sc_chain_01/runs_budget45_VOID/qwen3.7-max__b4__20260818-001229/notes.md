# Notes - Superconductor Tc Study

## Budget Tracking
- Total: 45 units, all used
- Searches: 21 pages (6 for series + 14 for Hg + 1 wasted + 2 for Tl + 1 for Cu1Ba2Hg1 = ~24)
  Actually: Ca1Cu2Ba2Hg(6) + O6Ca1Cu2Ba2Hg1(1) + Hg(14) + Tl(2) + Cu1Ba2Hg1(1) = 24 pages
  Wait the budget shows 45 used. Let me just track by the counter.
- Measurements (successful): 10 series + 10 four-elem + 1 Tl-material = 21
- Measurements (failed, free): O6.160, O6.270, O4.047, O4.066, O4.160, O4.180, O7Ca1Cu2Sr2Tl0.500Pb0.500
- Budget ran out trying to measure O10Ca2Cu3Ba2Tl2 and O6Cu1Cd0.250Ba2Tl1.750

## Step A: Series Peak ✓
Peak: O6.210Ca1Cu2Ba2Hg1 with T* = 127.0 K
(Tied with O6.220Ca1Cu2Ba2Hg1 at 127.0 K)

Series trend (dome shape):
O6.080→92, O6.100→92.5, O6.130→102, O6.140→105, [O6.160→~113 unmeasurable],
O6.190→122, O6.210→127*, O6.220→127*, O6.240→123, [O6.270→~112 unmeasurable],
O6.300→100, O6.310→98

## Step B: Q1 ✓
threshold1 = 127.0 - 32 = 95.0 K
Q1 = {O4.081Cu1Ba2Hg1(98.0), O4.096Cu1Ba2Hg1(95.6), O4.097Cu1Ba2Hg1(95.3),
       O4.150Cu1Ba2Hg1(97.0), O4.162Cu1Ba2Hg1(97.4), O4.220Cu1Ba2Hg1(97.0)}

## Step C: Predictions ✓
Unmeasurable materials (no sample exists):
1. O6.160Ca1Cu2Ba2Hg1 → HIGH (within series O range, interpolated ~113 K)
2. O6.270Ca1Cu2Ba2Hg1 → HIGH (within series O range, interpolated ~112 K)
3. O4.047Cu1Ba2Hg1 → LOW (O=4.047 below measurable range 4.081-4.220)
4. O4.066Cu1Ba2Hg1 → LOW (O=4.066 below measurable range)
5. O4.160Cu1Ba2Hg1 → HIGH (O=4.160, within measurable range, ~97 K)
6. O4.180Cu1Ba2Hg1 → HIGH (O=4.180, within measurable range, ~96 K)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 → LOW (contains Tl)

Rule: Material is HIGH iff (1) no Tl AND (2) oxygen content is within the 
measured range for its structural family. Single-factor rules fail:
- "No Tl → high" wrongly predicts O4.047, O4.066 as high
- "O in range → high" wrongly predicts Tl-containing materials as high

## Step D: Final Pick ✓
HIGH predictions with fewest elements:
- O4.160Cu1Ba2Hg1 (4 elem, ~97.2 K expected)
- O4.180Cu1Ba2Hg1 (4 elem, ~96 K expected)
- O6.160Ca1Cu2Ba2Hg1 (5 elem)
- O6.270Ca1Cu2Ba2Hg1 (5 elem)

Tie at 4 elements: O4.160 has higher expected Tc → **O4.160Cu1Ba2Hg1**

## Discovered but not measurable due to budget (have samples):
- O10Ca2Cu3Ba2Tl2 (5 elem, no Hg, has Tl)
- O6Cu1Cd0.250Ba2Tl1.750 (6 elem)
- 5th Tl material (unknown, page 3 of Tl search)
