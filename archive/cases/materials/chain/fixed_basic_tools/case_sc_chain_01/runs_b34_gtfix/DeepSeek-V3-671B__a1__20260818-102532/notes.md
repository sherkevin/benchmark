# Notes for Superconductor Tc Study

## Step A — Series Peak
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Measured Tc values:
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
- **T* (maximum Tc)**: 127.0 K (compositions `O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`)

## Step B — Filter Family B
- Threshold1 = T* - 32 = 95.0 K
- Measured Tc values for compositions with exactly 4 elements and containing Hg:
  - `O4.081Cu1Ba2Hg1`: 98.0 K
  - `O4.162Cu1Ba2Hg1`: 97.4 K
  - `O4.150Cu1Ba2Hg1`: 97.0 K
  - `O4.220Cu1Ba2Hg1`: 97.0 K
  - `O4.096Cu1Ba2Hg1`: 95.6 K
  - `O4.097Cu1Ba2Hg1`: 95.3 K
  - `O4.047Cu1Ba2Hg1`: 95.0 K
  - `O4.200Cu1Ba2Hg1`: 94.2 K
  - `O4.101Cu1Ba2Hg1`: 93.4 K
  - `O4.103Cu1Ba2Hg1`: 92.5 K
  - `O4.085Cu1Ba2Hg1`: 92.0 K
- **Qualifying compositions (Tc >= 95.0 K)**:
  - `O4.081Cu1Ba2Hg1`
  - `O4.162Cu1Ba2Hg1`
  - `O4.150Cu1Ba2Hg1`
  - `O4.220Cu1Ba2Hg1`
  - `O4.096Cu1Ba2Hg1`
  - `O4.097Cu1Ba2Hg1`
  - `O4.047Cu1Ba2Hg1`

## Step C — Extrapolate
- Measured Tc values for other compositions:
  - `O6Cu1Ba2Hg1Tl1`: 0.0 K (invalid measurement)
  - `O10Ca2Cu3Ba2Tl2`: 118.97 K
  - `O6Cu1Cd0.250Ba2Tl1.750`: 88.0 K
  - `O7Cu3Ba2La1`: 89.6 K
  - `O7Cu3Y0.200Ba2La0.800`: 81.0 K
- Observations:
  - High Tc (> 90 K) is associated with compositions containing `Hg` or `Tl`.
  - Compositions with `Cu` and `Ba` seem to favor higher Tc.
  - Compositions with `Y` or `La` tend to have lower Tc.
- **Rule**: A composition is likely to have high Tc if:
  - It contains `Hg` or `Tl`.
  - It has a high oxygen content (e.g., `O6` or higher).
  - It avoids elements like `Y` or `La`.

## Unmeasurable Compositions
- Predictions:
  - `O6.160Ca1Cu2Ba2Hg1`: high (contains `Hg`, high oxygen)
  - `O6.270Ca1Cu2Ba2Hg1`: high (contains `Hg`, high oxygen)
  - `O4.180Cu1Ba2Hg1`: low (oxygen content too low)
  - `O8Ca0.850Cu2.200Ba2Tl1.800`: high (contains `Tl`, high oxygen)
  - `O7Ca1Cu2Sr2Tl0.500Pb0.500`: high (contains `Tl`, high oxygen)
  - `O8Cu3Y1Ba1.900La0.100`: low (contains `Y` and `La`)
  - `O8Cu4Y1Ba1.870La0.130`: low (contains `Y` and `La`)

## Step D — Final Pick
- High-predicted unmeasurable compositions:
  - `O6.160Ca1Cu2Ba2Hg1`
  - `O6.270Ca1Cu2Ba2Hg1`
  - `O8Ca0.850Cu2.200Ba2Tl1.800`
  - `O7Ca1Cu2Sr2Tl0.500Pb0.500`
- Among these, the one with the fewest elements is `O6.160Ca1Cu2Ba2Hg1`.

## Measurement Budget
- Total budget: 34 `measure_tc` calls.
- Current usage: 26 calls.