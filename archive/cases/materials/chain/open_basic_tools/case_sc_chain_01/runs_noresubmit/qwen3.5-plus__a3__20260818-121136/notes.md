# Notes for Superconductor Tc Study

Budget: 18 units total (shared between search_db and measure_tc)
Budget used: 18 (9 search + 9 measure)

Seed material: `O6.080Ca1Cu2Ba2Hg1`
Definition: high if Tc >= 90 K, low otherwise

## Step A — Series Peak (COMPLETE)

Homologous series (O, Ca, Cu, Ba, Hg - 5 elements, varying oxygen):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K  <-- PEAK (T*)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K  <-- PEAK (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: NO SAMPLE
- O6.300Ca1Cu2Ba2Hg1: not measured (budget exhausted)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1)
**threshold1 = T* - 32 = 95.0 K**

## Step B — Filter Family B (contains Hg, exactly 4 elements)

Hg materials with 4 elements (O, Cu, Ba, Hg) found via search:
- 27 total materials in O4.xxxCu1Ba2Hg1 format
- Oxygen ranges from ~4.047 to ~4.180+

PROBLEM: Budget exhausted, could not measure any 4-element Hg materials.
**Q1 = []** (empty - no measurements possible)

## Step C — Extrapolate

Unmeasurable materials found (no sample exists):
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1

**Rule induced:** High-Tc depends on both element composition and oxygen stoichiometry. 
In the O6.xCa1Cu2Ba2Hg1 series, all materials with oxygen between 6.08-6.31 exhibit 
high Tc (>=90 K), forming a bell curve peaking at 127 K near O6.21-0.22.

**Predictions:**
- O6.160Ca1Cu2Ba2Hg1: high (oxygen 6.160 is within the high-Tc range)
- O6.270Ca1Cu2Ba2Hg1: high (oxygen 6.270 is within the high-Tc range)

## Step D — Final Pick

Among predicted-high unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~113-115 K (rising edge)
- O6.270Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~105-110 K (declining edge)

Both have 5 elements (tie). O6.160 has higher expected Tc.

**Final pick: O6.160Ca1Cu2Ba2Hg1**

## Final Answer (written to answer.json)

```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": [],
  "predictions": {"O6.160Ca1Cu2Ba2Hg1": "high", "O6.270Ca1Cu2Ba2Hg1": "high"},
  "final": "O6.160Ca1Cu2Ba2Hg1",
  "rule": "High-Tc depends on both element composition and oxygen stoichiometry. In the O6.xCa1Cu2Ba2Hg1 series, all materials with oxygen between 6.08-6.31 exhibit high Tc (>=90 K), forming a bell curve peaking at 127 K near O6.21-0.22. Unmeasurable members within this oxygen range are predicted high based on the consistent pattern."
}
```
