# Notes for Superconductor Tc Study

## Step A — Series Peak
- **Seed Material**: `O6.080Ca1Cu2Ba2Hg1`
- **Measured Tc Values**:
  - Highest Tc: 127.0 K (`O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`)
  - **T***: 127.0 K
  - **Peak Composition**: `O6.210Ca1Cu2Ba2Hg1`

## Step B — Filter Family B
- **Threshold**: T* - 32 = 95.0 K
- **Qualifying Materials (Q1)**:
  - `O6.130Ca1Cu2Ba2Hg1` (102.0 K)
  - `O6.140Ca1Cu2Ba2Hg1` (105.0 K)
  - `O6.190Ca1Cu2Ba2Hg1` (122.0 K)
  - `O6.210Ca1Cu2Ba2Hg1` (127.0 K)
  - `O6.220Ca1Cu2Ba2Hg1` (127.0 K)
  - `O6.240Ca1Cu2Ba2Hg1` (123.0 K)
  - `O6.300Ca1Cu2Ba2Hg1` (100.0 K)
  - `O6.310Ca1Cu2Ba2Hg1` (98.0 K)
  - `O4.081Cu1Ba2Hg1` (98.0 K)
  - `O4.162Cu1Ba2Hg1` (97.4 K)
  - `O4.150Cu1Ba2Hg1` (97.0 K)
  - `O4.220Cu1Ba2Hg1` (97.0 K)
  - `O4.096Cu1Ba2Hg1` (95.6 K)
  - `O4.097Cu1Ba2Hg1` (95.3 K)

## Step C — Extrapolate
- **Goal**: Induce a rule to predict high/low Tc for materials in `data/unmeasurable.txt` based on measurable materials.

## Step D — Final Pick
- **Goal**: Among unmeasurable materials predicted high in Step C, pick the one with the fewest elements (break ties by higher expected Tc).

### Measurement Budget
- Total: 34 `measure_tc` calls.
- Used so far: 25.