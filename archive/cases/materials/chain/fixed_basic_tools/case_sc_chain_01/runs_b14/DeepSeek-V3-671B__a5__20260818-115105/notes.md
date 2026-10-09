# Superconductor Tc Study Notes

## Step A: Series Peak
- Measured Tc for all measurable compositions in `data/measurable.txt`.
- Peak Tc: **127.0 K** (for `O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`).
- Selected `O6.210Ca1Cu2Ba2Hg1` as the peak composition.

## Step B: Filter Family B
- Threshold (threshold1): **95.0 K** (T* - 32).
- Qualifying materials (Q1):
  - `O4.081Cu1Ba2Hg1` (98.0 K)
  - `O4.162Cu1Ba2Hg1` (97.4 K)
  - `O4.150Cu1Ba2Hg1` (97.0 K)
  - `O4.220Cu1Ba2Hg1` (97.0 K)

## Step C: Induced Rule
- High Tc materials have **5 elements** and oxygen content between **6.0 and 6.3**.
- Applied rule to predict high/low for unmeasurable compositions.

## Step D: Final Pick
- Selected `O6.160Ca1Cu2Ba2Hg1` as the final answer (fewest elements among predicted high Tc materials).

## Budget Usage
- Total `measure_tc` calls: **14/14**.