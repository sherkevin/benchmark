# Notes for Superconductor Tc Study

## Step A — Series Peak

**Objective**: Identify the homologous doping series for the seed material `O6.080Ca1Cu2Ba2Hg1` and measure their Tc values to find the maximum Tc (T*).

**Results**:
- **Peak Tc (T*)**: 127.0 K (`O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`).
- **Other measured materials**:
  - `O6.130Ca1Cu2Ba2Hg1` (102.0 K).
  - `O6.140Ca1Cu2Ba2Hg1` (105.0 K).
  - `O6.190Ca1Cu2Ba2Hg1` (122.0 K).
  - `O6.240Ca1Cu2Ba2Hg1` (123.0 K).
  - `O6.300Ca1Cu2Ba2Hg1` (100.0 K).
  - `O6.310Ca1Cu2Ba2Hg1` (98.0 K).
- **Unmeasurable materials**:
  - `O4.047Cu1Ba2Hg1`.
  - `O4.066Cu1Ba2Hg1`.
  - `O4.160Cu1Ba2Hg1`.
  - `O4.180Cu1Ba2Hg1`.
  - `O6.160Ca1Cu2Ba2Hg1`.
  - `O6.270Ca1Cu2Ba2Hg1`.

**Budget Used**: 28 units (17 remaining).

## Step B — Filter Family B

**Objective**: Among materials containing `Hg` with exactly 4 elements, report all with Tc >= (T* - 32) = 95.0 K.

**Results**:
- **Qualifying materials (Q1)**:
  - `O6.210Ca1Cu2Ba2Hg1` (127.0 K).
  - `O6.220Ca1Cu2Ba2Hg1` (127.0 K).
  - `O6.130Ca1Cu2Ba2Hg1` (102.0 K).
  - `O6.140Ca1Cu2Ba2Hg1` (105.0 K).
  - `O6.190Ca1Cu2Ba2Hg1` (122.0 K).
  - `O6.240Ca1Cu2Ba2Hg1` (123.0 K).
  - `O6.300Ca1Cu2Ba2Hg1` (100.0 K).
  - `O6.310Ca1Cu2Ba2Hg1` (98.0 K).

## Step C — Extrapolate

**Objective**: Induce a rule to predict high/low Tc for unmeasurable materials.

**Results**:
- **Rule**:
  - If oxygen content >= 4.096, predict **high**.
  - If oxygen content < 4.096, predict **low**.
- **Predictions**:
  - `O4.047Cu1Ba2Hg1`: low.
  - `O4.066Cu1Ba2Hg1`: low.
  - `O4.160Cu1Ba2Hg1`: high.
  - `O4.180Cu1Ba2Hg1`: high.
  - `O6.160Ca1Cu2Ba2Hg1`: high.
  - `O6.270Ca1Cu2Ba2Hg1`: high.

## Step D — Final Pick

**Objective**: Among unmeasurable materials predicted as **high**, choose the one with the fewest elements (break ties by higher expected Tc).

**Plan**:
1. Identify unmeasurable materials predicted as **high**.
2. Select the one with the fewest elements (all have 4 elements).
3. Break ties by higher expected Tc (oxygen content correlates with Tc).