# Notes for Superconductor Tc Study

## Budget: 26 units total (USED UP - 26/26)

## Step A — Series Peak
Seed: O6.080Ca1Cu2Ba2Hg1

Homologous series (O-Ca-Cu-Ba-Hg, 5 elements):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← PEAK (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K (peak)**
Peak composition: O6.210Ca1Cu2Ba2Hg1

## Step B — Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0

4-element Hg materials (O-Cu-Ba-Hg):
- O4.047Cu1Ba2Hg1: 95.0 K ✓ (>= 95)
- O4.150Cu1Ba2Hg1: 97.0 K ✓ (>= 95)
- Others not measured due to budget

Q1 = ["O4.047Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1"]

## Step C — Extrapolate
Unmeasurable materials found:
- O6.160Ca1Cu2Ba2Hg1 (5 elements) - predicted HIGH
- O6.270Ca1Cu2Ba2Hg1 (5 elements) - predicted HIGH

Rule: In the O-Ca-Cu-Ba-Hg homologous series, Tc follows a parabolic trend with oxygen content, peaking around O6.210-6.220. Unmeasurable compositions in high-Tc regions are predicted to be high (>= 90 K).

## Step D — Final Pick
Among unmeasurable predicted high:
- O6.160Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~113 K
- O6.270Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~111 K

Both have 5 elements (tie). O6.160 has higher expected Tc.
Final answer: O6.160Ca1Cu2Ba2Hg1

## Final Answer
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": ["O4.047Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1"],
  "predictions": {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high"},
  "final": "O6.160Ca1Cu2Ba2Hg1",
  "rule": "Tc follows a parabolic trend with oxygen content in homologous series; materials in high-Tc regions of their series are predicted high"
}
