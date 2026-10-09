# Notes for Superconductor Tc Study

## Step A — Series Peak
- **Peak Composition**: `O6.210Ca1Cu2Ba2Hg1`
- **Peak Tc (T*)**: 127.0 K

## Step B — Filter Family B
- **Threshold1**: 95.0 K (T* - 32)
- **Qualifying Compositions (Q1)**:
  - `O4.081Cu1Ba2Hg1` (98.0 K)
  - `O4.162Cu1Ba2Hg1` (97.4 K)
  - `O4.150Cu1Ba2Hg1` (97.0 K)
  - `O4.220Cu1Ba2Hg1` (97.0 K)
  - `O4.096Cu1Ba2Hg1` (95.6 K)
  - `O4.097Cu1Ba2Hg1` (95.3 K)

## Step C — Extrapolate
- **Unmeasurable Materials**:
  - `O6.160Ca1Cu2Ba2Hg1`
  - `O6.270Ca1Cu2Ba2Hg1`
  - `O4.180Cu1Ba2Hg1`
  - `O4.047Cu1Ba2Hg1`
  - `O4.066Cu1Ba2Hg1`
  - `O4.160Cu1Ba2Hg1`
  - `O8Ca0.850Cu2.200Ba2Tl1.800`
  - `O7Ca1Cu2Sr2Tl0.500Pb0.500`
  - `O8Cu3Y1Ba1.900La0.100`
  - `O8Cu4Y1Ba1.870La0.130`
- **Goal**: Induce a rule to predict high/low Tc based on measurable materials.

## Step D — Final Pick
- **Criteria**:
  - Among unmeasurable materials predicted **high** in Step C.
  - Select the one with the **fewest elements** (tie-break by higher expected Tc).

## Next Steps
1. Analyze measurable materials to induce a rule for predicting high/low Tc.
2. Apply the rule to unmeasurable materials.
3. Select the final pick and write `answer.json`.
