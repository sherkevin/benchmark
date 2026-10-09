# Notes for Superconductor Tc Study

## Step A: Identify Homologous Doping Series
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Goal: Find series members by varying oxygen content and measure Tc to find the maximum (T*).

### Results:
- Measured Tc for all synthesized materials in the series.
- Highest Tc observed: **127.0 K** for `O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`.
- **T*** (peak Tc) = **127.0 K** (composition: `O6.210Ca1Cu2Ba2Hg1`).

### Unmeasurable Materials:
- `O6.160Ca1Cu2Ba2Hg1` (not synthesized)
- `O6.270Ca1Cu2Ba2Hg1` (not synthesized)

## Step B: Filter Family B (contains Hg and has exactly 4 elements)
- **threshold1** = T* - 32 = 127.0 - 32 = **95.0 K**
- Searched for materials containing Hg and having exactly 4 elements.
- Measured Tc for synthesized materials.
- **Q1**: Materials with Tc ≥ 95.0 K:
  - `O4.081Cu1Ba2Hg1` (98.0 K)
  - `O4.096Cu1Ba2Hg1` (95.6 K)
  - `O4.097Cu1Ba2Hg1` (95.3 K)

### Unmeasurable Materials:
- `O4.047Cu1Ba2Hg1` (not synthesized)
- `O4.066Cu1Ba2Hg1` (not synthesized)

## Step C: Extrapolate (Predict Tc for Unmeasurable Materials)
### Induced Rule:
- For materials with 5 elements (e.g., homologous series), high Tc (>90 K) is associated with oxygen content ≥ 6.0.
- For materials with 4 elements, high Tc (>90 K) is associated with oxygen content ≥ 4.08.

### Predictions:
- `O6.160Ca1Cu2Ba2Hg1` (5 elements, oxygen = 6.16 ≥ 6.0) → **high**
- `O6.270Ca1Cu2Ba2Hg1` (5 elements, oxygen = 6.27 ≥ 6.0) → **high**
- `O4.047Cu1Ba2Hg1` (4 elements, oxygen = 4.047 < 4.08) → **low**
- `O4.066Cu1Ba2Hg1` (4 elements, oxygen = 4.066 < 4.08) → **low**

## Budget Tracking
- Initial budget: 34 units
- Used: 20 units (12 for search_db, 8 for measure_tc)
- Remaining: 14 units