# Notes for Superconductor Tc Study

## Step A — Series Peak
- Goal: Measure Tc for each composition in `data/measurable.txt` and find the maximum Tc (T*).
- Seed material: `O6.080Ca1Cu2Ba2Hg1`.
- Measurement budget: 18 `measure_tc` calls.
- Results:
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
  - `O4.081Cu1Ba2Hg1`: 98.0 K
  - `O4.162Cu1Ba2Hg1`: 97.4 K
  - `O4.150Cu1Ba2Hg1`: 97.0 K
  - `O4.220Cu1Ba2Hg1`: 97.0 K
  - `O4.096Cu1Ba2Hg1`: 95.6 K
  - `O4.097Cu1Ba2Hg1`: 95.3 K
  - `O4.047Cu1Ba2Hg1`: 95.0 K
  - `O4.200Cu1Ba2Hg1`: 94.2 K
- Maximum Tc (T*): 127.0 K (`O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`).

## Step B — Filter Family B
- Threshold: T* - 32 = 127.0 - 32 = 95.0 K.
- Filter: Materials containing Hg and having exactly 4 elements.
- Qualifying materials (Tc >= 95.0 K):
  - `O4.081Cu1Ba2Hg1`: 98.0 K
  - `O4.162Cu1Ba2Hg1`: 97.4 K
  - `O4.150Cu1Ba2Hg1`: 97.0 K
  - `O4.220Cu1Ba2Hg1`: 97.0 K
  - `O4.096Cu1Ba2Hg1`: 95.6 K
  - `O4.097Cu1Ba2Hg1`: 95.3 K
  - `O4.047Cu1Ba2Hg1`: 95.0 K

## Step C — Extrapolate
- Goal: Induce a rule to predict high/low Tc for unmeasurable materials in `data/unmeasurable.txt`.
- Rule must consider multiple factors (not just single-factor rules).

## Step D — Final Pick
- Among unmeasurable materials predicted as high in Step C, pick the one with fewest elements (break ties by higher expected Tc).

## Progress
- Completed Step A and Step B.
- Next: Induce a rule for Step C.