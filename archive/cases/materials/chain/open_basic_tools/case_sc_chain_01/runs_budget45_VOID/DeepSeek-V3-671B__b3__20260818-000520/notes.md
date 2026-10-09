# Notes for Superconductor Tc Study

## Step A: Find the homologous series peak (T*)
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Goal: Find all materials in the same series (same elements, varying oxygen) and measure their Tc to identify the peak.

### Observations:
1. Initial search for `Ca Cu Ba Hg` returned no results.
2. Search for `O Ca Cu Ba Hg` also returned no results.
3. Measured Tc of seed material: **92.0 K**.

### Conclusion:
- The seed material is unique; no other materials in the homologous series were found.
- Peak Tc (T*) = 92.0 K.

### Budget tracking:
- Total budget: 45 units
- Used so far: 15 units (6 search_db, 9 measure_tc)

## Step B: Filter family B (contains Hg and has exactly 4 elements)
- Threshold: T* - 32 = 60 K
- Goal: Find all materials containing Hg with exactly 4 elements and Tc ≥ 60 K.

### Observations:
1. Searched for `Hg` and found:
   - `O4.047Cu1Ba2Hg1`: Unmeasurable (no sample).
   - `O4.066Cu1Ba2Hg1`: Unmeasurable (no sample).
   - `O4.081Cu1Ba2Hg1`: Tc = 98.0 K (qualifies).
   - `O4.085Cu1Ba2Hg1`: Tc = 92.0 K (qualifies).
   - `O4.096Cu1Ba2Hg1`: Tc = 95.6 K (qualifies).
   - `O4.097Cu1Ba2Hg1`: Tc = 95.3 K (qualifies).
   - `O4.101Cu1Ba2Hg1`: Tc = 93.4 K (qualifies).
   - `O4.103Cu1Ba2Hg1`: Tc = 92.5 K (qualifies).
   - `O4.150Cu1Ba2Hg1`: Tc = 97.0 K (qualifies).
   - `O4.160Cu1Ba2Hg1`: Unmeasurable (no sample).

### Qualifying materials for Step B (Q1):
- `O4.081Cu1Ba2Hg1`
- `O4.085Cu1Ba2Hg1`
- `O4.096Cu1Ba2Hg1`
- `O4.097Cu1Ba2Hg1`
- `O4.101Cu1Ba2Hg1`
- `O4.103Cu1Ba2Hg1`
- `O4.150Cu1Ba2Hg1`

### Unmeasurable materials (for Step C):
- `O4.047Cu1Ba2Hg1`
- `O4.066Cu1Ba2Hg1`
- `O4.160Cu1Ba2Hg1`

### Plan:
1. Continue searching for more `Hg`-containing materials with exactly 4 elements.
2. Measure Tc for qualifying materials.
