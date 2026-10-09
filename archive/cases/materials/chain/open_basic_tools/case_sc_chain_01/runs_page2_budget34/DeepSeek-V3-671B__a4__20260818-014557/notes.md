# Notes for Superconductor Tc Study

## Step A — Series Peak
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Goal: Find and measure homologous series members (same element set, varying oxygen) to identify the composition with maximum Tc (T*).

## Step B — Filter Family B
- Threshold1 = T* - 32
- Goal: Report all materials containing Hg and exactly 4 elements with Tc >= threshold1 (set Q1).

## Step C — Extrapolate
- Goal: Induce a rule from measurable materials to predict high/low for unmeasurable ones.

## Step D — Final Pick
- Goal: Among unmeasurable materials predicted high, choose the one with the fewest elements (break ties by higher expected Tc).

## Budget
- Total budget: 34 units
- Tools: `measure_tc` and `search_db` each cost 1 unit per use.

## Next Steps
1. Search for homologous series members of the seed material.
2. Measure their Tc to find T*.
3. Proceed with Steps B, C, and D as outlined.