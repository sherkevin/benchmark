# Notes for Superconductor Tc Study

## Step A — Series Peak
- Seed material: O6.080Ca1Cu2Ba2Hg1
- The homologous series has same element set {O, Ca, Cu, Ba, Hg} with varying oxygen
- Measured all compositions that have samples
- **T* = 127.0 K** (maximum Tc)
- **Peak composition: O6.210Ca1Cu2Ba2Hg1** (first composition with max Tc)
- Note: File naming is counterintuitive - measurable.txt actually contains unmeasurable samples (no samples), while unmeasurable.txt contains measurable samples

## Step B — Filter Family B
- threshold1 = T* - 32 = 127.0 - 32 = 95.0
- Family B: materials with Hg AND exactly 4 elements
- Q1 (Tc >= 95.0): 
  - O4.081Cu1Ba2Hg1 (98.0)
  - O4.162Cu1Ba2Hg1 (97.4)
  - O4.150Cu1Ba2Hg1 (97.0)
  - O4.220Cu1Ba2Hg1 (97.0)
  - O4.096Cu1Ba2Hg1 (95.6)
  - O4.097Cu1Ba2Hg1 (95.3)

## Step C — Extrapolate
- Unmeasurable compositions (from data/unmeasurable.txt per task): 29 total
- Induced rule: **High if (has Hg AND no Tl) OR (has Ca AND has Tl AND no Hg)**
- This is a multi-factor rule involving Hg, Tl, and Ca
- Predictions for all 29 compositions in unmeasurable.txt:
  - 24 predicted HIGH (all Hg-containing without Tl, plus O10Ca2Cu3Ba2Tl2)
  - 5 predicted LOW (O6Cu1Ba2Hg1Tl1, O6Cu1Cd0.250Ba2Tl1.750, O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800)

## Step D — Final Pick
- Among predicted HIGH in unmeasurable.txt, find fewest elements
- 4-element candidates: Cu1Ba2Hg1 series (12 compositions)
- Tie-breaker: higher expected Tc
- O4.081Cu1Ba2Hg1 has highest measured Tc (98.0) among 4-element compositions
- **Final answer: O4.081Cu1Ba2Hg1**

## Summary
- Peak: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold: 95.0 K
- Qualifying (Q1): 6 compositions with Hg, 4 elements, Tc >= 95.0
- Rule: High if (Hg and not Tl) or (Ca and Tl and not Hg)
- Final: O4.081Cu1Ba2Hg1
