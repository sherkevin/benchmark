# Notes for Superconductor Tc Study

## Step A — Series Peak
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- List of measurable compositions includes 26 materials.
- Goal: Measure Tc for each and find the composition with the maximum Tc (T*).

## Step B — Filter Family B
- Threshold1 = T* - 32.
- Filter materials containing Hg and having exactly 4 elements.
- Report all with Tc >= threshold1 as set Q1.

## Step C — Extrapolate
- Unmeasurable materials: 7 compositions.
- Induce a rule to predict high/low Tc based on measurable materials.
- Rule must consider multiple factors (not just single-factor rules).

## Step D — Final Pick
- Among unmeasurable materials predicted as high in Step C, pick the one with fewest elements (break ties by higher expected Tc).

## Measurement Budget
- Total budget: 18 `measure_tc` calls.
- Step A will require 26 measurements, but budget is only 18. Need to prioritize or find a subset.
