# Notes for Superconductor Tc Study

## Budget: 18 units total - EXHAUSTED

## Step A - Series Peak
Seed: O6.080Ca1Cu2Ba2Hg1, Tc=92.0 K

Measured the Ca1Cu2Ba2Hg series (same element set as seed):
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K ← PEAK (T*)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

T* = 123.0 K at O6.240Ca1Cu2Ba2Hg1

## Step B - Filter Family B
threshold1 = T* - 32 = 123.0 - 32 = 91.0 K

4-element Hg materials (O4.xxxCu1Ba2Hg1 series - 14 materials):
- O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, O4.150Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

Cannot measure due to budget exhaustion. Unknown which qualify (Tc >= 91.0).

## Step C - Extrapolate
Unmeasurable materials: Cannot determine without budget to test.
Potential candidate: O6Cu1Ba2Hg1Tl1 (unusual composition with Tl)

## Step D - Final Pick
Cannot determine without knowing which materials are unmeasurable and their predictions.

## Rule Induction
From measured Ca-containing series: Tc shows parabolic dependence on oxygen content.
Peak at optimal doping (O6.240). All measured values >= 90 K (all "high").

Possible rule: "Materials with Ca and oxygen content near optimal doping (~6.24) have highest Tc. Materials without Ca may have lower Tc."

But task warns single-factor rules are wrong. Need multi-factor rule considering:
- Presence of Ca
- Oxygen content
- Number of elements
- Element ratios

## Final Answer Reasoning
- Peak: O6.240Ca1Cu2Ba2Hg1 with Tc=123.0 K (measured)
- Threshold: 123.0 - 32 = 91.0 K
- Q1: Unknown (couldn't measure O4 series due to budget exhaustion)
- Unmeasurable: O6Cu1Ba2Hg1Tl1 (inferred from unusual composition format)
- Prediction for O6Cu1Ba2Hg1Tl1: high (Tl is known to enhance Tc in cuprates)
- Final: O6Cu1Ba2Hg1Tl1 (only unmeasurable predicted high found, has 5 elements)
- Rule: Multi-factor rule based on presence of Tl/Ca and oxygen doping level

## Budget Summary
- Total: 18 units
- Spent: 18 units (exhausted)
- Breakdown: 1 (seed measurement) + 3 (wasted searches) + 9 (Hg search pages) + 5 (series measurements) = 18

## Limitations
- Could not measure O4 series for Step B (qualifying list empty)
- Could not verify which materials are actually unmeasurable
- May have missed unmeasurable materials without Hg (didn't search other elements)
