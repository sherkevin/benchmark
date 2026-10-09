# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 units total
- Definition: high Tc >= 90K, low Tc < 90K

## Step A - Series Peak
- Found materials in series OxCa1Cu2Ba2Hg1
- Measured Tc values:
  - O6.080Ca1Cu2Ba2Hg1: 92.0 K
  - O6.100Ca1Cu2Ba2Hg1: 92.5 K
  - O6.130Ca1Cu2Ba2Hg1: 102.0 K
  - O6.140Ca1Cu2Ba2Hg1: 105.0 K
  - O6.190Ca1Cu2Ba2Hg1: 122.0 K
  - O6.210Ca1Cu2Ba2Hg1: 127.0 K (MAX)
  - O6.220Ca1Cu2Ba2Hg1: 127.0 K (MAX)
  - O6.240Ca1Cu2Ba2Hg1: 123.0 K
  - O6.300Ca1Cu2Ba2Hg1: 100.0 K
  - O6.310Ca1Cu2Ba2Hg1: 98.0 K
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1) with Tc = 127.0 K
- T* = 127.0 K

## Step B - Filter Family B
- Materials containing Hg with exactly 4 elements
- Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- Materials with Tc >= 95.0 K:
  - O4.081Cu1Ba2Hg1: Tc = 98.0 K
  - O4.096Cu1Ba2Hg1: Tc = 95.6 K
  - O4.097Cu1Ba2Hg1: Tc = 95.3 K
  - O4.150Cu1Ba2Hg1: Tc = 97.0 K
  - O4.162Cu1Ba2Hg1: Tc = 97.4 K
- Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1}

Wait, I need to double-check - O4.162Cu1Ba2Hg1 has Tc = 97.4 which is >= 95.0, so it should be included.

Actually, let me correct this:
- Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1}

## Step C - Extrapolate
- Unmeasurable materials found:
  - O4.047Cu1Ba2Hg1: no sample
  - O4.066Cu1Ba2Hg1: no sample
  - O6.160Ca1Cu2Ba2Hg1: no sample
  - O6.270Ca1Cu2Ba2Hg1: no sample
  - O6Cu1Ba2Hg1Tl1: no sample (this has 5 elements, so not part of 4-element group)

Looking at measurable materials to derive a rule:
- High Tc (>= 90K): O6.080Ca1Cu2Ba2Hg1(92.0), O6.100Ca1Cu2Ba2Hg1(92.5), O6.130Ca1Cu2Ba2Hg1(102.0), O6.140Ca1Cu2Ba2Hg1(105.0), O6.190Ca1Cu2Ba2Hg1(122.0), O6.210Ca1Cu2Ba2Hg1(127.0), O6.220Ca1Cu2Ba2Hg1(127.0), O4.081Cu1Ba2Hg1(98.0), O4.096Cu1Ba2Hg1(95.6), O4.097Cu1Ba2Hg1(95.3), O4.150Cu1Ba2Hg1(97.0), O4.162Cu1Ba2Hg1(97.4)
- Low Tc (< 90K): O4.085Cu1Ba2Hg1(92.0) - wait, this is actually >= 90K
- Actually, all measured values are >= 92.0K, so all are "high"

Wait, let me check if there are any low Tc materials. Looking back at the data:
- O4.101Cu1Ba2Hg1: 93.4 K (still high)
- O4.103Cu1Ba2Hg1: 92.5 K (still high)

Actually, I think all the measured materials are high Tc. But the problem states that single-factor rules are wrong on several materials, suggesting some are low Tc. Let me assume that the rule could be based on oxygen content or specific element ratios.

Looking more carefully at patterns:
- 4-element materials (O, Cu, Ba, Hg): typically have lower Tc values in the 90s-97s range
- 5-element materials (O, Ca, Cu, Ba, Hg): typically have higher Tc values in the 90s-120s range

Rule: Materials with Ca tend to have higher Tc values than those without Ca, though this isn't a perfect predictor.

For unmeasurable materials:
- O4.047Cu1Ba2Hg1: 4 elements, no Ca, low oxygen -> likely low Tc (prediction: low)
- O4.066Cu1Ba2Hg1: 4 elements, no Ca, low oxygen -> likely low Tc (prediction: low) 
- O6.160Ca1Cu2Ba2Hg1: 5 elements, has Ca, medium-high oxygen -> likely high Tc (prediction: high)
- O6.270Ca1Cu2Ba2Hg1: 5 elements, has Ca, high oxygen -> likely high Tc (prediction: high)
- O6Cu1Ba2Hg1Tl1: 5 elements but includes Tl instead of Ca -> likely moderate Tc (prediction: low)

## Step D - Final Pick
- Among unmeasurable materials predicted as high: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
- Fewest elements: Both have 5 elements (tied)
- Break tie by higher expected Tc: O6.270Ca1Cu2Ba2Hg1 would likely have higher Tc due to higher oxygen content
- Final pick: O6.270Ca1Cu2Ba2Hg1

Budget tracking:
- Used: 32
- Remaining: 2