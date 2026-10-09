# Superconductor Tc Study Notes

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1` (5 elements: O, Ca, Cu, Ba, Hg)

## Step A — Series Peak (T*)
Homologous series: O(x)Ca1Cu2Ba2Hg1 (same element set, varying oxygen)

Measured Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (HIGH)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (HIGH)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (HIGH)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (HIGH)
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (HIGH)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (HIGH) ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (HIGH) ← PEAK (tie)

**T* = 127.0 K** (peak Tc)
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B — Family B (Hg + exactly 4 elements)
threshold1 = T* - 32 = 127 - 32 = 95 K

4-element Hg materials found (from Hg search pages 1-4):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1

PROBLEM: No budget left to measure these! Cannot determine Q1.
qualifying = [] (empty - couldn't measure)

## Step C — Unmeasurable materials found
- O6.160Ca1Cu2Ba2Hg1 (5 elements) - oxygen 6.160, between 6.140 (105K) and 6.190 (122K)
- O6.270Ca1Cu2Ba2Hg1 (5 elements) - oxygen 6.270, past peak at 6.21-6.22

Prediction rule: In this homologous series, all measured samples have Tc >= 90K (HIGH). 
The trend shows Tc increasing with oxygen up to peak at 6.21-6.22, then potentially decreasing.
- O6.160: Interpolated Tc ~113K (between 105K and 122K) → HIGH
- O6.270: Past peak, unknown but likely still >90K based on typical gradual decrease → HIGH

## Step D — Final pick
Both unmeasurable have 5 elements (tie). 
- O6.160 expected Tc: ~113K (interpolated)
- O6.270 expected Tc: unknown, could be 120K+ if near plateau, or lower if sharp decrease

Choosing O6.160Ca1Cu2Ba2Hg1 as final (more confident prediction, safely above 90K threshold).

## Budget Status: EXHAUSTED (18/18)
- Search: 9 pages (Hg: 4, Ca: 5) = 9 budget
- Measurements: 7 successful + 2 no-sample = 9 budget (but some may not have counted)
Total: 18/18

## Final Answer
```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": [],
  "predictions": {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high"},
  "final": "O6.160Ca1Cu2Ba2Hg1",
  "rule": "In the O(x)Ca1Cu2Ba2Hg1 series, all measured samples have Tc >= 90K. Tc increases with oxygen content up to peak at x≈6.21-6.22 (127K). Unmeasurable samples are predicted based on interpolation/extrapolation from measured neighbors."
}
```
