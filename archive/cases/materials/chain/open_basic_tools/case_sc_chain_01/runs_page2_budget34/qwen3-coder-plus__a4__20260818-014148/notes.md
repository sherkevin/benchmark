# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 34 units total (for both search_db and measure_tc)
- Definition: high Tc >= 90 K, low Tc < 90 K

## Step A - Series Peak
The seed belongs to a homologous doping series (same element set, varying oxygen). Need to find and measure series members to identify the composition with MAXIMUM Tc (= T*).

First, I'll search for materials similar to the seed with varying oxygen content.

Found homologous series around seed material O6.080Ca1Cu2Ba2Hg1:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: null (not synthesized)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: null (not synthesized)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

Maximum Tc = 127.0 K (compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

## Step B - Filter family B (contains Hg and has exactly 4 elements)
Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Materials containing Hg with exactly 4 elements (O, Cu, Ba, Hg) that have Tc >= 95.0:
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K

## Step C & D - Predictions and Final Pick
For unmeasurable materials, I identified patterns in Tc vs oxygen content.
Unmeasurable materials:
- O4.047Cu1Ba2Hg1: predicted low (lowest oxygen content in series)
- O4.066Cu1Ba2Hg1: predicted low (below peak oxygen range)
- O6.160Ca1Cu2Ba2Hg1: predicted high (between high Tc values)
- O6.270Ca1Cu2Ba2Hg1: predicted high (close to peak region)

Among unmeasurable materials predicted high (O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1), 
O6.160Ca1Cu2Ba2Hg1 has fewer elements (6 elements) compared to O6.270Ca1Cu2Ba2Hg1 (6 elements as well),
so breaking tie by higher expected Tc, O6.160Ca1Cu2Ba2Hg1 is selected as it's between moderate-high Tc values.

Final answer written to answer.json