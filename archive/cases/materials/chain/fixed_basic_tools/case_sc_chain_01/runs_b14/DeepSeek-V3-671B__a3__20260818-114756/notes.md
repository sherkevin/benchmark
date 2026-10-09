# Notes for Superconductor Tc Study

## Step A — Series Peak
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Goal: Measure Tc for all materials in `data/measurable.txt` to find the composition with the maximum Tc (T*).
- Measurement budget: 14 `measure_tc` calls.

## Step B — Filter Family B
- Threshold1 = T* - 32
- Goal: Among materials containing Hg and having exactly 4 elements, report all with Tc >= threshold1 (set Q1).

## Step C — Extrapolate
- Goal: Induce a rule to predict high/low Tc for materials in `data/unmeasurable.txt` based on measurable data.
- Warning: Single-factor rules are insufficient; the rule must account for multiple interacting factors.

## Step D — Final Pick
- Goal: Among unmeasurable materials predicted as high in Step C, pick the one with the fewest elements (break ties by higher expected Tc).

### Measurements Log
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
- Budget used: 10/14
