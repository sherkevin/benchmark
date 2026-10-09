# Notes for Superconductor Tc Study

## Step A — Series Peak
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Measured Tc values:
  - `O6.080Ca1Cu2Ba2Hg1`: 92.0 K
  - `O6.100Ca1Cu2Ba2Hg1`: 92.5 K
  - `O6.130Ca1Cu2Ba2Hg1`: 102.0 K
  - `O6.140Ca1Cu2Ba2Hg1`: 105.0 K
  - `O6.190Ca1Cu2Ba2Hg1`: 122.0 K
  - `O6.210Ca1Cu2Ba2Hg1`: 127.0 K (highest so far)
  - `O6.220Ca1Cu2Ba2Hg1`: 127.0 K
  - `O6.240Ca1Cu2Ba2Hg1`: 123.0 K
  - `O6.300Ca1Cu2Ba2Hg1`: 100.0 K
  - `O6.310Ca1Cu2Ba2Hg1`: 98.0 K
  - `O4.081Cu1Ba2Hg1`: 98.0 K
  - `O4.162Cu1Ba2Hg1`: 97.4 K
  - `O4.150Cu1Ba2Hg1`: 97.0 K
  - `O4.220Cu1Ba2Hg1`: 97.0 K
  - `O4.096Cu1Ba2Hg1`: 95.6 K
  - `O4.097Cu1Ba2Hg1`: 95.3 K
  - `O4.047Cu1Ba2Hg1`: 95.0 K
  - `O4.200Cu1Ba2Hg1`: 94.2 K

- **T* (maximum Tc)**: 127.0 K (for `O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`).

## Step B — Filter Family B
- **Threshold1**: T* - 32 = 127.0 - 32 = 95.0 K
- Goal: Identify all materials containing Hg with exactly 4 elements where Tc >= 95.0 K.
- Measured materials with exactly 4 elements and Tc >= 95.0 K:
  - `O4.081Cu1Ba2Hg1`: 98.0 K
  - `O4.162Cu1Ba2Hg1`: 97.4 K
  - `O4.150Cu1Ba2Hg1`: 97.0 K
  - `O4.220Cu1Ba2Hg1`: 97.0 K
  - `O4.096Cu1Ba2Hg1`: 95.6 K
  - `O4.097Cu1Ba2Hg1`: 95.3 K
  - `O4.047Cu1Ba2Hg1`: 95.0 K

- **Q1**: [`O4.081Cu1Ba2Hg1`, `O4.162Cu1Ba2Hg1`, `O4.150Cu1Ba2Hg1`, `O4.220Cu1Ba2Hg1`, `O4.096Cu1Ba2Hg1`, `O4.097Cu1Ba2Hg1`, `O4.047Cu1Ba2Hg1`]

## Step C — Extrapolate
- Materials in `data/unmeasurable.txt`:
  - `O6.160Ca1Cu2Ba2Hg1`
  - `O6.270Ca1Cu2Ba2Hg1`
  - `O4.180Cu1Ba2Hg1`
  - `O8Ca0.850Cu2.200Ba2Tl1.800`
  - `O7Ca1Cu2Sr2Tl0.500Pb0.500`
  - `O8Cu3Y1Ba1.900La0.100`
  - `O8Cu4Y1Ba1.870La0.130`

- Goal: Induce a rule to predict high/low Tc based on measurable materials.

## Step D — Final Pick
- Goal: Among unmeasurable materials predicted as high in Step C, pick the one with the fewest elements (tie-break by higher expected Tc).

### Next Steps:
1. Analyze the measurable materials to induce a rule for predicting high/low Tc.
2. Apply the rule to unmeasurable materials.
3. Complete Step D based on predictions.