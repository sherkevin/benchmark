# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high Tc >= 90 K, low Tc < 90 K
- Measurement budget: 18 measure_tc calls

## Step A: Series Peak
Need to measure the series members in data/measurable.txt and find the composition with MAXIMUM Tc.

The series appears to be oxygen-doped variants of Ca1Cu2Ba2Hg1 with different oxygen amounts.

Results:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.047Cu1Ba2Hg1: 95.0 K
- O4.200Cu1Ba2Hg1: 94.2 K

Maximum Tc = 127.0 K, for compositions O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
T* = 127.0 K

## Step C Analysis - Rule Induction

Let me analyze the measurable compositions I measured to identify patterns between high (≥90K) and low (<90K) Tc:

High Tc materials (≥90K):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (5 elements: O,Ca,Cu,Ba,Hg)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (5 elements: O,Ca,Cu,Ba,Hg)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (5 elements: O,Ca,Cu,Ba,Hg)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (5 elements: O,Ca,Cu,Ba,Hg)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (5 elements: O,Ca,Cu,Ba,Hg)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (5 elements: O,Ca,Cu,Ba,Hg)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (5 elements: O,Ca,Cu,Ba,Hg)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (5 elements: O,Ca,Cu,Ba,Hg)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (5 elements: O,Ca,Cu,Ba,Hg)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (5 elements: O,Ca,Cu,Ba,Hg)
- O4.081Cu1Ba2Hg1: 98.0 K (4 elements: O,Cu,Ba,Hg)
- O4.162Cu1Ba2Hg1: 97.4 K (4 elements: O,Cu,Ba,Hg)
- O4.150Cu1Ba2Hg1: 97.0 K (4 elements: O,Cu,Ba,Hg)
- O4.220Cu1Ba2Hg1: 97.0 K (4 elements: O,Cu,Ba,Hg)
- O4.096Cu1Ba2Hg1: 95.6 K (4 elements: O,Cu,Ba,Hg)
- O4.097Cu1Ba2Hg1: 95.3 K (4 elements: O,Cu,Ba,Hg)
- O4.047Cu1Ba2Hg1: 95.0 K (4 elements: O,Cu,Ba,Hg)

Low Tc materials (<90K):
- O4.200Cu1Ba2Hg1: 94.2 K (4 elements: O,Cu,Ba,Hg) - Wait, this is still high Tc according to definition!

Actually, looking again at the definition: high Tc >= 90 K, low Tc < 90 K.
So all the measured materials above are actually high Tc.

Let me reconsider: I ran out of measurements, so I only measured 18 compositions out of the full list. Let me focus on what I have learned:

Materials I measured:
- All Ca1Cu2Ba2Hg1 series: high Tc (92.0K - 127.0K)
- All Cu1Ba2Hg1 series: high Tc (94.2K - 98.0K)

Since all measured materials have high Tc, I need to make predictions based on patterns I observe:
Looking at the oxygen content:
- O4.x compositions: 94.2K - 98.0K (relatively lower range of high Tc)
- O6.x compositions: 92.0K - 127.0K (higher range of high Tc)

The Ca1Cu2Ba2Hg1 series shows a peak around O6.21-O6.22 (127.0K), then drops again.
The Cu1Ba2Hg1 series shows values mostly in 94-98K range.

## Step D Analysis

From my predictions in Step C, the unmeasurable materials predicted to be HIGH are:
- O6.160Ca1Cu2Ba2Hg1: HIGH (5 elements: O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: HIGH (5 elements: O, Ca, Cu, Ba, Hg)
- O4.180Cu1Ba2Hg1: HIGH (4 elements: O, Cu, Ba, Hg)

Among these, O4.180Cu1Ba2Hg1 has the fewest elements (4), so this is the final pick.