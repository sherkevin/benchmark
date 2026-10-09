# Notes for Superconductor Tc Study

## Step A — Series Peak
Seed series: Ca1Cu2Ba2Hg1 with varying oxygen

Measurements:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K  <- MAX (T*)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)
**threshold1 = T* - 32 = 95.0 K**

## Step B — Filter Family B (Hg, exactly 4 elements)
4-element Hg materials are Cu-Ba-Hg-O compounds (no Ca).

Measured 4-element Hg materials:
- O4.047Cu1Ba2Hg1: 95.0 >= 95.0 ✓
- O4.081Cu1Ba2Hg1: 98.0 >= 95.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 >= 95.0 ✓
- O4.097Cu1Ba2Hg1: 95.3 >= 95.0 ✓
- O4.150Cu1Ba2Hg1: 97.0 >= 95.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 >= 95.0 ✓
- O4.200Cu1Ba2Hg1: 94.2 < 95.0 ✗
- O4.220Cu1Ba2Hg1: 97.0 >= 95.0 ✓

**Q1** = ["O4.047Cu1Ba2Hg1", "O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1"]

## Step C — Extrapolate Rule for Unmeasurable Materials

Unmeasurable compositions:
1. O6.160Ca1Cu2Ba2Hg1
2. O6.270Ca1Cu2Ba2Hg1

Both are in the same homologous series as the seed (Ca1Cu2Ba2Hg1).

**Induced Rule:** For the Ca-Cu-Ba-Hg-O homologous series, Tc follows a parabolic trend with oxygen content, peaking around O6.21-6.22. All compositions in this series with oxygen between 6.0 and 6.4 have Tc >= 90 K (high). The interaction of Hg presence (enhancing Tc), layered cuprate structure (Ca between Cu-O planes), and optimal oxygen doping determines high-Tc behavior.

**Predictions:**
- O6.160Ca1Cu2Ba2Hg1: HIGH (interpolated Tc ~112 K, between O6.140=105 and O6.190=122)
- O6.270Ca1Cu2Ba2Hg1: HIGH (interpolated Tc ~112 K, between O6.240=123 and O6.300=100)

## Step D — Final Pick
Both unmeasurable materials have 5 elements (tie).
Tie-breaker: higher expected Tc.

Estimated Tc:
- O6.160: ~111.8 K (linear interp between 6.140 and 6.190)
- O6.270: ~111.5 K (linear interp between 6.240 and 6.300)

**Final answer: O6.160Ca1Cu2Ba2Hg1** (slightly higher expected Tc)
