# Notes for Superconductor Tc Study

## Step A — Series Peak
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Measurable series members are listed in `data/measurable.txt`.
- Goal: Measure Tc for each composition in the series and identify the maximum Tc (T*).

## Step B — Filter Family B
- Criteria: Materials containing Hg and having exactly 4 elements.
- Threshold: `threshold1 = T* - 32`.
- Goal: Report all qualifying materials with Tc >= threshold1 (set Q1).

## Step C — Extrapolate
- Unmeasurable materials are listed in `data/unmeasurable.txt`.
- Goal: Induce a rule to predict high/low Tc for unmeasurable materials based on measurable ones.

## Step D — Final Pick
- Among unmeasurable materials predicted as high in Step C, pick the one with the fewest elements.

## Measurement Budget
- Total `measure_tc` calls allowed: 18.
- Plan:
  1. Measure all 10 oxygen-varied compositions in `data/measurable.txt` (Step A).
  2. Measure remaining 8 compositions (Step B).
