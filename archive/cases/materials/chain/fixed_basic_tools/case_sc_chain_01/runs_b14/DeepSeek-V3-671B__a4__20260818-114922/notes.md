# Notes for Superconductor Tc Study

## Step A: Series Peak
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Goal: Measure Tc for all compositions in `data/measurable.txt` to find the maximum Tc (T*).
- Measurement budget: 14 `measure_tc` calls.

## Step B: Filter Family B
- Threshold1 = T* - 32
- Filter: Materials containing Hg and having exactly 4 elements.
- Report all materials with Tc >= threshold1 as set Q1.

## Step C: Extrapolate
- Materials in `data/unmeasurable.txt` cannot be measured.
- Induce a rule based on measurable materials to predict high/low Tc for unmeasurable ones.
- Rule must consider multiple factors (not just single-factor rules).

## Step D: Final Pick
- Among unmeasurable materials predicted as high in Step C, pick the one with the fewest elements (break ties by higher expected Tc).

## Next Steps
1. Measure Tc for all compositions in `data/measurable.txt` to find T*.
2. Calculate threshold1 and identify Q1.
3. Analyze measurable materials to induce a rule for predicting Tc in unmeasurable ones.
4. Apply the rule to unmeasurable materials and select the final pick.

## Measurements Log
- To be populated as measurements are taken.