# Superconductor Tc Study Notes

## Step A - Series Peak
- Seed composition: O6.080Ca1Cu2Ba2Hg1
- Measured Tc values for series members in measurable.txt
- Maximum Tc found: 127.0 K for O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
- T* = 127.0 K

## Step B - Filter Family B
- Contains Hg and has exactly 4 elements
- threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- Materials that contain Hg and have exactly 4 elements with Tc >= 95.0 K:
  - O4.081Cu1Ba2Hg1: 98.0 K
  - O4.162Cu1Ba2Hg1: 97.4 K
  - O4.150Cu1Ba2Hg1: 97.0 K
  - O4.220Cu1Ba2Hg1: 97.0 K
- Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}

## Step C - Extrapolate
- Rule: Materials with Tl or La are likely to have low Tc; materials without Tl or La but with Hg/Cu/Ba/O combinations are likely to have high Tc
- Predictions for unmeasurable.txt:
  - O6.160Ca1Cu2Ba2Hg1: HIGH (follows Ca+Cu+Ba+Hg+O pattern)
  - O6.270Ca1Cu2Ba2Hg1: HIGH (follows Ca+Cu+Ba+Hg+O pattern)
  - O4.180Cu1Ba2Hg1: HIGH (follows Cu+Ba+Hg+O pattern)
  - O4.047Cu1Ba2Hg1: HIGH (follows Cu+Ba+Hg+O pattern)
  - O4.066Cu1Ba2Hg1: HIGH (follows Cu+Ba+Hg+O pattern)
  - O4.160Cu1Ba2Hg1: HIGH (follows Cu+Ba+Hg+O pattern)
  - O8Ca0.850Cu2.200Ba2Tl2: LOW (contains Tl)
  - O7Ca1Cu2Sr2Tl0.500Pb0.500: LOW (contains Tl)
  - O8Cu3Y1Ba1.900La0.100: LOW (contains La and Y)
  - O8Cu4Y1Ba1.870La0.130: LOW (contains La and Y)

## Step D - Final Pick
- Unmeasurable materials predicted as HIGH: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- Materials with fewest elements (4): O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- Breaking tie by expected Tc: O4.160Cu1Ba2Hg1 likely has high Tc based on similarity to O4.162Cu1Ba2Hg1 (97.4 K)
- Final answer: O4.160Cu1Ba2Hg1