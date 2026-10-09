# Notes for Superconductor Tc Study

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Measurement budget: 14 `measure_tc` calls
- Definition: High Tc ≥ 90K, Low Tc < 90K

## Step A - Series Peak
Need to measure all compositions in `data/measurable.txt` to find the one with MAXIMUM Tc.

Results:
- O6.080Ca1Cu2Ba2Hg1: 92.0K
- O6.100Ca1Cu2Ba2Hg1: 92.5K
- O6.130Ca1Cu2Ba2Hg1: 102.0K
- O6.140Ca1Cu2Ba2Hg1: 105.0K
- O6.190Ca1Cu2Ba2Hg1: 122.0K
- O6.210Ca1Cu2Ba2Hg1: 127.0K
- O6.220Ca1Cu2Ba2Hg1: 127.0K
- O6.240Ca1Cu2Ba2Hg1: 123.0K
- O6.300Ca1Cu2Ba2Hg1: 100.0K
- O6.310Ca1Cu2Ba2Hg1: 98.0K
- O4.081Cu1Ba2Hg1: 98.0K
- O4.162Cu1Ba2Hg1: 97.4K
- O4.150Cu1Ba2Hg1: 97.0K
- O4.220Cu1Ba2Hg1: 97.0K

MAXIMUM Tc (T*) = 127.0K
Peak composition: O6.210Ca1Cu2Ba2Hg1

## Step B - Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0K
Materials containing Hg with exactly 4 elements that have Tc >= 95.0K:
- O4.081Cu1Ba2Hg1: 98.0K ≥ 95.0K ✓
- O4.162Cu1Ba2Hg1: 97.4K ≥ 95.0K ✓
- O4.150Cu1Ba2Hg1: 97.0K ≥ 95.0K ✓
- O4.220Cu1Ba2Hg1: 97.0K ≥ 95.0K ✓

Q1 = ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1"]

## Step C - Predictions for unmeasurable materials
Rule hypothesis: Materials with high Tc contain Cu + (Hg/Tl/Pb) + (Ba/Sr/Ca) with optimized oxygen stoichiometry. Materials with competing heavy elements (like both Tl and Pb) may have structural interference leading to low Tc.

Unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 - Contains Cu, Ca, Ba, Hg, O → high
2. O6.270Ca1Cu2Ba2Hg1 - Contains Cu, Ca, Ba, Hg, O → high
3. O4.180Cu1Ba2Hg1 - Contains Cu, Ba, Hg, O → high
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Contains Cu, Ca, Ba, Tl, O → high
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Contains Cu, Ca, Sr, Tl, Pb, O → low (structural competition between Tl and Pb)
6. O8Cu3Y1Ba1.900La0.100 - Contains Cu, Y, Ba, La, O → high (Y-Ba-Cu-O system)
7. O8Cu4Y1Ba1.870La0.130 - Contains Cu, Y, Ba, La, O → high (Y-Ba-Cu-O system)

Predictions:
{
  "O6.160Ca1Cu2Ba2Hg1": "high",
  "O6.270Ca1Cu2Ba2Hg1": "high",
  "O4.180Cu1Ba2Hg1": "high",
  "O8Ca0.850Cu2.200Ba2Tl1.800": "high",
  "O7Ca1Cu2Sr2Tl0.500Pb0.500": "low",
  "O8Cu3Y1Ba1.900La0.100": "high",
  "O8Cu4Y1Ba1.870La0.130": "high"
}

## Step D - Final pick
Unmeasurable materials predicted high: 
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements) 
- O4.180Cu1Ba2Hg1 (4 elements)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements)
- O8Cu3Y1Ba1.900La0.100 (5 elements)
- O8Cu4Y1Ba1.870La0.130 (5 elements)

Among these, O4.180Cu1Ba2Hg1 has the fewest elements (4). This is the final pick.