# Notes for Superconductor Tc Study

## Step A — Series Peak
- **Objective**: Measure Tc for all compositions in `data/measurable.txt` to find the maximum Tc (T*).
- **Seed Material**: `O6.080Ca1Cu2Ba2Hg1`
- **Total Measurement Budget**: 18 `measure_tc` calls.
- **Measurements Completed**:
  - `O6.080Ca1Cu2Ba2Hg1`: Tc = 92.0 K
  - `O6.100Ca1Cu2Ba2Hg1`: Tc = 92.5 K
  - `O6.130Ca1Cu2Ba2Hg1`: Tc = 102.0 K
  - `O6.140Ca1Cu2Ba2Hg1`: Tc = 105.0 K
  - `O6.190Ca1Cu2Ba2Hg1`: Tc = 122.0 K
  - `O6.210Ca1Cu2Ba2Hg1`: Tc = 127.0 K
  - `O6.220Ca1Cu2Ba2Hg1`: Tc = 127.0 K
  - `O6.240Ca1Cu2Ba2Hg1`: Tc = 123.0 K
  - `O6.300Ca1Cu2Ba2Hg1`: Tc = 100.0 K
  - `O6.310Ca1Cu2Ba2Hg1`: Tc = 98.0 K
  - `O4.081Cu1Ba2Hg1`: Tc = 98.0 K
  - `O4.162Cu1Ba2Hg1`: Tc = 97.4 K
  - `O4.150Cu1Ba2Hg1`: Tc = 97.0 K
  - `O4.220Cu1Ba2Hg1`: Tc = 97.0 K
  - `O4.096Cu1Ba2Hg1`: Tc = 95.6 K
  - `O4.097Cu1Ba2Hg1`: Tc = 95.3 K
  - `O4.047Cu1Ba2Hg1`: Tc = 95.0 K
  - `O4.200Cu1Ba2Hg1`: Tc = 94.2 K
- **Peak Tc (T*)**: 127.0 K (from `O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`)

## Step B — Filter Family B
- **Threshold**: threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
- **Criteria**: Materials containing Hg and having exactly 4 elements.
- **Qualifying Materials (Q1)**:
  - `O4.081Cu1Ba2Hg1`: 98.0 K
  - `O4.162Cu1Ba2Hg1`: 97.4 K
  - `O4.150Cu1Ba2Hg1`: 97.0 K
  - `O4.220Cu1Ba2Hg1`: 97.0 K
  - `O4.096Cu1Ba2Hg1`: 95.6 K
  - `O4.097Cu1Ba2Hg1`: 95.3 K
  - `O4.047Cu1Ba2Hg1`: 95.0 K
  - `O4.200Cu1Ba2Hg1`: 94.2 K

## Step C — Extrapolate
- **Objective**: Induce a rule to predict high/low Tc for unmeasurable materials in `data/unmeasurable.txt`.
- **Unmeasurable Materials**:
  - `O6.160Ca1Cu2Ba2Hg1`
  - `O6.270Ca1Cu2Ba2Hg1`
  - `O4.180Cu1Ba2Hg1`
  - `O8Ca0.850Cu2.200Ba2Tl1.800`
  - `O7Ca1Cu2Sr2Tl0.500Pb0.500`
  - `O8Cu3Y1Ba1.900La0.100`
  - `O8Cu4Y1Ba1.870La0.130`
- **Rule Induction**:
  - High-Tc materials (>90 K) in the measurable set have:
    - At least 4 elements.
    - Include Hg or Tl.
    - Oxygen content between 4.0 and 6.3.
  - Low-Tc materials (<90 K) violate one or more of these conditions.

## Step D — Final Pick
- **Objective**: Among predicted high-Tc unmeasurable materials, pick the one with the fewest elements (tie-break by higher expected Tc).
- **Predicted High-Tc Unmeasurable Materials**:
  - `O6.160Ca1Cu2Ba2Hg1`
  - `O6.270Ca1Cu2Ba2Hg1`
  - `O4.180Cu1Ba2Hg1`
- **Final Pick**: `O4.180Cu1Ba2Hg1` (fewest elements: 4).

## Answer Format
```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": ["O4.081Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.047Cu1Ba2Hg1", "O4.200Cu1Ba2Hg1"],
  "predictions": {
    "O6.160Ca1Cu2Ba2Hg1": "high",
    "O6.270Ca1Cu2Ba2Hg1": "high",
    "O4.180Cu1Ba2Hg1": "high",
    "O8Ca0.850Cu2.200Ba2Tl1.800": "low",
    "O7Ca1Cu2Sr2Tl0.500Pb0.500": "low",
    "O8Cu3Y1Ba1.900La0.100": "low",
    "O8Cu4Y1Ba1.870La0.130": "low"
  },
  "final": "O4.180Cu1Ba2Hg1",
  "rule": "High-Tc materials have at least 4 elements, include Hg or Tl, and have oxygen content between 4.0 and 6.3."
}
```