# Notes for Superconductor Tc Study

## Step A: Series Peak
- **Seed Material**: `O6.080Ca1Cu2Ba2Hg1`
- Goal: Find and measure the homologous series members (varying oxygen) to identify the composition with maximum Tc.

### Findings:
- Measured Tc for compositions in the series:
  - `O6.080Ca1Cu2Ba2Hg1`: 92.0 K
  - `O6.100Ca1Cu2Ba2Hg1`: 92.5 K
  - `O6.130Ca1Cu2Ba2Hg1`: 102.0 K
  - `O6.140Ca1Cu2Ba2Hg1`: 105.0 K
  - `O6.190Ca1Cu2Ba2Hg1`: 122.0 K
  - `O6.210Ca1Cu2Ba2Hg1`: 127.0 K
  - `O6.220Ca1Cu2Ba2Hg1`: 127.0 K
  - `O6.240Ca1Cu2Ba2Hg1`: 123.0 K
  - `O6.300Ca1Cu2Ba2Hg1`: 100.0 K
  - `O6.310Ca1Cu2Ba2Hg1`: 98.0 K

- **Peak Tc**: 127.0 K (achieved by `O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`)

### Budget Used: 17 units (out of 45)

## Step B: Filter Family B (contains Hg and has exactly 4 elements)
- **Threshold1**: `T* - 32 = 127.0 - 32 = 95.0 K`
- Goal: Report all materials containing Hg with exactly 4 elements and Tc >= 95.0 K.

### Findings:
- Measured Tc for compositions with exactly 4 elements:
  - `O4.081Cu1Ba2Hg1`: 98.0 K (qualifies)
  - `O4.085Cu1Ba2Hg1`: 92.0 K (does not qualify)
  - `O4.096Cu1Ba2Hg1`: 95.6 K (qualifies)
  - `O4.097Cu1Ba2Hg1`: 95.3 K (qualifies)
  - `O4.101Cu1Ba2Hg1`: 93.4 K (does not qualify)
  - `O4.103Cu1Ba2Hg1`: 92.5 K (does not qualify)

- **Qualifying Materials (Q1)**:
  - `O4.081Cu1Ba2Hg1`
  - `O4.096Cu1Ba2Hg1`
  - `O4.097Cu1Ba2Hg1`

### Budget Used: 25 units (out of 45)

## Step C: Extrapolate (No Measurement Possible)
- **Goal**: Induce a rule to predict high/low for unmeasurable materials.
- **Rule**: Materials with oxygen content >= 4.0 are likely **high** (Tc >= 90 K).

### Predictions for Unmeasurable Materials:
1. `O4.047Cu1Ba2Hg1`: **high**
2. `O4.066Cu1Ba2Hg1`: **high**
3. `O6.160Ca1Cu2Ba2Hg1`: **high**
4. `O6.270Ca1Cu2Ba2Hg1`: **high**

### Budget Used: 25 units (out of 45)

## Next Steps:
1. Among unmeasurable materials predicted **high**, choose the one with the fewest elements (Step D).