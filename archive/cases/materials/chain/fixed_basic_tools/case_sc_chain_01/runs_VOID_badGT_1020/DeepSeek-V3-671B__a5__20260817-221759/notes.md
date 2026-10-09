# Notes for Superconductor Tc Study

## Step A — Series Peak
- **Seed material**: `O6.080Ca1Cu2Ba2Hg1`
- **Goal**: Measure Tc for all compositions in `data/measurable.txt` to find the maximum Tc (T*).
- **Measurement budget**: 34 `measure_tc` calls.

## Step B — Filter Family B
- **Threshold**: threshold1 = T* - 32
- **Criteria**: Materials containing Hg and exactly 4 elements.
- **Goal**: Identify all materials meeting the criteria with Tc >= threshold1 (set Q1).

## Step C — Extrapolate
- **Goal**: Induce a rule to predict high/low Tc for materials in `data/unmeasurable.txt` based on measurable data.
- **Warning**: Avoid single-factor rules; the governing rule involves multiple factors.

## Step D — Final Pick
- **Goal**: Among predicted high-Tc unmeasurable materials, select the one with the fewest elements (break ties by higher expected Tc).

### Next Steps
1. Measure Tc for all compositions in `data/measurable.txt`.
2. Identify T* and threshold1.
3. Filter materials for Q1.
4. Induce a rule for unmeasurable materials.
5. Predict high/low Tc for unmeasurable materials.
6. Select the final material.