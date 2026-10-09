# Notes for Superconductor Tc Study

## Step A: Series Peak Identification
- **Seed Material**: `O6.080Ca1Cu2Ba2Hg1`
- Goal: Find all members of the homologous doping series (same element set, varying oxygen) and measure their Tc to identify the composition with maximum Tc (T*).
- Plan:
  1. Use `search_db》 to find all materials with the same element set (`Ca1Cu2Ba2Hg1`) but varying oxygen content.
  2. Measure Tc for each found composition.
  3. Identify the composition with the highest Tc (T*).

### Results:
- Measured Tc for all 12 compositions:
  - `O6.080Ca1Cu2Ba2Hg1`: 92.0 K
  - `O6.100Ca1Cu2Ba2Hg1》: 92.5 K
  - `O6.130Ca1Cu2Ba2Hg1`: 102.0 K
  - `O6.140Ca1Cu2Ba2Hg1`: 105.0 K
  - `O6.160Ca1Cu2Ba2Hg1`: No sample
  - `O6.190Ca1Cu2Ba2Hg1》: 122.0 K
  - `O6.210Ca1Cu2Ba2Hg1`: 127.0 K
  - `O6.220Ca1Cu2Ba2Hg1`: 127.0 K
  - `O6.240Ca1Cu2Ba2Hg1`: 123.0 K
  - `O6.270Ca1Cu2Ba2Hg1`: No sample
  - `O6.300Ca1Cu2Ba2Hg1`: 100.0 K
  - `O6.310Ca1Cu2Ba2Hg1`: 98.0 K

- **Peak Composition**: `O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1` (both 127.0 K).
- **T***: 127.0 K

## Step B: Filter Family B
- Goal: Among materials containing Hg and having exactly 4 elements, report all whose Tc >= (T* - 32) = 95.0 K.
- Plan:
  1. Use `search_db` to find materials containing Hg and having exactly 4 elements.
  2. Measure Tc for each candidate.
  3. Filter those with Tc >= 95.0 K.

### Results:
- Measured Tc for all 15 candidates:
  - `O4.047Cu1Ba2Hg1`: No sample
  - `O4.066Cu1Ba2Hg1`: No sample
  - `O4.081Cu1Ba2Hg1`: 98.0 K
  - `O4.085Cu1Ba2Hg1`: 92.0 K
  - `O4.096Cu1Ba2Hg1`: 95.6 K
  - `O4.097Cu1Ba2Hg1`: 95.3 K
  - `O4.101Cu1Ba2Hg1》: 93.4 K
  - `O4.103Cu1Ba2Hg1`: 92.5 K
  - `O4.150Cu1Ba2Hg1`: 97.0 K
  - `O4.160Cu1Ba2Hg1`: No sample
  - `O4.162Cu1Ba2Hg1`: 97.4 K
  - `O4.180Cu1Ba2Hg1`: No sample
  - `O4.200Cu1Ba2Hg1`: 94.2 K
  - `O4.220Cu1Ba2Hg1`: 97.0 K
  - `O6Cu1Ba2Hg1Tl1`: 0.0 K

- **Qualifying Materials**:
  - `O4.081Cu1Ba2Hg1`: 98.0 K
  - `O4.096Cu1Ba2Hg1`: 95.6 K
  - `O4.097Cu1Ba2Hg1》: 95.3 K
  - `O4.150Cu1Ba2Hg1`: 97.0 K
  - `O4.162Cu1Ba2Hg1`: 97.4 K
  - `O4.220Cu1Ba2Hg1`: 97.0 K

## Step C: Extrapolate Unmeasurable Materials
- Goal: Induce a rule from measurable materials to predict high/low for unmeasurable ones.
- Plan:
  1. Analyze measurable materials to identify patterns.
  2. Predict high/low for unmeasurable materials based on the rule.

## Step D: Final Pick
- Goal: Among predicted high unmeasurable materials, choose the one with the fewest elements.
- Plan:
  1. Select unmeasurable materials predicted as high.
  2. Pick the one with the fewest elements, breaking ties by higher expected Tc.

## Budget Tracking
- Total budget: 45 units.
- Spent so far: 31 units.

## Step C: Extrapolate Unmeasurable Materials
- Goal: Induce a rule from measurable materials to predict high/low for unmeasurable ones.
- Plan:
  1. Analyze measurable materials to identify patterns.
  2. Predict high/low for unmeasurable materials based on the rule.

## Step D: Final Pick
- Goal: Among predicted high unmeasurable materials, choose the one with the fewest elements.
- Plan:
  1. Select unmeasurable materials predicted as high.
  2. Pick the one with the fewest elements, breaking ties by higher expected Tc.

## Budget Tracking
- Total budget: 45 units.
- Spent so far: 20 units.
