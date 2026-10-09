# Notes for Superconductor Tc Study

## Step A — Series Peak
- **Goal**: Measure Tc for compositions in `data/measurable.txt` to find the maximum Tc (T*).
- **Measurements Completed**:
  - Highest Tc: **127.0 K** (`O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`).
  - Lowest Tc: **0.0 K** (`O6Cu1Ba2Hg1Tl1`).
- **Measurement Budget**: 25/34 calls used.

## Step B — Filter Family B
- **Goal**: Among materials containing Hg and having exactly 4 elements, report all with Tc >= (T* - 32) = **95.0 K**.
- **Approach**: Filter the measurable list for the specified criteria.

## Step C — Extrapolate
- **Goal**: Induce a rule to predict high/low Tc for unmeasurable materials based on measurable data.
- **Approach**: Analyze measurable data to identify patterns or rules that correlate with high/low Tc.

## Step D — Final Pick
- **Goal**: Select the unmeasurable material predicted as high Tc with the fewest elements.
- **Approach**: Apply the rule from Step C to predict high Tc materials and select the one with the fewest elements.

### Next Steps
1. **Step B**: Identify qualifying materials (Hg-containing, 4 elements, Tc >= 95.0 K).
2. **Step C**: Analyze measurable data to induce a rule for predicting high/low Tc.
3. **Step D**: Apply the rule to select the final unmeasurable material.