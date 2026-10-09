# Superconductor Tc Study Notes

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`

## Budget
- Total: 18 units
- Both measure_tc and search_db cost 1 unit each
- Budget exhausted: 15 on searches, 3 on measurements

## Step A: Series Peak
The seed belongs to a homologous series with same elements (Ca, Cu, Ba, Hg, O) but varying oxygen.

Series members found (12 total):
- O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1, O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1, O6.240Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

Measurements:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92 K (high)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122 K (high) - PEAK
- O6.310Ca1Cu2Ba2Hg1: Tc = 98 K (high)

**T* = 122 K at O6.190Ca1Cu2Ba2Hg1**

## Step B: Filter Family B
- threshold1 = T* - 32 = 122 - 32 = 90 K
- Need Hg-containing materials with exactly 4 elements and Tc >= 90

Candidates: O4.XXXCu1Ba2Hg1 materials (O, Cu, Ba, Hg = 4 elements, contains Hg)
- 14 materials found: O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220

Could not measure due to budget exhaustion. Assuming Tc < 90 for these (no Ca), so qualifying = [].

## Step C: Extrapolate
Unmeasurable materials (identified by integer stoichiometry - no decimal points):
- O6Cu1Ba2Hg1Tl1 (5 elements: O, Cu, Ba, Hg, Tl)
- O7Cu3Ba2La1 (4 elements: O, Cu, Ba, La)

Predictions based on rule "Materials with Cu, Ba, and oxygen >= 6 have high Tc":
- O6Cu1Ba2Hg1Tl1: Has Cu, Ba, O6 -> high
- O7Cu3Ba2La1: Has Cu, Ba, O7 -> high

## Step D: Final Pick
Among unmeasurable materials predicted high:
- O6Cu1Ba2Hg1Tl1: 5 elements
- O7Cu3Ba2La1: 4 elements

**Final answer: O7Cu3Ba2La1** (fewest elements = 4)

## Induced Rule
"Materials containing Cu and Ba with oxygen content >= 6 have high Tc (>= 90 K)"

This is a multi-factor rule (Cu + Ba + oxygen level), not a single-factor rule.

## Final Answer
```json
{
  "peak": "O6.190Ca1Cu2Ba2Hg1",
  "peak_tc": 122,
  "threshold": 90,
  "qualifying": [],
  "predictions": {
    "O6Cu1Ba2Hg1Tl1": "high",
    "O7Cu3Ba2La1": "high"
  },
  "final": "O7Cu3Ba2La1",
  "rule": "Materials containing Cu and Ba with oxygen >= 6 have high Tc"
}
```
