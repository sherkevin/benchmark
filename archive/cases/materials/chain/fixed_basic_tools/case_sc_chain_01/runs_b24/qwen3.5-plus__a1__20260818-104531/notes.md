# Notes on Superconductor Tc Study

## Task Summary
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc >= 90 K, low Tc < 90 K
- Budget: 24 measure_tc calls

## Step A: Find series peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Series: O-Ca-Cu-Ba-Hg with Ca1Cu2Ba2Hg1 metal ratios, varying oxygen.

Measured series members (from unmeasurable.txt which actually has samples):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K  <- MAX
- O6.220Ca1Cu2Ba2Hg1: 127.0 K  <- MAX (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1** (first one with max Tc)

## Step B: Filter family B
threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

Materials with Hg and exactly 4 elements (O-Cu-Ba-Hg):
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓
- O4.200Cu1Ba2Hg1: 94.2 < 95.0 ✗
- O4.101Cu1Ba2Hg1: 93.4 < 95.0 ✗
- O4.103Cu1Ba2Hg1: not measured (budget exhausted)
- O4.085Cu1Ba2Hg1: not measured (budget exhausted)

**Q1 = {O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}**

## Step C: Extrapolate to unmeasurable (measurable.txt)

Induced rule from measured data:
- HIGH: Materials with (Hg or Tl) but NOT both, AND no (La or Y or Cd)
- LOW: Materials with (both Hg and Tl) OR (La) OR (Y) OR (Cd)

Verification against measured data:
- O-Ca-Cu-Ba-Hg series: Hg only → HIGH ✓
- O-Cu-Ba-Hg series: Hg only → HIGH ✓
- O10Ca2Cu3Ba2Tl2: Tl only → HIGH (118.97) ✓
- O6Cu1Ba2Hg1Tl1: Hg AND Tl → LOW (0.0) ✓
- O6Cu1Cd0.250Ba2Tl1.750: Tl AND Cd → LOW (88.0) ✓
- O7Cu3Ba2La1: La, no Hg → LOW (89.6) ✓
- O7Cu3Y0.200Ba2La0.800: Y and La → LOW (81.0) ✓

Predictions for measurable.txt compositions:
1. O6.160Ca1Cu2Ba2Hg1: Hg only → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Hg only → HIGH
3. O4.180Cu1Ba2Hg1: Hg only → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Tl only → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl only (Pb not exclusion factor) → HIGH
6. O8Cu3Y1Ba1.900La0.100: has Y and La → LOW
7. O8Cu4Y1Ba1.870La0.130: has Y and La → LOW

## Step D: Final pick
Among HIGH predictions, find one with FEWEST elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, highest Tc among 4-element candidates)

## Summary for answer.json
- peak: "O6.210Ca1Cu2Ba2Hg1"
- peak_tc: 127.0
- threshold: 95.0
- qualifying: ["O4.047Cu1Ba2Hg1", "O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1"]
- predictions: {all 7 measurable.txt compositions}
- final: "O4.180Cu1Ba2Hg1"
- rule: "High Tc if contains (Hg or Tl) but not both, and does not contain (La or Y or Cd)"
