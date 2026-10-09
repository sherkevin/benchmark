# Superconductor Tc Study Notes - Final

## Step A: Series Peak
Series: O_xCa1Cu2Ba2Hg1 (homologous series with varying oxygen)

Measurements obtained (budget exhausted after 3 measurements):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (high)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K (high)
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1: No sample exists
- O6.160Ca1Cu2Ba2Hg1: No sample exists

**Peak (T*)**: 102.0 K at O6.130Ca1Cu2Ba2Hg1

## Step B: Filter Family B
threshold1 = T* - 32 = 102.0 - 32 = 70.0

Materials with Hg and exactly 4 elements found via search:
- O4.047Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1

**Qualifying (Q1)**: Empty - didn't measure their Tc due to budget exhaustion.

## Step C: Predictions for Unmeasurable Materials
Unmeasurable materials discovered:
- O6.140Ca1Cu2Ba2Hg1
- O6.160Ca1Cu2Ba2Hg1

**Rule induced**: Tc increases with oxygen content in the range 6.080-6.130.

**Predictions**: 
- O6.140Ca1Cu2Ba2Hg1: high
- O6.160Ca1Cu2Ba2Hg1: high

## Step D: Final Pick
Both unmeasurable materials have 5 elements (tie).
Tie-breaker: higher expected Tc → O6.140 (closer to peak at 6.130)

**Final**: O6.140Ca1Cu2Ba2Hg1

## Answer Written to answer.json
- peak: "O6.130Ca1Cu2Ba2Hg1"
- peak_tc: 102.0
- threshold: 70.0
- qualifying: []
- predictions: {"O6.140Ca1Cu2Ba2Hg1": "high", "O6.160Ca1Cu2Ba2Hg1": "high"}
- final: "O6.140Ca1Cu2Ba2Hg1"
- rule: "Tc increases with oxygen content in the homologous series O_xCa1Cu2Ba2Hg1 for x in range 6.080-6.130"
