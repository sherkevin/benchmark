# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 units total
- Both search_db and measure_tc cost 1 unit each
- High material: Tc >= 90 K
- Low material: Tc < 90 K

## Step A - Series Peak
Found materials in the series O6.xCa1Cu2Ba2Hg1:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: Sample not available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAX T*)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: Sample not available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Maximum Tc (T*) = 127.0 K at composition O6.210Ca1Cu2Ba2Hg1

## Step B - Filter Family B
Threshold1 = T* - 32 = 127.0 - 32 = 95.0
Need materials containing Hg with exactly 4 elements where Tc >= 95.0

Found Hg-containing materials with 4 elements:
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1

(Plus more that I couldn't retrieve due to budget limits)

Unfortunately, I've exhausted my budget and cannot measure these materials to determine which meet the threshold.

## Step C - Extrapolate
From the measured materials in the series, I can look for patterns to predict high/low for unmeasurable materials.

Measured materials:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (Low)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K (Low)
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K (High)
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K (High)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K (High)
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (High)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (High)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K (High)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K (High)
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K (High)

Unmeasurable materials in the series:
- O6.160Ca1Cu2Ba2Hg1: Sample not available
- O6.270Ca1Cu2Ba2Hg1: Sample not available

Looking at the pattern based on oxygen content:
- Oxygen 6.080: 92.0 K (Low)
- Oxygen 6.100: 92.5 K (Low)
- Oxygen 6.130: 102.0 K (High)
- Oxygen 6.140: 105.0 K (High)
- Oxygen 6.160: Not measured
- Oxygen 6.190: 122.0 K (High)
- Oxygen 6.210: 127.0 K (High)
- Oxygen 6.220: 127.0 K (High)
- Oxygen 240: 123.0 K (High)
- Oxygen 6.270: Not measured
- Oxygen 6.300: 100.0 K (High)
- Oxygen 6.310: 98.0 K (High)

Based on the trend, Tc increases with oxygen content from ~6.08 to ~6.22, peaking at ~6.21-6.22, then decreases. So O6.160Ca1Cu2Ba2Hg1 might have a Tc between 105.0 and 122.0 K (likely high), and O6.270Ca1Cu2Ba2Hg1 might have a Tc between 123.0 and 100.0 K (likely high).