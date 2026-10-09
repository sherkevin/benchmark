# Notes for Superconductor Tc Study

## Seed Material
- O6.080Ca1Cu2Ba2Hg1

## Step A: Homologous series peak (T*)
Series: O-Ca-Cu-Ba-Hg (5 elements, varying oxygen)

Measured values:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K  <-- PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K  <-- PEAK (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (peak)
**peak composition = O6.210Ca1Cu2Ba2Hg1**

## Step B: Filter family B (Hg + exactly 4 elements, Tc >= threshold1)
threshold1 = T* - 32 = 127.0 - 32 = 95.0

4-element Hg materials (O-Cu-Ba-Hg) measured:
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓ QUALIFIES
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓ QUALIFIES
- O4.085Cu1Ba2Hg1: 92.0 < 95.0 ✗
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓ QUALIFIES
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓ QUALIFIES
- O4.101Cu1Ba2Hg1: 93.4 < 95.0 ✗
- O4.103Cu1Ba2Hg1: 92.5 < 95.0 ✗
- O4.180Cu1Ba2Hg1: NO SAMPLE (unmeasurable)

Q1 (qualifying): ["O4.047Cu1Ba2Hg1", "O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1"]

## Step C: Unmeasurable materials and predictions
Unmeasurable found:
- O6.160Ca1Cu2Ba2Hg1 (5 elements) → predicted HIGH (interpolated Tc ~112)
- O6.270Ca1Cu2Ba2Hg1 (5 elements) → predicted HIGH (interpolated Tc ~111.5)
- O4.180Cu1Ba2Hg1 (4 elements) → predicted LOW (oxygen 4.18 outside HIGH range)

**Rule induced**: Tc depends on element count and oxygen content. 5-element O-Ca-Cu-Ba-Hg series stays high (>=90K) across wide oxygen range (6.08-6.31), while 4-element O-Cu-Ba-Hg series is high only in narrow oxygen window (4.04-4.10) and drops below 90K outside this range.

## Step D: Final pick
Among unmeasurable HIGH predictions:
- O6.160Ca1Cu2Ba2Hg1 (5 elements, expected Tc ~112)
- O6.270Ca1Cu2Ba2Hg1 (5 elements, expected Tc ~111.5)

Both have 5 elements (tie). O6.160 has slightly higher expected Tc.

**Final answer: O6.160Ca1Cu2Ba2Hg1**

## Budget: 26/26 (exhausted)
