# Superconductor Tc Study Notes

## Step A: Series Peak
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Need to measure the series members in data/measurable.txt to find the composition with MAXIMUM Tc
- Will call this maximum T*

## Step B: Filter Family B
- threshold1 = T* - 32
- Find materials that contain Hg and have exactly 4 elements with Tc >= threshold1
- Call this set Q1

## Step C: Extrapolate
- Materials in data/unmeasurable.txt cannot be measured
- Need to induce a rule that separates high from low Tc using measurable materials
- Predict high/low for every unmeasurable material

## Step D: Final Pick
- Among unmeasurable materials predicted as high, return the one with fewest elements