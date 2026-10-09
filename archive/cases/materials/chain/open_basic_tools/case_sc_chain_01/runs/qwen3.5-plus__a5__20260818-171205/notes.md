# Superconductor Tc Study Notes

## Seed Material
- Seed: `O6.080Ca1Cu2Ba2Hg1`

## Step A — Series Peak (COMPLETED)
The seed belongs to homologous series with elements: O, Ca, Cu, Ba, Hg

**Measurements:**
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← MAX
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← MAX
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Hg1: NO SAMPLE (unmeasurable) - Wait, checking: O6.270Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)

## Step B — Filter Family B
- threshold1 = T* - 32 = 127.0 - 32 = **95.0**
- Need: materials with Hg AND exactly 4 elements AND Tc >= 95.0

**Measured 4-element Hg materials:**
- O4.047Cu1Ba2Hg1: 95.0 ✓ (>= 95)
- O4.066Cu1Ba2Hg1: couldn't measure (error)
- O4.081Cu1Ba2Hg1: 98.0 ✓
- O4.085Cu1Ba2Hg1: 92.0 ✗
- O4.096Cu1Ba2Hg1: 95.6 ✓
- O4.097Cu1Ba2Hg1: 95.3 ✓

**Q1 (qualifying):** O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1

## Budget Tracking
- Total budget: 26 units
- Used: 26 (11 search + 15 measurements)
- Remaining: 0

## Step C — Extrapolate
**Unmeasurable materials found:**
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)

**Prediction rule:** Tc follows a parabolic trend with oxygen content in homologous series. Materials with oxygen content in the middle range (near the peak of the series) have higher Tc. Both unmeasurable materials have oxygen content between measured high-Tc neighbors, so both predicted "high".

**Predictions:**
- O6.160Ca1Cu2Ba2Hg1: high (between 105 and 122 neighbors)
- O6.270Ca1Cu2Ba2Hg1: high (between 123 and 100 neighbors)

## Step D — Final pick
Both unmeasurable predicted-high materials have 5 elements (tie).
Tie-breaker: higher expected Tc.
- O6.160: expected ~112 K (interpolating between 105 and 122)
- O6.270: expected ~111 K (interpolating between 123 and 100)

**Final answer: O6.160Ca1Cu2Ba2Hg1** (slightly higher expected Tc)

## Final Answer
```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": ["O4.047Cu1Ba2Hg1", "O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1"],
  "predictions": {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high"},
  "final": "O6.160Ca1Cu2Ba2Hg1",
  "rule": "Tc follows a parabolic trend with oxygen content; unmeasurable materials with oxygen in the peak region (between high-Tc measured neighbors) are predicted high"
}
```
