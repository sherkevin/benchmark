# Notes for Superconductor Tc Study

## Budget: 18 units total (EXHAUSTED)

## Step A: Series Peak (T*) - COMPLETE
Homologous series: O(x)Ca1Cu2Ba2Hg1 with varying oxygen

Measured Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (high)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (high)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (high)
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (high)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (high) <- PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (high) <- PEAK (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (high)
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (high)
- O6.310Ca1Cu2Ba2Hg1: NOT MEASURED (budget exhausted)

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B: Filter Family B - INCOMPLETE
threshold1 = T* - 32 = 127 - 32 = 95

Candidates (Hg, exactly 4 elements):
- O4.047Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg)
- O4.066Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg)
- O4.081Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg)

COULD NOT MEASURE - budget exhausted. qualifying = [] (unknown)

## Step C: Predictions for Unmeasurable Materials
Found 2 unmeasurable materials in the series:
- O6.160Ca1Cu2Ba2Hg1: predicted HIGH (interpolating between 105 and 122)
- O6.270Ca1Cu2Ba2Hg1: predicted HIGH (interpolating between 123 and 100)

Rule induced: Tc follows a dome-shaped relationship with oxygen content in homologous series. Peak Tc occurs at optimal oxygen doping, with underdoped and overdoped regions showing lower Tc. All measured members of the 5-element Ca-Cu-Ba-Hg-O series have Tc >= 90 K (high).

## Step D: Final Pick
Both unmeasurable materials have 5 elements (tie).
- O6.160 expected Tc ~113-114 (between 105 and 122)
- O6.270 expected Tc ~110-115 (between 123 and 100)

Picked O6.160Ca1Cu2Ba2Hg1 as final answer (slightly higher expected Tc).

## Final Answer
```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127,
  "threshold": 95,
  "qualifying": [],
  "predictions": {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high"},
  "final": "O6.160Ca1Cu2Ba2Hg1",
  "rule": "Tc follows a dome-shaped curve with oxygen content; peak Tc occurs at intermediate oxygen levels"
}
```

Note: Budget was exhausted before completing Step B measurements and fully exploring the database for unmeasurable materials. Answer is based on available data.
