# Notes for Superconductor Tc Study

## Step A — Series Peak
- **Seed material**: `O6.080Ca1Cu2Ba2Hg1`
- **Definition**: A material is **high** if Tc >= 90 K, otherwise **low**.
- **Measurement budget**: 34 `measure_tc` calls.

### Results:
- **Maximum Tc (T*)**: 127.0 K (composition: `O6.210Ca1Cu2Ba2Hg1`).
- **Threshold1**: 95.0 K (T* - 32).

## Step B — Filter Family B (Contains Hg and Exactly 4 Elements)
- **Qualifying materials (Q1)**:
  ```json
  [
    "O6.130Ca1Cu2Ba2Hg1",
    "O6.140Ca1Cu2Ba2Hg1",
    "O6.190Ca1Cu2Ba2Hg1",
    "O6.210Ca1Cu2Ba2Hg1",
    "O6.220Ca1Cu2Ba2Hg1",
    "O6.240Ca1Cu2Ba2Hg1",
    "O6.300Ca1Cu2Ba2Hg1",
    "O6.310Ca1Cu2Ba2Hg1",
    "O4.081Cu1Ba2Hg1",
    "O4.162Cu1Ba2Hg1",
    "O4.150Cu1Ba2Hg1",
    "O4.220Cu1Ba2Hg1",
    "O4.096Cu1Ba2Hg1",
    "O4.097Cu1Ba2Hg1",
    "O4.047Cu1Ba2Hg1"
  ]
  ```

### Next Steps:
- **Step C**: Induce a rule to predict high/low Tc for unmeasurable materials in `data/unmeasurable.txt`.
- **Step D**: Among predicted high unmeasurable materials, pick the one with the fewest elements.
