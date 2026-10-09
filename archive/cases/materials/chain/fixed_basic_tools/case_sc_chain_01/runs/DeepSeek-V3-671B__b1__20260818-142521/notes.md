# Notes for Superconductor Tc Study

## Step A — Series Peak
- Measured Tc for all compositions in `data/measurable.txt`.
- Highest Tc: **127.0 K** for `O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`.
- **T* (peak Tc)**: 127.0 K.

## Step B — Filter Family B (contains Hg and has exactly 4 elements)
- **Threshold1**: T* - 32 = 95.0 K.
- Qualifying materials (Tc ≥ 95.0 K, contains Hg, exactly 4 elements):
  - `O4.081Cu1Ba2Hg1` (98.0 K)
  - `O4.162Cu1Ba2Hg1` (97.4 K)
  - `O4.150Cu1Ba2Hg1` (97.0 K)
  - `O4.220Cu1Ba2Hg1` (97.0 K)
  - `O4.096Cu1Ba2Hg1` (95.6 K)
  - `O4.097Cu1Ba2Hg1` (95.3 K)
  - `O4.047Cu1Ba2Hg1` (95.0 K)

## Step C — Extrapolate (Unmeasurable Materials)
- To predict high/low Tc for materials in `data/unmeasurable.txt`.
- Need to induce a rule based on measurable materials.

## Step D — Final Pick
- Among predicted high-Tc unmeasurable materials, pick the one with the fewest elements.