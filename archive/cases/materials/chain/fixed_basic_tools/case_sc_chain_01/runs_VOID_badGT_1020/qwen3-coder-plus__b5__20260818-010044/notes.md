# Superconductor Tc Study Notes

## Step A: Series Peak
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Need to measure the series members in measurable.txt and find the composition with MAXIMUM Tc
- This is a homologous doping series (same element set, varying oxygen)

## Step B: Filter Family B
- Contains Hg and has exactly 4 elements
- threshold1 = T* - 32 (where T* is the maximum Tc from Step A)
- Find all materials in this category with Tc >= threshold1

## Step C: Extrapolate
- Materials in unmeasurable.txt cannot be measured
- Need to induce a rule that separates high from low Tc using measurable materials
- Predict high/low for every unmeasurable material

## Step D: Final Pick
- Among unmeasurable materials predicted as high in Step C
- Return the one with fewest elements (break ties by higher expected Tc)