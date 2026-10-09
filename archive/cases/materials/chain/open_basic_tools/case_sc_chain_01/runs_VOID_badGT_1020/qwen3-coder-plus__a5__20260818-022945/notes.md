# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 34 units total
- Definition: high Tc >= 90 K, low Tc < 90 K
- Tools: measure_tc() and search_db()

## Step A - Series Peak Results
Materials with same elements (Ca, Cu, Ba, Hg) but different oxygen content:

- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: No sample available (not synthesized yet)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: No sample available (not synthesized yet)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

The peak Tc (T*) is 127.0 K, achieved by O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1.

## Step B - Filter Family B Results
Materials that contain Hg and have exactly 4 elements (Cu, Ba, Hg, O) with Tc >= 95.0 K (threshold1 = 127.0 - 32 = 95.0 K):
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K

So Q1 = ["O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1"]

## Step C - Extrapolate
Some materials in the database have never been synthesized. I found these unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1 (from series with Ca, Cu, Ba, Hg, O)
- O6.270Ca1Cu2Ba2Hg1 (from series with Ca, Cu, Ba, Hg, O)
- O4.047Cu1Ba2Hg1 (from series with Cu, Ba, Hg, O)
- O4.066Cu1Ba2Hg1 (from series with Cu, Ba, Hg, O)

Based on the data I collected, I can see a pattern:
- For the Ca1Cu2Ba2Hg1 series with varying oxygen:
  - O6.080Ca1Cu2Ba2Hg1: 92.0 K
  - O6.100Ca1Cu2Ba2Hg1: 92.5 K
  - O6.130Ca1Cu2Ba2Hg1: 102.0 K
  - O6.140Ca1Cu2Ba2Hg1: 105.0 K
  - O6.160Ca1Cu2Ba2Hg1: No sample available (predicted based on trend)
  - O6.190Ca1Cu2Ba2Hg1: 122.0 K
  - O6.210Ca1Cu2Ba2Hg1: 127.0 K
  - O6.220Ca1Cu2Ba2Hg1: 127.0 K
  - O6.240Ca1Cu2Ba2Hg1: 123.0 K
  - O6.270Ca1Cu2Ba2Hg1: No sample available (predicted based on trend)
  - O6.300Ca1Cu2Ba2Hg1: 100.0 K
  - O6.310Ca1Cu2Ba2Hg1: 98.0 K

Looking at the Ca series, Tc peaks around O6.21-O6.22, suggesting an optimal oxygen content. The unmeasured O6.160Ca1Cu2Ba2Hg1 likely has Tc around 115-120 K (interpolating between O6.140 at 105.0 K and O6.190 at 122.0 K), which would be high (>90 K).

The O6.270Ca1Cu2Ba2Hg1 is after the peak and going down (O6.240 is 123.0 K, O3.00 is 100.0 K), so likely around 110-115 K, which is high (>90 K).

For the Cu1Ba2Hg1 series with varying oxygen:
- O4.047Cu1Ba2Hg1: No sample available
- O4.066Cu1Ba2Hg1: No sample available
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K

The Tc seems to peak around O4.081 and then decrease slightly. So O4.047 might be lower than 98.0 K but could still be high, perhaps around 94-96 K (extrapolating from the trend), which would be high (>90 K).

O4.066 is between O4.047 (estimated ~95 K) and O4.081 (98.0 K), so maybe around 96-97 K, which would be high (>90 K).

Rule: Based on oxygen content optimization, materials with oxygen content near the peak values tend to have high Tc. The optimal oxygen content appears to depend on the specific elemental composition.

Predictions:
- O6.160Ca1Cu2Ba2Hg1: high (likely ~115-120 K)
- O6.270Ca1Cu2Ba2Hg1: high (likely ~110-115 K)
- O4.047Cu1Ba2Hg1: high (likely ~94-96 K)
- O4.066Cu1Ba2Hg1: high (likely ~96-97 K)

## Step D - Final Pick
Among unmeasurable materials predicted to be high, I need to choose the one with the fewest elements.
- O6.160Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O)
- O6.270Ca1Cu2Ba2Hg1: 5 elements (Ca, Cu, Ba, Hg, O)
- O4.047Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O)
- O4.066Cu1Ba2Hg1: 4 elements (Cu, Ba, Hg, O)

The ones with fewest elements (4) are O4.047Cu1Ba2Hg1 and O4.066Cu1Ba2Hg1.
Between these two, O4.047Cu1Ba2Hg1 is likely to have higher Tc (~95-96 K vs ~96-97 K), so O4.066Cu1Ba2Hg1 would have higher expected Tc.

Wait, actually, I estimated O4.047 to be ~94-96 K and O4.066 to be ~96-97 K, so O4.066Cu1Ba2Hg1 would have higher expected Tc.

Final pick: O4.066Cu1Ba2Hg1